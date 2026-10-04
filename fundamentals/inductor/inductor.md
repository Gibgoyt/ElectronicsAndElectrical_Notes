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
nothing separating anything. What makes it useful is one fact about currents: **a current makes a
magnetic field**. A wire carrying a current wraps a circular magnetic field ![B](inductor.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> (measured in
tesla, T) around itself — point your right thumb along the current and your fingers curl the way
the field goes.

**Ampère's law** makes that quantitative. Pick any closed loop ![C](inductor.assets/eq-inline/32096c2e0e.svg)<!--m:C--> in space and walk once around it,
adding up the component of ![B](inductor.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> along your path at each step ![d l](inductor.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}-->. The total equals a
constant times the current ![I_enc](inductor.assets/eq-inline/7406b82902.svg)<!--m:I_{enc}--> that pierces the loop:

![the closed line integral of B dot dl around C equals mu_0 times I enclosed](inductor.assets/eq-ampere.svg)

The constant ![mu_0 approx 4 pi times 10^-7 H/m](inductor.assets/eq-inline/b848ce8ec5.svg)<!--m:\mu_0 \approx 4\pi\times10^{-7}\ \mathrm{H/m}--> is the *permeability of free space*. The law is true for
every loop, but it is only *useful* when symmetry tells you ![B](inductor.assets/eq-inline/ae4f281df5.svg)<!--m:B--> is constant along a well-chosen loop.
The coil is such a case. Wind ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns evenly along a length ![l](inductor.assets/eq-inline/07c342be6e.svg)<!--m:l--> and the fields of neighbouring
turns cancel between the wires and add inside, so (for a long coil) the field inside is strong,
uniform and along the axis, while the field outside is weak and spread out. Take a rectangular loop
with one side of length ![l](inductor.assets/eq-inline/07c342be6e.svg)<!--m:l--> running inside the coil and the opposite side far outside. Only the
inside side contributes (![B](inductor.assets/eq-inline/ae4f281df5.svg)<!--m:B--> times ![l](inductor.assets/eq-inline/07c342be6e.svg)<!--m:l-->): the two short sides cross the field at right angles and
the outside side sits where ![B](inductor.assets/eq-inline/ae4f281df5.svg)<!--m:B--> is negligible. The loop is pierced by all ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns, each carrying
![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I-->, so ![I_enc = NI](inductor.assets/eq-inline/a55b8fe0a1.svg)<!--m:I_{enc} = NI-->:

![B l equals mu_0 N I, so B equals mu_0 N I over l](inductor.assets/eq-solenoid.svg)

Fill the coil with a magnetic core and the material's own atomic magnets line up with the field and
add to it; this multiplies the result by the core's *relative permeability* ![mu_r](inductor.assets/eq-inline/de4a3aca4d.svg)<!--m:\mu_r--> (1 for air,
thousands for ferrite), giving ![B = mu N I/l](inductor.assets/eq-inline/a6e2aa7ad3.svg)<!--m:B = \mu N I/l--> with ![mu = mu_0 mu_r](inductor.assets/eq-inline/62cb256de9.svg)<!--m:\mu = \mu_0\mu_r-->. So coiling the wire turns
the thin field of one wire into one strong, concentrated field through the core, and that field is
proportional to the current. (The full treatment, with the straight-wire case and worked numbers,
is in [../electromagnetism/electromagnetism.md §4–§5](../electromagnetism/electromagnetism.md#4-where-b-comes-from--ampere-and-biot-savart).)

That magnetic field is where an inductor keeps its energy. Its size,
![E = 1 over 2 LI^2](inductor.assets/eq-inline/baa846ef13.svg)<!--m:E = \tfrac{1}{2}LI^2-->, needs the inductance ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->, which is defined in §2 — the energy formula is
derived at the end of §3. There is no charge sitting anywhere waiting to be released — the energy
lives entirely in the field created by the moving charge (the current). This is the first thing to get straight,
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

**Magnetic flux — how much field threads the coil.** A magnetic field ![B](inductor.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> (measured in tesla, T)
fills space, but a coil only cares about how much of it passes *through* each of its turns. That
amount is the **magnetic flux** ![Phi_B](inductor.assets/eq-inline/df96567662.svg)<!--m:\Phi_B-->: add up the component of ![B](inductor.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> perpendicular to a surface,
over the whole area of that surface. For a uniform field crossing a flat area ![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> at an angle
![theta](inductor.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> to the surface's normal, the integral collapses to a product, and inside a coil the field
runs straight along the axis, perpendicular to each turn (![theta = 0](inductor.assets/eq-inline/5e8b7ec255.svg)<!--m:\theta = 0-->), so it is simply ![B](inductor.assets/eq-inline/ae4f281df5.svg)<!--m:B--> times
the cross-sectional area:

![Phi_B equals the surface integral of B dot dA; for uniform B, Phi_B equals B A cos theta; with theta equal to zero, Phi_B equals B A](inductor.assets/eq-flux-def.svg)

The unit of flux is the **weber** (Wb). Note the last form — a weber is a volt-second, which is the
first hint that flux and voltage are tied together through time:

![one weber equals one tesla square metre equals one volt second](inductor.assets/eq-weber.svg)

A coil of ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns is threaded by that same flux ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> times over, once per turn. The total — the flux
counted once for every turn it passes through — is the **flux linkage** ![lambda](inductor.assets/eq-inline/b3931f1ce2.svg)<!--m:\lambda-->:

![lambda equals N Phi_B, flux linkage in weber-turns](inductor.assets/eq-linkage.svg)

**Faraday's law of induction.** Faraday's discovery is that a magnetic flux produces a voltage in a
coil **only while the flux is changing**. A steady flux, however large, produces nothing. A changing
flux produces an EMF proportional to how fast it changes:

![EMF equals minus N times d Phi_B by dt](inductor.assets/eq-faraday.svg)

- **![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}-->** — the **induced EMF** ("electromotive force"), in volts. Despite the name it is not a
  force: it is the work done per coulomb on charge carried once along the wire. The changing flux
  sets up an electric field that *circulates* around it and pushes the free electrons along the
  wire, exactly as a battery's chemistry pushes them. So ![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> is a *source* of voltage that
  lives inside the coil, and it exists whether or not a current is flowing — with the terminals
  open you can measure it directly as a voltage across them. It is reckoned positive when it pushes
  charge in the same direction as the coil's reference current ![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I--> (the direction that makes
  positive ![Phi_B](inductor.assets/eq-inline/df96567662.svg)<!--m:\Phi_B--> by the right-hand rule).
- **![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N-->** — the number of turns. Each turn is one loop the flux threads, and each contributes an EMF
  of ![-d Phi_B/dt](inductor.assets/eq-inline/01e5051e39.svg)<!--m:-d\Phi_B/dt-->. The turns are connected in series, so their EMFs add — that is the only reason
  ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> is there.
- **![Phi_B](inductor.assets/eq-inline/df96567662.svg)<!--m:\Phi_B-->** — the magnetic flux through one turn, in webers, as defined above.
- **![d Phi_B/dt](inductor.assets/eq-inline/3dab0b0f34.svg)<!--m:d\Phi_B/dt-->** — its rate of change, in webers per second. Since ![1 Wb = 1 V times s](inductor.assets/eq-inline/b54cec509a.svg)<!--m:1\ \mathrm{Wb} = 1\ \mathrm{V\cdot s}-->, a weber
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

**Step 1 — the current makes a flux proportional to itself.** As §1 showed from Ampère's law, a long
coil (a solenoid) of ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns wound along a length ![l](inductor.assets/eq-inline/07c342be6e.svg)<!--m:l-->, carrying current ![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I-->, has a uniform field
inside:

![B equals mu N I over l, where mu equals mu_0 mu_r](inductor.assets/eq-step-b.svg)

Here ![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--m:\mu--> is the permeability of whatever fills the coil — ![mu_0 approx 4 pi times 10^-7 H/m](inductor.assets/eq-inline/b848ce8ec5.svg)<!--m:\mu_0 \approx 4\pi\times10^{-7}\ \mathrm{H/m}--> for air,
times the relative permeability ![mu_r](inductor.assets/eq-inline/de4a3aca4d.svg)<!--m:\mu_r--> (thousands, for a ferrite core).

**Step 2 — so the flux is proportional to the current.** Multiply by the cross-sectional area ![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--m:A-->:

![Phi_B equals B A equals mu N A over l times I, so Phi_B is proportional to I](inductor.assets/eq-step-flux.svg)

Every factor in front of ![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I--> — ![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--m:\mu-->, ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N-->, ![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--m:A-->, ![l](inductor.assets/eq-inline/07c342be6e.svg)<!--m:l--> — is fixed by how the coil is built. Double the
current and you double the flux; that proportionality is the whole basis of inductance.

**Step 3 — name the constant: that is the inductance.** Multiply both sides by ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> to get the flux
linkage. It is a constant times ![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I-->, and that constant is what we *define* as the inductance ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->:

![N Phi_B equals mu N squared A over l times I, defined as L I; so L equals N Phi_B over I equals mu N squared A over l](inductor.assets/eq-step-define-l.svg)

The current cancels out of ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->: it depends only on the geometry (![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N-->, ![A](inductor.assets/eq-inline/6dcd4ce23d.svg)<!--m:A-->, ![l](inductor.assets/eq-inline/07c342be6e.svg)<!--m:l-->) and the core
material (![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--m:\mu-->). This is what "a constant fixed by geometry alone" means in §3. The definition
![N Phi_B = LI](inductor.assets/eq-inline/b0ac725a5c.svg)<!--m:N\Phi_B = LI--> is general — a toroid, a gapped ferrite core or a single loop each has its own formula
for ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->, but every one of them satisfies ![N Phi_B = LI](inductor.assets/eq-inline/b0ac725a5c.svg)<!--m:N\Phi_B = LI-->; the solenoid is just the case where the
formula is easiest to see. It also gives the henry a second meaning: ![1 H = 1 Wb/A](inductor.assets/eq-inline/8e7b87e4e2.svg)<!--m:1\ \mathrm{H} = 1\ \mathrm{Wb/A}-->, one weber-turn
of linkage per ampere.

**Step 4 — substitute into Faraday's law.** Now let the current change with time. Start from
Faraday, move the constant ![N](inductor.assets/eq-inline/b51a60734d.svg)<!--m:N--> inside the derivative, replace ![N Phi_B](inductor.assets/eq-inline/406d115654.svg)<!--m:N\Phi_B--> by ![LI](inductor.assets/eq-inline/ba32a77087.svg)<!--m:LI--> (step 3), and move the
constant ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L--> back out:

![EMF equals minus N d Phi_B by dt, equals minus d of N Phi_B by dt, equals minus d of L I by dt, equals minus L dI by dt](inductor.assets/eq-step-substitute.svg)

The last move — pulling ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L--> out of the derivative — is only allowed because ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L--> is a constant. That
is a real assumption, and it is worth stating plainly: **the geometry is fixed** (the coil is not
being stretched or its core pulled out) and **the core is not saturated** (so ![mu](inductor.assets/eq-inline/3a4e56595d.svg)<!--m:\mu-->, and with it ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->,
does not change with the current; see §8). Under those conditions ![N Phi_B = LI](inductor.assets/eq-inline/b0ac725a5c.svg)<!--m:N\Phi_B = LI--> holds at *every
instant*, and two quantities equal at every instant have equal rates of change.

Read the result with Lenz in mind: when ![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I--> is rising (![dI/dt > 0](inductor.assets/eq-inline/3373e6b57b.svg)<!--m:dI/dt > 0-->), ![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> is **negative** —
the induced EMF pushes *against* the current. That is the back-EMF, now with a size: exactly
![L dI/dt](inductor.assets/eq-inline/a254d5e686.svg)<!--m:L\,dI/dt--> volts.

**Step 5 — from the EMF to the terminal voltage: where the minus sign goes.** This is the step
people get wrong, so slowly. ![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> and ![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L--> are **not the same quantity measured twice**; they
are measured in *opposite senses*:

- ![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> is a **push** (a potential *rise*) acting along the direction of the current. Think of
  a battery: going through it from its ![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--m:---> terminal to its ![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--m:+--> terminal, the potential rises by its
  EMF. An EMF ![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> acting from terminal A to terminal B (the current's direction) therefore
  makes B higher than A by ![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}-->:  ![V_B - V_A = E](inductor.assets/eq-inline/d67f1c50dc.svg)<!--m:V_B - V_A = \mathcal{E}-->.
- ![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L-->, in the **passive sign convention** used for every component in this tree (resistor
  included), is the voltage **drop** in the direction of the current — label the terminal where the
  current enters (A) as ![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--m:+-->, and ![V_L = V_A - V_B](inductor.assets/eq-inline/df4835dee2.svg)<!--m:V_L = V_A - V_B-->.

A rise of ![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> from A to B is the same thing as a drop of ![- E](inductor.assets/eq-inline/96dd850526.svg)<!--m:-\mathcal{E}--> from A to B. So:

![V_L equals minus EMF equals minus of minus L dI_L by dt, so V_L equals L dI_L by dt](inductor.assets/eq-step-sign.svg)

The minus sign has not been thrown away or "dropped for convenience": it has been *used up*
converting a rise into a drop. Both equations describe the same physics. When the current rises,
![E < 0](inductor.assets/eq-inline/39422ffb87.svg)<!--m:\mathcal{E} < 0--> (the coil pushes back, from B towards A), and equivalently ![V_L > 0](inductor.assets/eq-inline/665a90c162.svg)<!--m:V_L > 0--> (the entry terminal A
sits higher, so the coil *drops* voltage like a resistor, absorbing energy into its field).

**Check it with Kirchhoff.** Kirchhoff's voltage law (KVL) says that **the voltages around any
closed loop of a circuit add up to zero**. The reason is energy conservation: a volt is a joule per
coulomb, so the voltage between two points is the energy a coulomb of charge gains or loses going
between them. A charge carried once around a closed loop ends where it started, at the same
potential, so the gains (EMFs that push it along) must exactly cancel the losses (drops). Now connect an ideal voltage source ![V_s](inductor.assets/eq-inline/2fd8ef7403.svg)<!--m:V_s--> straight across an ideal coil, ![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--m:+-->
to terminal A. Walk once around the loop: the source's EMF ![V_s](inductor.assets/eq-inline/2fd8ef7403.svg)<!--m:V_s--> plus the coil's induced EMF
![E](inductor.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> must add to zero (there is no resistance to drop anything):

![V_s plus EMF equals zero, so V_s minus L dI_L by dt equals zero, so dI_L by dt equals V_s over L](inductor.assets/eq-step-kvl.svg)

The current rises at exactly the rate where the back-EMF cancels the applied voltage — no faster, no
slower — and ![V_L = V_s](inductor.assets/eq-inline/1003dfff71.svg)<!--m:V_L = V_s-->, as the plus-sign law says. A positive applied voltage gives a rising
current; nothing is backwards. (The same sign argument from the field-theory side, and the
integral form of Faraday's law, are in
[../electromagnetism/electromagnetism.md §8–§9](../electromagnetism/electromagnetism.md#8-lenzs-law--the-sign-and-back-emf).)

## 3 The defining law

That is the result of the derivation just completed in §2: Faraday's law of induction, applied to
a coil whose flux is made by its own current, makes the voltage across an inductor proportional to
the rate of change of the current through it:

![V_L equals L times dI_L by dt](inductor.assets/eq-law.svg)

- **![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L-->** — the voltage across the inductor's two terminals ("poles"), in volts (V).
- **![I_L](inductor.assets/eq-inline/aab68a829a.svg)<!--m:I_L-->** — the current flowing through it, in amps (A).
- **![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->** — the inductance, in henries (H). A constant fixed by geometry and core material alone —
  for a solenoid ![L = mu N^2 A/l](inductor.assets/eq-inline/e35c08d8b1.svg)<!--m:L = \mu N^2 A/l--> (§2, step 3): number of turns, core material, size. Think of it as *stiffness against sudden current changes* — a bigger ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->
  means the same voltage produces a slower change of current.

Rearranged for the rate of change:

![dI_L by dt equals V_L over L](inductor.assets/eq-law-rearranged.svg)

**The units of the henry.** A natural question: what are the units of ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L-->? Solve the defining law
for it — the ratio of volts to (amps per second) is *defined* as the henry:

![units of L reduce to volt second per ampere, one henry](inductor.assets/eq-units-henry.svg)

Check it forwards — multiply ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L--> (in V·s/A) by ![dI/dt](inductor.assets/eq-inline/867de4f022.svg)<!--m:dI/dt--> (in A/s) and the amps and seconds cancel,
leaving volts:

![V s over A times A over s equals V](inductor.assets/eq-units-check.svg)

> **Tip —** This is a good habit for the whole subject: whenever a formula looks unfamiliar,
> cancel the units. If they don't reduce to what the left-hand side claims, you've mis-remembered
> the formula.

**Power, and the energy stored.** First, why voltage times current is power. A volt is a joule per
coulomb (energy per unit charge) and an ampere is a coulomb per second (charge per unit time), so
their product is joules per second — energy per unit time, which is the watt:

![P equals V I; volts times amps equals joules per coulomb times coulombs per second equals joules per second equals watts](inductor.assets/eq-power.svg)

With the passive sign convention, a positive ![P = V_L I_L](inductor.assets/eq-inline/902a3391e7.svg)<!--m:P = V_L I_L--> means energy flowing *into* the component,
and a negative one means energy flowing *out*. For the inductor, substitute the defining law:

![p equals V_L I_L equals L I_L dI_L by dt](inductor.assets/eq-power-inductor.svg)

Energy is power added up over time. Start from zero current at ![t = 0](inductor.assets/eq-inline/fee440f68f.svg)<!--m:t = 0--> and let the current reach
![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I-->. Integrating ![p](inductor.assets/eq-inline/516b9783fc.svg)<!--m:p--> over time, the factor ![dI_L over dt dt](inductor.assets/eq-inline/facda424cf.svg)<!--m:\frac{dI_L}{dt}\,dt--> is just the small change of current
![di](inductor.assets/eq-inline/f502e82c25.svg)<!--m:di-->, so the time integral becomes an integral over the current itself, from 0 to ![I](inductor.assets/eq-inline/ca73ab6556.svg)<!--m:I-->:

![E equals the integral from 0 to t of p dt prime, equals the integral from 0 to I of L i di, equals one half L I squared](inductor.assets/eq-energy.svg)

That is the energy held in the magnetic field of §1. It depends only on the current *now*, not on
how the current got there, and it is handed back as the current falls — which is exactly the
"absorbing power" and "delivering power" of §5. (The same result from the field side, as an energy
density in the core, is in
[../electromagnetism/electromagnetism.md §10](../electromagnetism/electromagnetism.md#10-energy-stored-in-the-magnetic-field).)

## 4 From the law to the ramp — the integral, done slowly

Here is the step that is genuinely easy to lose. We have ![dI_L/dt = V_L/L](inductor.assets/eq-inline/64f54db2ce.svg)<!--m:dI_L/dt = V_L/L-->. Suppose ![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L--> is
**held constant** (this is exactly what happens inside a converter during each switching
interval). Integrate both sides with respect to time, from the moment we start the clock (![0](inductor.assets/eq-inline/b6589fc6ab.svg)<!--m:0-->) to
some later time ![t](inductor.assets/eq-inline/8efd86fb78.svg)<!--m:t-->:

![definite integral of dI_L over 0 to t gives I_L of t minus I_L of 0 equals V_L over L times t](inductor.assets/eq-integral.svg)

Two things worth pinning down, because both are common sticking points:

- **This is a *definite* integral, not an indefinite one.** There is no floating "+C" to worry
  about. The left side evaluates to ![I_L(t) - I_L(0)](inductor.assets/eq-inline/5249bbc77d.svg)<!--m:I_L(t) - I_L(0)--> by the Fundamental Theorem of Calculus —
  full stop. Nobody is secretly assuming a constant of integration is zero.
- **![I_L(0)](inductor.assets/eq-inline/4da7c0360b.svg)<!--m:I_L(0)--> is the initial condition** — whatever current was already flowing when you started
  the clock. If you start from zero current it simplifies to ![I_L(t) = (V_L/L) times t](inductor.assets/eq-inline/b966658ffb.svg)<!--m:I_L(t) = (V_L/L) \cdot t-->, but that is a
  *choice of scenario*, not something baked into the maths. Inside a converter in steady state,
  ![I_L(0)](inductor.assets/eq-inline/4da7c0360b.svg)<!--m:I_L(0)--> is genuinely non-zero. The full result is:

![I_L of t equals I_L of 0 plus V_L over L times t](inductor.assets/eq-ramp-result.svg)

With ![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L--> constant, ![I_L](inductor.assets/eq-inline/aab68a829a.svg)<!--m:I_L--> is a **straight line** in time. Its slope is ![V_L/L](inductor.assets/eq-inline/12ef7ca3f7.svg)<!--m:V_L/L--> and never changes
— so there is no curve, just a ramp.

![Constant voltage across an inductor produces a linear current ramp](inductor.assets/fig-01.svg)

_The flat cause (![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L--> on top) sets the slope of the straight-line effect (![I_L](inductor.assets/eq-inline/aab68a829a.svg)<!--m:I_L--> below): double the
voltage and the ramp gets twice as steep. This is ![dI_L/dt = V_L/L](inductor.assets/eq-inline/64f54db2ce.svg)<!--m:dI_L/dt = V_L/L--> made visible._

> **Watch out —** If ![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L--> is **not** constant — say it ramps, ![V_L = k times t](inductor.assets/eq-inline/f284d45471.svg)<!--m:V_L = k \cdot t--> — then
> ![dI_L/dt = k times t/L](inductor.assets/eq-inline/c30d8cdcdd.svg)<!--m:dI_L/dt = k \cdot t/L-->, and integrating gives ![I_L proportional to t^2](inductor.assets/eq-inline/19d640e093.svg)<!--m:I_L \propto t^{2}-->, a curve. A straight-line current requires a
> *flat* voltage. And constant current (![dI/dt = 0](inductor.assets/eq-inline/8ec73c5ed0.svg)<!--m:dI/dt = 0-->) requires ![V_L = 0](inductor.assets/eq-inline/23267682eb.svg)<!--m:V_L = 0--> — zero volts across the
> inductor, not a steady voltage. Getting this backwards is the most common inductor mistake.

## 5 Polarity, Lenz, and the sign flip

Which terminal is positive? Use the passive-component convention (the same one you use for a
resistor): current flows from the ![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--m:+--> terminal to the ![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--m:---> terminal *inside* the component.

- **While the current is increasing** (![dI/dt > 0](inductor.assets/eq-inline/3373e6b57b.svg)<!--m:dI/dt > 0-->, so ![V_L > 0](inductor.assets/eq-inline/665a90c162.svg)<!--m:V_L > 0-->): the entry terminal is ![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--m:+--> and
  the exit is ![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--m:---> — exactly like a resistor. The inductor is *absorbing* power
  (![P = V_L I_L > 0](inductor.assets/eq-inline/5ab94fe49e.svg)<!--m:P = V_L I_L > 0-->, §3), and by Lenz's law (§2)
  it generates a back-EMF that opposes the increase.
- **While the current is decreasing** (![dI/dt < 0](inductor.assets/eq-inline/e91f2e6ee4.svg)<!--m:dI/dt < 0-->, so ![V_L < 0](inductor.assets/eq-inline/948b9ac2a6.svg)<!--m:V_L < 0-->): the polarity **flips**. Now the
  exit terminal is ![+](inductor.assets/eq-inline/a979ef10cc.svg)<!--m:+--> and the entry is ![-](inductor.assets/eq-inline/3bc15c8aae.svg)<!--m:--->. The inductor is *delivering* power (![P < 0](inductor.assets/eq-inline/3f229ff951.svg)<!--m:P < 0-->: energy comes back out of the field), acting like a
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

Because voltage is proportional to ![dI/dt](inductor.assets/eq-inline/867de4f022.svg)<!--m:dI/dt-->, forcing the current to change *infinitely fast* would
demand an *infinite* voltage. So what happens if you simply open a switch in series with an
inductor, breaking its current path outright?

The inductor "tries" to keep its current flowing and, denied any path, generates a huge voltage
spike — in an idealised model, unboundedly large. This is the **inductive kick** (or "flyback"):
it is exactly how an old car ignition coil makes tens of thousands of volts from a 12 V battery,
and exactly why every relay or motor driven by a transistor needs a *flyback diode* across it.

> **Watch out —** In a buck or boost converter this is precisely what the **diode** (or the
> second MOSFET in a synchronous design) prevents. A *diode* is a one-way valve for current: it
> conducts in one direction and blocks the other (see
> [../../rectifiers/half-wave.md §1](../../rectifiers/half-wave.md#1-what-a-diode-is-and-its-iv-curve)).
> A *MOSFET* is a transistor used as a voltage-controlled switch: a voltage on its gate turns the
> path between its other two terminals fully on or fully off (see
> [../../dc-ac-inverters/h-bridge/h-bridge.md §5](../../dc-ac-inverters/h-bridge/h-bridge.md#5-the-mosfet-as-a-switch)). The instant the switch opens, the diode gives
> the inductor's current an alternate path within nanoseconds. The current itself never stops —
> only its *slope* changes abruptly, from ramping up to ramping down, making a sharp kink in the
> triangle wave. A kink is finite and fine; a *broken path* is what causes the dangerous spike.

## 8 What this costs you

- **An inductor resists changes you might *want* to be fast.** The same stiffness that smooths a
  converter's current also limits how quickly the circuit can respond to a load step.
- **Real inductors are not ideal.** This document assumes a pure ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L--> in henries. Real parts also
  have winding resistance (I²R loss), a saturation current above which ![L](inductor.assets/eq-inline/d160e0986a.svg)<!--m:L--> collapses, and core
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
  ![I_C = C times dV/dt](inductor.assets/eq-inline/56baca3b41.svg)<!--m:I_C = C \cdot dV/dt-->, the same equation with ![V I](inductor.assets/eq-inline/4ebcd6796a.svg)<!--m:V \leftrightarrow I--> and ![L C](inductor.assets/eq-inline/6cef383173.svg)<!--m:L \leftrightarrow C--> swapped.
- **Where the ramp is used:**
  [../../dc-dc-converters/buck/buck.md](../../dc-dc-converters/buck/buck.md) and
  [../../dc-dc-converters/boost/boost.md](../../dc-dc-converters/boost/boost.md) — each switching
  interval is one constant-![V_L](inductor.assets/eq-inline/136d4e3fb2.svg)<!--m:V_L--> ramp from §4.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
