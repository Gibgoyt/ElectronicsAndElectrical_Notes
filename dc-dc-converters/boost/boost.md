# The boost converter — where `V_out = V_in/(1−D)` comes from

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
> When the switch opens, the inductor's voltage flips sign and adds *on top of* `V_in` — that
> reversal, forced by volt-second balance, is what pushes the output above the input.

---

## 1 The circuit

![Boost converter schematic with inductor first, switch to ground, diode to output capacitor and load](boost.assets/fig-01.svg)

_Inductor first, then the switch node; the switch **S1** shorts that node to ground, and a diode
carries current on to the output. Same four parts as the buck, wired in a different order._

Same vocabulary as the buck: **ON = closed**, `D` is the fraction of each period `T` the switch is
ON, `f_sw = 1/T`. The one new idea you need is the inductor's **polarity flip**: when its current is
forced to fall, `V_L` changes sign and the inductor behaves like a battery in series with the source
(see [../../fundamentals/inductor/inductor.md §4](../../fundamentals/inductor/inductor.md#4-polarity-lenz-and-the-sign-flip)).

## 2 The two intervals

**Interval 1 — switch ON, lasting `D·T`.** S1 shorts the switch node to ground, so the inductor sits
straight across the input and the diode is reverse-biased (off). The inductor sees the full input:

![V_L equals V_in, constant and positive](boost.assets/eq-vl-on.svg)

so its current ramps **up**, storing energy:

![delta I_L rise equals V_in times D T over L](boost.assets/eq-rise.svg)

**Interval 2 — switch OFF, lasting `(1−D)·T`.** The switch path vanishes; the inductor's current
can't stop, so it forces the diode on and flows into the output. Its voltage flips so the switch-node
end rises to `V_out` (above `V_in`). The inductor now sees:

![V_L equals V_in minus V_out, constant and negative since V_out exceeds V_in](boost.assets/eq-vl-off.svg)

so its current ramps **down**:

![delta I_L fall equals V_out minus V_in times one minus D times T over L](boost.assets/eq-fall.svg)

## 3 Volt-second balance — the step-up ratio

Same steady-state argument as the buck: the rise must exactly cancel the fall, or the current would
drift forever. Set `rise = fall` and cancel `T` and `L`:

![V_in D equals V_out minus V_in times one minus D](boost.assets/eq-balance.svg)

Expand and collect — the `V_in·D` terms cancel and `V_in·D + V_in·(1−D) = V_in`:

![V_in D plus V_in minus V_in D equals V_out times one minus D, so V_in equals V_out times one minus D](boost.assets/eq-balance-solve.svg)

![V_out equals V_in over one minus D, boxed](boost.assets/eq-result.svg)

Since `1 − D < 1`, the output is always *above* the input — a step-up. As `D → 1` the ratio blows up
(in an ideal model), which is why real boosts keep `D` well below 1.

## 4 Sizing the inductor

From the ON-interval rise, with a target ripple `ΔI_L` and `T = 1/f_sw`:

![L equals V_in D over f_sw delta I_L](boost.assets/eq-inductor-sizing.svg)

## 5 Sizing the output capacitor

This is where the boost genuinely differs from the buck. **While the switch is ON, the diode is
completely cut off** — so for that whole interval the capacitor is the *only* thing feeding the load.
No recharging happens until the switch opens again.

![Boost switch-node square wave above and the sawtooth output-voltage ripple below](boost.assets/fig-02.svg)

_While the switch is ON the diode is off and the output voltage falls in a straight line as the
capacitor alone supplies the load; when the switch opens the diode recharges it. That "cap on its
own" interval is why a boost needs a bigger output capacitor than a buck._

During the ON interval the capacitor discharges at (roughly) the constant load current `I_load`.
Using `I_C = C·dV/dt` with `I_C = −I_load` held over the interval `D·T`:

![delta V_out equals I_load D T over C, so C equals I_load D over f_sw delta V_out](boost.assets/eq-cap-sizing.svg)

## 6 Worked numbers — 12 V to 24 V

Same spec as the buck: `f_sw = 100 kHz` (`T = 10 µs`), 1 A load, 20 % current ripple, 1 % output
ripple. With `V_in = 12 V`, `V_out = 24 V`: from `V_out = V_in/(1−D)`, `1 − D = 12/24 = 0.5`, so
`D = 0.5`, `ΔI_L = 0.2 A`, `ΔV_out = 0.24 V`:

![L equals 12 times 0.5 over 100000 times 0.2 equals 300 microhenry](boost.assets/eq-worked-L.svg)

![C equals 1 times 0.5 over 100000 times 0.24 equals about 20.8 microfarad](boost.assets/eq-worked-C.svg)

Compare with the buck (`L = 112.5 µH`, `C = 8.3 µF`) at a comparable spec:

| Quantity | Buck (12→3 V) | Boost (12→24 V) | Boost / Buck |
|---|---|---|---|
| Inductance `L` | 112.5 µH | 300 µH | ≈ 2.7× |
| Output cap `C` | 8.3 µF | 20.8 µF | ≈ 2.5× |

> **📝 Note —** The boost needs roughly **3× the inductance and 3× the capacitance** for the same
> ripple spec. That is not a coincidence — it is the direct consequence of §5: the capacitor carries
> the load alone for a whole interval, so it has to be larger.

## 7 What this costs you

- **Bigger output capacitor, unavoidably.** §5 is the reason; there is no wiring trick around it,
  only better capacitor technology (lower ESR, higher capacitance density).
- **No inherent short-circuit protection.** Unlike a buck, a boost has a direct DC path from input to
  output through the inductor and diode, so it cannot limit output current by turning the switch off.
- **High duty cycles are dangerous.** As `D → 1` the ratio `1/(1−D)` runs away and peak currents
  soar; practical boosts avoid very high `D`.
- **Same ideal-parts caveat as the buck** (§7 there): diode drop, capacitor ESR, and core losses all
  shift the real numbers.

## 8 Sources and cross-links

- The polarity flip that makes step-up possible:
  [../../fundamentals/inductor/inductor.md §4](../../fundamentals/inductor/inductor.md#4-polarity-lenz-and-the-sign-flip).
- The buck derivation this mirrors: [../buck/buck.md §3](../buck/buck.md#3-volt-second-balance--the-step-down-ratio).
- The capacitor law behind §5:
  [../../fundamentals/capacitor/capacitor.md §4](../../fundamentals/capacitor/capacitor.md#4-from-the-law-to-the-ramp).
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
