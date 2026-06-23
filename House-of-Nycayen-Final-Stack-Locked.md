# House of Nycayen — Final Technical Stack (LOCKED)

Supersedes the open platform/scheduler decisions in `House-of-Nycayen-Components-and-Decisions.md`. Cosmetics is out of scope; the product line is **hair care + hair accessories** (secondary to services).

## The decision

**Square is the unified operational backbone. The site is built headless against Square's APIs — never Square's hosted widgets.**

Rationale: a services-primary business with a real in-person studio + a small physical product line is exactly Square's strength (appointments + best-in-class POS + one customer/payment record across online and in person + native gift cards & loyalty). With cosmetics removed, Shopify's heavier e-commerce/compliance machinery now solves a problem this business doesn't have, while its lack of native booking/POS would force the fragmentation we're avoiding.

## What Square owns

| Function | Square mechanism | Build approach |
|---|---|---|
| Appointments | **Bookings API** (availability, create/manage) | Custom themed booking UI; **syncs to Google Calendar** so the owner manages from Google |
| In-person service + retail | **Square POS** + hardware | Native; unifies in-person with online |
| Online checkout | **Web Payments SDK** + Payments API | Custom PCI-light checkout themed to the site (tokenize client-side, charge server-side) |
| Product catalog / stock | **Catalog + Inventory APIs** | Hair care + accessories; one inventory across online + in person |
| Cart / orders / tax / discounts | **Orders API** | Custom cart on the Next.js storefront |
| Customers | **Customers API** | One record across booking, online, in-person |
| Gift cards / loyalty | **Gift Cards + Loyalty APIs** | Native programs (bridal gifting, repeat clients) |

Wire server-side with Square's Node SDK via Vercel serverless functions.

## The honest tradeoff

Square's **headless commerce developer experience is less turnkey than Shopify's Storefront API** — you build more of the catalog/cart/checkout plumbing yourself. That is the price of one unified system + full front-end theming. For a dev-led bespoke build, it's worth paying. Do **not** fall back to Square's hosted booking widget or Square Online site builder — they cannot carry the couture experience and would undercut the whole project.

## Future hedge

The store is secondary now. If online product retail later becomes a serious DTC channel, **add Shopify for the store alone at that point** (clean, additive) and keep Square as the service/booking/POS hub. Do not pre-build Shopify for a future that may not arrive.

## Non-Square pieces (best-of-breed — don't force Square here)

| Layer | Pick | Why not Square |
|---|---|---|
| CMS (editorial self-management) | **Sanity** | Square has no editorial CMS |
| Email / lifecycle | **Klaviyo** | Better for the product-launch roadmap; Square Marketing is the unified-but-weaker fallback |
| Analytics | **GA4** (or Plausible/Fathom) | Track booking + cart as conversions |
| Instagram feed | **Graph API** / maintained widget | Account must be **Professional**; cache; no inline comments |
| Reviews | **Google Business Profile + Yelp** | Display + review schema |
| Hosting | **Vercel** | Claude Design → Claude Code → deploy |

## Updated decisions to lock

- **Platform:** Square backbone — LOCKED.
- **Booking:** Square Bookings API (custom UI) + Google Calendar sync — LOCKED (supersedes Cal.com).
- **Store:** Square headless (Catalog/Orders/Web Payments) — LOCKED; full structure built now, products added later.
- **Email:** Klaviyo (recommended) vs Square Marketing (unified-now) — confirm.
- Still open from prior docs: NAP reconciliation, font licensing, credentials verification, logo/SVG.
