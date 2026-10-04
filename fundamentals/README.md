# Fundamentals — the defining laws and the language of waveforms

The whole of switched-mode power conversion rests on two component laws and a little calculus.
This topic builds both from first principles, then goes one level down to the field law beneath
them (Faraday's), uses it to derive the transformer, and sets up the waveform language — Fourier
series, AC and RMS — that the inverter documents speak. So when you reach the
[buck](../dc-dc-converters/buck/) and [boost](../dc-dc-converters/boost/) converters, the
[rectifiers](../rectifiers/) or the [inverters](../dc-ac-inverters/), nothing is asserted — it is
all derived.

> **The thesis in one line**
>
> An inductor fixes how fast **current** can change; a capacitor fixes how fast **voltage** can
> change. They are exact mirrors of each other — learn one and you get the other by swapping
> ![V I](README.assets/eq-inline/4ebcd6796a.svg)<!--m:V \leftrightarrow I--> and ![L C](README.assets/eq-inline/6cef383173.svg)<!--m:L \leftrightarrow C-->.

## Documents

| Subtopic | The law | What it covers |
|---|---|---|
| [inductor/](inductor/) | ![V_L = L times dI/dt](README.assets/eq-inline/4e5d46c427.svg)<!--m:V_L = L \cdot dI/dt--> | magnetic-field storage, the constant-voltage ramp (with the definite-integral proof done slowly), the polarity flip that powers a boost, and the inductive-kick footgun |
| [capacitor/](capacitor/) | ![I_C = C times dV/dt](README.assets/eq-inline/56baca3b41.svg)<!--m:I_C = C \cdot dV/dt--> | electric-field storage, **the proper proof of why you may differentiate ![Q = C times V](README.assets/eq-inline/205c11f7c4.svg)<!--m:Q = C \cdot V-->**, the mirror ramp, and the inductor↔capacitor duality |
| [electromagnetism/](electromagnetism/) | ![v = N d Phi/dt](README.assets/eq-inline/bc981422f7.svg)<!--m:v = N\,d\Phi/dt--> | charge, field and flux; Ampère and Biot–Savart; Faraday's and Lenz's laws; self-inductance ![L = N Phi/I](README.assets/eq-inline/2f88ce88e0.svg)<!--m:L = N\Phi/I-->, which derives the inductor law; mutual inductance; energy in the field; magnetic materials; and why "B = dV/dt" is not the law |
| [transformer/](transformer/) | ![v_s/v_p = N_s/N_p](README.assets/eq-inline/28d0571142.svg)<!--m:v_s/v_p = N_s/N_p--> | the ideal transformer from Faraday's law; turns ratio for voltage and current; magnetising inductance; peak core flux ![Phi = V/(4Nf)](README.assets/eq-inline/bbe0bb314f.svg)<!--m:\hat\Phi = V/(4Nf)-->, so a higher frequency means a smaller core; saturation and why DC cannot pass; core and copper losses; 50 Hz iron vs 50 kHz ferrite |
| [signals/](signals/) | ![V_rms = V_pk/sqrt 2](README.assets/eq-inline/2803e3393c.svg)<!--m:V_{rms} = V_{pk}/\sqrt2--> | [edges and Fourier series](signals/edges-and-fourier.md) (rise time, slew rate, the square wave as ![(4V/pi ) sum sin (n omega t)/n](README.assets/eq-inline/514697ef02.svg)<!--m:(4V/\pi)\sum \sin(n\omega t)/n-->, the H-bridge spectrum, THD); [AC and RMS](signals/ac-and-rms.md) (why 230 V means 325 V peak, RMS vs average, South African mains quality under NRS 048-2) |

## Reading order

1. [inductor/inductor.md](inductor/inductor.md) — start here; the ramp argument is simplest to
   see with current as the effect.
2. [capacitor/capacitor.md](capacitor/capacitor.md) — the mirror, plus the calculus rule that
   the whole subject leans on.
3. [electromagnetism/electromagnetism.md](electromagnetism/electromagnetism.md) — where the
   inductor law comes from, and Faraday's law in full.
4. [transformer/transformer.md](transformer/transformer.md) — Faraday's law applied to two
   windings on one core.
5. [signals/ac-and-rms.md](signals/ac-and-rms.md), then
   [signals/edges-and-fourier.md](signals/edges-and-fourier.md) (which uses the RMS result) — the
   waveform language for the inverter documents.

## Conventions

Follows [../STYLE.md](../STYLE.md). All maths and diagrams are generated SVGs — display equations,
inline symbols, and the arXiv-style ![V](README.assets/eq-inline/c9ee5681d3.svg)<!--m:V-->-vs-![t](README.assets/eq-inline/8efd86fb78.svg)<!--m:t--> / ![I](README.assets/eq-inline/ca73ab6556.svg)<!--m:I-->-vs-![t](README.assets/eq-inline/8efd86fb78.svg)<!--m:t--> figures are each typeset once by the
[toolchain](../../toolchain/README.md) and embedded as images, so they render identically
everywhere with no markdown math plugin.
