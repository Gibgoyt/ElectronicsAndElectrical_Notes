# Edges and Fourier series — every switched waveform is a sum of sines

Every circuit in this tree switches. A buck converter chops its input, an H-bridge flips a battery
back and forth across a lamp, and a PWM inverter does both thousands of times per cycle. Each switch
transition is an **edge**. Edges have a speed, and that speed has consequences. The square waves that
edges build turn out to be sums of plain sine waves. That one fact, the **Fourier series**, explains
why an H-bridge's square output makes a lamp glow, why it makes a transformer hum and run hot, and why
the PWM and SPWM documents later in this tree go to such lengths to shape it. This document builds
the maths from nothing. You only need to know what a sine wave and an integral are.

**Contents**

1. [What an edge is](#1-what-an-edge-is)
2. [Why real edges are not vertical](#2-why-real-edges-are-not-vertical)
3. [Why steep edges matter — C dv/dt and L di/dt](#3-why-steep-edges-matter--c-dvdt-and-l-didt)
4. [The Fourier idea — building shapes out of sines](#4-the-fourier-idea--building-shapes-out-of-sines)
5. [Orthogonality — the trick that makes it work](#5-orthogonality--the-trick-that-makes-it-work)
6. [The square wave, derived](#6-the-square-wave-derived)
7. [Reading a spectrum — Gibbs, Parseval and THD](#7-reading-a-spectrum--gibbs-parseval-and-thd)
8. [The H-bridge output as a Fourier series](#8-the-h-bridge-output-as-a-fourier-series)
9. [Why the harmonics matter downstream](#9-why-the-harmonics-matter-downstream)
10. [What this costs you](#10-what-this-costs-you)
11. [Sources and cross-links](#11-sources-and-cross-links)

> **The thesis in one line**
>
> Any repeating waveform, however square, is exactly a sum of sines at whole-number multiples of its
> frequency. An ideal H-bridge driving a lamp produces the series below. Every real-world detail
> (MOSFET on-resistance, dead time, edge speed) only scales or reshapes those amplitudes:

![v of t equals 4 V over pi times the sum over odd n of sin n omega t over n](edges-and-fourier.assets/eq-sq-result.svg)

---

## 1 What an edge is

An **edge** is the moment a signal jumps from one level to another. A jump upward, for example from
0 V to 12 V when a MOSFET turns on, is a **rising edge**. A jump downward is a **falling edge**. A
square wave is nothing more than flat stretches joined by alternating rising and falling edges. The
flat parts carry the energy. The edges carry most of the trouble.

On paper the edge is a perfect vertical line. This is the **ideal step**, written with the unit step
function <!--m:u(t)-->![u(t)](edges-and-fourier.assets/eq-inline/843ca7f6da.svg)<!--/m-->:

![v of t equals V times u of t, where u is 0 before t equals 0 and 1 after](edges-and-fourier.assets/eq-step.svg)

The ideal step goes from 0 to <!--m:V-->![V](edges-and-fourier.assets/eq-inline/c9ee5681d3.svg)<!--/m--> in zero time, so its slope <!--m:dv/dt-->![dv/dt](edges-and-fourier.assets/eq-inline/7631ac0fee.svg)<!--/m--> at <!--m:t = 0-->![t = 0](edges-and-fourier.assets/eq-inline/fee440f68f.svg)<!--/m--> is
**infinite**. No physical voltage can do that, for a reason this whole tree keeps coming back to.
Every real node has some capacitance, and the capacitor law <!--m:i_C = C \, dv/dt-->![i_C = C dv/dt](edges-and-fourier.assets/eq-inline/37a5867086.svg)<!--/m-->
([../capacitor/capacitor.md](../capacitor/capacitor.md)) says an infinite <!--m:dv/dt-->![dv/dt](edges-and-fourier.assets/eq-inline/7631ac0fee.svg)<!--/m--> needs infinite
current. Real edges therefore have a finite **rise time** <!--m:t_r-->![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--/m--> (and **fall time** <!--m:t_f-->![t_f](edges-and-fourier.assets/eq-inline/1f679eb63d.svg)<!--/m-->).

**The 10–90 % rule.** Real edges approach their final value gradually, curving in at the start and
creeping in at the end. That makes "start" and "end" hard to pin down. So by convention the rise time
is measured from the moment the signal crosses **10 %** of its swing to the moment it crosses **90 %**.
The fall time is measured from 90 % down to 10 %. Oscilloscopes measure it this way automatically.

The cleanest worked case is a step driven through a resistance <!--m:R-->![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--/m--> into a capacitance <!--m:C-->![C](edges-and-fourier.assets/eq-inline/32096c2e0e.svg)<!--/m--> (an "RC
edge"). This is roughly what a gate driver charging a MOSFET gate looks like:

![v of t equals V times 1 minus e to the minus t over tau, with tau equal to R C](edges-and-fourier.assets/eq-rc-edge.svg)

Find the two crossing times by setting <!--m:v-->![v](edges-and-fourier.assets/eq-inline/7a38d8cbd2.svg)<!--/m--> to 10 % and 90 % of <!--m:V-->![V](edges-and-fourier.assets/eq-inline/c9ee5681d3.svg)<!--/m--> and solving for <!--m:t-->![t](edges-and-fourier.assets/eq-inline/8efd86fb78.svg)<!--/m-->:

![solving for t_10 equals tau ln 10 over 9 and t_90 equals tau ln 10](edges-and-fourier.assets/eq-t10-t90.svg)

Subtract them. The logarithms combine because <!--m:\ln a - \ln b = \ln(a/b)-->![ln a - ln b = ln (a/b)](edges-and-fourier.assets/eq-inline/98f56d6dfe.svg)<!--/m-->:

![t_r equals t_90 minus t_10 equals tau ln 9, approximately 2.2 tau](edges-and-fourier.assets/eq-rise-time.svg)

So an RC edge's rise time is about <!--m:2.2\,\tau-->![2.2 tau](edges-and-fourier.assets/eq-inline/4bfa788429.svg)<!--/m-->. Differentiating shows the edge is steepest at the
very start, where its slope is <!--m:V/\tau-->![V/tau](edges-and-fourier.assets/eq-inline/720e3621cb.svg)<!--/m-->. A straight line at that slope would reach the final value
after exactly one <!--m:\tau-->![tau](edges-and-fourier.assets/eq-inline/c9148a5f77.svg)<!--/m-->, and that is the tangent drawn in Figure 20:

![dv by dt equals V over tau times e to the minus t over tau, so the slope at t equals 0 is V over tau](edges-and-fourier.assets/eq-rc-slope.svg)

![An ideal step versus a real rising and falling edge with the 10 to 90 percent rise time marked](edges-and-fourier.assets/fig-01.svg)

_The dashed ideal step has zero width. The real edge needs about 2.2 time constants to cover the
middle 80 % of its swing, and the same again on the way down._

**Slew rate.** The other way to describe an edge is by its steepness, the **slew rate** (SR), in volts
per second (usually V/ns or V/µs). Over the 10–90 % portion the signal covers <!--m:0.8\,V-->![0.8 V](edges-and-fourier.assets/eq-inline/939fd1e31b.svg)<!--/m--> in <!--m:t_r-->![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--/m-->:

![SR equals dv by dt approximately equal to 0.8 V over t_r](edges-and-fourier.assets/eq-slew.svg)

As an example, take the 325 V DC bus of the inverter's second H-bridge, switched in 50 ns:

![SR approximately 0.8 times 325 V over 50 ns equals 5.2 V per ns](edges-and-fourier.assets/eq-slew-example.svg)

That is 5.2 billion volts per second. The number looks absurd, and it is completely normal for a
modern MOSFET. Its consequences are the subject of §3.

## 2 Why real edges are not vertical

Three physical effects set the speed of a real switching edge. Each one is a capacitor or an
inductor that the edge has to drive.

**Gate charge (the MOSFET's own capacitance).** A MOSFET is turned on by charging its gate, which
is physically a capacitor plate insulated from the channel. The channel only conducts once enough
charge has been pushed onto the gate. The slow part is the **Miller plateau**. While the drain
voltage is swinging, the gate driver has to supply the gate-drain charge <!--m:Q_{gd}-->![Q_gd](edges-and-fourier.assets/eq-inline/748697ddf2.svg)<!--/m-->, and the gate
voltage stays flat until it has. With a gate-drive current <!--m:I_g-->![I_g](edges-and-fourier.assets/eq-inline/e707592b00.svg)<!--/m-->, the drain edge takes roughly:

![t_sw approximately Q_gd over I_g equals 10 nC over 1 A equals 10 ns](edges-and-fourier.assets/eq-miller.svg)

Double the driver current and the edge is twice as fast. That is why gate drivers are rated in amps
even though the gate draws no steady current.

**Parasitic capacitance at the switching node.** The switching node (the H-bridge midpoint, or the
buck's switch node) has capacitance to everything around it. That includes the MOSFETs' own output
capacitance <!--m:C_{oss}-->![C_oss](edges-and-fourier.assets/eq-inline/076d485f98.svg)<!--/m-->, the PCB copper, the heatsink and the transformer windings. The current
available to charge it is finite, so by the capacitor law the slew rate is capped:

![dv by dt equals i over C_node, so t_r is approximately C_node V over I](edges-and-fourier.assets/eq-cap-limit.svg)

**Inductance in the current loop.** Every wire and PCB trace is a one-turn inductor. A few
centimetres of loop is tens of nanohenries. By the inductor law <!--m:v_L = L \, di/dt-->![v_L = L di/dt](edges-and-fourier.assets/eq-inline/da690c1e1b.svg)<!--/m-->
([../inductor/inductor.md](../inductor/inductor.md)), current cannot change instantly in it either.
The current edge is slowed, and any attempt to force it fast shows up as a voltage spike (§3).

> **Note —** So the ideal step is not just "very fast". It is physically forbidden, by the same two
> laws that run every converter in this tree. Any real node has some <!--m:C-->![C](edges-and-fourier.assets/eq-inline/32096c2e0e.svg)<!--/m-->, so <!--m:v-->![v](edges-and-fourier.assets/eq-inline/7a38d8cbd2.svg)<!--/m--> cannot jump. Any
> real loop has some <!--m:L-->![L](edges-and-fourier.assets/eq-inline/d160e0986a.svg)<!--/m-->, so <!--m:i-->![i](edges-and-fourier.assets/eq-inline/042dc4512f.svg)<!--/m--> cannot jump. Real edges are the compromise between how hard you
> drive and how much <!--m:L-->![L](edges-and-fourier.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](edges-and-fourier.assets/eq-inline/32096c2e0e.svg)<!--/m--> is in the way.

## 3 Why steep edges matter — C dv/dt and L di/dt

Fast edges are good for efficiency. While a MOSFET is half-on it carries current and supports voltage
at the same time, and that overlap is pure heat (switching loss). So designers want short edges. But
a steep edge drives the circuit's hidden capacitors and inductors hard, and both answer back.

**Steep dv/dt pushes current through every capacitance.** Take the 5.2 V/ns edge from §1 and a mere
100 pF of stray capacitance. That could be the capacitance between a transformer's primary and
secondary, or from a MOSFET's drain tab to a grounded heatsink:

![i_C equals C dv by dt equals 100 pF times 5.2 V per ns equals 0.52 A](edges-and-fourier.assets/eq-cap-current.svg)

Half an amp flows for the 50 ns of the edge, through a part that is supposed to be an insulator.
It goes wherever the capacitance leads: into the secondary side, into the chassis, into ground.
This is **common-mode noise**. It is why insulation between windings matters at high frequency,
and why a fast edge can falsely trigger a logic input nearby.

**Steep di/dt makes every inductance spike.** When the MOSFET interrupts, say, 10 A in 10 ns, the
20 nH of loop inductance answers with:

![v_L equals L_loop di by dt equals 20 nH times 10 A over 10 ns equals 20 V](edges-and-fourier.assets/eq-ind-voltage.svg)

That 20 V adds on top of the supply across the turning-off MOSFET. It is a small cousin of the
inductive kick in [../inductor/inductor.md §6](../inductor/inductor.md#6-the-inductive-kick-and-why-the-diode-is-there).
The loop inductance and the node capacitance then form an LC tank. Kicked by the edge, it **rings**
at its natural frequency:

![f_ring equals 1 over 2 pi root L_loop C_oss, about 35.6 MHz](edges-and-fourier.assets/eq-ring.svg)

![A switching edge with inductive overshoot and ringing above, and the capacitive current pulses C dv/dt it drives below](edges-and-fourier.assets/fig-02.svg)

_Top: the overshoot and ringing come from loop inductance and node capacitance. Bottom: current
flows into a capacitance only while the voltage is moving. A flat top draws nothing, and a fast
edge draws a tall pulse._

**The bandwidth of an edge.** An edge also contains high frequencies, and the rise time tells you
how high. An RC network passes frequencies up to its −3 dB corner <!--m:f_{3\mathrm{dB}} = 1/(2\pi\tau)-->![f_3 dB = 1/(2 pi tau )](edges-and-fourier.assets/eq-inline/7f8aa3ccc5.svg)<!--/m-->.
Combine that with <!--m:t_r = \tau \ln 9-->![t_r = tau ln 9](edges-and-fourier.assets/eq-inline/4fa73506c0.svg)<!--/m--> and you get one of the most used rules of thumb in electronics:

![f_3dB equals 1 over 2 pi tau and t_r equals tau ln 9, so t_r times f_3dB equals ln 9 over 2 pi, about 0.35](edges-and-fourier.assets/eq-bandwidth.svg)

A 50 ns edge carries significant content up to about <!--m:0.35/50\,\mathrm{ns} = 7\,\mathrm{MHz}-->![0.35/50 ns = 7 MHz](edges-and-fourier.assets/eq-inline/b428f93b71.svg)<!--/m-->, even
when the waveform repeats at only 50 Hz. To see *why* a fast edge contains high frequencies, we need
the Fourier series.

## 4 The Fourier idea — building shapes out of sines

**Periodic functions.** A signal is **periodic** if it repeats exactly every <!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m--> seconds. <!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m--> is the
**period**, and its inverse is the **fundamental frequency** <!--m:f_1-->![f_1](edges-and-fourier.assets/eq-inline/0b35cc5e94.svg)<!--/m-->:

![f of t plus T equals f of t for all t, f_1 equals 1 over T, omega_1 equals 2 pi f_1 equals 2 pi over T](edges-and-fourier.assets/eq-periodic.svg)

The angular frequency <!--m:\omega_1 = 2\pi f_1-->![omega_1 = 2 pi f_1](edges-and-fourier.assets/eq-inline/fb69270322.svg)<!--/m--> (in radians per second) just counts one full cycle as
<!--m:2\pi-->![2 pi](edges-and-fourier.assets/eq-inline/0833718ca4.svg)<!--/m--> radians instead of 1. That makes sine waves tidy: <!--m:\sin(\omega_1 t)-->![sin ( omega_1 t)](edges-and-fourier.assets/eq-inline/d0650a0829.svg)<!--/m--> completes exactly one
cycle as <!--m:t-->![t](edges-and-fourier.assets/eq-inline/8efd86fb78.svg)<!--/m--> goes from 0 to <!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m-->.

**The claim.** Joseph Fourier's claim (1807) was that *any* reasonable periodic function, square,
triangle, sawtooth or whatever shape, can be written as a sum of sines and cosines whose frequencies
are whole-number multiples of <!--m:f_1-->![f_1](edges-and-fourier.assets/eq-inline/0b35cc5e94.svg)<!--/m-->:

![f of t equals a_0 plus the sum from n equals 1 to infinity of a_n cos n omega_1 t plus b_n sin n omega_1 t](edges-and-fourier.assets/eq-series.svg)

Here is what each piece means:

- **<!--m:a_0-->![a_0](edges-and-fourier.assets/eq-inline/4a5997da73.svg)<!--/m-->** is a constant offset, the **DC component**. It is the signal's average value.
- **<!--m:n = 1-->![n = 1](edges-and-fourier.assets/eq-inline/92ee913214.svg)<!--/m-->** is the **fundamental**. It repeats at the same rate as the signal itself.
- **<!--m:n = 2, 3, 4, \dots-->![n = 2, 3, 4,](edges-and-fourier.assets/eq-inline/a0dd14301c.svg)<!--/m-->** are the **harmonics**. The <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m-->-th harmonic oscillates <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m--> times per period,
  at frequency <!--m:n f_1-->![n f_1](edges-and-fourier.assets/eq-inline/ef548d158f.svg)<!--/m-->. For 50 Hz the 3rd harmonic is 150 Hz and the 5th is 250 Hz.
- **<!--m:a_n-->![a_n](edges-and-fourier.assets/eq-inline/278ab95d3a.svg)<!--/m--> and <!--m:b_n-->![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--/m-->** are the **coefficients**: *how much* of each cosine and sine the recipe needs.
  Finding them is the whole job.

Only whole-number multiples can appear, and the reason is simple. Every term must itself repeat every
<!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m-->, otherwise the sum would not repeat every <!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m-->. A sine at <!--m:2.5 f_1-->![2.5 f_1](edges-and-fourier.assets/eq-inline/7168feadac.svg)<!--/m--> does not fit a whole number of
cycles into <!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m-->, so it can't be part of the recipe.

**Why sines and not some other building block?** Because sines are the one shape that linear
circuits leave alone. Differentiate <!--m:\sin\omega t-->![sin omega t](edges-and-fourier.assets/eq-inline/22c43934a5.svg)<!--/m--> and you get <!--m:\omega \cos\omega t-->![omega cos omega t](edges-and-fourier.assets/eq-inline/44e70d9fed.svg)<!--/m-->: the same
frequency, just scaled and shifted. Integrate it and the same thing happens. Inductors differentiate
current (<!--m:v = L\,di/dt-->![v = L di/dt](edges-and-fourier.assets/eq-inline/169359fd71.svg)<!--/m-->) and capacitors differentiate voltage (<!--m:i = C\,dv/dt-->![i = C dv/dt](edges-and-fourier.assets/eq-inline/7f5f5ec54b.svg)<!--/m-->). So a sine going into
any network of R, L and C comes out as **a sine of the same frequency**, only bigger or smaller and
shifted in time. No other waveform has this property. A square wave into an RC filter comes out
rounded. That is a different *shape*, not just a different size. So the strategy is: break any
waveform into sines, push each sine through the circuit separately (easy), and add the results.
That is the whole reason Fourier series run electrical engineering.

## 5 Orthogonality — the trick that makes it work

The series is useless unless we can find the coefficients. The tool is a set of integrals called the
**orthogonality relations**. We derive them from nothing.

**Step 1 — a cosine integrates to zero over whole cycles.** For any non-zero whole number <!--m:k-->![k](edges-and-fourier.assets/eq-inline/13fbd79c3d.svg)<!--/m-->,
<!--m:\cos(k\omega_1 t)-->![cos (k omega_1 t)](edges-and-fourier.assets/eq-inline/c4aff40c35.svg)<!--/m--> fits exactly <!--m:k-->![k](edges-and-fourier.assets/eq-inline/13fbd79c3d.svg)<!--/m--> whole cycles into one period. Its positive humps and negative
troughs cancel:

![integral from 0 to T of cos k omega_1 t dt equals sine over k omega_1 evaluated, which is sin 2 pi k over k omega_1, equals 0](edges-and-fourier.assets/eq-cos-integral.svg)

The last step uses <!--m:\omega_1 T = 2\pi-->![omega_1 T = 2 pi](edges-and-fourier.assets/eq-inline/119c8f6d73.svg)<!--/m-->, so <!--m:\sin(k\omega_1 T) = \sin(2\pi k) = 0-->![sin (k omega_1 T) = sin (2 pi k) = 0](edges-and-fourier.assets/eq-inline/28721a7aa8.svg)<!--/m--> for every whole
number <!--m:k-->![k](edges-and-fourier.assets/eq-inline/13fbd79c3d.svg)<!--/m-->. The same argument shows <!--m:\sin(k\omega_1 t)-->![sin (k omega_1 t)](edges-and-fourier.assets/eq-inline/d366841a87.svg)<!--/m--> also integrates to zero over a period.

**Step 2 — turn products into sums.** We will need to integrate *products* of two sinusoids. The
trigonometric product-to-sum identities turn a product into a sum of single cosines (or sines), each
of which Step 1 can kill. They come straight from adding and subtracting the angle-sum formulas,
for example <!--m:\cos(A-B) - \cos(A+B) = 2\sin A \sin B-->![cos (A-B) - cos (A+B) = 2 sin A sin B](edges-and-fourier.assets/eq-inline/c859e3def0.svg)<!--/m-->:

![sin A sin B equals one half of cos A minus B minus cos A plus B; cos A cos B equals one half of cos A minus B plus cos A plus B; sin A cos B equals one half of sin A minus B plus sin A plus B](edges-and-fourier.assets/eq-product-to-sum.svg)

**Step 3 — integrate a product of two harmonics.** Take harmonics <!--m:m-->![m](edges-and-fourier.assets/eq-inline/6b0d31c0d5.svg)<!--/m--> and <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m--> (both positive whole
numbers). Apply the first identity:

![integral of sin m omega_1 t sin n omega_1 t equals one half integral of cos m minus n omega_1 t minus one half integral of cos m plus n omega_1 t](edges-and-fourier.assets/eq-orth-sin.svg)

The second integral always vanishes by Step 1, because <!--m:m + n-->![m + n](edges-and-fourier.assets/eq-inline/3b69d04ea5.svg)<!--/m--> is a non-zero whole number. The first
vanishes too, *unless* <!--m:m = n-->![m = n](edges-and-fourier.assets/eq-inline/e6ad14b62e.svg)<!--/m-->. Then <!--m:m - n = 0-->![m - n = 0](edges-and-fourier.assets/eq-inline/499e45ffd9.svg)<!--/m-->, the cosine is <!--m:\cos 0 = 1-->![cos 0 = 1](edges-and-fourier.assets/eq-inline/7f28a78900.svg)<!--/m-->, and its integral is
just the length of the interval, <!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m-->:

![the integral is 0 when m differs from n, and T over 2 when m equals n](edges-and-fourier.assets/eq-orth-sin-result.svg)

The other two identities give the cosine–cosine and sine–cosine cases the same way:

![integral of cos m cos n is 0 for m not equal n and T over 2 for m equal n; integral of sin m cos n is always 0](edges-and-fourier.assets/eq-orth-all.svg)

This is **orthogonality**. Two *different* harmonics multiplied together average to exactly zero.
A harmonic multiplied by *itself* does not. The word comes from geometry. Perpendicular
(orthogonal) vectors have zero dot product, and the integral of a product plays the role of a dot
product for functions.

![The product sin theta times sin 3 theta has equal positive and negative areas, while sin squared theta is never negative](edges-and-fourier.assets/fig-04.svg)

_Top: a product of two different harmonics swings positive and negative, and the shaded areas cancel
exactly. Bottom: a harmonic times itself is a square, never negative, so it averages to one half and
integrates to half the period._

**Step 4 — use it to extract a coefficient.** Now the payoff. Multiply both sides of the series by
<!--m:\sin(m\omega_1 t)-->![sin (m omega_1 t)](edges-and-fourier.assets/eq-inline/6a7a96adf1.svg)<!--/m--> for some chosen <!--m:m-->![m](edges-and-fourier.assets/eq-inline/6b0d31c0d5.svg)<!--/m--> and integrate over one period. On the right-hand side every
term is a product of two sinusoids, and orthogonality kills all of them except the single term where
the harmonic matches:

![integral of f times sin m omega_1 t equals a_0 times 0 plus sum of a_n times 0 plus sum of b_n times the sine integral, which equals b_m T over 2](edges-and-fourier.assets/eq-extract.svg)

Solve for <!--m:b_m-->![b_m](edges-and-fourier.assets/eq-inline/dc8edb5b1e.svg)<!--/m-->. Repeating the same move with <!--m:\cos(m\omega_1 t)-->![cos (m omega_1 t)](edges-and-fourier.assets/eq-inline/88b75bcce5.svg)<!--/m--> gives <!--m:a_m-->![a_m](edges-and-fourier.assets/eq-inline/d5c812e96c.svg)<!--/m-->, and integrating the
series by itself (multiplying by 1) gives <!--m:a_0-->![a_0](edges-and-fourier.assets/eq-inline/4a5997da73.svg)<!--/m-->:

![a_0 equals 1 over T times integral of f; a_n equals 2 over T times integral of f cos n omega_1 t; b_n equals 2 over T times integral of f sin n omega_1 t](edges-and-fourier.assets/eq-coeffs.svg)

These are the **Fourier coefficient formulas**. Read <!--m:b_n-->![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--/m--> in words: *multiply the signal by a test
sine at frequency <!--m:n f_1-->![n f_1](edges-and-fourier.assets/eq-inline/ef548d158f.svg)<!--/m--> and average*. If the signal contains that sine, the product has a steady
positive part and the average is non-zero. If it does not, the product swings both ways and averages
to zero. A Fourier analyser, from a pencil to a spectrum analyser, is doing exactly this.

> **Tip —** It is often neater to measure time in **angle**, <!--m:\theta = \omega_1 t-->![theta = omega_1 t](edges-and-fourier.assets/eq-inline/aa02c13138.svg)<!--/m-->, so one period is
> always <!--m:0-->![0](edges-and-fourier.assets/eq-inline/b6589fc6ab.svg)<!--/m--> to <!--m:2\pi-->![2 pi](edges-and-fourier.assets/eq-inline/0833718ca4.svg)<!--/m--> no matter what <!--m:T-->![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--/m--> is. The substitution <!--m:d\theta = (2\pi/T)\,dt-->![d theta = (2 pi/T) dt](edges-and-fourier.assets/eq-inline/1b0566e043.svg)<!--/m--> turns the
> <!--m:b_n-->![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--/m--> formula into:

![with theta equal to omega_1 t, b_n equals 1 over pi times the integral from 0 to 2 pi of f of theta sin n theta d theta](edges-and-fourier.assets/eq-coeffs-angle.svg)

**Two symmetry shortcuts.** Most power-electronics waveforms have symmetries that halve the work.

- **Odd symmetry**, <!--m:f(-\theta) = -f(\theta)-->![f(- theta ) = -f( theta )](edges-and-fourier.assets/eq-inline/a51d2fe445.svg)<!--/m-->. The waveform is a mirror image through the origin, as
  a sine is. All the <!--m:a_n-->![a_n](edges-and-fourier.assets/eq-inline/278ab95d3a.svg)<!--/m--> (and <!--m:a_0-->![a_0](edges-and-fourier.assets/eq-inline/4a5997da73.svg)<!--/m-->) vanish and only sines remain, because <!--m:f\cos-->![f cos](edges-and-fourier.assets/eq-inline/624d9d5541.svg)<!--/m--> is then odd and
  integrates to zero over a symmetric interval.
- **Half-wave symmetry**, <!--m:f(\theta + \pi) = -f(\theta)-->![f( theta + pi ) = -f( theta )](edges-and-fourier.assets/eq-inline/4ed766ba65.svg)<!--/m-->. The second half-cycle is the first one
  flipped upside down. This is exactly what an H-bridge produces, because it applies <!--m:+V_{dc}-->![+V_dc](edges-and-fourier.assets/eq-inline/0458144a16.svg)<!--/m--> then
  <!--m:-V_{dc}-->![-V_dc](edges-and-fourier.assets/eq-inline/b2ceae7532.svg)<!--/m--> by symmetric switching. Substitute <!--m:\theta = s + \pi-->![theta = s + pi](edges-and-fourier.assets/eq-inline/81df2d694b.svg)<!--/m--> in the second half of the integral,
  using <!--m:\sin(n(s+\pi)) = (-1)^n \sin ns-->![sin (n(s+ pi )) = (-1)^n sin ns](edges-and-fourier.assets/eq-inline/095ed93610.svg)<!--/m-->:

![with half-wave symmetry, the integral over the second half equals minus 1 to the n plus 1 times the integral over the first half](edges-and-fourier.assets/eq-half-wave.svg)

![b_n equals 1 plus minus 1 to the n plus 1 over pi times the first-half integral: 2 over pi times the integral for odd n, and 0 for even n](edges-and-fourier.assets/eq-half-wave-result.svg)

For even <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m--> the two halves cancel exactly. **A half-wave-symmetric waveform contains only odd
harmonics.** That is why an H-bridge's output has no 2nd, 4th or 6th harmonic, and why the
interesting numbers below are all 3, 5, 7 and so on.

## 6 The square wave, derived

Now the waveform from the video. An H-bridge flipping <!--m:\pm V-->![plus-minus V](edges-and-fourier.assets/eq-inline/4545201178.svg)<!--/m--> across a load, with one period
written in angle:

![f of theta equals plus V for theta between 0 and pi, and minus V for theta between pi and 2 pi](edges-and-fourier.assets/eq-square-def.svg)

It is odd, so every <!--m:a_n = 0-->![a_n = 0](edges-and-fourier.assets/eq-inline/872e3899b1.svg)<!--/m-->. Its average is zero, so <!--m:a_0 = 0-->![a_0 = 0](edges-and-fourier.assets/eq-inline/f1977494dc.svg)<!--/m-->. Only the <!--m:b_n-->![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--/m--> remain. Split the
integral at <!--m:\theta = \pi-->![theta = pi](edges-and-fourier.assets/eq-inline/42cf7bfb68.svg)<!--/m-->, where the waveform changes sign:

![b_n equals 1 over pi times the integral from 0 to pi of V sin n theta minus the integral from pi to 2 pi of V sin n theta](edges-and-fourier.assets/eq-sq-step1.svg)

Each piece is an elementary integral, since the antiderivative of <!--m:\sin n\theta-->![sin n theta](edges-and-fourier.assets/eq-inline/f045facc17.svg)<!--/m--> is
<!--m:-\cos n\theta / n-->![- cos n theta/n](edges-and-fourier.assets/eq-inline/2c58694146.svg)<!--/m-->. Use <!--m:\cos 2n\pi = 1-->![cos 2n pi = 1](edges-and-fourier.assets/eq-inline/6c3f1e37ea.svg)<!--/m-->:

![integral from 0 to pi of sin n theta equals 1 minus cos n pi over n; integral from pi to 2 pi equals cos n pi minus 1 over n](edges-and-fourier.assets/eq-sq-step2.svg)

Subtracting the second from the first gives twice the first. Then <!--m:\cos n\pi-->![cos n pi](edges-and-fourier.assets/eq-inline/d0019ff0f1.svg)<!--/m--> is <!--m:+1-->![+1](edges-and-fourier.assets/eq-inline/acb72b9476.svg)<!--/m--> for even <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m-->
and <!--m:-1-->![-1](edges-and-fourier.assets/eq-inline/7984b0a0e1.svg)<!--/m--> for odd <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m-->, which is written <!--m:(-1)^n-->![(-1)^n](edges-and-fourier.assets/eq-inline/f21498e059.svg)<!--/m-->:

![b_n equals 2 V over n pi times 1 minus minus 1 to the n, which is 4 V over n pi for odd n and 0 for even n](edges-and-fourier.assets/eq-sq-step3.svg)

Even harmonics vanish, as the half-wave symmetry promised. Odd harmonics have amplitude
<!--m:4V/(n\pi)-->![4V/(n pi )](edges-and-fourier.assets/eq-inline/bb802859e2.svg)<!--/m-->, falling as <!--m:1/n-->![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--/m-->. Put the coefficients back into the series:

![v of t equals 4 V over pi times the sum over odd n of sin n omega_1 t over n, which is 4 V over pi times sin omega_1 t plus a third sin 3 omega_1 t plus a fifth sin 5 omega_1 t and so on](edges-and-fourier.assets/eq-sq-result.svg)

**Check it.** At a quarter period, <!--m:t = T/4-->![t = T/4](edges-and-fourier.assets/eq-inline/63e6542200.svg)<!--/m-->, the square wave is plainly <!--m:+V-->![+V](edges-and-fourier.assets/eq-inline/d84db4c8f2.svg)<!--/m-->. The series gives
<!--m:\sin(n\pi/2) = +1, -1, +1, -1, \dots-->![sin (n pi/2) = +1, -1, +1, -1,](edges-and-fourier.assets/eq-inline/c153189203.svg)<!--/m--> for <!--m:n = 1, 3, 5, 7, \dots-->![n = 1, 3, 5, 7,](edges-and-fourier.assets/eq-inline/c3167b1d69.svg)<!--/m-->. The bracket becomes Leibniz's
famous series for <!--m:\pi/4-->![pi/4](edges-and-fourier.assets/eq-inline/58f2155161.svg)<!--/m-->, and the result is exactly <!--m:V-->![V](edges-and-fourier.assets/eq-inline/c9ee5681d3.svg)<!--/m-->:

![v at T over 4 equals 4 V over pi times 1 minus a third plus a fifth minus a seventh and so on, which is 4 V over pi times pi over 4, equals V](edges-and-fourier.assets/eq-leibniz.svg)

![The first three odd sine harmonics above, and partial Fourier sums with 1, 3 and 9 terms approaching a square wave below](edges-and-fourier.assets/fig-03.svg)

_Top: the building blocks, each odd harmonic at one-<!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m-->-th the amplitude of the fundamental.
Bottom: adding them up. One term is a plain sine. Three terms already show flat-ish tops. Nine terms
give steep edges, but there is a stubborn overshoot at every jump._

Notice what the edges need. The fundamental alone has a gentle slope. Each higher harmonic
contributes a steeper and steeper wiggle, and it takes the high harmonics to make the jump sharp.
**A steep edge *is* high-frequency content.** That is the precise sense in which §3's 50 ns edge
"contains" 7 MHz. Section 8 shows that finite rise time gently rolls off the harmonics above about
<!--m:1/(\pi t_r)-->![1/( pi t_r)](edges-and-fourier.assets/eq-inline/65aa0de6e5.svg)<!--/m-->.

## 7 Reading a spectrum — Gibbs, Parseval and THD

**The spectrum.** Plotting each harmonic's amplitude against <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m--> gives the waveform's **spectrum**.
It is the same information as the time plot, sorted by frequency instead of by time. For the square
wave it is a row of bars at odd <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m--> only, falling as <!--m:4V/(n\pi)-->![4V/(n pi )](edges-and-fourier.assets/eq-inline/bb802859e2.svg)<!--/m-->:

![Amplitude spectrum of a plus or minus V square wave with bars at odd harmonics falling as 4 V over n pi](edges-and-fourier.assets/fig-05.svg)

_The fundamental stands taller than the square wave itself, at 1.27 V. The harmonics fall slowly, as
<!--m:1/n-->![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--/m-->, which is why square waves are "noisy". Their energy reaches far up the frequency axis._

The fundamental's amplitude is <!--m:4/\pi \approx 1.273-->![4/pi approx 1.273](edges-and-fourier.assets/eq-inline/5829bc02d8.svg)<!--/m--> times the square wave's height. That surprises
almost everyone the first time, so it is worth stating plainly: **the best-fitting sine to a ±V
square wave peaks at about 1.27 V, higher than the square wave ever goes.** The harmonics then pull
the tops down flat and push the shoulders up.

**Gibbs overshoot.** Adding more terms makes the partial sum converge to the square wave *everywhere
except right at the jumps*. Next to each jump the partial sum overshoots, and the overshoot does not
shrink as you add terms. It only gets narrower. In the limit it settles at about 9 % of the jump:

![the maximum of the partial sum tends to 2 over pi times Si of pi times V, about 1.179 V, an overshoot of about 9 percent of the 2 V jump](edges-and-fourier.assets/eq-gibbs.svg)

(<!--m:\operatorname{Si}-->![Si](edges-and-fourier.assets/eq-inline/3c5737d86c.svg)<!--/m--> is the sine integral, <!--m:\operatorname{Si}(x) = \int_0^x \sin u / u \, du-->![Si(x) = integral_0^x sin u/u du](edges-and-fourier.assets/eq-inline/146f14e16b.svg)<!--/m-->.) Gibbs
overshoot is a real effect, not a maths curiosity. Any time a sharp edge passes through a system that
cuts off sharply above some frequency (a brick-wall filter, a band-limited amplifier), the edge
comes out with this ringing overshoot.

**Parseval — where the power goes.** Power in a resistor goes as voltage squared. Square the series,
average over a period, and orthogonality kills every cross term between different harmonics. What
survives is the sum of each harmonic's own mean square:

![the mean of f squared equals a_0 squared plus one half the sum of a_n squared plus b_n squared](edges-and-fourier.assets/eq-parseval.svg)

This is **Parseval's theorem**. In words: *the total power is the sum of the powers in each
harmonic*, and harmonics never interfere in their power contribution. For the square wave the mean
square is obviously <!--m:V^2-->![V^2](edges-and-fourier.assets/eq-inline/13bbb9f936.svg)<!--/m-->, because <!--m:v^2 = V^2-->![v^2 = V^2](edges-and-fourier.assets/eq-inline/1421d0c4d4.svg)<!--/m--> at every instant. Parseval agrees, which needs the sum
of <!--m:1/n^2-->![1/n^2](edges-and-fourier.assets/eq-inline/dc31304943.svg)<!--/m--> over odd <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m-->, <!--m:\pi^2/8-->![pi^2/8](edges-and-fourier.assets/eq-inline/504a023cae.svg)<!--/m-->:

![V squared equals one half the sum of 16 V squared over n squared pi squared, which is 8 V squared over pi squared times pi squared over 8, equals V squared](edges-and-fourier.assets/eq-parseval-square.svg)

The fundamental's share is therefore:

![P_1 over P_total equals one half of 4 V over pi squared, over V squared, equals 8 over pi squared, about 0.811](edges-and-fourier.assets/eq-fund-fraction.svg)

About 81 % of a square wave's power is in the fundamental and 19 % is in the harmonics.

**THD.** **Total harmonic distortion** measures how far a waveform is from a pure sine: the RMS of
all the harmonics together, divided by the RMS of the fundamental. With Parseval, the harmonics'
combined mean square is the total minus the fundamental's:

![THD equals the square root of V_2 squared plus V_3 squared and so on, over V_1, which equals the square root of V_rms squared minus V_1 rms squared, over V_1 rms](edges-and-fourier.assets/eq-thd-def.svg)

For the square wave, <!--m:V_{rms} = V-->![V_rms = V](edges-and-fourier.assets/eq-inline/e515b7b30c.svg)<!--/m--> and <!--m:V_{1,rms} = (4V/\pi)/\sqrt2-->![V_1,rms = (4V/pi )/sqrt 2](edges-and-fourier.assets/eq-inline/ad8fb6ecd1.svg)<!--/m-->, so:

![THD of the square wave equals the square root of V squared minus 8 over pi squared V squared, over root 8 V over pi, equals the square root of pi squared over 8 minus 1, about 48.3 percent](edges-and-fourier.assets/eq-thd-square.svg)

A square wave has **48.3 % THD**. For comparison, South African mains is allowed at most 8 % (see
[ac-and-rms.md §7](ac-and-rms.md#7-is-mains-really-a-sine-wave)). A square wave is nowhere near
mains quality, and that is the problem the inverter's later stages exist to solve.

## 8 The H-bridge output as a Fourier series

Now the question that started this document: *can an H-bridge be represented as a Fourier series,
with variables for the MOSFET details and the lamp's resistance?* Yes, and every real-world detail
slots in as one more factor on the harmonic amplitudes. We build it up in layers.

![H-bridge driving a lamp, and its equivalent circuit during one half-cycle: the supply, two on-resistances and the lamp resistance in series](edges-and-fourier.assets/fig-06.svg)

_The video's S1 to S4 are drawn here as MOSFETs Q1 to Q4, with the same positions: Q1 top left, Q2
top right, Q3 bottom left, Q4 bottom right. The diagonal pairs Q1 with Q4 and Q2 with Q3 conduct
together. In each half-cycle the lamp current passes through two on-resistances in series with the
filament._

**Layer 1 — the ideal bridge.** Diagonal pairs alternate at 50 Hz. Q1 and Q4 put <!--m:+V_{dc}-->![+V_dc](edges-and-fourier.assets/eq-inline/0458144a16.svg)<!--/m--> across the
lamp, then Q2 and Q3 put <!--m:-V_{dc}-->![-V_dc](edges-and-fourier.assets/eq-inline/b2ceae7532.svg)<!--/m-->. This is exactly the square wave of §6 with <!--m:V = V_{dc}-->![V = V_dc](edges-and-fourier.assets/eq-inline/ad4f903633.svg)<!--/m-->:

![v_AB of t equals 4 V_dc over pi times the sum over odd n of sin n omega_1 t over n, with omega_1 equal to 2 pi times 50 Hz](edges-and-fourier.assets/eq-hb-ideal.svg)

With the video's 12 V battery:

![V_1 equals 4 over pi V_dc, about 1.273 V_dc, which for 12 V is 15.28 V peak or 10.80 V rms](edges-and-fourier.assets/eq-hb-fund.svg)

**Layer 2 — the lamp as a resistance <!--m:R-->![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--/m-->.** A resistor obeys Ohm's law at every instant and at every
frequency, so the current is the same series divided by <!--m:R-->![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--/m-->. Each harmonic of voltage drives its own
harmonic of current:

![i of t equals v_AB over R, which is 4 V_dc over pi R times the sum over odd n of sin n omega_1 t over n](edges-and-fourier.assets/eq-hb-current.svg)

The power each harmonic delivers to the lamp is its peak voltage squared over <!--m:2R-->![2R](edges-and-fourier.assets/eq-inline/f51a431ecb.svg)<!--/m--> (the factor of 2
because a sine's mean square is half its peak squared, derived in
[ac-and-rms.md §3](ac-and-rms.md#3-rms--defined-by-equal-heating)):

![P_n equals V_n squared over 2 R, which is 8 V_dc squared over n squared pi squared R](edges-and-fourier.assets/eq-hb-power-n.svg)

![the sum of P_n equals 8 V_dc squared over pi squared R times pi squared over 8, which is V_dc squared over R](edges-and-fourier.assets/eq-hb-power-total.svg)

The total is <!--m:V_{dc}^2/R-->![V_dc^2/R](edges-and-fourier.assets/eq-inline/c5cd21d089.svg)<!--/m-->, exactly what a DC supply of <!--m:V_{dc}-->![V_dc](edges-and-fourier.assets/eq-inline/1091080009.svg)<!--/m--> would deliver. A lamp's filament
cannot tell a ±12 V square wave from 12 V DC, because its glow depends on heating and <!--m:v^2-->![v^2](edges-and-fourier.assets/eq-inline/d96f95b7a2.svg)<!--/m--> is
<!--m:144\,\mathrm{V}^2-->![144 V^2](edges-and-fourier.assets/eq-inline/a4f1daad66.svg)<!--/m--> at every instant either way. Here are the numbers for a 12 V, 24 W lamp (hot
resistance <!--m:R = 12^2/24 = 6\,\Omega-->![R = 12^2/24 = 6 Omega](edges-and-fourier.assets/eq-inline/7c03b4e57b.svg)<!--/m-->):

![for R equals 6 ohms and V_dc equals 12 V: P_1 equals 19.45 W, P_3 equals 2.16 W, P_5 equals 0.78 W, P_7 equals 0.40 W, total 24 W](edges-and-fourier.assets/eq-hb-power-example.svg)

Of the 24 W, 19.45 W is carried by the 50 Hz fundamental. The other 4.55 W arrives at 150 Hz,
250 Hz, 350 Hz and up. The lamp doesn't care. A motor or a transformer does (§9).

**Layer 3 — MOSFET on-resistance.** A conducting MOSFET is not a perfect short. It behaves as a small
resistor <!--m:R_{ds(on)}-->![R_ds(on)](edges-and-fourier.assets/eq-inline/6dace34914.svg)<!--/m-->, typically a few milliohms to a few hundred. In each half-cycle the current
passes through **two** of them in series with the lamp (Q1 and Q4, or Q2 and Q3). That makes a
voltage divider:

![i equals V_dc over R plus 2 R_ds on, so v_R equals i R equals V_dc times R over R plus 2 R_ds on](edges-and-fourier.assets/eq-hb-divider.svg)

The divider is purely resistive, so it scales every harmonic by the same factor. The shape and THD
are unchanged, and only the size shrinks:

![v_R of t equals 4 V_dc over pi times R over R plus 2 R_ds on times the sum over odd n of sin n omega_1 t over n](edges-and-fourier.assets/eq-hb-rds-series.svg)

With <!--m:R_{ds(on)} = 50\,\mathrm{m}\Omega-->![R_ds(on) = 50 m Omega](edges-and-fourier.assets/eq-inline/db3d76cf57.svg)<!--/m--> and the 6 Ω lamp:

![the divider ratio is 6 over 6.1, which is 0.984; lamp power 23.2 W; conduction loss in the two switches 0.39 W](edges-and-fourier.assets/eq-hb-rds-example.svg)

So 98.4 % of the battery's power reaches the lamp, and the switches warm up by 0.39 W between them.

> **Watch out —** The lamp's resistance is not constant. A tungsten filament's cold resistance is
> roughly a tenth of its hot value, so at switch-on the current is around ten times normal for the
> first tens of milliseconds. The Fourier picture assumes steady state, with the filament hot and its
> <!--m:R-->![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--/m--> effectively constant over a 20 ms cycle (its thermal time constant is much longer). Size the
> MOSFETs for the cold inrush, not the steady-state 2 A.

**Layer 4 — zero intervals (dead time and the quasi-square wave).** Real bridges do not go straight
from <!--m:+V_{dc}-->![+V_dc](edges-and-fourier.assets/eq-inline/0458144a16.svg)<!--/m--> to <!--m:-V_{dc}-->![-V_dc](edges-and-fourier.assets/eq-inline/b2ceae7532.svg)<!--/m-->. Between turning one diagonal pair off and the other on, the controller
inserts a **dead time**, an interval with all four switches off. Without it, Q1 and Q3 (or Q2 and Q4), the two switches in one leg,
could briefly conduct together and short the supply. This is shoot-through, covered in
[../../dc-ac-inverters/h-bridge/](../../dc-ac-inverters/h-bridge/). With a resistive lamp, all
switches off means no current, so the lamp voltage is **zero** during that interval. Some inverters
also add zero intervals *deliberately*, by turning on both low-side switches, to shape the output.
Either way the waveform becomes a **quasi-square wave**: zero for an angle <!--m:\alpha-->![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--/m--> on each side of
every zero crossing.

![v_AB of theta is 0 for theta between 0 and alpha, V_dc between alpha and pi minus alpha, 0 between pi minus alpha and pi, with half-wave symmetry](edges-and-fourier.assets/eq-qs-def.svg)

It is still odd and half-wave symmetric, so only odd sines survive. Use the half-wave formula from
§5. The integrand is non-zero only between <!--m:\alpha-->![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--/m--> and <!--m:\pi - \alpha-->![pi - alpha](edges-and-fourier.assets/eq-inline/13f23193bf.svg)<!--/m-->:

![b_n equals 2 over pi times the integral from alpha to pi minus alpha of V_dc sin n theta, which is 2 V_dc over n pi times cos n alpha minus cos of n pi minus n alpha](edges-and-fourier.assets/eq-qs-step.svg)

Expand the second cosine with the angle-difference formula. For odd <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m-->, <!--m:\cos n\pi = -1-->![cos n pi = -1](edges-and-fourier.assets/eq-inline/22e9f135cb.svg)<!--/m--> and
<!--m:\sin n\pi = 0-->![sin n pi = 0](edges-and-fourier.assets/eq-inline/d9f9bc06ad.svg)<!--/m-->:

![cos of n pi minus n alpha equals cos n pi cos n alpha plus sin n pi sin n alpha, which is minus cos n alpha for odd n](edges-and-fourier.assets/eq-qs-step2.svg)

![b_n equals 4 V_dc over n pi times cos n alpha for odd n, and 0 for even n](edges-and-fourier.assets/eq-qs-result.svg)

The square-wave amplitudes are simply multiplied by <!--m:\cos(n\alpha)-->![cos (n alpha )](edges-and-fourier.assets/eq-inline/770e27da25.svg)<!--/m-->. Setting <!--m:\alpha = 0-->![alpha = 0](edges-and-fourier.assets/eq-inline/08b777d1d0.svg)<!--/m--> recovers
the square wave, as it must. The new factor lets you **choose a harmonic to delete**:

![b_n equals 0 exactly when cos n alpha equals 0, which is when n alpha equals 90 degrees, so alpha equals 90 degrees over n: alpha 30 degrees removes the 3rd, alpha 18 degrees removes the 5th](edges-and-fourier.assets/eq-qs-kill.svg)

At <!--m:\alpha = 30^\circ-->![alpha = 30^ deg](edges-and-fourier.assets/eq-inline/63580e7f38.svg)<!--/m--> the 3rd harmonic disappears, and with it every harmonic divisible by 3. The
reason is that <!--m:\cos(3(2k+1) \cdot 30^\circ) = \cos((2k+1) \cdot 90^\circ) = 0-->![cos (3(2k+1) times 30^ deg ) = cos ((2k+1) times 90^ deg ) = 0](edges-and-fourier.assets/eq-inline/b10471a0bf.svg)<!--/m-->:

![at alpha equals 30 degrees: b_1 equals 4 V_dc over pi cos 30 degrees equals 1.103 V_dc; b_3, b_9, b_15 are 0; b_5 and b_7 are minus 4 V_dc over pi times 0.866 over n](edges-and-fourier.assets/eq-qs-30.svg)

![A quasi-square wave with zero intervals of width alpha and its fundamental above, and the normalised harmonic amplitudes cos n alpha over n versus alpha below](edges-and-fourier.assets/fig-07.svg)

_Top: the quasi-square wave at 30 degrees and its fundamental. Bottom: each harmonic's amplitude as
the zero interval widens. Wherever a curve crosses zero, that harmonic is gone from the output. The
price is a smaller fundamental, which falls as <!--m:\cos\alpha-->![cos alpha](edges-and-fourier.assets/eq-inline/9f1f363b16.svg)<!--/m-->._

The RMS of a quasi-square wave follows from its definition (the waveform is <!--m:\pm V_{dc}-->![plus-minus V_dc](edges-and-fourier.assets/eq-inline/38ec47d61b.svg)<!--/m--> for a
fraction <!--m:(\pi - 2\alpha)/\pi-->![( pi - 2 alpha )/pi](edges-and-fourier.assets/eq-inline/14c93c4ed9.svg)<!--/m--> of the time and zero otherwise):

![V_rms equals the square root of 1 over pi times the integral from alpha to pi minus alpha of V_dc squared, which is V_dc times the square root of pi minus 2 alpha over pi](edges-and-fourier.assets/eq-qs-rms.svg)

and so its THD at <!--m:\alpha = 30^\circ-->![alpha = 30^ deg](edges-and-fourier.assets/eq-inline/63580e7f38.svg)<!--/m--> is:

![THD at 30 degrees equals the square root of two thirds minus 1.103 over root 2 squared, over 1.103 over root 2, which is about 31.1 percent](edges-and-fourier.assets/eq-qs-thd-30.svg)

Removing the triplen harmonics cuts THD from 48.3 % to 31.1 %. This is the simplest example of
**selective harmonic elimination**, and the seed of the idea behind SPWM: place the switching
instants to put zeros in the spectrum where you want them.

**The "modified sine wave" inverter.** Cheap inverters sold as "modified sine" are quasi-square
waves. They choose <!--m:\alpha-->![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--/m--> so that the peak *and* the RMS both match mains, 325 V peak and 230 V RMS:

![V_rms over V_pk equals the square root of pi minus 2 alpha over pi, equals 230 over 325, which is 1 over root 2, so alpha equals 45 degrees](edges-and-fourier.assets/eq-modified-sine.svg)

At <!--m:\alpha = 45^\circ-->![alpha = 45^ deg](edges-and-fourier.assets/eq-inline/d888911a73.svg)<!--/m--> the 3rd harmonic is *not* removed (<!--m:\cos 135^\circ = -0.707-->![cos 135^ deg = -0.707](edges-and-fourier.assets/eq-inline/7c81fd1f8a.svg)<!--/m-->). Working it
through, the THD comes out at 48.3 %, the same as a plain square wave. The modified sine wave fixes
the peak and the RMS. It does not fix the distortion.

**How big is protective dead time, in angle?** Dead time <!--m:t_d-->![t_d](edges-and-fourier.assets/eq-inline/6c703960eb.svg)<!--/m--> makes a zero interval of width <!--m:t_d-->![t_d](edges-and-fourier.assets/eq-inline/6c703960eb.svg)<!--/m-->
at each transition. In the notation above that interval is <!--m:2\alpha-->![2 alpha](edges-and-fourier.assets/eq-inline/008b07ecdf.svg)<!--/m--> wide:

![2 alpha equals 360 degrees times t_d over T, so alpha equals 180 degrees times t_d over T: for t_d equals 500 ns, alpha is 0.0045 degrees at 50 Hz and 4.5 degrees at 50 kHz](edges-and-fourier.assets/eq-dead-alpha.svg)

At 50 Hz, protective dead time is spectrally invisible. At the 50 kHz switching of the inverter's
first stage it is a 4.5° zero interval. That trims the fundamental by <!--m:1 - \cos 4.5^\circ \approx 0.3\,\%-->![1 - cos 4.5^ deg approx 0.3 %](edges-and-fourier.assets/eq-inline/eaab2731ca.svg)<!--/m-->
and starts reshaping the harmonics, so it matters there.

**Layer 5 — finite edges.** The last idealisation is the vertical edge. A trapezoidal wave with rise
time <!--m:t_r-->![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--/m--> is a square wave smoothed by a moving average of width <!--m:t_r-->![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--/m-->. A moving average multiplies
each harmonic by a <!--m:\sin x / x-->![sin x/x](edges-and-fourier.assets/eq-inline/6dcfc84d85.svg)<!--/m--> factor. Below the corner <!--m:f_c = 1/(\pi t_r)-->![f_c = 1/( pi t_r)](edges-and-fourier.assets/eq-inline/913d3b6838.svg)<!--/m--> the factor is about 1.
Above it, the harmonics fall as <!--m:1/n^2-->![1/n^2](edges-and-fourier.assets/eq-inline/dc31304943.svg)<!--/m--> instead of <!--m:1/n-->![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--/m-->:

![b_n trapezoid is approximately 4 V over n pi times sin of n pi t_r over T, over n pi t_r over T, with a corner at f_c equal to 1 over pi t_r](edges-and-fourier.assets/eq-trapezoid.svg)

For a 50 ns edge, <!--m:f_c \approx 6.4\,\mathrm{MHz}-->![f_c approx 6.4 MHz](edges-and-fourier.assets/eq-inline/2ee34ef0f4.svg)<!--/m-->, which is roughly the 127 000th harmonic of 50 Hz.
It is irrelevant to the lamp and decisive for radio-frequency interference.

**Everything together.** Stacking all five layers gives the H-bridge-into-a-lamp output, with every
variable visible:

![v_lamp of t equals the sum over odd n of: 4 V_dc over n pi (square wave), times R over R plus 2 R_ds on (MOSFET divider), times cos n alpha (zero intervals), times sin of n pi t_r over T over n pi t_r over T (finite edges), times sin n omega_1 t](edges-and-fourier.assets/eq-full.svg)

Each factor is something you can change. Raise <!--m:V_{dc}-->![V_dc](edges-and-fourier.assets/eq-inline/1091080009.svg)<!--/m--> and everything scales. Choose better
MOSFETs and the divider factor approaches 1. Choose <!--m:\alpha-->![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--/m--> to place zeros. Slow the edges to tame
the radio-frequency tail, at the cost of switching loss.

## 9 Why the harmonics matter downstream

A lamp is the one load that genuinely doesn't care: heat is heat. Almost everything else in the
inverter chain responds to frequency, so the harmonics of §8 become real costs.

- **Transformers.** Each harmonic adds its own core loss (eddy-current loss per unit of flux rises
  steeply with frequency) and audible hum, so a 50 Hz transformer fed a square wave runs warmer and
  noisier than on a sine of the same RMS. The fast edges also push
  <!--m:C\,dv/dt-->![C dv/dt](edges-and-fourier.assets/eq-inline/b96b608a56.svg)<!--/m--> current through the interwinding capacitance (§3), straight into the secondary. And a
  transformer cannot pass the DC offset an asymmetric bridge might produce. See
  [../transformer/](../transformer/) and [../electromagnetism/](../electromagnetism/).
- **Inductive loads (motors, fans).** An inductor's impedance is <!--m:\omega L-->![omega L](edges-and-fourier.assets/eq-inline/b3beb438d7.svg)<!--/m-->. Harmonic <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m--> has <!--m:1/n-->![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--/m--> of
  the fundamental's voltage and meets <!--m:n-->![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--/m--> times the impedance, so it draws <!--m:1/n^2-->![1/n^2](edges-and-fourier.assets/eq-inline/dc31304943.svg)<!--/m--> of the
  fundamental's current. The harmonic currents are small, but they make torque ripple, buzzing and
  extra copper heating.
- **Filters.** To turn a square wave into a sine you must remove the 3rd harmonic at 150 Hz while
  keeping the fundamental at 50 Hz. Those are only a factor of 3 apart. An LC low-pass filter
  attenuates 40 dB per decade above its corner (see [../../filters/lc-filter/](../../filters/lc-filter/)),
  so separating 50 Hz from 150 Hz needs a corner squeezed between them and enormous L and C.

That last point is the motivation for everything after the H-bridge in this tree. **Pulse-width
modulation** ([../../pwm/](../../pwm/)) and its sinusoidal form **SPWM**
([../../dc-ac-inverters/spwm/](../../dc-ac-inverters/spwm/)) switch the bridge at tens of kilohertz
and vary the pulse widths so that the low-order harmonics nearly vanish. The unwanted energy is
pushed up to clusters around the switching frequency, far from 50 Hz, where a small LC filter removes
it easily. The Fourier series is the tool that both predicts this and proves it works.

## 10 What this costs you

- **Fast edges trade switching loss for noise.** Faster edges waste less energy in the MOSFET but
  inject more <!--m:C\,dv/dt-->![C dv/dt](edges-and-fourier.assets/eq-inline/b96b608a56.svg)<!--/m--> current and <!--m:L\,di/dt-->![L di/dt](edges-and-fourier.assets/eq-inline/f24cc20a0b.svg)<!--/m--> ringing (§3). Every real design picks an edge speed,
  usually with a gate resistor, as a compromise. There is no setting that is both efficient and
  quiet.
- **The Fourier series assumes steady state and linearity.** The derivation needs a waveform that
  repeats forever, into a circuit whose R, L and C do not change with voltage or current. Start-up
  transients, a filament heating up and a saturating core all break those assumptions. The series
  then describes an idealisation, not the measured waveform.
- **Truncation and Gibbs.** Any practical calculation keeps finitely many harmonics. Near jumps the
  partial sum overshoots by about 9 % no matter how many terms you keep (§7). Don't mistake that
  overshoot for a real voltage spike, and do expect it from any sharply band-limited system.
- **Ideal zero intervals assume a resistive load.** With an inductive load the current keeps flowing
  through the body diodes during dead time, and the voltage is <!--m:\pm V_{dc}-->![plus-minus V_dc](edges-and-fourier.assets/eq-inline/38ec47d61b.svg)<!--/m--> (depending on current
  direction), not zero. The clean <!--m:\cos(n\alpha)-->![cos (n alpha )](edges-and-fourier.assets/eq-inline/770e27da25.svg)<!--/m--> result then no longer holds exactly.
- **Harmonic elimination costs fundamental.** Choosing <!--m:\alpha-->![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--/m--> to kill a harmonic also shrinks
  the fundamental by <!--m:\cos\alpha-->![cos alpha](edges-and-fourier.assets/eq-inline/9f1f363b16.svg)<!--/m-->. At <!--m:\alpha = 30^\circ-->![alpha = 30^ deg](edges-and-fourier.assets/eq-inline/63580e7f38.svg)<!--/m--> you lose 13.4 % of the useful output to
  remove the 3rd.

## 11 Sources and cross-links

- **The two laws behind §2–3:** [../inductor/inductor.md](../inductor/inductor.md)
  (<!--m:v = L\,di/dt-->![v = L di/dt](edges-and-fourier.assets/eq-inline/169359fd71.svg)<!--/m-->, the inductive kick) and [../capacitor/capacitor.md](../capacitor/capacitor.md)
  (<!--m:i = C\,dv/dt-->![i = C dv/dt](edges-and-fourier.assets/eq-inline/7f5f5ec54b.svg)<!--/m-->).
- **The companion document:** [ac-and-rms.md](ac-and-rms.md). It explains sinusoids, averages, why
  RMS uses <!--m:\sqrt2-->![sqrt 2](edges-and-fourier.assets/eq-inline/6d0fdf0909.svg)<!--/m-->, and why 230 V mains peaks at 325 V.
- **The H-bridge itself:** [../../dc-ac-inverters/h-bridge/](../../dc-ac-inverters/h-bridge/). It
  covers switch states, shoot-through, dead time, gate drive and body diodes.
- **Where the harmonics go next:** [../../pwm/](../../pwm/),
  [../../dc-ac-inverters/spwm/](../../dc-ac-inverters/spwm/),
  [../../filters/lc-filter/](../../filters/lc-filter/), [../transformer/](../transformer/) and
  [../../rectifiers/](../../rectifiers/).
- The same volt-second, switch-node square waves appear in
  [../../dc-dc-converters/buck/buck.md](../../dc-dc-converters/buck/buck.md).
- Standard references: any signals-and-systems text (for example Oppenheim and Willsky, *Signals
  and Systems*) for the series and Parseval. Mohan, Undeland and Robbins, *Power Electronics*, for
  square-wave and quasi-square-wave inverter harmonics. Johnson and Graham, *High-Speed Digital
  Design*, for the <!--m:0.35/t_r-->![0.35/t_r](edges-and-fourier.assets/eq-inline/74e3f4690c.svg)<!--/m--> bandwidth rule.
- Source video: *DC_to_AC_1.mp4* (12 V battery, H-bridge S1 to S4, lamp, ±12 V square wave).
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
