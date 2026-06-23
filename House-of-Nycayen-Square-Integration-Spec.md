# House of Nycayen — Square Headless Integration Spec (Claude Code)

The production-stage spec: wiring the Next.js site to Square as the operational backbone. Accurate to the current `square@^40` SDK (`SquareClient`, API version 2026-05). Build the UI in Claude Design first; this is the layer that makes it a working business.

> **Two rules that decide whether this is solid or a liability.** (1) The browser never sends prices — the server always builds the order from catalog IDs and charges the server-computed total. (2) The access token and webhook key live server-side only, and every webhook is signature-verified against the raw body. Everything below enforces these.

---

## 0. Architecture

Headless **Next.js (App Router)** on **Vercel**. All Square calls run **server-side** (Route Handlers / Server Actions). The only Square value the browser ever sees is the public Application ID + Location ID needed by the Web Payments SDK to tokenize a card.

```
Browser (themed UI)                 Server (Vercel route handlers)        Square
─ booking UI ───────────────────▶  /api/bookings/* ───────────────────▶  Bookings API ─▶ Square Appointments ─▶ Google Calendar (dashboard sync)
─ Web Payments SDK (tokenize) ──▶  /api/checkout (builds order, pays) ─▶  Catalog · Orders · Payments
─ product pages ────────────────▶  /api/catalog (server fetch) ───────▶  Catalog · Inventory
                                    /api/webhooks/square (verified) ◀───  Webhooks (order/payment/booking)
```

Square owns: appointments, store/payments, inventory, customers, gift cards, loyalty, in-person POS. Google Calendar sync is a **Square Appointments dashboard setting** (staff calendar sync) — not an API call.

---

## 1. Setup

1. **Square Dashboard → Developer → create Application.** Capture: **Application ID**, **Access Token** (sandbox + production), **Location ID**.
2. **Enable Square Appointments.** Create each service as a **bookable Catalog service** (item type `APPOINTMENTS_SERVICE`) with a **service variation** + assigned **team member**. Map the consultation types (Discovery / In-Studio / Bridal / On-Location) to service variation IDs.
3. **Turn on staff Google Calendar sync** in Appointments settings (this is the "use Google Calendar" requirement — handled in the dashboard, not code).
4. `npm i square@^40` (the new `SquareClient` SDK). The SDK pins the `Square-Version` header.

---

## 2. Environment variables

```
# server-only (secret — NEVER expose)
SQUARE_ACCESS_TOKEN=
SQUARE_ENVIRONMENT=sandbox        # or production
SQUARE_LOCATION_ID=
SQUARE_WEBHOOK_SIGNATURE_KEY=
SQUARE_WEBHOOK_NOTIFICATION_URL=https://nycayen.com/api/webhooks/square

# public (safe to ship — required by the Web Payments SDK in the browser)
NEXT_PUBLIC_SQUARE_APPLICATION_ID=
NEXT_PUBLIC_SQUARE_LOCATION_ID=
```

Only the two `NEXT_PUBLIC_*` values reach the client. The access token and webhook key never do.

---

## 3. Server client — `lib/square.ts`

```ts
import { SquareClient, SquareEnvironment } from "square";

export const square = new SquareClient({
  token: process.env.SQUARE_ACCESS_TOKEN!,
  environment:
    process.env.SQUARE_ENVIRONMENT === "production"
      ? SquareEnvironment.Production
      : SquareEnvironment.Sandbox,
});

export const LOCATION_ID = process.env.SQUARE_LOCATION_ID!;
```
Money is `BigInt` in the smallest denomination (cents). Methods return the named field, e.g. `(await square.orders.create(...)).order`. Wrap calls and catch `SquareError`.

---

## 4. Booking flow (Bookings API)

**Availability** — `app/api/bookings/availability/route.ts`:
```ts
import { square, LOCATION_ID } from "@/lib/square";

export async function POST(req: Request) {
  const { serviceVariationId, teamMemberId, startAt, endAt } = await req.json();
  const { availabilities } = await square.bookings.searchAvailability({
    query: { filter: {
      startAtRange: { startAt, endAt },
      locationId: LOCATION_ID,
      segmentFilters: [{ serviceVariationId, teamMemberIdFilter: { any: [teamMemberId] } }],
    }},
  });
  return Response.json({ availabilities });
}
```

**Create** — `app/api/bookings/create/route.ts`: validate inputs server-side, upsert the **Customer** (Customers API), then:
```ts
import { randomUUID } from "crypto";
const { booking } = await square.bookings.create({
  idempotencyKey: randomUUID(),
  booking: {
    locationId: LOCATION_ID,
    startAt,                         // ISO 8601 from a returned availability slot
    customerId,
    appointmentSegments: [{ teamMemberId, serviceVariationId, serviceVariationVersion, durationMinutes }],
    customerNote,                    // carry the qualifying intake here
  },
});
```
Confirmation + SMS/email reminders and the cancellation/deposit policy are configured in Square Appointments. The **themed booking UI calls these routes** — do not use Square's hosted booking widget.

---

## 5. Store checkout (the secure path)

**Catalog** — fetch products server-side (`square.catalog.list`/`search` for `ITEM` objects), render PLP/PDP, cache. The cart on the client holds **catalog object IDs + quantities only** — never prices.

**Client — tokenize the card with the Web Payments SDK** (the only client-side Square code):
```html
<script src="https://web.squarecdn.com/v1/square.js"></script>
```
```ts
const payments = window.Square.payments(
  process.env.NEXT_PUBLIC_SQUARE_APPLICATION_ID!,
  process.env.NEXT_PUBLIC_SQUARE_LOCATION_ID!,
);
const card = await payments.card();
await card.attach("#card-container");
const { token } = await card.tokenize();   // -> sourceId; raw card data never touches your server
// POST { items: [{ catalogObjectId, quantity }], sourceId, customer } to /api/checkout
```

**Server — build the order from the catalog, then charge the server-computed total** — `app/api/checkout/route.ts`:
```ts
import { randomUUID } from "crypto";
import { square, LOCATION_ID } from "@/lib/square";

export async function POST(req: Request) {
  const { items, sourceId, customerId } = await req.json();   // NOTE: no prices accepted from client

  // Square prices the order from the catalog — the client cannot dictate amounts.
  const { order } = await square.orders.create({
    idempotencyKey: randomUUID(),
    order: {
      locationId: LOCATION_ID,
      lineItems: items.map((i: any) => ({ catalogObjectId: i.catalogObjectId, quantity: String(i.quantity) })),
    },
  });

  const { payment } = await square.payments.create({
    sourceId,
    idempotencyKey: randomUUID(),
    amountMoney: order!.totalMoney!,        // server-computed total, BigInt cents
    orderId: order!.id,
    locationId: LOCATION_ID,
    customerId,
  });

  return Response.json({ status: payment!.status, orderId: order!.id });
}
```
This is the bulletproof pattern: amounts come from Square's catalog, every create carries an `idempotencyKey` (no double charges on retry), and tokenization keeps you in the lightest PCI scope (**SAQ A** — card data never hits your server).

---

## 6. Webhooks — `app/api/webhooks/square/route.ts`

Verify **against the raw body** (read `text()`, not `json()`), or every event is spoofable:
```ts
import { WebhooksHelper } from "square";

export async function POST(req: Request) {
  const body = await req.text();                                  // RAW body — required
  const ok = await WebhooksHelper.verifySignature({
    requestBody: body,
    signatureHeader: req.headers.get("x-square-hmacsha256-signature")!,
    signatureKey: process.env.SQUARE_WEBHOOK_SIGNATURE_KEY!,
    notificationUrl: process.env.SQUARE_WEBHOOK_NOTIFICATION_URL!,
  });
  if (!ok) return new Response("invalid signature", { status: 403 });

  const event = JSON.parse(body);
  switch (event.type) {
    case "payment.updated": /* fulfill / notify */ break;
    case "order.updated":   break;
    case "booking.updated": break;
    case "inventory.count.updated": break;
  }
  return new Response("ok");
}
```
Subscribe to those events in the dashboard and set the notification URL to the route above.

---

## 7. Gift cards · loyalty · customers

`square.giftCards`, `square.loyalty`, `square.customers`. One customer record spans booking, online store, and in-person POS — pass the same `customerId` through booking and checkout so the relationship is unified.

---

## 8. Non-negotiable security rules

1. **Access token + webhook key are server-only.** Never `NEXT_PUBLIC_`, never in the client bundle.
2. **Never trust client-sent amounts.** Build every order from catalog IDs server-side; charge `order.totalMoney`.
3. **Idempotency key (`randomUUID()`) on every create/payment** — prevents duplicate charges on retry.
4. **Verify webhook signatures against the raw body**; 403 on mismatch.
5. **Sandbox first**, then swap env + tokens for production. No live keys in development.
6. **Money is `BigInt`, in cents.** Don't pass floats.
7. Handle `SquareError`; rely on the SDK's built-in retry/backoff; respect rate limits.
8. HTTPS only; tokenize with the Web Payments SDK so raw card data never reaches your server.

---

## 9. File / route structure for Claude Code to scaffold

```
lib/square.ts
app/api/bookings/availability/route.ts
app/api/bookings/create/route.ts
app/api/catalog/route.ts            (or server-component fetch)
app/api/checkout/route.ts
app/api/webhooks/square/route.ts
components/booking/BookingFlow.tsx   (themed UI -> /api/bookings/*)
components/store/Checkout.tsx        (Web Payments SDK card mount -> /api/checkout)
.env.example
```

**Ready-to-paste Claude Code prompt:**
> "Scaffold a headless Square integration in this Next.js (App Router) project per the attached spec. Create the server client, the bookings availability/create routes, a catalog fetch, a secure checkout route that builds the order from catalog IDs server-side and charges `order.totalMoney` with an idempotency key, and a webhook route that verifies the signature against the raw body. The themed BookingFlow and Checkout components call these routes — do not use Square's hosted widgets. Use `square@^40` (`SquareClient`). Enforce the security rules in §8: token server-only, never trust client prices, idempotency keys, verified webhooks. Validate exact method signatures against current Square docs as you go, since the API version moves."

---

## 10. Sandbox → production checklist

- Swap `SQUARE_ENVIRONMENT` + access token; regenerate the webhook signature key for production.
- Re-create bookable services/team members in the production catalog; re-confirm Google Calendar sync.
- Point the production webhook subscription at the live notification URL.
- Verify a sandbox booking, a sandbox card payment (declined + approved test cards), and a signed webhook before going live.
