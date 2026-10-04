# The inductor

Deep treatment of the first of the two fundamental laws: ![V_L = L times dI/dt](README.assets/eq-inline/4e5d46c427.svg)<!--m:V_L = L \cdot dI/dt-->. The full argument is
in [inductor.md](inductor.md), in nine short sections with six figures (Figures 1–3, 90, 135 and
136; the last two animated).

> **The thesis in one line**
>
> The voltage across an inductor is proportional to *how fast its current is changing* — so a
> constant voltage forces the current to climb (or fall) in a perfectly straight line. It is not an
> extra assumption: it follows from Faraday's law of induction applied to the coil's own flux.

## Contents

The treatment covers:

1. What an inductor actually is — a coil, storing energy in a magnetic field (no plates);
   Ampère's law and the solenoid field it makes (full treatment:
   [../amperes-law/](../amperes-law/)); the right-hand rule (Figure 135, animated).
2. Faraday's law — magnetic flux and flux linkage, Faraday's law of induction, Lenz's minus sign,
   and the five-step derivation of the inductor law, including where the minus sign goes
   (fig-04, Figure 90; animated step by step in Figure 136). Full treatment:
   [../faradays-law/](../faradays-law/).
3. The defining law, symbol by symbol, the units of the henry, and why power is VI; the
   stored energy derived.
4. From the law to the ramp — the definite-integral proof, done slowly (fig-01).
5. Polarity, Lenz, and back-EMF — resistor-like while charging, battery-like while
   discharging; the sign flip that later powers the boost converter (fig-03).
6. At the poles — current flows straight through the coil (fig-02).
7. The inductive-kick footgun and why the diode has to be there.
8. What this costs you.
9. Sources and cross-links.

## Reading order

Read the field laws it builds on first — [../amperes-law/](../amperes-law/) and
[../faradays-law/](../faradays-law/) (and [../coulombs-law/](../coulombs-law/) before them) — or
follow the links from inductor.md as you need them. Read [inductor.md](inductor.md) top to bottom, then the mirror in
[../capacitor/capacitor.md](../capacitor/capacitor.md). The two together are everything the
converters need.

## Links

- Parent index: [../README.md](../README.md)
- Style guide: [../../STYLE.md](../../STYLE.md)
- Mirror law: [../capacitor/capacitor.md](../capacitor/capacitor.md)
- Field laws: [../amperes-law/](../amperes-law/), [../faradays-law/](../faradays-law/); the overview
  [../electromagnetism/](../electromagnetism/)
- Where it is used: [../../dc-dc-converters/buck/buck.md](../../dc-dc-converters/buck/buck.md),
  [../../dc-dc-converters/boost/boost.md](../../dc-dc-converters/boost/boost.md)
