# Signals — edges, Fourier series, AC and RMS

The language every switching circuit in this tree is described in: what an edge is and why its
speed matters, how any repeating waveform breaks down into sines (Fourier series), and what the
"230 V" on a mains socket actually measures. Two documents, ten figures (20–29).

> **The thesis in one line**
>
> A switched waveform is a sum of sines whose amplitudes the circuit sets. An H-bridge into a lamp
> gives <!--m:(4V_{dc}/\pi)\sum_{n\ \text{odd}} \sin(n\omega t)/n-->![(4V_dc/ ) _n odd (n t)/n](README.assets/eq-inline/c7cd52a986.svg)<!--/m-->. Mains is quoted by the RMS of its sine,
> so 230 V means a peak of <!--m:230\sqrt2 = 325\,\mathrm{V}-->![230 2 = 325 V](README.assets/eq-inline/bed7f4dde5.svg)<!--/m-->.

## Documents

| Document | Result | What it covers |
|---|---|---|
| [edges-and-fourier.md](edges-and-fourier.md) | <!--m:\tfrac{4V}{\pi}\sum_{n\ \text{odd}}\tfrac{\sin n\omega t}{n}-->![4V _n odd n tn](README.assets/eq-inline/61022f1f8e.svg)<!--/m--> | rising and falling edges, 10–90 % rise time, slew rate, why real edges are finite (gate charge, parasitic C, loop L), the <!--m:C\,dv/dt-->![C dv/dt](README.assets/eq-inline/b96b608a56.svg)<!--/m--> and <!--m:L\,di/dt-->![L di/dt](README.assets/eq-inline/f24cc20a0b.svg)<!--/m--> consequences; Fourier series from scratch with orthogonality derived; the square wave derived, Gibbs, Parseval, THD of 48.3 %; **the H-bridge output as a Fourier series** with lamp resistance, <!--m:R_{ds(on)}-->![R_ds(on)](README.assets/eq-inline/6dace34914.svg)<!--/m-->, dead time / quasi-square angle <!--m:\alpha-->![](README.assets/eq-inline/f7c665b459.svg)<!--/m--> and edge speed as variables (figs 20–26) |
| [ac-and-rms.md](ac-and-rms.md) | <!--m:V_{rms} = V_{pk}/\sqrt2-->![V_rms = V_pk/ 2](README.assets/eq-inline/2803e3393c.svg)<!--/m--> | sinusoid, period, phase; signed average 0, rectified average <!--m:0.637\,V_{pk}-->![0.637 V_pk](README.assets/eq-inline/445f8a5890.svg)<!--/m-->; RMS defined by equal heating and derived for a sine; why 230 V peaks at 325 V; square and triangle RMS; is SA mains really a sine (flat-topping, NRS 048-2, ±10 %, 8 % THD); true-RMS versus average-responding meters (figs 27–29) |

## Reading order

1. [ac-and-rms.md](ac-and-rms.md) first if the 325 V question is what brought you here. It needs
   nothing beyond a sine and an integral.
2. [edges-and-fourier.md](edges-and-fourier.md) next. It uses the RMS result in its power-per-harmonic
   section and is the foundation for [../../pwm/](../../pwm/),
   [../../dc-ac-inverters/spwm/](../../dc-ac-inverters/spwm/) and
   [../../filters/lc-filter/](../../filters/lc-filter/).

## Links

- Parent index: [../README.md](../README.md)
- Style guide: [../../STYLE.md](../../STYLE.md)
- The two laws behind edge behaviour: [../inductor/inductor.md](../inductor/inductor.md),
  [../capacitor/capacitor.md](../capacitor/capacitor.md)
- Where the square wave comes from: [../../dc-ac-inverters/h-bridge/](../../dc-ac-inverters/h-bridge/)
