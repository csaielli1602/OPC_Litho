# OPC & Resolution Enhancement Explorer

A single-file explainer for optical proximity correction (OPC) in
photolithography — the mask trick that makes it possible to print chip
features smaller than the wavelength of light doing the printing. Draw the
mask "wrong" in specific, calculated ways, and the blur of the optics
reconstructs the shape you actually wanted.

## How it actually works

Everything lives in `index.html`, one file, no dependencies. Three test
shapes (rectangle, T-piece, zigzag) and a six-bar grating are defined as
arrays of polygon points. Two SVG panels redraw on every control change: the
mask panel shows what you'd actually draw — corrected polygon, plus serifs,
hammerheads, or SRAFs depending on your settings — and the print panel shows
a degraded, blurred version standing in for what lands on the wafer.

The degradation is four numbers per shape: corner-rounding radius, line-end
pullback distance, uniform edge inset (or bloat), and a Gaussian blur
`stdDeviation` applied through an SVG filter. Illumination mode sets a
baseline for those four numbers (`ILLUM` in the source), OPC level
multiplies them down (`OPCM`), and each shape carries its own severity
multiplier so a busy zigzag degrades harder than a plain rectangle. Flip a
control and the numbers animate to their new target over 320 ms with a
smoothstep ease, re-rendering the print panel every frame — that's what
makes switching OPC levels feel like the shape sharpening, rather than
snapping between two static states.

None of this touches actual optics. There's no diffraction integral, no
Hopkins imaging model, no resist chemistry. The four defect numbers are
chosen by hand so each control moves the print in the physically correct
direction — quadrupole illumination genuinely does sharpen horizontal and
vertical edges at the cost of rounder corners in real lithography, and that
trade-off is deliberately preserved here, but the magnitudes are picked to
look right on screen, not computed from wave optics.

## Toy vs. real calibration

Production OPC doesn't degrade a shape with four scalar knobs. It runs a
full imaging model (Hopkins or Abbe formulation, fed by measured scanner
data — numerical aperture, illumination pupil shape, resist contrast
curves) across every polygon edge on a mask, iterating the mask geometry
until the simulated print matches the design target to single-digit
nanometers, for hundreds of millions of shapes per layer. That's why OPC
normally runs on a compute cluster overnight instead of reacting to a
slider in real time. This tool compresses that whole pipeline into
hand-picked numbers that move the right direction, so you can feel what
rule-based versus model-based correction buys you, or why a phase-shift
mask sharpens a grating, without any of the physics underneath.

## How to run it

Open `index.html` in any modern browser — just double-click it. No install,
no build step, no server required.

## File structure

- `index.html` — the whole tool: page, styles, both views, and the
  stylized "print" model, with comments in the source explaining the
  parameter tables.
- `README.md` — this file.

## Credits / basis

Concepts follow standard lithography references: optical proximity
correction, off-axis illumination, sub-resolution assist features,
alternating phase-shift masks. No claim of authorship over the underlying
science. Companion piece to the Czochralski crystal-pulling simulator
(`Silicon_Pull/`).

## Things to try

- Zigzag with OPC off: rounded corners, pulled-back ends, a thinned trace —
  this is the baseline problem OPC exists to fix. Switch to model-based
  with 2-D effects on and the print snaps onto the dashed target.
- With model-based selected, toggle the 2-D checkbox. Width stays right
  either way, but corners and line ends only recover with 2-D on. Try the
  same toggle on the rectangle — it barely moves, because a rectangle has
  almost no 2-D complexity to fix.
- Rectangle with quadrupole illumination looks nearly perfect. Zigzag with
  quadrupole has crisper edges but visibly rounder corners than annular
  gives you.
- Switch to the grating view with everything off: fat, merged bars. Toggle
  alternating phase-shift masking and watch the printed grating sharpen and
  thin.
