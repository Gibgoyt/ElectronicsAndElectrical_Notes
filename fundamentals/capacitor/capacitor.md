# The capacitor — current is proportional to the rate of change of voltage

The capacitor is the exact mirror of the inductor. Its law comes from one line of algebra and one
line of calculus — but *that* calculus step is the one this whole subject trips on. So this
document slows all the way down and proves, properly, **why you are allowed to differentiate
![Q = C times V](capacitor.assets/eq-inline/205c11f7c4.svg)<!--m:Q = C \cdot V-->**, and what rule makes it legal.

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
> The current into a capacitor is proportional to how fast its voltage is changing — the
> inductor's law with voltage and current swapped:

![I_C equals C times dV_C by dt](capacitor.assets/eq-law.svg)

---

## 1 What a capacitor actually is

A capacitor is **two conducting plates separated by a gap** (an insulator, the dielectric). Push
charge onto one plate and it repels an equal charge off the facing plate, so one plate ends up
![+Q](capacitor.assets/eq-inline/de10a3e4cf.svg)<!--m:+Q--> and the other ![-Q](capacitor.assets/eq-inline/6422eedc12.svg)<!--m:-Q-->. That separated charge sets up an electric field across the gap, and *that
field* is where the capacitor keeps its energy. How much energy is worked out at the end of this
section, once the charge–voltage law it rests on is on the table.

The defining relationship is simply that the stored charge is proportional to the voltage across
the plates, with the capacitance ![C](capacitor.assets/eq-inline/32096c2e0e.svg)<!--m:C--> as the constant of proportionality:

![Q equals C V_C](capacitor.assets/eq-charge.svg)

- **![Q](capacitor.assets/eq-inline/c3156e00d3.svg)<!--m:Q-->** — the charge separated onto the plates, in coulombs (C).
- **![V_C](capacitor.assets/eq-inline/b1fec46ec0.svg)<!--m:V_C-->** — the voltage across the plates, in volts (V).
- **![C](capacitor.assets/eq-inline/32096c2e0e.svg)<!--m:C-->** — the capacitance, in farads (F): how much charge it stores per volt.

**The stored energy, derived.** A voltage is energy per unit charge: one volt means one joule of
work to carry one coulomb from the ![-](capacitor.assets/eq-inline/3bc15c8aae.svg)<!--m:---> plate to the ![+](capacitor.assets/eq-inline/a979ef10cc.svg)<!--m:+--> plate (![1 V = 1 J/C](capacitor.assets/eq-inline/9ad7202dce.svg)<!--m:1\,\mathrm{V} = 1\,\mathrm{J/C}-->).
So charge the capacitor a little at a time and add up the work. When it already holds a charge ![q](capacitor.assets/eq-inline/22ea1c649c.svg)<!--m:q-->,
its voltage is ![v = q/C](capacitor.assets/eq-inline/86b089ff35.svg)<!--m:v = q/C-->, and carrying the next small charge ![dq](capacitor.assets/eq-inline/3bf4ab708f.svg)<!--m:dq--> across costs ![dW = v dq = (q/C) dq](capacitor.assets/eq-inline/e896fd3aec.svg)<!--m:dW = v\,dq = (q/C)\,dq-->.
Sum every such step from empty (![q = 0](capacitor.assets/eq-inline/3b2d28909c.svg)<!--m:q = 0-->) to full (![q = Q](capacitor.assets/eq-inline/b2450387e2.svg)<!--m:q = Q-->), then use ![Q = C V_C](capacitor.assets/eq-inline/bf31347312.svg)<!--m:Q = C\,V_C-->:

![E equals the integral from 0 to Q of q over C dq, which equals Q squared over 2 C, which equals one half C V_C squared](capacitor.assets/eq-energy.svg)

- **![E](capacitor.assets/eq-inline/e0184adedf.svg)<!--m:E-->** — the stored energy, in joules (J). Check the units: farads times volts squared is
  ![( C/V) times V^2 = C times V = J](capacitor.assets/eq-inline/d8ac14abe9.svg)<!--m:(\mathrm{C/V})\cdot\mathrm{V}^2 = \mathrm{C}\cdot\mathrm{V} = \mathrm{J}-->.

The factor of one half is not decoration. The first charge was carried across almost no voltage
and only the last charge across the full ![V_C](capacitor.assets/eq-inline/b1fec46ec0.svg)<!--m:V_C-->, so on average each coulomb was carried across
![V_C/2](capacitor.assets/eq-inline/e4e51aaed1.svg)<!--m:V_C/2-->.

## 2 The rule you need — when you may differentiate an equation

Here is the exact step that feels illegal, and the honest worry behind it: *"we can't just
differentiate — and we definitely can't multiply an equation by ![d/dt](capacitor.assets/eq-inline/9560a2e5f1.svg)<!--m:d/dt-->. So when are we actually
allowed to do this?"* That worry is correct, and worth answering precisely, because the answer is
the tool the rest of the subject runs on.

**The rule.** If two quantities are *equal as functions of time* — meaning they are the same
number at every instant ![t](capacitor.assets/eq-inline/8efd86fb78.svg)<!--m:t--> — then their rates of change are also equal at every instant. In
symbols: if ![f(t) = g(t)](capacitor.assets/eq-inline/9ea1eebbbd.svg)<!--m:f(t) = g(t)--> for all ![t](capacitor.assets/eq-inline/8efd86fb78.svg)<!--m:t-->, then ![df/dt = dg/dt](capacitor.assets/eq-inline/d4d7f60dc5.svg)<!--m:df/dt = dg/dt-->. You are **not** "multiplying the
equation by an operator." You are applying *the same function* — the derivative — to *two
expressions that are already the same function*, so the results are still the same function.

Two moves, so the distinction is unmistakable:

- **Legal — differentiate an identity.** ![Q(t) = C times V(t)](capacitor.assets/eq-inline/69b2d90d54.svg)<!--m:Q(t) = C \cdot V(t)--> holds at every instant, so applying
  ![d/dt](capacitor.assets/eq-inline/9560a2e5f1.svg)<!--m:d/dt--> to both sides is the same operation done to two things that were already equal.
- **Not a thing — "multiply the equation by ![d/dt](capacitor.assets/eq-inline/9560a2e5f1.svg)<!--m:d/dt-->".** ![d/dt](capacitor.assets/eq-inline/9560a2e5f1.svg)<!--m:d/dt--> is an operator, not a number; you do
  not "multiply both sides" by it the way you'd multiply by 3. "Differentiate both sides" is
  shorthand for the *legal* move above, and it only works because the two sides are equal as
  functions.

Carry out the legal move on ![Q = C times V](capacitor.assets/eq-inline/205c11f7c4.svg)<!--m:Q = C \cdot V-->. The capacitance ![C](capacitor.assets/eq-inline/32096c2e0e.svg)<!--m:C--> is a constant, so it comes out front of
the derivative:

![differentiate both sides of Q equals C V_C to get dQ by dt equals C dV_C by dt](capacitor.assets/eq-differentiate.svg)

Now bring in the **second** fact — the definition of current. Current *is* the rate at which
charge moves:

![I equals dQ by dt](capacitor.assets/eq-current-def.svg)

This is the piece that felt "missing": the charge–voltage relationship relates charge to voltage;
the definition of current relates charge to current. Substitute the second into the differentiated
first — replace ![dQ/dt](capacitor.assets/eq-inline/d399529a90.svg)<!--m:dQ/dt--> with ![I](capacitor.assets/eq-inline/ca73ab6556.svg)<!--m:I--> — and the capacitor law falls out:

![I_C equals C times dV_C by dt, boxed](capacitor.assets/eq-law.svg)

> **Tip —** The whole trick is: **an equation between two functions of ![t](capacitor.assets/eq-inline/8efd86fb78.svg)<!--m:t--> stays true if you
> differentiate both sides**, because you are doing the identical thing to two things that are
> already identical. That is the licence to go from ![Q = C times V](capacitor.assets/eq-inline/205c11f7c4.svg)<!--m:Q = C \cdot V--> to ![I_C = C times dV/dt](capacitor.assets/eq-inline/56baca3b41.svg)<!--m:I_C = C \cdot dV/dt--> — and, read the
> other way (integration is the inverse), the licence to go from a constant current back to a
> voltage ramp in §4.

> **Note —** This is the same shape as the inductor, where we integrated the constant-voltage
> law to get a current ramp ([../inductor/inductor.md §4](../inductor/inductor.md#4-from-the-law-to-the-ramp--the-integral-done-slowly)).
> Differentiation and integration are inverses, so "differentiate ![Q = C times V](capacitor.assets/eq-inline/205c11f7c4.svg)<!--m:Q = C \cdot V-->" and "integrate
> ![V_L = L times dI/dt](capacitor.assets/eq-inline/4e5d46c427.svg)<!--m:V_L = L \cdot dI/dt-->" are the same manoeuvre run in opposite directions.

## 3 The units of the farad

From ![Q = C times V](capacitor.assets/eq-inline/205c11f7c4.svg)<!--m:Q = C \cdot V-->, a farad is a coulomb per volt. And since current is charge per time, a coulomb is
an ampere-second (![1 C = 1 A times s](capacitor.assets/eq-inline/ac720afbd6.svg)<!--m:1 C = 1 A \cdot s-->):

![one farad equals one coulomb per volt equals ampere second per volt](capacitor.assets/eq-units-farad.svg)

Check it against the law ![I_C = C times dV/dt](capacitor.assets/eq-inline/56baca3b41.svg)<!--m:I_C = C \cdot dV/dt--> — volts and seconds cancel, leaving amps:

![A s over V times V over s equals A](capacitor.assets/eq-units-check.svg)

## 4 From the law to the ramp

Run the mirror of the inductor argument. Suppose the **current is held constant** (exactly what a
converter's inductor forces into the capacitor for part of each cycle). Then ![dV_C/dt = I_C/C](capacitor.assets/eq-inline/62bb5338e4.svg)<!--m:dV_C/dt = I_C/C--> is a
constant, so the voltage climbs in a straight line from wherever it started:

![V_C of t minus V_C of 0 equals I_C over C times t](capacitor.assets/eq-ramp.svg)

![Constant current into a capacitor produces a linear voltage ramp](capacitor.assets/fig-01.svg)

_The flat cause (![I_C](capacitor.assets/eq-inline/d697a02e80.svg)<!--m:I_C--> on top) sets the slope of the straight-line effect (![V_C](capacitor.assets/eq-inline/b1fec46ec0.svg)<!--m:V_C--> below) — the exact
mirror of the inductor's ![I](capacitor.assets/eq-inline/ca73ab6556.svg)<!--m:I-->-vs-![t](capacitor.assets/eq-inline/8efd86fb78.svg)<!--m:t--> ramp. Swap ![V I](capacitor.assets/eq-inline/4ebcd6796a.svg)<!--m:V \leftrightarrow I--> and ![L C](capacitor.assets/eq-inline/6cef383173.svg)<!--m:L \leftrightarrow C--> and it is the same picture._

## 5 At the poles

This is where a capacitor is genuinely different from an inductor. **No charge ever crosses the
gap** between the plates. Current *appears* to flow "through" the capacitor only because charge
arrives on the near plate and an equal charge is pushed off the far plate at the same instant.

![Charge accumulates on capacitor plates while no charge crosses the dielectric gap](capacitor.assets/fig-02.svg)

_Charge piles up (![V_C](capacitor.assets/eq-inline/b1fec46ec0.svg)<!--m:V_C--> grows) rather than passing through — which is why ![I_C](capacitor.assets/eq-inline/d697a02e80.svg)<!--m:I_C--> can flow while the
voltage is changing, yet a fully-charged capacitor blocks DC entirely (once ![dV/dt = 0](capacitor.assets/eq-inline/9ba4f1161e.svg)<!--m:dV/dt = 0-->, ![I_C = 0](capacitor.assets/eq-inline/93293972a2.svg)<!--m:I_C = 0-->).
Contrast the inductor, where current flows straight through the wire the whole time
([../inductor/inductor.md §6](../inductor/inductor.md#6-at-the-poles))._

> **Note —** "Blocks DC, passes AC" is just this law restated. At DC the voltage is steady, so
> ![dV/dt = 0](capacitor.assets/eq-inline/9ba4f1161e.svg)<!--m:dV/dt = 0--> and ![I_C = 0](capacitor.assets/eq-inline/93293972a2.svg)<!--m:I_C = 0--> — an open circuit. The faster the voltage changes (for a repeating waveform: the more
> cycles per second it makes, that is, the higher its frequency), the more current flows for the same capacitance.

## 6 The duality — one table, read both ways

Everything above is the inductor's story with two swaps: ![V I](capacitor.assets/eq-inline/4ebcd6796a.svg)<!--m:V \leftrightarrow I--> and ![L C](capacitor.assets/eq-inline/6cef383173.svg)<!--m:L \leftrightarrow C-->. Learn one law and
you have the other for free.

![Duality table comparing the inductor and capacitor defining laws term by term](capacitor.assets/fig-03.svg)

_Same rows, two columns: the inductor holds back changes in current; the capacitor holds back
changes in voltage. This mirror is why the buck's inductor-sizing maths and the boost's
capacitor-sizing maths look so alike._

## 7 Sources and cross-links

- **Mirror law:** [../inductor/inductor.md](../inductor/inductor.md) — ![V_L = L times dI/dt](capacitor.assets/eq-inline/4e5d46c427.svg)<!--m:V_L = L \cdot dI/dt-->, the same
  equation before the ![V I](capacitor.assets/eq-inline/4ebcd6796a.svg)<!--m:V \leftrightarrow I-->, ![L C](capacitor.assets/eq-inline/6cef383173.svg)<!--m:L \leftrightarrow C--> swap.
- **Where the ramp is used:** the output-capacitor sizing in
  [../../dc-dc-converters/buck/buck.md §5](../../dc-dc-converters/buck/buck.md#5-sizing-the-output-capacitor)
  and [../../dc-dc-converters/boost/boost.md §5](../../dc-dc-converters/boost/boost.md#5-sizing-the-output-capacitor).
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
