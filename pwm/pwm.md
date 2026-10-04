# Pulse-width modulation — controlling an average by switching fast

A switch can only be fully ON or fully OFF; it cannot "half conduct" without burning the
difference as heat. Pulse-width modulation (PWM) gets an in-between value anyway: it flips the
switch so fast that whatever is downstream only responds to the *time average*, and it sets that
average by choosing what fraction of each period the switch spends ON. Every converter in this
tree, from the [buck](../dc-dc-converters/buck/) to the
[SPWM inverter](../dc-ac-inverters/spwm/), is driven this way.

**Contents**

1. [What a PWM wave is](#1-what-a-pwm-wave-is)
2. [The average is D times the amplitude — derived](#2-the-average-is-d-times-the-amplitude--derived)
3. [Resolution — how finely you can set D](#3-resolution--how-finely-you-can-set-d)
4. [Making PWM in a microcontroller — counter and compare](#4-making-pwm-in-a-microcontroller--counter-and-compare)
5. [Making PWM with analogue parts — ramp and comparator](#5-making-pwm-with-analogue-parts--ramp-and-comparator)
6. [Edge-aligned vs. centre-aligned](#6-edge-aligned-vs-centre-aligned)
7. [The spectrum — why a filter recovers the average](#7-the-spectrum--why-a-filter-recovers-the-average)
8. [Where the average goes — motors, LEDs, converters](#8-where-the-average-goes--motors-leds-converters)
9. [Complementary outputs and dead time](#9-complementary-outputs-and-dead-time)
10. [From constant D to a moving D — the step to SPWM](#10-from-constant-d-to-a-moving-d--the-step-to-spwm)
11. [What this costs you](#11-what-this-costs-you)
12. [Sources and cross-links](#12-sources-and-cross-links)

> **The thesis in one line**
>
> A PWM wave is a DC level plus harmonics that all sit at multiples of the switching frequency;
> the DC level is exactly ![D V_in](pwm.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->, so anything slow enough to ignore the harmonics — a
> motor's inertia, your eye, an LC filter — sees a voltage you set with one number, the duty cycle.

---

## 1 What a PWM wave is

A PWM signal is a rectangular wave that repeats every **period** ![T](pwm.assets/eq-inline/c2c53d6694.svg)<!--m:T-->. Its **switching
frequency** is the number of periods per second, ![f_sw = 1/T](pwm.assets/eq-inline/cc2c6208db.svg)<!--m:f_{sw} = 1/T-->. In each period the signal is
high (the switch is ON, the output sits at ![V_in](pwm.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}-->) for an **on-time** ![t_on](pwm.assets/eq-inline/b0100d7e48.svg)<!--m:t_{on}-->, and low (switch
OFF, output at 0 V) for the rest. The **duty cycle** is the fraction of the period spent high:

![D equals t_on over T equals t_on times f_sw, with T equal to one over f_sw and D between 0 and 1](pwm.assets/eq-duty.svg)

So a PWM wave is fully described by two numbers: how often it repeats (![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->) and what
fraction of each repetition it is ON (![D](pwm.assets/eq-inline/50c9e8d5fc.svg)<!--m:D-->). The amplitude ![V_in](pwm.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> is whatever the switch is
connected to; PWM does not change it.

![PWM waveform anatomy with period, on-time and average, and a duty-cycle sweep whose average tracks D](pwm.assets/fig-01.svg)

_Top: one PWM wave with its period and on-time marked; the blue line is its average. Bottom: the
same 12 V switched at three duty cycles — the average moves from 1.2 V to 6 V to 10.8 V while the
pulse height never changes. Only the width carries the information._

> **Note —** "Pulse-*width*" is literal: the information is in the width of each pulse, not its
> height. Two PWM waves with the same ![D](pwm.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> have the same average whatever their frequency; the
> frequency only decides how hard the ripple is to filter out (§7).

## 2 The average is D times the amplitude — derived

The average of any periodic signal is its integral over one period divided by the period — the
area under one cycle, spread evenly across the cycle:

![v bar equals one over T times the integral from 0 to T of v of t dt](pwm.assets/eq-average-def.svg)

For a PWM wave the integrand is piecewise constant: ![V_in](pwm.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> for the first ![D T](pwm.assets/eq-inline/2c5fea785b.svg)<!--m:D T--> seconds and 0 for
the remaining ![(1-D) T](pwm.assets/eq-inline/5e1e919fc7.svg)<!--m:(1-D) T-->. Split the integral at the switching instant and each piece is just a
rectangle (height times width):

![v bar equals one over T times the integral of V_in from 0 to DT plus the integral of 0 from DT to T, which equals V_in D T over T, which is D V_in](pwm.assets/eq-average-derive.svg)

![v bar equals D V_in, boxed](pwm.assets/eq-average-result.svg)

That is all there is to it: the period cancels, so the average depends only on the duty cycle and
the amplitude. Doubling the frequency at the same ![D](pwm.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> leaves the average unchanged.

If the switch instead flips the output between ![+V](pwm.assets/eq-inline/d84db4c8f2.svg)<!--m:+V--> and ![-V](pwm.assets/eq-inline/6bfad0cb61.svg)<!--m:-V--> (as an H-bridge does — see
[../dc-ac-inverters/h-bridge/](../dc-ac-inverters/h-bridge/)), the same rectangle argument gives:

![v bar equals D times plus V plus one minus D times minus V, equals two D minus one times V](pwm.assets/eq-average-bipolar.svg)

Now ![D = 0.5](pwm.assets/eq-inline/a2406f7d12.svg)<!--m:D = 0.5--> means 0 V average, ![D = 1](pwm.assets/eq-inline/992a13a31d.svg)<!--m:D = 1--> means ![+V](pwm.assets/eq-inline/d84db4c8f2.svg)<!--m:+V--> and ![D = 0](pwm.assets/eq-inline/1526d346fb.svg)<!--m:D = 0--> means ![-V](pwm.assets/eq-inline/6bfad0cb61.svg)<!--m:-V-->. Keep this one in mind:
it is the formula the [SPWM inverter](../dc-ac-inverters/spwm/) runs on.

## 3 Resolution — how finely you can set D

A digital PWM cannot produce any duty cycle; it produces one of a finite set. A timer counts clock
ticks at ![f_clk](pwm.assets/eq-inline/2f63f8533f.svg)<!--m:f_{clk}--> and one PWM period lasts ![N](pwm.assets/eq-inline/b51a60734d.svg)<!--m:N--> ticks, so the on-time can only be a whole number of
ticks. The number of steps, the smallest change in duty, and the equivalent number of bits are:

![N equals f_clk over f_sw, delta D equals one over N, bits equals log base 2 of N](pwm.assets/eq-steps.svg)

A typical STM32 timer clocked at 84 MHz running a 50 kHz PWM:

![N equals 84 MHz over 50 kHz equals 1680, so log base 2 of 1680 is about 10.7 bits, and delta D equals one over 1680, about 0.06 percent](pwm.assets/eq-steps-worked.svg)

On a 12 V supply that is a smallest average-voltage step of:

![delta v bar equals V_in over N equals 12 V over 1680, about 7.1 millivolts](pwm.assets/eq-step-volts.svg)

The catch is that ![N](pwm.assets/eq-inline/b51a60734d.svg)<!--m:N--> and ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> share a fixed budget, the clock. Raise the switching frequency
and you lose steps in direct proportion:

![N times f_sw equals f_clk, so f_sw 100 kHz gives N 840 and f_sw 500 kHz gives N 168](pwm.assets/eq-tradeoff.svg)

This is the central trade-off of digital PWM: a higher ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> makes the ripple easier to filter
(§7) but makes ![D](pwm.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> coarser. A converter regulating to 0.1 % needs roughly 1000 steps, which on an
84 MHz clock caps it near 84 kHz unless the chip has a high-resolution timer (some parts
interpolate edges to a fraction of a tick, e.g. 184 ps steps on STM32 HRTIM).

## 4 Making PWM in a microcontroller — counter and compare

A microcontroller timer has three registers that matter: a **counter** (CNT) that increments on
every clock tick (after an optional prescaler PSC), an **auto-reload register** (ARR) at which the
counter wraps back to 0, and a **capture/compare register** (CCR). A comparator in the hardware
watches CNT continuously and drives the output pin:

- output **high** while CNT < CCR,
- output **low** while CNT ≥ CCR.

The counter ramps 0, 1, 2, … ARR, 0, 1, … — a digital sawtooth. Comparing that sawtooth with the
fixed number in CCR produces a pulse whose width is proportional to CCR:

![f_sw equals f_clk over PSC plus one times ARR plus one, and D equals CCR over ARR plus one](pwm.assets/eq-timer-edge.svg)

Changing the duty cycle is a single register write to CCR; the hardware takes the new value at the
next wrap, so the pulse in progress is never cut short. The CPU does nothing per cycle.

![Timer counter compared with a compare register: edge-aligned sawtooth count and centre-aligned up-down count with their PWM outputs](pwm.assets/fig-02.svg)

_(a) An up-counter (blue staircase) compared with CCR (amber): the output is high while the count
is below the line. (b) The same rule on an up-down count gives a pulse centred in the period. Each
step of the staircase is one clock tick — that granularity is the resolution of §3._

## 5 Making PWM with analogue parts — ramp and comparator

Before microcontrollers, and still inside most analogue controller ICs, PWM was made from two
signals and a comparator: a **triangle (or sawtooth) wave** at ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->, and a slowly-varying
**control voltage** ![v_ctrl](pwm.assets/eq-inline/7f5d51a4dd.svg)<!--m:v_{ctrl}-->. The comparator's output is high whenever ![v_ctrl](pwm.assets/eq-inline/7f5d51a4dd.svg)<!--m:v_{ctrl}--> is above the
triangle. Nothing else is needed.

![Analogue PWM: a triangle generator and a control voltage feeding a comparator, with the triangle, the control level and the output pulses](pwm.assets/fig-03.svg)

_The comparator only answers one question — which input is higher? — but because the triangle
rises and falls in straight lines, the time it spends below the control level is proportional to
that level. A voltage becomes a width._

Why is the on-time proportional? Because the triangle is a straight line. On its rising half it
goes from 0 to its peak ![V_pk](pwm.assets/eq-inline/a753175303.svg)<!--m:V_{pk}--> in half a period, so its value is a fixed multiple of time, and
the crossing instant ![t_1](pwm.assets/eq-inline/a90caf2e2b.svg)<!--m:t_1--> is found by setting it equal to the control voltage:

![v_tri of t equals 2 V_pk over T times t for t from 0 to T over 2; setting v_tri of t_1 equal to v_ctrl gives t_1 equals v_ctrl over V_pk times T over 2](pwm.assets/eq-ramp.svg)

The falling half is the mirror image, so the output is high for ![t_1](pwm.assets/eq-inline/a90caf2e2b.svg)<!--m:t_1--> on each side of every valley:

![t_on equals 2 t_1 equals v_ctrl over V_pk times T, so D equals v_ctrl over V_pk, boxed](pwm.assets/eq-ramp-duty.svg)

**It is the same idea as §4.** The timer's counter *is* a sawtooth (a staircase of clock ticks)
and CCR *is* the control level; the hardware compare *is* the comparator. Digital PWM is the
analogue ramp-and-comparator with the ramp quantised into ticks. Keep this picture: the
[SPWM inverter](../dc-ac-inverters/spwm/) is exactly this circuit with ![v_ctrl](pwm.assets/eq-inline/7f5d51a4dd.svg)<!--m:v_{ctrl}--> replaced by a sine
wave.

> **Tip —** The triangle must be *linear*. If the ramp bows (an RC charge curve instead of a
> constant-current ramp), the on-time is no longer proportional to ![v_ctrl](pwm.assets/eq-inline/7f5d51a4dd.svg)<!--m:v_{ctrl}--> and the converter's
> gain changes with operating point.

## 6 Edge-aligned vs. centre-aligned

The two counting modes in Figure 67 produce the same average but different pulse positions.

- **Edge-aligned** (up-counter, sawtooth): every pulse starts at the beginning of the period. All
  channels of a timer switch on at the same instant, which bunches the current steps together.
- **Centre-aligned** (up-down counter, triangle): every pulse is centred on the middle of the
  period (here, the counter's valley). The period now takes 2·ARR ticks, so at the same clock the
  resolution halves:

![f_sw equals f_clk over 2 times PSC plus 1 times ARR, D equals CCR over ARR, N equals f_clk over 2 f_sw, 840 at 84 MHz and 50 kHz](pwm.assets/eq-timer-centre.svg)

Centre-aligned PWM is preferred for motor drives and inverters: the pulses of different phases are
symmetrical about the same instant, and the
midpoint of each pulse is the ideal moment to sample the current (it equals the cycle average).
It is also the digital twin of the triangle carrier used in [SPWM](../dc-ac-inverters/spwm/).

## 7 The spectrum — why a filter recovers the average

A PWM wave is not "a DC voltage with some noise". By Fourier's theorem (see
[../fundamentals/signals/](../fundamentals/signals/)) any periodic wave is a sum of a constant plus
sinusoids at whole-number multiples of its repetition frequency. For PWM:

![v of t equals the DC term D V_in plus the sum over n from 1 to infinity of a_n cosine of 2 pi n f_sw t minus phi_n](pwm.assets/eq-fourier-series.svg)

The DC term is exactly the average from §2. The harmonic amplitudes are quickest to get in the
*complex* form of the Fourier coefficients. Euler's formula, ![e^-jx = cos x - j sin x](pwm.assets/eq-inline/c4fbd4be3e.svg)<!--m:e^{-jx} = \cos x - j\sin x--> with
![j = sqrt -1](pwm.assets/eq-inline/63717dd03d.svg)<!--m:j = \sqrt{-1}-->, packs the cosine and sine coefficient integrals of
[../fundamentals/signals/edges-and-fourier.md §5](../fundamentals/signals/edges-and-fourier.md#5-orthogonality--the-trick-that-makes-it-work)
into one: ![c_n = 1 over T integral_0^T v e^-j2 pi nt/T dt = (A_n - j B_n)/2](pwm.assets/eq-inline/d5e8ed821e.svg)<!--m:c_n = \tfrac{1}{T}\int_0^T v\,e^{-j2\pi nt/T}\,dt = (A_n - j B_n)/2-->, where ![A_n](pwm.assets/eq-inline/5aa3f2ac5e.svg)<!--m:A_n--> and ![B_n](pwm.assets/eq-inline/4d79d51257.svg)<!--m:B_n-->
are that section's cosine and sine coefficients (named ![a_n](pwm.assets/eq-inline/278ab95d3a.svg)<!--m:a_n-->, ![b_n](pwm.assets/eq-inline/54d608cbef.svg)<!--m:b_n--> there). So the peak amplitude
of harmonic ![n](pwm.assets/eq-inline/d1854cae89.svg)<!--m:n-->, ![sqrt A_n^2 + B_n^2](pwm.assets/eq-inline/c4acc11253.svg)<!--m:\sqrt{A_n^2 + B_n^2}--> — the ![a_n](pwm.assets/eq-inline/278ab95d3a.svg)<!--m:a_n--> of the series above — is ![2|c_n|](pwm.assets/eq-inline/a0773cede0.svg)<!--m:2|c_n|-->. For PWM the integral runs over the ON interval
only (the OFF interval contributes nothing because the signal is 0 there), and the exponential
integrates like any exponential:

![c_n equals one over T times the integral from 0 to DT of V_in e to the minus j 2 pi n t over T dt, which equals V_in over j 2 pi n times one minus e to the minus j 2 pi n D](pwm.assets/eq-fourier-coeff.svg)

The bracket has magnitude twice a sine: factor out ![e^-j pi nD](pwm.assets/eq-inline/c6040f97a1.svg)<!--m:e^{-j\pi nD}--> to get
![e^-j pi nD (e^j pi nD - e^-j pi nD) = e^-j pi nD times 2j sin ( pi nD)](pwm.assets/eq-inline/5c8b63179f.svg)<!--m:e^{-j\pi nD}\,(e^{j\pi nD} - e^{-j\pi nD}) = e^{-j\pi nD} \cdot 2j\sin(\pi nD)-->, by Euler's formula
again, and the factor in front has size 1. That gives the peak amplitude of the *n*th harmonic:

![the magnitude of one minus e to the minus j 2 pi n D is 2 times the absolute value of sine pi n D, so a_n equals 2 times c_n equals 2 V_in over n pi times the absolute value of sine n pi D](pwm.assets/eq-fourier-mag.svg)

![D 0.3, V_in 12 V: a_0 3.6 V, a_1 equals 24 over pi times sine 0.3 pi equals 6.18 V, a_2 3.63 V, a_10 equals 0](pwm.assets/eq-fourier-worked.svg)

Three facts fall straight out of that formula. **(i)** There is *nothing* between DC and
![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->: the lowest unwanted frequency is the switching frequency itself. **(ii)** Harmonics fall
off as ![1/n](pwm.assets/eq-inline/5f556983ad.svg)<!--m:1/n-->. **(iii)** Harmonic ![n](pwm.assets/eq-inline/d1854cae89.svg)<!--m:n--> vanishes whenever ![nD](pwm.assets/eq-inline/2c3d532b57.svg)<!--m:nD--> is a whole number (at ![D = 0.5](pwm.assets/eq-inline/a2406f7d12.svg)<!--m:D = 0.5--> every
even harmonic is gone; at ![D = 0.3](pwm.assets/eq-inline/ca10ae1714.svg)<!--m:D = 0.3--> the 10th is).

![Computed spectrum of a 30 percent PWM wave with DC and harmonics at multiples of the switching frequency, and the simulated LC-filtered output settling to the average](pwm.assets/fig-04.svg)

_Top: the exact Fourier amplitudes of a 12 V, 30 % PWM wave — one DC bar and a comb at multiples
of the switching frequency. Bottom: the same wave at 50 kHz through a simulated 100 µH / 10 µF
filter into 4 Ω: after the start-up overshoot it sits on 3.6 V with about 0.13 V of ripple._

So recovering the average is a filtering problem with a very convenient shape: keep DC, reject
everything at ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> and above. A second-order LC low-pass (see
[../filters/lc-filter/](../filters/lc-filter/)) does this with an attenuation that grows as the
square of frequency above its corner:

![f_0 equals one over 2 pi root L C, and the magnitude of H of f is approximately f_0 over f squared for f much greater than f_0](pwm.assets/eq-lc-atten.svg)

For the filter in Figure 69:

![f_0 equals one over 2 pi root of 100 microhenry times 10 microfarad, about 5.03 kHz; a_1 times f_0 over f_sw squared equals 6.18 times 0.0101, about 63 millivolts peak](pwm.assets/eq-lc-worked.svg)

63 mV peak is 126 mV peak-to-peak — the 127 mV the simulation measured. Place the corner a decade
below ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> and the first harmonic is cut 100-fold; a higher ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> lets the same attenuation
come from a smaller ![L](pwm.assets/eq-inline/d160e0986a.svg)<!--m:L--> and ![C](pwm.assets/eq-inline/32096c2e0e.svg)<!--m:C-->. That is the whole reason converters switch fast.

## 8 Where the average goes — motors, LEDs, converters

Different loads do the averaging for you, in different ways:

- **DC motors.** The winding inductance smooths the current (its time constant is typically a
  few milliseconds, far longer than a 20 kHz period), and the rotor's inertia smooths the speed
  further. The motor runs as if fed ![D V_in](pwm.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->. Pick ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> above about 20 kHz or the
  winding whines audibly at the switching frequency.
- **LEDs.** An LED does not average at all — it flashes at full brightness — but your eye
  integrates over roughly 10–20 ms, so perceived brightness tracks ![D](pwm.assets/eq-inline/50c9e8d5fc.svg)<!--m:D-->. Frequencies above a few
  hundred hertz look steady; above about 1–3 kHz avoids flicker on camera sensors. Because the
  current at each instant is the full rated current, colour stays constant as you dim, unlike
  analogue dimming.
- **Switching converters.** The [buck](../dc-dc-converters/buck/) uses an explicit LC filter, and
  its ![V_out = D V_in](pwm.assets/eq-inline/ee5553b7dd.svg)<!--m:V_{out} = D\,V_{in}--> is the §2 result: the inductor's volt-second balance forces the
  output to the switch node's average. The [boost](../dc-dc-converters/boost/) uses the same
  switching but a different topology, so its ratio is ![1/(1-D)](pwm.assets/eq-inline/a84fc27741.svg)<!--m:1/(1-D)-->. The soft-start ramp in
  [../dc-dc-converters/buck/startup.md](../dc-dc-converters/buck/startup.md) is simply ![D](pwm.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> raised slowly from 0.
- **Heaters, solenoids, servos.** Heaters average thermally (seconds), so their PWM can be very
  slow; RC servos read the pulse width itself (1–2 ms in a 20 ms frame) as a position command.

## 9 Complementary outputs and dead time

A half-bridge or [H-bridge](../dc-ac-inverters/h-bridge/) leg has two switches stacked across the
supply: a high-side switch to ![+V_dc](pwm.assets/eq-inline/0458144a16.svg)<!--m:+V_{dc}--> and a low-side switch to ground. Driving the output
high means turning the top one ON and the bottom one OFF; driving it low is the reverse. So the two
gates get **complementary** PWM signals, ![G_H](pwm.assets/eq-inline/5f34e47483.svg)<!--m:G_H--> and its inverse ![G_L](pwm.assets/eq-inline/d87d1f52ba.svg)<!--m:G_L-->.

If you literally invert one signal to make the other, both switches change state at the same
instant — and real MOSFETs and IGBTs (insulated-gate bipolar transistors, the other common power switch)
turn OFF more slowly than they turn ON. For a few tens or
hundreds of nanoseconds both conduct at once, shorting the supply straight through the leg
(**shoot-through**). The current is limited only by stray resistance; the parts heat up or fail.

The fix is **dead time**: every turn-ON edge is delayed by ![t_d](pwm.assets/eq-inline/6c703960eb.svg)<!--m:t_d-->, so that after one switch turns
OFF, both stay OFF for ![t_d](pwm.assets/eq-inline/6c703960eb.svg)<!--m:t_d--> before the other turns ON.

![Complementary gate signals for the high-side and low-side switch of one leg with dead-time gaps where both are off](pwm.assets/fig-05.svg)

_The command PWM, and the two gate signals derived from it. In the shaded windows both switches
are OFF; the load current keeps flowing through one switch's body diode (the diode built into every power
MOSFET, see [../dc-ac-inverters/h-bridge/h-bridge.md §5](../dc-ac-inverters/h-bridge/h-bridge.md#5-the-mosfet-as-a-switch)), so the output voltage
during those windows is set by the current's direction, not by the controller._

Dead time costs accuracy. During each window the output follows the current, not the command, so
the average is shifted by a small amount whose sign follows the load current:

![delta v bar is approximately plus or minus t_d f_sw V_dc, e.g. 500 ns times 50 kHz times 12 V equals 0.3 V](pwm.assets/eq-deadtime.svg)

Typical ![t_d](pwm.assets/eq-inline/6c703960eb.svg)<!--m:t_d--> is 100 ns to 1 µs, set in the timer (STM32 advanced timers have a DTG field in the
BDTR register), in the gate-driver IC (several half-bridge drivers have a fixed internal dead
time), or with an RC delay. Too short risks shoot-through; too long wastes voltage and, in an
inverter, distorts the sine near each current zero crossing.

## 10 From constant D to a moving D — the step to SPWM

Everything above assumed ![D](pwm.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> is fixed, or changes slowly as a control loop adjusts it. Nothing in
the hardware requires that. Write a new CCR every period, or replace the analogue ![v_ctrl](pwm.assets/eq-inline/7f5d51a4dd.svg)<!--m:v_{ctrl}--> with
a moving signal, and the switching-cycle average becomes a function of time,
![v(t) = D(t) V_in](pwm.assets/eq-inline/05845d776a.svg)<!--m:\overline{v}(t) = D(t)\,V_{in}-->. Make ![D(t)](pwm.assets/eq-inline/a6f14a1480.svg)<!--m:D(t)--> follow a sine and the filtered output *is* a
sine — that is a DC-to-AC inverter. The details (why the comparator against a triangle gives
exactly the right ![D(t)](pwm.assets/eq-inline/a6f14a1480.svg)<!--m:D(t)-->, how high the carrier must be, where the harmonics land) are in
[../dc-ac-inverters/spwm/](../dc-ac-inverters/spwm/).

## 11 What this costs you

- **Switching losses grow with frequency.** Every edge dissipates roughly
  ![12 V I t_r](pwm.assets/eq-inline/cca0057a0f.svg)<!--m:\tfrac12 V I t_r--> (or ![12 V I t_f](pwm.assets/eq-inline/ad4d29f7e3.svg)<!--m:\tfrac12 V I t_f-->) in the switch, the triangle of overlapping voltage and
  current derived in
  [../fundamentals/transformer/transformer.md §15](../fundamentals/transformer/transformer.md#15-core-loss--the-ceiling-on-frequency),
  and there are two per period, so loss is
  proportional to ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->. This is the counterweight to the smaller filter a high ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> allows.
- **Resolution falls with frequency.** ![N = f_clk/f_sw](pwm.assets/eq-inline/09492a4c75.svg)<!--m:N = f_{clk}/f_{sw}-->: at 84 MHz you have 1680
  steps at 50 kHz but only 168 at 500 kHz (§3).
- **EMI.** Fast edges with large ![dv/dt](pwm.assets/eq-inline/7631ac0fee.svg)<!--m:dv/dt--> radiate and couple capacitively into everything
  nearby; the harmonic comb of §7 extends to many times ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->. Layout, snubbers and input
  filters are part of the price.
- **Audible noise.** Below about 20 kHz, inductors and motor windings magnetostrict and whine at
  ![f_sw](pwm.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->.
- **Dead-time error and the shoot-through risk** (§9) — a correctness cost you cannot design away,
  only minimise.
- **The average is only the average.** The load still sees the full pulse height at every instant.
  A component rated for ![D V_in](pwm.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}--> but not for ![V_in](pwm.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> will fail even though the
  meter reads the safe value.

## 12 Sources and cross-links

- Where ![V_out = D V_in](pwm.assets/eq-inline/ee5553b7dd.svg)<!--m:V_{out} = D\,V_{in}--> is used in a real circuit, via volt-second balance:
  [../dc-dc-converters/buck/](../dc-dc-converters/buck/), and the step-up mirror
  [../dc-dc-converters/boost/](../dc-dc-converters/boost/).
- The filter that recovers the average: [../filters/lc-filter/](../filters/lc-filter/).
- Fourier series and spectra: [../fundamentals/signals/](../fundamentals/signals/).
- The bridge whose legs need complementary drive and dead time:
  [../dc-ac-inverters/h-bridge/](../dc-ac-inverters/h-bridge/).
- PWM with a sinusoidally varying duty cycle: [../dc-ac-inverters/spwm/](../dc-ac-inverters/spwm/).
- Inductor and capacitor laws behind the filter:
  [../fundamentals/inductor/](../fundamentals/inductor/), [../fundamentals/capacitor/](../fundamentals/capacitor/).
- Background: N. Mohan, T. Undeland, W. Robbins, *Power Electronics: Converters, Applications and
  Design*, ch. 7–8; ST application note AN4013, *STM32 cross-series timer overview*.
- Style and figure conventions: [../STYLE.md](../STYLE.md).
