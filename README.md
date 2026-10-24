# Venom Magnetic Button — Liquid Capture

A single-page interaction experiment inspired by the supplied glossy black “let’s collaborate” button. The pointer connection is built from a short gooey mass at the button edge, separate variable-width liquid ribbons, fine irregular filaments, dangling drips, and a hand-drawn pointer.

## Run locally

No build step is required. Open `index.html` in a modern desktop browser, or run:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## How the effect is built

- A warm, lightly textured background and a glossy, tilted black pill.
- SVG `feGaussianBlur → feColorMatrix` for a thresholded goo silhouette, then `feTurbulence → feDisplacementMap` for an irregular, wet edge.
- A short root mass stays connected to the button; tapered ribbon streams and smaller hair-like paths stay separately animated along the tether. This avoids blending every stream into a single flat triangular cable.
- The paths use multiple Bézier segments with filtered pointer velocity and independent phases, so they bend unevenly as the pointer moves.
- Unequal gravity-driven drips hang from the button's edge; an illustrated hand follows the smoothed pointer position.
- GSAP `quickTo()` is loaded from jsDelivr for retargetable pointer/button motion. A requestAnimationFrame fallback keeps the interaction usable when the CDN is unavailable.
- The real OS pointer is hidden during the effect and replaced with the illustrated hand. Ordinary webpages cannot physically move the system cursor; the capture is visual.

## Tune the effect

The interaction settings are near the top of the script in `CFG`:

- `fieldRadius`: interaction radius (CSS pixels from viewport centre).
- `pull`: button movement amount.
- `baseTilt` and `tilt`: resting and reactive rotation.
- `streamCount`, `threadCount`, `dripCount`: number of liquid ribbons, fine filaments, and hanging drips.

SVG filter intensity and path roughness are under `<defs>` in `index.html`. Keep the broad body filter on the short liquid mass; filtering every long filament together tends to merge it into a filled wedge.

## Research references

- [Codrops — Gooey Cursor Effect](https://tympanus.net/codrops/2023/06/07/gooey-cursor-effect/) — SVG goo filters and the combination of blur, alpha threshold, and displacement.
- [Codrops — Creative Gooey Effects](https://tympanus.net/codrops/2015/03/10/creative-gooey-effects/) — fundamentals of SVG gooey filter primitives.
- [GSAP `quickTo()`](https://gsap.com/docs/v3/GSAP/gsap.quickTo/) — repeatedly retargeting pointer-driven animation without creating a new tween for each event.
- [GSAP ticker](https://gsap.com/docs/v3/GSAP/gsap.ticker/) — synchronize geometry updates with the animation loop.
- [Interactive WebGL metaballs](https://github.com/sigco3111/interactive-metaballs) — a GPU-based alternative. WebGL was not chosen here because this is a single 2D interaction and SVG remains simpler to adjust.

## Validation

- [x] Inline JavaScript passed `node --check`.
- [x] Chromium/Playwright pointer simulation ran with no JavaScript errors.
- [x] Tested a pointer near the lower-left, above the button, and through a rapid diagonal change.
- [ ] Still worth testing manually in your regular browser with GSAP CDN available; the test container could not reach jsDelivr, so its fallback animation path was the one exercised there.

## Browser notes

Touch/coarse pointers and `prefers-reduced-motion` receive a reduced/static treatment. The effect makes the button and ink respond to the pointer; it does not move the operating-system cursor.
