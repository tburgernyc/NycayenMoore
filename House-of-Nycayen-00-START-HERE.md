# House of Nycayen — START HERE

The single front door to the build package. Read this, then work top to bottom.

## The project in one breath

A luxury **hair-artist** site — services-primary; hair care + accessories secondary. A dark editorial / ballroom-fashion concept: a living fashion publication crossed with a runway, with hair treated as **medium, sculpture, material, and light — never a fabricated client result.** A scroll-driven WebGL experience. The one job: **book consultations.** The bar: work Apple's design team and Vogue's editors would ship and claim as their own.

## Build sequence (the runway)

1. **Generate the assets** — `AI-Asset-Prompt-Pack` + the lock-one-anchor / cull-ruthlessly discipline in `Asset-Production-and-Handoff` §2.
2. **Build the UI in Claude Design** — `Asset-Production-and-Handoff` §5 is the session-by-session playbook. Seed with the tokens, the style tile, and the WebGL reference.
3. **Wire the backend in Claude Code** — `Square-Integration-Spec` (booking + store + payments), then the lighter Klaviyo / GA4 / Instagram Graph hookups.
4. **Deploy on Vercel** — then the 60fps + mobile pass.

## The files (open in this order)

| Order | File | What it is | When |
|---|---|---|---|
| 0 | `House-of-Nycayen-00-START-HERE.md` | This index | First |
| 1 | `House-of-Nycayen-Claude-Design-Build-Package.md` | Master spec — concept, IA, scenes, components, full copy | Read first |
| 2 | `House-of-Nycayen-Components-and-Decisions.md` | Component coverage + integration decisions | Reference |
| 3 | `House-of-Nycayen-Final-Stack-Locked.md` | Locked tech stack (Square + Sanity + Klaviyo + GA4 + Vercel) | Reference |
| 4 | `house-of-nycayen-design-tokens.css` | Importable design system | Claude Design — Session 0 |
| 5 | `house-of-nycayen-style-tile.html` | The visual reference Claude Design anchors on | Claude Design — Session 0 |
| 6 | `nycayen-webgl-scroll.html` | Homepage seed code | Claude Design — Session 1 |
| 7 | `nycayen-the-sweep.html` | Hero seed (optional landing) | Claude Design — optional |
| 8 | `House-of-Nycayen-AI-Asset-Prompt-Pack.md` | Generation prompts for every visual | Asset sprint |
| 9 | `House-of-Nycayen-Asset-Production-and-Handoff.md` | Asset method + the Claude Design session playbook | Throughout |
| 10 | `House-of-Nycayen-Square-Integration-Spec.md` | Square backend wiring | Claude Code stage |

**Background (rationale, optional):** `House-of-Nycayen-Creative-Direction-v2.md`, `House-of-Nycayen-Scroll-Architecture.md`.
**Superseded — ignore:** `Nycayen-Moore-Website-Build-Brief.md` (the early conservative version; replaced by everything above).

## What kills this build (the non-negotiables)

- **Inconsistent assets.** Lock one style anchor, generate the whole set from it, cull ruthlessly. Mismatched imagery reads as amateur in half a second.
- **Faking his work.** No people, no faces, no fabricated results — ever. Hair as medium only.
- **Scroll-jacking done wrong.** Scroll-*driven*, never wheel-locked; `prefers-reduced-motion` gets a full static fallback.
- **Placeholder copy.** Use the real copy in the build package, never lorem — placeholder content drags down every other quality signal.
- **The two Square rules.** The server prices every order from the catalog (never trust the client's amounts); the access token + webhook key stay server-only, and webhooks are signature-verified against the raw body.
- **Burying the content.** WebGL is a layer, not the content — real text is server-rendered and crawlable, with `LocalBusiness` schema and consistent NAP, or a local business won't rank.

## Still to lock

- **NAP:** reconcile 265 W 37th St vs Yelp's 224 W 35th St; align the Google Business Profile.
- **Logo:** obtain a vector SVG wordmark.
- **Fonts:** free pairing (Fraunces + Hanken Grotesk) is fine; confirm if going paid.
- **Credentials:** verify any Fashion Week / red-carpet / press claims before publishing.
- **Email:** Klaviyo (locked) — wire the capture form at launch.
- **Instagram:** convert @nycayenmoore to a Professional account before the feed can pull.

## Quick reference

- **Stack:** Next.js (App Router) · Three.js · GSAP/ScrollTrigger · Lenis · Framer Motion · Square (headless) · Sanity · Klaviyo · GA4 · Vercel.
- **Brand:** ink `#0E0B0C` · ivory `#F3ECE2` · gold `#C8A45C` · iridescent accent (decorative only); Fraunces (display) + Hanken Grotesk (body).
- **NAP:** 265 W 37th St, 4th flr, New York, NY 10018 · (347) 556-3860 (call/text) · info@nycayenmoorenyc.com · IG @nycayenmoore · YouTube @Nycayen.
- **Imagery rule:** hair as medium/sculpture/material/light — never a person or a result.
