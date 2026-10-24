# Venom Magnetic Button — V2

A Venom-inspired magnetic CTA experiment: a glossy black pill on a warm paper surface, a larger cursor attraction field, and animated SVG ink tendrils that reach toward the live pointer.

## Run locally

This is a static page with no build step. Open `index.html` in a modern desktop browser, or start a local server:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Interaction

- The button stays centered at rest and tilts/pulls toward the pointer.
- The magnetic influence radius is approximately **740 CSS pixels** from the viewport centre, so the effect responds much earlier than the previous version.
- GSAP `quickTo()` handles responsive pointer tracking with interruption-friendly tweens.
- SVG Bézier strands use the button's ellipse edge as their anchor and follow a smoothed cursor target.
- Strand control points react subtly to pointer velocity for a more organic, ink-like pull.
- When the pointer leaves the attraction field or viewport, the button returns and the filaments fade out from their last real pointer position. No artificial fallback target is used.
- Touch/coarse pointers and `prefers-reduced-motion` use a reduced/static state.
- Clicking the button only provides a short press response; no network request or navigation occurs.

> A webpage cannot physically move the operating-system cursor. This demo conveys a grab/capture feeling by moving the button and extending animated tendrils toward the pointer.

## Tune the effect

In `index.html`, adjust the `CONFIG` object:

- `fieldRadius`: increase/decrease how far from the centre the effect begins.
- `maxPullX` / `maxPullY`: limit the button's movement.
- `pullX` / `pullY`: movement strength.
- `strandCount` / `hairCount`: visual thread density.
- `fieldFade`: how quickly strands appear/disappear.

The standalone page loads GSAP 3.15.0 from jsDelivr. SVG geometry itself is custom-generated in the ticker; no paid MorphSVG plugin or WebGL dependency is required for this effect.

## Research notes

The implementation combines established patterns rather than adding a heavy rendering engine for a small number of curves:

- [GSAP `quickTo()`](https://gsap.com/docs/v3/GSAP/gsap.quickTo/) — optimized retargetable tweens suitable for rapidly changing pointer coordinates.
- [GSAP ticker](https://gsap.com/docs/v3/GSAP/gsap.ticker/) — synchronizes the custom SVG path update loop with the animation engine.
- [Codrops — Magnetic Buttons](https://tympanus.net/codrops/2020/08/05/magnetic-buttons/) — examples of magnetic movement and separately animated button details.
- [Codrops — Custom Cursor Effects](https://tympanus.net/codrops/2019/01/31/custom-cursor-effects/) — reference for cursor-following graphics and organic/noisy motion.

## Validation checklist

- [x] Inline JavaScript passes `node --check`.
- [x] Replaced the previous fixed lower fallback endpoint, which could pull tendrils downward when the cursor left the attraction area.
- [x] Cursor movement is tracked at viewport level, not just on the button.
- [x] Button motion and cursor/tendril motion are smoothed independently.
- [ ] Verify movement visually in a desktop browser at normal and high pointer speeds.
- [ ] Verify on a narrow viewport and with reduced-motion enabled.
