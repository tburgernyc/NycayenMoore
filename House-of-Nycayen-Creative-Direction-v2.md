# House of Nycayen — Creative Direction & Build Brief (v2)

**For:** Claude Design (UI build) + creative front-end developer
**Supersedes:** the previous "conservative" brief. That brief was miscalibrated — it treated this as a generic local salon and prioritized restraint over expression. This is an **artist** rooted in **NYC ballroom and fashion.** The site is a statement piece. It must perform.

**The bar:** stand next to apple.com (scroll craft, precision), louisvuitton.com (opulence, materiality, motion), Dover Street Market (avant-garde courage, anti-template), vogue.com (editorial authority, masthead type) — **without imitating any of them.** Originality is the point; award juries and the designers at those houses recognize a template or an AI-default layout on sight, so every choice here is rooted in *his* world, not a reference clone.

---

## 1. The thesis

**The site is a living fashion publication crossed with a ballroom runway — "House of Nycayen."**

Three worlds fused into one language:

- **Editorial fashion** — masthead typography, lookbook grids, fashion-credit captions ("Hair — Nycayen Moore / Look 04 / SS·26"), seasonal "volumes / issues," a contents/index. Authority and taste.
- **Ballroom theater** — the runway, the spotlight, the category being *called*, gloss, opulence, defiance, joy. Navigation is reframed as **categories**; the homepage is a **runway** the work walks down; the tone is performative, not polite.
- **Hair as sculpture** — the head as a form, texture and silhouette as the medium. The work is the content; photography and motion treat hair the way a fashion house treats a garment.

This concept is the differentiator. It is not "another dark luxury salon site." It's a point of view no template can fake, and it's *true to him* — which is exactly what makes it land with a fashion audience.

A working hero/cover concept demonstrating the art direction ships alongside this brief: **`nycayen-hero-concept.html`** — open it. It is the visual north star (dark stage, spotlight, film grain, iridescent accent, kinetic variable-serif masthead, ballroom-category index). Build from its language; it is a concept frame, not the final responsive system.

---

## 2. Art direction

### 2.1 Palette — dramatic, glossy, not timid

The previous taupe-on-espresso was too safe. This palette keeps warm dark sophistication but adds **spotlight, gloss, and an iridescent sheen** — opulence with a queer/ballroom charge, used with discipline.

| Token | Hex | Role |
|---|---|---|
| `--ink` | `#0E0B0C` | Base stage (warm near-black) |
| `--ink-2` | `#15100F` | Raised surfaces |
| `--oxblood` | `#3A1620` | Spotlight warmth / atmospheric glow |
| `--aubergine` | `#241326` | Secondary atmospheric tone |
| `--ivory` | `#F3ECE2` | Primary text (≈16:1 on ink — AAA) |
| `--muted` | `#B9A99A` | Secondary text (≈8:1 — AAA) |
| `--gold` | `#C8A45C` | Primary accent / CTA fill / credits |
| `--gold-soft` | `#E4CE97` | Hover / large display accents |
| **Iridescent** | `#E7B6C6 → #C8A45C → #9FD7C9 → #B9A6E0` | Oil-slick sheen: pink → gold → mint → lilac. **Decorative only** — eyebrows, hairline rules, transitions. Never body text. |

**Discipline:** the iridescent is the single bold move (per "spend boldness in one place"). Everything around it stays quiet and editorial. Spotlight = radial warm glow top-center (the runway light); grain = subtle film texture overlay; vignette darkens the edges. Re-verify any new accent-on-color text pairing at ≥4.5:1.

### 2.2 Typography — the masthead is the personality

- **Display — `Fraunces`** (variable; opsz / wght / **SOFT** / **WONK** / ital axes; SIL OFL, free). High optical contrast for couture authority, but the SOFT/WONK axes give it a subversive, hand-touched edge that suits ballroom's playful defiance — distinct from a straight Bodoni/Vogue clone. Used huge, tight (line-height ~0.86), with the wonk dialed in. Italic for descriptors.
- **Body / UI / nav — `Hanken Grotesk`** *(or `Archivo`)* — quiet, modern, legible on dark; carries eyebrows, nav, credits in tracked uppercase (`letter-spacing` 0.18–0.34em).
- **Kinetic type (current, on-brief):** animate Fraunces' variable axes — a weight/optical settle on load, a subtle weight shift on hover for headlines, cursor-reactive display moments. Oversized type + variable-font motion is exactly what's winning right now; make the type a *performance*, not a label.

**Premium option (budget decision):** swap display for a paid foundry didone/serif (`Canela`, `GT Sectra`, `Reckless`) — flag per-traffic licensing. Not Playfair (overexposed).

### 2.3 Photography & film — non-negotiable

Award-tier juries (and LV's designers) reject placeholder/stock instantly; thin content drags down everything around it. **This rebuild requires real, art-directed photography and motion of his work** — hair as couture, on-figure, in the gloss-and-shadow language above, plus his Reels and any runway/editorial footage. Budget a shoot as part of the build, not an afterthought. Treatments: deep duotone options, spotlight grade, grain, full-bleed plates with fashion-credit captions.

---

## 3. Signature experiences (what makes designers stop)

These are the moments. Each is **scroll-driven, never scroll-jacked** — the user always controls pace and direction (the distinction the previous brief blurred; the best award sites obey it).

1. **The cover / hero (WebGL, progressively enhanced).** A cinematic opening: art-directed hair imagery treated with a shader — liquid-gloss / iridescent distortion / a strand-filament field that reacts to cursor and ambient light (Three.js / WebGL, WebGPU where available for 60fps on heavier scenes). **Critical engineering:** the headline, CTA, and nav are real DOM (crawlable, accessible); the WebGL is a layer that **lazy-inits after LCP** and **degrades to the static art-directed hero** on low-power devices and under reduced-motion. Ambition *and* crawlability — the way the houses do it.
2. **The runway (category gallery).** The signature section: the work *walks* — a horizontal, scroll-linked sequence (GSAP ScrollTrigger `scrub`, pinned, reversible) where looks advance like models on a catwalk, with ballroom-emcee-style kinetic captions ("the category is… BRIDAL REALNESS"). Controlled motion, never a hijack.
3. **Editorial scroll reveals.** Image/type reveals as you progress (CSS scroll-driven `animation-timeline: view()` where supported, GSAP fallback) — magazine spreads assembling on scroll. Restraint: gentle, intentional, once.
4. **Cinematic page navigation (View Transitions API).** Cross-document view transitions morph between pages like a film cut / a runway change — e.g. a category card's image morphs into the sub-page hero via shared `view-transition-name`. Native, GPU-composited, accessible. Ship as **progressive enhancement** (`@supports`) since Firefox cross-document is still in progress; pair with Speculation Rules / prefetch so transitions feel instant and beat the ~4s navigation timeout.
5. **Material details.** Film grain, spotlight that tracks the cursor, magnetic CTAs, hover-reveal of looks, iridescent rules that shimmer. The accumulation of craft is the luxury.

---

## 4. Motion system & the one rule

- **The rule:** scroll-*driven*, never scroll-*jacked*. The user's scroll controls pace and can reverse; we never trap them or convert the wheel into a forced scrubber. (This is the legitimate half of the previous brief — kept.)
- Easing `cubic-bezier(0.22, 1, 0.36, 1)`; durations 250–600ms for UI, longer for orchestrated hero/runway beats.
- **`prefers-reduced-motion` fully respected:** WebGL → static hero, scroll animations off, view transitions off, autoplay motion off. This is a hard quality gate, not optional.
- Orchestrate; don't scatter. One hero load sequence, one runway, deliberate reveals. Over-animation reads as AI-generated — discipline is the tell of a real studio.

---

## 5. Information architecture (reframed as a House)

Keep the client's real pages; rename and restage them in the concept's language.

- **Home** — the cover: hero → trust/credentials line → **the runway** (categories) → the House (about teaser) → in motion (Instagram) → bridal & events editorial → the consultation model → final appointment CTA → footer.
- **Categories** (= Salon Services), each a sub-page styled as an editorial: **Hair Replacement** (wigs, men's toupees, human-hair systems), **Hair Styling**, **Men's Grooming**, **Bridal**. Honest description, who it's for, "what to expect," gallery, FAQ, appointment CTA. **No pricing.**
- **Lookbook** — the portfolio/gallery (his work, by season/volume). This is central for an artist; give it real weight.
- **The House** (= About) — Nyck Moore's story, philosophy, ballroom + fashion pedigree, credentials (verified — see §9), the nationwide-travel differentiator. Carries local-SEO weight with real, crawlable text.
- **Journal** (= Blog) — editorial articles (hair/wig/toupee care, bridal prep, culture, technique). Feeds the AI concierge and SEO.
- **Store** — accessories / hair-care / wig products. Decide the commerce backend (see §10). Themed to tokens; if deferred, a tasteful "coming soon," never broken.
- **Appointments** — see §6.

Navigation language: "Atelier · Categories · Lookbook · The House · Appointments." Keep nav minimal and editorial; the persistent action is **Request an appointment.**

---

## 6. Booking — "request an appointment with the House" (price-blind)

The core conversion surface, restaged in-concept. Primary action everywhere: **Request an appointment** — never a priced service.

- **Consultation types:** Discovery Call (phone/virtual) · In-Studio Consultation · Bridal/Event Consultation · On-Location (travel) Inquiry.
- **Flow:** choose type → live availability → intake that *qualifies* (location NYC vs travel + city, category of interest, hair type/texture, event date, optional photo upload, notes) → confirm → email/SMS reminders. Optional deposit to reduce no-shows (owner decision).
- This replaces the current dumb contact-form-as-booking. Keep a lightweight call/text/email fallback always visible.
- **Backend (integration point):** themable scheduler — **Cal.com** (open-source, fully themable to the dark palette) recommended; **Square Appointments** strong if he already uses Square (deposits, in his tooling). Build the UI to embed a headless scheduler; don't hardcode a vendor into layout.

The price-blind model isn't a gap — stage it as a feature on a "How the House works" beat: every service is bespoke to hair, goals, and occasion, so a look is quoted after consultation.

---

## 7. Instagram & AI concierge (carried forward, elevated)

- **Instagram (@nycayenmoore reels):** rendered natively in the dark theme — not a clunky iframe. **2026 constraint:** Basic Display API was retired Dec 4 2024; the account must be a **Professional (Creator/Business)** account, accessed via the Instagram API with Instagram Login or the Graph API, with **token refresh on schedule, server-side caching (~200 calls/hr limit), and App Review/Advanced Access for public production.** Path A: maintained widget (least maintenance, monthly fee, styling limits). Path B: custom server-side Graph API + native dark reel cards (full control; you own refresh/caching/review). For this brand, B is ideal — flag the maintenance honestly. **Do not render raw IG comments inline** (moderation liability on a luxury homepage).
- **AI concierge:** persistent, dismissible, dark-themed; in-concept it's "the House concierge." Scope: services, wigs/toupees/extensions, hair & wig care, accessories, how it works, travel/availability, routing to booking. **Guardrails (system prompt):** never quotes prices (explains the consultation model, offers to book); stays in scope; hands off to booking or call/text when unsure; grounded on real site content (Journal, category pages), not "trained on five FAQs." Accessible (focus trap, ARIA, Esc to close), lazy-loaded, never blocks render. Wire to the Claude API as an integration point.

---

## 8. (Reserved)

---

## 9. Engineering guardrails — ambition *and* rigor

The houses are ambitious **and** engineered. Both, or it fails.

- **Performance:** target 60fps on all motion; Core Web Vitals respected (**LCP < 2.5s, CLS < 0.1, INP < 200ms**) *despite* the experience. Techniques: code-split and lazy-init WebGL after LCP, level-of-detail / efficient textures, `next/image`, defer the IG feed and concierge, self-host fonts (swap + preconnect), prefetch/Speculation Rules for transitions. WebGPU where it buys headroom.
- **Accessibility — WCAG 2.1 AA, non-negotiable:** keyboard operable; visible focus (gold/iridescent ring); full reduced-motion fallbacks (§4); alt text; video captions; labeled forms; ≥44px targets; color never the sole signifier; concierge fully accessible. Avant-garde is not an excuse for inaccessible.
- **SEO / local:** server-rendered crawlable content (the WebGL is a layer, not the content); preserve/improve titles, meta, OG; **`LocalBusiness`/`HairSalon` JSON-LD** with consistent NAP; reconcile the address discrepancy (site: 265 W 37th St vs Yelp: 224 W 35th St) and align Google Business Profile; semantic headings; sitemap. An artist still gets found locally.
- **Real content:** see §2.3 — placeholder/stock is disqualifying at this tier.

---

## 10. Tech stack & honest build-tier reality

**Stack:** Next.js (App Router) · React · Three.js / WebGL (WebGPU where it helps) · GSAP (ScrollTrigger) + CSS scroll-driven animations · View Transitions API · Framer Motion for component motion · variable fonts (`next/font`) · headless scheduler (Cal.com / Square) · Instagram via maintained widget or server-side Graph API · Claude API concierge · deploy on Vercel.

**The honest part — read this before scoping.** This is **award-tier creative development**, not a one-click UI generation. Be clear with the client and with yourself about the two layers:

- **Claude Design can build, to a high standard:** the full editorial UI system (tokens, kinetic variable type, layout, the runway/lookbook grids, scroll reveals, cross-document View Transitions, component motion, a styled hero with a strong static + CSS treatment), responsive and accessible. That is most of the site, and most of the impression.
- **A creative front-end developer is needed for the bespoke top layer:** the custom WebGL/WebGPU shader hero, the pinned scroll-scrubbed runway physics, performance tuning to hold 60fps. Hand-coding or tight iteration territory — the houses you named all do this with creative-dev teams.

Brief Claude Design to build the ambitious UI foundation; flag the WebGL hero and runway as creative-dev integration points so the project is scoped (and budgeted) honestly rather than promised as a button-press.

---

## 11. Decisions to lock before build

1. **Photography/film:** commission the art-directed shoot (non-negotiable for this tier) — looks, on-figure, runway/editorial, the gloss-and-shadow grade.
2. **Build scope/budget:** confirm appetite for the WebGL/runway creative-dev layer vs. the (still strong) UI-foundation-only version.
3. **Typography:** free pairing (Fraunces + Hanken Grotesk) vs paid foundry display (licensing cost).
4. **Scheduler:** Cal.com vs Square Appointments; consultation types/durations; deposit yes/no.
5. **Instagram:** convert @nycayenmoore to Professional **now**; widget vs custom; comments excluded.
6. **Store:** keep commerce? backend (Shopify headless / Snipcart / Square)? what's sold?
7. **NAP:** confirm canonical address (265 W 37th vs Yelp 224 W 35th) and fix everywhere + Google Business Profile.
8. **Credentials:** which fashion/ballroom/editorial claims are verified and approved to publish.
9. **Logo:** obtain vector/SVG; consider a refined wordmark treatment fit for a masthead.
