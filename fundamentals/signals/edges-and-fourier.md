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
function ![u(t)](edges-and-fourier.assets/eq-inline/843ca7f6da.svg)<!--m:u(t)-->:

![v of t equals V times u of t, where u is 0 before t equals 0 and 1 after](edges-and-fourier.assets/eq-step.svg)

The ideal step goes from 0 to ![V](edges-and-fourier.assets/eq-inline/c9ee5681d3.svg)<!--m:V--> in zero time, so its slope ![dv/dt](edges-and-fourier.assets/eq-inline/7631ac0fee.svg)<!--m:dv/dt--> at ![t = 0](edges-and-fourier.assets/eq-inline/fee440f68f.svg)<!--m:t = 0--> is
**infinite**. No physical voltage can do that, for a reason this whole tree keeps coming back to.
Every real node has some capacitance, and the capacitor law ![i_C = C dv/dt](edges-and-fourier.assets/eq-inline/37a5867086.svg)<!--m:i_C = C \, dv/dt-->
([../capacitor/capacitor.md](../capacitor/capacitor.md)) says an infinite ![dv/dt](edges-and-fourier.assets/eq-inline/7631ac0fee.svg)<!--m:dv/dt--> needs infinite
current. Real edges therefore have a finite **rise time** ![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--m:t_r--> (and **fall time** ![t_f](edges-and-fourier.assets/eq-inline/1f679eb63d.svg)<!--m:t_f-->).

**The 10–90 % rule.** Real edges approach their final value gradually, curving in at the start and
creeping in at the end. That makes "start" and "end" hard to pin down. So by convention the rise time
is measured from the moment the signal crosses **10 %** of its swing to the moment it crosses **90 %**.
The fall time is measured from 90 % down to 10 %. Oscilloscopes measure it this way automatically.

The cleanest worked case is a step driven through a resistance ![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--m:R--> into a capacitance ![C](edges-and-fourier.assets/eq-inline/32096c2e0e.svg)<!--m:C--> (an "RC
edge"). This is roughly what a gate driver charging a MOSFET gate looks like. Where the exponential
comes from: when the capacitor has reached ![v](edges-and-fourier.assets/eq-inline/7a38d8cbd2.svg)<!--m:v-->, the resistor has the rest, ![V - v](edges-and-fourier.assets/eq-inline/4033043807.svg)<!--m:V - v-->, across it
(Kirchhoff's voltage law,
[../electromagnetism/electromagnetism.md §1](../electromagnetism/electromagnetism.md#1-charge-current-and-the-electric-field)),
so it passes ![i = (V - v)/R](edges-and-fourier.assets/eq-inline/13e4e1ae10.svg)<!--m:i = (V - v)/R--> by Ohm's law. All of that current flows on into the capacitor
(Kirchhoff's current law), and the capacitor law turns it into a rate of rise:
![C dv/dt = (V - v)/R](edges-and-fourier.assets/eq-inline/d7815fb502.svg)<!--m:C\,dv/dt = (V - v)/R-->. Write ![tau = RC](edges-and-fourier.assets/eq-inline/3f3c99c08b.svg)<!--m:\tau = RC-->, put everything with ![v](edges-and-fourier.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> on one side, and integrate from
the empty capacitor (![v = 0](edges-and-fourier.assets/eq-inline/c7c64bf3d5.svg)<!--m:v = 0--> at ![t = 0](edges-and-fourier.assets/eq-inline/fee440f68f.svg)<!--m:t = 0-->):

![C dv by dt equals V minus v over R, so dv over V minus v equals dt over tau, so minus the natural log of V minus v over V equals t over tau](edges-and-fourier.assets/eq-rc-ode.svg)

Undo the logarithm and solve for ![v](edges-and-fourier.assets/eq-inline/7a38d8cbd2.svg)<!--m:v-->. The **time constant** ![tau = RC](edges-and-fourier.assets/eq-inline/3f3c99c08b.svg)<!--m:\tau = RC--> is in seconds (ohms times
farads is ![( V/A)( A times s/V) = s](edges-and-fourier.assets/eq-inline/b7c1fd31c5.svg)<!--m:(\mathrm{V/A})(\mathrm{A\cdot s/V}) = \mathrm{s}-->):

![v of t equals V times 1 minus e to the minus t over tau, with tau equal to R C](edges-and-fourier.assets/eq-rc-edge.svg)

Find the two crossing times by setting ![v](edges-and-fourier.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> to 10 % and 90 % of ![V](edges-and-fourier.assets/eq-inline/c9ee5681d3.svg)<!--m:V--> and solving for ![t](edges-and-fourier.assets/eq-inline/8efd86fb78.svg)<!--m:t-->:

![solving for t_10 equals tau ln 10 over 9 and t_90 equals tau ln 10](edges-and-fourier.assets/eq-t10-t90.svg)

Subtract them. The logarithms combine because ![ln a - ln b = ln (a/b)](edges-and-fourier.assets/eq-inline/98f56d6dfe.svg)<!--m:\ln a - \ln b = \ln(a/b)-->:

![t_r equals t_90 minus t_10 equals tau ln 9, approximately 2.2 tau](edges-and-fourier.assets/eq-rise-time.svg)

So an RC edge's rise time is about ![2.2 tau](edges-and-fourier.assets/eq-inline/4bfa788429.svg)<!--m:2.2\,\tau-->. Differentiating shows the edge is steepest at the
very start, where its slope is ![V/tau](edges-and-fourier.assets/eq-inline/720e3621cb.svg)<!--m:V/\tau-->. A straight line at that slope would reach the final value
after exactly one ![tau](edges-and-fourier.assets/eq-inline/c9148a5f77.svg)<!--m:\tau-->, and that is the tangent drawn in Figure 20:

![dv by dt equals V over tau times e to the minus t over tau, so the slope at t equals 0 is V over tau](edges-and-fourier.assets/eq-rc-slope.svg)

![An ideal step versus a real rising and falling edge with the 10 to 90 percent rise time marked](edges-and-fourier.assets/fig-01.svg)

_The dashed ideal step has zero width. The real edge needs about 2.2 time constants to cover the
middle 80 % of its swing, and the same again on the way down._

**Slew rate.** The other way to describe an edge is by its steepness, the **slew rate** (SR), in volts
per second (usually V/ns or V/µs). Over the 10–90 % portion the signal covers ![0.8 V](edges-and-fourier.assets/eq-inline/939fd1e31b.svg)<!--m:0.8\,V--> in ![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--m:t_r-->:

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
voltage is swinging, the gate driver has to supply the gate-drain charge ![Q_gd](edges-and-fourier.assets/eq-inline/748697ddf2.svg)<!--m:Q_{gd}-->, and the gate
voltage stays flat until it has. With a gate-drive current ![I_g](edges-and-fourier.assets/eq-inline/e707592b00.svg)<!--m:I_g-->, the drain edge takes roughly:

![t_sw approximately Q_gd over I_g equals 10 nC over 1 A equals 10 ns](edges-and-fourier.assets/eq-miller.svg)

Double the driver current and the edge is twice as fast. That is why gate drivers are rated in amps
even though the gate draws no steady current.

**Parasitic capacitance at the switching node.** The switching node (the H-bridge midpoint, or the
buck's switch node) has capacitance to everything around it. That includes the MOSFETs' own output
capacitance ![C_oss](edges-and-fourier.assets/eq-inline/076d485f98.svg)<!--m:C_{oss}-->, the PCB copper, the heatsink and the transformer windings. The current
available to charge it is finite, so by the capacitor law the slew rate is capped:

![dv by dt equals i over C_node, so t_r is approximately C_node V over I](edges-and-fourier.assets/eq-cap-limit.svg)

**Inductance in the current loop.** Every wire and PCB trace is a one-turn inductor. A few
centimetres of loop is tens of nanohenries. By the inductor law ![v_L = L di/dt](edges-and-fourier.assets/eq-inline/da690c1e1b.svg)<!--m:v_L = L \, di/dt-->
([../inductor/inductor.md](../inductor/inductor.md)), current cannot change instantly in it either.
The current edge is slowed, and any attempt to force it fast shows up as a voltage spike (§3).

> **Note —** So the ideal step is not just "very fast". It is physically forbidden, by the same two
> laws that run every converter in this tree. Any real node has some ![C](edges-and-fourier.assets/eq-inline/32096c2e0e.svg)<!--m:C-->, so ![v](edges-and-fourier.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> cannot jump. Any
> real loop has some ![L](edges-and-fourier.assets/eq-inline/d160e0986a.svg)<!--m:L-->, so ![i](edges-and-fourier.assets/eq-inline/042dc4512f.svg)<!--m:i--> cannot jump. Real edges are the compromise between how hard you
> drive and how much ![L](edges-and-fourier.assets/eq-inline/d160e0986a.svg)<!--m:L--> and ![C](edges-and-fourier.assets/eq-inline/32096c2e0e.svg)<!--m:C--> is in the way.

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
inductive kick in [../inductor/inductor.md §7](../inductor/inductor.md#7-the-inductive-kick-and-why-the-diode-is-there).
The loop inductance and the node capacitance then form an LC tank. Kicked by the edge, it **rings**
at its natural frequency. Where that frequency comes from: in the tank the same current ![i](edges-and-fourier.assets/eq-inline/042dc4512f.svg)<!--m:i--> flows
through both parts, and the inductor's voltage is the capacitor's voltage reversed, so
![L di/dt = -v](edges-and-fourier.assets/eq-inline/7b385bf2b9.svg)<!--m:L\,di/dt = -v--> and ![C dv/dt = i](edges-and-fourier.assets/eq-inline/528d051b56.svg)<!--m:C\,dv/dt = i-->. Differentiate the second and substitute the first:

![C d squared v by dt squared equals di by dt equals minus v over L, so d squared v by dt squared equals minus v over L C, solved by v equals V peak sin omega_0 t with omega_0 equal 1 over root L C](edges-and-fourier.assets/eq-ring-ode.svg)

A sine is the function whose second derivative is minus itself times a constant (differentiating
![sin omega_0 t](edges-and-fourier.assets/eq-inline/544719e1ec.svg)<!--m:\sin\omega_0 t--> twice brings out ![- omega_0^2](edges-and-fourier.assets/eq-inline/d4f745b759.svg)<!--m:-\omega_0^2-->), so the voltage oscillates at the angular frequency
![omega_0 = 1/sqrt LC](edges-and-fourier.assets/eq-inline/f11b5cd8a1.svg)<!--m:\omega_0 = 1/\sqrt{LC}--> radians per second. Dividing by the ![2 pi](edges-and-fourier.assets/eq-inline/0833718ca4.svg)<!--m:2\pi--> radians in one cycle (§4) gives
the ringing frequency in hertz:

![f_ring equals 1 over 2 pi root L_loop C_oss, about 35.6 MHz](edges-and-fourier.assets/eq-ring.svg)

![A switching edge with inductive overshoot and ringing above, and the capacitive current pulses C dv/dt it drives below](edges-and-fourier.assets/fig-02.svg)

_Top: the overshoot and ringing come from loop inductance and node capacitance. Bottom: current
flows into a capacitance only while the voltage is moving. A flat top draws nothing, and a fast
edge draws a tall pulse._

**The bandwidth of an edge.** An edge also contains high frequencies, and the rise time tells you
how high. An RC network passes slow sine waves and attenuates fast ones. To see by how much, suppose
the capacitor voltage is a sine, ![v_C = A sin omega t](edges-and-fourier.assets/eq-inline/aa94dc2123.svg)<!--m:v_C = A\sin\omega t-->, where ![omega = 2 pi f](edges-and-fourier.assets/eq-inline/10f7ad86c0.svg)<!--m:\omega = 2\pi f--> is the angular
frequency in radians per second (§4 below). The capacitor law gives its current,
![i = C dv_C/dt = omega C A cos omega t](edges-and-fourier.assets/eq-inline/7b61febc24.svg)<!--m:i = C\,dv_C/dt = \omega C A\cos\omega t-->; the resistor drops ![iR = omega tau A cos omega t](edges-and-fourier.assets/eq-inline/18fae3f5c4.svg)<!--m:iR = \omega\tau A\cos\omega t-->; and by
Kirchhoff's voltage law the input is the sum of the two. A sine plus a cosine is a single sine whose
amplitude is the Pythagorean sum (![a sin x + b cos x = sqrt a^2 + b^2 sin (x + phi )](edges-and-fourier.assets/eq-inline/21622640c8.svg)<!--m:a\sin x + b\cos x = \sqrt{a^2 + b^2}\,\sin(x + \varphi)-->, the
angle-sum formula read backwards), so:

![v_in equals A times sin omega t plus omega tau cos omega t, so the amplitude ratio of v_C to v_in is 1 over the square root of 1 plus omega tau squared](edges-and-fourier.assets/eq-rc-gain.svg)

At ![omega tau = 1](edges-and-fourier.assets/eq-inline/c4d1fa2d7c.svg)<!--m:\omega\tau = 1--> the output amplitude is ![1/sqrt 2](edges-and-fourier.assets/eq-inline/a70612754f.svg)<!--m:1/\sqrt2--> of the input, which is half the power.
Ratios like this are quoted in **decibels**: ![20 log_10](edges-and-fourier.assets/eq-inline/fc732c015f.svg)<!--m:20\log_{10}--> of the amplitude ratio, so
![20 log_10(1/sqrt 2) approx -3 dB](edges-and-fourier.assets/eq-inline/347ad47d7f.svg)<!--m:20\log_{10}(1/\sqrt2) \approx -3\ \mathrm{dB}-->, and a factor of 10 in amplitude is 20 dB. The
frequency where it happens is the network's **−3 dB corner**, ![f_3 dB = 1/(2 pi tau )](edges-and-fourier.assets/eq-inline/7f8aa3ccc5.svg)<!--m:f_{3\mathrm{dB}} = 1/(2\pi\tau)-->: it
passes frequencies below it and increasingly attenuates those above. Combine that with ![t_r = tau ln 9](edges-and-fourier.assets/eq-inline/4fa73506c0.svg)<!--m:t_r = \tau \ln 9--> and you get one of the most used rules of thumb in electronics:

![f_3dB equals 1 over 2 pi tau and t_r equals tau ln 9, so t_r times f_3dB equals ln 9 over 2 pi, about 0.35](edges-and-fourier.assets/eq-bandwidth.svg)

A 50 ns edge carries significant content up to about ![0.35/50 ns = 7 MHz](edges-and-fourier.assets/eq-inline/b428f93b71.svg)<!--m:0.35/50\,\mathrm{ns} = 7\,\mathrm{MHz}-->, even
when the waveform repeats at only 50 Hz. To see *why* a fast edge contains high frequencies, we need
the Fourier series.

## 4 The Fourier idea — building shapes out of sines

**Periodic functions.** A signal is **periodic** if it repeats exactly every ![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T--> seconds. ![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T--> is the
**period**, and its inverse is the **fundamental frequency** ![f_1](edges-and-fourier.assets/eq-inline/0b35cc5e94.svg)<!--m:f_1-->:

![f of t plus T equals f of t for all t, f_1 equals 1 over T, omega_1 equals 2 pi f_1 equals 2 pi over T](edges-and-fourier.assets/eq-periodic.svg)

The angular frequency ![omega_1 = 2 pi f_1](edges-and-fourier.assets/eq-inline/fb69270322.svg)<!--m:\omega_1 = 2\pi f_1--> (in radians per second) just counts one full cycle as
![2 pi](edges-and-fourier.assets/eq-inline/0833718ca4.svg)<!--m:2\pi--> radians instead of 1. That makes sine waves tidy: ![sin ( omega_1 t)](edges-and-fourier.assets/eq-inline/d0650a0829.svg)<!--m:\sin(\omega_1 t)--> completes exactly one
cycle as ![t](edges-and-fourier.assets/eq-inline/8efd86fb78.svg)<!--m:t--> goes from 0 to ![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T-->.

**The claim.** Joseph Fourier's claim (1807) was that *any* reasonable periodic function, square,
triangle, sawtooth or whatever shape, can be written as a sum of sines and cosines whose frequencies
are whole-number multiples of ![f_1](edges-and-fourier.assets/eq-inline/0b35cc5e94.svg)<!--m:f_1-->:

![f of t equals a_0 plus the sum from n equals 1 to infinity of a_n cos n omega_1 t plus b_n sin n omega_1 t](edges-and-fourier.assets/eq-series.svg)

Here is what each piece means:

- **![a_0](edges-and-fourier.assets/eq-inline/4a5997da73.svg)<!--m:a_0-->** is a constant offset, the **DC component**. It is the signal's average value.
- **![n = 1](edges-and-fourier.assets/eq-inline/92ee913214.svg)<!--m:n = 1-->** is the **fundamental**. It repeats at the same rate as the signal itself.
- **![n = 2, 3, 4,](edges-and-fourier.assets/eq-inline/a0dd14301c.svg)<!--m:n = 2, 3, 4, \dots-->** are the **harmonics**. The ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n-->-th harmonic oscillates ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> times per period,
  at frequency ![n f_1](edges-and-fourier.assets/eq-inline/ef548d158f.svg)<!--m:n f_1-->. For 50 Hz the 3rd harmonic is 150 Hz and the 5th is 250 Hz.
- **![a_n](edges-and-fourier.assets/eq-inline/278ab95d3a.svg)<!--m:a_n--> and ![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--m:b_n-->** are the **coefficients**: *how much* of each cosine and sine the recipe needs.
  Finding them is the whole job.

Only whole-number multiples can appear, and the reason is simple. Every term must itself repeat every
![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T-->, otherwise the sum would not repeat every ![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T-->. A sine at ![2.5 f_1](edges-and-fourier.assets/eq-inline/7168feadac.svg)<!--m:2.5 f_1--> does not fit a whole number of
cycles into ![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T-->, so it can't be part of the recipe.

**Why sines and not some other building block?** Because sines are the one shape that linear
circuits leave alone. Differentiate ![sin omega t](edges-and-fourier.assets/eq-inline/22c43934a5.svg)<!--m:\sin\omega t--> and you get ![omega cos omega t](edges-and-fourier.assets/eq-inline/44e70d9fed.svg)<!--m:\omega \cos\omega t-->: the same
frequency, just scaled and shifted. Integrate it and the same thing happens. Inductors differentiate
current (![v = L di/dt](edges-and-fourier.assets/eq-inline/169359fd71.svg)<!--m:v = L\,di/dt-->) and capacitors differentiate voltage (![i = C dv/dt](edges-and-fourier.assets/eq-inline/7f5f5ec54b.svg)<!--m:i = C\,dv/dt-->). So a sine going into
any network of R, L and C comes out as **a sine of the same frequency**, only bigger or smaller and
shifted in time. No other waveform has this property. A square wave into an RC filter comes out
rounded. That is a different *shape*, not just a different size. So the strategy is: break any
waveform into sines, push each sine through the circuit separately (easy), and add the results.
That is the whole reason Fourier series run electrical engineering.

## 5 Orthogonality — the trick that makes it work

The series is useless unless we can find the coefficients. The tool is a set of integrals called the
**orthogonality relations**. We derive them from nothing.

**Step 1 — a cosine integrates to zero over whole cycles.** For any non-zero whole number ![k](edges-and-fourier.assets/eq-inline/13fbd79c3d.svg)<!--m:k-->,
![cos (k omega_1 t)](edges-and-fourier.assets/eq-inline/c4aff40c35.svg)<!--m:\cos(k\omega_1 t)--> fits exactly ![k](edges-and-fourier.assets/eq-inline/13fbd79c3d.svg)<!--m:k--> whole cycles into one period. Its positive humps and negative
troughs cancel:

![integral from 0 to T of cos k omega_1 t dt equals sine over k omega_1 evaluated, which is sin 2 pi k over k omega_1, equals 0](edges-and-fourier.assets/eq-cos-integral.svg)

The last step uses ![omega_1 T = 2 pi](edges-and-fourier.assets/eq-inline/119c8f6d73.svg)<!--m:\omega_1 T = 2\pi-->, so ![sin (k omega_1 T) = sin (2 pi k) = 0](edges-and-fourier.assets/eq-inline/28721a7aa8.svg)<!--m:\sin(k\omega_1 T) = \sin(2\pi k) = 0--> for every whole
number ![k](edges-and-fourier.assets/eq-inline/13fbd79c3d.svg)<!--m:k-->. The same argument shows ![sin (k omega_1 t)](edges-and-fourier.assets/eq-inline/d366841a87.svg)<!--m:\sin(k\omega_1 t)--> also integrates to zero over a period.

**Step 2 — turn products into sums.** We will need to integrate *products* of two sinusoids. The
trigonometric product-to-sum identities turn a product into a sum of single cosines (or sines), each
of which Step 1 can kill. They come straight from adding and subtracting the angle-sum formulas,
for example ![cos (A-B) - cos (A+B) = 2 sin A sin B](edges-and-fourier.assets/eq-inline/c859e3def0.svg)<!--m:\cos(A-B) - \cos(A+B) = 2\sin A \sin B-->:

![sin A sin B equals one half of cos A minus B minus cos A plus B; cos A cos B equals one half of cos A minus B plus cos A plus B; sin A cos B equals one half of sin A minus B plus sin A plus B](edges-and-fourier.assets/eq-product-to-sum.svg)

**Step 3 — integrate a product of two harmonics.** Take harmonics ![m](edges-and-fourier.assets/eq-inline/6b0d31c0d5.svg)<!--m:m--> and ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> (both positive whole
numbers). Apply the first identity:

![integral of sin m omega_1 t sin n omega_1 t equals one half integral of cos m minus n omega_1 t minus one half integral of cos m plus n omega_1 t](edges-and-fourier.assets/eq-orth-sin.svg)

The second integral always vanishes by Step 1, because ![m + n](edges-and-fourier.assets/eq-inline/3b69d04ea5.svg)<!--m:m + n--> is a non-zero whole number. The first
vanishes too, *unless* ![m = n](edges-and-fourier.assets/eq-inline/e6ad14b62e.svg)<!--m:m = n-->. Then ![m - n = 0](edges-and-fourier.assets/eq-inline/499e45ffd9.svg)<!--m:m - n = 0-->, the cosine is ![cos 0 = 1](edges-and-fourier.assets/eq-inline/7f28a78900.svg)<!--m:\cos 0 = 1-->, and its integral is
just the length of the interval, ![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T-->:

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
![sin (m omega_1 t)](edges-and-fourier.assets/eq-inline/6a7a96adf1.svg)<!--m:\sin(m\omega_1 t)--> for some chosen ![m](edges-and-fourier.assets/eq-inline/6b0d31c0d5.svg)<!--m:m--> and integrate over one period. On the right-hand side every
term is a product of two sinusoids, and orthogonality kills all of them except the single term where
the harmonic matches:

![integral of f times sin m omega_1 t equals a_0 times 0 plus sum of a_n times 0 plus sum of b_n times the sine integral, which equals b_m T over 2](edges-and-fourier.assets/eq-extract.svg)

Solve for ![b_m](edges-and-fourier.assets/eq-inline/dc8edb5b1e.svg)<!--m:b_m-->. Repeating the same move with ![cos (m omega_1 t)](edges-and-fourier.assets/eq-inline/88b75bcce5.svg)<!--m:\cos(m\omega_1 t)--> gives ![a_m](edges-and-fourier.assets/eq-inline/d5c812e96c.svg)<!--m:a_m-->, and integrating the
series by itself (multiplying by 1) gives ![a_0](edges-and-fourier.assets/eq-inline/4a5997da73.svg)<!--m:a_0-->:

![a_0 equals 1 over T times integral of f; a_n equals 2 over T times integral of f cos n omega_1 t; b_n equals 2 over T times integral of f sin n omega_1 t](edges-and-fourier.assets/eq-coeffs.svg)

These are the **Fourier coefficient formulas**. Read ![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--m:b_n--> in words: *multiply the signal by a test
sine at frequency ![n f_1](edges-and-fourier.assets/eq-inline/ef548d158f.svg)<!--m:n f_1--> and average*. If the signal contains that sine, the product has a steady
positive part and the average is non-zero. If it does not, the product swings both ways and averages
to zero. A Fourier analyser, from a pencil to a spectrum analyser, is doing exactly this.

> **Tip —** It is often neater to measure time in **angle**, ![theta = omega_1 t](edges-and-fourier.assets/eq-inline/aa02c13138.svg)<!--m:\theta = \omega_1 t-->, so one period is
> always ![0](edges-and-fourier.assets/eq-inline/b6589fc6ab.svg)<!--m:0--> to ![2 pi](edges-and-fourier.assets/eq-inline/0833718ca4.svg)<!--m:2\pi--> no matter what ![T](edges-and-fourier.assets/eq-inline/c2c53d6694.svg)<!--m:T--> is. The substitution ![d theta = (2 pi/T) dt](edges-and-fourier.assets/eq-inline/1b0566e043.svg)<!--m:d\theta = (2\pi/T)\,dt--> turns the
> ![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--m:b_n--> formula into:

![with theta equal to omega_1 t, b_n equals 1 over pi times the integral from 0 to 2 pi of f of theta sin n theta d theta](edges-and-fourier.assets/eq-coeffs-angle.svg)

**Two symmetry shortcuts.** Most power-electronics waveforms have symmetries that halve the work.

- **Odd symmetry**, ![f(- theta ) = -f( theta )](edges-and-fourier.assets/eq-inline/a51d2fe445.svg)<!--m:f(-\theta) = -f(\theta)-->. The waveform is a mirror image through the origin, as
  a sine is. All the ![a_n](edges-and-fourier.assets/eq-inline/278ab95d3a.svg)<!--m:a_n--> (and ![a_0](edges-and-fourier.assets/eq-inline/4a5997da73.svg)<!--m:a_0-->) vanish and only sines remain, because ![f cos](edges-and-fourier.assets/eq-inline/624d9d5541.svg)<!--m:f\cos--> is then odd and
  integrates to zero over a symmetric interval.
- **Half-wave symmetry**, ![f( theta + pi ) = -f( theta )](edges-and-fourier.assets/eq-inline/4ed766ba65.svg)<!--m:f(\theta + \pi) = -f(\theta)-->. The second half-cycle is the first one
  flipped upside down. This is exactly what an H-bridge produces, because it applies ![+V_dc](edges-and-fourier.assets/eq-inline/0458144a16.svg)<!--m:+V_{dc}--> then
  ![-V_dc](edges-and-fourier.assets/eq-inline/b2ceae7532.svg)<!--m:-V_{dc}--> by symmetric switching. Substitute ![theta = s + pi](edges-and-fourier.assets/eq-inline/81df2d694b.svg)<!--m:\theta = s + \pi--> in the second half of the integral,
  using ![sin (n(s+ pi )) = (-1)^n sin ns](edges-and-fourier.assets/eq-inline/095ed93610.svg)<!--m:\sin(n(s+\pi)) = (-1)^n \sin ns-->:

![with half-wave symmetry, the integral over the second half equals minus 1 to the n plus 1 times the integral over the first half](edges-and-fourier.assets/eq-half-wave.svg)

![b_n equals 1 plus minus 1 to the n plus 1 over pi times the first-half integral: 2 over pi times the integral for odd n, and 0 for even n](edges-and-fourier.assets/eq-half-wave-result.svg)

For even ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> the two halves cancel exactly. **A half-wave-symmetric waveform contains only odd
harmonics.** That is why an H-bridge's output has no 2nd, 4th or 6th harmonic, and why the
interesting numbers below are all 3, 5, 7 and so on.

## 6 The square wave, derived

Now the waveform from the video. An H-bridge flipping ![plus-minus V](edges-and-fourier.assets/eq-inline/4545201178.svg)<!--m:\pm V--> across a load, with one period
written in angle:

![f of theta equals plus V for theta between 0 and pi, and minus V for theta between pi and 2 pi](edges-and-fourier.assets/eq-square-def.svg)

It is odd, so every ![a_n = 0](edges-and-fourier.assets/eq-inline/872e3899b1.svg)<!--m:a_n = 0-->. Its average is zero, so ![a_0 = 0](edges-and-fourier.assets/eq-inline/f1977494dc.svg)<!--m:a_0 = 0-->. Only the ![b_n](edges-and-fourier.assets/eq-inline/54d608cbef.svg)<!--m:b_n--> remain. Split the
integral at ![theta = pi](edges-and-fourier.assets/eq-inline/42cf7bfb68.svg)<!--m:\theta = \pi-->, where the waveform changes sign:

![b_n equals 1 over pi times the integral from 0 to pi of V sin n theta minus the integral from pi to 2 pi of V sin n theta](edges-and-fourier.assets/eq-sq-step1.svg)

Each piece is an elementary integral, since the antiderivative of ![sin n theta](edges-and-fourier.assets/eq-inline/f045facc17.svg)<!--m:\sin n\theta--> is
![- cos n theta/n](edges-and-fourier.assets/eq-inline/2c58694146.svg)<!--m:-\cos n\theta / n-->. Use ![cos 2n pi = 1](edges-and-fourier.assets/eq-inline/6c3f1e37ea.svg)<!--m:\cos 2n\pi = 1-->:

![integral from 0 to pi of sin n theta equals 1 minus cos n pi over n; integral from pi to 2 pi equals cos n pi minus 1 over n](edges-and-fourier.assets/eq-sq-step2.svg)

Subtracting the second from the first gives twice the first. Then ![cos n pi](edges-and-fourier.assets/eq-inline/d0019ff0f1.svg)<!--m:\cos n\pi--> is ![+1](edges-and-fourier.assets/eq-inline/acb72b9476.svg)<!--m:+1--> for even ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n-->
and ![-1](edges-and-fourier.assets/eq-inline/7984b0a0e1.svg)<!--m:-1--> for odd ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n-->, which is written ![(-1)^n](edges-and-fourier.assets/eq-inline/f21498e059.svg)<!--m:(-1)^n-->:

![b_n equals 2 V over n pi times 1 minus minus 1 to the n, which is 4 V over n pi for odd n and 0 for even n](edges-and-fourier.assets/eq-sq-step3.svg)

Even harmonics vanish, as the half-wave symmetry promised. Odd harmonics have amplitude
![4V/(n pi )](edges-and-fourier.assets/eq-inline/bb802859e2.svg)<!--m:4V/(n\pi)-->, falling as ![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--m:1/n-->. Put the coefficients back into the series:

![v of t equals 4 V over pi times the sum over odd n of sin n omega_1 t over n, which is 4 V over pi times sin omega_1 t plus a third sin 3 omega_1 t plus a fifth sin 5 omega_1 t and so on](edges-and-fourier.assets/eq-sq-result.svg)

**Check it.** At a quarter period, ![t = T/4](edges-and-fourier.assets/eq-inline/63e6542200.svg)<!--m:t = T/4-->, the square wave is plainly ![+V](edges-and-fourier.assets/eq-inline/d84db4c8f2.svg)<!--m:+V-->. The series gives
![sin (n pi/2) = +1, -1, +1, -1,](edges-and-fourier.assets/eq-inline/c153189203.svg)<!--m:\sin(n\pi/2) = +1, -1, +1, -1, \dots--> for ![n = 1, 3, 5, 7,](edges-and-fourier.assets/eq-inline/c3167b1d69.svg)<!--m:n = 1, 3, 5, 7, \dots-->. The bracket becomes Leibniz's
famous series for ![pi/4](edges-and-fourier.assets/eq-inline/58f2155161.svg)<!--m:\pi/4-->, and the result is exactly ![V](edges-and-fourier.assets/eq-inline/c9ee5681d3.svg)<!--m:V-->:

![v at T over 4 equals 4 V over pi times 1 minus a third plus a fifth minus a seventh and so on, which is 4 V over pi times pi over 4, equals V](edges-and-fourier.assets/eq-leibniz.svg)

![The first three odd sine harmonics above, and partial Fourier sums with 1, 3 and 9 terms approaching a square wave below](edges-and-fourier.assets/fig-03.svg)

_Top: the building blocks, each odd harmonic at one-![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n-->-th the amplitude of the fundamental.
Bottom: adding them up. One term is a plain sine. Three terms already show flat-ish tops. Nine terms
give steep edges, but there is a stubborn overshoot at every jump._

Notice what the edges need. The fundamental alone has a gentle slope. Each higher harmonic
contributes a steeper and steeper wiggle, and it takes the high harmonics to make the jump sharp.
**A steep edge *is* high-frequency content.** That is the precise sense in which §3's 50 ns edge
"contains" 7 MHz. Section 8 shows that finite rise time gently rolls off the harmonics above about
![1/( pi t_r)](edges-and-fourier.assets/eq-inline/65aa0de6e5.svg)<!--m:1/(\pi t_r)-->.

## 7 Reading a spectrum — Gibbs, Parseval and THD

**The spectrum.** Plotting each harmonic's amplitude against ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> gives the waveform's **spectrum**.
It is the same information as the time plot, sorted by frequency instead of by time. For the square
wave it is a row of bars at odd ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> only, falling as ![4V/(n pi )](edges-and-fourier.assets/eq-inline/bb802859e2.svg)<!--m:4V/(n\pi)-->:

![Amplitude spectrum of a plus or minus V square wave with bars at odd harmonics falling as 4 V over n pi](edges-and-fourier.assets/fig-05.svg)

_The fundamental stands taller than the square wave itself, at 1.27 V. The harmonics fall slowly, as
![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--m:1/n-->, which is why square waves are "noisy". Their energy reaches far up the frequency axis._

The fundamental's amplitude is ![4/pi approx 1.273](edges-and-fourier.assets/eq-inline/5829bc02d8.svg)<!--m:4/\pi \approx 1.273--> times the square wave's height. That surprises
almost everyone the first time, so it is worth stating plainly: **the best-fitting sine to a ±V
square wave peaks at about 1.27 V, higher than the square wave ever goes.** The harmonics then pull
the tops down flat and push the shoulders up.

**Gibbs overshoot.** Adding more terms makes the partial sum converge to the square wave *everywhere
except right at the jumps*. Next to each jump the partial sum overshoots, and the overshoot does not
shrink as you add terms. It only gets narrower. In the limit it settles at about 9 % of the jump:

![the maximum of the partial sum tends to 2 over pi times Si of pi times V, about 1.179 V, an overshoot of about 9 percent of the 2 V jump](edges-and-fourier.assets/eq-gibbs.svg)

(![Si](edges-and-fourier.assets/eq-inline/3c5737d86c.svg)<!--m:\operatorname{Si}--> is the sine integral, ![Si(x) = integral_0^x sin u/u du](edges-and-fourier.assets/eq-inline/146f14e16b.svg)<!--m:\operatorname{Si}(x) = \int_0^x \sin u / u \, du-->.) Gibbs
overshoot is a real effect, not a maths curiosity. Any time a sharp edge passes through a system that
cuts off sharply above some frequency (a brick-wall filter, a band-limited amplifier), the edge
comes out with this ringing overshoot.

**Parseval — where the power goes.** Power in a resistor goes as voltage squared (![p = vi = v^2/R](edges-and-fourier.assets/eq-inline/1a045a7637.svg)<!--m:p = vi = v^2/R-->
by Ohm's law). Square the series,
average over a period, and orthogonality kills every cross term between different harmonics. What
survives is the sum of each harmonic's own mean square:

![the mean of f squared equals a_0 squared plus one half the sum of a_n squared plus b_n squared](edges-and-fourier.assets/eq-parseval.svg)

This is **Parseval's theorem**. In words: *the total power is the sum of the powers in each
harmonic*, and harmonics never interfere in their power contribution. For the square wave the mean
square is obviously ![V^2](edges-and-fourier.assets/eq-inline/13bbb9f936.svg)<!--m:V^2-->, because ![v^2 = V^2](edges-and-fourier.assets/eq-inline/1421d0c4d4.svg)<!--m:v^2 = V^2--> at every instant. Parseval agrees, which needs the sum
of ![1/n^2](edges-and-fourier.assets/eq-inline/dc31304943.svg)<!--m:1/n^2--> over odd ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n-->. That sum is ![pi^2/8](edges-and-fourier.assets/eq-inline/504a023cae.svg)<!--m:\pi^2/8-->: Euler's classic result is that ![1/n^2](edges-and-fourier.assets/eq-inline/dc31304943.svg)<!--m:1/n^2--> summed over
*all* ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> gives ![pi^2/6](edges-and-fourier.assets/eq-inline/b26d2e8713.svg)<!--m:\pi^2/6-->; the even terms, ![1/(2m)^2 = 14 times 1/m^2](edges-and-fourier.assets/eq-inline/a695d85ecf.svg)<!--m:1/(2m)^2 = \tfrac14 \cdot 1/m^2-->, add up to a quarter of
that; so the odd terms are the remaining three quarters, ![34 times pi^2/6 = pi^2/8](edges-and-fourier.assets/eq-inline/95336d2a4d.svg)<!--m:\tfrac34 \cdot \pi^2/6 = \pi^2/8-->:

![V squared equals one half the sum of 16 V squared over n squared pi squared, which is 8 V squared over pi squared times pi squared over 8, equals V squared](edges-and-fourier.assets/eq-parseval-square.svg)

The fundamental's share is therefore:

![P_1 over P_total equals one half of 4 V over pi squared, over V squared, equals 8 over pi squared, about 0.811](edges-and-fourier.assets/eq-fund-fraction.svg)

About 81 % of a square wave's power is in the fundamental and 19 % is in the harmonics.

**THD.** **Total harmonic distortion** measures how far a waveform is from a pure sine: the RMS of
all the harmonics together, divided by the RMS of the fundamental. With Parseval, the harmonics'
combined mean square is the total minus the fundamental's:

![THD equals the square root of V_2 squared plus V_3 squared and so on, over V_1, which equals the square root of V_rms squared minus V_1 rms squared, over V_1 rms](edges-and-fourier.assets/eq-thd-def.svg)

For the square wave, ![V_rms = V](edges-and-fourier.assets/eq-inline/e515b7b30c.svg)<!--m:V_{rms} = V--> and ![V_1,rms = (4V/pi )/sqrt 2](edges-and-fourier.assets/eq-inline/ad8fb6ecd1.svg)<!--m:V_{1,rms} = (4V/\pi)/\sqrt2-->, so:

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

**Layer 1 — the ideal bridge.** Diagonal pairs alternate at 50 Hz. Q1 and Q4 put ![+V_dc](edges-and-fourier.assets/eq-inline/0458144a16.svg)<!--m:+V_{dc}--> across the
lamp, then Q2 and Q3 put ![-V_dc](edges-and-fourier.assets/eq-inline/b2ceae7532.svg)<!--m:-V_{dc}-->. This is exactly the square wave of §6 with ![V = V_dc](edges-and-fourier.assets/eq-inline/ad4f903633.svg)<!--m:V = V_{dc}-->:

![v_AB of t equals 4 V_dc over pi times the sum over odd n of sin n omega_1 t over n, with omega_1 equal to 2 pi times 50 Hz](edges-and-fourier.assets/eq-hb-ideal.svg)

With the video's 12 V battery:

![V_1 equals 4 over pi V_dc, about 1.273 V_dc, which for 12 V is 15.28 V peak or 10.80 V rms](edges-and-fourier.assets/eq-hb-fund.svg)

**Layer 2 — the lamp as a resistance ![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--m:R-->.** A resistor obeys Ohm's law at every instant and at every
frequency, so the current is the same series divided by ![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--m:R-->. Each harmonic of voltage drives its own
harmonic of current:

![i of t equals v_AB over R, which is 4 V_dc over pi R times the sum over odd n of sin n omega_1 t over n](edges-and-fourier.assets/eq-hb-current.svg)

The power each harmonic delivers to the lamp is its peak voltage squared over ![2R](edges-and-fourier.assets/eq-inline/f51a431ecb.svg)<!--m:2R--> (the factor of 2
because a sine's mean square is half its peak squared, derived in
[ac-and-rms.md §3](ac-and-rms.md#3-rms--defined-by-equal-heating)):

![P_n equals V_n squared over 2 R, which is 8 V_dc squared over n squared pi squared R](edges-and-fourier.assets/eq-hb-power-n.svg)

![the sum of P_n equals 8 V_dc squared over pi squared R times pi squared over 8, which is V_dc squared over R](edges-and-fourier.assets/eq-hb-power-total.svg)

The total is ![V_dc^2/R](edges-and-fourier.assets/eq-inline/c5cd21d089.svg)<!--m:V_{dc}^2/R-->, exactly what a DC supply of ![V_dc](edges-and-fourier.assets/eq-inline/1091080009.svg)<!--m:V_{dc}--> would deliver. A lamp's filament
cannot tell a ±12 V square wave from 12 V DC, because its glow depends on heating and ![v^2](edges-and-fourier.assets/eq-inline/d96f95b7a2.svg)<!--m:v^2--> is
![144 V^2](edges-and-fourier.assets/eq-inline/a4f1daad66.svg)<!--m:144\,\mathrm{V}^2--> at every instant either way. Here are the numbers for a 12 V, 24 W lamp (hot
resistance ![R = 12^2/24 = 6 Omega](edges-and-fourier.assets/eq-inline/7c03b4e57b.svg)<!--m:R = 12^2/24 = 6\,\Omega-->):

![for R equals 6 ohms and V_dc equals 12 V: P_1 equals 19.45 W, P_3 equals 2.16 W, P_5 equals 0.78 W, P_7 equals 0.40 W, total 24 W](edges-and-fourier.assets/eq-hb-power-example.svg)

Of the 24 W, 19.45 W is carried by the 50 Hz fundamental. The other 4.55 W arrives at 150 Hz,
250 Hz, 350 Hz and up. The lamp doesn't care. A motor or a transformer does (§9).

**Layer 3 — MOSFET on-resistance.** A conducting MOSFET is not a perfect short. It behaves as a small
resistor ![R_ds(on)](edges-and-fourier.assets/eq-inline/6dace34914.svg)<!--m:R_{ds(on)}-->, typically a few milliohms to a few hundred. In each half-cycle the current
passes through **two** of them in series with the lamp (Q1 and Q4, or Q2 and Q3). That makes a
voltage divider:

![i equals V_dc over R plus 2 R_ds on, so v_R equals i R equals V_dc times R over R plus 2 R_ds on](edges-and-fourier.assets/eq-hb-divider.svg)

The divider is purely resistive, so it scales every harmonic by the same factor. The shape and THD
are unchanged, and only the size shrinks:

![v_R of t equals 4 V_dc over pi times R over R plus 2 R_ds on times the sum over odd n of sin n omega_1 t over n](edges-and-fourier.assets/eq-hb-rds-series.svg)

With ![R_ds(on) = 50 m Omega](edges-and-fourier.assets/eq-inline/db3d76cf57.svg)<!--m:R_{ds(on)} = 50\,\mathrm{m}\Omega--> and the 6 Ω lamp:

![the divider ratio is 6 over 6.1, which is 0.984; lamp power 23.2 W; conduction loss in the two switches 0.39 W](edges-and-fourier.assets/eq-hb-rds-example.svg)

So 98.4 % of the battery's power reaches the lamp, and the switches warm up by 0.39 W between them.

> **Watch out —** The lamp's resistance is not constant. A tungsten filament's cold resistance is
> roughly a tenth of its hot value, so at switch-on the current is around ten times normal for the
> first tens of milliseconds. The Fourier picture assumes steady state, with the filament hot and its
> ![R](edges-and-fourier.assets/eq-inline/06576556d1.svg)<!--m:R--> effectively constant over a 20 ms cycle (its thermal time constant is much longer). Size the
> MOSFETs for the cold inrush, not the steady-state 2 A.

**Layer 4 — zero intervals (dead time and the quasi-square wave).** Real bridges do not go straight
from ![+V_dc](edges-and-fourier.assets/eq-inline/0458144a16.svg)<!--m:+V_{dc}--> to ![-V_dc](edges-and-fourier.assets/eq-inline/b2ceae7532.svg)<!--m:-V_{dc}-->. Between turning one diagonal pair off and the other on, the controller
inserts a **dead time**, an interval with all four switches off. Without it, Q1 and Q3 (or Q2 and Q4), the two switches in one leg,
could briefly conduct together and short the supply. This is shoot-through, covered in
[../../dc-ac-inverters/h-bridge/](../../dc-ac-inverters/h-bridge/). With a resistive lamp, all
switches off means no current, so the lamp voltage is **zero** during that interval. Some inverters
also add zero intervals *deliberately*, by turning on both low-side switches, to shape the output.
Either way the waveform becomes a **quasi-square wave**: zero for an angle ![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--m:\alpha--> on each side of
every zero crossing.

![v_AB of theta is 0 for theta between 0 and alpha, V_dc between alpha and pi minus alpha, 0 between pi minus alpha and pi, with half-wave symmetry](edges-and-fourier.assets/eq-qs-def.svg)

It is still odd and half-wave symmetric, so only odd sines survive. Use the half-wave formula from
§5. The integrand is non-zero only between ![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--m:\alpha--> and ![pi - alpha](edges-and-fourier.assets/eq-inline/13f23193bf.svg)<!--m:\pi - \alpha-->:

![b_n equals 2 over pi times the integral from alpha to pi minus alpha of V_dc sin n theta, which is 2 V_dc over n pi times cos n alpha minus cos of n pi minus n alpha](edges-and-fourier.assets/eq-qs-step.svg)

Expand the second cosine with the angle-difference formula. For odd ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n-->, ![cos n pi = -1](edges-and-fourier.assets/eq-inline/22e9f135cb.svg)<!--m:\cos n\pi = -1--> and
![sin n pi = 0](edges-and-fourier.assets/eq-inline/d9f9bc06ad.svg)<!--m:\sin n\pi = 0-->:

![cos of n pi minus n alpha equals cos n pi cos n alpha plus sin n pi sin n alpha, which is minus cos n alpha for odd n](edges-and-fourier.assets/eq-qs-step2.svg)

![b_n equals 4 V_dc over n pi times cos n alpha for odd n, and 0 for even n](edges-and-fourier.assets/eq-qs-result.svg)

The square-wave amplitudes are simply multiplied by ![cos (n alpha )](edges-and-fourier.assets/eq-inline/770e27da25.svg)<!--m:\cos(n\alpha)-->. Setting ![alpha = 0](edges-and-fourier.assets/eq-inline/08b777d1d0.svg)<!--m:\alpha = 0--> recovers
the square wave, as it must. The new factor lets you **choose a harmonic to delete**:

![b_n equals 0 exactly when cos n alpha equals 0, which is when n alpha equals 90 degrees, so alpha equals 90 degrees over n: alpha 30 degrees removes the 3rd, alpha 18 degrees removes the 5th](edges-and-fourier.assets/eq-qs-kill.svg)

At ![alpha = 30^ deg](edges-and-fourier.assets/eq-inline/63580e7f38.svg)<!--m:\alpha = 30^\circ--> the 3rd harmonic disappears, and with it every harmonic divisible by 3. The
reason is that ![cos (3(2k+1) times 30^ deg ) = cos ((2k+1) times 90^ deg ) = 0](edges-and-fourier.assets/eq-inline/b10471a0bf.svg)<!--m:\cos(3(2k+1) \cdot 30^\circ) = \cos((2k+1) \cdot 90^\circ) = 0-->:

![at alpha equals 30 degrees: b_1 equals 4 V_dc over pi cos 30 degrees equals 1.103 V_dc; b_3, b_9, b_15 are 0; b_5 and b_7 are minus 4 V_dc over pi times 0.866 over n](edges-and-fourier.assets/eq-qs-30.svg)

![A quasi-square wave with zero intervals of width alpha and its fundamental above, and the normalised harmonic amplitudes cos n alpha over n versus alpha below](edges-and-fourier.assets/fig-07.svg)

_Top: the quasi-square wave at 30 degrees and its fundamental. Bottom: each harmonic's amplitude as
the zero interval widens. Wherever a curve crosses zero, that harmonic is gone from the output. The
price is a smaller fundamental, which falls as ![cos alpha](edges-and-fourier.assets/eq-inline/9f1f363b16.svg)<!--m:\cos\alpha-->._

The RMS of a quasi-square wave follows from its definition (the waveform is ![plus-minus V_dc](edges-and-fourier.assets/eq-inline/38ec47d61b.svg)<!--m:\pm V_{dc}--> for a
fraction ![( pi - 2 alpha )/pi](edges-and-fourier.assets/eq-inline/14c93c4ed9.svg)<!--m:(\pi - 2\alpha)/\pi--> of the time and zero otherwise):

![V_rms equals the square root of 1 over pi times the integral from alpha to pi minus alpha of V_dc squared, which is V_dc times the square root of pi minus 2 alpha over pi](edges-and-fourier.assets/eq-qs-rms.svg)

and so its THD at ![alpha = 30^ deg](edges-and-fourier.assets/eq-inline/63580e7f38.svg)<!--m:\alpha = 30^\circ--> is:

![THD at 30 degrees equals the square root of two thirds minus 1.103 over root 2 squared, over 1.103 over root 2, which is about 31.1 percent](edges-and-fourier.assets/eq-qs-thd-30.svg)

Removing the triplen harmonics cuts THD from 48.3 % to 31.1 %. This is the simplest example of
**selective harmonic elimination**, and the seed of the idea behind SPWM: place the switching
instants to put zeros in the spectrum where you want them.

**The "modified sine wave" inverter.** Cheap inverters sold as "modified sine" are quasi-square
waves. They choose ![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--m:\alpha--> so that the peak *and* the RMS both match mains, 325 V peak and 230 V RMS:

![V_rms over V_pk equals the square root of pi minus 2 alpha over pi, equals 230 over 325, which is 1 over root 2, so alpha equals 45 degrees](edges-and-fourier.assets/eq-modified-sine.svg)

At ![alpha = 45^ deg](edges-and-fourier.assets/eq-inline/d888911a73.svg)<!--m:\alpha = 45^\circ--> the 3rd harmonic is *not* removed (![cos 135^ deg = -0.707](edges-and-fourier.assets/eq-inline/7c81fd1f8a.svg)<!--m:\cos 135^\circ = -0.707-->). Working it
through, the THD comes out at 48.3 %, the same as a plain square wave. The modified sine wave fixes
the peak and the RMS. It does not fix the distortion.

**How big is protective dead time, in angle?** Dead time ![t_d](edges-and-fourier.assets/eq-inline/6c703960eb.svg)<!--m:t_d--> makes a zero interval of width ![t_d](edges-and-fourier.assets/eq-inline/6c703960eb.svg)<!--m:t_d-->
at each transition. In the notation above that interval is ![2 alpha](edges-and-fourier.assets/eq-inline/008b07ecdf.svg)<!--m:2\alpha--> wide:

![2 alpha equals 360 degrees times t_d over T, so alpha equals 180 degrees times t_d over T: for t_d equals 500 ns, alpha is 0.0045 degrees at 50 Hz and 4.5 degrees at 50 kHz](edges-and-fourier.assets/eq-dead-alpha.svg)

At 50 Hz, protective dead time is spectrally invisible. At the 50 kHz switching of the inverter's
first stage it is a 4.5° zero interval. That trims the fundamental by ![1 - cos 4.5^ deg approx 0.3 %](edges-and-fourier.assets/eq-inline/eaab2731ca.svg)<!--m:1 - \cos 4.5^\circ \approx 0.3\,\%-->
and starts reshaping the harmonics, so it matters there.

**Layer 5 — finite edges.** The last idealisation is the vertical edge. A trapezoidal wave with rise
time ![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--m:t_r--> is a square wave smoothed by a moving average of width ![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--m:t_r-->. A moving average multiplies
each harmonic by a ![sin x/x](edges-and-fourier.assets/eq-inline/6dcfc84d85.svg)<!--m:\sin x / x--> factor. To see it, average harmonic ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> over a window of width ![t_r](edges-and-fourier.assets/eq-inline/a6684eb7a2.svg)<!--m:t_r-->
centred on ![t](edges-and-fourier.assets/eq-inline/8efd86fb78.svg)<!--m:t--> (the antiderivative of ![sin](edges-and-fourier.assets/eq-inline/f5356d1e92.svg)<!--m:\sin--> is ![- cos](edges-and-fourier.assets/eq-inline/6a630a5d56.svg)<!--m:-\cos-->, and the difference of the two cosines is
turned into a product by the identity ![cos (A-B) - cos (A+B) = 2 sin A sin B](edges-and-fourier.assets/eq-inline/2e2ceb5931.svg)<!--m:\cos(A-B) - \cos(A+B) = 2\sin A\sin B--> of §5):

![one over t_r times the integral from t minus t_r over 2 to t plus t_r over 2 of sin n omega_1 s ds equals sin n omega_1 t times sin x over x, with x equal to n omega_1 t_r over 2, which is n pi t_r over T](edges-and-fourier.assets/eq-moving-average.svg)

The harmonic comes out unchanged in shape, only scaled by ![sin x/x](edges-and-fourier.assets/eq-inline/8b360c5537.svg)<!--m:\sin x/x-->. While ![x < 1](edges-and-fourier.assets/eq-inline/d2241f4b17.svg)<!--m:x < 1--> that factor is
close to 1; beyond it the factor falls as ![1/x](edges-and-fourier.assets/eq-inline/df95313afc.svg)<!--m:1/x-->. Below the corner ![f_c = 1/( pi t_r)](edges-and-fourier.assets/eq-inline/913d3b6838.svg)<!--m:f_c = 1/(\pi t_r)--> the factor is about 1.
Above it, the harmonics fall as ![1/n^2](edges-and-fourier.assets/eq-inline/dc31304943.svg)<!--m:1/n^2--> instead of ![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--m:1/n-->:

![b_n trapezoid is approximately 4 V over n pi times sin of n pi t_r over T, over n pi t_r over T, with a corner at f_c equal to 1 over pi t_r](edges-and-fourier.assets/eq-trapezoid.svg)

For a 50 ns edge, ![f_c approx 6.4 MHz](edges-and-fourier.assets/eq-inline/2ee34ef0f4.svg)<!--m:f_c \approx 6.4\,\mathrm{MHz}-->, which is roughly the 127 000th harmonic of 50 Hz.
It is irrelevant to the lamp and decisive for radio-frequency interference.

**Everything together.** Stacking all five layers gives the H-bridge-into-a-lamp output, with every
variable visible:

![v_lamp of t equals the sum over odd n of: 4 V_dc over n pi (square wave), times R over R plus 2 R_ds on (MOSFET divider), times cos n alpha (zero intervals), times sin of n pi t_r over T over n pi t_r over T (finite edges), times sin n omega_1 t](edges-and-fourier.assets/eq-full.svg)

Each factor is something you can change. Raise ![V_dc](edges-and-fourier.assets/eq-inline/1091080009.svg)<!--m:V_{dc}--> and everything scales. Choose better
MOSFETs and the divider factor approaches 1. Choose ![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--m:\alpha--> to place zeros. Slow the edges to tame
the radio-frequency tail, at the cost of switching loss.

## 9 Why the harmonics matter downstream

A lamp is the one load that genuinely doesn't care: heat is heat. Almost everything else in the
inverter chain responds to frequency, so the harmonics of §8 become real costs.

- **Transformers.** Each harmonic adds its own core loss (eddy-current loss per unit of flux rises
  steeply with frequency) and audible hum, so a 50 Hz transformer fed a square wave runs warmer and
  noisier than on a sine of the same RMS. The fast edges also push
  ![C dv/dt](edges-and-fourier.assets/eq-inline/b96b608a56.svg)<!--m:C\,dv/dt--> current through the interwinding capacitance (§3), straight into the secondary. And a
  transformer cannot pass the DC offset an asymmetric bridge might produce. See
  [../transformer/](../transformer/) and [../electromagnetism/](../electromagnetism/).
- **Inductive loads (motors, fans).** An inductor's impedance is ![omega L](edges-and-fourier.assets/eq-inline/b3beb438d7.svg)<!--m:\omega L-->: drive a sine
  current of amplitude ![I](edges-and-fourier.assets/eq-inline/48786abc01.svg)<!--m:\hat I--> through it and ![v = L di/dt](edges-and-fourier.assets/eq-inline/169359fd71.svg)<!--m:v = L\,di/dt--> has amplitude ![omega L I](edges-and-fourier.assets/eq-inline/90fb26c529.svg)<!--m:\omega L\hat I-->, so the
  ratio of voltage amplitude to current amplitude (its AC "resistance", the *impedance* magnitude)
  is ![omega L](edges-and-fourier.assets/eq-inline/b3beb438d7.svg)<!--m:\omega L-->; by the mirror argument a capacitor's is ![1/( omega C)](edges-and-fourier.assets/eq-inline/a157b00b7e.svg)<!--m:1/(\omega C)-->. Harmonic ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> has ![1/n](edges-and-fourier.assets/eq-inline/5f556983ad.svg)<!--m:1/n--> of
  the fundamental's voltage and meets ![n](edges-and-fourier.assets/eq-inline/d1854cae89.svg)<!--m:n--> times the impedance, so it draws ![1/n^2](edges-and-fourier.assets/eq-inline/dc31304943.svg)<!--m:1/n^2--> of the
  fundamental's current. The harmonic currents are small, but they make torque ripple, buzzing and
  extra copper heating.
- **Filters.** To turn a square wave into a sine you must remove the 3rd harmonic at 150 Hz while
  keeping the fundamental at 50 Hz. Those are only a factor of 3 apart. An LC low-pass filter
  attenuates 40 dB per decade above its corner (a decade is a factor of 10 in frequency, and 40 dB
  is a factor of 100 in amplitude, §3; see [../../filters/lc-filter/](../../filters/lc-filter/)),
  so separating 50 Hz from 150 Hz needs a corner squeezed between them and enormous L and C.

That last point is the motivation for everything after the H-bridge in this tree. **Pulse-width
modulation** ([../../pwm/](../../pwm/)) and its sinusoidal form **SPWM**
([../../dc-ac-inverters/spwm/](../../dc-ac-inverters/spwm/)) switch the bridge at tens of kilohertz
and vary the pulse widths so that the low-order harmonics nearly vanish. The unwanted energy is
pushed up to clusters around the switching frequency, far from 50 Hz, where a small LC filter removes
it easily. The Fourier series is the tool that both predicts this and proves it works.

## 10 What this costs you

- **Fast edges trade switching loss for noise.** Faster edges waste less energy in the MOSFET but
  inject more ![C dv/dt](edges-and-fourier.assets/eq-inline/b96b608a56.svg)<!--m:C\,dv/dt--> current and ![L di/dt](edges-and-fourier.assets/eq-inline/f24cc20a0b.svg)<!--m:L\,di/dt--> ringing (§3). Every real design picks an edge speed,
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
  through the body diodes (the diode built into every MOSFET, from source to drain — see
  [../../dc-ac-inverters/h-bridge/h-bridge.md §5](../../dc-ac-inverters/h-bridge/h-bridge.md#5-the-mosfet-as-a-switch))
  during dead time, and the voltage is ![plus-minus V_dc](edges-and-fourier.assets/eq-inline/38ec47d61b.svg)<!--m:\pm V_{dc}--> (depending on current
  direction), not zero. The clean ![cos (n alpha )](edges-and-fourier.assets/eq-inline/770e27da25.svg)<!--m:\cos(n\alpha)--> result then no longer holds exactly.
- **Harmonic elimination costs fundamental.** Choosing ![alpha](edges-and-fourier.assets/eq-inline/f7c665b459.svg)<!--m:\alpha--> to kill a harmonic also shrinks
  the fundamental by ![cos alpha](edges-and-fourier.assets/eq-inline/9f1f363b16.svg)<!--m:\cos\alpha-->. At ![alpha = 30^ deg](edges-and-fourier.assets/eq-inline/63580e7f38.svg)<!--m:\alpha = 30^\circ--> you lose 13.4 % of the useful output to
  remove the 3rd.

## 11 Sources and cross-links

- **The two laws behind §2–3:** [../inductor/inductor.md](../inductor/inductor.md)
  (![v = L di/dt](edges-and-fourier.assets/eq-inline/169359fd71.svg)<!--m:v = L\,di/dt-->, the inductive kick) and [../capacitor/capacitor.md](../capacitor/capacitor.md)
  (![i = C dv/dt](edges-and-fourier.assets/eq-inline/7f5f5ec54b.svg)<!--m:i = C\,dv/dt-->).
- **The companion document:** [ac-and-rms.md](ac-and-rms.md). It explains sinusoids, averages, why
  RMS uses ![sqrt 2](edges-and-fourier.assets/eq-inline/6d0fdf0909.svg)<!--m:\sqrt2-->, and why 230 V mains peaks at 325 V.
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
  Design*, for the ![0.35/t_r](edges-and-fourier.assets/eq-inline/74e3f4690c.svg)<!--m:0.35/t_r--> bandwidth rule.
- Source video: *DC_to_AC_1.mp4* (12 V battery, H-bridge S1 to S4, lamp, ±12 V square wave).
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
