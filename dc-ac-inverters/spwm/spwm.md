# Sinusoidal PWM — how a comparator and a triangle wave draw a sine

An inverter's output stage is an [H-bridge](../h-bridge/): four switches that can connect the load
to the DC bus one way round (<!--m:+V_{dc}-->![+V_dc](spwm.assets/eq-inline/0458144a16.svg)<!--/m-->), the other way round (<!--m:-V_{dc}-->![-V_dc](spwm.assets/eq-inline/b2ceae7532.svg)<!--/m-->), or not at all. It cannot produce
230 V RMS of smooth sine directly; it can only switch. Sinusoidal PWM (SPWM) is the rule that decides
*when* to switch so that, once an LC filter has averaged away the switching, what is left is the
sine you asked for. This document derives that rule from scratch, explains the step in the video
that looks like magic (screenshots 22–23 of the notes, at 12:55–13:24: "as we raise the carrier frequency the average somehow
becomes the sine"), and puts real numbers on the 325 V inverter of the DC-to-AC project.

**Contents**

1. [Where this sits in the inverter](#1-where-this-sits-in-the-inverter)
2. [From constant D to a time-varying D](#2-from-constant-d-to-a-time-varying-d)
3. [The two signals and the comparator](#3-the-two-signals-and-the-comparator)
4. [The core result — the average follows the reference](#4-the-core-result--the-average-follows-the-reference)
5. [Why a faster carrier gets closer to the sine](#5-why-a-faster-carrier-gets-closer-to-the-sine)
6. [Filtering — what actually does the averaging](#6-filtering--what-actually-does-the-averaging)
7. [The modulation index — linear region and overmodulation](#7-the-modulation-index--linear-region-and-overmodulation)
8. [The frequency ratio and the harmonic spectrum](#8-the-frequency-ratio-and-the-harmonic-spectrum)
9. [Bipolar vs. unipolar switching on an H-bridge](#9-bipolar-vs-unipolar-switching-on-an-h-bridge)
10. [Natural vs. regular sampling — SPWM in a microcontroller](#10-natural-vs-regular-sampling--spwm-in-a-microcontroller)
11. [The analogue implementation from the video — AD9833 and LM311](#11-the-analogue-implementation-from-the-video--ad9833-and-lm311)
12. [Practical numbers for a 230 V inverter](#12-practical-numbers-for-a-230-v-inverter)
13. [What this costs you](#13-what-this-costs-you)
14. [Sources and cross-links](#14-sources-and-cross-links)

> **The thesis in one line**
>
> Within one carrier period a straight-sided triangle acts as a ruler: the comparator's on-time is
> proportional to the reference's value, so the switching-cycle average of the <!--m:\pm V_{dc}-->![plus-minus V_dc](spwm.assets/eq-inline/38ec47d61b.svg)<!--/m--> output is
> <!--m:m_a V_{dc}\sin\theta-->![m_a V_dc sin theta](spwm.assets/eq-inline/ea8a7d58a4.svg)<!--/m--> — a sampled copy of the sine, one sample per carrier period. More carrier
> periods per sine period means more samples, and a filter turns the samples back into the sine.

---

## 1 Where this sits in the inverter

The DC-to-AC project steps 12 V up to a 325 V DC bus (first H-bridge at 50 kHz, a transformer, a
[rectifier](../../rectifiers/)), then a *second* H-bridge turns that bus into AC, and an
[LC filter](../../filters/lc-filter/) smooths it into 230 V at 50 Hz. SPWM is the control signal for
that second bridge.

![SPWM signal chain: two AD9833 waveform generators feed an LM311 comparator, a gate driver with dead time drives an H-bridge on the DC bus, and an LC filter delivers the sine to the load](spwm.assets/fig-01.svg)

_The control side is three small chips: a sine generator, a triangle generator and a comparator. The
comparator's one-bit decision, through a gate driver that inserts dead time, chooses which diagonal
pair of the bridge conducts. Everything after the bridge is passive averaging._

## 2 From constant D to a time-varying D

Start from what the [PWM document](../../pwm/) proves. A switch that spends a fraction <!--m:D-->![D](spwm.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> of each
period at one level and the rest at another has a switching-cycle average set by <!--m:D-->![D](spwm.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> alone. For an
H-bridge that alternates between <!--m:+V_{dc}-->![+V_dc](spwm.assets/eq-inline/0458144a16.svg)<!--/m--> and <!--m:-V_{dc}-->![-V_dc](spwm.assets/eq-inline/b2ceae7532.svg)<!--/m--> the average is <!--m:(2D-1)V_{dc}-->![(2D-1)V_dc](spwm.assets/eq-inline/bf4a8b8ee9.svg)<!--/m-->:
<!--m:D = 0.5-->![D = 0.5](spwm.assets/eq-inline/a2406f7d12.svg)<!--/m--> gives 0 V, <!--m:D = 1-->![D = 1](spwm.assets/eq-inline/992a13a31d.svg)<!--/m--> gives <!--m:+V_{dc}-->![+V_dc](spwm.assets/eq-inline/0458144a16.svg)<!--/m-->, <!--m:D = 0-->![D = 0](spwm.assets/eq-inline/1526d346fb.svg)<!--/m--> gives <!--m:-V_{dc}-->![-V_dc](spwm.assets/eq-inline/b2ceae7532.svg)<!--/m-->.

A [buck converter](../../dc-dc-converters/buck/) holds <!--m:D-->![D](spwm.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> fixed and gets a fixed output. Nothing
stops us changing <!--m:D-->![D](spwm.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> from one switching period to the next. If we choose, for the *k*th period,
whatever duty makes the average equal to the sine's value at that moment, the sequence of averages
traces a sine. Video screenshots 14 and 15 (10:25–10:32) show exactly this on a buck: a fixed pulse pattern
gives a filtered triangle-ish output, a varying pulse pattern gives a filtered sine. The question
SPWM answers is: **how do we compute the right <!--m:D-->![D](spwm.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> for every period, precisely and cheaply?** The
answer is that a comparator does it for free.

## 3 The two signals and the comparator

Two low-voltage waveforms are generated (video screenshot 16, 11:05):

**The reference** — the *shape* you want at the output, at exactly the output frequency:

![v_ref of t equals m_a sine 2 pi f_1 t, equals m_a sine theta, with f_1 equal to 50 hertz](spwm.assets/eq-ref.svg)

**The carrier** — a symmetric triangle of amplitude 1 and a much higher frequency <!--m:f_c-->![f_c](spwm.assets/eq-inline/b1071317de.svg)<!--/m-->. It sets the
switching frequency; its shape is what makes the method work. Over one carrier period
<!--m:T_c = 1/f_c-->![T_c = 1/f_c](spwm.assets/eq-inline/f508d10360.svg)<!--/m--> it is two straight lines:

![v_c of t equals minus 1 plus 4 t over T_c for t from 0 to T_c over 2, and 3 minus 4 t over T_c for t from T_c over 2 to T_c; T_c equals 1 over f_c, repeating](spwm.assets/eq-carrier.svg)

Two dimensionless numbers describe the pair:

![m_a equals V_ref peak over V_c peak, the amplitude modulation index; m_f equals f_c over f_1, the frequency modulation ratio](spwm.assets/eq-ratios.svg)

Both go into a **comparator** (video screenshots 18–20, 11:44–12:05), which answers one question only — *which input
is higher?* — and the answer drives the bridge:

![v_AB equals plus V_dc when v_ref is greater than v_c, with Q_1 and Q_4 on; minus V_dc when v_ref is less than v_c, with Q_2 and Q_3 on](spwm.assets/eq-rule.svg)

![Natural sampling with a 50 hertz reference and 100 hertz carrier: the crossings define the comparator output edges](spwm.assets/fig-02.svg)

_The video's own example (screenshots 21–22, 12:42–12:55): a 50 Hz reference against a 100 Hz carrier, so
<!--m:m_f = 2-->![m_f = 2](spwm.assets/eq-inline/c0ed7f581d.svg)<!--/m-->. Every edge of the output lies exactly under a crossing. With only two carrier periods per
sine period the output is a crude, lopsided square wave — nothing like a sine yet._

> **Note —** The comparator does not "know" anything about sines. It produces a stream of
> full-height pulses. All of the sine is hidden in *where* the edges fall, and the next section is
> about reading it back out.

## 4 The core result — the average follows the reference

This is the step to go through slowly, because everything else rests on it.

**Assumption.** The carrier is much faster than the reference, so during one carrier period the
reference barely moves. Call its value during period *k* simply <!--m:r-->![r](spwm.assets/eq-inline/4dc7c9ec43.svg)<!--/m-->, a number between −1 and +1.
(§5 is about what happens when this assumption fails.)

**The intuition: the triangle is a ruler.** In one period the triangle sweeps the whole range from
−1 up to +1 and back down again, at a *constant speed*. A constant speed means it spends equal time
at every level. So the fraction of the period it spends *below* the level <!--m:r-->![r](spwm.assets/eq-inline/4dc7c9ec43.svg)<!--/m--> is just the fraction
of the range that lies below <!--m:r-->![r](spwm.assets/eq-inline/4dc7c9ec43.svg)<!--/m-->:

![the time the triangle spends below r, divided by T_c, equals r minus minus 1 over 1 minus minus 1, equals 1 plus r over 2](spwm.assets/eq-ruler.svg)

And "triangle below the reference" is exactly when the comparator outputs <!--m:+V_{dc}-->![+V_dc](spwm.assets/eq-inline/0458144a16.svg)<!--/m-->. That is the
whole idea. Now the same thing done with the equations, so nothing is taken on trust.

**The algebra.** Put the carrier period between two valleys. On the rising edge the triangle meets
the reference at <!--m:t_1-->![t_1](spwm.assets/eq-inline/a90caf2e2b.svg)<!--/m-->; by symmetry it meets it again on the falling edge at <!--m:t_2-->![t_2](spwm.assets/eq-inline/8077eb7241.svg)<!--/m-->:

![v_c of t_1 equals minus 1 plus 4 t_1 over T_c equals r, so t_1 equals 1 plus r times T_c over 4, and t_2 equals T_c minus t_1](spwm.assets/eq-crossing.svg)

The output is high from the valley up to <!--m:t_1-->![t_1](spwm.assets/eq-inline/a90caf2e2b.svg)<!--/m-->, and again from <!--m:t_2-->![t_2](spwm.assets/eq-inline/8077eb7241.svg)<!--/m--> to the next valley:

![t_on equals t_1 plus T_c minus t_2, equals 2 t_1, equals 1 plus r times T_c over 2, so D equals t_on over T_c equals 1 plus r over 2](spwm.assets/eq-ton.svg)

![One carrier period with the reference held at level r: the crossing times are linear in r, giving duty cycle one plus r over two and an average of r times V_dc](spwm.assets/fig-03.svg)

_The shaded triangle is similar to the whole rising edge: its height is <!--m:1 + r-->![1 + r](spwm.assets/eq-inline/7fa3cceeef.svg)<!--/m-->, out of a
total rise of 2, so its width <!--m:t_1-->![t_1](spwm.assets/eq-inline/a90caf2e2b.svg)<!--/m--> is the same fraction of the half-period. Because the edge is a
straight line, the crossing time is a linear function of the reference._

Since <!--m:r-->![r](spwm.assets/eq-inline/4dc7c9ec43.svg)<!--/m--> is the reference's value during period *k*, write it as <!--m:r = m_a \sin\theta_k-->![r = m_a sin theta_k](spwm.assets/eq-inline/e625757aef.svg)<!--/m-->. The
duty cycle that the comparator produces, with no computation at all, is:

![D_k equals 1 plus m_a sine theta_k over 2, where theta_k is 2 pi f_1 t_k, the phase during carrier period k](spwm.assets/eq-dk.svg)

Now average the output over that period with the bipolar formula from the PWM document:

![v bar_k equals D_k times plus V_dc plus one minus D_k times minus V_dc, equals 2 D_k minus 1 times V_dc, equals r V_dc](spwm.assets/eq-avg.svg)

The ½ and the 1 cancel exactly — that is why the triangle is centred on zero with amplitude 1 —
leaving:

![v bar_k equals m_a V_dc sine theta_k, boxed](spwm.assets/eq-core.svg)

**The switching-cycle average of the output is the reference, scaled by** <!--m:V_{dc}-->![V_dc](spwm.assets/eq-inline/1091080009.svg)<!--/m-->. Each carrier
period delivers one *sample* of the sine, encoded as a pulse width. This is the core idea behind
every SPWM inverter, and it holds for any reference shape, not just sines: the comparator turns
whatever voltage you feed it into a proportional average. Its fundamental amplitude follows
directly:

![V_1 peak equals m_a V_dc, and V_1 rms equals m_a V_dc over root 2, for m_a between 0 and 1](spwm.assets/eq-fundamental.svg)

## 5 Why a faster carrier gets closer to the sine

The derivation assumed the reference is constant over a carrier period. How wrong is that? In one
carrier period the sine's phase advances by <!--m:2\pi/m_f-->![2 pi/m_f](spwm.assets/eq-inline/16424cf45c.svg)<!--/m-->, so its value can change by up to:

![delta theta equals 2 pi over m_f, so the change in r is at most m_a times 2 pi over m_f: 2.5 at m_f 2, 0.24 at m_f 21, 0.013 at m_f 400, for m_a 0.8](spwm.assets/eq-drift.svg)

At <!--m:m_f = 2-->![m_f = 2](spwm.assets/eq-inline/c0ed7f581d.svg)<!--/m--> the reference swings by more than its whole range within a single carrier period —
there is no meaningful "value of the reference during this period", and the output is a poor copy.
At <!--m:m_f = 400-->![m_f = 400](spwm.assets/eq-inline/b682ed8934.svg)<!--/m--> (a 20 kHz carrier for a 50 Hz output) it changes by about 1 % of full scale, and each
pulse width is an excellent sample.

This is the answer to the question in video screenshots 22 and 23. The **carrier frequency is
the sampling rate**. One sine period contains <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> carrier periods, so the bridge gets <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m-->
independent chances per cycle to set its average. Few samples give a coarse staircase; many samples
give a fine one that hugs the sine.

![Bipolar SPWM pulses and their switching-cycle averages for frequency ratios 2, 6, 21 and 100: the average staircase converges onto the sine reference](spwm.assets/fig-04.svg)

_Computed for <!--m:m_a = 0.8-->![m_a = 0.8](spwm.assets/eq-inline/2dd2ee5741.svg)<!--/m-->: the pulses (amber), the exact average over each carrier period (green
staircase) and the reference (blue). The green staircase is precisely what the video labels
"average". The RMS error between staircase and sine falls from 58 % at <!--m:m_f = 2-->![m_f = 2](spwm.assets/eq-inline/c0ed7f581d.svg)<!--/m--> to 8.6 % at 21
and 1.8 % at 100._

The video used 100, 200, 400 and 800 Hz carriers (<!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> = 2, 4, 8, 16); Figure 74 continues the
same sequence further. Notice two things in the top row. First, the staircase is lopsided and does
not even average the sine correctly over each window: with the reference moving that fast, the
"triangle as ruler" argument breaks down. Second, by <!--m:m_f = 21-->![m_f = 21](spwm.assets/eq-inline/58e407653b.svg)<!--/m--> every step already sits on the
sine to within a few per cent. The error shrinks roughly in proportion to <!--m:1/m_f-->![1/m_f](spwm.assets/eq-inline/42a94e65a0.svg)<!--/m-->.

> **Tip —** The staircase is not something any circuit produces; it is the *ideal* local average.
> The physical averaging is done by the LC filter, which is never a perfect window-average — §6 shows
> what a real filter does with the same pulse trains.

## 6 Filtering — what actually does the averaging

The load must not see the pulses: the [LC filter](../../filters/lc-filter/) between the bridge and
the load is a low-pass that passes 50 Hz and blocks the switching frequency. Its job is easy or
hard depending on one ratio: how far above its corner <!--m:f_0-->![f_0](spwm.assets/eq-inline/bdd0794289.svg)<!--/m--> the switching harmonics sit.

![The same LC low-pass filter applied to bipolar SPWM with frequency ratios 6, 21 and 100: the output becomes a clean sine only when the carrier is far above the filter corner](spwm.assets/fig-05.svg)

_One filter (corner 250 Hz, Q = 0.71) simulated against three carriers. At <!--m:m_f = 6-->![m_f = 6](spwm.assets/eq-inline/e84128b427.svg)<!--/m--> the carrier sits
almost on the corner and leaks straight through (65 % distortion). At 21 it is attenuated about
18-fold (6.3 %). At 100 the output is a clean sine (0.3 %). The fixed 16° lag is the filter's phase
shift at 50 Hz, the same in every row._

A second-order filter attenuates as <!--m:(f_0/f)^2-->![(f_0/f)^2](spwm.assets/eq-inline/32b646eba5.svg)<!--/m--> above its corner, so every factor of 10 in
carrier frequency buys a factor of 100 in ripple rejection — or, at equal ripple, a filter with
ten times smaller <!--m:L-->![L](spwm.assets/eq-inline/d160e0986a.svg)<!--/m--> and <!--m:C-->![C](spwm.assets/eq-inline/32096c2e0e.svg)<!--/m-->. That is the practical reason to push <!--m:f_c-->![f_c](spwm.assets/eq-inline/b1071317de.svg)<!--/m--> up: the higher the
carrier, the further the harmonics are from 50 Hz, and the smaller, cheaper and lighter the filter
that separates them. It is also why the filter corner must sit well above 50 Hz (or it attenuates
and phase-shifts the output you want) and well below <!--m:f_c-->![f_c](spwm.assets/eq-inline/b1071317de.svg)<!--/m-->.

## 7 The modulation index — linear region and overmodulation

The amplitude modulation index <!--m:m_a-->![m_a](spwm.assets/eq-inline/45e7c279a5.svg)<!--/m--> is the ratio of the reference's peak to the carrier's peak.
For <!--m:m_a \le 1-->![m_a leq 1](spwm.assets/eq-inline/c51edbb400.svg)<!--/m--> the reference never leaves the triangle's range, every carrier period
contains two crossings, and the core result holds exactly: the fundamental is <!--m:m_a V_{dc}-->![m_a V_dc](spwm.assets/eq-inline/860cc5cd90.svg)<!--/m-->,
a straight line through the origin. This is the **linear region**.

Push <!--m:m_a > 1-->![m_a > 1](spwm.assets/eq-inline/572b17bbd8.svg)<!--/m--> and near the sine's peaks the reference rises above the whole triangle. For
those carrier periods there are no crossings: the output stays at <!--m:+V_{dc}-->![+V_dc](spwm.assets/eq-inline/0458144a16.svg)<!--/m--> and pulses go missing
(*pulse dropping*). The average is clipped at <!--m:V_{dc}-->![V_dc](spwm.assets/eq-inline/1091080009.svg)<!--/m--> while the reference keeps climbing, so the
fundamental grows more slowly than <!--m:m_a-->![m_a](spwm.assets/eq-inline/45e7c279a5.svg)<!--/m--> and low-order harmonics (3rd, 5th, 7th — close to 50 Hz and
therefore hard to filter) appear. In the limit the output is a <!--m:\pm V_{dc}-->![plus-minus V_dc](spwm.assets/eq-inline/38ec47d61b.svg)<!--/m--> square wave, whose
fundamental is:

![as m_a goes to infinity, V_1 peak equals 2 over T_1 times the integral of v_AB sine theta, equals 4 V_dc over pi, about 1.273 V_dc](spwm.assets/eq-square.svg)

![Computed fundamental output amplitude versus amplitude modulation index: linear up to 1, then overmodulation flattening towards the square-wave limit of 4 over pi](spwm.assets/fig-06.svg)

_The fundamental of the exact pulse train, computed at each <!--m:m_a-->![m_a](spwm.assets/eq-inline/45e7c279a5.svg)<!--/m-->. It follows <!--m:m_a V_{dc}-->![m_a V_dc](spwm.assets/eq-inline/860cc5cd90.svg)<!--/m--> exactly
up to 1, then bends over: <!--m:m_a = 2-->![m_a = 2](spwm.assets/eq-inline/ae6fedef8e.svg)<!--/m--> gives only <!--m:1.216\,V_{dc}-->![1.216 V_dc](spwm.assets/eq-inline/5d6dc5578a.svg)<!--/m-->, and nothing ever exceeds <!--m:1.273\,V_{dc}-->![1.273 V_dc](spwm.assets/eq-inline/af3b378991.svg)<!--/m-->. The
small kinks are individual pulses dropping out._

So overmodulation buys up to 27 % more output voltage from the same bus, at the cost of distortion
your filter cannot remove. Designs that need a clean sine stay in the linear region and size the
bus so that <!--m:m_a \approx 0.8\text{–}0.9-->![m_a approx 0.8 –0.9](spwm.assets/eq-inline/9cec039f64.svg)<!--/m--> at full output.

## 8 The frequency ratio and the harmonic spectrum

Where does the energy that is not 50 Hz go? Because the output is a pulse train repeating once per
50 Hz period, its spectrum contains only multiples of 50 Hz, and the exact amplitudes can be computed
from the switching instants. The result is clean: the harmonics are bunched in **clusters around
the carrier frequency and its multiples**, with sidebands spaced at twice the output frequency.

![Computed harmonic spectra of bipolar and unipolar SPWM at modulation index 0.8 and frequency ratio 21: bipolar clusters at m_f and its multiples, unipolar clusters only around 2 m_f](spwm.assets/fig-07.svg)

_The exact Fourier amplitudes of one 50 Hz period at <!--m:m_a = 0.8-->![m_a = 0.8](spwm.assets/eq-inline/2dd2ee5741.svg)<!--/m-->, <!--m:m_f = 21-->![m_f = 21](spwm.assets/eq-inline/58e407653b.svg)<!--/m-->. The fundamental is
<!--m:0.800\,V_{dc}-->![0.800 V_dc](spwm.assets/eq-inline/b8fa07fcdf.svg)<!--/m--> as §4 predicts. Nothing appears below order 17: the whole region from 50 Hz up to near the
carrier is empty. That gap is what lets a filter work._

For bipolar SPWM, harmonics appear only at orders:

![n equals j m_f plus or minus k with j plus k odd: for j 1, k is 0, 2, 4 and so on; for j 2, k is 1, 3 and so on](spwm.assets/eq-orders.svg)

and their amplitudes have a closed form in Bessel functions (Black's double-Fourier analysis; see
Holmes and Lipo in §14). <!--m:J_k-->![J_k](spwm.assets/eq-inline/6a2b242208.svg)<!--/m--> is the Bessel function of the first kind of order <!--m:k-->![k](spwm.assets/eq-inline/13fbd79c3d.svg)<!--/m-->, a standard
tabulated function, <!--m:J_k(x) = \tfrac1\pi\int_0^\pi \cos(k\tau - x\sin\tau)\,d\tau-->![J_k(x) = 1 pi integral_0^ pi cos (k tau - x sin tau ) d tau](spwm.assets/eq-inline/b22ff240e0.svg)<!--/m-->; it appears because
the pulse edges move sinusoidally, and the Fourier coefficients of a sinusoidally shifted edge are
exactly these integrals:

![V at j m_f plus or minus k equals 4 V_dc over j pi times J_k of j m_a pi over 2 times the absolute value of sine j plus k pi over 2](spwm.assets/eq-bessel.svg)

![for m_a 0.8, V at m_f equals 4 over pi J_0 of 0.4 pi times V_dc, equals 1.273 times 0.6425 V_dc, equals 0.818 V_dc](spwm.assets/eq-bessel-check.svg)

The computed spectrum in Figure 77 gives 0.818 at order 21 and 0.220 at orders 19 and 23 — matching
the formula and the standard textbook table.

**Choosing** <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m-->. Three conventions follow from the spectrum:

- **Integer (synchronous).** If <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> is not an integer, the carrier slides relative to the sine
  from cycle to cycle and produces *subharmonics* below 50 Hz, which no filter can remove. For small
  <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> (below about 21) the carrier must be locked to the reference.
- **Odd.** With an odd integer <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> and a valley-aligned carrier, the bipolar output has half-wave
  symmetry, so even harmonics vanish. (For three-phase inverters, also a multiple of 3, so the
  carrier-frequency harmonics cancel in the line voltages.)
- **Large.** Above roughly <!--m:m_f = 21-->![m_f = 21](spwm.assets/eq-inline/58e407653b.svg)<!--/m--> the subharmonics from an asynchronous carrier are tiny, so
  real inverters simply run a fixed 10–20 kHz carrier with <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> in the hundreds and do not bother
  to lock it.

## 9 Bipolar vs. unipolar switching on an H-bridge

So far the bridge has been switched **bipolar**: one comparator, diagonal pairs swapping, output
always <!--m:\pm V_{dc}-->![plus-minus V_dc](spwm.assets/eq-inline/38ec47d61b.svg)<!--/m-->. An [H-bridge](../h-bridge/) has two legs, and each can be switched on its own.

**Unipolar** SPWM gives each leg its own reference: leg A compares <!--m:+v_{ref}-->![+v_ref](spwm.assets/eq-inline/82514c4add.svg)<!--/m--> with the carrier,
leg B compares <!--m:-v_{ref}-->![-v_ref](spwm.assets/eq-inline/88a57ba7c1.svg)<!--/m--> (the same sine shifted 180°). Each leg output is 0 or <!--m:V_{dc}-->![V_dc](spwm.assets/eq-inline/1091080009.svg)<!--/m-->, and
the load sees their difference. Applying the core result to each leg (a leg switching between 0 and <!--m:V_{dc}-->![V_dc](spwm.assets/eq-inline/1091080009.svg)<!--/m--> averages to
<!--m:V_{dc}(1+r)/2-->![V_dc(1+r)/2](spwm.assets/eq-inline/bd39cba4e9.svg)<!--/m-->):

![v bar_A equals V_dc times 1 plus r over 2, v bar_B equals V_dc times 1 minus r over 2, and v bar_AB equals v bar_A minus v bar_B equals r V_dc](spwm.assets/eq-unipolar.svg)

The same average as bipolar, so the same fundamental. What changes is the ripple.

![Unipolar SPWM: the carrier compared with the reference and its inverse drives each leg separately, and the difference is a three-level output whose pulses repeat at twice the carrier frequency](spwm.assets/fig-08.svg)

_During the positive half-cycle the line voltage switches between <!--m:+V_{dc}-->![+V_dc](spwm.assets/eq-inline/0458144a16.svg)<!--/m--> and 0, never to
<!--m:-V_{dc}-->![-V_dc](spwm.assets/eq-inline/b2ceae7532.svg)<!--/m-->; in the negative half between 0 and <!--m:-V_{dc}-->![-V_dc](spwm.assets/eq-inline/b2ceae7532.svg)<!--/m-->. Each carrier period now contains two output
pulses — one from each leg's edge — so the ripple repeats at twice the carrier frequency._

Three advantages follow. **Three levels:** the voltage steps are <!--m:V_{dc}-->![V_dc](spwm.assets/eq-inline/1091080009.svg)<!--/m--> instead of
<!--m:2V_{dc}-->![2V_dc](spwm.assets/eq-inline/5578b7dfd2.svg)<!--/m-->, halving the ripple amplitude and the <!--m:dv/dt-->![dv/dt](spwm.assets/eq-inline/7631ac0fee.svg)<!--/m--> stress. **Effective frequency
<!--m:2 f_c-->![2 f_c](spwm.assets/eq-inline/cf52f826d9.svg)<!--/m-->:** the odd-<!--m:j-->![j](spwm.assets/eq-inline/5c2dd944dd.svg)<!--/m--> clusters cancel between the legs, leaving only:

![n equals 2 m_f plus or minus 1, 2 m_f plus or minus 3, and so on, 4 m_f plus or minus 1 and so on; only even j survive](spwm.assets/eq-unipolar-orders.svg)

— visible in Figure 77(b), where the cluster at <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> has vanished and the largest harmonic is
<!--m:0.314\,V_{dc}-->![0.314 V_dc](spwm.assets/eq-inline/9c7df77b54.svg)<!--/m--> at order 43. **Smaller filter:** with the first cluster an octave higher and a third of the
amplitude, the same LC filter leaves several times less ripple (worked in §12). The costs are two
comparators (or two timer channels) instead of one, and a slightly more complex gate-drive pattern.
Most commercial single-phase inverters use unipolar switching for these reasons.

## 10 Natural vs. regular sampling — SPWM in a microcontroller

Everything so far is **natural sampling**: the live analogue sine is compared with the triangle, and
each edge falls exactly where they cross. A microcontroller cannot do that — it has no continuous
sine — so it uses **regular (sampled) SPWM**: it samples the sine once per carrier period, holds that
value, and compares the *held* value with the carrier. With the held value constant across the
period, the §4 derivation becomes exact rather than approximate: <!--m:D_k = (1 + r_k)/2-->![D_k = (1 + r_k)/2](spwm.assets/eq-inline/368db1e28f.svg)<!--/m--> with
<!--m:r_k = m_a\sin(2\pi k/m_f)-->![r_k = m_a sin (2 pi k/m_f)](spwm.assets/eq-inline/beb6fbdd3c.svg)<!--/m-->.

![Natural sampling compares the live sine with the carrier; regular sampling holds the sine sampled once per carrier period, which is what a microcontroller lookup table does](spwm.assets/fig-09.svg)

_Top: the held samples (green) are what the timer actually compares with its up-down count. The
two pulse trains differ only slightly. The fundamental drops from <!--m:0.800-->![0.800](spwm.assets/eq-inline/c30e9631e3.svg)<!--/m--> to <!--m:0.786\,V_{dc}-->![0.786 V_dc](spwm.assets/eq-inline/5011032257.svg)<!--/m--> at this very
low <!--m:m_f = 9-->![m_f = 9](spwm.assets/eq-inline/434657633d.svg)<!--/m--> because holding a sample delays it by half a carrier period on average; at
<!--m:m_f = 400-->![m_f = 400](spwm.assets/eq-inline/b682ed8934.svg)<!--/m--> the difference is negligible._

In practice this is a **lookup table** and a timer in centre-aligned mode (its up-down counter *is*
the triangle carrier — see the PWM document, §6). Precompute <!--m:N = f_c/f_1-->![N = f_c/f_1](spwm.assets/eq-inline/5018476e6a.svg)<!--/m--> compare values for one
sine period, and at every timer update load the next one into CCR (usually by DMA, so the CPU is
not involved):

![CCR_k equals round of ARR times 1 plus m_a sine 2 pi k over N, over 2; N equals f_c over f_1 equals 400; ARR equals f_clk over 2 f_c equals 84 MHz over 40 kHz equals 2100](spwm.assets/eq-lut.svg)

An 84 MHz timer at a 20 kHz carrier has 2100 counts (11 bits) of duty resolution and needs a
400-entry table. The digital approach makes several things trivial that are awkward in analogue:
<!--m:m_a-->![m_a](spwm.assets/eq-inline/45e7c279a5.svg)<!--/m--> is a multiplier in software (so output-voltage regulation is a control loop changing one
number), the carrier is automatically synchronous, dead time is a timer register, and unipolar
switching is a second channel with the table read in antiphase.

## 11 The analogue implementation from the video — AD9833 and LM311

The video builds the control signal from discrete parts (screenshots 17–20). They are real,
widely available ICs:

**AD9833 (Analog Devices) — programmable waveform generator.** The chip in video screenshot 17 (11:31) is
marked AD9833. It is a *direct digital synthesis* (DDS) generator in a 10-lead MSOP package: a
28-bit phase accumulator steps through a sine lookup table at the master clock rate (MCLK, up to
25 MHz), and a 10-bit DAC turns the result into a voltage. Configured over a 3-wire SPI interface at
start-up, it outputs a **sine, a triangle or a square** wave, which is why the video uses one chip
for the reference and a second, identical chip for the carrier (screenshot 18). The frequency is set by
a 28-bit register:

![f_out equals FREQREG times f_MCLK over 2 to the 28, and delta f equals 25 MHz over 2 to the 28, about 0.093 hertz](spwm.assets/eq-dds.svg)

Its output is small — about 0.6 V peak-to-peak. The video's "about 0.3 V peak" is that swing
described about its midpoint; on the real pin the waveform does not swing around 0 V but rides on a
DC offset of roughly 0.3 V (from about 38 mV to 0.65 V). Because both chips share the same offset,
comparing them directly still works: the offsets cancel at the comparator.

**LM311 — voltage comparator** (video screenshots 19–20). A classic single comparator with an
open-collector output (it needs a pull-up resistor, and can pull up to a different supply than its
own, convenient for driving gate-driver logic) and a response time of about 200 ns. It outputs high
when its + input (the sine) is above its − input (the triangle), exactly the rule of §3.

Honest notes on this analogue chain, none of which the video mentions:

- **<!--m:m_a-->![m_a](spwm.assets/eq-inline/45e7c279a5.svg)<!--/m--> is fixed at about 1 by default.** Both AD9833s produce the same amplitude whether set to
  sine or triangle, so sine peak equals triangle peak — <!--m:m_a = 1-->![m_a = 1](spwm.assets/eq-inline/714eb8c7e3.svg)<!--/m-->, the very edge of the
  linear region (§7). The chip has no amplitude control; setting <!--m:m_a = 0.8-->![m_a = 0.8](spwm.assets/eq-inline/2dd2ee5741.svg)<!--/m--> needs an
  attenuator on the sine (around its offset) or a digital potentiometer. Regulating the output
  voltage against load changes means adjusting that attenuator in a loop.
- **Synchronising the two chips.** Two independent DDS chips on separate clocks drift relative to
  each other, giving an asynchronous carrier. Clock both from the *same* MCLK, start them with the
  same RESET, and choose the carrier tuning word as an exact integer multiple of the sine's:

![FREQREG_1 equals 537 gives f_1 equal to 50.012 hertz; FREQREG_c equals 400 times 537 equals 214800 gives f_c equal to 20004.9 hertz, exactly 400 f_1](spwm.assets/eq-dds-sync.svg)

  The output is then 50.012 Hz rather than exactly 50 Hz (the price of a 0.093 Hz step), but the
  ratio is exactly 400.
- **Chatter at the crossings.** Near a crossing the two inputs differ by millivolts, and noise makes
  a fast comparator toggle several times. A little hysteresis (a large resistor from output to the +
  input) gives one clean edge per crossing.
- **The comparator gives one signal; the bridge needs four.** A gate driver must produce the
  complementary high/low-side signals and insert dead time (the PWM document, §9). Half-bridge driver
  ICs with built-in dead time (the IR2104 family, about 0.5 µs) or an RC delay network do this.

## 12 Practical numbers for a 230 V inverter

**The modulation index.** 230 V RMS means a peak of:

![V_out peak equals root 2 times 230 V equals 325.3 V, so m_a equals 325.3 over 325 equals 1.001 with V_dc 325 V, and m_a equals 325.3 over 400 equals 0.813 with V_dc 400 V](spwm.assets/eq-user-ma.svg)

The video's 325 V bus requires <!--m:m_a \approx 1.0-->![m_a approx 1.0](spwm.assets/eq-inline/e17358e92b.svg)<!--/m--> — **zero headroom**. Real losses then push it
into overmodulation: switch and diode drops, the dead-time error (about 13 V at 325 V with the
numbers below), the filter inductor's drop at full load, and the bus's own 100 Hz ripple and
sag under load all subtract from the 325 V. The output would come out low and distorted at load.
Practical 230 V inverters use a **350–400 V** bus (a slightly higher transformer ratio in the
first stage) so that full output needs only <!--m:m_a \approx 0.8\text{–}0.9-->![m_a approx 0.8 –0.9](spwm.assets/eq-inline/9cec039f64.svg)<!--/m-->, leaving room for the
regulator to compensate for load and supply changes.

**The carrier.** Typically 10–20 kHz for IGBT or MOSFET bridges at this power: high enough that the
filter is small and (at the top end) above hearing, low enough to keep switching losses modest. At
20 kHz, <!--m:m_f = 400-->![m_f = 400](spwm.assets/eq-inline/b682ed8934.svg)<!--/m-->: 400 samples per sine period.

**The filter.** Take <!--m:L = 2\,\mathrm{mH}-->![L = 2 mH](spwm.assets/eq-inline/2bdaa6938b.svg)<!--/m--> and <!--m:C = 10\,\mu\mathrm{F}-->![C = 10 mu F](spwm.assets/eq-inline/a68d258fbb.svg)<!--/m-->:

![f_0 equals 1 over 2 pi root of 2 mH times 10 microfarad, about 1.13 kHz; f_0 over 20 kHz squared is about 1 over 316, minus 50 dB; f_0 over 40 kHz squared is about 1 over 1264, minus 62 dB](spwm.assets/eq-filter.svg)

The corner sits 22 times above 50 Hz (so the fundamental passes almost untouched) and 18 times below
the carrier. With a 400 V bus at <!--m:m_a = 0.8-->![m_a = 0.8](spwm.assets/eq-inline/2dd2ee5741.svg)<!--/m-->, the largest carrier harmonic left on the output
is:

![bipolar: 0.818 times 400 V over 316, about 1.0 V; unipolar: 0.314 times 400 V over 1264, about 0.10 V, peak, largest carrier harmonic](spwm.assets/eq-ripple.svg)

About 1 V of 20 kHz ripple on 325 V peak with bipolar switching; about 0.1 V with unipolar — the
same parts, ten times cleaner. The filter has its own running costs. A capacitor's current amplitude is <!--m:\omega C-->![omega C](spwm.assets/eq-inline/368bb3750b.svg)<!--/m--> times its
voltage amplitude, and an inductor's voltage amplitude is <!--m:\omega L-->![omega L](spwm.assets/eq-inline/b3beb438d7.svg)<!--/m--> times its current amplitude
(the impedances of [../../filters/lc-filter/lc-filter.md §2](../../filters/lc-filter/lc-filter.md#2-two-laws-read-as-smoothing-rules)),
so in RMS terms the capacitor draws
<!--m:230 \times 2\pi \cdot 50 \times 10\,\mu\mathrm{F} \approx 0.72\,\mathrm{A}-->![230 times 2 pi times 50 times 10 mu F approx 0.72 A](spwm.assets/eq-inline/5266203bc2.svg)<!--/m--> of reactive current
even with no load, and at 1 kW (4.35 A) the inductor drops
<!--m:2\pi \cdot 50 \times 2\,\mathrm{mH} \times 4.35\,\mathrm{A} \approx 2.7\,\mathrm{V}-->![2 pi times 50 times 2 mH times 4.35 A approx 2.7 V](spwm.assets/eq-inline/71d226ab95.svg)<!--/m-->.

## 13 What this costs you

- **Switching losses scale with the carrier.** Each switch loses energy at every edge; roughly:

![P_sw is approximately one half V_dc I bar t_r plus t_f f_c, approximately one half times 400 times 3.9 times 100 ns times 20 kHz, about 1.6 W per switch](spwm.assets/eq-switching-loss.svg)

  At 20 kHz that is a few watts for the bridge at 1 kW; at 100 kHz it is five times more, plus gate
  drive. A higher carrier buys a smaller filter with heat.
- **Dead-time distortion.** Every carrier period loses (or gains, depending on the current's
  direction) a slice <!--m:t_d-->![t_d](spwm.assets/eq-inline/6c703960eb.svg)<!--/m--> of each leg's pulse. The error is a square wave in phase with the load
  current:

![delta v bar is approximately 2 t_d f_c V_dc, equals 2 times 1 microsecond times 20 kHz times 400 V, equals 16 V, about 5 percent of 325 V](spwm.assets/eq-deadtime.svg)

  It flattens the sine near every current zero crossing and adds 3rd, 5th and 7th harmonics that the
  filter passes. It grows with <!--m:f_c-->![f_c](spwm.assets/eq-inline/b1071317de.svg)<!--/m-->, so it is another price of a fast carrier; digital controllers
  compensate by adding <!--m:\pm t_d-->![plus-minus t_d](spwm.assets/eq-inline/e2ab02aabc.svg)<!--/m--> to each CCR according to the measured current sign.
- **EMI.** The bridge slews hundreds of volts in tens of nanoseconds, tens of thousands of times a
  second. That couples through stray capacitance to the heatsink and earth (common-mode noise) and
  radiates. Unipolar switching halves the step size; layout, snubbers and an EMI filter do the rest.
- **Audible noise.** Carriers below about 20 kHz make the filter inductor and transformer
  magnetostrict and whine at <!--m:f_c-->![f_c](spwm.assets/eq-inline/b1071317de.svg)<!--/m--> (and at <!--m:2f_c-->![2f_c](spwm.assets/eq-inline/4160ba2d6d.svg)<!--/m--> with unipolar). Many low-cost inverters sit
  at 16–20 kHz and are faintly audible.
- **No headroom without a bigger bus.** The linear region tops out at <!--m:\hat V_1 = V_{dc}-->![V_1 = V_dc](spwm.assets/eq-inline/a723c56ffd.svg)<!--/m-->. A
  230 V output needs comfortably more than 325 V of bus (§12); overmodulation is a last resort that
  trades voltage for low-order distortion.
- **Low <!--m:m_f-->![m_f](spwm.assets/eq-inline/eff6a35f63.svg)<!--/m--> needs care.** Below about 21 the carrier must be synchronous and odd, or
  subharmonics appear that no filter can remove — the analogue two-chip circuit must share a clock to
  achieve this.

## 14 Sources and cross-links

- PWM itself — duty cycle, the average, timers, the spectrum, dead time: [../../pwm/](../../pwm/).
- The bridge that SPWM drives, and its switching states: [../h-bridge/](../h-bridge/).
- The filter that recovers the sine: [../../filters/lc-filter/](../../filters/lc-filter/).
- Fourier series, spectra and RMS: [../../fundamentals/signals/](../../fundamentals/signals/).
- Where the 325 V bus comes from: [../../rectifiers/](../../rectifiers/),
  [../../fundamentals/transformer/](../../fundamentals/transformer/).
- The constant-D version of the same averaging: [../../dc-dc-converters/buck/](../../dc-dc-converters/buck/).
- N. Mohan, T. Undeland, W. Robbins, *Power Electronics: Converters, Applications and Design*,
  3rd ed., ch. 8 (switch-mode DC–AC inverters; the harmonic table for <!--m:m_a = 0.8-->![m_a = 0.8](spwm.assets/eq-inline/2dd2ee5741.svg)<!--/m--> that
  Figure 77 reproduces).
- D. G. Holmes, T. A. Lipo, *Pulse Width Modulation for Power Converters*, IEEE Press/Wiley, 2003
  (natural and regular sampling, the Bessel-function harmonic solution).
- Analog Devices, *AD9833 Low Power, Programmable Waveform Generator* data sheet; Texas Instruments,
  *LM311 Differential Comparator* data sheet.
- Source video: *DC to AC* (`Power_Supply/Videos/DC_to_AC_1.mp4`), SPWM section ≈ 10:25–16:45.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
