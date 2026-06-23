# House of Nycayen — Claude Design Build Package

The single source of truth for building the site in Claude Design. Read §2 first — it's the playbook.

## 0. Package manifest

| File | What it is | How to use it |
|---|---|---|
| **This brief** | The build spec | Paste sections into Claude Design as you build each part |
| `House-of-Nycayen-AI-Asset-Prompt-Pack.md` | Prompts to generate every visual | Generate assets, then drop them into the slots in §6–§7 |
| `nycayen-webgl-scroll.html` | **Working homepage scroll experience** (Three.js field, Z-parallax, scrub, scrollytelling, the Sweep entry) | **Upload as the seed** — tell Claude Design to match/extend it |
| `nycayen-the-sweep.html` | The standalone hair-curtain hero | Seed for the landing/intro if used on its own |
| `House-of-Nycayen-Creative-Direction-v2.md`, `House-of-Nycayen-Scroll-Architecture.md` | Deeper creative + technical rationale | Optional background; the build itself is fully covered here |

---

## 1. Project at a glance

- **Client:** Nycayen Moore — a hair *artist* (men's & women's), rooted in NYC ballroom and the fashion industry. Specialties: hair replacement (wigs, men's systems, human-hair), bridal, men's grooming, editorial styling. NYC-based, travels nationwide.
- **The one job of the site:** drive **consultation bookings** through an experiential, editorial site that signals artist-tier credibility.
- **The bar:** stand beside louisvuitton.com / vogue.com / Dover Street Market / Apple product pages — without imitating any of them.
- **Concept:** **House of Nycayen** — a living fashion publication crossed with a ballroom runway. Editorial type + ballroom theater (services are "categories"; the homepage is a runway) + hair-as-sculpture.
- **The imagery rule (non-negotiable):** hair is shown as **medium, sculpture, material, gesture, and atmosphere — never as a fabricated client result.** No faces, no fake "before/afters," no real-but-not-his work. This is both an ethics line and the creative concept.

---

## 2. How to build this in Claude Design (the playbook)

Claude Design supports 3D/shaders/code prototypes and works best from references. Build in this order; don't try to one-shot it.

**Step A — Establish the design system.** Paste §4 (tokens) into Claude Design as the project's design system so every screen inherits the palette, type, spacing, motion. (If your org uses Claude Design's design-system import, load §4 as a raw upload.) Do **not** web-capture the current nycayen.com — we're replacing it, not matching it.

**Step B — Seed the homepage with the reference code.** Upload `nycayen-webgl-scroll.html` and instruct: *"Build the homepage to match and extend this reference exactly — same dark editorial aesthetic, the Three.js hair-filament field, the scroll-driven camera, the scrollytelling beats, and the in-curtain (Sweep) opening. Keep the progress bar, the persistent 'Request an appointment' CTA, and the reduced-motion static fallback."* References beat prose; this is the highest-leverage move in the whole build.

**Step C — Build in stages, one focused session each (it's token-heavy):**
1. **Homepage scroll experience** — refine fidelity, drop in real copy (§9) and the generated assets (§10 + prompt pack).
2. **Interior pages** — category template, The House, Lookbook, Journal, Contact, Appointments (§7), all on the §4 system, with cross-document View Transitions between them (§5).
3. **Components** — nav, booking flow, Instagram feed, AI concierge, footer (§8).

**Step D — Place the assets.** Generate visuals from the prompt pack, then drop them into the slots called out in §6–§7 and §10.

**Step E — Hand off to production.** Export standalone HTML or use Claude Design's **handoff to Claude Code**, then deploy on **Vercel**. WebGL fidelity/perf hardening and the live integrations (scheduler, Instagram API, concierge backend) are finished here, not in Claude Design.

**Practical notes:**
- It's a research preview and token-heavy — work in focused sessions, and use direct **canvas edits / sliders** for small tweaks instead of burning a full turn.
- It may ask clarifying questions (good — answer them). Known quirk: inline comments can vanish before it reads them; **paste comment text into chat** instead.
- To branch without losing work: *"Save what we have and try a different approach for [X]."*

**Ready-to-paste opening prompt for Claude Design:**
> "I'm building the homepage for 'House of Nycayen,' a luxury hair-artist site with a dark editorial / ballroom-fashion aesthetic. I'm uploading a working reference (`nycayen-webgl-scroll.html`) — build the homepage to match and extend it exactly: the Three.js hair-filament field you fly through on scroll (Z-axis parallax), scroll-linked camera scrubbing, scrollytelling beats (cover → the House → five categories → appointment), and the in-curtain opening. Use the attached design system for all color, type, spacing, and motion. Real copy and section content are in the attached brief. No photos of people anywhere — the hair is the world. Keep the scroll-progress bar, the persistent 'Request an appointment' button, and a reduced-motion static fallback."

---

## 3. The reference implementations (seed code)

- **`nycayen-webgl-scroll.html`** — the homepage. Three.js (r128) filament field, fog depth, scroll-driven camera dolly (smoothed), 8 scrollytelling beats, the in-curtain Sweep opening, progress bar, persistent CTA, WebGL-failure + reduced-motion fallbacks. **This is the homepage seed.**
- **`nycayen-the-sweep.html`** — the hair curtain that sweeps aside to reveal the masthead (canvas). Use if you want a distinct landing/intro moment.
- **`nycayen-scroll-experience.html`** (prior, GSAP + CSS-3D, no WebGL dependency) — the robust fallback approach if WebGL proves too heavy on target devices.

---

## 4. Design system / tokens (importable)

**Color** — dark, warm, glossy; champagne accent; iridescent for sheen only. Contrast verified against `--ink`.

| Token | Hex | Role | Contrast |
|---|---|---|---|
| `--ink` | `#0E0B0C` | Base stage | — |
| `--ink-2` | `#15100F` | Raised surfaces | — |
| `--oxblood` | `#3A1620` | Atmospheric glow | — |
| `--aubergine` | `#241326` | Atmospheric tone | — |
| `--ivory` | `#F3ECE2` | Primary text | ≈16:1 (AAA) |
| `--muted` | `#B9A99A` | Secondary text | ≈8:1 (AAA) |
| `--gold` | `#C8A45C` | Accent / primary CTA fill / credits | ≈8:1 |
| `--gold-soft` | `#E4CE97` | Hover / large accents | higher |
| Iridescent | `#E7B6C6 → #C8A45C → #9FD7C9 → #B9A6E0` | **Decorative only** (eyebrows, rules, transitions). Never body text. |

**Type** — `Fraunces` (display, variable: opsz/wght/SOFT/WONK/ital) + `Hanken Grotesk` (body/UI). Both free (SIL OFL).
Import: `https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght,SOFT,WONK@0,9..144,100..900,0..100,0..1;1,9..144,100..900,0..100,0..1&family=Hanken+Grotesk:wght@300..700&display=swap`
Scale (clamp): Hero `clamp(3rem,12vw,10rem)` / H2 `clamp(2.2rem,7vw,5.2rem)` / H3 `clamp(1.375rem,2.2vw,1.875rem)` / body `1.0625rem` (line-height 1.6) / eyebrow & caption `0.72–0.8125rem` small-caps, `letter-spacing 0.14–0.34em`, champagne.
Display set tight: line-height 0.86–0.96, letter-spacing −0.02em; italic uses SOFT 60. *(Premium option: paid foundry serif — Canela / GT Sectra — flag licensing. Not Playfair.)*

**Spacing** (8px base): 4/8/12/16/24/32/48/64/96/128. Section rhythm ≥96px desktop.
**Radius:** `--radius-sm:4px`, low-radius everywhere (couture, not pill); chat bubble may be softer.
**Motion:** easing `cubic-bezier(0.22,1,0.36,1)`; UI 250–600ms; orchestrated hero/scroll beats longer. **`prefers-reduced-motion` fully respected.**
**Texture:** film grain overlay (subtle, `mix-blend overlay`, ~6% opacity); cursor-tracked spotlight (warm radial). Both in the reference code.

---

## 5. Information architecture

- **Home** — the scroll experience (§6).
- **Categories** (Salon Services), each an editorial sub-page (§7): **Hair Replacement, Hair Styling, Men's Grooming, Bridal.**
- **The House** (About) · **Lookbook** (portfolio) · **Journal** (Blog) · **Store** · **Contact** · **Appointments.**
- **Nav language:** Atelier · Categories · Lookbook · The House · Appointments. Persistent **Request an appointment** at every breakpoint.
- **Page transitions:** cross-document **View Transitions** (Chromium + Safari 18.2+, progressive enhancement via `@supports`; pair with prefetch). A category card's image morphs into the sub-page hero via shared `view-transition-name`.

---

## 6. Homepage scroll storyboard

Mirrors the reference file. Each beat surfaces as the camera flies through the filament field. The opening = inside the curtain (the Sweep); scrolling parts it.

| # | Beat | Copy (verbatim) | Asset slot | Motion |
|---|---|---|---|---|
| 1 | Cover | eyebrow "The category is — transformation"; "Nycayen Moore, sculpted."; cue "Scroll — enter the House" | dense filament curtain (Lane A) | start inside curtain; scroll pulls camera forward, strands part |
| 2 | The House | eyebrow "The House"; "Great hair can change a life."; sub (see §9) | atmospheric haze (Lane D) | strands thin; ethos surfaces |
| 3 | Hair Replacement | "01 / Hair Replacement" + sub; emcee "The category is — realness" | sculptural vitrine object (Lane C) | object emerges from depth |
| 4 | Bridal | "02 / Bridal" + sub; emcee "the vow" | sculptural object (Lane C) | flies in, passes camera |
| 5 | Men's Grooming | "03 / Men's Grooming" + sub; emcee "executive" | material macro (Lane B) | depth reveal |
| 6 | Styling | "04 / Styling" + sub; emcee "runway ready" | material macro (Lane B) | depth reveal |
| 7 | Editorial & Runway | "05 / Editorial & Runway" + sub; emcee "legendary" | silhouette/light (Lane D/E) | depth reveal |
| 8 | Appointment | eyebrow "Appointments"; "Begin with a consultation."; CTA + NAP | spotlight resolves | CTA arrives from depth |

Scroll behavior: **scroll-linked camera dolly, smoothed** (the cinematic, reversible "scrolljack feel" — not a wheel-lock). Progress bar + persistent CTA visible throughout. Reduced-motion → static, stacked, fully readable (in the reference).

---

## 7. Interior page templates

- **Category sub-page** (×4): hero (category name + Lane C/B asset) → honest description (who it's for) → "what to expect" (consultation-led) → gallery (sculptural/material assets, Lane C/B) → category FAQ → **Request an appointment**. **No pricing anywhere.** Hair Replacement speaks to wigs/men's toupees/human-hair systems; Bridal captures event-date + travel.
- **The House** (About): Nyck Moore's story, philosophy, **credentials (verify before publishing — §12)**, the nationwide-travel differentiator. Real crawlable text (local-SEO weight).
- **Lookbook**: the sculptural / material / silhouette gallery, by volume/season — the **honest replacement for a portfolio of client looks.**
- **Journal** (Blog): editorial articles (hair/wig/toupee care, bridal prep, culture, technique). Feeds the concierge + SEO.
- **Contact**: NAP, embedded map, call/text/email actions, hours, lightweight message form (in addition to booking).
- **Store**: accessories / hair-care / wig products. Backend TBD (§12); tasteful "Shop — coming soon" if deferred, never broken.
- **Appointments**: the booking flow (§8).

---

## 8. Component specs

- **Nav + persistent CTA** — sticky, transparent over hero → solid on scroll; mobile drawer; "Request an appointment" pinned everywhere.
- **Scroll progress + escape hatch** — progress bar; the always-available CTA is the escape from the experience.
- **Booking / consultation (price-blind)** — primary action everywhere is **Request an appointment**, never a priced service. Types: Discovery Call · In-Studio · Bridal/Event · On-Location (travel). Flow: choose type → live availability → **qualifying intake** (location NYC vs travel + city, category, hair type/texture, event date, optional photo, notes) → confirm → email/SMS reminders. Optional deposit. Backend: **Cal.com** (themable; recommended) or **Square Appointments** (if already in use). Keep a call/text/email fallback always visible. Build the UI to embed a headless scheduler — don't hardcode a vendor into layout.
- **Instagram feed (@nycayenmoore reels)** — native dark-themed cards. **2026 reality:** Basic Display API was retired Dec 4 2024; the account must be a **Professional (Creator/Business)** account, accessed via the Instagram API with Instagram Login / Graph API, with **token refresh on schedule, server-side caching (~200 req/hr), App Review for public production.** Path A: maintained widget (least maintenance, monthly fee). Path B: custom server-side Graph API + native cards (full control; you own refresh/caching/review). **Do not render raw IG comments inline** (moderation liability). Poster first, play in view, lazy-load.
- **AI concierge** — persistent, dismissible, dark-themed "House concierge." Scope: services, wigs/toupees/extensions, hair & wig care, accessories, how-it-works, travel/availability, routing to booking. **Guardrails (system prompt):** never quotes prices (explains the consultation model, offers to book); stays in scope; hands off to booking or call/text when unsure; grounded on real site content. Accessible (focus trap, ARIA, Esc), lazy-loaded, never blocks render.
- **Footer** — NAP, hours, Google Map link, social row with **clean URLs** (strip the current `?locale`/`?lang` junk), copyright.

---

## 9. Copy deck (real content — no lorem)

**Brand lines:** "Elevate Your Look, Elevate Your Life." · "Radiate Confidence." · "Hair as couture."
**Positioning line:** New York atelier — available across the U.S. · Established 2000.
**Hero (cover):** eyebrow "The category is — transformation"; masthead "Nycayen Moore, sculpted."; sub "Hair as couture — for the runway, the ball, the aisle, and the everyday."
**The House:** "Great hair can change a life." / "An atelier & a house — New York, and wherever you are. Twenty-five years of building looks for the person wearing them."

**Categories** (name / sub / emcee caption):
- **Hair Replacement** — "Wigs, men's systems, and human-hair work — matched to you, built to disappear." / "the category is — realness"
- **Bridal** — "The aisle look, rehearsed and resolved. On location, on the day." / "the vow"
- **Men's Grooming** — "Precision cuts and grooming for the modern face." / "executive"
- **Styling** — "Cut, color, and finish — for any room you walk into." / "runway ready"
- **Editorial & Runway** — "Sets, shows, and shoots. Hair direction for the camera and the floor." / "legendary"

**Fashion-credit caption format** (under any image): `Hair — Nycayen Moore / [Look NN] / [Volume SS·26]`.
**Appointment:** "Begin with a consultation." / "Every look is bespoke — to your hair, your goals, your occasion. Tell the House what you're after." / CTA "Request an appointment."
**Price-blind framing (a "How the House works" beat):** every service is built around the person, their goals, and the occasion, so a look is quoted after a consultation — not from a menu.

**FAQ (from the brand):** specializes in men's grooming, bridal, and personalized styling for any occasion; experienced with all hair types and textures; books anywhere in the United States; hair services only (no makeup).

**NAP / hours / socials:** 265 W 37th St, 4th flr, New York, NY 10018 · (347) 556-3860 (call/text) · info@nycayenmoorenyc.com · Mon–Fri 10AM–6PM, Sat & Sun closed · Instagram @nycayenmoore, YouTube @Nycayen, Facebook, X, LinkedIn, Yelp, Pinterest.

---

## 10. Imagery model → asset slots

Hair as medium, never a result (§1). Five lanes, each filling specific slots (generation prompts in the prompt pack):
- **Lane A — Filaments / strand fields** → the homepage WebGL field, hero textures.
- **Lane B — Material macro** (gloss of a lock, weave, braid architecture) → category openers, texture sections.
- **Lane C — Sculptural vitrine objects** (coiled braid, abstract updo, wig-block bust on a plinth) → category heroes, the Lookbook.
- **Lane D — Light & haze / atmosphere** → backgrounds, transitions, the stage.
- **Lane E — Silhouette / shadow** → graphic punctuation.

**Formats:** generate/upscale to 4000px+; export **AVIF + WebP**; transparent **cutouts** for composited objects; **depth maps** for the hero parallax; **image sequences** (not video) for any scrubbed cinematic motion; reels come from Instagram (lazy). Use a **commercial-safe** generator for client-facing assets and verify licensing (see prompt pack §0).

---

## 11. Engineering guardrails

- **Performance:** 60fps on all motion; **LCP < 2.5s, CLS < 0.1, INP < 200ms.** Lazy-init WebGL after first paint, efficient geometry/textures, lazy-load below-fold, defer the IG feed + concierge, self-host fonts (swap + preconnect), prefetch for transitions.
- **Accessibility — WCAG 2.1 AA:** keyboard operable, visible focus (gold ring), full reduced-motion fallback (in the reference), alt text, video captions, labeled forms, ≥44px targets, color never the sole signifier, concierge accessible.
- **SEO / local:** server-rendered crawlable content (the WebGL is a layer, not the content); titles/meta/OG; **`LocalBusiness`/`HairSalon` JSON-LD** with consistent NAP; reconcile the address discrepancy (site 265 W 37th vs Yelp 224 W 35th) + Google Business Profile; sitemap.
- **Production stack target:** Next.js (App Router) · GSAP + ScrollTrigger · Three.js (WebGPU where it helps) · Lenis (smooth scroll) · View Transitions · Framer Motion · `next/image` · deploy Vercel. (Claude Design → handoff to Claude Code → Vercel.)

---

## 12. Decisions to lock before/while building

1. **NAP** — confirm canonical address (265 W 37th vs Yelp's 224 W 35th); fix everywhere + Google Business Profile.
2. **Scheduler** — Cal.com vs Square Appointments; consultation types/durations; deposit yes/no.
3. **Instagram** — convert @nycayenmoore to a Professional (Creator) account **now**; widget vs custom; comments excluded.
4. **Typography** — free pairing (Fraunces + Hanken Grotesk) vs paid foundry display (licensing cost).
5. **Store** — keep commerce? backend (Shopify headless / Snipcart / Square)? what's sold?
6. **Credentials** — which fashion/ballroom/editorial claims (Fashion Week, red-carpet, etc.) are verified and approved to publish.
7. **Visual assets** — generate via the prompt pack; pick the commercial-safe tool for client-facing pieces.
8. **Logo** — obtain a vector/SVG wordmark fit for a masthead.
