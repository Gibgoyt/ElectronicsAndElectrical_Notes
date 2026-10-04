# The buck converter — where V_out = D·V_in actually comes from

A buck converter steps a DC voltage *down* — 12 V in, 3 V out — by switching a transistor on and
off fast and letting an inductor and capacitor average the result. The famous formula
<!--m:V_{out} = D \cdot V_{in}-->![V_out = D times V_in](buck.assets/eq-inline/645cc1e7b2.svg)<!--/m--> is not an assumption; it is forced by one steady-state fact about the inductor.
This document derives it, then sizes the parts.

**Contents**

1. [The circuit, and what ON/OFF means](#1-the-circuit-and-what-onoff-means)
2. [The two intervals](#2-the-two-intervals)
3. [Volt-second balance — the step-down ratio](#3-volt-second-balance--the-step-down-ratio)
4. [Sizing the inductor](#4-sizing-the-inductor)
5. [Sizing the output capacitor](#5-sizing-the-output-capacitor)
6. [Worked numbers — 12 V to 3 V](#6-worked-numbers--12-v-to-3-v)
7. [What this costs you](#7-what-this-costs-you)
8. [Sources and cross-links](#8-sources-and-cross-links)

> **The thesis in one line**
>
> In steady state the inductor's average voltage over a full cycle is zero, so the volt-seconds it
> gains while the switch is ON must exactly cancel those it loses while OFF — and that cancellation
> *is* <!--m:V_{out} = D \cdot V_{in}-->![V_out = D times V_in](buck.assets/eq-inline/645cc1e7b2.svg)<!--/m-->.

---

## 1 The circuit, and what ON/OFF means

![Buck converter schematic with switch, freewheel diode, inductor, output capacitor and load](buck.assets/fig-01.svg)

_Switch, then inductor, along the top; a freewheel diode from the switch node down to ground; the
capacitor across the output. When the switch opens, the diode gives the inductor's current a path
so it never breaks._

First, kill an ambiguity that trips everyone up:

- **Switch ON = closed = conducting** (current is allowed through, like a closed gate).
- **Switch OFF = open** (that path is blocked, like an open gate).

The switch **S1** is driven by a PWM signal at a fixed period <!--m:T-->![T](buck.assets/eq-inline/c2c53d6694.svg)<!--/m-->. The fraction of each period it
spends ON is the **duty cycle <!--m:D-->![D](buck.assets/eq-inline/50c9e8d5fc.svg)<!--/m-->** (so ON lasts <!--m:D \cdot T-->![D times T](buck.assets/eq-inline/94b24dfcfd.svg)<!--/m-->, OFF lasts <!--m:(1-D) \cdot T-->![(1-D) times T](buck.assets/eq-inline/9ee718adde.svg)<!--/m-->). The **switch node** is
the point right after the switch, where the diode also connects — it is chopped between <!--m:V_{in}-->![V_in](buck.assets/eq-inline/29f560cdfe.svg)<!--/m-->
(switch ON) and roughly 0 V (switch OFF, diode conducting).

> **Note —** If you literally probed the 12 V *supply's own terminals* you'd see a flat 12 V — an
> ideal source doesn't care what's downstream. The square wave lives at the **switch node**, not at
> the supply. This is a common labelling slip.

## 2 The two intervals

Because the inductor obeys <!--m:V_L = L \cdot dI/dt-->![V_L = L times dI/dt](buck.assets/eq-inline/4e5d46c427.svg)<!--/m-->, a *constant* voltage across it makes its current a
*straight ramp* (see [../../fundamentals/inductor/inductor.md §4](../../fundamentals/inductor/inductor.md#4-from-the-law-to-the-ramp--the-integral-done-slowly)).
The buck gives the inductor two constant-voltage intervals per cycle.

**Interval 1 — switch ON, lasting <!--m:D \cdot T-->![D times T](buck.assets/eq-inline/94b24dfcfd.svg)<!--/m-->.** Current flows V_in → switch → L → output. The diode is
reverse-biased (off). The inductor sees the difference between what pushes on its left end (<!--m:V_{in}-->![V_in](buck.assets/eq-inline/29f560cdfe.svg)<!--/m-->)
and what pushes back on its right (<!--m:V_{out}-->![V_out](buck.assets/eq-inline/05b247c888.svg)<!--/m-->):

![V_L equals V_in minus V_out, constant and positive](buck.assets/eq-vl-on.svg)

so the current ramps **up** by:

![delta I_L rise equals V_in minus V_out times D T over L](buck.assets/eq-rise.svg)

**Interval 2 — switch OFF, lasting <!--m:(1-D) \cdot T-->![(1-D) times T](buck.assets/eq-inline/9ee718adde.svg)<!--/m-->.** The switch path is gone, so the inductor forces the
diode to conduct, pinning its left end near ground. Now:

![V_L equals minus V_out, constant and negative](buck.assets/eq-vl-off.svg)

so the current ramps **down** by:

![delta I_L fall equals V_out times one minus D times T over L](buck.assets/eq-fall.svg)

![Buck switch-node square wave above and the resulting triangular inductor current](buck.assets/fig-02.svg)

_The flat-topped square voltage at the switch node makes the inductor current a straight-sided
triangle: up while ON, down while OFF. Each side is one constant-<!--m:V_L-->![V_L](buck.assets/eq-inline/136d4e3fb2.svg)<!--/m--> ramp from the inductor law._

## 3 Volt-second balance — the step-down ratio

Now the key idea. In **steady state** the current waveform repeats identically every cycle — the
triangle starts each period exactly where it started the last one. For that to be true, whatever
the current *rose* by in Interval 1 must be *exactly* undone by what it *fell* by in Interval 2. If
it weren't, the current would creep up (or down) a little every cycle, forever. So <!--m:\text{rise} = \text{fall}-->![rise = fall](buck.assets/eq-inline/7fcffce6ac.svg)<!--/m-->;
cancelling <!--m:T-->![T](buck.assets/eq-inline/c2c53d6694.svg)<!--/m--> and <!--m:L-->![L](buck.assets/eq-inline/d160e0986a.svg)<!--/m--> from both sides:

![V_in minus V_out times D equals V_out times one minus D](buck.assets/eq-balance.svg)

Expand and simplify — the <!--m:V_{out} \cdot D-->![V_out times D](buck.assets/eq-inline/ee7ea248da.svg)<!--/m--> terms cancel:

![V_in D minus V_out D equals V_out minus V_out D, so V_in D equals V_out](buck.assets/eq-balance-solve.svg)

![V_out equals D V_in, boxed](buck.assets/eq-result.svg)

That is the entire origin of the buck's step-down ratio — *derived*, not assumed. Equivalently, the
average inductor voltage over a cycle is zero (**volt-second balance**): the positive volt-seconds
cancel the negative ones.

> **Tip —** Volt-second balance is the master key for *every* converter in this tree. Write <!--m:V_L-->![V_L](buck.assets/eq-inline/136d4e3fb2.svg)<!--/m-->
> for each interval, set the average over one period to zero, and solve. The boost uses the
> identical move — see [../boost/boost.md §3](../boost/boost.md#3-volt-second-balance--the-step-up-ratio).

## 4 Sizing the inductor

The rise/fall equations already contain <!--m:L-->![L](buck.assets/eq-inline/d160e0986a.svg)<!--/m-->, so pick a target ripple <!--m:\Delta I_L-->![Delta I_L](buck.assets/eq-inline/c856ab20fc.svg)<!--/m--> and solve. A common
rule of thumb is 20–40 % of the average (load) current. Using the ON-interval rise with <!--m:T = 1/f_{sw}-->![T = 1/f_sw](buck.assets/eq-inline/2a8cbfbb37.svg)<!--/m-->:

![L equals V_in minus V_out times D T over delta I_L equals V_in minus V_out times D over f_sw delta I_L](buck.assets/eq-inductor-sizing.svg)

Bigger <!--m:L-->![L](buck.assets/eq-inline/d160e0986a.svg)<!--/m--> → smaller ripple, but a physically bigger, costlier part with more resistance. That is
the trade-off <!--m:\Delta I_L-->![Delta I_L](buck.assets/eq-inline/c856ab20fc.svg)<!--/m--> sets.

## 5 Sizing the output capacitor

The inductor current is a triangle averaging to the load current <!--m:I_{load}-->![I_load](buck.assets/eq-inline/7902e72f89.svg)<!--/m-->. The load takes that
average; the *leftover* — <!--m:i_C = i_L - I_{load}-->![i_C = i_L - I_load](buck.assets/eq-inline/c804854714.svg)<!--/m--> — is what actually flows in and out of the capacitor.
It is a triangle centred on zero, swinging <!--m:\pm \Delta I_L/2-->![plus-minus Delta I_L/2](buck.assets/eq-inline/975bfe81a9.svg)<!--/m-->.

![Capacitor current as the difference between triangular inductor current and constant load current](buck.assets/fig-03.svg)

_The capacitor current is the inductor triangle minus the flat load line: a triangle centred on
zero. The shaded positive half is the charge <!--m:\Delta Q-->![Delta Q](buck.assets/eq-inline/9fd1467576.svg)<!--/m--> piled onto the cap; that charge over <!--m:C-->![C](buck.assets/eq-inline/32096c2e0e.svg)<!--/m--> is the
output voltage ripple._

The charge added during the positive (charging) half-cycle is the **area of that little triangle** —
base <!--m:T/2-->![T/2](buck.assets/eq-inline/12bfe0f94c.svg)<!--/m-->, height <!--m:\Delta I_L/2-->![Delta I_L/2](buck.assets/eq-inline/d2ae25f58e.svg)<!--/m-->:

![delta Q equals one half times T over 2 times delta I_L over 2 equals T delta I_L over 8](buck.assets/eq-charge-triangle.svg)

That charge on a capacitance <!--m:C-->![C](buck.assets/eq-inline/32096c2e0e.svg)<!--/m--> is a voltage ripple <!--m:\Delta V_{out} = \Delta Q/C-->![Delta V_out = Delta Q/C](buck.assets/eq-inline/47c897d053.svg)<!--/m-->, so:

![delta V_out equals delta Q over C, so C equals delta I_L over 8 f_sw delta V_out](buck.assets/eq-cap-sizing.svg)

## 6 Worked numbers — 12 V to 3 V

Take <!--m:f_{sw} = 100 \,\mathrm{kHz}-->![f_sw = 100 kHz](buck.assets/eq-inline/b2d8a51533.svg)<!--/m--> (so <!--m:T = 10 \mu s-->![T = 10 mu s](buck.assets/eq-inline/7872cc0fa0.svg)<!--/m-->), a 1 A load, a target 20 % current ripple and 1 % output
voltage ripple. With <!--m:V_{in} = 12 V-->![V_in = 12 V](buck.assets/eq-inline/1c9e4d0896.svg)<!--/m-->, <!--m:V_{out} = 3 V-->![V_out = 3 V](buck.assets/eq-inline/eee2a17355.svg)<!--/m-->, the duty cycle is <!--m:D = V_{out}/V_{in} = 0.25-->![D = V_out/V_in = 0.25](buck.assets/eq-inline/2dd02985de.svg)<!--/m-->, and
<!--m:\Delta I_L = 0.2 A-->![Delta I_L = 0.2 A](buck.assets/eq-inline/c1d7cfca6b.svg)<!--/m-->, <!--m:\Delta V_{out} = 0.03 V-->![Delta V_out = 0.03 V](buck.assets/eq-inline/73c6a248f6.svg)<!--/m-->:

![L equals 12 minus 3 times 0.25 over 100000 times 0.2 equals 112.5 microhenry](buck.assets/eq-worked-L.svg)

![C equals 0.2 over 8 times 100000 times 0.03 equals about 8.3 microfarad](buck.assets/eq-worked-C.svg)

> **Note —** Sanity-check the duty cycle against the result: <!--m:V_{out} = D \cdot V_{in} = 0.25 \times 12 = 3 V-->![V_out = D times V_in = 0.25 times 12 = 3 V](buck.assets/eq-inline/a9d7c5c64b.svg)<!--/m-->.
> Correct. And the ripple assumptions are self-consistent — <!--m:\Delta I_L = 0.2 A-->![Delta I_L = 0.2 A](buck.assets/eq-inline/c1d7cfca6b.svg)<!--/m--> on a 1 A load is 20 %, exactly
> what we targeted.

## 7 What this costs you

- **Ripple vs. size.** Smaller ripple means a bigger <!--m:L-->![L](buck.assets/eq-inline/d160e0986a.svg)<!--/m--> and bigger <!--m:C-->![C](buck.assets/eq-inline/32096c2e0e.svg)<!--/m--> — more board area and cost.
  The 20–40 % rule is a compromise, not a law.
- **The diode drop.** A real freewheel diode drops ~0.3–0.7 V while conducting, so <!--m:V_{out}-->![V_out](buck.assets/eq-inline/05b247c888.svg)<!--/m--> is
  slightly below <!--m:D \cdot V_{in}-->![D times V_in](buck.assets/eq-inline/a25bedb66e.svg)<!--/m--> and some power is lost as heat. A *synchronous* buck replaces the diode
  with a second MOSFET to cut that loss — at the cost of gate-drive complexity.
- **Ideal-parts assumption.** These formulas assume a pure <!--m:L-->![L](buck.assets/eq-inline/d160e0986a.svg)<!--/m--> and pure <!--m:C-->![C](buck.assets/eq-inline/32096c2e0e.svg)<!--/m-->. Real parts have
  resistance (the capacitor's ESR adds its own ripple term, often dominant) and frequency-dependent
  losses. Treat the results as a starting point.

## 8 Sources and cross-links

- The inductor ramp this is built on:
  [../../fundamentals/inductor/inductor.md §4](../../fundamentals/inductor/inductor.md#4-from-the-law-to-the-ramp--the-integral-done-slowly).
- The capacitor-charge argument for §5:
  [../../fundamentals/capacitor/capacitor.md §4](../../fundamentals/capacitor/capacitor.md#4-from-the-law-to-the-ramp).
- Mirror circuit, same method: [../boost/boost.md](../boost/boost.md).
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
