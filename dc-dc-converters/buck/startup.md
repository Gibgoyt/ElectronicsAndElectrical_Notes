# Buck start-up — inrush, overshoot, and why a deep step-down is not slower

[buck.md](buck.md) derives <!--m:V_{out} = D\,V_{in}-->![V_out = D V_in](startup.assets/eq-inline/ee5553b7dd.svg)<!--/m--> for the steady state, where every cycle repeats
the last. This document covers the run-up to that state: what happens in the first cycles after
the switch starts, how many cycles it takes to settle, whether a much lower target makes it take
longer, and what a large step-down really costs. It is the buck counterpart of
[../boost/startup.md](../boost/startup.md), which explains the boost staircase from the source
video (3.png).

**Contents**

1. [The buck version of the 3.png question](#1-the-buck-version-of-the-3png-question)
2. [The first cycles — the full input across the inductor](#2-the-first-cycles--the-full-input-across-the-inductor)
3. [The averaged model — a second-order LC step](#3-the-averaged-model--a-second-order-lc-step)
4. [Overshoot — up to twice the target](#4-overshoot--up-to-twice-the-target)
5. [How many cycles — worked numbers](#5-how-many-cycles--worked-numbers)
6. [Does it depend on the step-down ratio?](#6-does-it-depend-on-the-step-down-ratio)
7. [Soft-start](#7-soft-start)
8. [Large step-downs — where the ratio does bite](#8-large-step-downs--where-the-ratio-does-bite)
9. [What this costs you](#9-what-this-costs-you)
10. [Sources and cross-links](#10-sources-and-cross-links)

> **The thesis in one line**
>
> A buck reaches <!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m--> through an LC filter, so its start-up is the step response of a
> second-order system. It settles in about <!--m:8RC-->![8RC](startup.assets/eq-inline/cc601f6633.svg)<!--/m--> whatever the step-down ratio. Without soft-start
> it overshoots, up to nearly twice the target at light load, and it pulls an inrush current set
> by <!--m:\sqrt{L/C}-->![sqrt L/C](startup.assets/eq-inline/03508d8f6f.svg)<!--/m-->. A deep step-down is not slower. It is less efficient, and its on-time gets
> uncomfortably short.

---

## 1 The buck version of the 3.png question

In the boost, the output climbs in a staircase because each cycle moves one packet of energy into
the capacitor (see [../boost/startup.md §2](../boost/startup.md#2-one-cycle-one-packet-of-energy)).
Does a buck do the same thing, especially when the target is far below the input?

**Partly.** A buck also builds its output cycle by cycle. Volt-second balance
([buck.md §3](buck.md#3-volt-second-balance--the-step-down-ratio)) only holds once the averages
have stopped changing. Before that, each cycle leaves a net change in the inductor current and
net charge on the capacitor. But there are two big differences from the boost:

- **A buck feeds the output directly.** Its inductor is in series with the output during the ON
  interval, *and* during the OFF interval through the freewheel diode. Current flows into the
  capacitor for the whole cycle rather than in one dump per cycle. So a buck's start-up looks
  less like a staircase and more like a smooth, ringing curve with a little ripple on it.
- **A buck's climb is bounded.** Even with no load, a buck cannot exceed <!--m:V_{in}-->![V_in](startup.assets/eq-inline/29f560cdfe.svg)<!--/m-->: once
  <!--m:v_C = V_{in}-->![v_C = V_in](startup.assets/eq-inline/9066bdd86f.svg)<!--/m-->, the inductor has zero volts across it during ON and nothing more can be
  pushed in. A non-synchronous (diode) buck with no load does climb past <!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m-->, for the
  same discontinuous-conduction reason as the boost (§4), but it stops at <!--m:V_{in}-->![V_in](startup.assets/eq-inline/29f560cdfe.svg)<!--/m-->, never "endlessly".

## 2 The first cycles — the full input across the inductor

At power-on the capacitor is at 0 V. During the first ON intervals, the inductor sees almost the
**entire input** across it, not the small <!--m:V_{in} - V_{out}-->![V_in - V_out](startup.assets/eq-inline/fa00e9bcd6.svg)<!--/m--> it sees in steady state:

![V_L equals V_in minus v_C, which is about V_in in the first cycles; the current rises by V_in D T over L, 0.27 A per cycle](startup.assets/eq-first-on.svg)

During OFF it sees only <!--m:-v_C \approx 0-->![-v_C approx 0](startup.assets/eq-inline/ae225e3207.svg)<!--/m-->, so the current hardly falls. The current therefore
**ratchets upward cycle after cycle**. This is the buck's version of the staircase, but in the
*current*, not the voltage. Per cycle, the average current changes by

![The average inductor current changes per cycle by T over L times D V_in minus the average v_C](startup.assets/eq-net-current.svg)

which is positive for as long as the output is below the target <!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m-->. So the current keeps
growing until the capacitor reaches the target. By then the current is well above what the load
needs, and the surplus carries the output *past* the target. This is the **inrush current** and
the **overshoot**, and §3 to §4 turn it into numbers.

> **Note —** In steady state the inductor sees only <!--m:V_{in} - V_{out}-->![V_in - V_out](startup.assets/eq-inline/fa00e9bcd6.svg)<!--/m--> during ON (9 V for
> 12 V to 3 V). In the first cycle it sees 12 V, so it ramps 4/3 as fast. For a large step-down
> such as 48 V to 1 V the contrast is extreme: 48 V at start-up against 47 V in steady state is
> nearly the same. What changes is the OFF interval, where the inductor sees almost nothing
> (about 0 V instead of <!--m:-V_{out}-->![-V_out](startup.assets/eq-inline/cc06122fef.svg)<!--/m-->) until the output builds up.

## 3 The averaged model — a second-order LC step

Average both intervals over one period, as in [buck.md §3](buck.md#3-volt-second-balance--the-step-down-ratio).
The switch node averages to <!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m-->, so the converter becomes a voltage source of value
<!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m--> feeding an LC filter with a resistive load:

![L times d average i_L by dt equals D V_in minus average v; C times d average v by dt equals average i_L minus average v over R](startup.assets/eq-avg-model.svg)

Eliminating the current gives the textbook damped second-order equation:

![L C times the second derivative of v plus L over R times dv by dt plus v equals D V_in](startup.assets/eq-second-order.svg)

with the usual parameters:

![omega_0 equals one over root L C; zeta equals one over 2 R times root L over C; sigma equals zeta omega_0 equals one over 2 R C; Z_0 equals root L over C](startup.assets/eq-params.svg)

Starting from zero current and zero voltage, the underdamped step response is

![v of t equals D V_in times 1 minus e to the minus sigma t times cos omega_d t plus sigma over omega_d sin omega_d t, with omega_d equals omega_0 root 1 minus zeta squared](startup.assets/eq-step-response.svg)

This is why the buck's PWM-plus-LC is often described as *a low-pass filter applied to a square
wave*. That view is developed in [../../filters/lc-filter/](../../filters/lc-filter/). The filter
passes the average, <!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m-->, which is the steady-state formula, and its step response is the
start-up transient.

## 4 Overshoot — up to twice the target

The peak of that response is

![The peak over D V_in equals 1 plus e to the minus pi zeta over root 1 minus zeta squared, which tends to 2 as zeta tends to zero](startup.assets/eq-peak.svg)

With light damping (a light load, so a large <!--m:R-->![R](startup.assets/eq-inline/06576556d1.svg)<!--/m--> and a small <!--m:\zeta-->![zeta](startup.assets/eq-inline/08fe2529d0.svg)<!--/m-->) **the output overshoots
to almost twice its target**. The LC stores the surplus current as magnetic energy and hands it to
the capacitor, exactly as an undamped spring released from rest overshoots its equilibrium by its
full displacement. The peak inductor current while it does so is set by the characteristic
impedance:

![The peak inductor current is about D V_in over Z_0 plus I_load plus delta I_L over 2, for light damping](startup.assets/eq-inrush.svg)

![Buck converter start-up simulated: open-loop overshoot at light and full load versus soft-start](startup.assets/fig-01.svg)

_A switched simulation of the buck.md 12 V to 3 V converter. At full load (3 Ω, ζ = 0.61) the
LC is well damped and the output barely overshoots, to 3.28 V. At light load (30 Ω, ζ = 0.06) it
rings up to 5.49 V, 1.83 times the target, and the inductor current swings from +0.94 A to
−0.61 A while it rings. A 1 ms duty-cycle ramp removes almost all of it._

Two details in Figure 52 are worth noticing:

- **The current goes negative** at light load. In a synchronous buck (a second MOSFET in place of
  the diode) that is allowed: the converter pulls charge *back* out of the overshooting
  capacitor and returns it to the input. In a diode buck it cannot happen. The diode blocks
  reverse current, the inductor sits at zero (discontinuous conduction), and the capacitor can
  only discharge through the load. The overshoot is the same, but it decays much more slowly.
- **With no load at all, a diode buck climbs to the input.** In DCM the output is set by the load,
  not by <!--m:D-->![D](startup.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> alone, and it rises toward <!--m:V_{in}-->![V_in](startup.assets/eq-inline/29f560cdfe.svg)<!--/m--> as the load disappears:

![V_out over V_in equals 2 over 1 plus root of 1 plus 8 L over D squared R T, which tends to 1 as R tends to infinity](startup.assets/eq-dcm-noload.svg)

  The simulation of the unloaded diode buck at D = 0.25 shows 6 V after 10 cycles, 11.3 V after
  500, and the full 12 V after about 3000. This is the buck analogue of the endless boost climb,
  with a ceiling. Closed-loop control (skip/burst mode at light load) is what holds a real buck
  at its target.

## 5 How many cycles — worked numbers

Put in the parts of [buck.md §6](buck.md#6-worked-numbers--12-v-to-3-v):

![omega_0 equals 32,660 radians per second, 5.2 kHz; Z_0 equals 3.67 ohms](startup.assets/eq-worked.svg)

The ringing envelope decays at <!--m:\sigma = 1/(2RC)-->![sigma = 1/(2RC)](startup.assets/eq-inline/6037dace2d.svg)<!--/m-->, so it settles to within about 2 % in roughly
four time constants:

![t_s is about 4 over sigma, which equals 8 R C; the number of cycles is f_sw times t_s](startup.assets/eq-settle.svg)

![R equals 3 ohms: zeta 0.61, t_s about 200 microseconds, about 20 cycles; R equals 30 ohms: zeta 0.061, t_s about 2 ms, about 200 cycles](startup.assets/eq-worked-loads.svg)

The simulation agrees: 18 cycles at full load and 193 cycles at light load to settle within 2 %.
The light-load peak of 5.49 V matches the formula (<!--m:3 \times 1.83-->![3 times 1.83](startup.assets/eq-inline/51ec1fa1c5.svg)<!--/m-->), and so does the inrush
estimate (about 1.0 A predicted, 0.94 A simulated). Compare this with the boost's roughly 320
cycles ([../boost/startup.md §6](../boost/startup.md#6-how-many-cycles--the-averaged-model)). The
buck is quicker for two reasons: it has a smaller capacitor, and there is no <!--m:(1-D)^2-->![(1-D)^2](startup.assets/eq-inline/3b43cf6fe5.svg)<!--/m--> inflating
its effective inductance.

## 6 Does it depend on the step-down ratio?

**For the timing, essentially no.** Look at the averaged model again. The duty cycle enters only
through the size of the input step, <!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m-->. The dynamics, <!--m:\omega_0-->![omega_0](startup.assets/eq-inline/09a7be4d65.svg)<!--/m-->, <!--m:\zeta-->![zeta](startup.assets/eq-inline/08fe2529d0.svg)<!--/m--> and <!--m:\sigma-->![sigma](startup.assets/eq-inline/69c15416b6.svg)<!--/m-->,
depend on <!--m:L-->![L](startup.assets/eq-inline/d160e0986a.svg)<!--/m-->, <!--m:C-->![C](startup.assets/eq-inline/32096c2e0e.svg)<!--/m--> and <!--m:R-->![R](startup.assets/eq-inline/06576556d1.svg)<!--/m--> alone. The system is linear, so a smaller target gives a
proportionally smaller response *with exactly the same shape and timing*. The overshoot as a
fraction of the target is the same at every ratio.

![Normalised start-up responses at several conversion ratios for buck and boost with the same parts](startup.assets/fig-02.svg)

_Top: the same buck hardware stepped to 9 V, 3 V and 0.6 V (ratios 1.3:1, 4:1 and 20:1),
normalised to each target. The three simulated curves lie on top of each other; only the ripple
differs. Bottom: the same experiment on a boost. There the ratio changes the ring frequency
through <!--m:(1-D)-->![(1-D)](startup.assets/eq-inline/453e510d03.svg)<!--/m-->, so higher ratios rise later and ring slower._

What *does* scale with the ratio:

- **Absolute overshoot and inrush scale with the target**, not with the ratio. A lower target
  means a smaller swing in volts and a smaller current (<!--m:D\,V_{in}/Z_0-->![D V_in/Z_0](startup.assets/eq-inline/c20c7d5733.svg)<!--/m-->).
- **A current-limited start is faster for a lower target.** In a buck the inductor current *is*
  the capacitor's charging current, so a controller limiting it to <!--m:I_{lim}-->![I_lim](startup.assets/eq-inline/7e90bcc3db.svg)<!--/m--> fills the capacitor
  at a bounded rate:

![C dv by dt equals i_L minus v over R, at most I_lim minus I_load, so t_min is about C V_target over I_lim minus I_load](startup.assets/eq-current-limit.svg)

  The time is proportional to <!--m:V_{target}-->![V_target](startup.assets/eq-inline/308583ec15.svg)<!--/m-->. Contrast the boost, where the input-side current limit
  makes the minimum start-up time grow as <!--m:M^2-->![M^2](startup.assets/eq-inline/7d07210936.svg)<!--/m-->
  ([../boost/startup.md §7](../boost/startup.md#7-does-it-depend-on-the-step-up-ratio)).
- **In a real design, <!--m:L-->![L](startup.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](startup.assets/eq-inline/32096c2e0e.svg)<!--/m--> are sized for the ratio.** The ripple formulas of
  [buck.md §4–5](buck.md#4-sizing-the-inductor) contain <!--m:D-->![D](startup.assets/eq-inline/50c9e8d5fc.svg)<!--/m-->, so the parts you pick for 48 V to 1 V
  differ from those for 12 V to 9 V, and the settling time changes with them. But that is a
  consequence of the design choice, not of the ratio itself.

So for a deep step-down you do **not** need to wait for many more cycles. The cost of a large
buck ratio is elsewhere (§8).

## 7 Soft-start

The fix for the overshoot and the inrush is the same as for the boost: **ramp the duty cycle**
(or, in a closed-loop controller, the reference) from zero over a time much longer than the LC
period, which is about 0.2 ms here. In Figure 52 a 1 ms ramp holds the light-load peak to 3.09 V
(3 % over) instead of 5.49 V, and the peak inductor current to 0.22 A instead of 0.94 A. The
output follows the slowly moving average instead of being hit with a step.

Every buck controller IC has a soft-start, either internal or set by a capacitor on an SS pin.
Besides taming the overshoot, it keeps the inrush from tripping the input supply's own current
limit, saturating the inductor, or slamming a large output capacitor bank. Unlike the boost
([../boost/startup.md §5](../boost/startup.md#5-before-the-first-pulse--the-pre-charge-jump)),
a buck has no uncontrolled pre-charge path, because its switch sits between the input and
everything else. So soft-start covers the whole inrush.

## 8 Large step-downs — where the ratio does bite

A big step-down does not slow the start-up, but it costs you in four other ways:

**1. Minimum on-time.** The ON interval is <!--m:D\,T = V_{out}/(V_{in} f_{sw})-->![D T = V_out/(V_in f_sw)](startup.assets/eq-inline/ff63414fee.svg)<!--/m-->. Every controller
has a minimum on-time, typically 50 to 150 ns, below which it cannot make a clean pulse (gate
drive, current-sense blanking, propagation delay):

![t_on equals D T equals V_out over V_in f_sw, which must be at least t_on min, so V_in over V_out is at most 1 over f_sw t_on min](startup.assets/eq-min-on.svg)

![With t_on min of 80 ns: at 100 kHz the ratio can reach 125; at 2 MHz only 6.25](startup.assets/eq-min-on-worked.svg)

Small modern bucks switch at 1 to 3 MHz to shrink <!--m:L-->![L](startup.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](startup.assets/eq-inline/32096c2e0e.svg)<!--/m-->, and at those frequencies even a
12 V to 1 V conversion is near the limit. Below the minimum on-time the controller starts skipping
pulses and the output ripple grows. This is why a 48 V to 1 V rail is usually built as two
stages (48 V to 12 V, then 12 V to 1 V) or at a lower frequency.

**2. The freewheel diode's drop becomes a large fraction of the output.** In a deep step-down the
diode conducts for almost the whole cycle, <!--m:(1-D) \approx 1-->![(1-D) approx 1](startup.assets/eq-inline/ab1ffdd7c7.svg)<!--/m-->, carrying the full output current.
Its drop alone caps the efficiency:

![eta is at most V_out over V_out plus one minus D times V_F](startup.assets/eq-diode-eta.svg)

![With V_F of 0.5 V: 12 to 5 V gives at most 94.5 percent; 12 to 1 V gives at most 68.6 percent](startup.assets/eq-diode-eta-worked.svg)

A synchronous buck replaces the diode with a MOSFET whose drop is only <!--m:I \cdot R_{on}-->![I times R_on](startup.assets/eq-inline/8e9b7cf532.svg)<!--/m-->. That is why
every low-voltage processor rail is synchronous.

**3. The switch stress grows with the ratio.** The switch must block the full <!--m:V_{in}-->![V_in](startup.assets/eq-inline/29f560cdfe.svg)<!--/m--> while it
carries the full <!--m:I_{out}-->![I_out](startup.assets/eq-inline/617f9b2205.svg)<!--/m-->:

![S_buck equals V_sw max I_sw over P_out, which is V_in I_out over V_out I_out, which equals M](startup.assets/eq-stress.svg)

At 20:1 the switch is rated for 20 times the output voltage, at the full output current, to
deliver power at only 1/20 of that voltage. Switching losses scale with <!--m:V_{in} I_{out} f_{sw}-->![V_in I_out f_sw](startup.assets/eq-inline/5c8b8b75dd.svg)<!--/m-->,
so they grow with <!--m:M-->![M](startup.assets/eq-inline/c63ae6dd4f.svg)<!--/m--> relative to the output power.

**4. Efficiency versus ratio, quantified.** The full loss budget is plotted in
[../boost/startup.md §10](../boost/startup.md#10-efficiency-versus-step-ratio--the-honest-answer)
(Figure 55). For the buck at 50 W: stepping down *to* 12 V from 24 V, 60 V, 120 V and 324 V gives
96.7 %, 94.8 %, 93.2 % and 89.3 %. Stepping down *from* 12 V to 6 V, 2.4 V, 1.2 V and 0.44 V at
the same current gives 97.5 %, 93.9 %, 88.6 % and 74.1 % with a synchronous rectifier, but
94.0 %, 82.1 %, 67.9 % and 42.7 % with a diode.

**The rule of thumb:** a single buck stage is comfortable up to about 10:1, and up to about 20:1
at modest switching frequency with a synchronous rectifier. Beyond that, use two stages or a
transformer-isolated converter, for the same reasons as the boost.

## 9 What this costs you

- **Open-loop start-up overshoots.** At light load an un-soft-started buck can put almost
  <!--m:2\,D\,V_{in}-->![2 D V_in](startup.assets/eq-inline/74d6868e40.svg)<!--/m--> on its output, which can destroy a 3.3 V logic rail that sees 6 V. Soft-start
  and closed-loop control are not luxuries.
- **The inrush is real current through real parts.** The inductor must not saturate at the
  start-up peak, not just at the steady-state peak, and the input source must be able to supply
  it.
- **Synchronous rectification changes the transient.** With a MOSFET in place of the diode the
  current can reverse, which damps an overshoot actively. With a diode it cannot, and light-load
  behaviour moves into DCM, where <!--m:V_{out}-->![V_out](startup.assets/eq-inline/05b247c888.svg)<!--/m--> is no longer <!--m:D\,V_{in}-->![D V_in](startup.assets/eq-inline/6db2223680.svg)<!--/m-->.
- **A deep step-down costs efficiency, not time.** Minimum on-time, diode drop and switch stress
  all scale with the ratio (§8).
- **Ideal-parts simulation.** The figures use ideal switches and no ESR. Real resistance adds
  damping, so actual rings are somewhat smaller than Figure 52. The scaling arguments are
  unaffected.

## 10 Sources and cross-links

- The steady state this approaches: [buck.md](buck.md) (§3 volt-second balance, §6 the worked
  parts used throughout).
- The boost counterpart (the 3.png staircase, the endless no-load climb, the real gain curve and
  the efficiency-versus-ratio figure): [../boost/startup.md](../boost/startup.md).
- The LC filter view of PWM averaging: [../../filters/lc-filter/](../../filters/lc-filter/).
- How the duty cycle is generated and ramped: [../../pwm/](../../pwm/).
- The inductor and capacitor laws underneath the averaged model:
  [../../fundamentals/inductor/inductor.md](../../fundamentals/inductor/inductor.md),
  [../../fundamentals/capacitor/capacitor.md](../../fundamentals/capacitor/capacitor.md).
- For large ratios, the transformer route: [../../fundamentals/transformer/](../../fundamentals/transformer/),
  [../../dc-ac-inverters/h-bridge/](../../dc-ac-inverters/h-bridge/).
- Figures are switched-circuit simulations in `toolchain/figures/startup.js`.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
