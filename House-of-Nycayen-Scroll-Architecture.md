# House of Nycayen — Scroll-Experience Architecture

**Pairs with:** `House-of-Nycayen-Creative-Direction-v2.md` (art direction, palette, type, IA). This document covers the **cinematic scroll engine** — the Z-axis 3D parallax / scroll-driven / scrollytelling experience — and how to ship it without it costing bookings or ranking.
**Working reference:** `nycayen-scroll-experience.html` — open it and scroll. It implements the real technique (GSAP ScrollTrigger pin + scrub, Z-axis depth) at a first-draft level. Build from it.

---

## 1. The five concepts, mapped to the build

| Concept (your terms) | How it's implemented here |
|---|---|
| **3D / Z-axis parallax** | CSS `perspective` + `transform: translateZ()` on layered elements; the "camera" pushes forward, content emerges from and passes through depth. Heavier scenes graduate to a real **Three.js** scene. |
| **Scroll-linked / scrubbed animation** | **GSAP ScrollTrigger** with `scrub` — the scroll position is mapped to an animation timeline. You scrub a scene like a film, forward and back. |
| **Scrollytelling** | Each section is a pinned "scene" in a sequential narrative: cover → the House → the runway (categories) → in motion → the appointment. |
| **Scrolljacking** | Achieved via `pin: true` — the scene holds while you scrub through it. (See §4 — the *right* version of this vs. the version users hate.) |
| **WebGL / Three.js** | The hero and any true 3D depth/particle/shader work render in WebGL via Three.js (WebGPU where it buys 60fps headroom), layered over real DOM content. |

---

## 2. Engine & stack

- **GSAP + ScrollTrigger** — the scene timelines, pinning, scrubbing. The industry-standard tool behind Apple-style scroll pages.
- **Lenis** (smooth-scroll) — pair with ScrollTrigger for the buttery, weighted, "Apple" scroll feel. This single choice does most of the work in making scroll-driven motion feel premium rather than twitchy.
- **Three.js** (WebGL; WebGPU renderer where supported) — the hero shader scene and any genuine 3D depth/particles. Layered behind crawlable DOM.
- **Next.js (App Router)** — SSR/SSG so the content exists in the HTML (see §5). Deploy on Vercel.
- **Framer Motion** — component-level micro-motion (hover, UI).
- Variable fonts (`Fraunces`) via `next/font`; assets via `next/image`; lazy-init the WebGL after first paint.

---

## 3. Scene storyboard (the scrollytelling sequence)

Each scene pins and scrubs. The narrative is a runway show.

1. **Cover.** Masthead "Nycayen Moore," spotlight, grain. On scroll, the camera **flies forward through the wordmark** (scale + translateZ + fade/blur) while "01 — Transformation" emerges from deep Z behind it. Establishes the Z-axis language immediately. *(Built in the reference.)*
2. **The House.** Editorial interlude — ethos and credentials reveal as you scroll; a plate floats in from depth. Slower, lets the viewer breathe between cinematic beats.
3. **The Runway (signature scene).** You travel down a catwalk: each **category flies in from far Z** (small, faint), lands sharp and centered, then **passes the camera** (scales up, fades) as the next arrives — 01 Hair Replacement → 05 Editorial & Runway, each with an emcee-style caption ("the category is — realness"). This is the moment that earns the awards. *(Built in the reference.)*
4. **In Motion.** The @nycayenmoore reels grid assembles on scroll (staggered Z-float reveals). Live social proof.
5. **Arrival / Appointment.** The CTA **arrives from depth** and resolves under the spotlight: "Begin with a consultation." Primary action → the scheduler. *(Built in the reference.)*

Production scenes add: real art-directed photography/video (non-negotiable — see v2 §2.3), the Three.js shader hero, and richer depth in the runway (parallaxed background "audience"/light haze layers moving at their own Z-rate).

---

## 4. Doing scrolljacking *right* — the line between Apple and a support ticket

This is where the value is. The technique isn't the risk; sloppy execution is. Every item below is in the reference build or specified for production.

- **Pin + scrub, not wheel-hijack.** Apple's pages don't actually override your mouse wheel; they map scroll *position* to a timeline and pin the scene. The page still scrolls natively — you control pace and can reverse — but it *feels* like scrubbing a film. This is the version in the reference. (A fully locked wheel-override is a one-line posture change in the scroll layer if you insist — but even Apple doesn't do it, and it's the exact thing that generates the frustration you flagged. Pin + scrub gets the cinematic result without it.)
- **Always show progress.** A persistent scroll-progress bar (in the reference) tells the user where they are in the show, so a long pinned scene never feels like a freeze.
- **Always offer the exit.** A persistent "Request an appointment" in the top bar (in the reference) lets high-intent users skip straight to converting. Never trap anyone in the experience.
- **Pace it fast.** The #1 complaint about scroll-jacking is sluggishness — too much scroll distance per beat. Keep `end` distances tight (the reference uses ~140% for the cover, ~82% per runway category). If it feels slow, shorten the scroll length before anything else.
- **60fps or it reads as broken.** Only animate `transform` and `opacity` (GPU-composited). Lazy-init WebGL after LCP, use level-of-detail and efficient textures, and profile on a mid-range phone. Jank — not the concept — is what makes these sites feel cheap.
- **Mobile gets a lighter cut.** Pinned 3D scrubbing is heavy on phones and fights native momentum scroll. Production should ship a reduced mobile version: fewer/shorter pinned scenes, simpler depth, or graceful conversion to standard scroll reveals. Design the mobile experience deliberately, don't just shrink the desktop one.
- **Reduced motion is a hard gate.** Users with `prefers-reduced-motion` get a clean, static, fully readable editorial scroll — no pin, no scrub, no WebGL. The reference does this: with reduced motion (or no JS), every scene renders as normal stacked, legible content. This is both an accessibility requirement and your safety net.

---

## 5. Keeping it from costing bookings or ranking

The cinematic layer sits *on top of* a sound foundation — you get the show and the business results.

- **Crawlable content underneath.** Next.js renders the real copy, headings, category text, and credits into the HTML. The WebGL/3D is an enhancement layer, not the content. Search engines and screen readers read the document; the camera-fly is decorative. (This is the technique LV and Vogue use to be both experiential and indexed.)
- **`LocalBusiness` / `HairSalon` JSON-LD**, consistent NAP (reconcile 265 W 37th vs the stale Yelp 224 W 35th listing), Google Business Profile alignment, sitemap. An artist still has to be findable in NYC.
- **Core Web Vitals respected despite the experience:** LCP < 2.5s (lazy WebGL, prioritized hero poster), CLS < 0.1, INP < 200ms. Budget for it.
- **The booking path is never more than one tap away** (persistent CTA), so the experience drives consultations instead of burying them.

---

## 6. Build-tier reality

- **Claude Design can build:** the full scene layout and content, the GSAP ScrollTrigger pin/scrub scenes, the CSS Z-axis parallax, the reveals, the kinetic type, the progress/escape-hatch chrome, the reduced-motion fallback, responsive structure — i.e. essentially the reference build, productionized. That's most of the experience.
- **A creative front-end developer is needed for:** the bespoke **Three.js/WebGL shader hero**, true 3D particle/depth scenes, Lenis tuning, and the performance pass to hold 60fps across devices. Scope and budget this layer explicitly.

Hand Claude Design the reference HTML plus the v2 art-direction brief; flag the Three.js hero as the creative-dev piece.

---

## 7. Decisions to lock (scroll-specific; see v2 for the rest)

1. **Desktop scroll length / pacing** — confirm the show isn't too long; tune `end` distances.
2. **Mobile treatment** — full cinematic (heavy) vs. a deliberate lighter cut. Recommend the lighter cut.
3. **Hard wheel-lock vs. pin+scrub** — recommend pin+scrub (the reference default).
4. **WebGL hero scope** — shader complexity vs. budget; confirm the creative-dev engagement.
5. **Photography/video** — the art-directed shoot that the whole experience hangs on.
