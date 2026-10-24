# Build plan — Venom Magnetic Button (Liquid Capture)

## What the previous preview got wrong

- The tendrils were too straight and uniform, like a handful of rigid cables.
- The large ribbons overlapped into a single triangular wedge.
- The connection looked too clean and lacked the irregular edge, wet shine, and hanging drips from the reference.
- A hard fallback point could make the strands re-aim abruptly as the pointer moved through different areas.

## Research and rendering choice

1. Use a thresholded SVG goo filter for the compact liquid mass nearest the button: blur the source, threshold its alpha with a color matrix, then displace the result with turbulence.
2. Keep the long strands separate from the thresholded root mass. When all long ribbons pass through the same goo filter, they can merge into a single filled wedge instead of retaining distinct tendrils.
3. Use multiple tapered Bézier ribbons plus fine curved filaments and add controlled turbulence-based displacement for an imperfect, viscous edge.
4. Use GSAP `quickTo()` for retargetable easing; retain a requestAnimationFrame fallback so the page remains interactive if the external CDN is blocked.
5. Keep this as SVG + GSAP rather than WebGL: the effect is one button and a relatively small number of paths, so a full GPU metaball renderer would add complexity without a clear benefit.

Reference reading:
- https://tympanus.net/codrops/2023/06/07/gooey-cursor-effect/
- https://tympanus.net/codrops/2015/03/10/creative-gooey-effects/
- https://gsap.com/docs/v3/GSAP/gsap.quickTo/
- https://gsap.com/docs/v3/GSAP/gsap.ticker/
- https://github.com/sigco3111/interactive-metaballs

## V9 implementation

- Centered glossy black pill on a warm paper backdrop, with a slightly angled pose.
- A short goo-filtered root mass and uneven lobe details cling to the button edge.
- Six narrow liquid ribbons, twenty-two fine filaments, and ten edge drips are animated independently.
- Bézier control points react to time and filtered pointer velocity to avoid uniform parallel lines.
- A hand-drawn cursor icon follows the eased pointer location. No artificial lower fallback endpoint is used.
- The connection fades and the button returns to rest when the pointer leaves the viewport.
- Includes touch/coarse-pointer and reduced-motion fallbacks.

## Validation performed

- [x] Extracted the inline JavaScript and passed it through `node --check`.
- [x] Chromium/Playwright simulation at 1214 × 612: pointer at lower-left connection, pointer above the button, then a rapid diagonal change.
- [x] No browser JavaScript exceptions in those simulated pointer tests.
- [ ] Confirm GSAP's CDN-loaded path and motion preference on your regular internet-connected browser. The test container could not reach jsDelivr, so the fallback path was exercised in-browser.

## Limitations

The effect is an illustrative “grab” illusion. Browser pages cannot physically move the operating system's pointer; the button moves and the drawn hand/tendrils track the cursor instead.
