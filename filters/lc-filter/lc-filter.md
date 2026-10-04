# The LC filter — how a chopped voltage becomes a smooth, controllable one

A switch can only be fully on or fully off, so on its own it can only hand a load the full supply
or nothing at all. Put an inductor in series and a capacitor in parallel after it and something
remarkable happens: the load sees a smooth voltage equal to the *average* of the chopped one, and
by changing the duty cycle you can set that average to anything between zero and the supply. Do
that slowly, cycle by cycle, and you can draw *any slow waveform* — including the sine wave an
inverter needs. This document derives every step of that, with simulated waveforms from the real
switched circuit.

**Contents**

1. [The circuit, and why it is the buck converter](#1-the-circuit-and-why-it-is-the-buck-converter)
2. [Two laws, read as smoothing rules](#2-two-laws-read-as-smoothing-rules)
3. [Why the diode has to be there](#3-why-the-diode-has-to-be-there)
4. [The average of a PWM wave](#4-the-average-of-a-pwm-wave)
5. [The circuit equations, and why the filter keeps the average](#5-the-circuit-equations-and-why-the-filter-keeps-the-average)
6. [The transfer function, derived](#6-the-transfer-function-derived)
7. [Choosing the corner frequency — and the design used here](#7-choosing-the-corner-frequency--and-the-design-used-here)
8. [Ripple — what the filter lets through](#8-ripple--what-the-filter-lets-through)
9. [Constant D gives flat DC; change D, change the DC](#9-constant-d-gives-flat-dc-change-d-change-the-dc)
10. [A step in D — ringing, and damping by the load](#10-a-step-in-d--ringing-and-damping-by-the-load)
11. [The big idea — vary D cycle by cycle](#11-the-big-idea--vary-d-cycle-by-cycle)
12. [How fast may D change?](#12-how-fast-may-d-change)
13. [One switch, one polarity — the road to the inverter](#13-one-switch-one-polarity--the-road-to-the-inverter)
14. [What this costs you](#14-what-this-costs-you)
15. [Sources and cross-links](#15-sources-and-cross-links)

> **The thesis in one line**
>
> A series inductor smooths current and a shunt capacitor smooths voltage, so together they pass
> the slow *average* of a PWM wave and reject the fast switching — and because that average is
> <!--m:D\,V_{in}-->![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--/m-->, controlling <!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> cycle by cycle controls the output voltage as a function of time.

---

## 1 The circuit, and why it is the buck converter

![LC filter schematic: switch, freewheel diode, series inductor, shunt capacitor and load resistor — the buck converter](lc-filter.assets/fig-01.svg)

_Switch, freewheel diode, series inductor, shunt capacitor, load. Draw a dashed box around the
inductor and capacitor and call it a filter, or step back and call the whole thing a buck
converter — it is the same circuit, part for part._

Five parts, in order from the source:

- **The switch <!--m:S-->![S](lc-filter.assets/eq-inline/02aa629c8b.svg)<!--/m-->** (in practice a MOSFET) connects the supply <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m--> to the *switch node*
  <!--m:v_s-->![v_s](lc-filter.assets/eq-inline/85026ab589.svg)<!--/m-->, or disconnects it. It is driven by PWM at a fixed switching frequency <!--m:f_{sw} = 1/T-->![f_sw = 1/T](lc-filter.assets/eq-inline/cc2c6208db.svg)<!--/m-->;
  the fraction of each period it is ON (closed) is the duty cycle <!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m-->.
- **The freewheel diode <!--m:D_f-->![D_f](lc-filter.assets/eq-inline/f93c81428b.svg)<!--/m-->** runs from ground (anode) up to the switch node (cathode). It does
  nothing while the switch is ON (it is reverse-biased by <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m-->) and carries the inductor's
  current while the switch is OFF (§3).
- **The inductor <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m-->** sits *in series*: every bit of current that reaches the load passes through
  it.
- **The capacitor <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m-->** sits *in parallel* (shunt) with the load: it sees exactly the output voltage.
- **The load <!--m:R-->![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--/m-->** — whatever is being powered, modelled here as a resistor.

The switch node <!--m:v_s-->![v_s](lc-filter.assets/eq-inline/85026ab589.svg)<!--/m--> is therefore a square wave between <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m--> (switch ON) and about <!--m:0\,\mathrm{V}-->![0 V](lc-filter.assets/eq-inline/23534506b4.svg)<!--/m-->
(switch OFF, diode conducting) — a PWM wave. The inductor and capacitor are the **LC filter**, and
the voltage across the load is <!--m:v_{out}-->![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--/m-->.

If this looks familiar, it should: it is **exactly the [buck converter](../../dc-dc-converters/buck/)**,
with the same parts in the same order. The buck document derives <!--m:V_{out} = D\,V_{in}-->![V_out = D V_in](lc-filter.assets/eq-inline/ee5553b7dd.svg)<!--/m--> from
volt-second balance on the inductor; this document reaches the same result from the other side —
by treating the <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m--> as a *filter* that keeps the average of the PWM wave and throws away
the rest. The two views are the same physics, and holding both in your head is what makes the next
step obvious: if the filter keeps the *average*, and the average is something you control, then
you control the output — not just its DC level but its shape over time.

> **Note —** Many tutorials (including the source video for this note) label the load voltage
> <!--m:V_L-->![V_L](lc-filter.assets/eq-inline/136d4e3fb2.svg)<!--/m-->. Here it is <!--m:v_{out}-->![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--/m-->, because <!--m:v_L-->![v_L](lc-filter.assets/eq-inline/644bea706b.svg)<!--/m--> is reserved for the voltage *across the inductor* in
> <!--m:v_L = L\,di_L/dt-->![v_L = L di_L/dt](lc-filter.assets/eq-inline/7e61c8ca55.svg)<!--/m-->. Lower-case letters are instantaneous values; an overbar (<!--m:\overline{v_s}-->![v_s](lc-filter.assets/eq-inline/13e7f2d910.svg)<!--/m-->)
> means the average over one switching period.

## 2 Two laws, read as smoothing rules

The whole filter is two component laws, each read as a statement about what that part *refuses to
let happen quickly*. Start with the inductor:

![v_L equals L di_L by dt, so the magnitude of di_L by dt equals v_L over L, at most V_in over L](lc-filter.assets/eq-law-L.svg)

Read it backwards. The inductor's *voltage* can jump around however it likes — in this circuit it
jumps every time the switch flips — but its *current* can only change at a rate set by that
voltage. Since nothing in the circuit is bigger than <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m-->, the current's slope is **bounded**.
With the numbers used throughout this document (<!--m:V_{in} = 12\,\mathrm{V}-->![V_in = 12 V](lc-filter.assets/eq-inline/9a063a8f8e.svg)<!--/m-->, <!--m:L = 1\,\mathrm{mH}-->![L = 1 mH](lc-filter.assets/eq-inline/5367ea2324.svg)<!--/m-->):

![V_in over L equals 12 volts over 1 millihenry equals 12000 amps per second, 0.012 amps per microsecond](lc-filter.assets/eq-slope-L-num.svg)

In one <!--m:20\,\mu\mathrm{s}-->![20 mu s](lc-filter.assets/eq-inline/f0d5326bb0.svg)<!--/m--> switching period the current can move by at most a quarter of an amp, no
matter how violently the voltage across it jumps. That is what "an inductor smooths the current"
means precisely: **a voltage jump becomes a current slope**. The square wave on its left becomes a
gentle triangle of current through it (Fig. 58).

Now the capacitor — the same law with voltage and current swapped:

![i_C equals C dv_C by dt, so the magnitude of dv_C by dt equals i_C over C](lc-filter.assets/eq-law-C.svg)

The capacitor's *current* can jump around, but its *voltage* can only change at a rate set by that
current. Feed it a triangle of current and its voltage barely moves: **a current jump becomes a
voltage slope**. That is "a capacitor smooths the voltage".

Then notice *where* each part is placed, because placement is what makes each law useful:

- **The inductor is in series** — in the path of the current. A series element controls what
  *flows through* it, so a part that refuses fast changes of current, placed in series, makes the
  current delivered downstream smooth. At high frequency its impedance <!--m:|Z_L| = \omega L-->![|Z_L| = omega L](lc-filter.assets/eq-inline/3a906b535f.svg)<!--/m--> is large:
  it *blocks* the switching frequency.
- **The capacitor is in parallel** — across the output. A shunt element controls the voltage
  *across* it, so a part that refuses fast changes of voltage, placed in shunt, holds the output
  voltage steady. At high frequency its impedance <!--m:|Z_C| = 1/(\omega C)-->![|Z_C| = 1/( omega C)](lc-filter.assets/eq-inline/bafe731c11.svg)<!--/m--> is small: it *shorts* what
  is left of the switching frequency to ground.

The pair works as a two-stage voltage divider whose ratio collapses at high frequency from both
ends at once — the inductor's impedance rises while the capacitor's falls. Each contributes a factor
of <!--m:\omega-->![omega](lc-filter.assets/eq-inline/73b077a63e.svg)<!--/m-->, which is where the <!--m:-40\,\mathrm{dB}-->![-40 dB](lc-filter.assets/eq-inline/fefc844ddc.svg)<!--/m--> per decade of §6 comes from.

> **Tip —** This duality is the same table as in the
> [capacitor document §6](../../fundamentals/capacitor/capacitor.md#6-the-duality--one-table-read-both-ways):
> swap <!--m:V \leftrightarrow I-->![V I](lc-filter.assets/eq-inline/4ebcd6796a.svg)<!--/m-->, <!--m:L \leftrightarrow C-->![L C](lc-filter.assets/eq-inline/6cef383173.svg)<!--/m-->, and series <!--m:\leftrightarrow-->![](lc-filter.assets/eq-inline/eb753a72a0.svg)<!--/m--> parallel, and
> each statement about one part becomes the statement about the other.

## 3 Why the diode has to be there

The inductor's stiffness is a gift while the switch is ON and a threat the instant it opens. At that
moment the inductor is carrying roughly the load current — <!--m:0.6\,\mathrm{A}-->![0.6 A](lc-filter.assets/eq-inline/90937bd2dc.svg)<!--/m--> in the running example —
and the law in §2 says that current cannot change instantly. If the switch were the only path, opening
it would force the current from <!--m:0.6\,\mathrm{A}-->![0.6 A](lc-filter.assets/eq-inline/90937bd2dc.svg)<!--/m--> to zero in the few nanoseconds the switch takes to
open. The law then demands:

![v_L equals L times delta i_L over delta t, 1 millihenry times minus 0.6 amps over 10 nanoseconds, equals minus 60000 volts](lc-filter.assets/eq-kick.svg)

Sixty thousand volts across the inductor, appearing at the switch node with a negative sign. In
practice the switch breaks down and arcs long before that — this is the *inductive kick* described in
the [inductor document §7](../../fundamentals/inductor/inductor.md#7-the-inductive-kick-and-why-the-diode-is-there),
and it destroys transistors.

The freewheel diode removes the problem without being told to. As soon as the switch opens, the
inductor starts dragging the switch-node voltage down; the moment it dips just below ground, the
diode becomes forward-biased and gives the current a path: ground → diode → inductor → load →
ground. The node is clamped near <!--m:0\,\mathrm{V}-->![0 V](lc-filter.assets/eq-inline/23534506b4.svg)<!--/m--> (one diode drop below, in reality), the inductor
sees a bounded voltage <!--m:v_L = 0 - v_{out}-->![v_L = 0 - v_out](lc-filter.assets/eq-inline/d778776934.svg)<!--/m-->, and its current ramps *down* gently instead of being
chopped off. The green arrow in Fig. 56 is this freewheeling path. The current never stops — only its
slope changes, from rising to falling.

There is a mirror-image reason the capacitor must not come *first*. Put <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m--> directly on the switch
node with no inductor, and every time the switch closes it connects a stiff <!--m:12\,\mathrm{V}-->![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--/m--> source
straight across a capacitor sitting at, say, <!--m:6\,\mathrm{V}-->![6 V](lc-filter.assets/eq-inline/8ed5af7660.svg)<!--/m-->:

![i_C equals C dv_C by dt; v_C jumps from 6 to 12 volts in zero time, so i_C goes to infinity](lc-filter.assets/eq-cap-kick.svg)

That is a current spike limited only by stray resistance — the capacitor's version of the inductive
kick. The inductor in front prevents it: it is the series element that turns the source's voltage step
into a bounded current ramp *before* the current reaches the capacitor. So the order is not arbitrary:
**switch, then diode, then series <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m-->, then shunt <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m-->** is the one arrangement in which neither part is
ever asked to do the impossible.

## 4 The average of a PWM wave

Before asking what the filter does, pin down what it is being fed. The switch node spends <!--m:DT-->![DT](lc-filter.assets/eq-inline/f91a09d081.svg)<!--/m--> at
<!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m--> and <!--m:(1-D)\,T-->![(1-D) T](lc-filter.assets/eq-inline/e399568987.svg)<!--/m--> at <!--m:0-->![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--/m-->. Its average over one period is, by definition, the integral of the
waveform divided by the period:

![v_s bar equals 1 over T times the integral from 0 to T of v_s dt, which splits into V_in over 0 to DT plus zero over DT to T, giving D V_in](lc-filter.assets/eq-avg-def.svg)

The integral of a rectangle is its area. Only the ON part has any area, <!--m:V_{in} \times DT-->![V_in times DT](lc-filter.assets/eq-inline/05ce01dfde.svg)<!--/m-->, and
spreading that area over the whole period <!--m:T-->![T](lc-filter.assets/eq-inline/c2c53d6694.svg)<!--/m--> gives a level of <!--m:D\,V_{in}-->![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--/m-->. Geometrically: the tall
thin rectangle and the short wide one in Fig. 57 have the same area.

![PWM switch-node voltage with the ON area shaded and its average D V_in drawn as a dashed line](lc-filter.assets/fig-02.svg)

_The shaded ON rectangle (height <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m-->, width <!--m:DT-->![DT](lc-filter.assets/eq-inline/f91a09d081.svg)<!--/m-->) and the blue average rectangle (height <!--m:D\,V_{in}-->![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--/m-->,
width <!--m:T-->![T](lc-filter.assets/eq-inline/c2c53d6694.svg)<!--/m-->) hold the same area. An averaging filter cannot tell them apart._

So a PWM wave is a DC level <!--m:D\,V_{in}-->![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--/m--> **plus** a zero-average wobble at the switching frequency
and its harmonics. If the filter can keep the first and discard the second, the output is <!--m:D\,V_{in}-->![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--/m-->.
The rest of the document is about how well it does each.

## 5 The circuit equations, and why the filter keeps the average

Write one law for each energy-storing part. The inductor sits between the switch node and the output,
so its voltage is the difference between them; the capacitor sits at the output, and its current is
whatever the inductor delivers minus what the load takes:

![L di_L by dt equals v_s of t minus v_out of t](lc-filter.assets/eq-ode-L.svg)

![C dv_out by dt equals i_L of t minus v_out of t over R](lc-filter.assets/eq-ode-C.svg)

These two equations, with <!--m:v_s-->![v_s](lc-filter.assets/eq-inline/85026ab589.svg)<!--/m--> jumping between <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m--> and <!--m:0-->![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--/m--> (and the diode stopping <!--m:i_L-->![i_L](lc-filter.assets/eq-inline/0bd5fa35e8.svg)<!--/m--> from
going negative), are *exactly* what the simulator behind every waveform figure here integrates — the
figures are numerical solutions of these equations, not sketches.

Now average the inductor equation over one switching period. The left side becomes <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m--> times the
rate of change of the *average* current, and the right side becomes <!--m:\overline{v_s} - \overline{v_{out}}-->![v_s - v_out](lc-filter.assets/eq-inline/0e1b5020c6.svg)<!--/m-->.
We know <!--m:\overline{v_s} = D\,V_{in}-->![v_s = D V_in](lc-filter.assets/eq-inline/a760bba240.svg)<!--/m--> from §4. In steady state the average current is not drifting, so
the left side is zero:

![L times d i_L bar by dt equals D V_in minus v_out bar; in steady state zero equals D V_in minus V_out](lc-filter.assets/eq-avg-L.svg)

![V_out equals D V_in, boxed](lc-filter.assets/eq-avg-result.svg)

This is volt-second balance — the same argument as the
[buck document §3](../../dc-dc-converters/buck/buck.md#3-volt-second-balance--the-step-down-ratio),
reached by averaging rather than by matching the rise and fall of the current triangle. Notice it says
something stronger than the buck derivation needed: the averaged equations are *linear* in
<!--m:\overline{v_s}-->![v_s](lc-filter.assets/eq-inline/13e7f2d910.svg)<!--/m-->. Whatever you do to <!--m:\overline{v_s}-->![v_s](lc-filter.assets/eq-inline/13e7f2d910.svg)<!--/m-->, the averages of <!--m:i_L-->![i_L](lc-filter.assets/eq-inline/0bd5fa35e8.svg)<!--/m--> and <!--m:v_{out}-->![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--/m--> respond as a
linear circuit driven by it. That is the door to §11.

## 6 The transfer function, derived

Because the averaged circuit is linear, it has a transfer function from the switch-node voltage to
the output. Treat it as a voltage divider in the Laplace domain: the inductor (impedance <!--m:sL-->![sL](lc-filter.assets/eq-inline/1b3b95adda.svg)<!--/m-->) on top,
and the parallel combination of capacitor (impedance <!--m:1/(sC)-->![1/(sC)](lc-filter.assets/eq-inline/1d73a7e900.svg)<!--/m-->) and load <!--m:R-->![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--/m--> on the bottom:

![H of s equals V_out over V_s equals Z_p over Z_p plus sL, where Z_p is R parallel 1 over sC equals R over 1 plus sRC](lc-filter.assets/eq-divider.svg)

Substitute <!--m:Z_p-->![Z_p](lc-filter.assets/eq-inline/7c220bd4ef.svg)<!--/m--> and clear the inner fractions by multiplying top and bottom by <!--m:(1 + sRC)-->![(1 + sRC)](lc-filter.assets/eq-inline/71168ee26f.svg)<!--/m-->:

![H equals R over 1 plus sRC divided by R over 1 plus sRC plus sL, equals R over R plus sL times 1 plus sRC, equals R over s squared RLC plus sL plus R](lc-filter.assets/eq-tf-derive.svg)

Divide top and bottom by <!--m:R-->![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--/m-->:

![H of s equals 1 over s squared LC plus s L over R plus 1, boxed](lc-filter.assets/eq-tf.svg)

This is the standard second-order low-pass. Matching it term by term against the textbook form names
its two parameters — the natural (corner) frequency <!--m:\omega_0-->![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--/m--> and the quality factor <!--m:Q-->![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--/m-->:

![H of s equals 1 over s squared over omega_0 squared plus s over Q omega_0 plus 1, with omega_0 equal to 1 over root LC and Q equal to R root C over L](lc-filter.assets/eq-tf-standard.svg)

![1 over omega_0 squared equals LC, 1 over Q omega_0 equals L over R, so Q equals R over omega_0 L equals R root C over L equals R over Z_0, with Z_0 equal to root L over C](lc-filter.assets/eq-q-derive.svg)

<!--m:Z_0 = \sqrt{L/C}-->![Z_0 = sqrt L/C](lc-filter.assets/eq-inline/2ac38c12fb.svg)<!--/m--> is the filter's *characteristic impedance*. <!--m:Q-->![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--/m--> is simply the load resistance
measured in units of <!--m:Z_0-->![Z_0](lc-filter.assets/eq-inline/964f1c3f3e.svg)<!--/m-->: a heavy load (small <!--m:R-->![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--/m-->) gives a small <!--m:Q-->![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--/m-->, a light load (large <!--m:R-->![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--/m-->) a
large one. Put <!--m:s = j\omega-->![s = j omega](lc-filter.assets/eq-inline/52dfb07b61.svg)<!--/m--> to get the magnitude response:

![magnitude of H of j omega equals 1 over the square root of 1 minus omega squared over omega_0 squared, squared, plus omega over Q omega_0, squared](lc-filter.assets/eq-mag.svg)

Three regimes tell you everything:

![for omega much less than omega_0, H tends to 1; at omega_0, H equals Q; for omega much greater than omega_0, H is about omega_0 over omega squared](lc-filter.assets/eq-limits.svg)

- **Far below <!--m:\omega_0-->![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--/m-->** the gain is 1: slow things pass untouched. DC — the average — is the
  slowest thing there is (<!--m:H(0) = 1-->![H(0) = 1](lc-filter.assets/eq-inline/6d972adbf1.svg)<!--/m--> exactly).
- **At <!--m:\omega_0-->![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--/m-->** the gain is <!--m:Q-->![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--/m-->. For <!--m:Q > 1-->![Q > 1](lc-filter.assets/eq-inline/2fa298f53d.svg)<!--/m--> the filter *amplifies* signals near its resonance —
  the peak in Fig. 60.
- **Far above <!--m:\omega_0-->![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--/m-->** the gain falls as the square of frequency. In decibels:

![20 log of omega_0 over omega squared equals minus 40 log of omega over omega_0 dB, so minus 40 dB per decade](lc-filter.assets/eq-slope.svg)

Every factor of ten in frequency costs a factor of a hundred in amplitude — one factor of ten from the
inductor's rising impedance and one from the capacitor's falling one, as promised in §2.

![Bode magnitude plot of the LC filter for three load resistances, with 50 Hz passed and 50 kHz attenuated by 60 dB](lc-filter.assets/fig-05.svg)

_The same <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m--> with three loads. All three agree far from <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m-->: flat at 50 Hz, a <!--m:-40\,\mathrm{dB}-->![-40 dB](lc-filter.assets/eq-inline/fefc844ddc.svg)<!--/m-->/decade
cliff above. Only near <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> does the load matter — a light load lets the resonance peak up to
<!--m:+12\,\mathrm{dB}-->![+12 dB](lc-filter.assets/eq-inline/4493744e77.svg)<!--/m--> (<!--m:Q = 4-->![Q = 4](lc-filter.assets/eq-inline/233ccb4ea8.svg)<!--/m-->), a heavy one rounds it off._

## 7 Choosing the corner frequency — and the design used here

The filter has one job with two sides: pass the thing you want, reject the switching frequency. So
<!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> must sit **far above the slowest-changing signal you want to keep** and **far below the switching
frequency**. For a DC supply the first condition is free. For the inverter this tree is building
towards, the signal is a <!--m:50\,\mathrm{Hz}-->![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--/m--> sine and the switching frequency is <!--m:50\,\mathrm{kHz}-->![50 kHz](lc-filter.assets/eq-inline/c61affb2d7.svg)<!--/m-->, so:

![f_signal much less than f_0 much less than f_sw; f_0 near the geometric mean of 50 and 50000, about 1.58 kHz](lc-filter.assets/eq-separation.svg)

Placing <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> at the geometric mean gives equal margin on both sides: a factor of about 32 (one and a
half decades) each way. The design used for every figure in this document does exactly that:

![f_0 equals 1 over 2 pi root of 10 to the minus 3 times 10 to the minus 5, about 1.59 kHz; Z_0 equals 10 ohms; Q equals 1](lc-filter.assets/eq-design-num.svg)

with <!--m:V_{in} = 12\,\mathrm{V}-->![V_in = 12 V](lc-filter.assets/eq-inline/9a063a8f8e.svg)<!--/m-->, <!--m:L = 1\,\mathrm{mH}-->![L = 1 mH](lc-filter.assets/eq-inline/5367ea2324.svg)<!--/m-->, <!--m:C = 10\,\mu\mathrm{F}-->![C = 10 mu F](lc-filter.assets/eq-inline/a68d258fbb.svg)<!--/m-->, <!--m:R = 10\,\Omega-->![R = 10 Omega](lc-filter.assets/eq-inline/0b1b42866f.svg)<!--/m-->. Reading the
Bode plot at the two frequencies that matter:

![H at 50 kHz is about 1.59 over 50 squared, about 1.0 times 10 to the minus 3, minus 60 dB; H at 50 Hz is about 1.001, 0 dB](lc-filter.assets/eq-atten-num.svg)

The switching frequency is cut by a factor of a thousand; the <!--m:50\,\mathrm{Hz}-->![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--/m--> signal passes at full
size with only about <!--m:1.8°-->![1.8°](lc-filter.assets/eq-inline/0de2b0765a.svg)<!--/m--> of phase lag. How these particular values of <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m--> were picked from
<!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> and <!--m:Z_0-->![Z_0](lc-filter.assets/eq-inline/964f1c3f3e.svg)<!--/m--> is in §14.

## 8 Ripple — what the filter lets through

A thousand-fold attenuation is not infinite, so something of the switching survives. Compute it two
ways and check that they agree.

**Inductor current ripple.** While the switch is ON the inductor sees <!--m:V_{in} - V_{out} = V_{in}(1-D)-->![V_in - V_out = V_in(1-D)](lc-filter.assets/eq-inline/bf3b705fbc.svg)<!--/m-->
for a time <!--m:DT-->![DT](lc-filter.assets/eq-inline/f91a09d081.svg)<!--/m-->, so its current rises by:

![delta I_L equals V_in minus V_out times DT over L, equals V_in times 1 minus D times D over L f_sw](lc-filter.assets/eq-ripple-I.svg)

![delta I_L equals 12 times 0.5 times 0.5 over 10 to the minus 3 times 50000, equals 3 over 50, equals 0.06 A](lc-filter.assets/eq-ripple-I-num.svg)

(It falls by the same amount while OFF — that is steady state.) <!--m:D(1-D)-->![D(1-D)](lc-filter.assets/eq-inline/3b8ebc31f3.svg)<!--/m--> peaks at <!--m:D = 0.5-->![D = 0.5](lc-filter.assets/eq-inline/a2406f7d12.svg)<!--/m-->, so the
running example is the worst case.

**Capacitor voltage ripple.** The load takes the average current; the capacitor absorbs the triangle's
deviation from it, a triangle centred on zero. The charge in its positive half is <!--m:T\,\Delta I_L/8-->![T Delta I_L/8](lc-filter.assets/eq-inline/a5793fda45.svg)<!--/m--> (the
same triangle-area argument as the
[buck document §5](../../dc-dc-converters/buck/buck.md#5-sizing-the-output-capacitor)), so:

![delta V_C equals delta Q over C equals T delta I_L over 8 over C, equals delta I_L over 8 C f_sw](lc-filter.assets/eq-ripple-V.svg)

![delta V_C equals 0.06 over 8 times 10 to the minus 5 times 50000, equals 0.06 over 4, equals 15 mV](lc-filter.assets/eq-ripple-V-num.svg)

Fifteen millivolts on six volts — a quarter of a percent. Substituting <!--m:\Delta I_L-->![Delta I_L](lc-filter.assets/eq-inline/c856ab20fc.svg)<!--/m--> shows the ripple is
the filter-attenuation story in disguise:

![delta V_C equals V_in D times 1 minus D over 8 LC f_sw squared, equals pi squared over 2 times D times 1 minus D times V_in times f_0 over f_sw squared, at most pi squared over 8 V_in f_0 over f_sw squared](lc-filter.assets/eq-ripple-unified.svg)

The ripple scales as <!--m:(f_0/f_{sw})^2-->![(f_0/f_sw)^2](lc-filter.assets/eq-inline/77fc370b8f.svg)<!--/m--> — exactly the <!--m:-40\,\mathrm{dB}-->![-40 dB](lc-filter.assets/eq-inline/fefc844ddc.svg)<!--/m-->/decade slope evaluated at the
switching frequency. Halve <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> (or double <!--m:f_{sw}-->![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--/m-->) and the ripple drops by four.

![Simulated switch-node voltage, inductor current, output voltage and output ripple for a constant duty cycle of 0.5](lc-filter.assets/fig-03.svg)

_The simulation agrees with the hand calculation to the digit: a <!--m:0.060\,\mathrm{A}-->![0.060 A](lc-filter.assets/eq-inline/5a1efa6c08.svg)<!--/m--> current triangle and a
<!--m:15.0\,\mathrm{mV}-->![15.0 mV](lc-filter.assets/eq-inline/60be7e4951.svg)<!--/m--> voltage ripple. On the <!--m:0-->![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--/m-->–<!--m:12\,\mathrm{V}-->![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--/m--> scale (third panel) the output is a ruler-straight
line; only a thousand-fold zoom (bottom) shows the residue, now nearly a sine because the filter has
stripped the square wave's harmonics._

## 9 Constant D gives flat DC; change D, change the DC

Hold the duty cycle fixed and the output is a straight line at <!--m:D\,V_{in}-->![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--/m--> — the third panel of Fig. 58,
and what the source video shows in its first output trace. Change the fixed value and the line moves:

![Simulated PWM waveforms for duty cycles 0.25, 0.5 and 0.75 and the three flat output voltages 3 V, 6 V and 9 V](lc-filter.assets/fig-04.svg)

_Narrow pulses give a low line, wide pulses a high one: <!--m:D = 0.25, 0.5, 0.75-->![D = 0.25, 0.5, 0.75](lc-filter.assets/eq-inline/c4f063c6c7.svg)<!--/m--> give <!--m:3, 6, 9\,\mathrm{V}-->![3, 6, 9 V](lc-filter.assets/eq-inline/20ea02ba10.svg)<!--/m--> from
a <!--m:12\,\mathrm{V}-->![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--/m--> supply. Every one of them is still DC — the output has no time-variation beyond the
millivolt ripple._

This is already useful — it is a step-down DC supply with an adjustable output — but on its own it is
still just DC. The interesting question is what happens *between* the flat lines, when <!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> changes.

## 10 A step in D — ringing, and damping by the load

Jump <!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> from <!--m:0.25-->![0.25](lc-filter.assets/eq-inline/bdedc3fe49.svg)<!--/m--> to <!--m:0.75-->![0.75](lc-filter.assets/eq-inline/0cf1aeac03.svg)<!--/m-->. The average of the switch node jumps from <!--m:3\,\mathrm{V}-->![3 V](lc-filter.assets/eq-inline/2c9b84f130.svg)<!--/m--> to <!--m:9\,\mathrm{V}-->![9 V](lc-filter.assets/eq-inline/fe37df33fd.svg)<!--/m-->
instantly, but the output cannot: the capacitor's voltage can only rise as fast as the inductor's current
lets it, and that current can only rise as fast as <!--m:V_{in}/L-->![V_in/L](lc-filter.assets/eq-inline/703b9e93b8.svg)<!--/m--> allows. The output follows the
second-order step response of <!--m:H(s)-->![H(s)](lc-filter.assets/eq-inline/42687f71af.svg)<!--/m-->:

![overshoot equals e to the minus pi over root of 4 Q squared minus 1; decay envelope e to the minus t over tau, with tau equal to 2Q over omega_0 equal to 2RC](lc-filter.assets/eq-overshoot.svg)

![Q equal 1: about 16 percent overshoot, tau 0.2 ms; Q equal 4: about 67 percent overshoot, tau 0.8 ms](lc-filter.assets/eq-overshoot-num.svg)

![Simulated output voltage after a duty-cycle step from 0.25 to 0.75 and back, for three load resistances](lc-filter.assets/fig-06.svg)

_Same filter, three loads. At <!--m:Q = 1-->![Q = 1](lc-filter.assets/eq-inline/24fdb68929.svg)<!--/m--> (blue) the output overshoots to about <!--m:10\,\mathrm{V}-->![10 V](lc-filter.assets/eq-inline/a03ac5878a.svg)<!--/m--> and settles
within a millisecond. The light load (red, <!--m:Q = 4-->![Q = 4](lc-filter.assets/eq-inline/233ccb4ea8.svg)<!--/m-->) overshoots to about <!--m:13\,\mathrm{V}-->![13 V](lc-filter.assets/eq-inline/3606f977ee.svg)<!--/m--> — above the
<!--m:12\,\mathrm{V}-->![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--/m--> supply — and rings for several milliseconds. The heavy load (green) never overshoots but
is slow._

Three things to take from it:

- **The load resistor is the only damper.** An ideal <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m--> trade energy back and forth forever;
  the resistor is what bleeds that oscillation away, with time constant <!--m:2RC-->![2RC](lc-filter.assets/eq-inline/3f61238eeb.svg)<!--/m-->. Remove the load and the
  filter would ring indefinitely.
- **The output can exceed the supply.** Energy stored in the inductor during the rise keeps pushing
  charge into the capacitor after it has reached the target. A light-load design must survive that
  overshoot.
- **The diode makes the downward step asymmetric.** On the fall (at <!--m:3.5\,\mathrm{ms}-->![3.5 ms](lc-filter.assets/eq-inline/0270066b38.svg)<!--/m-->) the light-load
  ringing is visibly shorter than on the rise. Ringing needs the inductor current to swing negative,
  and the diode refuses to conduct backwards; the current sits at zero for part of each cycle
  (*discontinuous conduction*), which damps the oscillation. The simulator models this; an idealised
  linear analysis would not.

The practical upshot: the filter needs a few resonant periods (<!--m:1/f_0 \approx 0.63\,\mathrm{ms}-->![1/f_0 approx 0.63 ms](lc-filter.assets/eq-inline/8f40f108bf.svg)<!--/m--> each) to
follow a sudden change. It is not instantaneous — and that is the speed limit of §12.

## 11 The big idea — vary D cycle by cycle

Nothing says <!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> has to be the same in every switching period. The switch controller can choose a new
duty cycle for each period — narrow pulses here, wide ones there. Ramp <!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> up over many periods and the
local average of the switch node ramps up with it; the filter, which passes slow things and rejects the
switching, delivers that slowly-moving average to the load:

![Simulated PWM whose duty cycle ramps up then down, and the filtered output following D times V_in](lc-filter.assets/fig-07.svg)

_Pulses grow from a sliver to nearly full width and back. The output (solid) follows the local average
<!--m:D(t)\,V_{in}-->![D(t) V_in](lc-filter.assets/eq-inline/dfb6a6490e.svg)<!--/m--> (dashed) up and back down. The switching is slowed to <!--m:10\,\mathrm{kHz}-->![10 kHz](lc-filter.assets/eq-inline/b573bbc290.svg)<!--/m--> here so every pulse
can be seen; that is why the ripple is visible and the lag is noticeable._

This is the linearity of §5 paying off. The averaged circuit is linear and driven by
<!--m:\overline{v_s}(t) = D(t)\,V_{in}-->![v_s(t) = D(t) V_in](lc-filter.assets/eq-inline/a5eb53ec9a.svg)<!--/m-->, so the averaged output is that input passed through <!--m:H(s)-->![H(s)](lc-filter.assets/eq-inline/42687f71af.svg)<!--/m-->:

![V_out bar of s equals H of s times V_in times D of s; so v_out bar of t is about D of t times V_in when D varies well below f_0](lc-filter.assets/eq-tracking.svg)

For a ramp, the filter's finite speed shows up as a constant delay — the output runs parallel to the
target, a fixed time behind it:

![D equals k t gives v_out bar tending to k V_in times t minus 1 over Q omega_0, equals k V_in times t minus L over R](lc-filter.assets/eq-ramp-lag.svg)

<!--m:L/R = 0.1\,\mathrm{ms}-->![L/R = 0.1 ms](lc-filter.assets/eq-inline/680a8183e3.svg)<!--/m--> here, plus about half a switching period because each period's duty cycle is
decided at its start — together the visible gap in Fig. 62.

The ramp is just one shape. Make <!--m:D(t)-->![D(t)](lc-filter.assets/eq-inline/a6f14a1480.svg)<!--/m--> a sinusoid and the output is a sinusoid:

![Simulated PWM with a sinusoidally varying duty cycle and the filtered output, a sine wave that stays between 0 and V_in](lc-filter.assets/fig-08.svg)

_A sinusoidal duty-cycle pattern produces a sinusoidal output. Its centre is <!--m:V_{in}/2-->![V_in/2](lc-filter.assets/eq-inline/e4314a28a2.svg)<!--/m-->, its peaks
approach <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m--> and <!--m:0-->![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--/m--> — never beyond. At <!--m:500\,\mathrm{Hz}-->![500 Hz](lc-filter.assets/eq-inline/bfa338a3df.svg)<!--/m--> (a third of <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m-->) the filter's phase lag is
already visible._

Now do it with realistic numbers — a <!--m:50\,\mathrm{Hz}-->![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--/m--> pattern on a <!--m:50\,\mathrm{kHz}-->![50 kHz](lc-filter.assets/eq-inline/c61affb2d7.svg)<!--/m--> carrier, a thousand
switching periods per output cycle:

![D of t equals 0.5 plus 0.4 sin 2 pi 50 t, so v_out of t is about 6 plus 4.8 sin 2 pi 50 t volts](lc-filter.assets/eq-sine.svg)

![Simulated 50 Hz output synthesised from 50 kHz PWM, compared with the bipolar sine an AC load needs](lc-filter.assets/fig-09.svg)

_Top: two <!--m:200\,\mu\mathrm{s}-->![200 mu s](lc-filter.assets/eq-inline/3633d1d3b3.svg)<!--/m--> windows of the switch node — wide pulses near the crest, slivers near the
trough. Bottom: the simulated output (blue) lies on top of <!--m:D(t)\,V_{in}-->![D(t) V_in](lc-filter.assets/eq-inline/dfb6a6490e.svg)<!--/m--> (dashed amber); with three
decades between the signal and the switching, the ripple and lag are invisible. But it is a sine around
<!--m:6\,\mathrm{V}-->![6 V](lc-filter.assets/eq-inline/8ed5af7660.svg)<!--/m-->, not around zero (red: what an AC load needs)._

This is the central idea of every modern inverter, motor drive and class-D amplifier: **a switch that is
only ever fully on or fully off, plus an LC filter, behaves like a voltage source you can program in
time**. The pattern of duty cycles *is* the waveform; the filter just erases the switching. How to
generate that pattern — comparing a sine against a triangle carrier — is
[sinusoidal PWM](../../dc-ac-inverters/spwm/); the general machinery of duty cycle and carriers is in
[pwm/](../../pwm/).

## 12 How fast may D change?

The phrase "well below <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m-->" in §11 carries the whole caveat. The filter cannot tell the difference
between a fast change of <!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> that you *wanted* and switching ripple that you didn't — both are just
high-frequency content, and both are attenuated alike. The phase of <!--m:H-->![H](lc-filter.assets/eq-inline/7cf184f4c6.svg)<!--/m--> shows how the delay grows:

![phi of f equals minus arctan of f over f_0 over Q, over 1 minus f over f_0 squared; phi at 50 Hz is about minus 1.8 degrees, phi at f_0 is minus 90 degrees](lc-filter.assets/eq-phase.svg)

![Simulated output for sinusoidal duty cycles at a tenth of f0, at f0 and at three times f0, compared with D times V_in](lc-filter.assets/fig-10.svg)

_The same duty-cycle sine at three speeds. At a tenth of <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> the output tracks it. At <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> the output is
a quarter-cycle late (and, with a lighter load, would also be <!--m:Q-->![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--/m--> times too big). At three times <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> the
filter treats the wanted signal as ripple and removes almost all of it._

So there are two separate conditions for <!--m:v_{out}(t) \approx D(t)\,V_{in}-->![v_out(t) approx D(t) V_in](lc-filter.assets/eq-inline/5cc5b56e89.svg)<!--/m-->:

1. **<!--m:D-->![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> must change slowly compared with <!--m:f_{sw}-->![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--/m-->** — many switching periods per feature of the waveform —
   or there is no meaningful "local average" to follow.
2. **The waveform's frequency content must sit well below <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m-->** — or the filter itself distorts it.

Both are the same separation, <!--m:f_{signal} \ll f_0 \ll f_{sw}-->![f_signal much less than f_0 much less than f_sw](lc-filter.assets/eq-inline/a6c0685add.svg)<!--/m-->, read from the two ends.

## 13 One switch, one polarity — the road to the inverter

Every output in this document stays between <!--m:0-->![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--/m--> and <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m-->, and it cannot do otherwise. The switch node
only ever takes the values <!--m:V_{in}-->![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--/m--> and <!--m:0-->![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--/m-->, and the duty cycle is a fraction:

![0 at most D of t at most 1, so 0 at most v_out bar at most V_in](lc-filter.assets/eq-unipolar.svg)

A single switch can make any *slow, one-polarity* waveform — a DC level, a ramp, a sine riding on an
offset — but never a voltage below zero. Mains-style AC must swing both ways (red trace, Fig. 64).
Blocking the offset with a series capacitor is no answer for <!--m:50\,\mathrm{Hz}-->![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--/m--> power: the capacitor
would be enormous and the offset still wastes half the supply's range.

The fix is to make the switch node itself bipolar. An
[H-bridge](../../dc-ac-inverters/h-bridge/) of four switches can connect the load either way round
across the supply, so the voltage between its two output nodes takes the values <!--m:+V_{in}-->![+V_in](lc-filter.assets/eq-inline/088e3e6d5f.svg)<!--/m--> and
<!--m:-V_{in}-->![-V_in](lc-filter.assets/eq-inline/b15a60dd4f.svg)<!--/m-->. Averaging that wave exactly as in §4:

![v_AB in plus V_in, minus V_in, so v_AB bar equals D V_in minus 1 minus D V_in equals 2D minus 1 times V_in, between minus V_in and plus V_in](lc-filter.assets/eq-bipolar.svg)

Now <!--m:D = 0.5-->![D = 0.5](lc-filter.assets/eq-inline/a2406f7d12.svg)<!--/m--> gives zero, <!--m:D > 0.5-->![D > 0.5](lc-filter.assets/eq-inline/37b1b71070.svg)<!--/m--> positive and <!--m:D < 0.5-->![D < 0.5](lc-filter.assets/eq-inline/53408dfd7a.svg)<!--/m--> negative output, and the very same LC filter
and the very same cycle-by-cycle trick produce a true AC sine. That is the inverter, assembled from the
three ideas of this document: PWM average, LC filtering, and a duty cycle that moves.

## 14 What this costs you

- **Size and weight.** The filter's <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m--> must store enough energy to bridge a switching period at
  full load. At low switching frequency they are big; the <!--m:(f_0/f_{sw})^2-->![(f_0/f_sw)^2](lc-filter.assets/eq-inline/77fc370b8f.svg)<!--/m--> ripple law is why designers
  chase higher <!--m:f_{sw}-->![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--/m--> — which in turn costs switching loss in the transistor and diode.
- **Sizing is a two-number choice.** <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> fixes the product <!--m:LC-->![LC](lc-filter.assets/eq-inline/3b0e58d439.svg)<!--/m-->; the split between them is set by
  <!--m:Z_0 = \sqrt{L/C}-->![Z_0 = sqrt L/C](lc-filter.assets/eq-inline/2ac38c12fb.svg)<!--/m-->, which should be comparable to the load so that <!--m:Q-->![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--/m--> lands near 1. Given <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> and
  <!--m:Z_0-->![Z_0](lc-filter.assets/eq-inline/964f1c3f3e.svg)<!--/m-->:

  ![L equals Z_0 over omega_0 and C equals 1 over omega_0 Z_0, which with Z_0 of 10 ohms and omega_0 of 10 to the 4 gives 1 mH and 10 microfarads](lc-filter.assets/eq-sizing.svg)

  This is how the running example was chosen: <!--m:\omega_0 = 10^4\,\mathrm{rad/s}-->![omega_0 = 10^4 rad/s](lc-filter.assets/eq-inline/76fc228490.svg)<!--/m--> (<!--m:f_0 \approx 1.59\,\mathrm{kHz}-->![f_0 approx 1.59 kHz](lc-filter.assets/eq-inline/71874cd5e1.svg)<!--/m-->,
  the geometric mean of <!--m:50\,\mathrm{Hz}-->![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--/m--> and <!--m:50\,\mathrm{kHz}-->![50 kHz](lc-filter.assets/eq-inline/c61affb2d7.svg)<!--/m-->) and <!--m:Z_0 = R = 10\,\Omega-->![Z_0 = R = 10 Omega](lc-filter.assets/eq-inline/3d9818d8a6.svg)<!--/m--> for <!--m:Q = 1-->![Q = 1](lc-filter.assets/eq-inline/24fdb68929.svg)<!--/m-->.
  A bigger <!--m:L-->![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--/m--> with a smaller <!--m:C-->![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--/m--> lowers the ripple current but makes the filter softer (output sags more
  under sudden load steps); the reverse needs a capacitor that can carry more ripple current.
- **Resonance and load dependence.** <!--m:Q = R/Z_0-->![Q = R/Z_0](lc-filter.assets/eq-inline/73b00947fa.svg)<!--/m--> depends on the load, which the designer does not
  control. At light load <!--m:Q-->![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--/m--> grows, the response peaks at <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> (Fig. 60) and steps ring and overshoot
  above the supply (Fig. 61). At no load an ideal filter would not damp at all. Real designs add damping
  (a resistor in series with an extra capacitor across the output, or active damping in the control
  loop) and rely on feedback rather than trusting <!--m:v_{out} = D\,V_{in}-->![v_out = D V_in](lc-filter.assets/eq-inline/b09da4744e.svg)<!--/m--> open-loop.
- **A speed limit.** The output can only follow <!--m:D(t)-->![D(t)](lc-filter.assets/eq-inline/a6f14a1480.svg)<!--/m--> for frequencies well below <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m-->, and <!--m:f_0-->![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--/m--> must
  stay well below <!--m:f_{sw}-->![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--/m-->. A <!--m:50\,\mathrm{Hz}-->![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--/m--> inverter is comfortable; synthesising a <!--m:20\,\mathrm{kHz}-->![20 kHz](lc-filter.assets/eq-inline/bbc70d4c7e.svg)<!--/m-->
  audio signal needs a switching frequency in the hundreds of kilohertz.
- **Losses and non-idealities.** The inductor's winding resistance, the capacitor's ESR (which adds its own
  ripple term, often larger than the <!--m:15\,\mathrm{mV}-->![15 mV](lc-filter.assets/eq-inline/e53cb2e3b3.svg)<!--/m--> computed here), the diode's forward drop (so
  <!--m:v_{out}-->![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--/m--> sits slightly below <!--m:D\,V_{in}-->![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--/m-->), and the diode's refusal to conduct backwards at light load
  (discontinuous conduction, §10), which breaks the linear model. Synchronous rectification — a second
  transistor in place of the diode — removes the drop and the discontinuity at the cost of gate-drive
  complexity.
- **One polarity only.** A single switch with an LC filter cannot go negative (§13); AC needs the
  four-switch H-bridge and twice the transistors.

## 15 Sources and cross-links

- **The same circuit as a DC-DC converter:** [../../dc-dc-converters/buck/](../../dc-dc-converters/buck/)
  — volt-second balance, inductor and capacitor sizing; its start-up behaviour is in
  [../../dc-dc-converters/buck/startup.md](../../dc-dc-converters/buck/startup.md).
- **The two laws:** [../../fundamentals/inductor/](../../fundamentals/inductor/) (<!--m:v_L = L\,di/dt-->![v_L = L di/dt](lc-filter.assets/eq-inline/6e2526f84b.svg)<!--/m-->, the
  inductive kick) and [../../fundamentals/capacitor/](../../fundamentals/capacitor/) (<!--m:i_C = C\,dv/dt-->![i_C = C dv/dt](lc-filter.assets/eq-inline/20002f7e38.svg)<!--/m-->,
  the duality table).
- **Frequency, phase and decibels:** [../../fundamentals/signals/](../../fundamentals/signals/).
- **Where this leads:** [../../pwm/](../../pwm/) (duty cycle and carriers),
  [../../dc-ac-inverters/spwm/](../../dc-ac-inverters/spwm/) (sinusoidal PWM — the <!--m:D(t)-->![D(t)](lc-filter.assets/eq-inline/a6f14a1480.svg)<!--/m--> pattern of §11)
  and [../../dc-ac-inverters/h-bridge/](../../dc-ac-inverters/h-bridge/) (the bipolar switch node of §13).
- **Source material:** the walkthrough this note expands (a tutorial video on DC-to-AC conversion, PWM and
  LC filtering, around 9:30–10:20). Every waveform here is re-derived and re-simulated rather than copied:
  the simulator is `toolchain/figures/lc_filter.js`, which integrates the equations of §5 with exact
  switching edges and an ideal diode.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
