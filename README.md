# OPC & Resolution Enhancement Explorer

An interactive, simplified explainer of optical proximity correction (OPC)
and resolution enhancement in semiconductor photolithography — what we draw
on the photomask vs. what actually prints on the wafer.

## What it demonstrates

Chips are patterned by shining light through a stencil (the *photomask*), and
at modern feature sizes the shapes are smaller than the wavelength of the
light drawing them — so the printed result is a blurred version of the mask:
corners round off, line ends pull back, thin features shrink. Chipmakers
compensate by drawing the mask "wrong" on purpose. This tool lets you flip
through the standard tricks and watch mask and print change side by side:
rule-based vs. model-based OPC, serifs and hammerheads, shaped illumination
(annular, quadrupole), sub-resolution assist features (SRAFs), and — in the
periodic-pattern view — alternating phase-shift masks.

## Simplifications made

This is a teaching toy, not an optical simulation. Real OPC runs rigorous
imaging models on large compute clusters; this tool deliberately does not:

- **No optics are computed** — no diffraction, no Fourier/Hopkins imaging,
  no photoresist chemistry.
- The printed shape is the design target degraded by four hand-tuned
  parameters (corner rounding, line-end pullback, edge thinning/bloat, and
  edge softness), with values per toggle chosen so every control moves the
  result in the physically correct *direction* with roughly the right
  relative magnitude.
- Illumination modes, OPC levels, SRAF benefits, and the phase-shift
  sharpening are all **visually tuned approximations** of real qualitative
  behaviors — including quadrupole's H/V-vs-corner trade-off, which is
  deliberately preserved.
- The mask decorations (staircase nudges, serifs, hammerheads, assist bars)
  are illustrative geometry, not the output of a real OPC engine.

## How to run it

Open `opc_litho_visualizer.html` in any modern browser — just double-click
it. No install, no build step, no server required; everything is one
self-contained file.

## File structure

- `opc_litho_visualizer.html` — the whole tool: page, styles, both views, and
  the stylized "print" model (commented in the source).
- `README.md` — this file.

## Credits / basis

Conceptually based on standard semiconductor lithography references (optical
proximity correction, off-axis illumination, sub-resolution assist features,
alternating phase-shift masks). No claim of authorship of the underlying
science. Companion piece to the Czochralski crystal-pulling simulator
(`Silicon_Pull/`).

## Things to try

- Zigzag + OPC Off: the "why OPC exists" baseline — rounded corners,
  pulled-back ends, a thinned trace. Then Model-based + 2-D effects: the
  print snaps to the dashed target.
- With Model-based selected, flip the 2-D checkbox — the width stays right
  either way, but corners and line ends only recover with 2-D on. Try the
  same flip on the Rectangle: it barely moves.
- Rectangle + Quadrupole looks nearly perfect; Zigzag + Quadrupole has
  crisper edges but visibly rounder corners than Annular.
- Grating view, everything Off: fat, merged bars. Toggle Alternating PSM —
  the printed grating sharpens and thins dramatically.
