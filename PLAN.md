# Build plan — Venom Magnetic Button V2

## User-observed issues in V1

1. The magnetic influence area felt too small.
2. Tendrils appeared to break; when the cursor moved away, they first fell toward a fixed lower fallback point and then reconnected/moved again.
3. Motion felt generic and buggy rather than viscous and intentional.

## Research-backed approach

- Use GSAP core `quickTo()` for repeatedly retargeted pointer-driven properties, rather than creating a new tween on every mouse event.
- Use `gsap.ticker` for SVG geometry updates so the custom path animation stays synchronized with GSAP.
- Use custom cubic Bézier paths and pointer-velocity-dependent control points for organic strands; a WebGL scene or MorphSVG dependency would add complexity without clear benefit for ~31 SVG paths.
- Keep the magnet field broad and continuously weighted, instead of switching strands to a fabricated target when the cursor crosses one hard boundary.

References:
- https://gsap.com/docs/v3/GSAP/gsap.quickTo/
- https://gsap.com/docs/v3/GSAP/gsap.ticker/
- https://tympanus.net/codrops/2020/08/05/magnetic-buttons/
- https://tympanus.net/codrops/2019/01/31/custom-cursor-effects/

## V2 implementation

1. Center the glossy black CTA on a warm textured background.
2. Expand the magnetic radius to about 740 CSS pixels from the viewport centre.
3. Use GSAP retargetable tweens for X/Y/rotation/scale and for the cursor endpoint.
4. Anchor every strand to the button's ellipse edge on the side facing the pointer.
5. Smooth cursor tracking independently from button tracking and add filtered velocity to strand curves.
6. Remove the old hardcoded fallback target below the button. During release, keep the last real pointer coordinate while the strand opacity fades smoothly.
7. Keep pointer/touch and reduced-motion behavior as explicit fallbacks.
8. Keep the solution lightweight: GSAP core + native SVG, no WebGL/canvas renderer or paid plugin.

## Validation

- [x] The inline JavaScript passes `node --check`.
- [x] The new code removes the old fixed-bottom fallback endpoint.
- [x] The GitHub repository has the V2 HTML and updated docs.
- [ ] Manually test rapid top-to-bottom and diagonal pointer movement.
- [ ] Manually test pointer exiting the viewport and immediately re-entering.
- [ ] Confirm the visual intensity on desktop sizes and reduced-motion behavior.

## Honest limitation

The code change has been syntax-checked, but the updated browser motion has not yet been visually confirmed in a live human-operated browser. A webpage cannot physically reposition a system cursor; the design creates the capture illusion visually.
