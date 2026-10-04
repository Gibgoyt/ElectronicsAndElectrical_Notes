# Electronics & Electrical — study notes

A ground-up, first-principles knowledge base for power electronics, built the same way the
[Proqmed storage docs](https://example.invalid) are: narrative prose that *explains*, one
generated SVG figure per major idea, and every number stated verbatim with units. The
goal is understanding you can rebuild from scratch — not a formula sheet.

> **The thesis in one line**
>
> Three laws are the whole of it — the inductor law, the capacitor law, and Faraday's law beneath
> them both:

![V_L = L dI_L/dt](README.assets/eq-inductor-law.svg) &nbsp; , &nbsp; ![I_C = C dV_C/dt](README.assets/eq-capacitor-law.svg) &nbsp; and &nbsp; ![v = N dPhi/dt](README.assets/eq-faraday-law.svg)

Every circuit in this tree is those laws applied to something that switches on and off. Buck and
boost converters get their step ratios from volt-second balance on the inductor; a transformer
steps voltage by its turns ratio because both windings see the same changing flux; an LC filter
keeps the average of a PWM wave because the inductor resists changes of current and the capacitor
changes of voltage; a rectifier and its capacitor turn AC back into DC; and an H-bridge driven by
sinusoidal PWM makes a 230 V, 50 Hz sine out of a DC bus. Nothing is assumed — each result is
*derived* from the laws, and the whole chain adds up to the 12 V DC to 230 V AC inverter.

## How to read this tree

Start at the fundamentals and only then open the circuits — every circuit document leans on the
laws, the waveform language (Fourier series, RMS), and the transformer derived there.

| Topic | What it covers | Status |
|---|---|---|
| [fundamentals/](fundamentals/) | The three field laws from first principles — [Coulomb's](fundamentals/coulombs-law/) (charge, the electric field, the volt), [Ampère's](fundamentals/amperes-law/) (a current makes a magnetic field) and [Faraday's](fundamentals/faradays-law/) (a changing flux makes an EMF, ![E = -N d Phi_B/dt](README.assets/eq-inline/9ae24fbb2c.svg)<!--m:\mathcal{E} = -N\,d\Phi_B/dt-->, with Lenz's minus sign); the inductor and capacitor laws derived from them; the electromagnetism overview (self- and mutual inductance, cores, saturation, frequency and core size); the transformer (turns ratio, ![Phi = V/(4Nf)](README.assets/eq-inline/bbe0bb314f.svg)<!--m:\hat\Phi = V/(4Nf)-->, why high frequency means a small core); signals (AC and RMS, edges, Fourier series, mains quality) | **written** |
| [dc-dc-converters/](dc-dc-converters/) | Buck (step-down) and boost (step-up): the two switching intervals, volt-second balance, the derived step ratios, sizing L and C, and how each one starts up (the slow climb, overshoot, inrush, soft-start) | **written** |
| [filters/](filters/) | The LC low-pass filter as a PWM averager: ![omega_0 = 1/sqrt LC](README.assets/eq-inline/f11b5cd8a1.svg)<!--m:\omega_0 = 1/\sqrt{LC}-->, Q, 40 dB per decade, ripple maths, and varying the duty cycle to shape the output | **written** |
| [pwm/](pwm/) | Pulse-width modulation: duty cycle, the average ![D V_in](README.assets/eq-inline/6db2223680.svg)<!--m:D\,V_{in}-->, timer and comparator methods, the spectrum, dead time | **written** |
| [rectifiers/](rectifiers/) | Half-wave and full-bridge rectifiers, average and RMS, the reservoir capacitor and its ripple ![Delta V approx I/(2fC)](README.assets/eq-inline/5fe7f97b88.svg)<!--m:\Delta V \approx I/(2fC)-->, conduction angle, capacitor-only vs choke-input, 50 Hz vs 50 kHz | **written** |
| [dc-ac-inverters/](dc-ac-inverters/) | The H-bridge (switch states, shoot-through, dead time, gate drive, freewheeling) and sinusoidal PWM (carrier and reference, ![m_a V_dc sin theta](README.assets/eq-inline/ea8a7d58a4.svg)<!--m:m_a V_{dc}\sin\theta-->, spectrum, bipolar vs unipolar, overmodulation) | **written** |

Suggested order:

1. [fundamentals/coulombs-law/](fundamentals/coulombs-law/) — charge, current as charge per
   second, the electric field, and the volt as a joule per coulomb.
2. [fundamentals/amperes-law/](fundamentals/amperes-law/) — a current makes a magnetic field; the
   right-hand grip rule; the solenoid field every coil is built on.
3. [fundamentals/faradays-law/](fundamentals/faradays-law/) — a *changing* flux makes an EMF,
   ![E = -N d Phi_B/dt](README.assets/eq-inline/9ae24fbb2c.svg)<!--m:\mathcal{E} = -N\,d\Phi_B/dt-->, and Lenz's law explains the minus sign.
4. [fundamentals/inductor/](fundamentals/inductor/) — Faraday's law on a coil's own flux gives the
   inductor law; then the ramp and the polarity flip.
5. [fundamentals/capacitor/](fundamentals/capacitor/) — the mirror law, and **the proper
   proof of why you may differentiate ![Q = C times V](README.assets/eq-inline/205c11f7c4.svg)<!--m:Q = C \cdot V-->** (the step that trips everyone up).
6. [fundamentals/electromagnetism/](fundamentals/electromagnetism/) — the overview that ties the
   three laws together: the terminal-voltage form ![v = N d Phi/dt](README.assets/eq-inline/bc981422f7.svg)<!--m:v = N\,d\Phi/dt-->, why flux is the running
   integral of voltage, self- and mutual inductance, cores and saturation.
7. [fundamentals/transformer/](fundamentals/transformer/) — two windings, one flux; the turns
   ratio, and why a higher frequency means *less* peak flux and a smaller core.
8. [fundamentals/signals/](fundamentals/signals/) — AC and RMS first (why 230 V means a 325 V
   peak), then edges and Fourier series, which uses the RMS result.
9. [dc-dc-converters/buck/](dc-dc-converters/buck/) — ![V_out = D times V_in](README.assets/eq-inline/645cc1e7b2.svg)<!--m:V_{out} = D \cdot V_{in}-->, derived,
   then [its start-up](dc-dc-converters/buck/startup.md).
10. [dc-dc-converters/boost/](dc-dc-converters/boost/) — ![V_out = V_in/(1-D)](README.assets/eq-inline/ff557ad27a.svg)<!--m:V_{out} = V_{in}/(1-D)-->, same
    method, then [its start-up](dc-dc-converters/boost/startup.md).
11. [filters/lc-filter/](filters/lc-filter/) — the buck's LC seen as a filter that keeps the PWM
    average and rejects the switching.
12. [pwm/](pwm/) — PWM as a signal in its own right: generation, spectrum, dead time.
13. [rectifiers/](rectifiers/) — [half-wave](rectifiers/half-wave.md), then
    [full-bridge](rectifiers/full-bridge.md) with its reservoir capacitor (the inverter's 325 V bus).
14. [dc-ac-inverters/h-bridge/](dc-ac-inverters/h-bridge/) — four switches that reverse the load.
15. [dc-ac-inverters/spwm/](dc-ac-inverters/spwm/) — timing those switches so the filtered output
    is a sine.

If you already know the physics, start at step 4; each document links back to the field law it
uses.

## Conventions

Everything here follows [STYLE.md](STYLE.md): a one-line thesis, a numbered Contents, honest
"what this costs you" callouts, and figures that carry the argument on their own. **All maths and
all diagrams are generated SVGs** — every equation is typeset once by the
[toolchain](../toolchain/README.md) and embedded as an image, so it renders identically on GitHub,
GitLab, and any offline viewer, with no dependency on a markdown math plugin.

## Where this came from

These notes grew out of a long worked conversation and twelve pages of handwritten study
notes on buck/boost converters, then widened to the full 12 V DC to 230 V AC inverter shown in a
build video (H-bridge, transformer, rectifier, SPWM, LC filter). The confusions flagged in those notes — *when* it is legal to
apply ![d/dt](README.assets/eq-inline/9560a2e5f1.svg)<!--m:d/dt--> to both sides of an equation, why a constant voltage gives a straight-line
current, what the ramp graphs actually look like — are addressed head-on in the relevant
sections rather than glossed over.
