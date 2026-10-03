# The inductor

Deep treatment of the first of the two fundamental laws: <!--m:V_L = L \cdot dI/dt-->![V_L = L dI/dt](README.assets/eq-inline/4e5d46c427.svg)<!--/m-->. The full argument is
in [inductor.md](inductor.md), in eight short sections with three figures.

> **The thesis in one line**
>
> The voltage across an inductor is proportional to *how fast its current is changing* — so a
> constant voltage forces the current to climb (or fall) in a perfectly straight line.

## Contents

The treatment covers:

1. What an inductor actually is — a coil, storing energy in a magnetic field (no plates).
2. The defining law, symbol by symbol, and the units of the henry.
3. From the law to the ramp — the definite-integral proof, done slowly (fig-01).
4. Polarity, Lenz, and back-EMF — resistor-like while charging, battery-like while
   discharging; the sign flip that later powers the boost converter (fig-03).
5. At the poles — current flows straight through the coil (fig-02).
6. The inductive-kick footgun and why the diode has to be there.
7. What this costs you.
8. Sources and cross-links.

## Reading order

Read [inductor.md](inductor.md) top to bottom, then the mirror in
[../capacitor/capacitor.md](../capacitor/capacitor.md). The two together are everything the
converters need.

## Links

- Parent index: [../README.md](../README.md)
- Style guide: [../../STYLE.md](../../STYLE.md)
- Mirror law: [../capacitor/capacitor.md](../capacitor/capacitor.md)
- Where it is used: [../../dc-dc-converters/buck/buck.md](../../dc-dc-converters/buck/buck.md),
  [../../dc-dc-converters/boost/boost.md](../../dc-dc-converters/boost/boost.md)
