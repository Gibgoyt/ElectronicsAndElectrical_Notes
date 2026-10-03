# The buck (step-down) converter

Deep treatment of the buck converter: how <!--m:V_{out} = D \cdot V_{in}-->![V_out = D V_in](README.assets/eq-inline/645cc1e7b2.svg)<!--/m--> is *derived* from volt-second balance,
and how to size the inductor and output capacitor. Full argument in [buck.md](buck.md).

> **The thesis in one line**
>
> The buck's step-down ratio is not a rule to memorise — it is forced by the requirement that the
> inductor's average voltage over one cycle be zero.

## Contents

The treatment covers:

1. The circuit, and the ON/OFF vocabulary (closed = ON).
2. The two switching intervals and the inductor voltage in each (fig-01).
3. Volt-second balance → <!--m:V_{out} = D \cdot V_{in}-->![V_out = D V_in](README.assets/eq-inline/645cc1e7b2.svg)<!--/m--> (fig-02).
4. Sizing the inductor from a target current ripple.
5. Sizing the output capacitor from the triangle-charge argument (fig-03).
6. Worked numbers: 12 V → 3 V.
7. What this costs you.
8. Sources and cross-links.

## Reading order

Read [buck.md](buck.md) after the [inductor](../../fundamentals/inductor/inductor.md) and
[capacitor](../../fundamentals/capacitor/capacitor.md) laws, then the mirror in
[../boost/boost.md](../boost/boost.md).

## Links

- Parent index: [../README.md](../README.md)
- Style guide: [../../STYLE.md](../../STYLE.md)
- The laws it uses: [../../fundamentals/inductor/inductor.md](../../fundamentals/inductor/inductor.md),
  [../../fundamentals/capacitor/capacitor.md](../../fundamentals/capacitor/capacitor.md)
- Mirror circuit: [../boost/boost.md](../boost/boost.md)
