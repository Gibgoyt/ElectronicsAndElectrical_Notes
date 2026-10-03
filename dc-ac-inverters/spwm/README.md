# Sinusoidal PWM (SPWM) inverters

Deep treatment of SPWM: how comparing a sine reference with a triangle carrier switches an
H-bridge so that its switching-cycle average is a sine; the derivation of
<!--m:D_k = (1 + m_a\sin\theta_k)/2-->![D_k = (1 + m_a sin theta_k)/2](README.assets/eq-inline/3c0373f336.svg)<!--/m--> and <!--m:\overline{v}_k = m_a V_{dc}\sin\theta_k-->![v_k = m_a V_dc sin theta_k](README.assets/eq-inline/0ccbc1f3f8.svg)<!--/m-->; why raising the
carrier frequency gets the output closer to the reference (the question from video screenshots
22–23); the spectrum, the modulation index and overmodulation, bipolar vs. unipolar switching,
microcontroller lookup-table SPWM; the AD9833 + LM311 circuit from the video; and real numbers for
a 230 V inverter. Full argument in [spwm.md](spwm.md).

> **The thesis in one line**
>
> A straight-sided triangle turns the reference's value into a proportional on-time, so each
> carrier period's average output is one sample of <!--m:m_a V_{dc}\sin\theta-->![m_a V_dc sin theta](README.assets/eq-inline/ea8a7d58a4.svg)<!--/m-->; the carrier frequency is
> the sampling rate, and an LC filter turns the samples back into the sine.

## Contents

The treatment covers:

1. Where SPWM sits in the 12 V to 230 V inverter (fig-01).
2. From constant <!--m:D-->![D](README.assets/eq-inline/50c9e8d5fc.svg)<!--/m--> to a time-varying <!--m:D-->![D](README.assets/eq-inline/50c9e8d5fc.svg)<!--/m-->.
3. The reference, the triangle carrier, <!--m:m_a-->![m_a](README.assets/eq-inline/45e7c279a5.svg)<!--/m-->, <!--m:m_f-->![m_f](README.assets/eq-inline/eff6a35f63.svg)<!--/m--> and the comparator rule (fig-02).
4. **The core result**: <!--m:D_k = (1 + m_a\sin\theta_k)/2-->![D_k = (1 + m_a sin theta_k)/2](README.assets/eq-inline/3c0373f336.svg)<!--/m-->, so <!--m:\overline{v}_k = m_a V_{dc}\sin\theta_k-->![v_k = m_a V_dc sin theta_k](README.assets/eq-inline/0ccbc1f3f8.svg)<!--/m--> (fig-03).
5. Why a faster carrier gets closer to the sine — <!--m:m_f-->![m_f](README.assets/eq-inline/eff6a35f63.svg)<!--/m--> = 2, 6, 21, 100 (fig-04).
6. Filtering: one LC filter against three carriers (fig-05).
7. Modulation index, the linear region and overmodulation to <!--m:4/\pi-->![4/pi](README.assets/eq-inline/1b47755c76.svg)<!--/m--> (fig-06).
8. The frequency ratio and the computed harmonic spectrum, sidebands at <!--m:m_f \pm 2-->![m_f plus-minus 2](README.assets/eq-inline/cbad117c83.svg)<!--/m--> (fig-07).
9. Bipolar vs. unipolar switching, three-level output, ripple at <!--m:2f_c-->![2f_c](README.assets/eq-inline/4160ba2d6d.svg)<!--/m--> (fig-08).
10. Natural vs. regular sampling; lookup-table SPWM in a microcontroller (fig-09).
11. The analogue build from the video: two AD9833 DDS generators and an LM311 comparator.
12. Practical numbers: 325 V vs. 400 V bus, a 20 kHz carrier, a 2 mH / 10 µF filter.
13. What this costs you.
14. Sources and cross-links.

Figures 71–79.

## Reading order

Read [../../pwm/](../../pwm/) first (duty cycle, the average, timers, dead time), and the
[H-bridge](../h-bridge/) for the switching states. Then [spwm.md](spwm.md), and the
[LC filter](../../filters/lc-filter/) that finishes the job.

## Links

- Style guide: [../../STYLE.md](../../STYLE.md)
- PWM: [../../pwm/](../../pwm/)
- The bridge: [../h-bridge/](../h-bridge/)
- The filter: [../../filters/lc-filter/](../../filters/lc-filter/)
- Spectra and RMS: [../../fundamentals/signals/](../../fundamentals/signals/)
