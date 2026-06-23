# House of Nycayen — Asset Production & Claude Design Handoff

The definitive plan to get every asset into optimal form and build it in Claude Design at the Apple/Vogue bar. This is the handoff index — it points to every other file in the package.

## The package (single source of truth)

| File | Role |
|---|---|
| `House-of-Nycayen-Claude-Design-Build-Package.md` | Master build spec (IA, scenes, components, copy) |
| `House-of-Nycayen-Final-Stack-Locked.md` | Tech stack — Square headless backbone (+ Sanity, **Klaviyo**, GA4, Vercel) |
| `House-of-Nycayen-AI-Asset-Prompt-Pack.md` | Prompts to generate all imagery |
| `house-of-nycayen-design-tokens.css` | **Importable** design system |
| `house-of-nycayen-style-tile.html` | **The visual reference** Claude Design anchors on |
| `nycayen-webgl-scroll.html` / `nycayen-the-sweep.html` | Seed code for the homepage |

## The one principle that decides quality

**Consistency beats volume.** A coherent set of 12 perfectly-matched assets builds an institutional site; 40 mismatched ones build a beautiful mess no matter how good Claude Design is. Every step below serves consistency, and the rule is to hand Claude Design a *reference*, never an open prompt.

---

## 1. The complete asset bundle (and optimal form)

| Asset | Optimal form | Who produces |
|---|---|---|
| Design tokens | `design-tokens.css` (variables + classes) | ✅ Provided |
| Visual style tile | `style-tile.html` (rendered brand board) | ✅ Provided |
| Seed code | clean HTML (`webgl-scroll`, `the-sweep`) | ✅ Provided |
| Copy deck | structured by page/section (Build Package §9) | ✅ Provided |
| **Hair-material imagery** | AVIF + WebP, 4000px+, per spec below | **You** (AI tools + Prompt Pack) |
| **Sculptural objects** | transparent-PNG **cutouts** | **You** |
| **Hero depth map** | grayscale PNG paired to the hero still | **You** |
| **Scrubbed sequence** (optional) | numbered frames (image→video→frames) | **You** |
| Logo / wordmark | **vector SVG** (commission or vectorize) | **You** — outstanding |
| Fonts | already wired via Google Fonts in tokens | ✅ |
| Instagram reels | live via Graph API/widget (not files) | Build stage |

---

## 2. AI imagery — the production discipline (the part that makes or breaks it)

Run this as a focused sprint, not ad-hoc generation.

1. **Lock one visual reference first.** Generate variations of a single hero plate until one is perfect, then capture its style anchor — Midjourney **`--sref` / Omni Reference**, or a Flux reference image, or a fixed seed. **Every other asset is generated from that same anchor.** This is what makes the set read as one house.
2. **Generate per lane, from the anchor.** Work the Prompt Pack's lanes (A filaments, B macro, C sculptural objects, D light/haze, E silhouette) with the locked anchor + the shared style suffix + negative prompt on every generation.
3. **Cull ruthlessly.** Generate 5–10 per slot; keep the **one** that's flawless and on-anchor. Discipline here is the entire difference. If it's 90%, it's a no.
4. **Prep to spec** (§3) — resolution, format, cutouts, depth maps.
5. **Commercial-safe for anything client-facing:** generate those in **Adobe Firefly** (indemnity). Verify each tool's terms; purely AI images aren't copyrightable, so rights come from the ToS, not ownership.

**No people, no faces, no fabricated results — ever.** Hair is medium, sculpture, material, light.

---

## 3. Output specs

- **Resolution:** 4000px+ masters; downscale responsively.
- **Format:** **AVIF** primary + **WebP** fallback; **PNG** for cutouts.
- **Cutouts:** transparent PNG for every sculptural object that composites onto the dark stage.
- **Depth map:** grayscale PNG paired to the hero still (drives the WebGL/parallax).
- **Sequence:** `seq_[name]_0001.webp …` at display resolution.
- **Color:** grade everything to the palette — warm espresso shadows, champagne light, optional iridescent edge. Reject anything cool/clinical.

---

## 4. Slot → asset manifest (naming)

Convention: `lane[A–F]_[slot]_[NN].[ext]`. Build these and Claude Design drops them straight in.

| Slot | Lane | File | Spec |
|---|---|---|---|
| Homepage field / curtain | A | `laneA_field_01.avif` | 21:9, depth-rich |
| Hero depth map | A | `laneA_field_01_depth.png` | grayscale, matched |
| Hair Replacement hero | C | `laneC_replacement_01.png` | cutout, 4:5 |
| Bridal hero | C | `laneC_bridal_01.png` | cutout, 4:5 |
| Men's Grooming opener | B | `laneB_grooming_01.avif` | 3:2 macro |
| Styling opener | B | `laneB_styling_01.avif` | 3:2 macro |
| Editorial & Runway | D/E | `laneD_runway_01.avif` | 16:9 |
| Lookbook set | C/B/E | `laneC_lookbook_01..NN` | mixed, cutouts where composited |
| Section atmospheres | D | `laneD_haze_01..NN` | 16:9, full-bleed |
| Silhouette punctuation | E | `laneE_silhouette_01..NN` | 2:3 |

---

## 5. Feeding Claude Design (the exact sequence)

Build in stages, focused sessions (it's token-heavy).

**Session 0 — Set the system.** Upload `design-tokens.css` **and** `style-tile.html`. Prompt: *"Use this design system and style tile as the brand for everything you build — exact colors, type, spacing, motion, and the editorial dark aesthetic shown."*

**Session 1 — Homepage.** Upload `nycayen-webgl-scroll.html` + the Lane A field asset + depth map + the homepage copy. Prompt: *"Build the homepage to match and extend this reference exactly — the Three.js hair-filament field you fly through on scroll, scroll-linked camera, scrollytelling beats, the in-curtain opening. Apply the uploaded design system. Use the attached imagery and copy. No photos of people. Keep the progress bar, persistent 'Request an appointment' CTA, and reduced-motion fallback."*

**Session 2 — Interior pages.** Per page (category template, The House, Lookbook, Journal, Contact, Appointments), upload that page's copy + assets. Prompt for cross-document View Transitions between them.

**Session 3 — Components.** Booking UI (themed, against Square Bookings API), the store (themed catalog/cart/checkout against Square Catalog/Orders + Web Payments SDK), Instagram feed, the AI concierge, footer. (Wiring happens at the Claude Code stage — build the UI here.)

**Throughout:** use direct **canvas edits / sliders** for small tweaks instead of full prompts; paste comments into chat (known quirk); branch with *"save this and try a different direction for X."*

**Handoff:** export standalone HTML or use Claude Design's handoff to **Claude Code**, then deploy on **Vercel**. The Square API wiring, the Klaviyo capture, GA4, and the Instagram Graph integration are finished at that stage.

---

## 6. Honest division of labor

- **Provided (ready now):** design tokens, style tile, seed code, copy, the full spec + stack, the prompt pack, this plan.
- **Yours:** generate the imagery (Prompt Pack + the §2 discipline), get a vector SVG wordmark, and run the Claude Design sessions in §5.
- **Claude Code / production stage:** Square headless wiring, Klaviyo, GA4, Instagram Graph, performance/60fps + mobile pass, deploy.

## 7. Still open

Email = **Klaviyo** (locked). Outstanding: vector logo, NAP reconciliation (265 W 37th vs Yelp 224 W 35th), font licensing (free pairing is fine), credentials verification.
