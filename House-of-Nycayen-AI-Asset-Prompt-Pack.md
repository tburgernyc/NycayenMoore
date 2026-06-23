# House of Nycayen — AI Visual Asset Prompt Pack

Generate every visual here. The rule from the brief holds in every prompt: **hair as medium, sculpture, material, light — never a person, never a face, never a fabricated client result.** That's what keeps this honest *and* on-concept.

---

## 0. Principles & rights

- **No people, no faces, no identifiable individuals.** Objects, materials, textures, light, and abstract forms only.
- **Commercial-safe for client-facing assets.** Use **Adobe Firefly Image Model 5** (trained on licensed/public-domain material, IP indemnification on qualifying plans) for anything shipping publicly. Use Midjourney/Flux/Imagen for exploration and non-literal art.
- **Rights:** purely AI-generated images generally aren't copyrightable in the US — you can't protect them, and your commercial-use rights come from each tool's terms, not ownership. Check the license on anything you ship.
- **Consistency across a set:** use a style reference — Midjourney **SREF / Omni Reference**, or Flux multi-reference — so all assets read as one house. Lock a seed/SREF early and reuse it.
- **Generate big, then treat:** render at max resolution (or upscale to 4000px+), then export to the formats in §4.

---

## 1. The House style (reuse on every prompt)

**Style suffix** (append to each prompt):
> "...dark warm-espresso background (#0E0B0C), oxblood and aubergine shadows, champagne-gold (#C8A45C) accents, a subtle iridescent oil-slick sheen (soft pink, mint, lilac), single dramatic spotlight, volumetric haze, fine film grain, high-contrast editorial fashion lighting, museum/gallery presentation, medium-format photographic realism, glossy and opulent, cinematic, ballroom-glamour mood, extremely high detail."

**Negative prompt** (every generation):
> "people, person, face, model, portrait, eyes, skin, hands, text, letters, words, watermark, logo, signage, cartoon, illustration, plastic, uncanny, low-resolution, cluttered background."

**Params:** Midjourney V7 — append `--style raw --s 250 --q 2` and an aspect ratio per slot; reuse one `--sref <id>` across the set. Flux.2 — specify focal length / aperture / "medium format" in-prompt; use a reference image for series consistency.

**Aspect ratios:** hero & full-bleed `21:9` or `16:9`; vitrine objects & category heroes `4:5` or `1:1`; macro textures `3:2`; silhouettes `2:3`.

---

## 2. Asset lanes & prompts

### Lane A — Hair filaments / strand fields
*Used for: the homepage WebGL field texture, hero filament plates, the "curtain."*
- "A dense vertical field of fine human-hair strands suspended in dark space, individual filaments catching warm rim light, gold and deep-brown tones with occasional iridescent highlight, gentle flowing curves, deep volumetric depth fading into black, [style suffix]"
- "Macro of glossy hair filaments flowing like liquid silk through darkness, champagne-gold light raking across the strands, a few iridescent threads, motion and depth, [style suffix]"
- "An abstract curtain of cascading hair extensions, theatrical spotlight from above, strands parting to one side, rich and opulent, [style suffix]"

### Lane B — Material macro (the substance)
*Used for: category openers, texture sections.*
- "Extreme close-up of a single coiled lock of dark glossy hair, every strand sharp, raking gold light revealing sheen and texture, shot like luxury leather-grain photography, [style suffix]"
- "Macro of the woven architecture of a hair extension wefт, intricate over-under structure, warm light, fashion-material detail, [style suffix]"
- "Macro of a tightly coiled braid, sculptural geometry, deep shadow and gold highlight, abstract and tactile, [style suffix]"

### Lane C — Sculptural vitrine objects
*Used for: category heroes, the Lookbook (the honest portfolio replacement).*
- "A sculptural updo presented as an abstract art object on a dark stone plinth in a black gallery, single spotlight, hair shaped into a flowing architectural form, couture object, museum lighting, no head no face, [style suffix]"
- "A coiled braid sculpture displayed like a fashion object in a vitrine, glossy and rope-like, champagne light, dark gallery, [style suffix]"
- "An abstract wig form on a matte-black bust stand (no facial features), sculptural silhouette, dramatic spotlight, presented as couture, [style suffix]"

### Lane D — Light & haze / atmosphere
*Used for: backgrounds, scene transitions, the stage.*
- "An empty dark runway under a single warm spotlight, volumetric haze, soft iridescent reflections on a glossy black floor, cinematic ballroom stage, no people, [style suffix]"
- "Abstract flow of liquid-gold and iridescent light suggesting hair in motion through darkness, no figure, atmospheric and opulent, [style suffix]"
- "Volumetric haze and shafts of warm light in a black void, embers of champagne and soft pink, deep atmosphere, [style suffix]"

### Lane E — Silhouette / shadow
*Used for: graphic punctuation, transitions.*
- "Dramatic silhouette of a sculptural hairstyle defined only by its outline and hairline against a softly lit dark backdrop, graphic and editorial, no facial features, [style suffix]"
- "Shadow-play of a flowing updo cast on a dark textured wall, gold edge light, abstract form, [style suffix]"

### Lane F — Environment / set
*Used for: the dark gallery/runway space behind content.*
- "A dark editorial atelier interior, black walls, a single plinth and spotlight, glossy floor, haze, luxury fashion-house gallery, empty, [style suffix]"

---

## 3. Special workflows

- **Depth maps (for the hero parallax / WebGL):** generate a hero still, then produce a matching depth map (some tools export depth; otherwise run a monocular depth-estimation pass). The RGB + depth pair drives a parallax/camera-move in CSS-3D or Three.js — "video-like" life from one frame.
- **Scrubbed cinematic motion (a head-turn/transformation):** this is an **image sequence**, not a `<video>`. Take a hero still through an **image-to-video model** (Kling / Runway / Veo-class), then export the clip to numbered frames drawn to canvas on scroll. Expect to fight artifacts; abstract material/light motion animates far more cleanly than literal hair, so favor Lane A/D for any scrubbed moment.

---

## 4. Output specs

- **Resolution:** master 4000px+; downscale responsively.
- **Format:** **AVIF** primary, **WebP** fallback (plus PNG for cutouts).
- **Cutouts:** transparent PNG for any Lane C object that composites onto the dark stage.
- **Depth maps:** grayscale PNG paired with the hero still.
- **Naming:** `lane-[A–F]_[slot]_[NN].avif` (e.g., `laneC_category-bridal_01.avif`).
- **Sequences:** `seq_[name]_0001.webp …` sized to display resolution.

---

## 5. Tool quick-pick (current)

- **Art direction + cinematic quality + series consistency:** Midjourney V7 (SREF / Omni Reference).
- **Photoreal + control + local + LoRA/ControlNet + compositing:** Flux.2 (ComfyUI / Replicate / fal.ai).
- **Maximum photoreal hero:** Imagen 4 Ultra.
- **Same-subject consistency across a set:** Nano Banana Pro.
- **Client-facing, rights-safe:** Adobe Firefly Image Model 5 (indemnity on qualifying plans) — use for anything that ships.
