# The boost converter — where V_out = V_in/(1−D) comes from

A boost converter steps a DC voltage *up* — 12 V in, 24 V out — using the very same four parts as
the [buck](../buck/buck.md), just reordered: the inductor comes first, and the switch shorts the
switch node to ground. The step-up ratio is derived by the same volt-second balance; only the two
intervals swap roles.

**Contents**

1. [The circuit](#1-the-circuit)
2. [The two intervals](#2-the-two-intervals)
3. [Volt-second balance — the step-up ratio](#3-volt-second-balance--the-step-up-ratio)
4. [Sizing the inductor](#4-sizing-the-inductor)
5. [Sizing the output capacitor](#5-sizing-the-output-capacitor)
6. [Worked numbers — 12 V to 24 V](#6-worked-numbers--12-v-to-24-v)
7. [What this costs you](#7-what-this-costs-you)
8. [Sources and cross-links](#8-sources-and-cross-links)

> **The thesis in one line**
>
> When the switch opens, the inductor's voltage flips sign and adds *on top of* ![V_in](boost.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> — that
> reversal, forced by volt-second balance, is what pushes the output above the input.

---

## 1 The circuit

![Boost converter schematic with inductor first, switch to ground, diode to output capacitor and load](boost.assets/fig-01.svg)

_Inductor first, then the switch node; the switch **S1** shorts that node to ground, and a diode
carries current on to the output. Same four parts as the buck, wired in a different order._

Same vocabulary as the buck: **ON = closed**, ![D](boost.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> is the fraction of each period ![T](boost.assets/eq-inline/c2c53d6694.svg)<!--m:T--> the switch is
ON, ![f_sw = 1/T](boost.assets/eq-inline/cc2c6208db.svg)<!--m:f_{sw} = 1/T-->. The one new idea you need is the inductor's **polarity flip**: when its current is
forced to fall, ![V_L](boost.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L--> changes sign and the inductor behaves like a battery in series with the source
(see [../../fundamentals/inductor/inductor.md §5](../../fundamentals/inductor/inductor.md#5-polarity-lenz-and-the-sign-flip)).

## 2 The two intervals

**Interval 1 — switch ON, lasting ![D times T](boost.assets/eq-inline/94b24dfcfd.svg)<!--m:D \cdot T-->.** S1 shorts the switch node to ground, so the inductor sits
straight across the input and the diode is reverse-biased (off): its anode, on the switch node, is
at 0 V while its cathode sits at the output voltage the capacitor holds up (the ideal-diode rule of
[../buck/buck.md §1](../buck/buck.md#1-the-circuit-and-what-onoff-means)). By Kirchhoff's voltage
law the inductor sees the full input:

![V_L equals V_in, constant and positive](boost.assets/eq-vl-on.svg)

so its current ramps **up**, storing energy:

![delta I_L rise equals V_in times D T over L](boost.assets/eq-rise.svg)

**Interval 2 — switch OFF, lasting ![(1-D) times T](boost.assets/eq-inline/9ee718adde.svg)<!--m:(1-D) \cdot T-->.** The switch path vanishes; the inductor's current
can't stop, so it forces the diode on and flows into the output. Its voltage flips so the switch-node
end rises to ![V_out](boost.assets/eq-inline/05b247c888.svg)<!--m:V_{out}--> (above ![V_in](boost.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}-->). Kirchhoff's voltage law again gives the inductor's voltage as the difference of its two end
nodes, ![V_in](boost.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> and ![V_out](boost.assets/eq-inline/05b247c888.svg)<!--m:V_{out}-->:

![V_L equals V_in minus V_out, constant and negative since V_out exceeds V_in](boost.assets/eq-vl-off.svg)

so its current ramps **down**:

![delta I_L fall equals V_out minus V_in times one minus D times T over L](boost.assets/eq-fall.svg)

## 3 Volt-second balance — the step-up ratio

Same steady-state argument as the buck: the rise must exactly cancel the fall, or the current would
drift forever. Set ![rise = fall](boost.assets/eq-inline/7fcffce6ac.svg)<!--m:\text{rise} = \text{fall}--> and cancel ![T](boost.assets/eq-inline/c2c53d6694.svg)<!--m:T--> and ![L](boost.assets/eq-inline/d160e0986a.svg)<!--m:L-->:

![V_in D equals V_out minus V_in times one minus D](boost.assets/eq-balance.svg)

Expand and collect — the ![V_in times D](boost.assets/eq-inline/65f424a0a4.svg)<!--m:V_{in} \cdot D--> terms cancel and ![V_in times D + V_in times (1-D) = V_in](boost.assets/eq-inline/2ed2189b4b.svg)<!--m:V_{in} \cdot D + V_{in} \cdot (1-D) = V_{in}-->:

![V_in D plus V_in minus V_in D equals V_out times one minus D, so V_in equals V_out times one minus D](boost.assets/eq-balance-solve.svg)

![V_out equals V_in over one minus D, boxed](boost.assets/eq-result.svg)

Since ![1 - D < 1](boost.assets/eq-inline/97d60899b6.svg)<!--m:1 - D < 1-->, the output is always *above* the input — a step-up. As ![D to 1](boost.assets/eq-inline/31df83f2e2.svg)<!--m:D \to 1--> the ratio blows up
(in an ideal model), which is why real boosts keep ![D](boost.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> well below 1.

## 4 Sizing the inductor

From the ON-interval rise, with a target ripple ![Delta I_L](boost.assets/eq-inline/c856ab20fc.svg)<!--m:\Delta I_L--> and ![T = 1/f_sw](boost.assets/eq-inline/2a8cbfbb37.svg)<!--m:T = 1/f_{sw}-->:

![L equals V_in D over f_sw delta I_L](boost.assets/eq-inductor-sizing.svg)

## 5 Sizing the output capacitor

This is where the boost genuinely differs from the buck. **While the switch is ON, the diode is
completely cut off** — so for that whole interval the capacitor is the *only* thing feeding the load.
No recharging happens until the switch opens again.

![Boost switch-node square wave above and the sawtooth output-voltage ripple below](boost.assets/fig-02.svg)

_While the switch is ON the diode is off and the output voltage falls in a straight line as the
capacitor alone supplies the load; when the switch opens the diode recharges it. That "cap on its
own" interval is why a boost needs a bigger output capacitor than a buck._

During the ON interval the capacitor discharges at (roughly) the constant load current ![I_load](boost.assets/eq-inline/7902e72f89.svg)<!--m:I_{load}-->.
Using ![I_C = C times dV/dt](boost.assets/eq-inline/56baca3b41.svg)<!--m:I_C = C \cdot dV/dt--> with ![I_C = -I_load](boost.assets/eq-inline/7f021eedd7.svg)<!--m:I_C = -I_{load}--> held over the interval ![D times T](boost.assets/eq-inline/94b24dfcfd.svg)<!--m:D \cdot T-->:

![delta V_out equals I_load D T over C, so C equals I_load D over f_sw delta V_out](boost.assets/eq-cap-sizing.svg)

## 6 Worked numbers — 12 V to 24 V

Same spec as the buck: ![f_sw = 100 kHz](boost.assets/eq-inline/b2d8a51533.svg)<!--m:f_{sw} = 100 \,\mathrm{kHz}--> (![T = 10 mu s](boost.assets/eq-inline/7872cc0fa0.svg)<!--m:T = 10 \mu s-->), 1 A load, 20 % current ripple, 1 % output
ripple. With ![V_in = 12 V](boost.assets/eq-inline/1c9e4d0896.svg)<!--m:V_{in} = 12 V-->, ![V_out = 24 V](boost.assets/eq-inline/b3a4c57dab.svg)<!--m:V_{out} = 24 V-->: from ![V_out = V_in/(1-D)](boost.assets/eq-inline/ff557ad27a.svg)<!--m:V_{out} = V_{in}/(1-D)-->, ![1 - D = 12/24 = 0.5](boost.assets/eq-inline/950b4b5f54.svg)<!--m:1 - D = 12/24 = 0.5-->, so
![D = 0.5](boost.assets/eq-inline/a2406f7d12.svg)<!--m:D = 0.5-->, ![Delta I_L = 0.2 A](boost.assets/eq-inline/c1d7cfca6b.svg)<!--m:\Delta I_L = 0.2 A-->, ![Delta V_out = 0.24 V](boost.assets/eq-inline/14ebf8ce92.svg)<!--m:\Delta V_{out} = 0.24 V-->:

![L equals 12 times 0.5 over 100000 times 0.2 equals 300 microhenry](boost.assets/eq-worked-L.svg)

![C equals 1 times 0.5 over 100000 times 0.24 equals about 20.8 microfarad](boost.assets/eq-worked-C.svg)

Compare with the buck (![L = 112.5 mu H](boost.assets/eq-inline/ff772b9ad7.svg)<!--m:L = 112.5 \mu H-->, ![C = 8.3 mu F](boost.assets/eq-inline/a132e220df.svg)<!--m:C = 8.3 \mu F-->) at a comparable spec:

| Quantity | Buck (12→3 V) | Boost (12→24 V) | Boost / Buck |
|---|---|---|---|
| Inductance ![L](boost.assets/eq-inline/d160e0986a.svg)<!--m:L--> | 112.5 µH | 300 µH | ≈ 2.7× |
| Output cap ![C](boost.assets/eq-inline/32096c2e0e.svg)<!--m:C--> | 8.3 µF | 20.8 µF | ≈ 2.5× |

> **Note —** The boost needs roughly **3× the inductance and 3× the capacitance** for the same
> ripple spec. That is not a coincidence — it is the direct consequence of §5: the capacitor carries
> the load alone for a whole interval, so it has to be larger.

## 7 What this costs you

- **Bigger output capacitor, unavoidably.** §5 is the reason; there is no wiring trick around it,
  only better capacitor technology (lower ESR, higher capacitance density).
- **No inherent short-circuit protection.** Unlike a buck, a boost has a direct DC path from input to
  output through the inductor and diode, so it cannot limit output current by turning the switch off.
- **High duty cycles are dangerous.** As ![D to 1](boost.assets/eq-inline/31df83f2e2.svg)<!--m:D \to 1--> the ratio ![1/(1-D)](boost.assets/eq-inline/a84fc27741.svg)<!--m:1/(1-D)--> runs away and peak currents
  soar; practical boosts avoid very high ![D](boost.assets/eq-inline/50c9e8d5fc.svg)<!--m:D-->.
- **Same ideal-parts caveat as the buck** (§7 there): diode drop, capacitor ESR, and core losses all
  shift the real numbers.

## 8 Sources and cross-links

- The polarity flip that makes step-up possible:
  [../../fundamentals/inductor/inductor.md §5](../../fundamentals/inductor/inductor.md#5-polarity-lenz-and-the-sign-flip).
- The buck derivation this mirrors: [../buck/buck.md §3](../buck/buck.md#3-volt-second-balance--the-step-down-ratio).
- The capacitor law behind §5:
  [../../fundamentals/capacitor/capacitor.md §4](../../fundamentals/capacitor/capacitor.md#4-from-the-law-to-the-ramp).
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
