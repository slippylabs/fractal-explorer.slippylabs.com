# Fractal Explorer

Zoom into the Mandelbrot set, Julia sets, the Burning Ship and more — smooth continuous colouring, progressive rendering, shareable coordinates and a PNG export. Runs entirely in your browser.

**Live:** <https://fractal-explorer.slippylabs.com/>

## What it does

- Mandelbrot, Julia, Burning Ship, Tricorn and Multibrot (z^d + c for d = 2..8).
- Drag to pan, scroll or pinch to zoom, double-click to dive; shift-click any point in Mandelbrot view to jump to the Julia set it generates.
- Six palettes, three colour curves, an iteration limit that scales with zoom, and optional palette cycling.
- Fourteen preset locations — Seahorse Valley, Elephant Valley, the triple spiral, a mini Mandelbrot, a 10^9× spiral, the Douady rabbit, a Siegel disc.
- The URL always holds the current view, so every view is a link. PNG export of the frame.

## How it works

Colour comes from the continuous escape estimate, not the integer iteration count:

```
nu = n + 1 - log_d( ln|z| / ln(bailout) )
```

which lands in (n, n+1] and removes the stair-steps a raw count produces. Rendering is progressive — a coarse pass appears immediately, then finer ones, each with a frame time budget, so the page never locks up; a stale pass abandons itself the moment the view changes. Points provably inside the main cardioid or the period-2 bulb skip iteration entirely.

Everything is computed in `double`, which runs out at roughly 10^15× magnification. The readout says when you are close, and warns when the precision floor is breached.

## Verification

Escape counts for all five families were checked against a float64 reference and, for early escapes, against the same iteration carried out in 60-digit decimal arithmetic. The smooth estimate was checked against the escape-time potential: computing it at bailout 256 and at 10^6 must differ by *one constant for every pixel*, analytically log_d(ln b₂ / ln b₁) — which catches a wrong log base or a misplaced term that no amount of looking at the picture would. Plus statements that hold regardless of implementation: the main cardioid and period-2 bulb never escape, |c| > 2 always does, the set is exactly symmetric about the real axis, and the Julia set of c = 0 is the unit disc.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/fractal-explorer.slippylabs.com.git
cd fractal-explorer.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
