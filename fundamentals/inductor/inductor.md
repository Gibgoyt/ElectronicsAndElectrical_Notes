# The inductor — voltage is proportional to the rate of change of current

An inductor is the simplest component whose behaviour is genuinely a differential equation.
Once you see *why* a constant voltage across it produces a straight-line current, every
switching-converter waveform in this tree becomes readable at a glance.

**Contents**

1. [What an inductor actually is](#1-what-an-inductor-actually-is)
2. [The defining law](#2-the-defining-law)
3. [From the law to the ramp — the integral, done slowly](#3-from-the-law-to-the-ramp--the-integral-done-slowly)
4. [Polarity, Lenz, and the sign flip](#4-polarity-lenz-and-the-sign-flip)
5. [At the poles](#5-at-the-poles)
6. [The inductive kick, and why the diode is there](#6-the-inductive-kick-and-why-the-diode-is-there)
7. [What this costs you](#7-what-this-costs-you)
8. [Sources and cross-links](#8-sources-and-cross-links)

> **The thesis in one line**
>
> The voltage across an inductor is proportional to how fast its current is changing. Hold the
> voltage constant and the current is forced into a straight ramp:

![V_L equals L times dI_L by dt](inductor.assets/eq-law.svg)

---

## 1 What an inductor actually is

An inductor is **a coil of wire**. That is the entire physical object — no plates, no gap,
nothing separating anything. When current flows through a wire it wraps a circular magnetic
field around itself (Ampère's law: point your right thumb along the current, your fingers curl
the field). Coil the wire, and every loop's field lines add together inside the coil, building
one strong, concentrated magnetic field through the core.

That magnetic field **is** the stored energy:

![E equals one half L I squared](inductor.assets/eq-energy.svg)

There is no charge sitting anywhere waiting to be released — the energy lives entirely in the
field created by the moving charge (the current). This is the first thing to get straight,
because it is the exact opposite of a capacitor:

> **📝 Note —** An inductor does **not** have two plates and does **not** separate charge. If you
> are picturing charge building up on something, you are picturing a capacitor. An inductor stores
> energy in a *magnetic field made by current*, not in *separated static charge*. The two
> mechanisms are completely different — see [../capacitor/capacitor.md](../capacitor/capacitor.md).

## 2 The defining law

From Faraday's law of induction, the voltage across an inductor is proportional to the rate of
change of the current through it:

![V_L equals L times dI_L by dt](inductor.assets/eq-law.svg)

- **`V_L`** — the voltage across the inductor's two terminals ("poles"), in volts (V).
- **`I_L`** — the current flowing through it, in amps (A).
- **`L`** — the inductance, in henries (H). A constant fixed by geometry alone: number of turns,
  core material, size. Think of it as *stiffness against sudden current changes* — a bigger `L`
  means the same voltage produces a slower change of current.

Rearranged for the rate of change:

![dI_L by dt equals V_L over L](inductor.assets/eq-law-rearranged.svg)

**The units of the henry.** A natural question: what are the units of `L`? Solve the defining law
for it — the ratio of volts to (amps per second) is *defined* as the henry:

![units of L reduce to volt second per ampere, one henry](inductor.assets/eq-units-henry.svg)

Check it forwards — multiply `L` (in V·s/A) by `dI/dt` (in A/s) and the amps and seconds cancel,
leaving volts:

![V s over A times A over s equals V](inductor.assets/eq-units-check.svg)

> **💡 Tip —** This is a good habit for the whole subject: whenever a formula looks unfamiliar,
> cancel the units. If they don't reduce to what the left-hand side claims, you've mis-remembered
> the formula.

## 3 From the law to the ramp — the integral, done slowly

Here is the step that is genuinely easy to lose. We have `dI_L/dt = V_L/L`. Suppose `V_L` is
**held constant** (this is exactly what happens inside a converter during each switching
interval). Integrate both sides with respect to time, from the moment we start the clock (`0`) to
some later time `t`:

![definite integral of dI_L over 0 to t gives I_L of t minus I_L of 0 equals V_L over L times t](inductor.assets/eq-integral.svg)

Two things worth pinning down, because both are common sticking points:

- **This is a *definite* integral, not an indefinite one.** There is no floating "+C" to worry
  about. The left side evaluates to `I_L(t) − I_L(0)` by the Fundamental Theorem of Calculus —
  full stop. Nobody is secretly assuming a constant of integration is zero.
- **`I_L(0)` is the initial condition** — whatever current was already flowing when you started
  the clock. If you start from zero current it simplifies to `I_L(t) = (V_L/L)·t`, but that is a
  *choice of scenario*, not something baked into the maths. Inside a converter in steady state,
  `I_L(0)` is genuinely non-zero. The full result is:

![I_L of t equals I_L of 0 plus V_L over L times t](inductor.assets/eq-ramp-result.svg)

With `V_L` constant, `I_L` is a **straight line** in time. Its slope is `V_L/L` and never changes
— so there is no curve, just a ramp.

![Constant voltage across an inductor produces a linear current ramp](inductor.assets/fig-01.svg)

_The flat cause (V_L on top) sets the slope of the straight-line effect (I_L below): double the
voltage and the ramp gets twice as steep. This is `dI_L/dt = V_L/L` made visible._

> **⚠️ Watch out —** If `V_L` is **not** constant — say it ramps, `V_L = k·t` — then
> `dI_L/dt = k·t/L`, and integrating gives `I_L ∝ t²`, a curve. A straight-line current requires a
> *flat* voltage. And constant current (`dI/dt = 0`) requires `V_L = 0` — zero volts across the
> inductor, not a steady voltage. Getting this backwards is the most common inductor mistake.

## 4 Polarity, Lenz, and the sign flip

Which terminal is positive? Use the passive-component convention (the same one you use for a
resistor): current flows from the `+` terminal to the `−` terminal *inside* the component.

- **While the current is increasing** (`dI/dt > 0`, so `V_L > 0`): the entry terminal is `+` and
  the exit is `−` — exactly like a resistor. The inductor is *absorbing* power, and by Lenz's law
  it generates a back-EMF that opposes the increase.
- **While the current is decreasing** (`dI/dt < 0`, so `V_L < 0`): the polarity **flips**. Now the
  exit terminal is `+` and the entry is `−`. The inductor is *delivering* power, acting like a
  battery that pushes current forward using its stored magnetic energy.

![Inductor terminal polarity reverses when the current changes from rising to falling](inductor.assets/fig-03.svg)

_The inductor never commits to a fixed polarity — it takes whatever sign keeps the current
changing smoothly. That reversal, the exit terminal jumping above the entry, is literally how a
boost converter pushes its output above the input._

> **💡 Tip —** Remember this reversal. It is not a curiosity — it is the mechanism of the
> [boost converter](../../dc-dc-converters/boost/boost.md). When the switch opens and the current
> starts to fall, the inductor's voltage flips and *adds on top of* the input.

## 5 At the poles

At the two terminals, current flows **straight through the coil the entire time** — nothing is
blocked. The "storage" is not a dammed-up path; it is the magnetic field surrounding the wire,
which grows and shrinks as the current changes.

![Current flows continuously through an inductor coil while its magnetic field pulses](inductor.assets/fig-02.svg)

_Charge travels all the way through, terminal A to terminal B, while the B-field pulses. Contrast
this with a capacitor, where no charge ever crosses the gap — see
[../capacitor/capacitor.md §5](../capacitor/capacitor.md#5-at-the-poles)._

## 6 The inductive kick, and why the diode is there

Because voltage is proportional to `dI/dt`, forcing the current to change *infinitely fast* would
demand an *infinite* voltage. So what happens if you simply open a switch in series with an
inductor, breaking its current path outright?

The inductor "tries" to keep its current flowing and, denied any path, generates a huge voltage
spike — in an idealised model, unboundedly large. This is the **inductive kick** (or "flyback"):
it is exactly how an old car ignition coil makes tens of thousands of volts from a 12 V battery,
and exactly why every relay or motor driven by a transistor needs a *flyback diode* across it.

> **⚠️ Watch out —** In a buck or boost converter this is precisely what the **diode** (or the
> second MOSFET in a synchronous design) prevents. The instant the switch opens, the diode gives
> the inductor's current an alternate path within nanoseconds. The current itself never stops —
> only its *slope* changes abruptly, from ramping up to ramping down, making a sharp kink in the
> triangle wave. A kink is finite and fine; a *broken path* is what causes the dangerous spike.

## 7 What this costs you

- **An inductor resists changes you might *want* to be fast.** The same stiffness that smooths a
  converter's current also limits how quickly the circuit can respond to a load step.
- **Real inductors are not ideal.** This document assumes a pure `L` in henries. Real parts also
  have winding resistance (I²R loss), a saturation current above which `L` collapses, and core
  losses that rise with frequency — which is why a real design never just plugs into the ideal
  formula and stops.
- **The kick is always lurking.** Any time an inductor's current path can be interrupted, you must
  provide a freewheeling path or pay for it in arcs and dead transistors (§6).

## 8 Sources and cross-links

- **Mirror law:** [../capacitor/capacitor.md](../capacitor/capacitor.md) — the capacitor obeys
  `I_C = C·dV/dt`, the same equation with `V ↔ I` and `L ↔ C` swapped.
- **Where the ramp is used:**
  [../../dc-dc-converters/buck/buck.md](../../dc-dc-converters/buck/buck.md) and
  [../../dc-dc-converters/boost/boost.md](../../dc-dc-converters/boost/boost.md) — each switching
  interval is one constant-`V_L` ramp from §3.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
