# Nycayen Moore — Website Rebuild Build Brief

**For:** Claude Design (UI build) · **Prepared by:** Burger Consulting
**Subject:** Full rebuild of nycayen.com — a bespoke, travel-ready hair atelier (hair replacement, bridal, men's grooming, styling) headquartered in NYC.
**Single job of this site:** Get high-intent visitors to **book a consultation**. Everything else serves that.

> **Strategic note for the builder.** This is a conversion-driven local/luxury service site, not an art installation. Optimize for mobile, speed, crawlability, and an effortless booking path. **Do not build a WebGL/3D scroll experience, parallax scroll-scrubbing, or scroll-hijacking of any kind.** Luxury here is expressed through typography, photography, restraint, and motion discipline — not 3D. Tasteful motion only (CSS / Framer Motion), and `prefers-reduced-motion` must be fully respected.

---

## 1. What we keep from the existing site

Carry these assets and content forward (decoupled from TurnCage):

- **Logo / wordmark** (currently a PNG — request a vector/SVG from the client if available).
- **Photography** (hero and service imagery — confirm we have high-res rights; commission a shoot if not).
- **Page architecture:** Home, Salon Services (overview) with sub-pages **Hair Replacement, Hair Styling, Men's Grooming, Bridal Services**, plus **About, Contact, Blog, Store**.
- **Brand voice line:** *"Elevate Your Look, Elevate Your Life."* (fix the missing space — the current site renders "Look,Elevate").
- **About ethos:** *"Radiate Confidence."*
- **Verified business facts (NAP):** 265 W 37th St, 4th flr, New York, NY 10018 · (347) 556-3860 (call/text) · info@nycayenmoorenyc.com. Hours: Mon–Fri 10AM–6PM, Sat & Sun closed.
- **Differentiators:** decades of experience, specialization in hair replacement / human-hair extensions / men's toupees, bridal and event styling, all hair types and textures, **and travel anywhere in the U.S.**
- **Social matrix:** Instagram **@nycayenmoore**, YouTube **@Nycayen**, Facebook, X/Twitter, LinkedIn, Yelp, Pinterest.

**Replace, don't carry forward:** the dumb contact-form-as-booking; the five generic unattributed testimonials; the junk locale params on social links.

---

## 2. Design direction

### 2.1 Concept

An **editorial atelier** — the digital equivalent of a fashion house lookbook. Dark, warm, and quiet, with large photography and confident serif type. The reading experience should feel like turning the pages of a print magazine, not scrolling a brochure. The brand's own reels and transformations are the visual proof; the type and spacing are the luxury.

### 2.2 Signature element (the one thing this site is remembered by)

A **margin index system**: a thin vertical hairline rule runs down the content column, and each major section is marked by a small-caps, letter-spaced **champagne eyebrow label** (e.g. `ATELIER — 01`, `BRIDAL — 04`) sitting in the left margin like a couture lookbook index. Paired with **one orchestrated hero moment**: the brand video/imagery scales and crossfades slowly behind the wordmark on load (CSS/Framer Motion, ~1.2s, reduced-motion-safe). Spend boldness here; keep everything else disciplined.

> Builder guidance: numbered eyebrows are only justified because the homepage *is* a sequence (atelier → services → proof → booking). Do not sprinkle 01/02/03 decoratively elsewhere.

### 2.3 Color tokens (dark / warm / taupe — contrast verified for WCAG 2.1 AA)

Warm espresso base, not generic near-black. Champagne/brass is the single metallic accent. Contrast ratios below are computed against `--bg-base`.

| Token | Hex | Role | Contrast on base |
|---|---|---|---|
| `--bg-base` | `#1A1512` | Page background (warm espresso) | — |
| `--bg-surface` | `#241D18` | Raised panels / alternating sections | — |
| `--bg-surface-2` | `#2E2620` | Cards, inputs, chat surface | — |
| `--text-primary` | `#F5F0E8` | Headings & body (warm ivory) | **≈ 16:1** (AAA) |
| `--text-secondary` | `#B8AC9C` | Subtext, captions, long-form (taupe-gray) | **≈ 8:1** (AAA) |
| `--accent-champagne` | `#C9A86A` | Eyebrows, hairlines, icons, **primary CTA fill** | **≈ 8:1** (AA text both directions) |
| `--accent-champagne-soft` | `#D4BD8A` | Hover / large display accents | higher |
| `--line` | `rgba(201,168,106,0.22)` | Hairline dividers / margin rule (decorative) | n/a |
| `--taupe` | `#8A7A68` | Image framing, muted UI on dark | accent use |
| `--sand` | `#D9CFC2` | Optional light accent / image mats | accent use |

**Rules:** primary text uses `--text-primary`; secondary/captions use `--text-secondary` (both clear AA). Champagne passes AA for normal text on the base, **but the builder must re-verify any new champagne-on-color or text-on-champagne pairing with a contrast checker and bump to `--accent-champagne-soft` if it drops below 4.5:1.** Never use color as the sole signifier of state.

### 2.4 Typography

A deliberate pairing — chic, high-contrast, expensive. **Not Playfair** (overexposed and reads templated).

- **Display — `Fraunces`** (variable; SIL OFL, free). Use its high optical-contrast / soft / "wonky" axes for an editorial, couture feel. Weight ~340–460, tight leading (1.0–1.05), slightly negative letter-spacing on large sizes.
- **Body / UI — `Hanken Grotesk`** *(or `Geist Sans`)* (both free). Quiet, warm, highly legible on dark. Leading 1.6 for long-form.
- **Utility / eyebrow — small-caps from the body face**, `letter-spacing: 0.14em`, `0.8125rem`, champagne. Used for the margin index labels and section eyebrows.

**Premium upgrade option (client budget decision):** swap the display face for a paid foundry serif — `Canela`, `GT Sectra`, or `Reckless`. These carry per-traffic web licensing fees; do not ship a foundry font without a paid license sized to traffic. (Ogg, named in the original concept, is paid Sharp Type — same caveat.)

**Fluid type scale (`clamp`):**

| Role | Size | Notes |
|---|---|---|
| Hero | `clamp(2.75rem, 6vw, 5.5rem)` | Fraunces, leading 1.02 |
| H2 | `clamp(2rem, 4vw, 3.25rem)` | Fraunces |
| H3 | `clamp(1.375rem, 2.2vw, 1.875rem)` | Fraunces or body-semibold |
| Body-lg | `1.125rem` | intros |
| Body | `1.0625rem` | leading 1.6 |
| Eyebrow / caption | `0.8125rem` | small-caps, tracked |

Self-host fonts (or `next/font`), `font-display: swap`, preconnect. No layout shift.

### 2.5 Spacing, radius, motion

- **Spacing** (8px base): 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 96 / 128. Generous vertical rhythm between sections (min 96px desktop).
- **Radius** (restrained = expensive): `--radius-sm: 4px`, `--radius-md: 8px`. Buttons and cards stay low-radius; only the AI chat bubble may be softer. No pill buttons.
- **Motion:** 200–400ms, ease `cubic-bezier(0.22, 1, 0.36, 1)`. Scroll reveals = gentle fade + 16px rise, once. Hover micro-interactions on cards/links. **No parallax, no scroll-jacking, no autoplay motion when reduced-motion is set.** One orchestrated hero load sequence is the ceiling.

---

## 3. Information architecture & page specs

### 3.1 Home (sections, top to bottom)

1. **Header / nav** — sticky, transparent over hero then solid `--bg-surface` on scroll. Wordmark left; nav (Salon Services ▾, About, Contact, Blog, Store, cart). Persistent **"Book a Consultation"** button (champagne) on the right at all breakpoints. Mobile: full-screen drawer; booking CTA pinned.
2. **Hero** — art-directed full-bleed image *or* the existing brand reel (`youtube.com/embed/aKeGhnZv1wc`) as a muted, looping, lazy-loaded background with a poster image. Wordmark + "Elevate Your Look, Elevate Your Life." Single **primary CTA "Book a Consultation"** + secondary text link **"Call or text (347) 556-3860."** One line establishing who/where: *NYC atelier · available across the U.S.*
3. **Trust strip** — quiet band: *Established 2000 · New York City · By appointment & on location nationwide.* (If verified with client: Fashion Week / editorial / red-carpet credentials — see §7.)
4. **Services overview (`ATELIER — 01`)** — four editorial cards: **Hair Replacement, Hair Styling, Men's Grooming, Bridal Services.** Image + name + one-line description + "Explore →" to the sub-page. **No prices.**
5. **About teaser (`THE STYLIST — 02`)** — "Radiate Confidence." Short, specific story (Nyck Moore; decades; specialization in hair replacement and human-hair work; fashion pedigree). CTA to /about.
6. **Instagram reels feed (`IN MOTION — 03`)** — live recent Reels from @nycayenmoore, native dark-themed cards (see §5). Header: "Recent work." Link to the profile.
7. **Bridal & events (`BRIDAL — 04`)** — the wedding/events grid, re-styled editorially (Wedding Perfection, Trendsetting Styles, Versatile Looks, Expert Techniques, Special Occasion Styling, Quality Products).
8. **The consultation model (`HOW IT WORKS — 05`)** — *turn the missing price list into a feature.* Explain plainly: every service is custom to hair type, goals, location, and event, so pricing is quoted after a consultation. 3-step path: **Consult → Plan → Transform.** Removes the "where are the prices?" friction.
9. **Testimonials (`CLIENTS — 06`)** — real, attributed reviews (replace the generic set; see §7). Quiet quote treatment, champagne quote mark.
10. **Final booking CTA** — full-width, warm: "Begin with a consultation." Primary button → scheduler. Secondary → call/text.
11. **Footer** — NAP, hours, Google Map link, full social row (clean URLs — strip the `?locale`/`?lang` junk), copyright.
12. **Persistent AI concierge** — dismissible launcher, bottom-right (see §6).

### 3.2 Service sub-pages (×4)

Consistent template: hero (service name + editorial image), an honest description of the service and who it's for, "what to expect" (consultation-led), supporting imagery / mini-gallery, FAQ relevant to that service, and a **"Book a Consultation"** CTA. **No pricing.** Hair Replacement should speak to wigs, men's toupees, and human-hair systems specifically; Bridal should capture event-date and travel context.

### 3.3 About

Stylist story, philosophy, credentials (verified), team if applicable, and the nationwide-travel differentiator. CTA to booking. This page also carries the brand's local-SEO weight — real text, not images of text.

### 3.4 Contact

Real NAP, embedded map, call/text/email actions, hours, and a lightweight message form (kept *in addition to* booking, for users who don't want to schedule). Form: name, email, phone, message + honeypot + spam protection. Clear success/empty/error states in the interface's voice.

### 3.5 Blog

Editorial index + article template (hair care, wig & toupee maintenance, bridal prep, trends). Supports SEO and feeds the AI concierge's knowledge. Clean typographic reading experience using the type scale above.

### 3.6 Store

Preserve commerce capability (accessories / hair-care / wig products). **Decision required** on what powers it post-TurnCage (see §8). Build the UI to a standard PLP/PDP/cart pattern themed to the tokens; keep it minimal and on-brand. If commerce is deferred, design a tasteful "Shop — coming soon" state rather than a broken store.

---

## 4. Booking & consultation funnel (price-blind)

The core conversion surface. **Primary action everywhere is "Book a Consultation," never a priced service.**

**Flow:**
1. **Choose consultation type** — e.g. *Discovery Call* (phone/virtual, 15–20 min) · *In-Studio Consultation* · *Bridal/Event Consultation* · *On-Location (travel) Inquiry*.
2. **Pick date/time** from live availability.
3. **Intake (this replaces the dumb "Subject" dropdown with real qualification):** location (NYC vs travel + city), service interest, hair type/texture, event date (if bridal/event), optional photo upload, notes. This gives the owner everything needed to quote manually.
4. **Confirm** → confirmation screen + email/SMS reminders.
5. Optional **deposit** to reduce no-shows (owner decision).

**Always-visible fallback:** call/text/email for high-intent users who won't schedule online.

**Backend (integration point — see §8 for the decision):** embed a themable scheduler — **Cal.com** (open-source, fully themable to the dark palette, self-hostable) is the recommended default; **Square Appointments** is a strong alternative if the client already uses Square (supports deposits and is in the client's tooling). Build the UI to accept an embedded/headless scheduler; style it to match tokens. Do not hardcode a vendor into the layout.

---

## 5. Instagram reels feed (2026-correct spec)

**Goal:** show @nycayenmoore's most recent Reels natively, in the dark theme.

**Hard constraint (changed since the original concept):** Instagram's **Basic Display API was permanently shut down on December 4, 2024.** Personal accounts can no longer be read by any official API. To display the feed:

- **@nycayenmoore must be converted to a Professional account** (Creator is sufficient and free; Business + a linked Facebook Page unlocks more).
- Use the **Instagram API with Instagram Login** (Creator, no Facebook Page needed) **or the Instagram Graph API** (Business + Page).
- **Access tokens expire (~60 days) and must be refreshed on a schedule (~every 30–50 days).** Rate limit ~200 calls/hour → **cache responses server-side** (revalidate hourly/daily). A public production integration needs Meta **App Review / Advanced Access**.

**Two build paths (client decides — §8):**
- **(A) Maintained widget** (e.g. Behold, EmbedSocial, Elfsight-class) — handles tokens/refresh/App Review for a monthly fee; least maintenance, some styling limits. Style to match tokens as closely as the widget allows.
- **(B) Custom** — Next.js server route calls the Graph API, caches, and renders native dark reel cards (poster + play affordance + caption). Full design control; **you own token refresh, caching, and App Review.** Recommended for the luxury look, with the maintenance burden flagged honestly to the client.

**Scope:** show reels + play/like counts and link out to Instagram. **Do not render raw Instagram comments inline** — UGC on a luxury homepage is a moderation liability. If comments are a hard requirement, they must be curated/moderated.

---

## 6. AI concierge (scoped, not a gate)

A **supporting** feature, not the path to booking. Persistent, dismissible launcher; opens a dark-themed chat panel.

**Scope (what it answers):** services offered; wigs, men's toupees, and human-hair extensions; wig/hair/toupee care and maintenance; hair accessories; general "how it works" and travel/availability questions; routing to booking.

**Guardrails (must be enforced in the system prompt):**
- **Never quotes or estimates prices.** If asked, it explains the consultation model and offers to book.
- Stays in scope (hair/services). Politely declines off-topic requests.
- For anything it can't confidently answer, it **hands off to booking or call/text** — never guesses.
- Grounded on the site's real content (FAQ, service pages, blog) via system-prompt context / retrieval — not "trained on five FAQs."

**Build:** chat UI per tokens (softer radius allowed here), accessible (focus trap when open, ARIA roles, Escape to close, visible focus). Wire to the Claude API as an integration point (the UI build can stub responses; backend supplies the key and grounding). The launcher and panel must lazy-load and never block initial render.

---

## 7. Content & copy

- **Elevate the voice.** Current copy is generic salon hype ("tired of boring hairstyles," "turn heads"). Rewrite toward atelier register: specific, confident, plain — describe the craft and the experience, not adjectives. Keep the two brand lines ("Elevate Your Look, Elevate Your Life." / "Radiate Confidence.").
- **Testimonials:** replace the five generic unattributed quotes with **real, attributed reviews** pulled (with permission) from Yelp/Google. Authentic > polished.
- **Verify claims before publishing.** Legacy bios reference Fashion Week (Byron Lars), WEEN red-carpet styling, and celebrity/editorial work. Confirm with the client and keep wording defensible — no unverifiable celebrity name-drops (false-advertising risk).
- **Microcopy:** active voice, sentence case, consistent action labels through a flow (the "Book a Consultation" button leads to a flow that keeps saying "consultation," and confirmation says "Consultation booked"). Errors explain and direct; empty states invite action.

---

## 8. Decisions to lock before build

1. **Canonical address (NAP).** Confirm 265 W 37th St vs the stale Yelp listing (224 W 35th St). Fix everywhere + Google Business Profile — this is a live local-SEO bug.
2. **Scheduler:** Cal.com vs Square Appointments vs Acuity/Calendly. Consultation types, durations, deposit yes/no.
3. **Instagram:** convert @nycayenmoore to Professional **now**; choose widget (A) vs custom (B); confirm comments excluded.
4. **Typography budget:** free pairing (Fraunces + Hanken Grotesk/Geist) vs paid foundry display (license cost).
5. **Store:** keep commerce? What powers it (Shopify headless / Snipcart / Square)? What's sold?
6. **Testimonials & photography:** secure real reviews and confirm rights/high-res imagery, or commission a shoot.
7. **Credential claims:** which are verified and approved for publication.
8. **Logo:** obtain vector/SVG.

---

## 9. Quality floor (non-negotiable)

- **Accessibility — WCAG 2.1 AA.** Keyboard operable; visible focus (champagne outline); `prefers-reduced-motion` disables reveals and video motion; alt text on all imagery; captions on video; labeled form fields; ≥44px touch targets; color never the sole signifier; chat panel fully accessible.
- **Performance (mobile-first).** Targets: **LCP < 2.5s, CLS < 0.1, INP < 200ms.** `next/image` for all media; lazy-load below-fold; defer the IG feed and chat; self-host fonts (swap + preconnect); hero video lazy + poster + muted/inline. **No WebGL/3D.**
- **SEO / local.** Server-rendered crawlable content; preserve/improve titles, meta descriptions, OG tags; add **`LocalBusiness` / `HairSalon` JSON-LD** with consistent NAP, hours, geo; semantic heading order; sitemap.xml + robots.txt; fast mobile. (This is precisely what a 3D build would have destroyed.)

---

## 10. Recommended stack (for alignment)

Next.js (App Router) + React · Tailwind or CSS variables for the token system · Framer Motion for restrained motion · `next/image` · embedded scheduler (Cal.com / Square) · Instagram via maintained widget or server-side Graph API with caching · Claude API for the concierge · deploy on Vercel.

**For the Claude Design UI build specifically:** deliver the responsive front-end and components per this brief; treat the scheduler, Instagram tokens, and AI API key as integration points to stub. The build must be a clean, modern, content-driven UI — no 3D engine.
