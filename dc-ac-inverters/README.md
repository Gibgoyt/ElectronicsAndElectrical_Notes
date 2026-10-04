# DC-AC inverters — H-bridge and SPWM

How a DC rail becomes an alternating voltage. Two ideas carry the whole section: a bridge of four
switches that can connect the load to the supply forwards or backwards, and a modulation scheme
that times those switches so the *average* output traces a sine.

> **The thesis in one line**
>
> An H-bridge can only put ![+V_dc](README.assets/eq-inline/0458144a16.svg)<!--m:+V_{dc}-->, ![0](README.assets/eq-inline/b6589fc6ab.svg)<!--m:0--> or ![-V_dc](README.assets/eq-inline/b2ceae7532.svg)<!--m:-V_{dc}--> across its load; sinusoidal PWM chooses
> between them thousands of times a cycle so that, after a filter, the load sees a clean sine.

## Documents

| Subtopic | What it covers |
|---|---|
| [h-bridge/](h-bridge/) | four switches and every state; the ±V square wave, its RMS and fundamental; MOSFETs as switches; switching loss; bootstrap high-side drive; dead time; inductive loads and snubbers; square-wave, bipolar and unipolar drive; the two bridges of a 12 V to 230 V inverter worked through |
| [spwm/](spwm/) | sinusoidal PWM: a sine reference against a triangle carrier, modulation index, frequency ratio, the harmonic spectrum, bipolar versus unipolar, overmodulation |

## Reading order

1. [h-bridge/h-bridge.md](h-bridge/h-bridge.md) — the power stage.
2. [spwm/](spwm/) — how the second bridge of the inverter is driven to make 50 Hz.

Background that these assume: the [inductor](../fundamentals/inductor/) and
[capacitor](../fundamentals/capacitor/) laws, the Fourier view of a square wave in
[signals](../fundamentals/signals/), and [PWM](../pwm/) in general. The output is smoothed by the
[LC filter](../filters/lc-filter/).

## Conventions

Follows [../STYLE.md](../STYLE.md). Switches are named as in the source video: Q1 top-left, Q2
top-right, Q3 bottom-left, Q4 bottom-right; the load voltage is ![V_AB = V_A - V_B](README.assets/eq-inline/2d7b40a3c8.svg)<!--m:V_{AB} = V_A - V_B-->.
