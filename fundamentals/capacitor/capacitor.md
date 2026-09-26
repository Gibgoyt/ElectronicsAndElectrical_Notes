# The capacitor — current is proportional to the rate of change of voltage

The capacitor is the exact mirror of the inductor. Its law, `I_C = C·dV/dt`, comes from one
line of algebra and one line of calculus — but *that* calculus step is the one this whole
subject trips on. So this document slows all the way down and proves, properly, **why you are
allowed to differentiate `Q = C·V`**, and what rule makes it legal.

**Contents**

1. [What a capacitor actually is](#1-what-a-capacitor-actually-is)
2. [The rule you need — when you may differentiate an equation](#2-the-rule-you-need--when-you-may-differentiate-an-equation)
3. [The units of the farad](#3-the-units-of-the-farad)
4. [From the law to the ramp](#4-from-the-law-to-the-ramp)
5. [At the poles](#5-at-the-poles)
6. [The duality — one table, read both ways](#6-the-duality--one-table-read-both-ways)
7. [Sources and cross-links](#7-sources-and-cross-links)

> **The thesis in one line**
>
> `I_C = C·dV/dt`: the current into a capacitor is proportional to how fast its voltage is
> changing — the inductor's law with voltage and current swapped.

---

## 1 What a capacitor actually is

A capacitor is **two conducting plates separated by a gap** (an insulator, the dielectric).
Push charge onto one plate and it repels an equal charge off the facing plate, so one plate ends
up `+Q` and the other `−Q`. That separated charge sets up an electric field across the gap, and
*that field* is the stored energy:

$$E = \tfrac{1}{2}\,C\,V_C^2$$

The defining relationship is simply that the stored charge is proportional to the voltage across
the plates, with the capacitance `C` as the constant of proportionality:

$$Q = C\,V_C$$

- **$Q$** — the charge separated onto the plates, in coulombs (C).
- **$V_C$** — the voltage across the plates, in volts (V).
- **$C$** — the capacitance, in farads (F): how much charge it stores per volt.

## 2 The rule you need — when you may differentiate an equation

Here is the exact step that feels illegal, and the honest worry behind it: *"we can't just
differentiate — and we definitely can't multiply an equation by `d/dt`. So when are we actually
allowed to do this?"* That worry is correct, and worth answering precisely, because the answer is
the tool the rest of the subject runs on.

**The rule.** If two quantities are *equal as functions of time* — meaning they are the same
number at every instant `t` — then their rates of change are also equal at every instant. In
symbols: if `f(t) = g(t)` for all `t`, then `df/dt = dg/dt`. You are **not** "multiplying the
equation by an operator." You are applying *the same function* — the derivative — to *two
expressions that are already the same function*, so the results are still the same function.

Contrast the two moves so the distinction is unmistakable:

```text
LEGAL     — differentiate an identity:
            Q(t) = C·V(t)  holds at every instant t,
            so  d/dt[ Q(t) ]  =  d/dt[ C·V(t) ]         same operation, both sides
                                                        (both sides were the same function)

NOT A THING — "multiply the equation by d/dt":
            d/dt is an operator, not a number. You do not
            "multiply both sides" by it the way you'd multiply by 3.
            The legal move above is what people *mean* when they say
            "differentiate both sides", and it only works because
            the two sides are equal as functions.
```

So carry out the legal move on `Q = C·V`. The capacitance `C` is a constant, so it comes out
front of the derivative:

```text
Q(t)        = C · V_C(t)                     Eq. 1 — the defining relationship (an identity in t)
d/dt[Q(t)]  = d/dt[ C · V_C(t) ]             differentiate both sides (rule above)
dQ/dt       = C · dV_C/dt                    C constant → pulls out of d/dt
```

Now bring in the **second** fact — the definition of current. Current *is* the rate at which
charge moves:

```text
I = dQ/dt                                    Eq. 2 — definition of current
```

This is the piece that felt "missing": Eq. 1 relates charge to voltage; Eq. 2 relates charge to
current. Substitute Eq. 2 into the differentiated Eq. 1 — replace `dQ/dt` with `I` — and the
capacitor law falls out:

```text
dQ/dt  = C · dV_C/dt                         from Eq. 1, differentiated
  I    = dQ/dt                               Eq. 2
  ⇒  I_C = C · dV_C/dt                        substitute: the capacitor's defining law
```

$$\boxed{\,I_C(t) = C\,\frac{dV_C}{dt}\,}$$

> **💡 Tip —** The whole trick is: **an equation between two functions of `t` stays true if you
> differentiate both sides**, because you are doing the identical thing to two things that are
> already identical. That is the licence to go from `Q = C·V` to `I_C = C·dV/dt` — and, read the
> other way (integration is the inverse), the licence to go from a constant current back to a
> voltage ramp in §4.

> **📝 Note —** This is the same shape as the inductor, where we integrated the constant-voltage
> law to get a current ramp ([../inductor/inductor.md §3](../inductor/inductor.md#3-from-the-law-to-the-ramp--the-integral-done-slowly)).
> Differentiation and integration are inverses, so "differentiate `Q = C·V`" and "integrate
> `V_L = L·dI/dt`" are the same manoeuvre run in opposite directions.

## 3 The units of the farad

From `Q = C·V`, a farad is a coulomb per volt. And since current is charge per time (`I = dQ/dt`),
a coulomb is an ampere-second (`1 C = 1 A·s`). So:

```text
[C] = [Q] / [V]     = coulomb / volt        1 F = 1 C/V
[Q] = [I] · [t]     = ampere · second       1 C = 1 A·s
  ⇒  1 F = (A·s) / V
```

Check it against the law `I_C = C·dV/dt`:

```text
(A·s / V) × (V / s) = A                      volts and seconds cancel → amps. Consistent.
```

## 4 From the law to the ramp

Run the mirror of the inductor argument. Suppose the **current is held constant** (exactly what
a converter's inductor forces into the capacitor for part of each cycle). Then `dV_C/dt = I_C/C`
is a constant, so the voltage climbs in a straight line:

```text
dV_C/dt          = I_C / C                   rearrange the law; RHS is constant
∫₀ᵗ (dV_C/dt') dt' = ∫₀ᵗ (I_C / C) dt'         integrate both sides over 0..t
V_C(t) − V_C(0)  = (I_C / C) · t             FTC on the left; I_C, C pull out on the right
```

So a constant current forces the capacitor voltage into a **straight ramp**, starting from
whatever it already was, `V_C(0)`:

![Constant current into a capacitor produces a linear voltage ramp](capacitor.assets/fig-01.svg)

_The flat cause (I_C on top) sets the slope of the straight-line effect (V_C below) — the exact
mirror of the inductor's I-vs-t ramp. Swap `V ↔ I` and `L ↔ C` and it is the same picture._

## 5 At the poles

This is where a capacitor is genuinely different from an inductor. **No charge ever crosses the
gap** between the plates. Current *appears* to flow "through" the capacitor only because charge
arrives on the near plate and an equal charge is pushed off the far plate at the same instant.

![Charge accumulates on capacitor plates while no charge crosses the dielectric gap](capacitor.assets/fig-02.svg)

_Charge piles up (V_C grows) rather than passing through — which is why `I_C` can flow while the
voltage is changing, yet a fully-charged capacitor blocks DC entirely (once `dV/dt = 0`, `I_C = 0`).
Contrast the inductor, where current flows straight through the wire the whole time
([../inductor/inductor.md §5](../inductor/inductor.md#5-at-the-poles))._

> **📝 Note —** "Blocks DC, passes AC" is just this law restated. At DC the voltage is steady, so
> `dV/dt = 0` and `I_C = 0` — an open circuit. The faster the voltage changes (higher frequency),
> the more current flows for the same capacitance.

## 6 The duality — one table, read both ways

Everything above is the inductor's story with two swaps: `V ↔ I` and `L ↔ C`. Learn one law and
you have the other for free.

![Duality table comparing the inductor and capacitor defining laws term by term](capacitor.assets/fig-03.svg)

_Same rows, two columns: the inductor holds back changes in current; the capacitor holds back
changes in voltage. This mirror is why the buck's inductor-sizing maths and the boost's
capacitor-sizing maths look so alike._

## 7 Sources and cross-links

- **Mirror law:** [../inductor/inductor.md](../inductor/inductor.md) — `V_L = L·dI/dt`, the same
  equation before the `V ↔ I`, `L ↔ C` swap.
- **Where the ramp is used:** the output-capacitor sizing in
  [../../dc-dc-converters/buck/buck.md §5](../../dc-dc-converters/buck/buck.md#5-sizing-the-output-capacitor)
  and [../../dc-dc-converters/boost/boost.md §5](../../dc-dc-converters/boost/boost.md#5-sizing-the-output-capacitor),
  both of which are just `I_C = C·dV/dt` applied over one switching interval.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
