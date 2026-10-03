# Fundamentals — the two defining laws

The whole of switched-mode power conversion rests on two component laws and a little calculus.
This topic builds both from first principles, so that when you reach the
[buck](../dc-dc-converters/buck/) and [boost](../dc-dc-converters/boost/) converters, nothing
is asserted — it is all derived.

> **The thesis in one line**
>
> An inductor fixes how fast **current** can change; a capacitor fixes how fast **voltage** can
> change. They are exact mirrors of each other — learn one and you get the other by swapping
> <!--m:V \leftrightarrow I-->![V I](README.assets/eq-inline/4ebcd6796a.svg)<!--/m--> and <!--m:L \leftrightarrow C-->![L C](README.assets/eq-inline/6cef383173.svg)<!--/m-->.

## Documents

| Subtopic | The law | What it covers |
|---|---|---|
| [inductor/](inductor/) | <!--m:V_L = L \cdot dI/dt-->![V_L = L dI/dt](README.assets/eq-inline/4e5d46c427.svg)<!--/m--> | magnetic-field storage, the constant-voltage ramp (with the definite-integral proof done slowly), the polarity flip that powers a boost, and the inductive-kick footgun |
| [capacitor/](capacitor/) | <!--m:I_C = C \cdot dV/dt-->![I_C = C dV/dt](README.assets/eq-inline/56baca3b41.svg)<!--/m--> | electric-field storage, **the proper proof of why you may differentiate <!--m:Q = C \cdot V-->![Q = C V](README.assets/eq-inline/205c11f7c4.svg)<!--/m-->**, the mirror ramp, and the inductor↔capacitor duality |

## Reading order

1. [inductor/inductor.md](inductor/inductor.md) — start here; the ramp argument is simplest to
   see with current as the effect.
2. [capacitor/capacitor.md](capacitor/capacitor.md) — the mirror, plus the calculus rule that
   the whole subject leans on.

## Conventions

Follows [../STYLE.md](../STYLE.md). All maths and diagrams are generated SVGs — display equations,
inline symbols, and the arXiv-style <!--m:V-->![V](README.assets/eq-inline/c9ee5681d3.svg)<!--/m-->-vs-<!--m:t-->![t](README.assets/eq-inline/8efd86fb78.svg)<!--/m--> / <!--m:I-->![I](README.assets/eq-inline/ca73ab6556.svg)<!--/m-->-vs-<!--m:t-->![t](README.assets/eq-inline/8efd86fb78.svg)<!--/m--> figures are each typeset once by the
[toolchain](../../toolchain/README.md) and embedded as images, so they render identically
everywhere with no markdown math plugin.
