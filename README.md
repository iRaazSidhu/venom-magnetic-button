# Venom Magnetic Button

A single-page, Venom-inspired magnetic CTA experiment: a glossy black pill, a cursor-sensitive magnetic field, and animated ink-like tendrils.

## Live preview / deploy

This is a static website with no build step. It can be deployed via GitHub Pages or Vercel. For GitHub Pages, open **Settings → Pages**, choose **Deploy from a branch**, select **main** and **/(root)**, and save.

## Run locally

Open `index.html` directly in a modern desktop browser, or run a local server:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Interaction

- The button is centered in the viewport.
- Move a mouse within roughly **230 px** of the button to activate its magnetic field.
- The button leans toward the pointer with a limited translation and subtle rotation.
- SVG Bézier strands stretch toward the pointer and gently undulate.
- Move away to release the button; click it for a small `gotcha.` feedback state.
- Touch/coarse pointers and `prefers-reduced-motion` users receive a reduced-motion version.

> Browsers do not let regular web pages physically move the operating-system cursor. This demo creates the attraction illusion by moving the button and extending tendrils toward the real pointer.

## Tune the effect

In `index.html`, adjust these constants near the top of the script:

- `MAGNETIC_RADIUS` — how far away attraction begins.
- `MAX_PULL` — maximum button travel.
- `STRAND_COUNT` — number of animated tendrils.
- The `0.27` / `0.25` multipliers in `tick()` — horizontal and vertical pull strength.

The look uses CSS and native SVG only; there are no external image, font, or JavaScript requests.

## Research notes

The interaction follows established magnetic-button patterns: use a generous surrounding activation zone, apply a smoothed fraction of pointer offset, and reset to the resting position after the pointer leaves. References:

- [Studio Cursor — magnetic button](https://studiocursor.com/effects/magnetic-button/)
- [Kinetics — magnetic button hover effect](https://kinetics.colorion.co/effects/magnetic-button/)
- [phucbm/magnetic-button — lightweight implementation reference](https://github.com/phucbm/magnetic-button)

The custom visual treatment (warm paper surface, glossy black pill, moving SVG tendrils) is original for this demo and adapted to the supplied screenshot's direction.
