# The inductor — voltage is proportional to the rate of change of current

An inductor is the simplest component whose behaviour is genuinely a differential equation.
Once you see *why* a constant voltage across it produces a straight-line current, every
switching-converter waveform in this tree becomes readable at a glance.

**Contents**

1. [What an inductor actually is](#1-what-an-inductor-actually-is)
2. [Faraday's law — where the defining law comes from](#2-faradays-law--where-the-defining-law-comes-from)
3. [The defining law](#3-the-defining-law)
4. [From the law to the ramp — the integral, done slowly](#4-from-the-law-to-the-ramp--the-integral-done-slowly)
5. [Polarity, Lenz, and the sign flip](#5-polarity-lenz-and-the-sign-flip)
6. [At the poles](#6-at-the-poles)
7. [The inductive kick, and why the diode is there](#7-the-inductive-kick-and-why-the-diode-is-there)
8. [What this costs you](#8-what-this-costs-you)
9. [Sources and cross-links](#9-sources-and-cross-links)

> **The thesis in one line**
>
> The voltage across an inductor is proportional to how fast its current is changing — Faraday's
> law of induction applied to the coil's own magnetic flux. Hold the voltage constant and the
> current is forced into a straight ramp:

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

> **Note —** An inductor does **not** have two plates and does **not** separate charge. If you
> are picturing charge building up on something, you are picturing a capacitor. An inductor stores
> energy in a *magnetic field made by current*, not in *separated static charge*. The two
> mechanisms are completely different — see [../capacitor/capacitor.md](../capacitor/capacitor.md).

## 2 Faraday's law — where the defining law comes from

The inductor law is not a separate fact about inductors that you have to take on trust. It is
Faraday's law of induction applied to a coil whose magnetic flux is made by its *own* current. This
section builds that law from nothing — flux, Faraday, Lenz — and then derives the inductor law from
it in five short steps. (The full electromagnetic treatment, with worked numbers, is in
[../electromagnetism/electromagnetism.md](../electromagnetism/electromagnetism.md) §3–§9; this is the
part the inductor needs.)

**Magnetic flux — how much field threads the coil.** A magnetic field <!--m:\vec{B}-->![B](inductor.assets/eq-inline/84dd0d2d09.svg)<!--/m--> (measured in tesla, T)
fills space, but a coil only cares about how much of it passes *through* each of its turns. That
amount is the **magnetic flux** <!--m:\Phi_B-->![Phi_B](inductor.assets/eq-inline/df96567662.svg)<!--/m-->: add up the component of <!--m:\vec{B}-->![B](inductor.assets/eq-inline/84dd0d2d09.svg)<!--/m--> perpendicular to a surface,
over the whole area of that surface. For a uniform field crossing a flat area <!--m:A-->![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--/m--> at an angle
<!--m:\theta-->![theta](inductor.assets/eq-inline/cb005d76f9.svg)<!--/m--> to the surface's normal, the integral collapses to a product, and inside a coil the field
runs straight along the axis, perpendicular to each turn (<!--m:\theta = 0-->![theta = 0](inductor.assets/eq-inline/5e8b7ec255.svg)<!--/m-->), so it is simply <!--m:B-->![B](inductor.assets/eq-inline/ae4f281df5.svg)<!--/m--> times
the cross-sectional area:

![Phi_B equals the surface integral of B dot dA; for uniform B, Phi_B equals B A cos theta; with theta equal to zero, Phi_B equals B A](inductor.assets/eq-flux-def.svg)

The unit of flux is the **weber** (Wb). Note the last form — a weber is a volt-second, which is the
first hint that flux and voltage are tied together through time:

![one weber equals one tesla square metre equals one volt second](inductor.assets/eq-weber.svg)

A coil of <!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m--> turns is threaded by that same flux <!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m--> times over, once per turn. The total — the flux
counted once for every turn it passes through — is the **flux linkage** <!--m:\lambda-->![lambda](inductor.assets/eq-inline/b3931f1ce2.svg)<!--/m-->:

![lambda equals N Phi_B, flux linkage in weber-turns](inductor.assets/eq-linkage.svg)

**Faraday's law of induction.** Faraday's discovery is that a magnetic flux produces a voltage in a
coil **only while the flux is changing**. A steady flux, however large, produces nothing. A changing
flux produces an EMF proportional to how fast it changes:

![EMF equals minus N times d Phi_B by dt](inductor.assets/eq-faraday.svg)

- **<!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m-->** — the **induced EMF** ("electromotive force"), in volts. Despite the name it is not a
  force: it is the work done per coulomb on charge carried once along the wire. The changing flux
  sets up an electric field that *circulates* around it and pushes the free electrons along the
  wire, exactly as a battery's chemistry pushes them. So <!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m--> is a *source* of voltage that
  lives inside the coil, and it exists whether or not a current is flowing — with the terminals
  open you can measure it directly as a voltage across them. It is reckoned positive when it pushes
  charge in the same direction as the coil's reference current <!--m:I-->![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--/m--> (the direction that makes
  positive <!--m:\Phi_B-->![Phi_B](inductor.assets/eq-inline/df96567662.svg)<!--/m--> by the right-hand rule).
- **<!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m-->** — the number of turns. Each turn is one loop the flux threads, and each contributes an EMF
  of <!--m:-d\Phi_B/dt-->![-d Phi_B/dt](inductor.assets/eq-inline/01e5051e39.svg)<!--/m-->. The turns are connected in series, so their EMFs add — that is the only reason
  <!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m--> is there.
- **<!--m:\Phi_B-->![Phi_B](inductor.assets/eq-inline/df96567662.svg)<!--/m-->** — the magnetic flux through one turn, in webers, as defined above.
- **<!--m:d\Phi_B/dt-->![d Phi_B/dt](inductor.assets/eq-inline/3dab0b0f34.svg)<!--/m-->** — its rate of change, in webers per second. Since <!--m:1\ \mathrm{Wb} = 1\ \mathrm{V\cdot s}-->![1 Wb = 1 V times s](inductor.assets/eq-inline/b54cec509a.svg)<!--/m-->, a weber
  per second *is* a volt, so the law needs no conversion constant.
- **the minus sign** — Lenz's law, next.

**Lenz's law — the minus sign.** The minus sign says the induced EMF always pushes in the
direction that **opposes the change in flux that produced it**. If the flux through the coil is
rising, the EMF drives charge the way that would make an opposing flux and hold the rise back; if
the flux is falling, the EMF reverses and tries to keep it up. This is not a convention you may
choose; it is energy conservation. If the induced EMF *aided* the change, a rising flux would
cause more current, which would cause more flux, without limit — energy from nothing.

Apply Lenz to a coil carrying its *own* current. When that current rises, its own flux rises, and
the EMF it induces in itself pushes *against* the current — a **back-EMF**. When the current falls,
the back-EMF flips and pushes *with* the current, trying to keep it going. Hold on to that picture;
the derivation below just puts numbers on it.

![A coil with rising current: the flux it makes rises, the induced back-EMF pushes against the current, and the terminal where current enters is positive](inductor.assets/fig-04.svg)

_Everything in this section is in one picture: the current makes the flux (step 1), the rising flux
induces an EMF that pushes back against the current (Faraday and Lenz), and that push shows up at the
terminals as a voltage drop in the direction of the current — positive where the current enters
(step 5)._

### Deriving the defining law, step by step

**Step 1 — the current makes a flux proportional to itself.** By Ampère's law (§1), current in the
wire makes the field. For a long coil (a solenoid) of <!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m--> turns wound along a length <!--m:l-->![l](inductor.assets/eq-inline/07c342be6e.svg)<!--/m-->, carrying
current <!--m:I-->![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--/m-->, the field inside is uniform and is
([../electromagnetism/electromagnetism.md §4–§5](../electromagnetism/electromagnetism.md#4-where-b-comes-from--ampere-and-biot-savart)
derives it):

![B equals mu N I over l, where mu equals mu_0 mu_r](inductor.assets/eq-step-b.svg)

Here <!--m:\mu-->![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--/m--> is the permeability of whatever fills the coil — <!--m:\mu_0 \approx 4\pi\times10^{-7}\ \mathrm{H/m}-->![mu_0 approx 4 pi times 10^-7 H/m](inductor.assets/eq-inline/b848ce8ec5.svg)<!--/m--> for air,
times the relative permeability <!--m:\mu_r-->![mu_r](inductor.assets/eq-inline/de4a3aca4d.svg)<!--/m--> (thousands, for a ferrite core).

**Step 2 — so the flux is proportional to the current.** Multiply by the cross-sectional area <!--m:A-->![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--/m-->:

![Phi_B equals B A equals mu N A over l times I, so Phi_B is proportional to I](inductor.assets/eq-step-flux.svg)

Every factor in front of <!--m:I-->![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--/m--> — <!--m:\mu-->![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--/m-->, <!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m-->, <!--m:A-->![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--/m-->, <!--m:l-->![l](inductor.assets/eq-inline/07c342be6e.svg)<!--/m--> — is fixed by how the coil is built. Double the
current and you double the flux; that proportionality is the whole basis of inductance.

**Step 3 — name the constant: that is the inductance.** Multiply both sides by <!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m--> to get the flux
linkage. It is a constant times <!--m:I-->![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--/m-->, and that constant is what we *define* as the inductance <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m-->:

![N Phi_B equals mu N squared A over l times I, defined as L I; so L equals N Phi_B over I equals mu N squared A over l](inductor.assets/eq-step-define-l.svg)

The current cancels out of <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m-->: it depends only on the geometry (<!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m-->, <!--m:A-->![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--/m-->, <!--m:l-->![l](inductor.assets/eq-inline/07c342be6e.svg)<!--/m-->) and the core
material (<!--m:\mu-->![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--/m-->). This is what "a constant fixed by geometry alone" means in §3. The definition
<!--m:N\Phi_B = LI-->![N Phi_B = LI](inductor.assets/eq-inline/b0ac725a5c.svg)<!--/m--> is general — a toroid, a gapped ferrite core or a single loop each has its own formula
for <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m-->, but every one of them satisfies <!--m:N\Phi_B = LI-->![N Phi_B = LI](inductor.assets/eq-inline/b0ac725a5c.svg)<!--/m-->; the solenoid is just the case where the
formula is easiest to see. It also gives the henry a second meaning: <!--m:1\ \mathrm{H} = 1\ \mathrm{Wb/A}-->![1 H = 1 Wb/A](inductor.assets/eq-inline/8e7b87e4e2.svg)<!--/m-->, one weber-turn
of linkage per ampere.

**Step 4 — substitute into Faraday's law.** Now let the current change with time. Start from
Faraday, move the constant <!--m:N-->![N](inductor.assets/eq-inline/b51a60734d.svg)<!--/m--> inside the derivative, replace <!--m:N\Phi_B-->![N Phi_B](inductor.assets/eq-inline/406d115654.svg)<!--/m--> by <!--m:LI-->![LI](inductor.assets/eq-inline/ba32a77087.svg)<!--/m--> (step 3), and move the
constant <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m--> back out:

![EMF equals minus N d Phi_B by dt, equals minus d of N Phi_B by dt, equals minus d of L I by dt, equals minus L dI by dt](inductor.assets/eq-step-substitute.svg)

The last move — pulling <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m--> out of the derivative — is only allowed because <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m--> is a constant. That
is a real assumption, and it is worth stating plainly: **the geometry is fixed** (the coil is not
being stretched or its core pulled out) and **the core is not saturated** (so <!--m:\mu-->![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--/m-->, and with it <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m-->,
does not change with the current; see §8). Under those conditions <!--m:N\Phi_B = LI-->![N Phi_B = LI](inductor.assets/eq-inline/b0ac725a5c.svg)<!--/m--> holds at *every
instant*, and two quantities equal at every instant have equal rates of change.

Read the result with Lenz in mind: when <!--m:I-->![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--/m--> is rising (<!--m:dI/dt > 0-->![dI/dt > 0](inductor.assets/eq-inline/3373e6b57b.svg)<!--/m-->), <!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m--> is **negative** —
the induced EMF pushes *against* the current. That is the back-EMF, now with a size: exactly
<!--m:L\,dI/dt-->![L dI/dt](inductor.assets/eq-inline/a254d5e686.svg)<!--/m--> volts.

**Step 5 — from the EMF to the terminal voltage: where the minus sign goes.** This is the step
people get wrong, so slowly. <!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m--> and <!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m--> are **not the same quantity measured twice**; they
are measured in *opposite senses*:

- <!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m--> is a **push** (a potential *rise*) acting along the direction of the current. Think of
  a battery: going through it from its <!--m:--->![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--/m--> terminal to its <!--m:+-->![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--/m--> terminal, the potential rises by its
  EMF. An EMF <!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m--> acting from terminal A to terminal B (the current's direction) therefore
  makes B higher than A by <!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m-->:  <!--m:V_B - V_A = \mathcal{E}-->![V_B - V_A = E](inductor.assets/eq-inline/d67f1c50dc.svg)<!--/m-->.
- <!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m-->, in the **passive sign convention** used for every component in this tree (resistor
  included), is the voltage **drop** in the direction of the current — label the terminal where the
  current enters (A) as <!--m:+-->![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--/m-->, and <!--m:V_L = V_A - V_B-->![V_L = V_A - V_B](inductor.assets/eq-inline/df4835dee2.svg)<!--/m-->.

A rise of <!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m--> from A to B is the same thing as a drop of <!--m:-\mathcal{E}-->![- E](inductor.assets/eq-inline/96dd850526.svg)<!--/m--> from A to B. So:

![V_L equals minus EMF equals minus of minus L dI_L by dt, so V_L equals L dI_L by dt](inductor.assets/eq-step-sign.svg)

The minus sign has not been thrown away or "dropped for convenience": it has been *used up*
converting a rise into a drop. Both equations describe the same physics. When the current rises,
<!--m:\mathcal{E} < 0-->![E < 0](inductor.assets/eq-inline/39422ffb87.svg)<!--/m--> (the coil pushes back, from B towards A), and equivalently <!--m:V_L > 0-->![V_L > 0](inductor.assets/eq-inline/665a90c162.svg)<!--/m--> (the entry terminal A
sits higher, so the coil *drops* voltage like a resistor, absorbing energy into its field).

**Check it with Kirchhoff.** Connect an ideal voltage source <!--m:V_s-->![V_s](inductor.assets/eq-inline/2fd8ef7403.svg)<!--/m--> straight across an ideal coil, <!--m:+-->![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--/m-->
to terminal A. Walk once around the loop: the source's EMF <!--m:V_s-->![V_s](inductor.assets/eq-inline/2fd8ef7403.svg)<!--/m--> plus the coil's induced EMF
<!--m:\mathcal{E}-->![E](inductor.assets/eq-inline/2ac770400e.svg)<!--/m--> must add to zero (there is no resistance to drop anything):

![V_s plus EMF equals zero, so V_s minus L dI_L by dt equals zero, so dI_L by dt equals V_s over L](inductor.assets/eq-step-kvl.svg)

The current rises at exactly the rate where the back-EMF cancels the applied voltage — no faster, no
slower — and <!--m:V_L = V_s-->![V_L = V_s](inductor.assets/eq-inline/1003dfff71.svg)<!--/m-->, as the plus-sign law says. A positive applied voltage gives a rising
current; nothing is backwards. (The same sign argument from the field-theory side, and the
integral form of Faraday's law, are in
[../electromagnetism/electromagnetism.md §8–§9](../electromagnetism/electromagnetism.md#8-lenzs-law--the-sign-and-back-emf).)

## 3 The defining law

That is the result of the derivation just completed in §2: Faraday's law of induction, applied to
a coil whose flux is made by its own current, makes the voltage across an inductor proportional to
the rate of change of the current through it:

![V_L equals L times dI_L by dt](inductor.assets/eq-law.svg)

- **<!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m-->** — the voltage across the inductor's two terminals ("poles"), in volts (V).
- **<!--m:I_L-->![I_L](inductor.assets/eq-inline/aab68a829a.svg)<!--/m-->** — the current flowing through it, in amps (A).
- **<!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m-->** — the inductance, in henries (H). A constant fixed by geometry and core material alone —
  for a solenoid <!--m:L = \mu N^2 A/l-->![L = mu N^2 A/l](inductor.assets/eq-inline/e35c08d8b1.svg)<!--/m--> (§2, step 3): number of turns, core material, size. Think of it as *stiffness against sudden current changes* — a bigger <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m-->
  means the same voltage produces a slower change of current.

Rearranged for the rate of change:

![dI_L by dt equals V_L over L](inductor.assets/eq-law-rearranged.svg)

**The units of the henry.** A natural question: what are the units of <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m-->? Solve the defining law
for it — the ratio of volts to (amps per second) is *defined* as the henry:

![units of L reduce to volt second per ampere, one henry](inductor.assets/eq-units-henry.svg)

Check it forwards — multiply <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m--> (in V·s/A) by <!--m:dI/dt-->![dI/dt](inductor.assets/eq-inline/867de4f022.svg)<!--/m--> (in A/s) and the amps and seconds cancel,
leaving volts:

![V s over A times A over s equals V](inductor.assets/eq-units-check.svg)

> **Tip —** This is a good habit for the whole subject: whenever a formula looks unfamiliar,
> cancel the units. If they don't reduce to what the left-hand side claims, you've mis-remembered
> the formula.

## 4 From the law to the ramp — the integral, done slowly

Here is the step that is genuinely easy to lose. We have <!--m:dI_L/dt = V_L/L-->![dI_L/dt = V_L/L](inductor.assets/eq-inline/64f54db2ce.svg)<!--/m-->. Suppose <!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m--> is
**held constant** (this is exactly what happens inside a converter during each switching
interval). Integrate both sides with respect to time, from the moment we start the clock (<!--m:0-->![0](inductor.assets/eq-inline/b6589fc6ab.svg)<!--/m-->) to
some later time <!--m:t-->![t](inductor.assets/eq-inline/8efd86fb78.svg)<!--/m-->:

![definite integral of dI_L over 0 to t gives I_L of t minus I_L of 0 equals V_L over L times t](inductor.assets/eq-integral.svg)

Two things worth pinning down, because both are common sticking points:

- **This is a *definite* integral, not an indefinite one.** There is no floating "+C" to worry
  about. The left side evaluates to <!--m:I_L(t) - I_L(0)-->![I_L(t) - I_L(0)](inductor.assets/eq-inline/5249bbc77d.svg)<!--/m--> by the Fundamental Theorem of Calculus —
  full stop. Nobody is secretly assuming a constant of integration is zero.
- **<!--m:I_L(0)-->![I_L(0)](inductor.assets/eq-inline/4da7c0360b.svg)<!--/m--> is the initial condition** — whatever current was already flowing when you started
  the clock. If you start from zero current it simplifies to <!--m:I_L(t) = (V_L/L) \cdot t-->![I_L(t) = (V_L/L) times t](inductor.assets/eq-inline/b966658ffb.svg)<!--/m-->, but that is a
  *choice of scenario*, not something baked into the maths. Inside a converter in steady state,
  <!--m:I_L(0)-->![I_L(0)](inductor.assets/eq-inline/4da7c0360b.svg)<!--/m--> is genuinely non-zero. The full result is:

![I_L of t equals I_L of 0 plus V_L over L times t](inductor.assets/eq-ramp-result.svg)

With <!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m--> constant, <!--m:I_L-->![I_L](inductor.assets/eq-inline/aab68a829a.svg)<!--/m--> is a **straight line** in time. Its slope is <!--m:V_L/L-->![V_L/L](inductor.assets/eq-inline/12ef7ca3f7.svg)<!--/m--> and never changes
— so there is no curve, just a ramp.

![Constant voltage across an inductor produces a linear current ramp](inductor.assets/fig-01.svg)

_The flat cause (<!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m--> on top) sets the slope of the straight-line effect (<!--m:I_L-->![I_L](inductor.assets/eq-inline/aab68a829a.svg)<!--/m--> below): double the
voltage and the ramp gets twice as steep. This is <!--m:dI_L/dt = V_L/L-->![dI_L/dt = V_L/L](inductor.assets/eq-inline/64f54db2ce.svg)<!--/m--> made visible._

> **Watch out —** If <!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m--> is **not** constant — say it ramps, <!--m:V_L = k \cdot t-->![V_L = k times t](inductor.assets/eq-inline/f284d45471.svg)<!--/m--> — then
> <!--m:dI_L/dt = k \cdot t/L-->![dI_L/dt = k times t/L](inductor.assets/eq-inline/c30d8cdcdd.svg)<!--/m-->, and integrating gives <!--m:I_L \propto t^{2}-->![I_L proportional to t^2](inductor.assets/eq-inline/19d640e093.svg)<!--/m-->, a curve. A straight-line current requires a
> *flat* voltage. And constant current (<!--m:dI/dt = 0-->![dI/dt = 0](inductor.assets/eq-inline/8ec73c5ed0.svg)<!--/m-->) requires <!--m:V_L = 0-->![V_L = 0](inductor.assets/eq-inline/23267682eb.svg)<!--/m--> — zero volts across the
> inductor, not a steady voltage. Getting this backwards is the most common inductor mistake.

## 5 Polarity, Lenz, and the sign flip

Which terminal is positive? Use the passive-component convention (the same one you use for a
resistor): current flows from the <!--m:+-->![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--/m--> terminal to the <!--m:--->![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--/m--> terminal *inside* the component.

- **While the current is increasing** (<!--m:dI/dt > 0-->![dI/dt > 0](inductor.assets/eq-inline/3373e6b57b.svg)<!--/m-->, so <!--m:V_L > 0-->![V_L > 0](inductor.assets/eq-inline/665a90c162.svg)<!--/m-->): the entry terminal is <!--m:+-->![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--/m--> and
  the exit is <!--m:--->![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--/m--> — exactly like a resistor. The inductor is *absorbing* power, and by Lenz's law (§2)
  it generates a back-EMF that opposes the increase.
- **While the current is decreasing** (<!--m:dI/dt < 0-->![dI/dt < 0](inductor.assets/eq-inline/e91f2e6ee4.svg)<!--/m-->, so <!--m:V_L < 0-->![V_L < 0](inductor.assets/eq-inline/948b9ac2a6.svg)<!--/m-->): the polarity **flips**. Now the
  exit terminal is <!--m:+-->![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--/m--> and the entry is <!--m:--->![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--/m-->. The inductor is *delivering* power, acting like a
  battery that pushes current forward using its stored magnetic energy.

![Inductor terminal polarity reverses when the current changes from rising to falling](inductor.assets/fig-03.svg)

_The inductor never commits to a fixed polarity — it takes whatever sign keeps the current
changing smoothly. That reversal, the exit terminal jumping above the entry, is literally how a
boost converter pushes its output above the input._

> **Tip —** Remember this reversal. It is not a curiosity — it is the mechanism of the
> [boost converter](../../dc-dc-converters/boost/boost.md). When the switch opens and the current
> starts to fall, the inductor's voltage flips and *adds on top of* the input.

## 6 At the poles

At the two terminals, current flows **straight through the coil the entire time** — nothing is
blocked. The "storage" is not a dammed-up path; it is the magnetic field surrounding the wire,
which grows and shrinks as the current changes.

![Current flows continuously through an inductor coil while its magnetic field pulses](inductor.assets/fig-02.svg)

_Charge travels all the way through, terminal A to terminal B, while the B-field pulses. Contrast
this with a capacitor, where no charge ever crosses the gap — see
[../capacitor/capacitor.md §5](../capacitor/capacitor.md#5-at-the-poles)._

## 7 The inductive kick, and why the diode is there

Because voltage is proportional to <!--m:dI/dt-->![dI/dt](inductor.assets/eq-inline/867de4f022.svg)<!--/m-->, forcing the current to change *infinitely fast* would
demand an *infinite* voltage. So what happens if you simply open a switch in series with an
inductor, breaking its current path outright?

The inductor "tries" to keep its current flowing and, denied any path, generates a huge voltage
spike — in an idealised model, unboundedly large. This is the **inductive kick** (or "flyback"):
it is exactly how an old car ignition coil makes tens of thousands of volts from a 12 V battery,
and exactly why every relay or motor driven by a transistor needs a *flyback diode* across it.

> **Watch out —** In a buck or boost converter this is precisely what the **diode** (or the
> second MOSFET in a synchronous design) prevents. The instant the switch opens, the diode gives
> the inductor's current an alternate path within nanoseconds. The current itself never stops —
> only its *slope* changes abruptly, from ramping up to ramping down, making a sharp kink in the
> triangle wave. A kink is finite and fine; a *broken path* is what causes the dangerous spike.

## 8 What this costs you

- **An inductor resists changes you might *want* to be fast.** The same stiffness that smooths a
  converter's current also limits how quickly the circuit can respond to a load step.
- **Real inductors are not ideal.** This document assumes a pure <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m--> in henries. Real parts also
  have winding resistance (I²R loss), a saturation current above which <!--m:L-->![L](inductor.assets/eq-inline/d160e0986a.svg)<!--/m--> collapses, and core
  losses that rise with frequency — which is why a real design never just plugs into the ideal
  formula and stops.
- **The kick is always lurking.** Any time an inductor's current path can be interrupted, you must
  provide a freewheeling path or pay for it in arcs and dead transistors (§7).

## 9 Sources and cross-links

- **Where the law comes from, in full:**
  [../electromagnetism/electromagnetism.md](../electromagnetism/electromagnetism.md) — flux and the
  weber (§3), Ampère and the solenoid field (§4–§5), Faraday (§6), Lenz and back-EMF (§8), and
  self-inductance (§9), with worked numbers; §2 here is the condensed path through them.
- **Mirror law:** [../capacitor/capacitor.md](../capacitor/capacitor.md) — the capacitor obeys
  <!--m:I_C = C \cdot dV/dt-->![I_C = C times dV/dt](inductor.assets/eq-inline/56baca3b41.svg)<!--/m-->, the same equation with <!--m:V \leftrightarrow I-->![V I](inductor.assets/eq-inline/4ebcd6796a.svg)<!--/m--> and <!--m:L \leftrightarrow C-->![L C](inductor.assets/eq-inline/6cef383173.svg)<!--/m--> swapped.
- **Where the ramp is used:**
  [../../dc-dc-converters/buck/buck.md](../../dc-dc-converters/buck/buck.md) and
  [../../dc-dc-converters/boost/boost.md](../../dc-dc-converters/boost/boost.md) — each switching
  interval is one constant-<!--m:V_L-->![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--/m--> ramp from §4.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
