# The diode and the half-wave rectifier — one valve, half the wave

A rectifier turns alternating current into current that flows one way only. Its working part is the
diode, a two-terminal valve that lets current through in one direction and blocks it in the other.
This document builds the diode up from its I–V curve, explains the two non-ideal properties that
decide which diode you may use in a 50 kHz inverter (forward drop and reverse recovery), and then
derives everything about the simplest rectifier there is — one diode, one source, one load — with
every integral done in full. The [full bridge](full-bridge.md) that the inverter actually uses is
built on these results.

**Contents**

1. [What a diode is and its I–V curve](#1-what-a-diode-is-and-its-iv-curve)
2. [Reverse blocking, breakdown and PIV](#2-reverse-blocking-breakdown-and-piv)
3. [Reverse recovery and why it decides everything at 50 kHz](#3-reverse-recovery-and-why-it-decides-everything-at-50-khz)
4. [The half-wave rectifier circuit](#4-the-half-wave-rectifier-circuit)
5. [The average value, by integration](#5-the-average-value-by-integration)
6. [The RMS value, by integration](#6-the-rms-value-by-integration)
7. [Form factor, ripple factor and efficiency](#7-form-factor-ripple-factor-and-efficiency)
8. [Adding a reservoir capacitor](#8-adding-a-reservoir-capacitor)
9. [The transformer problem — DC in the winding](#9-the-transformer-problem--dc-in-the-winding)
10. [What this costs you](#10-what-this-costs-you)
11. [Sources and cross-links](#11-sources-and-cross-links)

> **The thesis in one line**
>
> A diode passes current only one way. Put one in series with an AC source and you keep one half
> of every cycle: the average is <!--m:V_{pk}/\pi-->![V_pk/](half-wave.assets/eq-inline/79c2b285b0.svg)<!--/m-->, the ripple repeats at the source frequency <!--m:f-->![f](half-wave.assets/eq-inline/4a0a19218e.svg)<!--/m-->, and
> the winding feeding it carries a DC current it was never designed for. That is why real power
> supplies, including the inverter's DC bus, use the full bridge instead.

---

## 1 What a diode is and its I–V curve

A silicon diode is a **PN junction**: one side of the crystal doped to have spare electrons (N),
the other doped to have spare holes (P). Where they meet, electrons and holes recombine and leave
a thin region with no free carriers at all, the *depletion region*, held in place by the built-in
electric field of the ions left behind. That field is a hill the carriers must climb.

- **Forward bias** (P side, the anode, more positive than N side, the cathode) lowers the hill.
  Once the applied voltage approaches the built-in potential, carriers flood across and the
  current rises *exponentially*.
- **Reverse bias** raises the hill. Essentially nothing crosses except a tiny thermally generated
  leakage current.

The exponential is the Shockley diode equation:

![I_F equals I_S times e to the V_F over n V_T minus 1](half-wave.assets/eq-shockley.svg)

- **<!--m:I_S-->![I_S](half-wave.assets/eq-inline/9f7a3241bf.svg)<!--/m-->** — the saturation current, a property of the junction area and material, typically
  <!--m:10^{-14}-->![10^-14](half-wave.assets/eq-inline/c5a0ca7250.svg)<!--/m--> to <!--m:10^{-8}\,\mathrm{A}-->![10^-8 A](half-wave.assets/eq-inline/b953458724.svg)<!--/m--> for silicon rectifiers.
- **<!--m:n-->![n](half-wave.assets/eq-inline/d1854cae89.svg)<!--/m-->** — the ideality factor, between 1 and 2.
- **<!--m:V_T-->![V_T](half-wave.assets/eq-inline/ea07f61e6f.svg)<!--/m-->** — the thermal voltage:

![V_T equals k T over q, about 25.9 millivolts at 300 kelvin](half-wave.assets/eq-thermal-voltage.svg)

Turn the equation round to ask the question a power designer actually asks — *at this current,
how many volts do I lose?* — and add the series resistance <!--m:R_S-->![R_S](half-wave.assets/eq-inline/d256319252.svg)<!--/m--> of the silicon and the leads:

![V_F equals n V_T ln of 1 plus I_F over I_S, plus I_F R_S](half-wave.assets/eq-invert-shockley.svg)

The logarithm is why every diode seems to have a fixed "forward drop". Multiply the current by ten
and the voltage rises by only:

![delta V_F per decade of current equals n V_T ln 10, about n times 60 millivolts](half-wave.assets/eq-per-decade.svg)

so across the three decades from 10 mA to 10 A a silicon rectifier's drop only wanders between
roughly 0.6 V and 1.1 V. That narrow band is the "0.7 V" rule of thumb.

![Diode current versus voltage, forward knee of silicon and Schottky diodes and reverse blocking up to breakdown](half-wave.assets/fig-80.svg)

_Left: the forward knee. The model silicon PN rectifier drops 0.89 V at 1 A; the Schottky drops
0.34 V. Right: the reverse side on a milliamp scale. The PN part blocks with nanoamps up to about
1000 V; a silicon Schottky leaks visibly and breaks down near 100 V, far below a 325 V bus._

A **Schottky diode** replaces the PN junction with a metal–semiconductor junction. Its barrier is
lower, so the drop is only about 0.3–0.5 V, and it has no stored minority charge (see §3). The
price is higher leakage and a low breakdown voltage: silicon Schottkys stop at roughly 100–200 V.
**Silicon carbide (SiC) Schottkys** move the same idea to a wide-bandgap material and block 650 V
or 1200 V, at the cost of a *higher* drop (about 1.3–1.7 V) and a higher price.

> **Note —** The forward drop is where the rectifier's conduction loss comes from. Model the diode
> as a fixed voltage <!--m:V_{F0}-->![V_F0](half-wave.assets/eq-inline/be5b358018.svg)<!--/m--> plus a small resistance <!--m:r_D-->![r_D](half-wave.assets/eq-inline/980fa0b2e6.svg)<!--/m--> and the loss is
>
> ![P_D equals V_F0 times I_D average plus r_D times I_D rms squared](half-wave.assets/eq-conduction-loss.svg)
>
> The average term dominates at low current; the RMS term punishes the peaky currents of §8.

## 2 Reverse blocking, breakdown and PIV

On the reverse side the diode stays off until the field across the depletion region is strong
enough to tear electrons loose and start an avalanche — the **breakdown voltage** <!--m:V_{BR}-->![V_BR](half-wave.assets/eq-inline/72b49cbd95.svg)<!--/m-->. A
rectifier diode in normal use must never get there. The number that matters is the largest reverse
voltage the circuit ever puts across the diode, the **peak inverse voltage (PIV)**, and the diode's
rated repetitive reverse voltage <!--m:V_{RRM}-->![V_RRM](half-wave.assets/eq-inline/3b94cceb49.svg)<!--/m--> must exceed it with margin.

The PIV is set by the circuit, not the diode, so each rectifier gets its own derivation. For the
half-wave rectifier with a reservoir capacitor (§8) it turns out to be <!--m:2V_{pk}-->![2V_pk](half-wave.assets/eq-inline/a88e400c7b.svg)<!--/m-->; for the full
bridge it is only <!--m:V_{pk}-->![V_pk](half-wave.assets/eq-inline/a753175303.svg)<!--/m--> ([full-bridge.md §3](full-bridge.md#3-the-full-bridge)). For a 325 V
bus a sensible choice is a 600 V or 650 V part — the extra margin is for the voltage spikes that
a transformer's leakage inductance rings up at every edge.

## 3 Reverse recovery and why it decides everything at 50 kHz

A conducting PN diode is full of *stored charge*: minority carriers injected across the junction
that have not yet recombined. When the circuit reverses the voltage, the diode cannot block until
that charge has been swept out — and the only way out is backwards, through the circuit. For a
short time the diode conducts **in reverse**, as if it were a short circuit. This is
**reverse recovery**:

- the current falls at a rate set by the circuit, <!--m:di/dt = -V/L_{lk}-->![di/dt = -V/L_lk](half-wave.assets/eq-inline/3b5b15f07c.svg)<!--/m-->, where <!--m:L_{lk}-->![L_lk](half-wave.assets/eq-inline/c8dd7f6896.svg)<!--/m--> is the
  stray (leakage) inductance in the loop;
- it overshoots through zero to a peak reverse current <!--m:I_{RRM}-->![I_RRM](half-wave.assets/eq-inline/23d061c179.svg)<!--/m-->;
- it then decays back to zero over the recovery time; the total time spent conducting backwards is
  the **reverse-recovery time <!--m:t_{rr}-->![t_rr](half-wave.assets/eq-inline/5add2248c2.svg)<!--/m-->**, and the charge that flowed backwards is

![Q_rr equals the integral of the magnitude of i_D over the recovery interval, about one half I_RRM t_rr](half-wave.assets/eq-rr-charge.svg)

![Diode current during turn-off showing reverse recovery for standard, ultrafast and silicon carbide diodes against a 10 microsecond half period](half-wave.assets/fig-81.svg)

_The same turn-off for three model diodes. The standard-recovery part conducts backwards for about
3 µs — almost a third of the 10 µs half-period of a 50 kHz square wave. The ultrafast part
recovers in tens of nanoseconds; the SiC Schottky has no stored charge at all, only a little
junction capacitance to charge._

**Why it does not matter at 50 Hz.** The mains half-period is 10 ms. A 1N4007-class diode with a
few microseconds of recovery is reverse-conducting for well under a thousandth of the cycle, and
its current is falling slowly anyway because the mains voltage changes slowly. Nobody notices.

**Why it matters enormously at 50 kHz.** In the inverter the rectifier is fed by a transformer
switching every 10 µs. Every edge, two diodes of the bridge must turn off while the other two turn
on. While a slow diode recovers, it and its newly conducting partner form a short across the
transformer secondary. Three things go wrong at once:

1. **Loss.** Every recovery dumps roughly <!--m:Q_{rr}-->![Q_rr](half-wave.assets/eq-inline/7bb9c14f79.svg)<!--/m--> worth of charge through the full reverse
   voltage, once per cycle per diode:

   ![P_rr approximately equals Q_rr times V_R times f](half-wave.assets/eq-rr-loss.svg)

   ![standard: 1.8 microcoulombs times 325 volts times 50 kilohertz is about 29 watts; ultrafast: 25 nanocoulombs gives about 0.4 watts](half-wave.assets/eq-rr-worked.svg)

   Twenty-nine watts per diode is not a loss figure; it is a diode on fire.
2. **Stress on the primary switches.** The recovery current is reflected through the transformer
   into H-bridge 1's MOSFETs as a current spike at every turn-on
   ([../dc-ac-inverters/h-bridge/](../dc-ac-inverters/h-bridge/)).
3. **Ringing and EMI.** When the recovery ends abruptly ("snappy" recovery), the current in
   <!--m:L_{lk}-->![L_lk](half-wave.assets/eq-inline/c8dd7f6896.svg)<!--/m--> is cut off suddenly and rings with the diode capacitance, producing voltage spikes well
   above <!--m:V_{pk}-->![V_pk](half-wave.assets/eq-inline/a753175303.svg)<!--/m--> — which is where the PIV margin of §2 gets used up.

The fix is to choose a diode built for it:

| Diode family | Typical <!--m:t_{rr}-->![t_rr](half-wave.assets/eq-inline/5add2248c2.svg)<!--/m--> | Forward drop | Fit for a 325 V, 50 kHz rectifier |
|---|---|---|---|
| Standard recovery (1N400x) | 2–30 µs | ~0.9–1.1 V | **No** — fine for 50 Hz mains only |
| Fast recovery | 150–500 ns | ~1.0–1.3 V | Marginal |
| Ultrafast (UF400x, MUR4xx, "hyperfast") | 25–75 ns | ~1.0–1.7 V | **Yes** — the usual choice |
| Silicon Schottky | ~0 (no stored charge) | ~0.3–0.5 V | **No** — cannot block 325 V |
| SiC Schottky | ~0 (capacitive charge only) | ~1.3–1.7 V | **Yes** — best switching, highest cost |

> **Watch out —** A datasheet's <!--m:t_{rr}-->![t_rr](half-wave.assets/eq-inline/5add2248c2.svg)<!--/m--> is measured at a stated <!--m:I_F-->![I_F](half-wave.assets/eq-inline/1785b4b06e.svg)<!--/m--> and <!--m:di/dt-->![di/dt](half-wave.assets/eq-inline/47bacb536a.svg)<!--/m-->. At higher
> current, higher temperature or faster edges it gets worse. Always compare at the conditions your
> circuit will produce.

## 4 The half-wave rectifier circuit

Put one diode in series between an AC source <!--m:v_s = V_{pk}\sin\omega t-->![v_s = V_pk t](half-wave.assets/eq-inline/1f9c16c4a0.svg)<!--/m--> and a load resistor <!--m:R_L-->![R_L](half-wave.assets/eq-inline/9640655061.svg)<!--/m-->.

- During the **positive half-cycle** the anode is above the cathode, the diode conducts, and the
  load sees the source minus one forward drop, <!--m:v_o \approx v_s - V_F-->![v_o v_s - V_F](half-wave.assets/eq-inline/cf9777a3e2.svg)<!--/m-->.
- During the **negative half-cycle** the diode is reverse-biased; no current flows and <!--m:v_o = 0-->![v_o = 0](half-wave.assets/eq-inline/e543903b34.svg)<!--/m-->.
  The whole source voltage appears across the diode instead.

Ignoring the drop for the moment, and writing the phase <!--m:\theta = \omega t-->![= t](half-wave.assets/eq-inline/95cafb9df1.svg)<!--/m-->:

![v_o of theta equals V_pk sin theta for theta from 0 to pi, and 0 for theta from pi to 2 pi](half-wave.assets/eq-hw-wave.svg)

![Half-wave rectifier schematic with its output on a resistor and with a reservoir capacitor, and the diode charging current](half-wave.assets/fig-82.svg)

_(a) On a resistor, one hump per cycle and nothing in between. (b) With a 1000 µF reservoir
capacitor and a 1 A load the output stays near the peak, sagging 18 V between recharges — once per
cycle, so the ripple is at 50 Hz. Bottom: each recharge is a single 18 A gulp lasting about 1.5 ms._

The output is now *unidirectional* — it never goes negative — but it is far from steady. To
describe how much "DC" it contains we need two numbers: the average and the RMS.

## 5 The average value, by integration

The average (DC) value of any periodic waveform is its area over one period divided by the period.
In terms of phase, one period is <!--m:2\pi-->![2](half-wave.assets/eq-inline/0833718ca4.svg)<!--/m--> radians. The integrand is zero for the second half, so only
the first half contributes:

![V_avg equals 1 over 2 pi times the integral from 0 to 2 pi of v_o, which equals V_pk over 2 pi times minus cos theta from 0 to pi, equals V_pk over 2 pi times 1 plus 1](half-wave.assets/eq-hw-avg.svg)

The evaluation is where sign slips happen, so slowly: <!--m:-\cos\pi = -(-1) = +1-->![- = -(-1) = +1](half-wave.assets/eq-inline/5e0c819083.svg)<!--/m--> and
<!--m:-\cos 0 = -1-->![- 0 = -1](half-wave.assets/eq-inline/bff4efa712.svg)<!--/m-->, so the bracket is <!--m:(+1) - (-1) = 2-->![(+1) - (-1) = 2](half-wave.assets/eq-inline/004ebd3535.svg)<!--/m-->. Therefore:

![V_avg equals V_pk over pi, about 0.318 V_pk, boxed](half-wave.assets/eq-hw-avg-result.svg)

For a 325 V peak that is about 103 V of DC — less than a third of the peak. A moving-coil DC meter
on the output would read this number.

## 6 The RMS value, by integration

RMS ("root mean square") is the value of DC that would heat a resistor equally. Square the
waveform, average the square, take the root. Squaring needs the identity that turns
<!--m:\sin^2-->![^2](half-wave.assets/eq-inline/9343065c8e.svg)<!--/m--> into something integrable:

![sin squared theta equals one minus cos 2 theta over 2, so the integral of sin squared from 0 to pi is pi over 2](half-wave.assets/eq-sin-squared.svg)

(The <!--m:\sin 2\theta-->![2](half-wave.assets/eq-inline/963ce7f77d.svg)<!--/m--> term vanishes at both <!--m:0-->![0](half-wave.assets/eq-inline/b6589fc6ab.svg)<!--/m--> and <!--m:\pi-->![](half-wave.assets/eq-inline/6ac47b6d73.svg)<!--/m-->, leaving <!--m:\pi/2-->![/2](half-wave.assets/eq-inline/9a0abc6cd5.svg)<!--/m-->.) Now the mean square:

![V_rms squared equals 1 over 2 pi times the integral of v_o squared, equals V_pk squared over 2 pi times pi over 2, equals V_pk squared over 4](half-wave.assets/eq-hw-rms.svg)

![V_rms equals V_pk over 2, boxed](half-wave.assets/eq-hw-rms-result.svg)

Compare the full sine wave, whose RMS is <!--m:V_{pk}/\sqrt{2}-->![V_pk/ 2](half-wave.assets/eq-inline/09ea721e14.svg)<!--/m-->. Throwing away half the wave halves
the *power* (the mean square goes from <!--m:V_{pk}^2/2-->![V_pk^2/2](half-wave.assets/eq-inline/acaf470de9.svg)<!--/m--> to <!--m:V_{pk}^2/4-->![V_pk^2/4](half-wave.assets/eq-inline/4bb5a543d0.svg)<!--/m-->), so the RMS falls by
<!--m:\sqrt{2}-->![2](half-wave.assets/eq-inline/bfe16f27eb.svg)<!--/m-->, not by 2. The signals background for RMS is in
[../fundamentals/signals/](../fundamentals/signals/).

## 7 Form factor, ripple factor and efficiency

Three ratios summarise how "un-DC" the output is.

**Form factor** — RMS over average:

![form factor equals V_rms over V_avg equals pi over 2, about 1.571](half-wave.assets/eq-form-factor.svg)

**Ripple factor** — the RMS of the AC part over the DC part. It uses the fact that a waveform's
mean square is the sum of its DC part squared and its AC part's mean square (the cross term
averages to zero because the AC part averages to zero):

![V_rms squared equals V_avg squared plus V_ac,rms squared](half-wave.assets/eq-ripple-factor-why.svg)

![gamma equals V_ac,rms over V_avg equals the square root of V_rms over V_avg squared minus 1, equals root of pi squared over 4 minus 1, about 1.21](half-wave.assets/eq-ripple-factor.svg)

A ripple factor above 1 means there is *more* AC than DC in the output.

**Rectification efficiency** — the fraction of the load power that is DC power:

![eta equals P_dc over P_total equals V_avg squared over V_rms squared equals 4 over pi squared, about 40.5 percent](half-wave.assets/eq-efficiency.svg)

The other 59.5 % heats the load as AC. The Fourier series makes the content explicit — the DC term,
a large component *at the source frequency itself*, and even harmonics:

![v_o of t equals V_pk over pi plus V_pk over 2 sin omega t minus 2 V_pk over pi times the sum of cos 2 k omega t over 4 k squared minus 1](half-wave.assets/eq-hw-fourier.svg)

That <!--m:\tfrac{1}{2}V_{pk}\sin\omega t-->![12V_pk t](half-wave.assets/eq-inline/f02708aaa3.svg)<!--/m--> term is the half-wave rectifier's signature: the strongest
ripple sits at <!--m:f-->![f](half-wave.assets/eq-inline/4a0a19218e.svg)<!--/m-->, the lowest frequency available, which is the hardest to filter. The full bridge
cancels it exactly ([full-bridge.md §4](full-bridge.md#4-average-rms-and-the-ripple-at-2f)).

## 8 Adding a reservoir capacitor

Put a capacitor <!--m:C-->![C](half-wave.assets/eq-inline/32096c2e0e.svg)<!--/m--> across the load. Near each positive peak the diode conducts and charges <!--m:C-->![C](half-wave.assets/eq-inline/32096c2e0e.svg)<!--/m-->
to almost <!--m:V_{pk}-->![V_pk](half-wave.assets/eq-inline/a753175303.svg)<!--/m-->. As soon as the source falls below the capacitor voltage the diode turns off,
and for the rest of the cycle the capacitor alone feeds the load. A capacitor feeding a roughly
constant current discharges in a straight line — the capacitor law from
[../fundamentals/capacitor/capacitor.md §4](../fundamentals/capacitor/capacitor.md#4-from-the-law-to-the-ramp)
run backwards:

![C dv_o by dt equals minus I_load, so delta V equals delta Q over C equals I_load delta t over C](half-wave.assets/eq-discharge.svg)

With one recharge per cycle, the discharge lasts nearly a whole period <!--m:T = 1/f-->![T = 1/f](half-wave.assets/eq-inline/75216c41f9.svg)<!--/m-->:

![delta t approximately T equals 1 over f, so delta V approximately equals I_load over f C, boxed](half-wave.assets/eq-hw-ripple.svg)

![1 amp over 50 hertz times 1000 microfarads equals 20 volts, simulated 18 volts](half-wave.assets/eq-hw-worked.svg)

The estimate is slightly pessimistic because the discharge really lasts <!--m:T-->![T](half-wave.assets/eq-inline/c2c53d6694.svg)<!--/m--> minus the charging
time. Three consequences follow directly from the picture:

- **The ripple frequency is <!--m:f-->![f](half-wave.assets/eq-inline/4a0a19218e.svg)<!--/m-->.** One recharge per cycle.
- **The capacitor must hold up the load for a full period**, so for a given ripple it needs twice
  the capacitance of a full-wave rectifier ([full-bridge.md §5](full-bridge.md#5-the-reservoir-capacitor)).
- **The PIV doubles.** At the negative peak the capacitor still holds about <!--m:+V_{pk}-->![+V_pk](half-wave.assets/eq-inline/718b7eb144.svg)<!--/m--> on the
  cathode while the source pulls the anode to <!--m:-V_{pk}-->![-V_pk](half-wave.assets/eq-inline/f346e6f004.svg)<!--/m-->:

![PIV equals v_C minus the minimum of v_s, equals V_pk minus minus V_pk, equals 2 V_pk, 650 volts for a 325 volt peak](half-wave.assets/eq-hw-piv.svg)

The charging current comes in one short, tall pulse per cycle — 18 A peak for a 1 A load in the
simulation of Figure 82. The full bridge halves the gap between pulses but keeps the shape; the
detailed analysis of conduction angle and peak current is in
[full-bridge.md §6](full-bridge.md#6-conduction-angle-and-the-tall-current-pulses).

## 9 The transformer problem — DC in the winding

This is the reason a half-wave rectifier is never put on a transformer secondary in a power supply
of any size. The secondary current flows one way only, so it has an average value — equal to the
load current — that never reverses:

![I_s,avg equals I_load, not zero, so the DC flux density is mu_0 mu_r N_s I_s,avg over l_e](half-wave.assets/eq-dc-flux.svg)

A transformer cannot pass DC: the primary only supplies whatever current is needed to cancel the
*changing* part of the secondary's magnetising force. The DC ampere-turns <!--m:N_s I_{s,avg}-->![N_s I_s,avg](half-wave.assets/eq-inline/f4bfb757cf.svg)<!--/m--> are
cancelled by nothing, so they bias the core flux permanently to one side. For an illustrative
ungapped ferrite core:

![B_dc equals 4 pi times 10 to the minus 7 times 2000 times 50 times 1 amp over 0.1 metre, about 1.26 tesla, much greater than B_sat about 0.35 tesla for ferrite](half-wave.assets/eq-dc-flux-worked.svg)

The core saturates long before the load is reached. A saturated core has almost no inductance, the
magnetising current balloons, and the primary switches in H-bridge 1 see current spikes they were
never sized for. (The transformer basics — why it passes only change, and what saturation is — are
in [../fundamentals/transformer/](../fundamentals/transformer/) and
[../fundamentals/electromagnetism/](../fundamentals/electromagnetism/).) Even with a gapped core
that avoids saturation, half the winding's copper sits idle half the time: the standard
**transformer utilisation factor** (DC output power over transformer VA rating) is only about 0.29
for half-wave against about 0.81 for the bridge.

> **Note —** Half-wave rectifiers survive where none of this bites: tiny auxiliary supplies,
> signal detectors, and the *flyback* converter, whose "transformer" is really a coupled inductor
> that is meant to store energy and carries DC by design (with a gapped core).

## 10 What this costs you

- **Only half the input is used.** Average <!--m:V_{pk}/\pi-->![V_pk/](half-wave.assets/eq-inline/79c2b285b0.svg)<!--/m-->, efficiency 40.5 %, ripple factor 1.21.
- **Ripple at <!--m:f-->![f](half-wave.assets/eq-inline/4a0a19218e.svg)<!--/m-->, not <!--m:2f-->![2f](half-wave.assets/eq-inline/88346ae6e0.svg)<!--/m-->.** For the same ripple the capacitor must be twice as large as a
  full-wave design's.
- **PIV of <!--m:2V_{pk}-->![2V_pk](half-wave.assets/eq-inline/a88e400c7b.svg)<!--/m-->** with a reservoir capacitor: 650 V on a 325 V peak, before ringing — a
  1000 V diode in practice.
- **DC in the transformer.** Core saturation or an oversized, gapped core; poor utilisation.
- **One forward drop** is the only advantage: 0.9 V lost instead of the bridge's 1.8 V. At 325 V
  that is 0.3 % and irrelevant; at 3.3 V it would matter, which is why low-voltage outputs use
  Schottkys or synchronous MOSFETs instead.
- **Recovery losses at high frequency** apply to every rectifier, half or full: at 50 kHz use
  ultrafast or SiC parts, never standard-recovery diodes.

## 11 Sources and cross-links

- Next: [full-bridge.md](full-bridge.md) — the rectifier the inverter uses, the reservoir
  capacitor in depth, and why the bus has no smoothing inductor.
- Topic landing page: [README.md](README.md).
- Capacitor law used in §8: [../fundamentals/capacitor/capacitor.md](../fundamentals/capacitor/capacitor.md).
- RMS, averages and Fourier series: [../fundamentals/signals/](../fundamentals/signals/).
- Transformers and core saturation: [../fundamentals/transformer/](../fundamentals/transformer/),
  [../fundamentals/electromagnetism/](../fundamentals/electromagnetism/).
- The H-bridge that drives the transformer: [../dc-ac-inverters/h-bridge/](../dc-ac-inverters/h-bridge/).
- Shockley's diode law: W. Shockley, "The theory of p-n junctions in semiconductors and p-n junction
  transistors", *Bell System Technical Journal* 28 (1949). Rectifier ratios (form factor, ripple
  factor, efficiency, utilisation factor): any power-electronics text, e.g. Mohan, Undeland &
  Robbins, *Power Electronics*, ch. 5; Rashid, *Power Electronics Handbook*, ch. on diode
  rectifiers.
- Figures 80–82 are generated by `toolchain/figures/rectifiers.js`; the waveforms in Figure 82 are
  simulated by `toolchain/parts/rectifiers.js` (Shockley diode with series resistance, 0.5 Ω
  source resistance, implicit-Euler time stepping).
- Style and figure conventions: [../STYLE.md](../STYLE.md).
