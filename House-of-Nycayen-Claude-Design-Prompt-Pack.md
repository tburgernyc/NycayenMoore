# House of Nycayen — Claude Design Prompt Pack

Paste-ready prompts for building the site in Claude Design, in order. These are **Claude Design** prompts (instructions to the UI builder) — not the image-generation prompts (those feed Midjourney/Flux and live in `AI-Asset-Prompt-Pack.md`).

## How to use this (the discipline that decides quality)

- **Attach a reference, don't ask it to invent.** Every session starts from the design system + a reference file. Open-ended "make it luxury" prompts produce template slop.
- **One screen or component per session.** Claude Design is token-heavy and gets muddy when overloaded. Build, refine, move on.
- **Be specific about copy, layout, and behavior** — all provided below; never let it fill with lorem.
- **Refine on the canvas.** After a screen exists, use direct edits / sliders for spacing, size, and color tweaks instead of burning a full prompt. Paste comments into chat (known quirk).
- **Generate the imagery first.** Don't start a screen whose asset doesn't exist yet.

**Attach with every relevant session:** `house-of-nycayen-design-tokens.css`, `house-of-nycayen-style-tile.html`, the screen's reference file, and the screen's generated imagery.

---

## Session 0 — Design system + house rules (do this first)

*Attach: `house-of-nycayen-design-tokens.css` + `house-of-nycayen-style-tile.html`.*

```
This is a new project: the website for "House of Nycayen," a luxury hair-artist brand in NYC rooted in ballroom and fashion. I'm attaching the design system (design-tokens.css) and a rendered style tile.

Use these as the binding brand for everything you build — exact colors, the Fraunces (display) + Hanken Grotesk (body) type system, spacing, low radius, the gold and iridescent treatment, film-grain + spotlight mood, and the dark editorial aesthetic shown in the style tile.

Lock these HOUSE RULES for every screen we build from now on:
1. Imagery is hair as medium, sculpture, material, and light — never a person, a face, or a fabricated client result. Treat any image slot accordingly.
2. Motion is scroll-DRIVEN, never scroll-jacked; the user always controls pace. Honor prefers-reduced-motion with a full static, readable fallback.
3. WCAG 2.1 AA: keyboard operable, visible gold focus rings, ≥44px targets, color never the sole signifier.
4. A persistent "Request an appointment" CTA appears at every breakpoint.
5. Restraint: one bold move per screen; the iridescent sheen is decorative only, never body text.
6. Use the real copy I provide; never placeholder/lorem text.

Confirm you've absorbed the system and rules. Don't build a page yet.
```

---

## Session 1 — Homepage (the scroll experience)

*Attach: `nycayen-webgl-scroll.html` + your `laneA_field_01` image + `laneA_field_01_depth` depth map.*

```
Build the homepage by matching and extending the attached reference (nycayen-webgl-scroll.html) exactly:
- A Three.js field of hair filaments the camera flies through on scroll (Z-axis parallax), scroll-linked/scrubbed and smoothed, opening INSIDE the filament curtain so the first scroll parts it.
- Scrollytelling beats in this order, each surfacing as the camera travels: Cover → The House → the 5 categories → Appointment.
- Keep the scroll-progress bar, the persistent "Request an appointment" CTA, and the reduced-motion static fallback.
Use the attached field image + depth map for the WebGL look. Apply the project design system. No people anywhere.

COPY (verbatim):
- Cover: eyebrow "The category is — transformation"; masthead "Nycayen Moore, sculpted."; cue "Scroll — enter the House".
- The House: eyebrow "The House"; "Great hair can change a life."; "An atelier & a house — New York, and wherever you are. Twenty-five years of building looks for the person wearing them."
- Categories (name / line / emcee caption): 01 Hair Replacement — "Wigs, men's systems, and human-hair work — matched to you, built to disappear." / "the category is — realness". 02 Bridal — "The aisle look, rehearsed and resolved. On location, on the day." / "the vow". 03 Men's Grooming — "Precision cuts and grooming for the modern face." / "executive". 04 Styling — "Cut, color, and finish — for any room you walk into." / "runway ready". 05 Editorial & Runway — "Sets, shows, and shoots. Hair direction for the camera and the floor." / "legendary".
- Appointment: eyebrow "Appointments"; "Begin with a consultation."; "Every look is bespoke — to your hair, your goals, your occasion. Tell the House what you're after."; CTA "Request an appointment"; "265 W 37th St · New York · (347) 556-3860".
```

---

## Session 2 — Category page template (build Hair Replacement, then replicate)

*Attach: your `laneC_replacement_01` cutout image.*

```
Build a category page template, per the design system and house rules, starting with HAIR REPLACEMENT. Sections: hero (category name + the attached sculptural image, fashion-credit caption "Hair — Nycayen Moore / Look 01 · Volume SS·26"), an honest description of the service and who it's for, a "What to expect" block framed around a consultation (not a price — there is NO pricing anywhere), a small gallery, a short FAQ, and a "Request an appointment" CTA. Add a cross-document View Transition so a homepage category card morphs into this hero.

COPY: headline "Hair Replacement"; "Wigs, men's systems, and human-hair work — matched to you, built to disappear."; expand into 2–3 short paragraphs on bespoke matching for all hair types and textures, discretion, and the consultation-led process. No prices.

Then replicate this exact template for Bridal, Men's Grooming, Styling, and Editorial & Runway — I'll supply each one's image and copy.
```

---

## Session 3 — The House (About)

*Attach: a `laneD` haze/atmosphere image.*

```
Build "The House" (about) page per the system and house rules. Editorial single-column-with-margin layout: the stylist's story and philosophy, the ballroom + fashion pedigree (leave a clearly-marked placeholder for credentials I'll verify), and the nationwide-travel differentiator. Use real, crawlable body text (this page carries SEO weight). Atmospheric image only — no people. End with a "Request an appointment" CTA.

COPY seed: "Great hair can change a life." / "An atelier & a house — New York, and wherever you are." Expand into a confident, plain-spoken artist's bio. Mark credentials as [VERIFY BEFORE PUBLISH].
```

---

## Session 4 — Lookbook (Gallery)

*Attach: your lookbook set (sculptural objects, material macros, silhouettes — cutouts where they composite).*

```
Build the Lookbook (gallery) per the system and house rules — the brand's portfolio, shown as sculptural and material hair imagery, NOT photos of clients. An editorial grid organized by volume/season, with fashion-credit captions ("Hair — Nycayen Moore / Look NN · Volume SS·26"), tasteful hover reveals, and a lightbox. No people, no faces. Lazy-load images.
```

---

## Session 5 — Journal (Blog)

```
Build the Journal (blog): an editorial index (cards with title, category, date) and an article template with a clean Fraunces/Hanken reading layout, pull quotes, and image support. Topics are hair/wig/toupee care, bridal prep, culture, and technique. Use 2–3 realistic sample posts (real-sounding titles and intros, not lorem). This will be powered by a CMS later — structure it cleanly.
```

---

## Session 6 — Contact

```
Build the Contact page per the system: NAP, an embedded map placeholder, click-to-call/text and email, hours, and a lightweight message form (name, email, phone, message) with clear success/empty/error states in the brand voice. NAP: 265 W 37th St, 4th flr, New York, NY 10018 · (347) 556-3860 (call/text) · info@nycayenmoorenyc.com · Mon–Fri 10AM–6PM, Sat & Sun closed.
```

---

## Session 7 — Appointments (themed booking UI)

```
Build the Appointments page as a themed, multi-step booking UI (front-end only — it will wire to the Square Bookings API later, so use realistic placeholder data and clearly named handlers). Steps: 1) choose consultation type (Discovery Call · In-Studio · Bridal/Event · On-Location/travel), 2) pick a date/time from a calendar, 3) a qualifying intake form (location NYC vs travel + city, category of interest, hair type/texture, event date, optional photo upload, notes), 4) confirmation. The primary action is "Request an appointment" — NEVER a priced service. Keep a call/text/email fallback visible. Do not use any third-party booking widget; this is a custom themed flow.
```

---

## Session 8 — Store (themed e-commerce UI)

```
Build the Store UI per the system (front-end only — wires to Square Catalog/Orders + Web Payments SDK later; use placeholder products and clearly named handlers). Include: a product listing grid (PLP), a product detail page (PDP) with variants and add-to-cart, a cart drawer, and a themed checkout layout with a card-entry container placeholder (where the Square Web Payments SDK will mount). Since products aren't confirmed yet, also build a tasteful "Atelier — launching soon" empty state for the catalog. Products will be hair care + accessories. Keep it minimal and on-brand.
```

---

## Session 9 — Global components

```
Build the global components per the system and house rules:
1. Header/nav — sticky, transparent over the hero then solid on scroll; mobile drawer; nav labels "Atelier · Categories · Lookbook · The House · Appointments"; persistent gold "Request an appointment" button at all breakpoints.
2. Footer — NAP, hours, a Google Map link, and a social row (Instagram @nycayenmoore, YouTube @Nycayen, Facebook, X, LinkedIn, Yelp, Pinterest) with clean URLs.
3. Instagram feed — a native dark-themed grid of recent reel cards (poster + play affordance + caption), header "Recent work", "Live from @nycayenmoore"; use placeholder reels (wires to the Instagram Graph API later). Do NOT render user comments.
4. AI concierge — a dismissible, dark-themed chat panel ("the House concierge"), launcher bottom-right, accessible (focus trap, Esc to close), lazy-loaded; scope is services, wig/toupee/hair care, accessories, and routing to booking; it never quotes prices.
```

---

## Refinement prompts (reusable)

```
Push this more editorial: bigger type contrast, more negative space, tighter letter-spacing on the display, calmer secondary text. It should feel like a fashion magazine spread, not a web template.
```
```
The vertical rhythm is uneven — standardize section spacing to the design system scale and align everything to a consistent baseline.
```
```
Strengthen the motion: gentle fade-and-rise reveals on scroll (once, not repeating), magnetic hover on the CTA, and confirm prefers-reduced-motion disables all of it.
```
```
Save this version and try a different direction for [section] — I want to compare.
```

---

## QA pass (run before export)

```
Audit the whole site against these and fix what fails: (1) WCAG 2.1 AA — contrast, keyboard nav, visible focus, ≥44px targets, alt text, labeled forms; (2) prefers-reduced-motion disables all scroll/WebGL motion and shows a readable static layout; (3) fully responsive incl. a lighter mobile treatment of the scroll experience; (4) zero placeholder/lorem text remains; (5) the persistent "Request an appointment" CTA is present on every page; (6) no image depicts a person or a fabricated result. List anything you couldn't fix for the Claude Code stage.
```

---

## Handoff

When the UI is right, export standalone HTML or use Claude Design's handoff to **Claude Code**, then build the Square wiring (see `Square-Integration-Spec.md`), Klaviyo, GA4, and the Instagram Graph integration, and deploy on **Vercel**.
