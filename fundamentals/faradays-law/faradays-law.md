# Faraday's law of induction — a changing flux drives charge around a loop

A magnet sitting still next to a coil does nothing. Move it, and a current flows for exactly as
long as it moves. That observation, made by Michael Faraday in 1831, is the reason generators,
transformers, inductors, induction cooktops and magnetic brakes work at all. This document builds
the law from nothing: what magnetic flux is and how it is measured in webers, what flux linkage
(written ![lambda](faradays-law.assets/eq-inline/b3931f1ce2.svg)<!--m:\lambda-->: the flux counted once for every turn of a coil, in weber-turns) means, what an
EMF physically is, why the law carries a minus sign, the two very
different mechanisms hiding behind one equation, the field form that Maxwell wrote down, the cases
where the simple rule gives the wrong answer, and the machines that use it, with worked numbers
throughout.

**Contents**

1. [Where this sits, and what you need first](#1-where-this-sits-and-what-you-need-first)
2. [History — from a compass needle to relativity](#2-history--from-a-compass-needle-to-relativity)
3. [Magnetic flux — how much field threads a loop](#3-magnetic-flux--how-much-field-threads-a-loop)
4. [The weber, and flux linkage](#4-the-weber-and-flux-linkage)
5. [EMF — what actually pushes the charges round](#5-emf--what-actually-pushes-the-charges-round)
6. [Faraday's law — the flux rule](#6-faradays-law--the-flux-rule)
7. [Lenz's law — the minus sign is energy conservation](#7-lenzs-law--the-minus-sign-is-energy-conservation)
8. [Two ways to change the flux](#8-two-ways-to-change-the-flux)
9. [Motional EMF — derived from the magnetic force on the charges](#9-motional-emf--derived-from-the-magnetic-force-on-the-charges)
10. [Transformer EMF — the induced electric field](#10-transformer-emf--the-induced-electric-field)
11. [The Maxwell–Faraday equation, and what curl means](#11-the-maxwellfaraday-equation-and-what-curl-means)
12. [Deriving the flux rule from the field laws](#12-deriving-the-flux-rule-from-the-field-laws)
13. [When the flux rule needs care — the Faraday paradox](#13-when-the-flux-rule-needs-care--the-faraday-paradox)
14. [One phenomenon, two descriptions — the road to relativity](#14-one-phenomenon-two-descriptions--the-road-to-relativity)
15. [Applications](#15-applications)
16. [What this costs you](#16-what-this-costs-you)
17. [Every symbol in one place](#17-every-symbol-in-one-place)
18. [Sources and cross-links](#18-sources-and-cross-links)

> **The thesis in one line**
>
> The EMF around any loop equals minus the rate at which the magnetic flux through it changes. The
> size of the flux is irrelevant; only its rate of change counts, and the minus sign says the
> induced current always fights that change:

![EMF equals minus d Phi_B by dt; for N turns, EMF equals minus N d Phi_B by dt, which equals minus d lambda by dt](faradays-law.assets/eq-thesis.svg)

Here ![E](faradays-law.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> is the EMF round the loop, in volts (V); ![Phi_B](faradays-law.assets/eq-inline/df96567662.svg)<!--m:\Phi_B--> is the magnetic flux through
it, in webers (Wb); ![t](faradays-law.assets/eq-inline/8efd86fb78.svg)<!--m:t--> is time, in seconds (s); ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> is the number of turns of a coil; and
![lambda = N Phi_B](faradays-law.assets/eq-inline/9e19700585.svg)<!--m:\lambda = N\Phi_B--> is the flux linkage, in weber-turns. Each is built up from scratch below: the flux
![Phi_B](faradays-law.assets/eq-inline/df96567662.svg)<!--m:\Phi_B--> in §3, the weber and the flux linkage ![lambda](faradays-law.assets/eq-inline/b3931f1ce2.svg)<!--m:\lambda--> in §4, the EMF ![E](faradays-law.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> in §5. A summary
table of every symbol is in §17.

---

## 1 Where this sits, and what you need first

This is the third of three field laws in this tree.

- [../coulombs-law/](../coulombs-law/) — charges push on charges; the **electric field**
  ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> (force per unit charge, in ![N times C^-1 = V times m^-1](faradays-law.assets/eq-inline/a676095112.svg)<!--m:\mathrm{N\cdot C^{-1}} = \mathrm{V\cdot m^{-1}}-->); voltage as
  work per unit charge; and the fact, used in §5, that the field of charges sitting still is
  *conservative* (the work done carrying a charge around any closed path is zero).
- [../amperes-law/](../amperes-law/) — currents make the **magnetic field** ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> (in tesla, T);
  the field of a wire and of a solenoid; the right-hand rule.
- This document — a *changing* magnetic field, or a conductor moving through a magnetic field,
  pushes charge around a loop.

The overview that ties all three to the inductor and the transformer is
[../electromagnetism/electromagnetism.md](../electromagnetism/electromagnetism.md). The two
components that are Faraday's law in disguise are the [inductor](../inductor/) and the
[transformer](../transformer/); §15 derives both.

Two pieces of vocabulary are used from the start.

**The magnetic field and its force.** The magnetic field ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> is defined by the force it exerts
on a moving charge. A charge ![q](faradays-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> (in coulombs, C) moving with velocity ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> (in
![m times s^-1](faradays-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}-->) through an electric field ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> and a magnetic field ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> feels the
**Lorentz force** ![F](faradays-law.assets/eq-inline/b0682d270b.svg)<!--m:\vec{F}--> (in newtons, N):

![F equals q times the quantity E plus v cross B](faradays-law.assets/eq-lorentz.svg)

The cross product ![v times B](faradays-law.assets/eq-inline/7159cb59b2.svg)<!--m:\vec{v}\times\vec{B}--> is a vector at right angles to both ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> and ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->, with
size ![vB sin phi](faradays-law.assets/eq-inline/0e1cff3c9c.svg)<!--m:vB\sin\varphi-->, where ![phi](faradays-law.assets/eq-inline/44294dbd19.svg)<!--m:\varphi--> is the angle between them; its direction is where your right
thumb points when the fingers curl from ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> towards ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->. Two consequences matter all
through this document: a charge at rest (![v = 0](faradays-law.assets/eq-inline/213adcb8d2.svg)<!--m:\vec{v} = 0-->) feels **no** magnetic force at all, and the
magnetic force is always sideways to the motion. Reading the units off the force law gives the tesla:

![one tesla equals one newton per ampere per metre](faradays-law.assets/eq-tesla.svg)

(The ampere, A, is one coulomb per second: ![1 A = 1 C times s^-1](faradays-law.assets/eq-inline/1ac70fbaf0.svg)<!--m:1\ \mathrm{A} = 1\ \mathrm{C\cdot s^{-1}}-->.) For scale:
the Earth's field is about ![50 mu T](faradays-law.assets/eq-inline/2cd225cb1f.svg)<!--m:50\ \mu\mathrm{T}-->, a fridge magnet a few ![mT](faradays-law.assets/eq-inline/419b000f21.svg)<!--m:\mathrm{mT}-->, a neodymium
magnet's face about ![0.5 T](faradays-law.assets/eq-inline/790c25363e.svg)<!--m:0.5\ \mathrm{T}-->, and a mains transformer core about ![1.5 T](faradays-law.assets/eq-inline/8338974309.svg)<!--m:1.5\ \mathrm{T}-->.

**The colour code in the figures.** Amber is the magnetic field and the flux; blue is whatever is
*induced* (current, EMF, induced electric field); green is motion; red is force (and the north pole of
a magnet); purple is the area normal ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->.

## 2 History — from a compass needle to relativity

**1820 — current makes magnetism.** Hans Christian Ørsted noticed that a wire carrying current
deflects a nearby compass needle. If electricity could make magnetism, the obvious question was
whether magnetism could make electricity. For eleven years the answer seemed to be no: a magnet,
however strong, placed beside a closed loop of wire produced no current, and nobody thought to look
at the moment the magnet was *moved*.

**29 August 1831 — the iron ring.** Michael Faraday wound two separate coils of insulated wire on
opposite sides of a soft-iron ring — a primitive toroidal transformer. Coil A went to a battery
through a switch; coil B went to a galvanometer (a sensitive current meter: a coil-driven needle
that swings one way or the other with the direction of the current). On closing the switch the
needle kicked, then settled back to zero although the battery current kept flowing. On opening the
switch it kicked the other way. Faraday described the effect as a "wave of electricity" passing
through the iron. The steady current did nothing; only its *starting* and *stopping* did.

![Faraday’s iron ring: closing or opening the switch on coil A briefly kicks the galvanometer on coil B; a steady current does nothing](faradays-law.assets/fig-01.svg)

_The ring is the experiment in one picture: B has no electrical connection to A, and a constant
current in A does nothing to it. Only a changing current in A — and so a changing flux in the iron —
reaches B. Section 15 shows this is exactly a transformer._

**Autumn 1831 — the moving magnet.** Faraday then pushed a bar magnet into a coil connected to the
galvanometer and pulled it out again. The needle swung one way as the magnet went in and the other
way as it came out, and stayed at zero whenever the magnet was still, wherever it was. That is the
experiment animated in Figure 118 below, and it is the cleanest statement of the law: the effect
depends on *motion*, on change, never on how strong the field is.

**The disk.** Faraday also spun a copper disk between the poles of a magnet, with sliding contacts at
the axle and the rim. It gave a *steady* current for as long as it turned: the first dynamo, now
called the Faraday disk or homopolar generator. It turns out to be the hardest case to explain with
the simple flux rule, and §13 is devoted to it.

**1832 — Joseph Henry.** The American physicist Joseph Henry found the same effect independently,
including self-induction (a coil inducing a voltage in itself), but Faraday published first.

**1834 — Lenz.** Emil Lenz stated the rule for the *direction* of the induced current: it always
flows so as to oppose the change that produced it. Section 7 shows why it could not be otherwise.

**1845–1851 — the mathematics.** Faraday thought in "lines of force" and wrote no equations, and his
pictures were met with scepticism. Franz Ernst Neumann put the laws of induction into mathematical
form in 1845; Wilhelm Weber built them into his own theory of electrodynamics; and from 1851 Riccardo
Felici tested them experimentally (Felici's law relates the total charge driven round a circuit to
the total change of flux — the result derived in §6 below).

**1861–1865 — Maxwell.** James Clerk Maxwell gave Faraday's field picture a full mathematical
expression and folded it, with the work of Neumann, Weber and Felici, into his complete theory of
the electromagnetic field. In *On Physical Lines of Force* (1861) he already noticed that induction
by a moving wire and induction by a changing field were two different physical processes obeying one
formula.

**1890s — Heaviside.** Oliver Heaviside rewrote the time-varying part as a differential equation —
a statement about the field at each single point rather than around whole loops (the "curl" form
derived in §11), the form in every modern list of Maxwell's equations. That form describes only the changing-field half of induction; the moving-wire half comes from
the Lorentz force (§9 and §12).

**1905 — Einstein.** Albert Einstein opened *On the Electrodynamics of Moving Bodies* with exactly
this effect. Move the magnet past a still coil, or move the coil past a still magnet: the current is
the same, yet the theory of the day explained the two cases by different mechanisms. That asymmetry
was one of the threads that led to special relativity (§14).

## 3 Magnetic flux — how much field threads a loop

Faraday's experiments say the effect depends on how much magnetic field passes *through* the loop
and how fast that amount changes. That amount needs a precise definition.

**The area vector.** A flat loop encloses an area ![A](faradays-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> (in ![m^2](faradays-law.assets/eq-inline/fd25baa3e2.svg)<!--m:\mathrm{m^2}-->). To say which way it
faces, attach to it a **unit normal** ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->: an arrow of length 1, perpendicular to the flat
surface. The **area vector** combines the two:

![the area vector A equals the area A times the unit normal n-hat](faradays-law.assets/eq-area-vector.svg)

There are two possible normals, one on each side. Which one we choose is tied to a direction of
travel around the loop by the **right-hand rule**: curl the fingers of your right hand along the
chosen direction around the rim, and your thumb gives ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->. This pairing is the sign convention
for the whole of Faraday's law (§6), so fix it now.

**A uniform field through a flat loop.** Only the part of ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> that crosses the surface counts;
the part lying along the surface slides past without threading it. If ![theta](faradays-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> is the angle between
![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> and ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->, the crossing part is ![B cos theta](faradays-law.assets/eq-inline/f14d3176fa.svg)<!--m:B\cos\theta-->, and the amount of field through the loop
is that times the area. The vector **dot product** does exactly this bookkeeping:

![B dot A equals B times A times cos theta](faradays-law.assets/eq-dot-product.svg)

So for a uniform field and a flat loop the **magnetic flux** ![Phi_B](faradays-law.assets/eq-inline/df96567662.svg)<!--m:\Phi_B--> is:

![for a uniform field and a flat loop, Phi_B equals B dot A, which equals B A cos theta](faradays-law.assets/eq-flux-uniform.svg)

![A flat loop of area A tilted at angle theta in a uniform field B: only the component of B along the normal crosses it, so the flux is B A cos theta](faradays-law.assets/fig-02.svg)

_Panel (b) is the useful way to see it: the flux is the field times the loop's "shadow" — the area
it presents to the field. Face-on (![theta = 0](faradays-law.assets/eq-inline/5e8b7ec255.svg)<!--m:\theta = 0-->) it catches the most; edge-on (![theta = 90^ deg](faradays-law.assets/eq-inline/b8894476c4.svg)<!--m:\theta = 90^{\circ}-->) it
catches none, however large the loop is._

**Worked number.** A ![10 cm times 10 cm](faradays-law.assets/eq-inline/55ad0f8d1c.svg)<!--m:10\ \mathrm{cm}\times10\ \mathrm{cm}--> loop (![A = 0.01 m^2](faradays-law.assets/eq-inline/61f1d717ee.svg)<!--m:A = 0.01\ \mathrm{m^2}-->) in a uniform
![0.2 T](faradays-law.assets/eq-inline/2a9b341da6.svg)<!--m:0.2\ \mathrm{T}--> field, tilted so its normal is ![60^ deg](faradays-law.assets/eq-inline/4593add399.svg)<!--m:60^{\circ}--> from the field:

![Phi_B equals 0.2 tesla times 0.01 square metres times cos 60 degrees, which is 1 milliweber](faradays-law.assets/eq-flux-worked.svg)

**Any field, any surface.** Real fields are not uniform and real loops are not flat. The fix is the
usual one: chop the surface into patches small enough that each is flat and the field across it is
uniform. Patch ![i](faradays-law.assets/eq-inline/042dc4512f.svg)<!--m:i--> has a tiny area vector ![Delta A_i](faradays-law.assets/eq-inline/047bf308e2.svg)<!--m:\Delta\vec{A}_i--> and sees field ![B_i](faradays-law.assets/eq-inline/bee7af55d2.svg)<!--m:\vec{B}_i-->; add up:

![Phi_B is approximately the sum over patches of B_i dot delta A_i](faradays-law.assets/eq-flux-sum.svg)

Let the patches shrink to zero size and the sum becomes a **surface integral**. The surface is
called ![Sigma](faradays-law.assets/eq-inline/cb5615b3fc.svg)<!--m:\Sigma--> (capital sigma), its rim — the loop itself — is ![partial Sigma](faradays-law.assets/eq-inline/67dc6e421e.svg)<!--m:\partial\Sigma--> ("the boundary of
sigma"), and ![d A = n dA](faradays-law.assets/eq-inline/f7e054df1f.svg)<!--m:d\vec{A} = \hat{n}\,dA--> is an infinitesimal patch with its normal:

![Phi_B equals the surface integral over sigma of B dot dA](faradays-law.assets/eq-flux-integral.svg)

The double integral sign only means "add up over a two-dimensional surface". For a uniform field and
a flat loop it collapses back to ![BA cos theta](faradays-law.assets/eq-inline/5c498ec3fe.svg)<!--m:BA\cos\theta-->.

**Which surface?** A loop of wire does not come with a surface; you choose one with the loop as its
rim. A flat disk, a bowl, a soap-bubble bulge — infinitely many surfaces share the same rim. It does
not matter which, because of a basic fact about magnetic fields, **Gauss's law for magnetism**: there
are no magnetic charges, so field lines never start or end, and every line that enters a closed
surface ![S](faradays-law.assets/eq-inline/02aa629c8b.svg)<!--m:S--> also leaves it:

![the closed surface integral of B dot dA equals zero](faradays-law.assets/eq-gauss-b.svg)

Take two surfaces ![Sigma_1](faradays-law.assets/eq-inline/0249b117e3.svg)<!--m:\Sigma_1--> and ![Sigma_2](faradays-law.assets/eq-inline/28f91bb80f.svg)<!--m:\Sigma_2--> on the same rim. Together they form a closed surface, so
every line that crosses ![Sigma_1](faradays-law.assets/eq-inline/0249b117e3.svg)<!--m:\Sigma_1--> must cross ![Sigma_2](faradays-law.assets/eq-inline/28f91bb80f.svg)<!--m:\Sigma_2--> as well: the flux through them is the same.
The flux through a loop is a property of the loop alone.

![A surface sigma bounded by the loop del sigma with its normal n set by the right-hand rule; a bulging surface on the same rim carries the same flux; a coil of N turns is crossed N times by each flux line](faradays-law.assets/fig-03.svg)

_Panel (a) is the orientation rule (fingers along the rim, thumb along the normal); panel (b) is why
the choice of surface does not matter; panel (c) is why a coil multiplies the flux by its number of
turns, the subject of the next section._

## 4 The weber, and flux linkage

**The weber.** Flux is a field times an area, so its natural unit is the tesla square metre. It has
its own name, the **weber** (Wb), and it is worth seeing why a weber is also a volt-second, because
that identity is the whole of Faraday's law in units. Start from the tesla of §1:

![one weber equals one tesla square metre, equals one newton per ampere per metre times a square metre, equals one newton metre per ampere](faradays-law.assets/eq-weber-1.svg)

A newton-metre is a joule (J), the unit of work and energy:

![a newton metre is a joule, so one weber equals one joule per ampere](faradays-law.assets/eq-weber-2.svg)

A volt (V) is a joule per coulomb, so a joule is a volt-coulomb, and a coulomb is an ampere-second:

![a joule is a volt coulomb, which is a volt ampere second, so one weber equals one volt second](faradays-law.assets/eq-weber-3.svg)

So ![1 Wb = 1 T times m^2 = 1 V times s](faradays-law.assets/eq-inline/f9bdceebd1.svg)<!--m:1\ \mathrm{Wb} = 1\ \mathrm{T\cdot m^2} = 1\ \mathrm{V\cdot s}-->. A weber is a volt applied for a second.
Turned round, a flux changing at one weber per second is worth one volt, which §6 makes precise.
Practical fluxes are small: the ![1 mWb](faradays-law.assets/eq-inline/152f68e0d0.svg)<!--m:1\ \mathrm{mWb}--> of the example in §3, or the ![15 mu Wb](faradays-law.assets/eq-inline/e7862cc294.svg)<!--m:15\ \mu\mathrm{Wb}--> peak in
the ferrite transformer of [../electromagnetism/electromagnetism.md §6](../electromagnetism/electromagnetism.md#6-faradays-law--voltage-from-changing-flux-done-slowly).

**Flux linkage.** A coil is not one loop but ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> loops in series (each loop is a **turn**). The
surface bounded by the whole wire is a spiral ramp, like a car-park ramp, and a field line running
along the coil's axis pierces that ramp once per turn (Figure 117c). So the total flux "seen" by the
wire is the sum of the fluxes through each turn. That total is the **flux linkage**, written
![lambda](faradays-law.assets/eq-inline/b3931f1ce2.svg)<!--m:\lambda--> (lambda):

![in general lambda equals the sum over turns k of Phi_k](faradays-law.assets/eq-linkage-general.svg)

Here ![Phi_k](faradays-law.assets/eq-inline/ff7d8f92c4.svg)<!--m:\Phi_k--> is the flux through turn number ![k](faradays-law.assets/eq-inline/13fbd79c3d.svg)<!--m:k-->. In a tightly wound coil, or any winding on a
closed core, every turn encloses the same flux ![Phi_B](faradays-law.assets/eq-inline/df96567662.svg)<!--m:\Phi_B-->, and the sum is just ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> copies:

![lambda equals N times Phi_B, in weber-turns](faradays-law.assets/eq-linkage.svg)

The unit is the **weber-turn**. A "turn" is a pure count, so dimensionally a weber-turn is a weber;
the word "turns" is kept as a reminder that the flux has been counted once per turn. In the
electromagnetism overview the same quantity appears as ![v = d lambda/dt](faradays-law.assets/eq-inline/48fc99a213.svg)<!--m:v = d\lambda/dt-->; it is this ![lambda](faradays-law.assets/eq-inline/b3931f1ce2.svg)<!--m:\lambda-->.

**Worked number.** Wind 200 turns around the loop of §3 (same ![0.2 T](faradays-law.assets/eq-inline/2a9b341da6.svg)<!--m:0.2\ \mathrm{T}--> field, same ![60^ deg](faradays-law.assets/eq-inline/4593add399.svg)<!--m:60^{\circ}-->
tilt). Each turn holds ![1 mWb](faradays-law.assets/eq-inline/152f68e0d0.svg)<!--m:1\ \mathrm{mWb}-->, so:

![lambda equals 200 turns times 1 milliweber, which is 0.2 weber-turns](faradays-law.assets/eq-linkage-worked.svg)

> **Tip —** Keep three quantities apart. ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> (tesla) is what the field is at a point; ![Phi_B](faradays-law.assets/eq-inline/df96567662.svg)<!--m:\Phi_B-->
> (weber) is how much of it threads one loop; ![lambda = N Phi_B](faradays-law.assets/eq-inline/9e19700585.svg)<!--m:\lambda = N\Phi_B--> (weber-turns) is what the whole
> winding sees. They differ by an area and by a count, and most magnetics mistakes are dropping one
> of the two.

## 5 EMF — what actually pushes the charges round

Faraday's law gives an **EMF**, written ![E](faradays-law.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> (script E). The name, *electromotive force*, is a
historical accident: it is not a force, and it is measured in volts. What it is, physically, needs
care, because it is *not* the same thing as the voltage between two points of a circuit.

**Work per charge around a loop.** Take a closed loop ![C](faradays-law.assets/eq-inline/32096c2e0e.svg)<!--m:C--> (a ring of wire, or just a closed path in
space). Carry a charge ![q](faradays-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> once around it and add up the work ![W_ loop](faradays-law.assets/eq-inline/29ed80d057.svg)<!--m:W_{\text{loop}}--> that the forces
acting on it do along the way. The EMF is that work per unit charge:

![EMF equals W over q; one volt equals one joule per coulomb](faradays-law.assets/eq-work-per-charge.svg)

Work is force times distance, added up along the path. If the path is cut into tiny steps
![d l](faradays-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> (each a little arrow along the loop, in metres, pointing in the chosen direction of
travel), each step contributes ![F times d l](faradays-law.assets/eq-inline/39ce1dbe80.svg)<!--m:\vec{F}\cdot d\vec{l}--> — the component of the force along the step,
times its length. Adding those around the whole loop is a **closed line integral**, written
![loop integral_C](faradays-law.assets/eq-inline/1f02c1df6a.svg)<!--m:\oint_C-->. Dividing by ![q](faradays-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->, and using the Lorentz force of §1:

![EMF equals the closed line integral of F over q dot dl, which equals the closed line integral of E plus v cross B, dot dl](faradays-law.assets/eq-emf-def.svg)

That is the general definition used in the rest of this document. The direction of travel around
![C](faradays-law.assets/eq-inline/32096c2e0e.svg)<!--m:C-->, and hence the sign of ![E](faradays-law.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}-->, is tied to the surface normal ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}--> by the right-hand
rule of §3.

**Why a battery-free loop normally has zero EMF.** The electric field made by charges sitting still
(the subject of [../coulombs-law/](../coulombs-law/)) is **conservative**: the work it does on a
charge carried from one point to another does not depend on the route, so the work around any
closed route is zero:

![for the electrostatic field of charges at rest, the closed line integral of E dot dl equals zero](faradays-law.assets/eq-conservative.svg)

That is why a voltage between two points means something for static fields, and it is Kirchhoff's
voltage law in field form. A closed ring of copper with no battery and no changing magnetism
therefore has zero EMF and no current.

**So something non-conservative is needed.** To keep current flowing round a closed loop against its
resistance, some agent must do net work on each charge per lap. In a battery it is chemistry, over a
thin region at each electrode. In induction there are exactly two possibilities, read straight off
the integrand ![E + v times B](faradays-law.assets/eq-inline/c4ec63b81e.svg)<!--m:\vec{E} + \vec{v}\times\vec{B}-->:

- the **magnetic force** ![q v times B](faradays-law.assets/eq-inline/2365d1224f.svg)<!--m:q\vec{v}\times\vec{B}--> on charges carried along by a *moving* conductor (§9), or
- an **electric field that is not conservative** — one whose closed line integral is not zero.
  Charges at rest cannot make such a field; a changing magnetic field does (§10).

**EMF, current and terminal voltage.** If the loop is a wire of total resistance ![R](faradays-law.assets/eq-inline/06576556d1.svg)<!--m:R--> (in ohms,
![Omega](faradays-law.assets/eq-inline/4959627bdb.svg)<!--m:\Omega-->, where ![1 Omega = 1 V times A^-1](faradays-law.assets/eq-inline/bdee4018cb.svg)<!--m:1\ \Omega = 1\ \mathrm{V\cdot A^{-1}}-->), the EMF drives a current ![I](faradays-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> (in amperes)
round it:

![I equals EMF over R](faradays-law.assets/eq-ohm-loop.svg)

If the loop is cut open and a voltmeter is put across the gap, almost no current flows, charge
piles up at the two ends until its own conservative field exactly cancels the push in the wire, and
the meter reads the EMF. That is what "the induced voltage of a coil" means in circuit work: the
open-circuit EMF, which appears across the terminals.

## 6 Faraday's law — the flux rule

Now the law itself. For any closed loop, with ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}--> and the direction of travel paired by the
right-hand rule:

![EMF equals minus d Phi_B by dt](faradays-law.assets/eq-faraday.svg)

In words: the EMF around a loop equals minus the rate of change of the magnetic flux through it.
Here ![t](faradays-law.assets/eq-inline/8efd86fb78.svg)<!--m:t--> is time in seconds, and ![d Phi_B/dt](faradays-law.assets/eq-inline/3dab0b0f34.svg)<!--m:d\Phi_B/dt--> is the rate of change of the flux (its slope against
time), in webers per second. The units check with no constant needed, by §4:

![the units of d Phi by dt are webers per second, which are volt seconds per second, which are volts](faradays-law.assets/eq-faraday-units.svg)

![A bar magnet moves into and out of a coil; the galvanometer needle swings one way as it enters and the other way as it leaves, and the induced current reverses](faradays-law.assets/fig-04-anim.svg)

_The animation is Faraday's second experiment. The needle swings while the magnet moves and only
then; it swings one way going in and the other way coming out; and the current dots reverse with it.
The flux through the coil is largest when the magnet is deepest inside — exactly the moment the
needle passes through zero, because the flux has momentarily stopped changing._

**N turns.** For a coil the EMFs of the turns add, because the turns are in series. Do it one step at
a time. First, the total is the sum of the per-turn EMFs, each given by Faraday's law for its own
turn:

![the turns are in series, so the total EMF is the sum of the EMFs of each turn](faradays-law.assets/eq-n-turns-1.svg)

The derivative of a sum is the sum of the derivatives, so the derivative can be pulled outside, and
what is left inside is the flux linkage of §4:

![a sum of derivatives is the derivative of the sum, so EMF equals minus d by dt of the sum of Phi_k, which is minus d lambda by dt](faradays-law.assets/eq-n-turns-2.svg)

When every turn sees the same flux, ![lambda = N Phi_B](faradays-law.assets/eq-inline/9e19700585.svg)<!--m:\lambda = N\Phi_B-->, and ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> is a constant that comes out of the
derivative:

![if every turn sees the same flux Phi_B, EMF equals minus N d Phi_B by dt](faradays-law.assets/eq-n-turns-3.svg)

That is the multi-turn form in the thesis. ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> is there for one reason only: each turn is a separate
loop threaded by the same changing flux, and the turns are in series.

**Average EMF over a finite change.** Often the flux changes by a known amount ![Delta Phi_B](faradays-law.assets/eq-inline/a0a09d0579.svg)<!--m:\Delta\Phi_B--> (in Wb)
over a known time ![Delta t](faradays-law.assets/eq-inline/fdecfb4216.svg)<!--m:\Delta t--> (in s). The average EMF over that interval is:

![the average EMF equals minus N delta Phi_B over delta t](faradays-law.assets/eq-average.svg)

**Worked numbers — the magnet into the coil.** A 200-turn coil of total resistance ![10 Omega](faradays-law.assets/eq-inline/894d60ce67.svg)<!--m:10\ \Omega-->
(galvanometer included). Pushing a magnet in raises the flux through each turn from about zero to
![0.5 mWb](faradays-law.assets/eq-inline/34d9ec9ffb.svg)<!--m:0.5\ \mathrm{mWb}--> in ![0.1 s](faradays-law.assets/eq-inline/2c3684816f.svg)<!--m:0.1\ \mathrm{s}-->:

![average EMF magnitude equals 200 times 0.5 milliwebers over 0.1 seconds, which is 1 volt; the current is 1 volt over 10 ohms, which is 0.1 ampere](faradays-law.assets/eq-magnet-worked.svg)

Push twice as fast and the EMF and current double; hold the magnet still inside the coil and both
are zero, though the flux is at its largest. Pull it out and the EMF has the same size, opposite sign.

**The charge does not care how fast.** Here is a less obvious consequence, the content of Felici's
law. The total charge ![Q](faradays-law.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> (in coulombs) driven round the circuit is the current integrated over time.
Substitute ![I = E/R](faradays-law.assets/eq-inline/63180aabed.svg)<!--m:I = \mathcal{E}/R--> and Faraday's law:

![Q equals the integral of I dt, which equals the integral of EMF over R dt, which equals minus N over R times the integral of d Phi_B by dt dt](faradays-law.assets/eq-charge-1.svg)

The integral of a rate of change over time is just the total change (the fundamental theorem of
calculus), so:

![Q equals minus N over R times delta Phi_B, whatever the speed](faradays-law.assets/eq-charge-2.svg)

A fast push gives a large current for a short time, a slow push a small current for a long time;
the charge is the same. For the coil above:

![Q equals 200 over 10 ohms times 0.5 milliwebers, which is 10 millicoulombs, fast or slow](faradays-law.assets/eq-charge-worked.svg)

This is how a *fluxmeter* measures flux: it integrates the current and reads ![Delta Phi_B](faradays-law.assets/eq-inline/a0a09d0579.svg)<!--m:\Delta\Phi_B--> directly.

![A magnet dropped through a coil: the flux rises to a peak and falls back; the EMF is a negative pulse then a larger, shorter positive pulse because the magnet leaves faster than it entered](faradays-law.assets/fig-05.svg)

_A magnet dropped through a coil, computed for a real geometry. The flux is one smooth bump; the EMF
is its slope, so it is two pulses of opposite sign. The leaving pulse is taller and narrower because
gravity has sped the magnet up, but its area is the same — the total flux change in and out is zero,
so by the result above no net charge flows._

## 7 Lenz's law — the minus sign is energy conservation

The minus sign in ![E = -d Phi_B/dt](faradays-law.assets/eq-inline/6a934915ac.svg)<!--m:\mathcal{E} = -d\Phi_B/dt--> carries the direction, and it is easy to apply once the
sign convention is clear.

**Reading the sign.** Choose ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->, and with it the positive direction around the loop by the
right-hand rule (§3). If the flux along ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}--> is increasing, ![d Phi_B/dt > 0](faradays-law.assets/eq-inline/9bdfb29c7a.svg)<!--m:d\Phi_B/dt > 0-->, so ![E < 0](faradays-law.assets/eq-inline/39422ffb87.svg)<!--m:\mathcal{E} < 0-->: the
EMF, and the current it drives, run *against* the curl of your fingers. That current makes its own
magnetic field (by [Ampère's law](../amperes-law/)), and inside the loop that field points along
![- n](faradays-law.assets/eq-inline/83eff3944a.svg)<!--m:-\hat{n}--> — against the increase. If the flux is decreasing, everything reverses and the induced
field points along ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->, propping the flux up.

**Lenz's law** says the same thing without any sign convention: *the induced current flows in the
direction whose own magnetic field opposes the change of flux that produced it.* It opposes the
change, not the flux: a falling flux is propped up, not pushed down.

> **Note —** The Wikipedia article gives an equivalent recipe with the *left* hand. Curl the fingers
> of the left hand along the loop; the thumb then gives a normal ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->. If the flux along that
> ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}--> is increasing, the EMF runs along the curled fingers; if it is decreasing, against them.
> Using the left hand is just a way of building the minus sign into the hand.

![Lenz law for a magnet and a ring in four cases: the induced current always makes a field that opposes the change in flux, so an approaching pole is repelled and a receding pole is attracted](faradays-law.assets/fig-06.svg)

_All four cases follow one rule. An approaching pole always meets a like pole on the ring's near face
and is repelled; a retreating pole always meets an unlike pole and is held back. The ring resists the
motion whichever way you move the magnet._

**Why it must be so — energy.** Suppose the sign were the other way, so that the induced current
*aided* the change. Push a north pole towards a ring: the ring would turn its near face into a south
pole and *pull the magnet in*. The magnet would speed up, the flux would rise faster, the current would
grow, the pull would grow — a runaway that produces kinetic energy and electrical heat from nothing.
No such device exists. With the actual sign, the ring repels the approaching magnet, so you have to
push, and the work you do is exactly the energy that turns up as heat in the ring:

![the power delivered to the loop equals EMF times I, which equals I squared R, which is positive](faradays-law.assets/eq-lenz-power.svg)

Section 9 checks this balance to the watt for the sliding bar. The minus sign is not a convention you
may choose; it is energy conservation.

## 8 Two ways to change the flux

For a uniform field and a flat loop, ![Phi_B = BA cos theta](faradays-law.assets/eq-inline/8b6cd89722.svg)<!--m:\Phi_B = BA\cos\theta-->, and any of the three factors may change
with time:

![Phi_B equals B A cos theta, and all three may change with time](faradays-law.assets/eq-product-1.svg)

Differentiate with the product rule (the derivative of a product is the sum of terms, each
differentiating one factor and keeping the others), using ![d( cos theta )/dt = - sin theta d theta/dt](faradays-law.assets/eq-inline/4771b46ce9.svg)<!--m:d(\cos\theta)/dt = -\sin\theta\,d\theta/dt-->:

![by the product rule, d Phi_B by dt equals dB by dt A cos theta plus B dA by dt cos theta minus B A sin theta d theta by dt](faradays-law.assets/eq-product-2.svg)

The three terms sort into two physically different mechanisms.

- **The field changes** (first term): the loop sits still and ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> grows or shrinks — a switched
  current in a nearby coil, an AC current, a magnet's field sweeping over. No charge in the wire is
  moving, so the magnetic force ![q v times B](faradays-law.assets/eq-inline/2365d1224f.svg)<!--m:q\vec{v}\times\vec{B}--> is zero; the push must be an electric field.
  This is **transformer EMF** (§10).
- **The loop moves or changes shape** (second and third terms): the field is steady but the area
  grows, or the loop turns. The charges move with the wire through the field and feel ![q v times B](faradays-law.assets/eq-inline/2365d1224f.svg)<!--m:q\vec{v}\times\vec{B}-->.
  This is **motional EMF** (§9).

![Three ways to raise the flux through a square loop: the loop moves into a field, the field moves over the loop, or the field grows in place; all three drive the same counter-clockwise current](faradays-law.assets/fig-07-anim.svg)

_Three different physical situations, one equation. In (a) the loop's charges move through the
field and feel the magnetic force; in (b) and (c) the loop is still, so only an electric field can
push them. Yet the current is identical, because in all three the flux into the page is rising at
the same rate. This coincidence is what Einstein asked about (§14)._

Case (b), a moving magnet and a still loop, looks like case (a) but is physically case (c): in the
loop's frame nothing moves, and the field at each point of the loop is changing in time.

## 9 Motional EMF — derived from the magnetic force on the charges

**A rod on its own.** Take a straight metal rod of length ![l](faradays-law.assets/eq-inline/07c342be6e.svg)<!--m:l--> (in metres) and move it at constant
velocity ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> through a uniform field ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->, with the rod, ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> and ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> all at right
angles to each other. Every charge in the rod moves with it, so each feels the magnetic part of the
Lorentz force:

![the magnetic force on a charge q in the rod is q v cross B](faradays-law.assets/eq-rod-force.svg)

Because ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> is perpendicular to ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->, the sine in the cross product is 1, and the
right-hand rule makes the force point *along the rod*:

![v is perpendicular to B, so the size of the force is q v B, and it points along the rod](faradays-law.assets/eq-rod-mag.svg)

The step people find confusing is this: the force is sideways to the *motion* of the rod, but that
sideways direction is *along the length* of the rod, and the charges are free to move along the rod.
So the magnetic force pushes them from one end towards the other. Carry a charge the full length
![l](faradays-law.assets/eq-inline/07c342be6e.svg)<!--m:l--> and the magnetic force does work:

![the work done on q over the length l of the rod is q v B l](faradays-law.assets/eq-rod-work.svg)

EMF is work per unit charge (§5), so:

![the EMF is the work per charge, B l v](faradays-law.assets/eq-rod-emf.svg)

> **Watch out —** A magnetic force does no work on a *free* charge, because it is always
> perpendicular to that charge's total velocity. Here the work is done because the rod's own
> velocity ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> and the charge's drift along the rod are at right angles: the magnetic force
> has a component along the drift, and an equal and opposite component pushes back on the rod as a
> whole (the drag force below). Whoever keeps the rod moving supplies the energy; the magnetic field
> only redirects it.

![A conducting rod moving through a magnetic field: the magnetic force pushes electrons to one end until the electric field of the separated charge balances it, leaving a voltage B l v between the ends](faradays-law.assets/fig-08.svg)

_Electrons, the actual mobile charges, carry charge ![-e](faradays-law.assets/eq-inline/2360917b93.svg)<!--m:-e--> (with ![e = 1.602 times 10^-19 C](faradays-law.assets/eq-inline/e82ad9559e.svg)<!--m:e = 1.602\times10^{-19}\ \mathrm{C}-->) and are
pushed the opposite way to the arrow for a positive charge — same separation, same sign of the
result. The pile-up stops when the electric field of the separated charge balances the magnetic push._

**What happens in an open rod.** With nowhere to go, the electrons pile up at one end, leaving
positive metal ions exposed at the other. That separated charge sets up an electric field ![E](faradays-law.assets/eq-inline/e0184adedf.svg)<!--m:E--> (in
![V times m^-1](faradays-law.assets/eq-inline/b6f586582e.svg)<!--m:\mathrm{V\cdot m^{-1}}-->) along the rod, pushing back. The pile-up stops when the electric push on each
charge balances the magnetic one:

![in equilibrium q E equals q v B, so E equals v B and the voltage between the ends is E l, which is B l v](faradays-law.assets/eq-rod-balance.svg)

Two routes, the same answer: the open-circuit voltage between the ends equals the EMF, as §5 said.
With ![B = 0.5 T](faradays-law.assets/eq-inline/1bd1e227e8.svg)<!--m:B = 0.5\ \mathrm{T}-->, ![l = 0.2 m](faradays-law.assets/eq-inline/6b06ecea67.svg)<!--m:l = 0.2\ \mathrm{m}-->, ![v = 10 m times s^-1](faradays-law.assets/eq-inline/44e2cc6cbf.svg)<!--m:v = 10\ \mathrm{m\cdot s^{-1}}-->:

![EMF equals 0.5 tesla times 0.2 metres times 10 metres per second, which is 1 volt](faradays-law.assets/eq-rod-worked.svg)

**Close the circuit: the sliding bar.** Lay the rod across two parallel rails a distance ![l](faradays-law.assets/eq-inline/07c342be6e.svg)<!--m:l--> apart,
joined at one end by a resistor ![R](faradays-law.assets/eq-inline/06576556d1.svg)<!--m:R-->. The bar, the rails and the resistor form a loop whose area
grows as the bar moves. Let ![x(t)](faradays-law.assets/eq-inline/62b10cd9e1.svg)<!--m:x(t)--> (in metres) be the bar's distance from the resistor, so
![dx/dt = v](faradays-law.assets/eq-inline/9b4f1f2fa2.svg)<!--m:dx/dt = v-->. Check the result against the flux rule:

![the flux through the circuit is B l x, so d Phi_B by dt equals B l dx by dt, which is B l v](faradays-law.assets/eq-bar-flux.svg)

The magnetic-force argument and the flux rule agree exactly, as §12 proves they must. The EMF
drives a current through the resistor:

![I equals EMF over R, which is B l v over R](faradays-law.assets/eq-bar-current.svg)

![A metal bar slides to the right along two rails joined by a resistor, in a field into the page; the enclosed area and flux grow, and current flows up the bar, back along the top rail and down through the resistor](faradays-law.assets/fig-09-anim.svg)

_The area to the left of the bar, and so the flux, grows steadily, so the current is steady. It runs
counter-clockwise: its own field inside the loop points out of the page, against the growing flux
into the page — Lenz again._

**A loop that is wholly inside the field.** Go back to the square loop of Figure 121(a). While only
its leading side is in the field, that side alone carries the push ![Blv](faradays-law.assets/eq-inline/56cd4183ff.svg)<!--m:Blv-->, and current flows. Once the
whole loop is inside a uniform field, its trailing side moves through the same field at the same
speed and carries the same push ![Blv](faradays-law.assets/eq-inline/56cd4183ff.svg)<!--m:Blv--> — but going round the loop, that side is walked in the opposite
direction, so the two pushes cancel and the EMF is zero. The flux rule agrees: the loop's flux is now
constant. When the leading side leaves the field the trailing side's push is left alone, and the
current flows the other way.

**The drag, and the energy.** The current in the bar flows through the field, so the bar feels a
force ![F_B = I l times B](faradays-law.assets/eq-inline/eb24f56dc9.svg)<!--m:\vec{F}_B = I\vec{l}\times\vec{B}-->, where ![l](faradays-law.assets/eq-inline/6a16ff3a0c.svg)<!--m:\vec{l}--> is a vector of length ![l](faradays-law.assets/eq-inline/07c342be6e.svg)<!--m:l--> along the bar in the
current's direction (this is the magnetic force on all the moving charges in the bar added up). It
points backwards, against ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}-->:

![the force on the current-carrying bar is I l cross B, of size I l B, which equals B squared l squared v over R](faradays-law.assets/eq-bar-force.svg)

To keep the speed constant you must push with an equal and opposite force, delivering mechanical
power ![P = Fv](faradays-law.assets/eq-inline/78307ba909.svg)<!--m:P = Fv--> (in watts, ![1 W = 1 J times s^-1](faradays-law.assets/eq-inline/392de75f6b.svg)<!--m:1\ \mathrm{W} = 1\ \mathrm{J\cdot s^{-1}}-->):

![the mechanical power F v equals B squared l squared v squared over R, which equals EMF squared over R, which equals EMF times I](faradays-law.assets/eq-bar-power.svg)

Power in from your hand equals electrical power out. With ![R = 0.5 Omega](faradays-law.assets/eq-inline/b263f96ae5.svg)<!--m:R = 0.5\ \Omega--> and the numbers above:

![with B 0.5 tesla, l 0.2 metres, v 10 metres per second and R 0.5 ohm: EMF 1 volt, I 2 amperes, force 0.2 newtons, power 2 watts](faradays-law.assets/eq-bar-worked.svg)

![The forces on the sliding bar and the energy chain: the hand pushes with force B I l at speed v, the EMF B l v delivers power E I, and the resistor dissipates I squared R; all three are 2 watts in the worked example](faradays-law.assets/fig-10.svg)

_The three powers are the same 2 W. That equality is Lenz's law in numbers: the induced current had
to point the way that makes the bar drag, or the books would not balance._

**If nobody pushes.** Give the bar (mass ![m](faradays-law.assets/eq-inline/6b0d31c0d5.svg)<!--m:m-->, in kg) a shove and let go. The only horizontal force is
the drag, so Newton's second law gives:

![if nobody pushes, m dv by dt equals minus B squared l squared v over R](faradays-law.assets/eq-coast-1.svg)

A quantity whose rate of decrease is proportional to itself decays exponentially, with time constant
![tau](faradays-law.assets/eq-inline/c9148a5f77.svg)<!--m:\tau--> (in seconds):

![so v of t equals v_0 times e to the minus t over tau, with tau equal to m R over B squared l squared](faradays-law.assets/eq-coast-2.svg)

Here ![v_0](faradays-law.assets/eq-inline/2ba313ffee.svg)<!--m:v_0--> is the speed at the moment you let go. For a ![0.1 kg](faradays-law.assets/eq-inline/6e37080e6a.svg)<!--m:0.1\ \mathrm{kg}--> bar on the same rails:

![for a 0.1 kilogram bar, tau equals 0.1 times 0.5 over 0.5 squared times 0.2 squared, which is 5 seconds](faradays-law.assets/eq-coast-worked.svg)

All the bar's kinetic energy ends up as heat in the resistor. This is magnetic braking in its
simplest form (§15).

## 10 Transformer EMF — the induced electric field

Now case (c) of Figure 121: the loop is stationary and the field through it changes. The charges in
the wire are at rest, so ![v = 0](faradays-law.assets/eq-inline/213adcb8d2.svg)<!--m:\vec{v} = 0--> and the magnetic force on them is exactly zero. Yet a current
flows. The only thing in the Lorentz force that can push a charge at rest is an electric field. So a
changing magnetic field must be accompanied by an electric field, and since it drives charge all the
way round a closed loop, it must be **non-conservative**: its closed line integral is the EMF, not
zero.

This field exists whether or not a wire is there. The wire is just a detector that lets charge
respond to it.

**Working out the field — a uniform B in a circular region.** Let a uniform field ![B(t)](faradays-law.assets/eq-inline/57f87001bb.svg)<!--m:B(t)--> fill a
circular region of radius ![R](faradays-law.assets/eq-inline/06576556d1.svg)<!--m:R--> (in metres) — the inside of a long solenoid, say — and change with
time. By symmetry the induced field circles the axis: on a circle of radius ![r](faradays-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> (in metres) it has
the same size ![E](faradays-law.assets/eq-inline/e0184adedf.svg)<!--m:E--> everywhere and points along the circle. Take that circle as the loop. Then the
closed line integral is just ![E](faradays-law.assets/eq-inline/e0184adedf.svg)<!--m:E--> times the circumference:

![around a circle of radius r, E is the same size everywhere and along the path, so the closed line integral is E times 2 pi r](faradays-law.assets/eq-ring-1.svg)

Inside the region the circle encloses flux:

![inside the region the flux through the circle is B pi r squared](faradays-law.assets/eq-ring-2.svg)

Faraday's law sets the two equal (with the sign):

![so E times 2 pi r equals minus pi r squared dB by dt, and E equals minus r over 2 dB by dt](faradays-law.assets/eq-ring-3.svg)

Outside the region the circle encloses all of it, ![pi R^2 B](faradays-law.assets/eq-inline/1a6243f6a8.svg)<!--m:\pi R^2 B-->, however large ![r](faradays-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> is:

![outside, all of the flux B pi R squared is enclosed, so E equals minus R squared over 2 r dB by dt](faradays-law.assets/eq-ring-4.svg)

So the induced field grows linearly from zero on the axis to the edge of the region, then falls off
as ![1/r](faradays-law.assets/eq-inline/525108fcf9.svg)<!--m:1/r--> outside — where ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> itself is zero. A wire loop around a long solenoid, in a region with no
magnetic field at all, still feels a push: the EMF depends on the flux enclosed, not on the field
where the wire is.

![A field into the page grows and shrinks inside a circular region; an induced electric field circulates around it, counter-clockwise while the field grows and clockwise while it shrinks, inside and outside the region](faradays-law.assets/fig-11-anim.svg)

_The field lines of this electric field are closed circles: they have no beginning and no end, so
there is no charge for them to start on and no potential to define. While ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> grows the push is
counter-clockwise; while it shrinks the dots turn back. The faster the change, the faster they move._

**Worked numbers — a mains transformer core.** A core of radius ![5 cm](faradays-law.assets/eq-inline/2ae14f3384.svg)<!--m:5\ \mathrm{cm}--> carries
![B(t) = 1.5 T sin omega t](faradays-law.assets/eq-inline/fd1ae568ab.svg)<!--m:B(t) = 1.5\ \mathrm{T}\,\sin\omega t--> at ![f = 50 Hz](faradays-law.assets/eq-inline/d7442e5208.svg)<!--m:f = 50\ \mathrm{Hz}-->. The **angular frequency** ![omega](faradays-law.assets/eq-inline/73b077a63e.svg)<!--m:\omega--> (in
radians per second, ![rad times s^-1](faradays-law.assets/eq-inline/da19396712.svg)<!--m:\mathrm{rad\cdot s^{-1}}-->, often written just ![s^-1](faradays-law.assets/eq-inline/5896553b55.svg)<!--m:\mathrm{s^{-1}}-->) is
![omega = 2 pi f](faradays-law.assets/eq-inline/10f7ad86c0.svg)<!--m:\omega = 2\pi f-->, because one cycle is ![2 pi](faradays-law.assets/eq-inline/0833718ca4.svg)<!--m:2\pi--> radians; here ![omega = 314 s^-1](faradays-law.assets/eq-inline/1e72a02b65.svg)<!--m:\omega = 314\ \mathrm{s^{-1}}-->. The steepest
slope of ![sin omega t](faradays-law.assets/eq-inline/22c43934a5.svg)<!--m:\sin\omega t--> is ![omega](faradays-law.assets/eq-inline/73b077a63e.svg)<!--m:\omega-->, at the zero crossings:

![for B equal to 1.5 tesla times sin of omega t at 50 hertz, the peak dB by dt is omega times 1.5 tesla, which is 471 tesla per second](faradays-law.assets/eq-dbdt-worked.svg)

At the surface of the core (![r = R = 5 cm](faradays-law.assets/eq-inline/2b94ec798f.svg)<!--m:r = R = 5\ \mathrm{cm}-->) the peak induced field, and the peak EMF in one
turn wound round the core, are:

![with dB by dt 471 tesla per second at r 5 centimetres, E is 11.8 volts per metre and the EMF round the circle is 3.7 volts](faradays-law.assets/eq-ring-worked.svg)

So every turn of a winding on this core gets ![3.7 V](faradays-law.assets/eq-inline/93189242ec.svg)<!--m:3.7\ \mathrm{V}--> peak, about ![2.6 V](faradays-law.assets/eq-inline/5939f21018.svg)<!--m:2.6\ \mathrm{V}--> RMS (root mean square, the equivalent steady value; for a sine it is the peak divided by ![sqrt 2](faradays-law.assets/eq-inline/6d0fdf0909.svg)<!--m:\sqrt2-->, §15.1): the
"volts per turn" of the [transformer](../transformer/transformer.md#3-the-ideal-transformer-derived-from-faraday).

**Voltage is no longer a single number.** Because this field is not conservative, the "voltage
between two points" depends on the path you measure along. Figure 126 makes this concrete. A ring of
two resistors, ![R_1 = 100 Omega](faradays-law.assets/eq-inline/9ea8639cac.svg)<!--m:R_1 = 100\ \Omega--> and ![R_2 = 900 Omega](faradays-law.assets/eq-inline/39d66d8447.svg)<!--m:R_2 = 900\ \Omega-->, surrounds a solenoid whose changing flux
gives ![E = 1 V](faradays-law.assets/eq-inline/43eba9c03f.svg)<!--m:\mathcal{E} = 1\ \mathrm{V}--> round the ring; the current is
![I = 1 V/1000 Omega = 1 mA](faradays-law.assets/eq-inline/2f43b8c127.svg)<!--m:I = 1\ \mathrm{V}/1000\ \Omega = 1\ \mathrm{mA}-->. Two voltmeters are clipped to the same two nodes A
and B, one with its leads on the left and one on the right:

![V_1 equals I R_1, V_2 equals minus I R_2, and V_1 minus V_2 equals I times R_1 plus R_2, which is the EMF](faradays-law.assets/eq-two-meters.svg)

![A loop of two resistors around a changing flux: a voltmeter on the left reads plus 0.1 volt and one on the right reads minus 0.9 volt across the same two nodes, because the induced field makes the reading depend on the path](faradays-law.assets/fig-12.svg)

_Neither meter is wrong. Each reads the work per charge along its own path, and the two paths
together go once round the changing flux, so they differ by exactly the EMF. Kirchhoff's voltage law
holds for each meter's loop because neither loop encloses the flux; the big ring does, and there it
fails by exactly ![E](faradays-law.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}-->._

> **Watch out —** This is why a scope probe's ground lead picks up "noise" near a switching
> converter: the loop formed by the probe tip, the ground lead and the circuit encloses changing flux,
> and the scope faithfully reports the EMF of that loop. Shrinking the loop (a ground spring instead of
> a long clip) is the cure, because it shrinks the enclosed flux.

## 11 The Maxwell–Faraday equation, and what curl means

**The integral form.** Section 10 says that around *any* closed path ![partial Sigma](faradays-law.assets/eq-inline/67dc6e421e.svg)<!--m:\partial\Sigma--> fixed in space,
the circulation of ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> equals minus the rate of change of the flux through any surface ![Sigma](faradays-law.assets/eq-inline/cb5615b3fc.svg)<!--m:\Sigma-->
bounded by it. Written as a statement about fields alone, with no wires in it, that is the
**Maxwell–Faraday equation**:

![the closed line integral of E dot dl around the rim of sigma equals minus the surface integral of partial B by partial t dot dA](faradays-law.assets/eq-mf-integral.svg)

The symbol ![partial B/partial t](faradays-law.assets/eq-inline/30f55b7e48.svg)<!--m:\partial\vec{B}/\partial t--> is a **partial derivative**: the rate of change of ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> with
time at a fixed point in space (the curly ![partial](faradays-law.assets/eq-inline/b99cc6ef78.svg)<!--m:\partial--> means "holding position fixed"). The left side is
the circulation of the electric field, the work per charge round the path. In electrostatics it is
always zero (§5); with a changing magnetic field it is not.

When the surface does not move, it makes no difference whether you differentiate before or after
adding up the flux, and the right side becomes the flux rule:

![for a surface that does not move, this equals minus d by dt of the surface integral of B dot dA, which is minus d Phi_B by dt](faradays-law.assets/eq-mf-fixed.svg)

Identify the left side with the work per charge done on the charges in a stationary wire along that
path, and this is Faraday's law for a stationary circuit.

**From loops to points.** The integral form talks about whole loops. To get a statement about a
single point, shrink the loop. The key is the **Kelvin–Stokes theorem**:

![Kelvin-Stokes theorem: the closed line integral of E dot dl equals the surface integral of curl E dot dA](faradays-law.assets/eq-stokes.svg)

Here ![nabla times E](faradays-law.assets/eq-inline/2596fbd332.svg)<!--m:\nabla\times\vec{E}-->, the **curl** of ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> (in ![V times m^-2](faradays-law.assets/eq-inline/0c8bb130d3.svg)<!--m:\mathrm{V\cdot m^{-2}}-->), is a vector field that
measures how much ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> circulates around each point. Figure 127(a) shows why the theorem is true:
tile the surface with tiny loops, all circulating the same way. Every inside edge is shared by two
tiles and walked once in each direction, so it cancels. Only the outer rim survives. The circulation
round the rim is therefore the sum of the tiny circulations — and a tiny circulation, divided by the
tiny area it encloses, is what curl *is*.

**Curl, done slowly.** Take a tiny rectangle with sides ![dx](faradays-law.assets/eq-inline/4a2b94f8a9.svg)<!--m:dx--> and ![dy](faradays-law.assets/eq-inline/d7dd16a449.svg)<!--m:dy--> in the ![x](faradays-law.assets/eq-inline/11f6ad8ec5.svg)<!--m:x-->–![y](faradays-law.assets/eq-inline/95cb0bfd29.svg)<!--m:y--> plane, walked
counter-clockwise (so its normal is ![+z](faradays-law.assets/eq-inline/81e5266b98.svg)<!--m:+z-->), with ![E_x](faradays-law.assets/eq-inline/3e63929dcf.svg)<!--m:E_x-->, ![E_y](faradays-law.assets/eq-inline/ec172354f3.svg)<!--m:E_y--> the components of ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> along ![x](faradays-law.assets/eq-inline/11f6ad8ec5.svg)<!--m:x--> and
![y](faradays-law.assets/eq-inline/95cb0bfd29.svg)<!--m:y-->. The bottom edge is walked in ![+x](faradays-law.assets/eq-inline/0eaccee34c.svg)<!--m:+x--> and the top in ![-x](faradays-law.assets/eq-inline/b858f570dc.svg)<!--m:-x-->, so together they contribute the
difference of ![E_x](faradays-law.assets/eq-inline/3e63929dcf.svg)<!--m:E_x--> across the height ![dy](faradays-law.assets/eq-inline/d7dd16a449.svg)<!--m:dy-->:

![the bottom and top edges give E_x at y times dx minus E_x at y plus dy times dx, which is minus partial E_x by partial y dx dy](faradays-law.assets/eq-curl-loop-1.svg)

The right edge is walked in ![+y](faradays-law.assets/eq-inline/84dbdf7165.svg)<!--m:+y--> and the left in ![-y](faradays-law.assets/eq-inline/fbcd37d727.svg)<!--m:-y-->:

![the right and left edges give E_y at x plus dx times dy minus E_y at x times dy, which is partial E_y by partial x dx dy](faradays-law.assets/eq-curl-loop-2.svg)

Add and divide by the area:

![so the circulation per unit area is partial E_y by partial x minus partial E_x by partial y, the z component of curl E](faradays-law.assets/eq-curl-loop-3.svg)

Doing the same in the other two planes gives the other two components:

![curl E has components partial E_z by partial y minus partial E_y by partial z, partial E_x by partial z minus partial E_z by partial x, and partial E_y by partial x minus partial E_x by partial y](faradays-law.assets/eq-curl-full.svg)

![Stokes theorem: tiny loops tiling a surface cancel along every shared edge so only the rim survives; one tiny loop gives the curl components; a paddle wheel spins in a curling field and not in a uniform one](faradays-law.assets/fig-13.svg)

_The paddle-wheel picture is the intuition to keep. Drop a tiny paddle wheel into a flowing field: if
the push is stronger on one side of the wheel than the other, it spins, and the spin rate and axis
are the curl. A uniform field pushes every paddle equally and the wheel stays still; a circulating
field spins it._

**The differential form.** Put Stokes into the integral form and move everything to one side:

![so the surface integral of curl E plus partial B by partial t, dot dA, is zero for every surface sigma](faradays-law.assets/eq-mf-combine.svg)

The only way an integral can vanish over *every* surface, however small and however oriented, is
for the thing being integrated to be zero at every point:

![curl E equals minus partial B by partial t](faradays-law.assets/eq-mf-diff.svg)

This is Faraday's law as one of Maxwell's four equations. Read it as: wherever the magnetic field is
changing, the electric field curls around the direction of change, in the left-handed sense (that is
the minus sign); where ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> is steady, ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> has no curl and is the ordinary conservative field
of charges.

**Check it on the ring field of §10.** With ![B = B_z(t) z](faradays-law.assets/eq-inline/9b0b2ead71.svg)<!--m:\vec{B} = B_z(t)\,\hat{z}--> (the ![z](faradays-law.assets/eq-inline/395df8f7c5.svg)<!--m:z--> axis out of the page) and
the field inside the region from §10, written in components:

![the ring field written in x and y is E equals dB by dt over 2 times y, minus x](faradays-law.assets/eq-curl-check-1.svg)

Apply the ![z](faradays-law.assets/eq-inline/395df8f7c5.svg)<!--m:z--> component of the curl:

![its curl z component is one half dB_z by dt times minus 1 minus 1, which is minus dB_z by dt](faradays-law.assets/eq-curl-check-2.svg)

Exactly ![- partial B_z/partial t](faradays-law.assets/eq-inline/395200c90c.svg)<!--m:-\partial B_z/\partial t-->, at every point inside the region. (Outside, the same calculation
with ![E proportional to 1/r](faradays-law.assets/eq-inline/83f095cba5.svg)<!--m:E \propto 1/r--> gives zero curl, as it must where ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> is steady at zero — the field circulates
there without having any curl, which is possible only because the loop round it encloses the region
where the curl lives.)

**Where the induced field comes from.** For slowly varying fields there is even an explicit
formula, the electric twin of the Biot–Savart law for ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->: every region where ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> is
changing contributes to the induced (circulating) part ![E_s](faradays-law.assets/eq-inline/d7b70d6a56.svg)<!--m:\vec{E}_s--> of the electric field at a point
![r](faradays-law.assets/eq-inline/e8436e4526.svg)<!--m:\vec{r}-->, weighted by the inverse square of the distance:

![the induced electric field at r is approximately minus one over 4 pi times the volume integral of partial B by partial t at r prime cross r minus r prime over the cube of the distance](faradays-law.assets/eq-es-integral.svg)

Here ![r '](faradays-law.assets/eq-inline/c423e9d16c.svg)<!--m:\vec{r}\,'--> runs over the volume ![V](faradays-law.assets/eq-inline/c9ee5681d3.svg)<!--m:V--> where the field is changing and ![d^3 r '](faradays-law.assets/eq-inline/63dacf6bbf.svg)<!--m:d^3\vec{r}\,'--> is a small volume
element (in ![m^3](faradays-law.assets/eq-inline/577fad1425.svg)<!--m:\mathrm{m^3}-->). You will rarely evaluate it, but it makes the point: the changing field at
one place produces circulating electric field everywhere around it, falling off with distance.

## 12 Deriving the flux rule from the field laws

The flux rule covers both motional and transformer EMF, while the Maxwell–Faraday equation covers
only the changing-field part. This section shows the flux rule *follows* from the field laws plus the
Lorentz force, and in doing so finds exactly when it holds.

**Step 1 — how fast does the flux through a moving surface change?** Let the surface ![Sigma (t)](faradays-law.assets/eq-inline/6a77d3f41e.svg)<!--m:\Sigma(t)--> and
its rim ![partial Sigma (t)](faradays-law.assets/eq-inline/38abc89225.svg)<!--m:\partial\Sigma(t)--> move, each point of the rim with velocity ![v_c](faradays-law.assets/eq-inline/d237395046.svg)<!--m:\vec{v}_c--> (in
![m times s^-1](faradays-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}-->). The flux changes for two reasons: the field changes at each point, and the
surface moves to enclose a different region. The three-dimensional **Leibniz integral rule** (also
called the flux transport theorem) counts both:

![d by dt of the flux through a moving surface equals the surface integral of partial B by partial t plus divergence of B times v_c, dot dA, minus the closed line integral of v_c cross B dot dl](faradays-law.assets/eq-leibniz.svg)

The first term is the field changing in place. The last term is the rim sweeping new area. Figure
128 shows where it comes from: in a time ![dt](faradays-law.assets/eq-inline/f7e6632892.svg)<!--m:dt--> a rim element ![d l](faradays-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> moves by ![v_c dt](faradays-law.assets/eq-inline/5c172462cc.svg)<!--m:\vec{v}_c\,dt--> and sweeps
a thin parallelogram of area ![( v_c dt) times d l](faradays-law.assets/eq-inline/ec50b0ce03.svg)<!--m:(\vec{v}_c\,dt)\times d\vec{l}-->, which adds flux
![B times ( v_c times d l) dt](faradays-law.assets/eq-inline/19e42166a4.svg)<!--m:\vec{B}\cdot(\vec{v}_c\times d\vec{l})\,dt-->. Swapping the order of a triple product cyclically
(it does not change its value) and then reversing the cross product (which flips its sign) turns that
into ![-( v_c times B) times d l dt](faradays-law.assets/eq-inline/d7f08955e0.svg)<!--m:-(\vec{v}_c\times\vec{B})\cdot d\vec{l}\,dt-->.

![A loop whose rim moves: in a time dt each rim element dl sweeps a thin parallelogram of area v dt cross dl, so the flux through the loop grows by B dot that area, which is minus v cross B dot dl times dt](faradays-law.assets/fig-14.svg)

_The rim, moving, sweeps a sliver of new surface; the flux through that sliver is the motional term.
The sliding bar of §9 is the simplest case: the only moving part of the rim is the bar, and the
sliver is a strip ![l times v dt](faradays-law.assets/eq-inline/aeae3f2098.svg)<!--m:l\times v\,dt-->._

The middle term contains the **divergence** ![nabla times B](faradays-law.assets/eq-inline/ca5e9aada4.svg)<!--m:\nabla\cdot\vec{B}-->, the net outflow of field per unit
volume from a point. Gauss's law for magnetism (§3) in point form says it is zero everywhere:

![divergence of B equals zero](faradays-law.assets/eq-div-b.svg)

So that term drops out:

![so d Phi_B by dt equals the surface integral of partial B by partial t dot dA minus the closed line integral of v_c cross B dot dl](faradays-law.assets/eq-derive-1.svg)

**Step 2 — use Maxwell–Faraday.** Replace the first term by minus the circulation of ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}-->
(§11), and combine the two line integrals:

![replace the surface integral using Maxwell-Faraday: d Phi_B by dt equals minus the closed line integral of E plus v_c cross B, dot dl](faradays-law.assets/eq-derive-2.svg)

This is exact. But ![v_c](faradays-law.assets/eq-inline/d237395046.svg)<!--m:\vec{v}_c--> is the velocity of an abstract rim, not of any charge.

**Step 3 — bring in the actual charges.** A charge carrier in a conductor moves with the conductor
material (velocity ![v_c](faradays-law.assets/eq-inline/d237395046.svg)<!--m:\vec{v}_c-->, now the velocity of the metal itself) plus its own drift through the
metal (velocity ![v_d](faradays-law.assets/eq-inline/c895457db9.svg)<!--m:\vec{v}_d-->, the drift velocity that makes up the current):

![the velocity of a charge carrier equals the conductor velocity v_c plus the drift velocity v_d](faradays-law.assets/eq-velocity-split.svg)

(Simple addition of velocities is fine at these speeds.) The EMF of §5 uses the charge's full
velocity. Split it, and recognise Step 2 in the first part:

![the EMF equals the closed line integral of E plus v_c cross B, plus the closed line integral of v_d cross B, which is minus d Phi_B by dt plus the closed line integral of v_d cross B dot dl](faradays-law.assets/eq-derive-3.svg)

**Step 4 — thin wire.** In a thin wire the drift is along the wire, parallel to ![d l](faradays-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}-->. A cross
product with ![v_d](faradays-law.assets/eq-inline/c895457db9.svg)<!--m:\vec{v}_d--> is perpendicular to ![v_d](faradays-law.assets/eq-inline/c895457db9.svg)<!--m:\vec{v}_d-->, hence to ![d l](faradays-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}-->, and its dot product with
![d l](faradays-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> is zero:

![in a thin wire v_d is parallel to dl, so v_d cross B dot dl is zero, and EMF equals minus d Phi_B by dt](faradays-law.assets/eq-thin-wire.svg)

That is the flux rule, derived — with the condition attached: **the rim must move with the metal**
(the ![v_c](faradays-law.assets/eq-inline/d237395046.svg)<!--m:\vec{v}_c--> of Step 1 must be the velocity of the conductor), and the conductor must be thin.
The equivalent form that never needs that condition keeps the two mechanisms separate:

![EMF equals minus the surface integral of partial B by partial t dot dA plus the closed line integral of v cross B dot dl](faradays-law.assets/eq-two-terms.svg)

**How big is the drift term?** In a thick conductor it need not vanish, but drift velocities are
tiny. For ![1 A](faradays-law.assets/eq-inline/4bd22980e0.svg)<!--m:1\ \mathrm{A}--> in a ![1 mm^2](faradays-law.assets/eq-inline/30fbc10ec2.svg)<!--m:1\ \mathrm{mm^2}--> copper wire, with ![n](faradays-law.assets/eq-inline/d1854cae89.svg)<!--m:n--> the number of free electrons per
cubic metre:

![the drift speed for 1 ampere in 1 square millimetre of copper is 1 over 8.5 times 10 to the 28 times 1.6 times 10 to the minus 19 times 10 to the minus 6, about 7 times 10 to the minus 5 metres per second](faradays-law.assets/eq-drift-worked.svg)

About ![0.07 mm times s^-1](faradays-law.assets/eq-inline/9266e3257c.svg)<!--m:0.07\ \mathrm{mm\cdot s^{-1}}--> — a snail would overtake it. There is one famous case where this
term is the *whole* effect: the **Hall effect**. A current-carrying plate in a magnetic field, with
nothing moving and nothing changing, develops a small voltage across its width, because the drifting
carriers are pushed sideways by ![q v_d times B](faradays-law.assets/eq-inline/c1a8dcb243.svg)<!--m:q\vec{v}_d\times\vec{B}-->. The flux term is zero and the drift term is
all there is; Hall sensors (in every brushless motor and many current probes) read it.

## 13 When the flux rule needs care — the Faraday paradox

It is tempting to promote the flux rule to "for any closed curve in space, the EMF is minus the rate
of change of flux through it". Section 12 shows that is only safe when the curve moves with the
conducting material. Two classic devices break the naive version.

**Faraday's disk.** A copper disk of radius ![R](faradays-law.assets/eq-inline/06576556d1.svg)<!--m:R--> (in metres) spins at angular speed ![omega](faradays-law.assets/eq-inline/73b077a63e.svg)<!--m:\omega--> (in
![rad times s^-1](faradays-law.assets/eq-inline/da19396712.svg)<!--m:\mathrm{rad\cdot s^{-1}}-->) in a steady uniform field ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> along its axle. Sliding contacts (brushes) at
the axle and the rim connect it to a meter. The circuit — axle brush, a radius of the disk, rim brush,
external wire — has a fixed shape, the field is steady, so the flux through the circuit never
changes. The naive rule predicts zero. Yet a steady current flows. That is the **Faraday paradox**.

The force law has no trouble. A piece of the disk at distance ![r](faradays-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> from the axle moves at speed
![omega r](faradays-law.assets/eq-inline/2b9d67ae25.svg)<!--m:\omega r-->, at right angles to both the radius and ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->, so a charge there feels a magnetic force
per unit charge ![omega r B](faradays-law.assets/eq-inline/8486031d58.svg)<!--m:\omega r B--> along the radius:

![a piece of the radius at distance r moves at speed omega r, so the force per charge along the radius is omega r B](faradays-law.assets/eq-disk-1.svg)

Add up the work per charge from the axle to the rim:

![the EMF is the integral from 0 to R of omega r B dr, which is one half B omega R squared](faradays-law.assets/eq-disk-2.svg)

For a ![10 cm](faradays-law.assets/eq-inline/05048e1c9c.svg)<!--m:10\ \mathrm{cm}-->-radius disk at ![50](faradays-law.assets/eq-inline/e1822db470.svg)<!--m:50--> revolutions per second in ![0.5 T](faradays-law.assets/eq-inline/790c25363e.svg)<!--m:0.5\ \mathrm{T}-->:

![with B 0.5 tesla, R 0.1 metre and omega 314 per second, EMF is about 0.79 volts](faradays-law.assets/eq-disk-worked.svg)

The flux rule can be rescued, but only by following the metal: a curve drawn on the spinning disk is
carried round with it, sweeping area at the rate ![1 over 2 R^2 omega](faradays-law.assets/eq-inline/745bb3bc1e.svg)<!--m:\tfrac{1}{2}R^2\omega-->, and the rule then gives the
same ![1 over 2 B omega R^2](faradays-law.assets/eq-inline/92f87b393e.svg)<!--m:\tfrac{1}{2}B\omega R^2-->. The curve that stays still while the metal slides through it gives the
wrong answer.

![Faraday’s disk spins in a steady field and makes a steady EMF although the flux through the circuit never changes; two rocking plates change the flux through the current path a lot yet make almost no EMF](faradays-law.assets/fig-15.svg)

_Two failures of "just count the flux", in opposite directions: the disk makes an EMF with no flux
change through its circuit, and the rocking plates change the flux through their current path a lot
while making almost no EMF. In both, the force on the actual charges gives the right answer._

**The rocking plates.** Two metal plates with curved edges touch at one point and are wired into a
circuit, the whole thing sitting in a uniform field normal to the plates. Rock the plates slightly:
the contact point slides a long way along the curved edges, so the current path through the plates —
and the area it encloses — changes a lot, quickly. The naive flux rule predicts a large EMF. In fact
the EMF is almost zero, because the metal itself barely moves through the field: ![v times B](faradays-law.assets/eq-inline/7159cb59b2.svg)<!--m:\vec{v}\times\vec{B}-->
is tiny everywhere. The current path jumped from one piece of metal to another; no charge was carried
across the field.

> **Tip —** When in doubt, never count flux. Compute
> ![E = loop integral ( E + v times B) times d l](faradays-law.assets/eq-inline/3194dc60c4.svg)<!--m:\mathcal{E} = \oint(\vec{E} + \vec{v}\times\vec{B})\cdot d\vec{l}--> with ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> the velocity of the
> material at each point (or the two-term form of §12). It is always right. The flux rule is a
> shortcut that agrees with it for thin wires that carry their circuit with them — which is every
> coil, transformer and ordinary generator, but not sliding contacts.

## 14 One phenomenon, two descriptions — the road to relativity

Go back to Figure 121. In (a) the loop moves through a still magnet's field: the EMF is the magnetic
force on moving charges, and there is no electric field anywhere. In (b) the magnet moves past a still
loop: the charges are at rest, so the EMF is an induced electric field. Two different explanations;
the same current, to every decimal place, depending only on the *relative* motion.

Maxwell had noticed the two processes in 1861. Einstein made the asymmetry the opening paragraph of
his 1905 relativity paper: a physical result that depends only on relative motion should not need two
different explanations depending on which object you call moving. His resolution was that there is
no preferred frame, and that electric and magnetic fields are not separate things but two aspects of
one electromagnetic field, which mix when you change frame. For speeds ![v](faradays-law.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> much smaller than the speed
of light ![c](faradays-law.assets/eq-inline/84a516841b.svg)<!--m:c--> (![3.0 times 10^8 m times s^-1](faradays-law.assets/eq-inline/0fcea4f44b.svg)<!--m:3.0\times10^8\ \mathrm{m\cdot s^{-1}}-->), an observer moving with velocity ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> measures:

![in a frame moving with velocity v, the electric field is E prime equals E plus v cross B, for speeds much less than c](faradays-law.assets/eq-frame.svg)

Apply it to the rod of §9. In the lab, ![E = 0](faradays-law.assets/eq-inline/0fa718d368.svg)<!--m:\vec{E} = 0--> and the rod's charges feel ![q v times B](faradays-law.assets/eq-inline/2365d1224f.svg)<!--m:q\vec{v}\times\vec{B}-->. Ride
along with the rod and the charges are at rest — no magnetic force — but you now measure an electric
field ![E ' = v times B](faradays-law.assets/eq-inline/318411ef4d.svg)<!--m:\vec{E}\,' = \vec{v}\times\vec{B}-->, which pushes them with exactly the same force. What one observer
calls motional EMF, another calls transformer EMF. The flux rule's single formula for both was a hint
that the two were one thing all along.

## 15 Applications

### 15.1 The generator

Turn a flat coil of ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns and area ![A](faradays-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> (in ![m^2](faradays-law.assets/eq-inline/fd25baa3e2.svg)<!--m:\mathrm{m^2}-->) at constant angular speed ![omega](faradays-law.assets/eq-inline/73b077a63e.svg)<!--m:\omega--> in a
uniform field ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B-->, about an axle at right angles to the field. Measure the angle ![theta](faradays-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> between the
coil's normal and the field from the moment they are aligned. The angle grows steadily, and the flux
follows §3:

![the angle between n-hat and B grows steadily: theta equals omega t, so Phi_B equals B A cos omega t](faradays-law.assets/eq-gen-1.svg)

![A rectangular loop rotates in a uniform field between two pole pieces; the bundle of field lines threading it widens and narrows, and seen along the field its shadow grows, vanishes and reverses each turn](faradays-law.assets/fig-16-anim.svg)

_In (a) the bright field lines are the ones threading the loop: a full bundle face-on, none edge-on.
In (b), looking along the field, the loop's shadow — its effective area — swells, vanishes and comes
back reversed (green) each turn. The EMF is largest at the edge-on moment, when the shadow is
changing fastest, not when it is biggest._

Apply Faraday's law for ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns. ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B-->, ![A](faradays-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> and ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> are constants and come out of the derivative:

![EMF equals minus N d by dt of B A cos omega t, equals minus N B A d by dt of cos omega t](faradays-law.assets/eq-gen-2.svg)

Now the derivative of ![cos ( omega t)](faradays-law.assets/eq-inline/b4caf747fe.svg)<!--m:\cos(\omega t)-->. Its argument is itself a function of time, so use the chain
rule: differentiate the cosine (giving minus the sine), then multiply by the derivative of the
argument, ![d( omega t)/dt = omega](faradays-law.assets/eq-inline/7c6661e65d.svg)<!--m:d(\omega t)/dt = \omega-->:

![by the chain rule, d by dt of cos omega t equals minus sin omega t times omega](faradays-law.assets/eq-gen-3.svg)

The two minus signs cancel:

![so EMF equals N B A omega sin omega t](faradays-law.assets/eq-gen-4.svg)

A rotating coil in a steady field is a sine-wave generator, with peak EMF ![E_pk = NBA omega](faradays-law.assets/eq-inline/a0dec257bb.svg)<!--m:\mathcal{E}_{pk} = NBA\omega-->.
The RMS value (the steady voltage that would heat a resistor equally; for a sine it is the peak over
![sqrt 2](faradays-law.assets/eq-inline/6d0fdf0909.svg)<!--m:\sqrt2-->, derived in [../signals/ac-and-rms.md](../signals/ac-and-rms.md)) follows. For a modest
coil at mains frequency:

![with N 100, B 0.2 tesla, A 0.01 square metres, omega 2 pi times 50, the peak EMF is 62.8 volts and the RMS is 44.4 volts](faradays-law.assets/eq-gen-worked.svg)

![Flux through the rotating loop follows cos omega t while the EMF follows sin omega t: the EMF peaks when the flux passes through zero and is zero when the flux peaks](faradays-law.assets/fig-17.svg)

_The flux is a cosine and the EMF a sine: a quarter turn apart. Peak EMF at edge-on, zero EMF at
face-on. Double the speed and both the frequency and the amplitude double, because ![omega](faradays-law.assets/eq-inline/73b077a63e.svg)<!--m:\omega--> appears in
both places._

**Cross-check with the magnetic force.** Take one rectangular turn with the two sides parallel to the
axle of length ![l](faradays-law.assets/eq-inline/07c342be6e.svg)<!--m:l-->, a width ![w](faradays-law.assets/eq-inline/aff024fe4a.svg)<!--m:w--> apart (so ![A = lw](faradays-law.assets/eq-inline/6541997453.svg)<!--m:A = lw-->). Each of those sides moves on a circle of radius
![w/2](faradays-law.assets/eq-inline/84867ebdfc.svg)<!--m:w/2--> at speed ![omega w/2](faradays-law.assets/eq-inline/a8d8e4c15c.svg)<!--m:\omega w/2-->; only the component of its velocity across the field,
![( omega w/2) sin omega t](faradays-law.assets/eq-inline/292b250006.svg)<!--m:(\omega w/2)\sin\omega t-->, gives a force along the wire (the end pieces' forces point across the wire and do nothing). Each
side contributes ![Bl](faradays-law.assets/eq-inline/0476abf003.svg)<!--m:Bl--> times that, and the two sides add round the loop:

![each long side moves at omega w over 2 and cuts the field with the component sin omega t; two sides give 2 B l omega w over 2 sin omega t, which is B A omega sin omega t](faradays-law.assets/eq-gen-check.svg)

Times ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns, the same ![NBA omega sin omega t](faradays-law.assets/eq-inline/d11888fc9f.svg)<!--m:NBA\omega\sin\omega t-->. Practical alternators usually do it the other way
round — spin the magnet and keep the heavy, high-current windings still — which by §14 changes
nothing.

### 15.2 The transformer

Wind two coils on one closed core, as Faraday did on his ring. The core guides essentially all the
flux ![Phi_B](faradays-law.assets/eq-inline/df96567662.svg)<!--m:\Phi_B--> through both, so each turn of either winding sees the same ![d Phi_B/dt](faradays-law.assets/eq-inline/3dab0b0f34.svg)<!--m:d\Phi_B/dt-->. With ![N_p](faradays-law.assets/eq-inline/7bf771758b.svg)<!--m:N_p-->
primary turns (driven by the voltage ![v_p](faradays-law.assets/eq-inline/60ec63eb4d.svg)<!--m:v_p-->) and ![N_s](faradays-law.assets/eq-inline/6d90b858de.svg)<!--m:N_s--> secondary turns (giving ![v_s](faradays-law.assets/eq-inline/85026ab589.svg)<!--m:v_s-->), and using the
circuit sign convention in which the coil's terminal voltage is ![v = - E](faradays-law.assets/eq-inline/044f7bead6.svg)<!--m:v = -\mathcal{E}--> (explained in
[../electromagnetism/electromagnetism.md §8](../electromagnetism/electromagnetism.md#8-lenzs-law--the-sign-and-back-emf)):

![v_p equals N_p d Phi_B by dt and v_s equals N_s d Phi_B by dt, so v_s over v_p equals N_s over N_p](faradays-law.assets/eq-xfmr.svg)

![A transformer core carries an alternating flux that builds up one way round the core, falls, and builds up the other way; current in the secondary circuit reverses in step with the rate of change of that flux](faradays-law.assets/fig-18-anim.svg)

_The flux builds up one way round the core, collapses, and builds up the other way. The secondary
current follows the rate of change, so it is strongest as the flux passes through zero — the same
quarter-cycle offset as the generator._

With ![12 V](faradays-law.assets/eq-inline/fe6e8c66b0.svg)<!--m:12\ \mathrm{V}--> across 4 primary turns, every turn on the core carries ![3 V](faradays-law.assets/eq-inline/28587cfc39.svg)<!--m:3\ \mathrm{V}-->, and a
108-turn secondary gives ![324 V](faradays-law.assets/eq-inline/01c1be4d23.svg)<!--m:324\ \mathrm{V}-->. Faraday's law also explains why a transformer cannot pass DC:
a constant voltage means a constant ![d Phi_B/dt](faradays-law.assets/eq-inline/3dab0b0f34.svg)<!--m:d\Phi_B/dt-->, so the flux ramps without limit until the core
saturates. The full story — current ratio, magnetising current, core size against frequency, losses —
is in [../transformer/](../transformer/).

### 15.3 The inductor

A single coil also links the flux made by its *own* current. Below saturation that flux linkage is
proportional to the current ![I](faradays-law.assets/eq-inline/ca73ab6556.svg)<!--m:I-->, and the constant of proportionality is the **inductance** ![L](faradays-law.assets/eq-inline/d160e0986a.svg)<!--m:L--> (in
henries, ![1 H = 1 Wb times A^-1 = 1 V times s times A^-1](faradays-law.assets/eq-inline/2e03edc1d6.svg)<!--m:1\ \mathrm{H} = 1\ \mathrm{Wb\cdot A^{-1}} = 1\ \mathrm{V\cdot s\cdot A^{-1}}-->). Faraday's law then
gives the coil's self-induced EMF:

![with lambda equal to L I, EMF equals minus d lambda by dt, which equals minus L dI by dt](faradays-law.assets/eq-inductor.svg)

The minus sign is Lenz again: the EMF opposes the change of current — a **back-EMF**. Interrupt
![1 A](faradays-law.assets/eq-inline/4bd22980e0.svg)<!--m:1\ \mathrm{A}--> in ![1 mu s](faradays-law.assets/eq-inline/c9694fb483.svg)<!--m:1\ \mu\mathrm{s}--> in a ![100 mu H](faradays-law.assets/eq-inline/2a88dd19c1.svg)<!--m:100\ \mu\mathrm{H}--> inductor:

![with L 100 microhenries and dI by dt 1 ampere per microsecond, the EMF magnitude is 100 volts](faradays-law.assets/eq-inductor-worked.svg)

That is the inductive kick that destroys unprotected switches. The defining law
![V_L = L dI_L/dt](faradays-law.assets/eq-inline/ffda83ef21.svg)<!--m:V_L = L\,dI_L/dt-->, its ramps and its kick are developed in [../inductor/](../inductor/).

### 15.4 Eddy currents and magnetic braking

In a solid piece of metal there is no wire to choose a loop for you: *every* closed path inside the
metal is a loop, and wherever the flux through it changes, current flows round it. These whirls are
**eddy currents**. By Lenz they always oppose the change, so in a conductor moving past a magnet they
drag it back, and in a transformer core they waste power as heat.

![Eddy currents: a plate moving past a magnet pole carries one current whirl where it enters the field and an opposite one where it leaves, and the force on them drags the plate; slots and laminations break the whirls up](faradays-law.assets/fig-19.svg)

_Where the plate enters the field the flux is rising and the whirl runs one way; where it leaves, the
flux is falling and the whirl runs the other. Under the pole both run the same way, and the force on
that current points backwards. Slots and insulated laminations cut the whirls into thin strips that
can carry little current._

**Braking.** The sliding bar of §9 is the model: the drag is proportional to ![B^2](faradays-law.assets/eq-inline/0d8784c323.svg)<!--m:B^2--> and to the speed,
and inversely to the resistance of the current path. That is why eddy-current brakes on trains,
roller coasters and exercise bikes are strong at speed, fade gently to nothing as the vehicle slows
(they cannot hold it stationary), and never wear, since nothing touches. A strong magnet dropped
down a thick copper tube falls slowly at a steady speed for the same reason: the faster it falls, the
harder the tube's eddy currents push back.

**Losses.** In a transformer core the changing flux drives eddy currents through the core metal. The
EMF round each eddy path grows with the frequency ![f](faradays-law.assets/eq-inline/4a0a19218e.svg)<!--m:f--> (in Hz) and the peak flux density ![B](faradays-law.assets/eq-inline/a35484dcdd.svg)<!--m:\hat{B}-->, the
power with the square of that EMF over the resistance; for sheets of thickness ![t](faradays-law.assets/eq-inline/8efd86fb78.svg)<!--m:t--> (in metres) and
resistivity ![rho](faradays-law.assets/eq-inline/c77a25750c.svg)<!--m:\rho--> (in ![Omega times m](faradays-law.assets/eq-inline/93c1177aa1.svg)<!--m:\Omega\cdot\mathrm{m}-->):

![eddy loss per volume grows as f squared B peak squared t squared over rho](faradays-law.assets/eq-eddy.svg)

Hence laminated silicon steel at ![50 Hz](faradays-law.assets/eq-inline/6233625054.svg)<!--m:50\ \mathrm{Hz}-->, and ferrite (a ceramic with enormous ![rho](faradays-law.assets/eq-inline/c77a25750c.svg)<!--m:\rho-->) at
tens of kilohertz; the classical formula and numbers are in
[../transformer/transformer.md §15](../transformer/transformer.md#15-core-loss--the-ceiling-on-frequency).

### 15.5 The induction cooktop

An induction hob is a transformer whose secondary is the pan. A flat spiral coil under a glass plate
carries current from an inverter at ![20](faradays-law.assets/eq-inline/91032ad7bb.svg)<!--m:20--> to ![100 kHz](faradays-law.assets/eq-inline/b87ad8b7a6.svg)<!--m:100\ \mathrm{kHz}-->. Its alternating flux, steered upward by
ferrite bars, passes through the steel base of the pan, and Faraday's law drives eddy currents round
inside the base. The pan's own resistance is the load; it heats from within. The glass carries no
current and only gets warm from the pan.

![Induction cooktop cross-section: a flat coil under a glass plate carries high-frequency current; its alternating flux passes up through the steel pan base and induces eddy currents in a thin skin that heat the pan directly](faradays-law.assets/fig-20.svg)

_The coil is the primary, the pan base a one-turn short-circuited secondary. At these frequencies the
eddy currents crowd into a thin layer at the bottom surface of the pan, and how thin that layer is
decides which pans work._

**Why steel and not copper.** At high frequency the induced currents flow only in a surface layer
whose thickness is the **skin depth** ![delta](faradays-law.assets/eq-inline/3a6a16552e.svg)<!--m:\delta--> (in metres), which depends on the resistivity ![rho](faradays-law.assets/eq-inline/c77a25750c.svg)<!--m:\rho-->,
the angular frequency ![omega](faradays-law.assets/eq-inline/73b077a63e.svg)<!--m:\omega--> and the permeability ![mu](faradays-law.assets/eq-inline/3a4e56595d.svg)<!--m:\mu--> (in ![H times m^-1](faradays-law.assets/eq-inline/94372b463a.svg)<!--m:\mathrm{H\cdot m^{-1}}-->; ![mu_r](faradays-law.assets/eq-inline/de4a3aca4d.svg)<!--m:\mu_r--> is the
material's relative permeability, ![mu_0 = 4 pi times 10^-7 H times m^-1](faradays-law.assets/eq-inline/6965637556.svg)<!--m:\mu_0 = 4\pi\times10^{-7}\ \mathrm{H\cdot m^{-1}}-->). The result comes
from combining Faraday's law with Ampère's law inside a conductor; here it is stated, not derived:

![the skin depth delta equals the square root of 2 rho over omega mu](faradays-law.assets/eq-skin.svg)

For carbon steel (![rho approx 1.5 times 10^-7 Omega times m](faradays-law.assets/eq-inline/c6c712e029.svg)<!--m:\rho \approx 1.5\times10^{-7}\ \Omega\cdot\mathrm{m}-->, effective ![mu_r approx 100](faradays-law.assets/eq-inline/b055b41a54.svg)<!--m:\mu_r \approx 100-->) and
for copper (![rho = 1.7 times 10^-8 Omega times m](faradays-law.assets/eq-inline/98ad04fc03.svg)<!--m:\rho = 1.7\times10^{-8}\ \Omega\cdot\mathrm{m}-->, ![mu_r = 1](faradays-law.assets/eq-inline/a9dcb15318.svg)<!--m:\mu_r = 1-->) at ![25 kHz](faradays-law.assets/eq-inline/4f19ceaf24.svg)<!--m:25\ \mathrm{kHz}-->:

![for steel at 25 kilohertz, delta is about 0.12 millimetres](faradays-law.assets/eq-skin-steel.svg)

![for copper at 25 kilohertz, delta is about 0.41 millimetres](faradays-law.assets/eq-skin-copper.svg)

The resistance of the heated skin goes as ![rho/delta](faradays-law.assets/eq-inline/636ba2ba78.svg)<!--m:\rho/\delta--> (resistivity over the thickness the current
gets to use):

![the skin resistance goes as rho over delta; for steel it is about 30 times that of copper](faradays-law.assets/eq-skin-ratio.svg)

A steel base presents about thirty times the resistance, which the hob's electronics can drive
efficiently; a copper or aluminium pan is almost a short circuit, takes huge current for little heat,
and a basic hob refuses it. (Steel also adds hysteresis loss, the magnetic friction of its domains,
which helps further.)

### 15.6 More of the same law

- **Electromagnetic flowmeter.** A conducting liquid flowing at speed ![v](faradays-law.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> through a pipe of diameter
  ![D](faradays-law.assets/eq-inline/50c9e8d5fc.svg)<!--m:D--> across a field ![B](faradays-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> is a moving conductor; electrodes on opposite walls read the motional EMF
  ![BDv](faradays-law.assets/eq-inline/5b434bdb49.svg)<!--m:BDv-->, with no moving parts in the flow:
  ![in an electromagnetic flowmeter the EMF across the pipe is B D v; with 0.05 tesla, 0.1 metre and 2 metres per second it is 10 millivolts](faradays-law.assets/eq-flowmeter.svg)
- **Guitar pickups and dynamic microphones.** A vibrating steel string (or a coil on a moving
  diaphragm) changes the flux through a coil; the EMF is proportional to the velocity of the vibration.
- **Wireless charging** of phones and toothbrushes: two coils sharing an alternating flux, a
  transformer with an air gap.
- **Metal detectors and card readers:** a changing flux induces eddy currents in a buried coin, or the
  stripe's magnetised pattern swept past a head induces an EMF.
- **Induction motors:** a rotating field induces currents in the rotor bars, and the force on those
  currents drags the rotor round after the field.

## 16 What this costs you

- **Every loop is a secondary winding.** Any closed loop near a changing current picks up an EMF
  (between neighbouring circuits this unwanted coupling is called crosstalk). A
  ![1 cm^2](faradays-law.assets/eq-inline/9cb04c93d8.svg)<!--m:1\ \mathrm{cm^2}--> loop of PCB track near a switching inductor whose stray field changes at
  ![10^3 T times s^-1](faradays-law.assets/eq-inline/26c6208060.svg)<!--m:10^3\ \mathrm{T\cdot s^{-1}}--> (for example ![10 mT](faradays-law.assets/eq-inline/31d3ab60e7.svg)<!--m:10\ \mathrm{mT}--> in ![10 mu s](faradays-law.assets/eq-inline/3b02ac376d.svg)<!--m:10\ \mu\mathrm{s}-->) collects
  ![a 1 square centimetre loop in a field changing at 1000 tesla per second picks up 0.1 volts](faradays-law.assets/eq-pcb-loop.svg)
  — enough to upset a logic input or a sensitive measurement. Small loop areas, return paths directly
  under signal tracks, and shielded or toroidal magnetics are the defence.
- **Kirchhoff's voltage law is an approximation.** It holds for loops that enclose no changing flux.
  Around a loop that does, the voltages do not sum to zero; they sum to the EMF (Figure 126). Circuit
  theory survives by booking that EMF as the terminal voltage of an inductor or transformer winding,
  but a measurement loop you did not draw on the schematic is not booked anywhere.
- **Lenz charges for every watt.** A generator's output current makes a drag torque on the rotor;
  the electrical power out is paid for, watt for watt, by mechanical power in, plus losses. There is
  no clever winding that avoids it.
- **Eddy currents heat what you did not want heated.** Transformer and motor cores need laminations
  or ferrite; mounting brackets, shields and enclosures near strong AC fields get warm; the cost rises
  with the square of frequency.
- **The inductive kick.** Any attempt to stop a current quickly in an inductance produces an EMF
  ![L dI/dt](faradays-law.assets/eq-inline/a254d5e686.svg)<!--m:L\,dI/dt--> that can reach hundreds of volts. Every relay coil and switching converter needs a path
  (a diode, a snubber, a clamp) for that energy.
- **The flux rule can mislead.** With sliding contacts, thick conductors, or a "loop" that does not
  move with the metal, counting flux can give the wrong EMF (§13). The force on the actual charges
  never does.
- **Only change counts.** A large static field does nothing for a stationary circuit. A sensor based
  on induction cannot measure a steady field (a Hall sensor, §12, is needed for that), and a
  generator gives nothing when it stops.

## 17 Every symbol in one place

| Symbol | Name | Unit | Defined in |
|---|---|---|---|
| ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> | magnetic field (flux density) | ![T = N times A^-1 times m^-1 = V times s times m^-2](faradays-law.assets/eq-inline/57efa28215.svg)<!--m:\mathrm{T} = \mathrm{N\cdot A^{-1}\cdot m^{-1}} = \mathrm{V\cdot s\cdot m^{-2}}--> | §1 |
| ![E](faradays-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> | electric field | ![V times m^-1 = N times C^-1](faradays-law.assets/eq-inline/affd448530.svg)<!--m:\mathrm{V\cdot m^{-1}} = \mathrm{N\cdot C^{-1}}--> | §1, [../coulombs-law/](../coulombs-law/) |
| ![q](faradays-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->, ![v](faradays-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}-->, ![F](faradays-law.assets/eq-inline/b0682d270b.svg)<!--m:\vec{F}--> | charge, its velocity, force on it | ![C](faradays-law.assets/eq-inline/b03ab2bc06.svg)<!--m:\mathrm{C}-->, ![m times s^-1](faradays-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}-->, ![N](faradays-law.assets/eq-inline/4aa469810b.svg)<!--m:\mathrm{N}--> | §1 |
| ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}-->, ![A](faradays-law.assets/eq-inline/427ea0eb71.svg)<!--m:\vec{A}-->, ![d A](faradays-law.assets/eq-inline/192de07f3e.svg)<!--m:d\vec{A}--> | unit normal, area vector, area element | (none), ![m^2](faradays-law.assets/eq-inline/fd25baa3e2.svg)<!--m:\mathrm{m^2}-->, ![m^2](faradays-law.assets/eq-inline/fd25baa3e2.svg)<!--m:\mathrm{m^2}--> | §3 |
| ![theta](faradays-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> | angle between ![B](faradays-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> and ![n](faradays-law.assets/eq-inline/4b6559edf9.svg)<!--m:\hat{n}--> | ![rad](faradays-law.assets/eq-inline/a8553a890a.svg)<!--m:\mathrm{rad}--> | §3 |
| ![Sigma](faradays-law.assets/eq-inline/cb5615b3fc.svg)<!--m:\Sigma-->, ![partial Sigma](faradays-law.assets/eq-inline/67dc6e421e.svg)<!--m:\partial\Sigma--> | a surface, and its rim (the loop) | | §3 |
| ![Phi_B](faradays-law.assets/eq-inline/df96567662.svg)<!--m:\Phi_B--> | magnetic flux through one loop | ![Wb = T times m^2 = V times s](faradays-law.assets/eq-inline/4934f713b0.svg)<!--m:\mathrm{Wb} = \mathrm{T\cdot m^2} = \mathrm{V\cdot s}--> | §3, §4 |
| ![N](faradays-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> | number of turns | (count) | §4 |
| ![lambda](faradays-law.assets/eq-inline/b3931f1ce2.svg)<!--m:\lambda--> | flux linkage, ![N Phi_B](faradays-law.assets/eq-inline/406d115654.svg)<!--m:N\Phi_B--> | weber-turns (![Wb](faradays-law.assets/eq-inline/ad708422a4.svg)<!--m:\mathrm{Wb}-->) | §4 |
| ![E](faradays-law.assets/eq-inline/2ac770400e.svg)<!--m:\mathcal{E}--> | EMF: work per unit charge once round a loop | ![V = J times C^-1](faradays-law.assets/eq-inline/79a1148316.svg)<!--m:\mathrm{V} = \mathrm{J\cdot C^{-1}}--> | §5 |
| ![d l](faradays-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> | element of the loop, along the direction of travel | ![m](faradays-law.assets/eq-inline/592112337d.svg)<!--m:\mathrm{m}--> | §5 |
| ![R](faradays-law.assets/eq-inline/06576556d1.svg)<!--m:R-->, ![I](faradays-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> | resistance, current | ![Omega = V times A^-1](faradays-law.assets/eq-inline/006ee6ee4b.svg)<!--m:\Omega = \mathrm{V\cdot A^{-1}}-->, ![A = C times s^-1](faradays-law.assets/eq-inline/b5bc7ffcd1.svg)<!--m:\mathrm{A} = \mathrm{C\cdot s^{-1}}--> | §5 |
| ![Q](faradays-law.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> | charge driven round the circuit | ![C](faradays-law.assets/eq-inline/b03ab2bc06.svg)<!--m:\mathrm{C}--> | §6 |
| ![l](faradays-law.assets/eq-inline/07c342be6e.svg)<!--m:l-->, ![x](faradays-law.assets/eq-inline/11f6ad8ec5.svg)<!--m:x-->, ![v](faradays-law.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> | rod length, bar position, bar speed | ![m](faradays-law.assets/eq-inline/592112337d.svg)<!--m:\mathrm{m}-->, ![m](faradays-law.assets/eq-inline/592112337d.svg)<!--m:\mathrm{m}-->, ![m times s^-1](faradays-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}--> | §9 |
| ![omega](faradays-law.assets/eq-inline/73b077a63e.svg)<!--m:\omega-->, ![f](faradays-law.assets/eq-inline/4a0a19218e.svg)<!--m:f--> | angular frequency, frequency | ![rad times s^-1](faradays-law.assets/eq-inline/da19396712.svg)<!--m:\mathrm{rad\cdot s^{-1}}-->, ![Hz = s^-1](faradays-law.assets/eq-inline/301a1fba59.svg)<!--m:\mathrm{Hz} = \mathrm{s^{-1}}--> | §10 |
| ![nabla times E](faradays-law.assets/eq-inline/2596fbd332.svg)<!--m:\nabla\times\vec{E}--> | curl: circulation per unit area | ![V times m^-2](faradays-law.assets/eq-inline/0c8bb130d3.svg)<!--m:\mathrm{V\cdot m^{-2}}--> | §11 |
| ![nabla times B](faradays-law.assets/eq-inline/ca5e9aada4.svg)<!--m:\nabla\cdot\vec{B}--> | divergence: outflow per unit volume | ![T times m^-1](faradays-law.assets/eq-inline/3b0e30fc74.svg)<!--m:\mathrm{T\cdot m^{-1}}--> | §12 |
| ![v_c](faradays-law.assets/eq-inline/d237395046.svg)<!--m:\vec{v}_c-->, ![v_d](faradays-law.assets/eq-inline/c895457db9.svg)<!--m:\vec{v}_d--> | conductor (rim) velocity, drift velocity | ![m times s^-1](faradays-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}--> | §12 |
| ![L](faradays-law.assets/eq-inline/d160e0986a.svg)<!--m:L--> | inductance, ![lambda/I](faradays-law.assets/eq-inline/c7b27e1d5f.svg)<!--m:\lambda/I--> | ![H = Wb times A^-1 = V times s times A^-1](faradays-law.assets/eq-inline/23c74a32c7.svg)<!--m:\mathrm{H} = \mathrm{Wb\cdot A^{-1}} = \mathrm{V\cdot s\cdot A^{-1}}--> | §15.3 |
| ![rho](faradays-law.assets/eq-inline/c77a25750c.svg)<!--m:\rho-->, ![mu](faradays-law.assets/eq-inline/3a4e56595d.svg)<!--m:\mu-->, ![delta](faradays-law.assets/eq-inline/3a6a16552e.svg)<!--m:\delta--> | resistivity, permeability, skin depth | ![Omega times m](faradays-law.assets/eq-inline/93c1177aa1.svg)<!--m:\Omega\cdot\mathrm{m}-->, ![H times m^-1](faradays-law.assets/eq-inline/94372b463a.svg)<!--m:\mathrm{H\cdot m^{-1}}-->, ![m](faradays-law.assets/eq-inline/592112337d.svg)<!--m:\mathrm{m}--> | §15.4, §15.5 |

## 18 Sources and cross-links

- **Primary source for structure and history:** *Faraday's law of induction*, Wikipedia
  (https://en.wikipedia.org/wiki/Faraday%27s_law_of_induction), read in full: history (Ørsted,
  Faraday's ring of 29 August 1831, the moving magnet, Henry, Lenz, Neumann, Weber, Felici, Maxwell,
  Heaviside, Einstein), the flux rule, motional and transformer EMF, the left-hand rule, the
  Maxwell–Faraday equation and its Biot–Savart-like solution, the derivation via the Leibniz rule and
  the Hall-effect exception, the limitations (Faraday's disk, the rocking plates), and the
  relativity discussion. All text here is a fresh explanation; the worked numbers are new.
- **The figures of three cases, the surface and its normal, and the moving rod** follow the article's
  illustrations of the flux rule in three cases, the Kelvin–Stokes surface, and the rod moving through
  a field (Figures 121, 117 and 122 here).
- D. J. Griffiths, *Introduction to Electrodynamics*, chapter 7 (motional EMF, the induced electric
  field, the flux rule and its exceptions); R. P. Feynman, R. B. Leighton and M. Sands, *The Feynman
  Lectures on Physics*, vol. II, chapter 17 (the rocking plates and the "exceptions to the flux
  rule"); A. Zangwill, *Modern Electrodynamics* (the Leibniz flux theorem).
- A. Einstein, *Zur Elektrodynamik bewegter Körper* (On the Electrodynamics of Moving Bodies), 1905,
  first paragraph.
- **The two companion field laws:** [../coulombs-law/](../coulombs-law/) (electric field, voltage,
  conservative fields) and [../amperes-law/](../amperes-law/) (the magnetic field of currents).
- **The overview that ties them to circuits:** [../electromagnetism/](../electromagnetism/) — its
  §3 (flux and linkage), §6 (voltage from changing flux, volt-seconds), §8 (Lenz and the circuit sign
  convention) and §13 (why switching faster shrinks a transformer).
- **The components that are this law:** [../inductor/](../inductor/) and
  [../transformer/](../transformer/).
- **Sine waves and RMS:** [../signals/ac-and-rms.md](../signals/ac-and-rms.md).
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
