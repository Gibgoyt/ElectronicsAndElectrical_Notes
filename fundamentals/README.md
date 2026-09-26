# Fundamentals — the two defining laws

The whole of switched-mode power conversion rests on two component laws and a little calculus.
This topic builds both from first principles, so that when you reach the
[buck](../dc-dc-converters/buck/) and [boost](../dc-dc-converters/boost/) converters, nothing
is asserted — it is all derived.

> **The thesis in one line**
>
> An inductor fixes how fast **current** can change; a capacitor fixes how fast **voltage** can
> change. They are exact mirrors of each other — learn one and you get the other by swapping
> `V ↔ I` and `L ↔ C`.

## Documents

| Subtopic | The law | What it covers |
|---|---|---|
| [inductor/](inductor/) | `V_L = L·dI/dt` | magnetic-field storage, the constant-voltage ramp (with the definite-integral proof done slowly), the polarity flip that powers a boost, and the inductive-kick footgun |
| [capacitor/](capacitor/) | `I_C = C·dV/dt` | electric-field storage, **the proper proof of why you may differentiate `Q = C·V`**, the mirror ramp, and the inductor↔capacitor duality |

## Reading order

1. [inductor/inductor.md](inductor/inductor.md) — start here; the ramp argument is simplest to
   see with current as the effect.
2. [capacitor/capacitor.md](capacitor/capacitor.md) — the mirror, plus the calculus rule that
   the whole subject leans on.

## Conventions

Follows [../STYLE.md](../STYLE.md). Maths appears three ways — display equations for the laws
and results, aligned ASCII fences for every-step derivations, and static SVG graphs for the
`V`-vs-`t` and `I`-vs-`t` shapes.
