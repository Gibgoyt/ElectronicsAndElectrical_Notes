# Electronics & Electrical — study notes

A ground-up, first-principles knowledge base for power electronics, built the same way the
[Proqmed storage docs](https://example.invalid) are: narrative prose that *explains*, one
hand-authored SVG figure per major idea, and every number stated verbatim with units. The
goal is understanding you can rebuild from scratch — not a formula sheet.

> **The thesis in one line**
>
> Two defining laws are the whole of it — one for the inductor, one for the capacitor:

![V_L = L dI_L/dt](README.assets/eq-inductor-law.svg) &nbsp; and &nbsp; ![I_C = C dV_C/dt](README.assets/eq-capacitor-law.svg)

Buck and boost converters are just those two laws applied to a circuit that switches on and off;
the step ratios are *derived* from them, never assumed.

## How to read this tree

Start at the fundamentals and only then open the converters — the converter maths is nothing
but the two fundamental laws applied twice per switching cycle.

| Topic | What it covers | Status |
|---|---|---|
| [fundamentals/](fundamentals/) | The two defining laws from first principles: the inductor law, the capacitor law, the calculus that connects a constant drive to a linear ramp, and what physically happens at the terminals | **written** |
| [dc-dc-converters/](dc-dc-converters/) | Buck (step-down) and boost (step-up): the two switching intervals, volt-second balance, the derived step ratios, and how to size L and C | **written** |

Suggested order:

1. [fundamentals/inductor/](fundamentals/inductor/) — the law, the ramp, the polarity flip.
2. [fundamentals/capacitor/](fundamentals/capacitor/) — the mirror law, and **the proper
   proof of why you may differentiate `Q = C·V`** (the step that trips everyone up).
3. [dc-dc-converters/buck/](dc-dc-converters/buck/) — `V_out = D·V_in`, derived.
4. [dc-dc-converters/boost/](dc-dc-converters/boost/) — `V_out = V_in/(1−D)`, same method.

## Conventions

Everything here follows [STYLE.md](STYLE.md): a one-line thesis, a numbered Contents, honest
"what this costs you" callouts, and figures that carry the argument on their own. **All maths and
all diagrams are generated SVGs** — every equation is typeset once by the
[toolchain](../toolchain/README.md) and embedded as an image, so it renders identically on GitHub,
GitLab, and any offline viewer, with no dependency on a markdown math plugin.

## Where this came from

These notes grew out of a long worked conversation and twelve pages of handwritten study
notes on buck/boost converters. The confusions flagged in those notes — *when* it is legal to
apply `d/dt` to both sides of an equation, why a constant voltage gives a straight-line
current, what the ramp graphs actually look like — are addressed head-on in the relevant
sections rather than glossed over.
