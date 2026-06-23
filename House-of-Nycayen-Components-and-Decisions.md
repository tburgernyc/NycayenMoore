# House of Nycayen — Components, Integrations & Decisions

Addendum to `House-of-Nycayen-Claude-Design-Build-Package.md`. Confirms the full component set, locks the integration decisions, and adds what a complete, value-added build needs beyond the visual experience.

---

## 1. Required-component coverage (client's list → status)

| Client requirement | Status | Where |
|---|---|---|
| Appointment scheduling | ✔ Covered + decided | §8 + §2 below |
| Live Instagram feed (@nycayenmoore) | ✔ Covered | Package §8 |
| Products / store page (sell products) | ✔ Covered + decided | Package §7 + §3 below |
| About page | ✔ Covered (display name "The House") | Package §7 |
| Services page (the 4 services) | ✔ Covered (display name "Categories") | Package §6/§7 |
| Blog page | ✔ Covered (display name "Journal") | Package §7 |
| Gallery page | ✔ Covered (display name "Lookbook") | Package §7 |
| Contact page *(not listed — essential)* | ✔ Covered | Package §7 |
| AI concierge *(bonus, already specced)* | ✔ Covered | Package §8 |

Nothing on your list is missing. Section 5 below adds the components a complete build needs.

---

## 2. Appointment scheduling — the decision

**Verdict: use a dedicated scheduler as the front-end, synced *to* Google Calendar as the backend. Do not use Google Calendar's own booking page as the client-facing component.**

Why: Google Calendar does have a built-in appointment-scheduling feature, but its booking page is Google-branded, barely customizable, weak on intake fields, and impossible to theme to a dark couture aesthetic — it would look like a different website bolted onto the experience. Worse, it can't cleanly run the price-blind, consultation-type intake we designed.

The right architecture gives you the Google Calendar benefit *without* the limitation: a real scheduler embeds a themed booking flow on the site **and two-way-syncs to the owner's Google Calendar**, so every booking lands in the calendar he already lives in (no double-booking, appointments on his phone), while clients get a branded experience.

**Recommended: Cal.com.** Open-source, fully white-label/themeable to the dark palette, embeds inline or as a modal, two-way Google Calendar sync, custom intake questions and routing, supports the consultation types and reminders, developer-friendly. It's the only option that matches the luxury look *and* syncs to Google Calendar.

**Alternatives (his call):**
- **Square Appointments** — choose this if the store also runs on Square (see §3); it unifies booking + deposits + product payments + inventory in one system. Strong operational simplicity; less brand-flexible than Cal.com.
- **Calendly / Acuity** — plug-and-play and reliable, but Calendly carries its own branding/limited theming on standard tiers; Acuity (Squarespace) is more flexible and good with intake/packages. Fine fallbacks if zero-maintenance polish matters more than full theming.

**Add to the booking flow regardless:** consultation types (Discovery Call / In-Studio / Bridal-Event / On-Location), the qualifying intake, email + **SMS** reminders (he uses call/text), and an **optional deposit + cancellation policy** to cut no-shows on a bespoke service.

---

## 3. Store / products — the decision

Build the **full e-commerce structure now** (catalog/PLP, product detail/PDP, cart, checkout, account) even though products aren't confirmed, with a tasteful "Atelier — launching soon" state until they are. Don't stub it; structure it so adding products later is data entry, not a rebuild.

**Recommended backend: Shopify (headless).** The client is heading toward a **private-label hair + skincare line** — that's a real product business (inventory, variants, payments, shipping, taxes, and eventually subscriptions for replenishment). Shopify is built for exactly this, and a **headless Shopify storefront** (Storefront API) themes perfectly to the luxury site while Shopify runs commerce underneath.

**Alternative:** **Square** — if you also pick Square Appointments, running the store on Square Online/commerce unifies bookings, deposits, payments, inventory, and gift cards in one operational stack. Simpler to run; less bespoke than headless Shopify.
**Lightweight option** (if the store stays small): Snipcart or Shopify Buy Buttons on the existing pages.

**Before any skincare/cosmetics sells (flag, not legal advice — I'm not a lawyer):** US cosmetics now fall under **MoCRA** (facility registration, product listing, safety substantiation, labeling/INCI requirements), plus FDA labeling rules, no drug claims, product-liability insurance, and sales-tax setup. Have the client engage a cosmetics-compliance specialist before launch. Hair tools/accessories are simpler; topical products are not.

---

## 4. CMS — so the client owns the content

A luxury site the client can't update is a liability. Add a **headless CMS so they self-manage** the Journal, Lookbook, testimonials, and copy without a developer:
- **Sanity** (recommended) for editorial content (Journal, Lookbook, page copy) — themeable studio, great with Next.js.
- **Shopify** owns the product catalog.

This is the difference between a site that stays current and one that's frozen the day it ships.

---

## 5. Recommended additions (my discernment)

**Build now (a complete site needs these):**
- **Contact page** — NAP, embedded map, call/text/email, hours, message form. (Essential for conversions and local SEO.)
- **Email capture / newsletter (Klaviyo)** — start building the owned audience **today**, so there's a list to launch the private-label products to later. This is the single highest-ROI addition for the product future.
- **Reviews / testimonials** — pull real **Google Business Profile + Yelp** reviews and display them, with **review schema** for rich results. Replaces the generic placeholder testimonials honestly.
- **FAQ page/section + FAQ schema** — uses the existing FAQ copy; earns Google rich results and lightens the concierge.
- **Legal & commerce policy pages** — Privacy Policy, Terms, and (once the store is live) Shipping / Returns / Refunds. Required by payment processors and privacy law.
- **Analytics + conversion tracking** — GA4 (or privacy-first Plausible/Fathom), with booking and add-to-cart tracked as conversions. You can't optimize what you don't measure.
- **Accessibility statement + cookie/privacy consent** — WCAG/ADA and CCPA exposure are real for consumer sites; the build is already AA, so state it and add consent for analytics/IG.

**Strong value-adds:**
- **Gift cards** — real revenue, ideal for bridal gifting; native to Shopify/Square.
- **Press / "As seen in"** — once credentials are verified, an authority strip (Fashion Week, editorial, red-carpet) lifts trust for a fashion audience.

**Future (flag, don't build yet):**
- **Product subscriptions** (skincare replenishment) via Shopify subscriptions.
- **Loyalty / membership** for repeat clients.

---

## 6. The operational integration stack

| Layer | Pick | Notes |
|---|---|---|
| Booking | **Cal.com** | ↔ two-way Google Calendar sync, themed embed, custom intake |
| Commerce | **Shopify (headless)** | Storefront API; themed to the site; or Square to unify |
| CMS | **Sanity** (+ Shopify catalog) | client self-manages editorial |
| Email | **Klaviyo** | newsletter + future product flows |
| Reviews | Google Business Profile + Yelp | display + review schema |
| Instagram | Graph API or maintained widget | account must be **Professional**; cache; no inline comments |
| Analytics | **GA4** (or Plausible/Fathom) | track booking + cart conversions |
| Payments | Shopify Payments / Stripe | deposits via scheduler |
| Hosting | **Vercel** | from Claude Design → Claude Code → deploy |

---

## 7. Page naming

The package uses editorial display names (The House, Categories, Journal, Lookbook). You can keep those as **nav labels** while the underlying pages/URLs stay conventional and SEO-clear (`/about`, `/services`, `/blog`, `/gallery` or `/lookbook`, `/store`, `/contact`, `/book`). Or use literal labels throughout — your call. Recommendation: editorial labels in the nav, conventional URLs underneath, so you get the brand voice *and* the search clarity.

---

## 8. Updated decisions to lock

Carry over the package's list (NAP, fonts, credentials, logo) and add:
1. **Scheduler:** Cal.com (default) vs Square (if unifying with the store).
2. **Commerce backend:** Shopify headless (default) vs Square (unify) vs lightweight.
3. **CMS:** confirm Sanity for editorial self-management.
4. **Email platform:** Klaviyo (recommended) and start capture at launch.
5. **Analytics:** GA4 vs privacy-first; define the conversions to track.
6. **Skincare compliance:** engage a cosmetics specialist before any topical-product launch.
