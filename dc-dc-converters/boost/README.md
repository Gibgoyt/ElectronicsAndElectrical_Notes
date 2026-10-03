# The boost (step-up) converter

Deep treatment of the boost converter: <!--m:V_{out} = V_{in}/(1-D)-->![V_out = V_in/(1-D)](README.assets/eq-inline/ff557ad27a.svg)<!--/m-->, derived by the same volt-second
balance as the buck, and why its output capacitor has to be bigger. Full argument in
[boost.md](boost.md).

> **The thesis in one line**
>
> Swap the order of the inductor and the switch and the same volt-second balance now *raises* the
> output above the input — because the inductor's stored energy is dumped on top of <!--m:V_{in}-->![V_in](README.assets/eq-inline/29f560cdfe.svg)<!--/m-->.

## Contents

The treatment covers:

1. The circuit — inductor first, switch to ground (fig-01).
2. The two intervals, with the roles swapped from the buck.
3. Volt-second balance → <!--m:V_{out} = V_{in}/(1-D)-->![V_out = V_in/(1-D)](README.assets/eq-inline/ff557ad27a.svg)<!--/m-->.
4. Sizing the inductor.
5. Sizing the output capacitor — genuinely different from the buck (fig-02).
6. Worked numbers: 12 V → 24 V, and the ~3× comparison.
7. What this costs you.
8. Sources and cross-links.

## Reading order

Read [../buck/buck.md](../buck/buck.md) first — this document leans on that derivation and only
highlights what changes.

## Links

- Parent index: [../README.md](../README.md)
- Style guide: [../../STYLE.md](../../STYLE.md)
- The laws it uses: [../../fundamentals/inductor/inductor.md](../../fundamentals/inductor/inductor.md)
  (especially the polarity flip),
  [../../fundamentals/capacitor/capacitor.md](../../fundamentals/capacitor/capacitor.md)
- Mirror circuit: [../buck/buck.md](../buck/buck.md)
- Start-up (the 3.png staircase, no-load runaway, settling, real gain limit, efficiency vs ratio): [startup.md](startup.md)
