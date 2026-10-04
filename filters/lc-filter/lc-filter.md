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
> ![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->, controlling ![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> cycle by cycle controls the output voltage as a function of time.

---

## 1 The circuit, and why it is the buck converter

![LC filter schematic: switch, freewheel diode, series inductor, shunt capacitor and load resistor — the buck converter](lc-filter.assets/fig-01.svg)

_Switch, freewheel diode, series inductor, shunt capacitor, load. Draw a dashed box around the
inductor and capacitor and call it a filter, or step back and call the whole thing a buck
converter — it is the same circuit, part for part._

Five parts, in order from the source:

- **The switch ![S](lc-filter.assets/eq-inline/02aa629c8b.svg)<!--m:S-->** (in practice a MOSFET) connects the supply ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> to the *switch node*
  ![v_s](lc-filter.assets/eq-inline/85026ab589.svg)<!--m:v_s-->, or disconnects it. It is driven by PWM at a fixed switching frequency ![f_sw = 1/T](lc-filter.assets/eq-inline/cc2c6208db.svg)<!--m:f_{sw} = 1/T-->;
  the fraction of each period it is ON (closed) is the duty cycle ![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D-->.
- **The freewheel diode ![D_f](lc-filter.assets/eq-inline/f93c81428b.svg)<!--m:D_f-->** runs from ground (anode) up to the switch node (cathode). It does
  nothing while the switch is ON (it is reverse-biased by ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}-->) and carries the inductor's
  current while the switch is OFF (§3).
- **The inductor ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L-->** sits *in series*: every bit of current that reaches the load passes through
  it.
- **The capacitor ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C-->** sits *in parallel* (shunt) with the load: it sees exactly the output voltage.
- **The load ![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--m:R-->** — whatever is being powered, modelled here as a resistor.

The switch node ![v_s](lc-filter.assets/eq-inline/85026ab589.svg)<!--m:v_s--> is therefore a square wave between ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> (switch ON) and about ![0 V](lc-filter.assets/eq-inline/23534506b4.svg)<!--m:0\,\mathrm{V}-->
(switch OFF, diode conducting) — a PWM wave. The inductor and capacitor are the **LC filter**, and
the voltage across the load is ![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--m:v_{out}-->.

If this looks familiar, it should: it is **exactly the [buck converter](../../dc-dc-converters/buck/)**,
with the same parts in the same order. The buck document derives ![V_out = D V_in](lc-filter.assets/eq-inline/ee5553b7dd.svg)<!--m:V_{out} = D\,V_{in}--> from
volt-second balance on the inductor; this document reaches the same result from the other side —
by treating the ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L--> and ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C--> as a *filter* that keeps the average of the PWM wave and throws away
the rest. The two views are the same physics, and holding both in your head is what makes the next
step obvious: if the filter keeps the *average*, and the average is something you control, then
you control the output — not just its DC level but its shape over time.

> **Note —** Many tutorials (including the source video for this note) label the load voltage
> ![V_L](lc-filter.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L-->. Here it is ![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--m:v_{out}-->, because ![v_L](lc-filter.assets/eq-inline/644bea706b.svg)<!--m:v_L--> is reserved for the voltage *across the inductor* in
> ![v_L = L di_L/dt](lc-filter.assets/eq-inline/7e61c8ca55.svg)<!--m:v_L = L\,di_L/dt-->. Lower-case letters are instantaneous values; an overbar (![v_s](lc-filter.assets/eq-inline/13e7f2d910.svg)<!--m:\overline{v_s}-->)
> means the average over one switching period.

## 2 Two laws, read as smoothing rules

The whole filter is two component laws, each read as a statement about what that part *refuses to
let happen quickly*. Start with the inductor:

![v_L equals L di_L by dt, so the magnitude of di_L by dt equals v_L over L, at most V_in over L](lc-filter.assets/eq-law-L.svg)

Read it backwards. The inductor's *voltage* can jump around however it likes — in this circuit it
jumps every time the switch flips — but its *current* can only change at a rate set by that
voltage. Since nothing in the circuit is bigger than ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}-->, the current's slope is **bounded**.
With the numbers used throughout this document (![V_in = 12 V](lc-filter.assets/eq-inline/9a063a8f8e.svg)<!--m:V_{in} = 12\,\mathrm{V}-->, ![L = 1 mH](lc-filter.assets/eq-inline/5367ea2324.svg)<!--m:L = 1\,\mathrm{mH}-->):

![V_in over L equals 12 volts over 1 millihenry equals 12000 amps per second, 0.012 amps per microsecond](lc-filter.assets/eq-slope-L-num.svg)

In one ![20 mu s](lc-filter.assets/eq-inline/f0d5326bb0.svg)<!--m:20\,\mu\mathrm{s}--> switching period the current can move by at most a quarter of an amp, no
matter how violently the voltage across it jumps. That is what "an inductor smooths the current"
means precisely: **a voltage jump becomes a current slope**. The square wave on its left becomes a
gentle triangle of current through it (Fig. 58).

Now the capacitor — the same law with voltage and current swapped:

![i_C equals C dv_C by dt, so the magnitude of dv_C by dt equals i_C over C](lc-filter.assets/eq-law-C.svg)

The capacitor's *current* can jump around, but its *voltage* can only change at a rate set by that
current. Feed it a triangle of current and its voltage barely moves: **a current jump becomes a
voltage slope**. That is "a capacitor smooths the voltage".

Then notice *where* each part is placed, because placement is what makes each law useful:

Both bullets below use **impedance**, the AC version of resistance: for a sine wave of angular
frequency ![omega = 2 pi f](lc-filter.assets/eq-inline/10f7ad86c0.svg)<!--m:\omega = 2\pi f-->, the ratio of a part's voltage amplitude to its current amplitude. The
two laws give it at once. A sine current of amplitude ![I](lc-filter.assets/eq-inline/48786abc01.svg)<!--m:\hat I--> through the inductor makes
![v_L = L di_L/dt](lc-filter.assets/eq-inline/7e61c8ca55.svg)<!--m:v_L = L\,di_L/dt--> of amplitude ![omega L I](lc-filter.assets/eq-inline/90fb26c529.svg)<!--m:\omega L\hat I-->, so ![|Z_L| = omega L](lc-filter.assets/eq-inline/3a906b535f.svg)<!--m:|Z_L| = \omega L-->; a sine voltage of amplitude
![V](lc-filter.assets/eq-inline/ec34d75919.svg)<!--m:\hat V--> on the capacitor makes ![i_C = C dv_C/dt](lc-filter.assets/eq-inline/4f14dbaf62.svg)<!--m:i_C = C\,dv_C/dt--> of amplitude ![omega C V](lc-filter.assets/eq-inline/2f8d11e727.svg)<!--m:\omega C\hat V-->, so
![|Z_C| = 1/( omega C)](lc-filter.assets/eq-inline/bafe731c11.svg)<!--m:|Z_C| = 1/(\omega C)--> (the same argument as
[../../fundamentals/signals/edges-and-fourier.md §9](../../fundamentals/signals/edges-and-fourier.md#9-why-the-harmonics-matter-downstream);
§6 adds the timing).

- **The inductor is in series** — in the path of the current. A series element controls what
  *flows through* it, so a part that refuses fast changes of current, placed in series, makes the
  current delivered downstream smooth. At high frequency its impedance ![|Z_L| = omega L](lc-filter.assets/eq-inline/3a906b535f.svg)<!--m:|Z_L| = \omega L--> is large:
  it *blocks* the switching frequency.
- **The capacitor is in parallel** — across the output. A shunt element controls the voltage
  *across* it, so a part that refuses fast changes of voltage, placed in shunt, holds the output
  voltage steady. At high frequency its impedance ![|Z_C| = 1/( omega C)](lc-filter.assets/eq-inline/bafe731c11.svg)<!--m:|Z_C| = 1/(\omega C)--> is small: it *shorts* what
  is left of the switching frequency to ground.

The pair works as a two-stage voltage divider whose ratio collapses at high frequency from both
ends at once — the inductor's impedance rises while the capacitor's falls. Each contributes a factor
of ![omega](lc-filter.assets/eq-inline/73b077a63e.svg)<!--m:\omega-->, which is where the ![-40 dB](lc-filter.assets/eq-inline/fefc844ddc.svg)<!--m:-40\,\mathrm{dB}--> per decade of §6 comes from.

> **Tip —** This duality is the same table as in the
> [capacitor document §6](../../fundamentals/capacitor/capacitor.md#6-the-duality--one-table-read-both-ways):
> swap ![V I](lc-filter.assets/eq-inline/4ebcd6796a.svg)<!--m:V \leftrightarrow I-->, ![L C](lc-filter.assets/eq-inline/6cef383173.svg)<!--m:L \leftrightarrow C-->, and series ![](lc-filter.assets/eq-inline/eb753a72a0.svg)<!--m:\leftrightarrow--> parallel, and
> each statement about one part becomes the statement about the other.

## 3 Why the diode has to be there

The inductor's stiffness is a gift while the switch is ON and a threat the instant it opens. At that
moment the inductor is carrying roughly the load current — ![0.6 A](lc-filter.assets/eq-inline/90937bd2dc.svg)<!--m:0.6\,\mathrm{A}--> in the running example —
and the law in §2 says that current cannot change instantly. If the switch were the only path, opening
it would force the current from ![0.6 A](lc-filter.assets/eq-inline/90937bd2dc.svg)<!--m:0.6\,\mathrm{A}--> to zero in the few nanoseconds the switch takes to
open. The law then demands:

![v_L equals L times delta i_L over delta t, 1 millihenry times minus 0.6 amps over 10 nanoseconds, equals minus 60000 volts](lc-filter.assets/eq-kick.svg)

Sixty thousand volts across the inductor, appearing at the switch node with a negative sign. In
practice the switch breaks down and arcs long before that — this is the *inductive kick* described in
the [inductor document §7](../../fundamentals/inductor/inductor.md#7-the-inductive-kick-and-why-the-diode-is-there),
and it destroys transistors.

The freewheel diode removes the problem without being told to. As soon as the switch opens, the
inductor starts dragging the switch-node voltage down; the moment it dips just below ground, the
diode becomes forward-biased and gives the current a path: ground → diode → inductor → load →
ground. The node is clamped near ![0 V](lc-filter.assets/eq-inline/23534506b4.svg)<!--m:0\,\mathrm{V}--> (one diode drop below, in reality), the inductor
sees a bounded voltage ![v_L = 0 - v_out](lc-filter.assets/eq-inline/d778776934.svg)<!--m:v_L = 0 - v_{out}-->, and its current ramps *down* gently instead of being
chopped off. The green arrow in Fig. 56 is this freewheeling path. The current never stops — only its
slope changes, from rising to falling.

There is a mirror-image reason the capacitor must not come *first*. Put ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C--> directly on the switch
node with no inductor, and every time the switch closes it connects a stiff ![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--m:12\,\mathrm{V}--> source
straight across a capacitor sitting at, say, ![6 V](lc-filter.assets/eq-inline/8ed5af7660.svg)<!--m:6\,\mathrm{V}-->:

![i_C equals C dv_C by dt; v_C jumps from 6 to 12 volts in zero time, so i_C goes to infinity](lc-filter.assets/eq-cap-kick.svg)

That is a current spike limited only by stray resistance — the capacitor's version of the inductive
kick. The inductor in front prevents it: it is the series element that turns the source's voltage step
into a bounded current ramp *before* the current reaches the capacitor. So the order is not arbitrary:
**switch, then diode, then series ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L-->, then shunt ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C-->** is the one arrangement in which neither part is
ever asked to do the impossible.

## 4 The average of a PWM wave

Before asking what the filter does, pin down what it is being fed. The switch node spends ![DT](lc-filter.assets/eq-inline/f91a09d081.svg)<!--m:DT--> at
![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> and ![(1-D) T](lc-filter.assets/eq-inline/e399568987.svg)<!--m:(1-D)\,T--> at ![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--m:0-->. Its average over one period is, by definition, the integral of the
waveform divided by the period:

![v_s bar equals 1 over T times the integral from 0 to T of v_s dt, which splits into V_in over 0 to DT plus zero over DT to T, giving D V_in](lc-filter.assets/eq-avg-def.svg)

The integral of a rectangle is its area. Only the ON part has any area, ![V_in times DT](lc-filter.assets/eq-inline/05ce01dfde.svg)<!--m:V_{in} \times DT-->, and
spreading that area over the whole period ![T](lc-filter.assets/eq-inline/c2c53d6694.svg)<!--m:T--> gives a level of ![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->. Geometrically: the tall
thin rectangle and the short wide one in Fig. 57 have the same area.

![PWM switch-node voltage with the ON area shaded and its average D V_in drawn as a dashed line](lc-filter.assets/fig-02.svg)

_The shaded ON rectangle (height ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}-->, width ![DT](lc-filter.assets/eq-inline/f91a09d081.svg)<!--m:DT-->) and the blue average rectangle (height ![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->,
width ![T](lc-filter.assets/eq-inline/c2c53d6694.svg)<!--m:T-->) hold the same area. An averaging filter cannot tell them apart._

So a PWM wave is a DC level ![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}--> **plus** a zero-average wobble at the switching frequency
and its harmonics. If the filter can keep the first and discard the second, the output is ![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->.
The rest of the document is about how well it does each.

## 5 The circuit equations, and why the filter keeps the average

Write one law for each energy-storing part. The inductor sits between the switch node and the output,
so its voltage is the difference between them; the capacitor sits at the output, and its current is
whatever the inductor delivers minus what the load takes:

![L di_L by dt equals v_s of t minus v_out of t](lc-filter.assets/eq-ode-L.svg)

![C dv_out by dt equals i_L of t minus v_out of t over R](lc-filter.assets/eq-ode-C.svg)

These two equations, with ![v_s](lc-filter.assets/eq-inline/85026ab589.svg)<!--m:v_s--> jumping between ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> and ![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--m:0--> (and the diode stopping ![i_L](lc-filter.assets/eq-inline/0bd5fa35e8.svg)<!--m:i_L--> from
going negative), are *exactly* what the simulator behind every waveform figure here integrates — the
figures are numerical solutions of these equations, not sketches.

Now average the inductor equation over one switching period. The left side becomes ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L--> times the
rate of change of the *average* current, and the right side becomes ![v_s - v_out](lc-filter.assets/eq-inline/0e1b5020c6.svg)<!--m:\overline{v_s} - \overline{v_{out}}-->.
We know ![v_s = D V_in](lc-filter.assets/eq-inline/a760bba240.svg)<!--m:\overline{v_s} = D\,V_{in}--> from §4. In steady state the average current is not drifting, so
the left side is zero:

![L times d i_L bar by dt equals D V_in minus v_out bar; in steady state zero equals D V_in minus V_out](lc-filter.assets/eq-avg-L.svg)

![V_out equals D V_in, boxed](lc-filter.assets/eq-avg-result.svg)

This is volt-second balance — the same argument as the
[buck document §3](../../dc-dc-converters/buck/buck.md#3-volt-second-balance--the-step-down-ratio),
reached by averaging rather than by matching the rise and fall of the current triangle. Notice it says
something stronger than the buck derivation needed: the averaged equations are *linear* in
![v_s](lc-filter.assets/eq-inline/13e7f2d910.svg)<!--m:\overline{v_s}-->. Whatever you do to ![v_s](lc-filter.assets/eq-inline/13e7f2d910.svg)<!--m:\overline{v_s}-->, the averages of ![i_L](lc-filter.assets/eq-inline/0bd5fa35e8.svg)<!--m:i_L--> and ![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--m:v_{out}--> respond as a
linear circuit driven by it. That is the door to §11.

## 6 The transfer function, derived

Because the averaged circuit is linear, it has a **transfer function** ![H](lc-filter.assets/eq-inline/7cf184f4c6.svg)<!--m:H-->: the ratio of output to
input for each frequency. The tool that gets it in three lines is the *Laplace variable* ![s](lc-filter.assets/eq-inline/a0f1490a20.svg)<!--m:s-->. Here
is all of it that this document needs.

- **Derivatives become multiplication.** For a signal that varies as ![e^st](lc-filter.assets/eq-inline/0e06863538.svg)<!--m:e^{st}-->, ![d/dt](lc-filter.assets/eq-inline/9560a2e5f1.svg)<!--m:d/dt--> just multiplies
  it by ![s](lc-filter.assets/eq-inline/a0f1490a20.svg)<!--m:s-->: ![d over dt e^st = s e^st](lc-filter.assets/eq-inline/715981fe6d.svg)<!--m:\tfrac{d}{dt}e^{st} = s\,e^{st}-->. A steady sine is the case ![s = j omega](lc-filter.assets/eq-inline/52dfb07b61.svg)<!--m:s = j\omega-->, where
  ![j = sqrt -1](lc-filter.assets/eq-inline/63717dd03d.svg)<!--m:j = \sqrt{-1}-->, because Euler's formula ![e^j omega t = cos omega t + j sin omega t](lc-filter.assets/eq-inline/e86c4365c8.svg)<!--m:e^{j\omega t} = \cos\omega t + j\sin\omega t--> packs a
  cosine and a sine into one exponential (the physical signal is the real part). The full Laplace
  transform extends this to any signal, but the rule is the same.
- **So every part has an impedance ![Z = V/I](lc-filter.assets/eq-inline/6c4b7b1bc9.svg)<!--m:Z = V/I-->, a plain algebraic factor.** The inductor law
  ![v = L di/dt](lc-filter.assets/eq-inline/169359fd71.svg)<!--m:v = L\,di/dt--> becomes ![V = sL I](lc-filter.assets/eq-inline/17bc31129d.svg)<!--m:V = sL\,I-->, so ![Z_L = sL](lc-filter.assets/eq-inline/d073d44e10.svg)<!--m:Z_L = sL-->. The capacitor law ![i = C dv/dt](lc-filter.assets/eq-inline/7f5f5ec54b.svg)<!--m:i = C\,dv/dt--> becomes
  ![I = sC V](lc-filter.assets/eq-inline/4437b456c9.svg)<!--m:I = sC\,V-->, so ![Z_C = 1/(sC)](lc-filter.assets/eq-inline/cecf0a6863.svg)<!--m:Z_C = 1/(sC)-->. A resistor is ![Z_R = R](lc-filter.assets/eq-inline/4ca3970103.svg)<!--m:Z_R = R-->. At ![s = j omega](lc-filter.assets/eq-inline/52dfb07b61.svg)<!--m:s = j\omega--> their sizes are the
  ![omega L](lc-filter.assets/eq-inline/b3beb438d7.svg)<!--m:\omega L--> and ![1/( omega C)](lc-filter.assets/eq-inline/a157b00b7e.svg)<!--m:1/(\omega C)--> of §2, and the ![j](lc-filter.assets/eq-inline/5c2dd944dd.svg)<!--m:j--> records the quarter-cycle shift.
- **Impedances combine exactly like resistances,** because Kirchhoff's laws are unchanged: in series
  they add; in parallel, ![Z_1 Z_2 = Z_1 Z_2/(Z_1 + Z_2)](lc-filter.assets/eq-inline/f78cf5be5c.svg)<!--m:Z_1 \parallel Z_2 = Z_1 Z_2/(Z_1 + Z_2)-->; and a divider with ![Z_top](lc-filter.assets/eq-inline/7b96a5d593.svg)<!--m:Z_{top}-->
  above ![Z_bottom](lc-filter.assets/eq-inline/9da657382e.svg)<!--m:Z_{bottom}--> passes the fraction ![Z_bottom/(Z_top + Z_bottom)](lc-filter.assets/eq-inline/07d260a5f0.svg)<!--m:Z_{bottom}/(Z_{top} + Z_{bottom})--> of its input.

So treat the filter as a voltage divider: the inductor (impedance ![sL](lc-filter.assets/eq-inline/1b3b95adda.svg)<!--m:sL-->) on top,
and the parallel combination of capacitor (impedance ![1/(sC)](lc-filter.assets/eq-inline/1d73a7e900.svg)<!--m:1/(sC)-->) and load ![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--m:R--> on the bottom:

![H of s equals V_out over V_s equals Z_p over Z_p plus sL, where Z_p is R parallel 1 over sC equals R over 1 plus sRC](lc-filter.assets/eq-divider.svg)

Substitute ![Z_p](lc-filter.assets/eq-inline/7c220bd4ef.svg)<!--m:Z_p--> and clear the inner fractions by multiplying top and bottom by ![(1 + sRC)](lc-filter.assets/eq-inline/71168ee26f.svg)<!--m:(1 + sRC)-->:

![H equals R over 1 plus sRC divided by R over 1 plus sRC plus sL, equals R over R plus sL times 1 plus sRC, equals R over s squared RLC plus sL plus R](lc-filter.assets/eq-tf-derive.svg)

Divide top and bottom by ![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--m:R-->:

![H of s equals 1 over s squared LC plus s L over R plus 1, boxed](lc-filter.assets/eq-tf.svg)

This is the standard second-order low-pass. Matching it term by term against the textbook form names
its two parameters — the natural (corner) frequency ![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--m:\omega_0--> and the quality factor ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q-->:

![H of s equals 1 over s squared over omega_0 squared plus s over Q omega_0 plus 1, with omega_0 equal to 1 over root LC and Q equal to R root C over L](lc-filter.assets/eq-tf-standard.svg)

![1 over omega_0 squared equals LC, 1 over Q omega_0 equals L over R, so Q equals R over omega_0 L equals R root C over L equals R over Z_0, with Z_0 equal to root L over C](lc-filter.assets/eq-q-derive.svg)

![Z_0 = sqrt L/C](lc-filter.assets/eq-inline/2ac38c12fb.svg)<!--m:Z_0 = \sqrt{L/C}--> is the filter's *characteristic impedance*. ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> is simply the load resistance
measured in units of ![Z_0](lc-filter.assets/eq-inline/964f1c3f3e.svg)<!--m:Z_0-->: a heavy load (small ![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--m:R-->) gives a small ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q-->, a light load (large ![R](lc-filter.assets/eq-inline/06576556d1.svg)<!--m:R-->) a
large one. Put ![s = j omega](lc-filter.assets/eq-inline/52dfb07b61.svg)<!--m:s = j\omega--> to get the response to a sine. Then ![s^2 = - omega^2](lc-filter.assets/eq-inline/801507d6f0.svg)<!--m:s^2 = -\omega^2-->, so the
denominator becomes a complex number ![a + jb](lc-filter.assets/eq-inline/7fc3046405.svg)<!--m:a + jb--> with ![a = 1 - omega^2/omega_0^2](lc-filter.assets/eq-inline/bede88f4c7.svg)<!--m:a = 1 - \omega^2/\omega_0^2--> and
![b = omega/(Q omega_0)](lc-filter.assets/eq-inline/ba9a315a73.svg)<!--m:b = \omega/(Q\omega_0)-->. Its size is ![sqrt a^2 + b^2](lc-filter.assets/eq-inline/b0e03cf3d2.svg)<!--m:\sqrt{a^2 + b^2}--> (Pythagoras on the real and imaginary
parts), and the size of ![1/(a + jb)](lc-filter.assets/eq-inline/3f99023ba2.svg)<!--m:1/(a + jb)--> is ![1/sqrt a^2 + b^2](lc-filter.assets/eq-inline/2cf16abbde.svg)<!--m:1/\sqrt{a^2 + b^2}-->. That is the magnitude response:

![magnitude of H of j omega equals 1 over the square root of 1 minus omega squared over omega_0 squared, squared, plus omega over Q omega_0, squared](lc-filter.assets/eq-mag.svg)

Three regimes tell you everything:

![for omega much less than omega_0, H tends to 1; at omega_0, H equals Q; for omega much greater than omega_0, H is about omega_0 over omega squared](lc-filter.assets/eq-limits.svg)

- **Far below ![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--m:\omega_0-->** the gain is 1: slow things pass untouched. DC — the average — is the
  slowest thing there is (![H(0) = 1](lc-filter.assets/eq-inline/6d972adbf1.svg)<!--m:H(0) = 1--> exactly).
- **At ![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--m:\omega_0-->** the gain is ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q-->. For ![Q > 1](lc-filter.assets/eq-inline/2fa298f53d.svg)<!--m:Q > 1--> the filter *amplifies* signals near its resonance —
  the peak in Fig. 60.
- **Far above ![omega_0](lc-filter.assets/eq-inline/09a7be4d65.svg)<!--m:\omega_0-->** the gain falls as the square of frequency. In decibels (![20 log_10](lc-filter.assets/eq-inline/fc732c015f.svg)<!--m:20\log_{10}--> of an
  amplitude ratio, as defined in
  [../../fundamentals/signals/edges-and-fourier.md §3](../../fundamentals/signals/edges-and-fourier.md#3-why-steep-edges-matter--c-dvdt-and-l-didt)),
  per *decade* (a factor of 10 in frequency):

![20 log of omega_0 over omega squared equals minus 40 log of omega over omega_0 dB, so minus 40 dB per decade](lc-filter.assets/eq-slope.svg)

Every factor of ten in frequency costs a factor of a hundred in amplitude — one factor of ten from the
inductor's rising impedance and one from the capacitor's falling one, as promised in §2.

![Bode magnitude plot of the LC filter for three load resistances, with 50 Hz passed and 50 kHz attenuated by 60 dB](lc-filter.assets/fig-05.svg)

_The same ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L--> and ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C--> with three loads. All three agree far from ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0-->: flat at 50 Hz, a ![-40 dB](lc-filter.assets/eq-inline/fefc844ddc.svg)<!--m:-40\,\mathrm{dB}-->/decade
cliff above. Only near ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> does the load matter — a light load lets the resonance peak up to
![+12 dB](lc-filter.assets/eq-inline/4493744e77.svg)<!--m:+12\,\mathrm{dB}--> (![Q = 4](lc-filter.assets/eq-inline/233ccb4ea8.svg)<!--m:Q = 4-->), a heavy one rounds it off._

## 7 Choosing the corner frequency — and the design used here

The filter has one job with two sides: pass the thing you want, reject the switching frequency. So
![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> must sit **far above the slowest-changing signal you want to keep** and **far below the switching
frequency**. For a DC supply the first condition is free. For the inverter this tree is building
towards, the signal is a ![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--m:50\,\mathrm{Hz}--> sine and the switching frequency is ![50 kHz](lc-filter.assets/eq-inline/c61affb2d7.svg)<!--m:50\,\mathrm{kHz}-->, so:

![f_signal much less than f_0 much less than f_sw; f_0 near the geometric mean of 50 and 50000, about 1.58 kHz](lc-filter.assets/eq-separation.svg)

Placing ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> at the geometric mean gives equal margin on both sides: a factor of about 32 (one and a
half decades) each way. The design used for every figure in this document does exactly that:

![f_0 equals 1 over 2 pi root of 10 to the minus 3 times 10 to the minus 5, about 1.59 kHz; Z_0 equals 10 ohms; Q equals 1](lc-filter.assets/eq-design-num.svg)

with ![V_in = 12 V](lc-filter.assets/eq-inline/9a063a8f8e.svg)<!--m:V_{in} = 12\,\mathrm{V}-->, ![L = 1 mH](lc-filter.assets/eq-inline/5367ea2324.svg)<!--m:L = 1\,\mathrm{mH}-->, ![C = 10 mu F](lc-filter.assets/eq-inline/a68d258fbb.svg)<!--m:C = 10\,\mu\mathrm{F}-->, ![R = 10 Omega](lc-filter.assets/eq-inline/0b1b42866f.svg)<!--m:R = 10\,\Omega-->. Reading the
Bode plot (Fig. 60: ![|H|](lc-filter.assets/eq-inline/96ceb9b4d8.svg)<!--m:|H|--> in decibels against frequency on a logarithmic axis) at the two
frequencies that matter:

![H at 50 kHz is about 1.59 over 50 squared, about 1.0 times 10 to the minus 3, minus 60 dB; H at 50 Hz is about 1.001, 0 dB](lc-filter.assets/eq-atten-num.svg)

The switching frequency is cut by a factor of a thousand; the ![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--m:50\,\mathrm{Hz}--> signal passes at full
size with only about ![1.8°](lc-filter.assets/eq-inline/0de2b0765a.svg)<!--m:1.8°--> of phase lag (the phase is worked out in §12). How these particular values of ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L--> and ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C--> were picked from
![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> and ![Z_0](lc-filter.assets/eq-inline/964f1c3f3e.svg)<!--m:Z_0--> is in §14.

## 8 Ripple — what the filter lets through

A thousand-fold attenuation is not infinite, so something of the switching survives. Compute it two
ways and check that they agree.

**Inductor current ripple.** While the switch is ON the inductor sees ![V_in - V_out = V_in(1-D)](lc-filter.assets/eq-inline/bf3b705fbc.svg)<!--m:V_{in} - V_{out} = V_{in}(1-D)-->
for a time ![DT](lc-filter.assets/eq-inline/f91a09d081.svg)<!--m:DT-->, so its current rises by:

![delta I_L equals V_in minus V_out times DT over L, equals V_in times 1 minus D times D over L f_sw](lc-filter.assets/eq-ripple-I.svg)

![delta I_L equals 12 times 0.5 times 0.5 over 10 to the minus 3 times 50000, equals 3 over 50, equals 0.06 A](lc-filter.assets/eq-ripple-I-num.svg)

(It falls by the same amount while OFF — that is steady state.) ![D(1-D)](lc-filter.assets/eq-inline/3b8ebc31f3.svg)<!--m:D(1-D)--> peaks at ![D = 0.5](lc-filter.assets/eq-inline/a2406f7d12.svg)<!--m:D = 0.5-->, so the
running example is the worst case.

**Capacitor voltage ripple.** The load takes the average current; the capacitor absorbs the triangle's
deviation from it, a triangle centred on zero. The charge in its positive half is ![T Delta I_L/8](lc-filter.assets/eq-inline/a5793fda45.svg)<!--m:T\,\Delta I_L/8--> (the
same triangle-area argument as the
[buck document §5](../../dc-dc-converters/buck/buck.md#5-sizing-the-output-capacitor)), so:

![delta V_C equals delta Q over C equals T delta I_L over 8 over C, equals delta I_L over 8 C f_sw](lc-filter.assets/eq-ripple-V.svg)

![delta V_C equals 0.06 over 8 times 10 to the minus 5 times 50000, equals 0.06 over 4, equals 15 mV](lc-filter.assets/eq-ripple-V-num.svg)

Fifteen millivolts on six volts — a quarter of a percent. Substituting ![Delta I_L](lc-filter.assets/eq-inline/c856ab20fc.svg)<!--m:\Delta I_L-->, and writing
![1/(LC) = omega_0^2 = (2 pi f_0)^2](lc-filter.assets/eq-inline/744c3c9e82.svg)<!--m:1/(LC) = \omega_0^2 = (2\pi f_0)^2-->, shows the ripple is the filter-attenuation story in disguise
(the last step uses ![D(1-D) leq 14](lc-filter.assets/eq-inline/605acfbba7.svg)<!--m:D(1-D) \le \tfrac14-->):

![delta V_C equals V_in D times 1 minus D over 8 LC f_sw squared, equals pi squared over 2 times D times 1 minus D times V_in times f_0 over f_sw squared, at most pi squared over 8 V_in f_0 over f_sw squared](lc-filter.assets/eq-ripple-unified.svg)

The ripple scales as ![(f_0/f_sw)^2](lc-filter.assets/eq-inline/77fc370b8f.svg)<!--m:(f_0/f_{sw})^2--> — exactly the ![-40 dB](lc-filter.assets/eq-inline/fefc844ddc.svg)<!--m:-40\,\mathrm{dB}-->/decade slope evaluated at the
switching frequency. Halve ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> (or double ![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->) and the ripple drops by four.

![Simulated switch-node voltage, inductor current, output voltage and output ripple for a constant duty cycle of 0.5](lc-filter.assets/fig-03.svg)

_The simulation agrees with the hand calculation to the digit: a ![0.060 A](lc-filter.assets/eq-inline/5a1efa6c08.svg)<!--m:0.060\,\mathrm{A}--> current triangle and a
![15.0 mV](lc-filter.assets/eq-inline/60be7e4951.svg)<!--m:15.0\,\mathrm{mV}--> voltage ripple. On the ![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--m:0-->–![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--m:12\,\mathrm{V}--> scale (third panel) the output is a ruler-straight
line; only a thousand-fold zoom (bottom) shows the residue, now nearly a sine because the filter has
stripped the square wave's harmonics._

## 9 Constant D gives flat DC; change D, change the DC

Hold the duty cycle fixed and the output is a straight line at ![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}--> — the third panel of Fig. 58,
and what the source video shows in its first output trace. Change the fixed value and the line moves:

![Simulated PWM waveforms for duty cycles 0.25, 0.5 and 0.75 and the three flat output voltages 3 V, 6 V and 9 V](lc-filter.assets/fig-04.svg)

_Narrow pulses give a low line, wide pulses a high one: ![D = 0.25, 0.5, 0.75](lc-filter.assets/eq-inline/c4f063c6c7.svg)<!--m:D = 0.25, 0.5, 0.75--> give ![3, 6, 9 V](lc-filter.assets/eq-inline/20ea02ba10.svg)<!--m:3, 6, 9\,\mathrm{V}--> from
a ![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--m:12\,\mathrm{V}--> supply. Every one of them is still DC — the output has no time-variation beyond the
millivolt ripple._

This is already useful — it is a step-down DC supply with an adjustable output — but on its own it is
still just DC. The interesting question is what happens *between* the flat lines, when ![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> changes.

## 10 A step in D — ringing, and damping by the load

Jump ![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> from ![0.25](lc-filter.assets/eq-inline/bdedc3fe49.svg)<!--m:0.25--> to ![0.75](lc-filter.assets/eq-inline/0cf1aeac03.svg)<!--m:0.75-->. The average of the switch node jumps from ![3 V](lc-filter.assets/eq-inline/2c9b84f130.svg)<!--m:3\,\mathrm{V}--> to ![9 V](lc-filter.assets/eq-inline/fe37df33fd.svg)<!--m:9\,\mathrm{V}-->
instantly, but the output cannot: the capacitor's voltage can only rise as fast as the inductor's current
lets it, and that current can only rise as fast as ![V_in/L](lc-filter.assets/eq-inline/703b9e93b8.svg)<!--m:V_{in}/L--> allows. The output follows the
second-order step response of ![H(s)](lc-filter.assets/eq-inline/42687f71af.svg)<!--m:H(s)-->. That response is derived in
[../../dc-dc-converters/buck/startup.md §3–§4](../../dc-dc-converters/buck/startup.md#3-the-averaged-model--a-second-order-lc-step)
in terms of the damping ratio ![zeta](lc-filter.assets/eq-inline/08fe2529d0.svg)<!--m:\zeta-->; comparing its standard form with §6 here gives
![zeta = 1/(2Q)](lc-filter.assets/eq-inline/eab4007e81.svg)<!--m:\zeta = 1/(2Q)-->, so the overshoot ![e^- pi zeta/sqrt 1- zeta^2](lc-filter.assets/eq-inline/a1c9e57084.svg)<!--m:e^{-\pi\zeta/\sqrt{1-\zeta^2}}--> and the envelope time constant
![tau = 1/( zeta omega_0)](lc-filter.assets/eq-inline/9271507b21.svg)<!--m:\tau = 1/(\zeta\omega_0)--> become, in terms of ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q-->:

![overshoot equals e to the minus pi over root of 4 Q squared minus 1; decay envelope e to the minus t over tau, with tau equal to 2Q over omega_0 equal to 2RC](lc-filter.assets/eq-overshoot.svg)

![Q equal 1: about 16 percent overshoot, tau 0.2 ms; Q equal 4: about 67 percent overshoot, tau 0.8 ms](lc-filter.assets/eq-overshoot-num.svg)

![Simulated output voltage after a duty-cycle step from 0.25 to 0.75 and back, for three load resistances](lc-filter.assets/fig-06.svg)

_Same filter, three loads. At ![Q = 1](lc-filter.assets/eq-inline/24fdb68929.svg)<!--m:Q = 1--> (blue) the output overshoots to about ![10 V](lc-filter.assets/eq-inline/a03ac5878a.svg)<!--m:10\,\mathrm{V}--> and settles
within a millisecond. The light load (red, ![Q = 4](lc-filter.assets/eq-inline/233ccb4ea8.svg)<!--m:Q = 4-->) overshoots to about ![13 V](lc-filter.assets/eq-inline/3606f977ee.svg)<!--m:13\,\mathrm{V}--> — above the
![12 V](lc-filter.assets/eq-inline/8c8848c481.svg)<!--m:12\,\mathrm{V}--> supply — and rings for several milliseconds. The heavy load (green) never overshoots but
is slow._

Three things to take from it:

- **The load resistor is the only damper.** An ideal ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L--> and ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C--> trade energy back and forth forever;
  the resistor is what bleeds that oscillation away, with time constant ![2RC](lc-filter.assets/eq-inline/3f61238eeb.svg)<!--m:2RC-->. Remove the load and the
  filter would ring indefinitely.
- **The output can exceed the supply.** Energy stored in the inductor during the rise keeps pushing
  charge into the capacitor after it has reached the target. A light-load design must survive that
  overshoot.
- **The diode makes the downward step asymmetric.** On the fall (at ![3.5 ms](lc-filter.assets/eq-inline/0270066b38.svg)<!--m:3.5\,\mathrm{ms}-->) the light-load
  ringing is visibly shorter than on the rise. Ringing needs the inductor current to swing negative,
  and the diode refuses to conduct backwards; the current sits at zero for part of each cycle
  (*discontinuous conduction*), which damps the oscillation. The simulator models this; an idealised
  linear analysis would not.

The practical upshot: the filter needs a few resonant periods (![1/f_0 approx 0.63 ms](lc-filter.assets/eq-inline/8f40f108bf.svg)<!--m:1/f_0 \approx 0.63\,\mathrm{ms}--> each) to
follow a sudden change. It is not instantaneous — and that is the speed limit of §12.

## 11 The big idea — vary D cycle by cycle

Nothing says ![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> has to be the same in every switching period. The switch controller can choose a new
duty cycle for each period — narrow pulses here, wide ones there. Ramp ![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> up over many periods and the
local average of the switch node ramps up with it; the filter, which passes slow things and rejects the
switching, delivers that slowly-moving average to the load:

![Simulated PWM whose duty cycle ramps up then down, and the filtered output following D times V_in](lc-filter.assets/fig-07.svg)

_Pulses grow from a sliver to nearly full width and back. The output (solid) follows the local average
![D(t) V_in](lc-filter.assets/eq-inline/dfb6a6490e.svg)<!--m:D(t)\,V_{in}--> (dashed) up and back down. The switching is slowed to ![10 kHz](lc-filter.assets/eq-inline/b573bbc290.svg)<!--m:10\,\mathrm{kHz}--> here so every pulse
can be seen; that is why the ripple is visible and the lag is noticeable._

This is the linearity of §5 paying off. The averaged circuit is linear and driven by
![v_s(t) = D(t) V_in](lc-filter.assets/eq-inline/a5eb53ec9a.svg)<!--m:\overline{v_s}(t) = D(t)\,V_{in}-->, so the averaged output is that input passed through ![H(s)](lc-filter.assets/eq-inline/42687f71af.svg)<!--m:H(s)-->:

![V_out bar of s equals H of s times V_in times D of s; so v_out bar of t is about D of t times V_in when D varies well below f_0](lc-filter.assets/eq-tracking.svg)

For a ramp, the filter's finite speed shows up as a constant delay — the output runs parallel to the
target, a fixed time behind it. The reason: for slowly varying inputs ![s](lc-filter.assets/eq-inline/a0f1490a20.svg)<!--m:s--> is small, and
![H(s) = 1/(1 + sL/R + s^2LC) approx 1 - sL/R](lc-filter.assets/eq-inline/8d573bff13.svg)<!--m:H(s) = 1/(1 + sL/R + s^2LC) \approx 1 - sL/R--> (drop the tiny ![s^2](lc-filter.assets/eq-inline/4b4d44903f.svg)<!--m:s^2--> term and use
![1/(1 + x) approx 1 - x](lc-filter.assets/eq-inline/39ba6f1099.svg)<!--m:1/(1 + x) \approx 1 - x-->). Multiplying by ![s](lc-filter.assets/eq-inline/a0f1490a20.svg)<!--m:s--> means differentiating, so the output is the input minus
![L/R](lc-filter.assets/eq-inline/744da8232a.svg)<!--m:L/R--> times its slope — which for a straight line is the same line shifted ![L/R](lc-filter.assets/eq-inline/744da8232a.svg)<!--m:L/R--> later:

![D equals k t gives v_out bar tending to k V_in times t minus 1 over Q omega_0, equals k V_in times t minus L over R](lc-filter.assets/eq-ramp-lag.svg)

![L/R = 0.1 ms](lc-filter.assets/eq-inline/680a8183e3.svg)<!--m:L/R = 0.1\,\mathrm{ms}--> here, plus about half a switching period because each period's duty cycle is
decided at its start — together the visible gap in Fig. 62.

The ramp is just one shape. Make ![D(t)](lc-filter.assets/eq-inline/a6f14a1480.svg)<!--m:D(t)--> a sinusoid and the output is a sinusoid:

![Simulated PWM with a sinusoidally varying duty cycle and the filtered output, a sine wave that stays between 0 and V_in](lc-filter.assets/fig-08.svg)

_A sinusoidal duty-cycle pattern produces a sinusoidal output. Its centre is ![V_in/2](lc-filter.assets/eq-inline/e4314a28a2.svg)<!--m:V_{in}/2-->, its peaks
approach ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> and ![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--m:0--> — never beyond. At ![500 Hz](lc-filter.assets/eq-inline/bfa338a3df.svg)<!--m:500\,\mathrm{Hz}--> (a third of ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0-->) the filter's phase lag is
already visible._

Now do it with realistic numbers — a ![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--m:50\,\mathrm{Hz}--> pattern on a ![50 kHz](lc-filter.assets/eq-inline/c61affb2d7.svg)<!--m:50\,\mathrm{kHz}--> carrier, a thousand
switching periods per output cycle:

![D of t equals 0.5 plus 0.4 sin 2 pi 50 t, so v_out of t is about 6 plus 4.8 sin 2 pi 50 t volts](lc-filter.assets/eq-sine.svg)

![Simulated 50 Hz output synthesised from 50 kHz PWM, compared with the bipolar sine an AC load needs](lc-filter.assets/fig-09.svg)

_Top: two ![200 mu s](lc-filter.assets/eq-inline/3633d1d3b3.svg)<!--m:200\,\mu\mathrm{s}--> windows of the switch node — wide pulses near the crest, slivers near the
trough. Bottom: the simulated output (blue) lies on top of ![D(t) V_in](lc-filter.assets/eq-inline/dfb6a6490e.svg)<!--m:D(t)\,V_{in}--> (dashed amber); with three
decades between the signal and the switching, the ripple and lag are invisible. But it is a sine around
![6 V](lc-filter.assets/eq-inline/8ed5af7660.svg)<!--m:6\,\mathrm{V}-->, not around zero (red: what an AC load needs)._

This is the central idea of every modern inverter, motor drive and class-D amplifier: **a switch that is
only ever fully on or fully off, plus an LC filter, behaves like a voltage source you can program in
time**. The pattern of duty cycles *is* the waveform; the filter just erases the switching. How to
generate that pattern — comparing a sine against a triangle carrier — is
[sinusoidal PWM](../../dc-ac-inverters/spwm/); the general machinery of duty cycle and carriers is in
[pwm/](../../pwm/).

## 12 How fast may D change?

The phrase "well below ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0-->" in §11 carries the whole caveat. The filter cannot tell the difference
between a fast change of ![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> that you *wanted* and switching ripple that you didn't — both are just
high-frequency content, and both are attenuated alike. The phase of ![H](lc-filter.assets/eq-inline/7cf184f4c6.svg)<!--m:H--> shows how the delay grows. With the denominator
![a + jb](lc-filter.assets/eq-inline/7fc3046405.svg)<!--m:a + jb--> of §6, the phase of ![1/(a + jb)](lc-filter.assets/eq-inline/3f99023ba2.svg)<!--m:1/(a + jb)--> is ![- (b/a)](lc-filter.assets/eq-inline/a57726e40e.svg)<!--m:-\arctan(b/a)-->, the angle of the point ![(a, b)](lc-filter.assets/eq-inline/6cea03da36.svg)<!--m:(a, b)-->
measured from the real axis and reversed in sign:

![phi of f equals minus arctan of f over f_0 over Q, over 1 minus f over f_0 squared; phi at 50 Hz is about minus 1.8 degrees, phi at f_0 is minus 90 degrees](lc-filter.assets/eq-phase.svg)

![Simulated output for sinusoidal duty cycles at a tenth of f0, at f0 and at three times f0, compared with D times V_in](lc-filter.assets/fig-10.svg)

_The same duty-cycle sine at three speeds. At a tenth of ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> the output tracks it. At ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> the output is
a quarter-cycle late (and, with a lighter load, would also be ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> times too big). At three times ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> the
filter treats the wanted signal as ripple and removes almost all of it._

So there are two separate conditions for ![v_out(t) approx D(t) V_in](lc-filter.assets/eq-inline/5cc5b56e89.svg)<!--m:v_{out}(t) \approx D(t)\,V_{in}-->:

1. **![D](lc-filter.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> must change slowly compared with ![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->** — many switching periods per feature of the waveform —
   or there is no meaningful "local average" to follow.
2. **The waveform's frequency content must sit well below ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0-->** — or the filter itself distorts it.

Both are the same separation, ![f_signal much less than f_0 much less than f_sw](lc-filter.assets/eq-inline/a6c0685add.svg)<!--m:f_{signal} \ll f_0 \ll f_{sw}-->, read from the two ends.

## 13 One switch, one polarity — the road to the inverter

Every output in this document stays between ![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--m:0--> and ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}-->, and it cannot do otherwise. The switch node
only ever takes the values ![V_in](lc-filter.assets/eq-inline/29f560cdfe.svg)<!--m:V_{in}--> and ![0](lc-filter.assets/eq-inline/b6589fc6ab.svg)<!--m:0-->, and the duty cycle is a fraction:

![0 at most D of t at most 1, so 0 at most v_out bar at most V_in](lc-filter.assets/eq-unipolar.svg)

A single switch can make any *slow, one-polarity* waveform — a DC level, a ramp, a sine riding on an
offset — but never a voltage below zero. Mains-style AC must swing both ways (red trace, Fig. 64).
Blocking the offset with a series capacitor is no answer for ![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--m:50\,\mathrm{Hz}--> power: the capacitor
would be enormous and the offset still wastes half the supply's range.

The fix is to make the switch node itself bipolar. An
[H-bridge](../../dc-ac-inverters/h-bridge/) of four switches can connect the load either way round
across the supply, so the voltage between its two output nodes takes the values ![+V_in](lc-filter.assets/eq-inline/088e3e6d5f.svg)<!--m:+V_{in}--> and
![-V_in](lc-filter.assets/eq-inline/b15a60dd4f.svg)<!--m:-V_{in}-->. Averaging that wave exactly as in §4:

![v_AB in plus V_in, minus V_in, so v_AB bar equals D V_in minus 1 minus D V_in equals 2D minus 1 times V_in, between minus V_in and plus V_in](lc-filter.assets/eq-bipolar.svg)

Now ![D = 0.5](lc-filter.assets/eq-inline/a2406f7d12.svg)<!--m:D = 0.5--> gives zero, ![D > 0.5](lc-filter.assets/eq-inline/37b1b71070.svg)<!--m:D > 0.5--> positive and ![D < 0.5](lc-filter.assets/eq-inline/53408dfd7a.svg)<!--m:D < 0.5--> negative output, and the very same LC filter
and the very same cycle-by-cycle trick produce a true AC sine. That is the inverter, assembled from the
three ideas of this document: PWM average, LC filtering, and a duty cycle that moves.

## 14 What this costs you

- **Size and weight.** The filter's ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L--> and ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C--> must store enough energy to bridge a switching period at
  full load. At low switching frequency they are big; the ![(f_0/f_sw)^2](lc-filter.assets/eq-inline/77fc370b8f.svg)<!--m:(f_0/f_{sw})^2--> ripple law is why designers
  chase higher ![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}--> — which in turn costs switching loss in the transistor and diode.
- **Sizing is a two-number choice.** ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> fixes the product ![LC](lc-filter.assets/eq-inline/3b0e58d439.svg)<!--m:LC-->; the split between them is set by
  ![Z_0 = sqrt L/C](lc-filter.assets/eq-inline/2ac38c12fb.svg)<!--m:Z_0 = \sqrt{L/C}-->, which should be comparable to the load so that ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> lands near 1. Given ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> and
  ![Z_0](lc-filter.assets/eq-inline/964f1c3f3e.svg)<!--m:Z_0-->:

  ![L equals Z_0 over omega_0 and C equals 1 over omega_0 Z_0, which with Z_0 of 10 ohms and omega_0 of 10 to the 4 gives 1 mH and 10 microfarads](lc-filter.assets/eq-sizing.svg)

  This is how the running example was chosen: ![omega_0 = 10^4 rad times s^-1](lc-filter.assets/eq-inline/d87e8509d2.svg)<!--m:\omega_0 = 10^4\,\mathrm{rad\cdot s^{-1}}--> (![f_0 approx 1.59 kHz](lc-filter.assets/eq-inline/71874cd5e1.svg)<!--m:f_0 \approx 1.59\,\mathrm{kHz}-->,
  the geometric mean of ![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--m:50\,\mathrm{Hz}--> and ![50 kHz](lc-filter.assets/eq-inline/c61affb2d7.svg)<!--m:50\,\mathrm{kHz}-->) and ![Z_0 = R = 10 Omega](lc-filter.assets/eq-inline/3d9818d8a6.svg)<!--m:Z_0 = R = 10\,\Omega--> for ![Q = 1](lc-filter.assets/eq-inline/24fdb68929.svg)<!--m:Q = 1-->.
  A bigger ![L](lc-filter.assets/eq-inline/d160e0986a.svg)<!--m:L--> with a smaller ![C](lc-filter.assets/eq-inline/32096c2e0e.svg)<!--m:C--> lowers the ripple current but makes the filter softer (output sags more
  under sudden load steps); the reverse needs a capacitor that can carry more ripple current.
- **Resonance and load dependence.** ![Q = R/Z_0](lc-filter.assets/eq-inline/73b00947fa.svg)<!--m:Q = R/Z_0--> depends on the load, which the designer does not
  control. At light load ![Q](lc-filter.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> grows, the response peaks at ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> (Fig. 60) and steps ring and overshoot
  above the supply (Fig. 61). At no load an ideal filter would not damp at all. Real designs add damping
  (a resistor in series with an extra capacitor across the output, or active damping in the control
  loop) and rely on feedback rather than trusting ![v_out = D V_in](lc-filter.assets/eq-inline/b09da4744e.svg)<!--m:v_{out} = D\,V_{in}--> open-loop.
- **A speed limit.** The output can only follow ![D(t)](lc-filter.assets/eq-inline/a6f14a1480.svg)<!--m:D(t)--> for frequencies well below ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0-->, and ![f_0](lc-filter.assets/eq-inline/bdd0794289.svg)<!--m:f_0--> must
  stay well below ![f_sw](lc-filter.assets/eq-inline/4ac287231a.svg)<!--m:f_{sw}-->. A ![50 Hz](lc-filter.assets/eq-inline/01368b7b9b.svg)<!--m:50\,\mathrm{Hz}--> inverter is comfortable; synthesising a ![20 kHz](lc-filter.assets/eq-inline/bbc70d4c7e.svg)<!--m:20\,\mathrm{kHz}-->
  audio signal needs a switching frequency in the hundreds of kilohertz.
- **Losses and non-idealities.** The inductor's winding resistance, the capacitor's ESR (which adds its own
  ripple term, often larger than the ![15 mV](lc-filter.assets/eq-inline/e53cb2e3b3.svg)<!--m:15\,\mathrm{mV}--> computed here), the diode's forward drop (so
  ![v_out](lc-filter.assets/eq-inline/56c103859e.svg)<!--m:v_{out}--> sits slightly below ![D V_in](lc-filter.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->), and the diode's refusal to conduct backwards at light load
  (discontinuous conduction, §10), which breaks the linear model. Synchronous rectification — a second
  transistor in place of the diode — removes the drop and the discontinuity at the cost of gate-drive
  complexity.
- **One polarity only.** A single switch with an LC filter cannot go negative (§13); AC needs the
  four-switch H-bridge and twice the transistors.

## 15 Sources and cross-links

- **The same circuit as a DC-DC converter:** [../../dc-dc-converters/buck/](../../dc-dc-converters/buck/)
  — volt-second balance, inductor and capacitor sizing; its start-up behaviour is in
  [../../dc-dc-converters/buck/startup.md](../../dc-dc-converters/buck/startup.md).
- **The two laws:** [../../fundamentals/inductor/](../../fundamentals/inductor/) (![v_L = L di/dt](lc-filter.assets/eq-inline/6e2526f84b.svg)<!--m:v_L = L\,di/dt-->, the
  inductive kick) and [../../fundamentals/capacitor/](../../fundamentals/capacitor/) (![i_C = C dv/dt](lc-filter.assets/eq-inline/20002f7e38.svg)<!--m:i_C = C\,dv/dt-->,
  the duality table).
- **Frequency, phase and decibels:** [../../fundamentals/signals/](../../fundamentals/signals/).
- **Where this leads:** [../../pwm/](../../pwm/) (duty cycle and carriers),
  [../../dc-ac-inverters/spwm/](../../dc-ac-inverters/spwm/) (sinusoidal PWM — the ![D(t)](lc-filter.assets/eq-inline/a6f14a1480.svg)<!--m:D(t)--> pattern of §11)
  and [../../dc-ac-inverters/h-bridge/](../../dc-ac-inverters/h-bridge/) (the bipolar switch node of §13).
- **Source material:** the walkthrough this note expands (a tutorial video on DC-to-AC conversion, PWM and
  LC filtering, around 9:30–10:20). Every waveform here is re-derived and re-simulated rather than copied:
  the simulator is `toolchain/figures/lc_filter.js`, which integrates the equations of §5 with exact
  switching edges and an ideal diode.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
