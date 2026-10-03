# The capacitor

Deep treatment of the second fundamental law: <!--m:I_C = C \cdot dV/dt-->![I_C = C dV/dt](README.assets/eq-inline/56baca3b41.svg)<!--/m-->. The full argument is in
[capacitor.md](capacitor.md), in seven sections with three figures.

> **The thesis in one line**
>
> The current into a capacitor is proportional to *how fast its voltage is changing* — the exact
> mirror of the inductor, obtained by swapping voltage with current.

## Contents

The treatment covers:

1. What a capacitor actually is — two plates, storing energy in an electric field.
2. **The rule you need: when you may differentiate <!--m:Q = C \cdot V-->![Q = C V](README.assets/eq-inline/205c11f7c4.svg)<!--/m-->, and when you may not.** A proper
   proof, because "just differentiate both sides" is the step that trips everyone up.
3. The units of the farad.
4. From the law to the ramp — constant current makes a straight voltage ramp (fig-01).
5. At the poles — charge piles on the plates; nothing crosses the gap (fig-02).
6. The inductor↔capacitor duality, in one table (fig-03).
7. Sources and cross-links.

## Reading order

Read [capacitor.md](capacitor.md) after [../inductor/inductor.md](../inductor/inductor.md) —
section 2 here is the calculus rule the whole tree leans on, so do not skip it.

## Links

- Parent index: [../README.md](../README.md)
- Style guide: [../../STYLE.md](../../STYLE.md)
- Mirror law: [../inductor/inductor.md](../inductor/inductor.md)
- Where it is used: [../../dc-dc-converters/buck/buck.md](../../dc-dc-converters/buck/buck.md)
  (output-capacitor sizing),
  [../../dc-dc-converters/boost/boost.md](../../dc-dc-converters/boost/boost.md)
