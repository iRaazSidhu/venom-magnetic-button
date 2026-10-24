# Build plan — Venom Magnetic Button

## Goal
Build a focused, responsive one-page demo of a glossy black CTA that appears to catch a nearby pointer with animated, ink-like tendrils.

## Design decisions
1. Keep the CTA centered at desktop and mobile sizes.
2. Use a warm off-white/paper backdrop and a glossy black, rounded pill to stay close to the supplied reference.
3. Keep the interaction radius invisible; the button should react before the pointer reaches it.
4. Use curved SVG strands rather than raster art so tendrils can stretch in real time.
5. Use requestAnimationFrame and interpolation for smooth movement; cap translation to avoid letting the button drift out of place.
6. Include focus-visible treatment, touch fallback, and reduced-motion handling.

## Interaction model
- Pointer distance controls a quadratic magnetic strength curve.
- The button follows a limited fraction of pointer offset and gains slight rotation/scale near the center.
- Thirteen SVG strands use responsive endpoints and time-varying Bezier control points.
- Leaving the field returns the button to rest and fades the strands.
- Clicking the CTA triggers brief demo feedback; no network or navigation occurs.

## Validation checklist
- [ ] Open `index.html` in a modern desktop browser.
- [ ] Verify the CTA stays visually centered at rest.
- [ ] Move the pointer around the magnetic field and confirm pull strength ramps smoothly.
- [ ] Verify strands track the pointer without intercepting clicks.
- [ ] Move away and confirm clean release/reset.
- [ ] Test a narrow viewport and keyboard focus.
- [ ] Enable reduced motion and verify movement/tendrils are suppressed.
- [ ] Confirm no external runtime dependencies.

## Scope note
A normal web page cannot physically reposition the operating-system cursor. The effect communicates capture through button motion and tendrils reaching toward the real pointer.
