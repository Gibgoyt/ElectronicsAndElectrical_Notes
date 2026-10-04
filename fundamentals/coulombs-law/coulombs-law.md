# Coulomb's law — electric charge, the force between charges, and the electric field

Every circuit in this tree pushes electric charge around, and every law in it rests on one
experimental fact: two charges push or pull on each other with a force that falls as the square of
the distance between them. This document starts from nothing. It says what charge is and how it is
measured, defines current as charge per second, states Coulomb's law with every symbol and unit,
and then builds the electric field, voltage, Gauss's law and the capacitance of two plates from it,
step by step and with worked numbers. It ends by asking why magnetism has no charge of its own,
which is where [Ampère's law](../amperes-law/) takes over.

**Contents**

1. [Electric charge](#1-electric-charge)
2. [Current is charge in motion](#2-current-is-charge-in-motion)
3. [Coulomb's law — the force between two point charges](#3-coulombs-law--the-force-between-two-point-charges)
4. [Worked numbers — how strong the electric force is](#4-worked-numbers--how-strong-the-electric-force-is)
5. [The vector form and superposition](#5-the-vector-form-and-superposition)
6. [The electric field](#6-the-electric-field)
7. [Work, potential and voltage](#7-work-potential-and-voltage)
8. [Gauss's law, derived from Coulomb's law](#8-gausss-law-derived-from-coulombs-law)
9. [Parallel plates and capacitance](#9-parallel-plates-and-capacitance)
10. [Why there is no magnetic charge](#10-why-there-is-no-magnetic-charge)
11. [What this costs you — the limits of Coulomb's law](#11-what-this-costs-you--the-limits-of-coulombs-law)
12. [Sources and cross-links](#12-sources-and-cross-links)

> **The thesis in one line**
>
> Like charges repel and unlike charges attract, with a force proportional to each charge and to the
> inverse square of their distance. The field, the volt, Gauss's law and the capacitor all follow
> from this one law:

![F equals 1 over 4 pi epsilon_0 times the magnitude of q1 q2 over r squared](coulombs-law.assets/eq-coulomb-full.svg)

---

## 1 Electric charge

**What it is.** Charge is a property that some particles carry, as they carry mass. It shows up in
only one way: charged objects push or pull on one another. Rub a plastic rod with wool and it
attracts scraps of paper. Two rods rubbed the same way push each other apart. A rubbed plastic rod and
a rubbed glass rod pull together. Experiments like these show that there are exactly two kinds of
charge, and that

- **like charges repel** — two of the same kind push apart;
- **unlike charges attract** — one of each kind pull together.

Benjamin Franklin named the two kinds **positive** (+) and **negative** (−). The names fit the
arithmetic: equal amounts of the two kinds cancel, so an object with equal amounts of each acts as
if it had no charge at all. We call it **neutral**. Ordinary matter is built of protons, which
carry positive charge, electrons, which carry an equal amount of negative charge, and neutrons,
which carry none. A rubbed rod is charged because it has gained or lost a tiny fraction of its
electrons.

**Quantisation.** Charge comes in whole-number multiples of one smallest amount, the
**elementary charge** ![e](coulombs-law.assets/eq-inline/58e6b3a414.svg)<!--m:e-->. A proton carries ![+e](coulombs-law.assets/eq-inline/b67b9a5e15.svg)<!--m:+e--> and an electron ![-e](coulombs-law.assets/eq-inline/2360917b93.svg)<!--m:-e-->. Any free object's charge
![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> is therefore

![q equals n times e, where n is a whole number](coulombs-law.assets/eq-quantised.svg)

- ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> — the charge of the object, in coulombs (C, defined next);
- ![n](coulombs-law.assets/eq-inline/d1854cae89.svg)<!--m:n--> — a whole number, the excess of protons over electrons (negative if electrons are in excess);
- ![e](coulombs-law.assets/eq-inline/58e6b3a414.svg)<!--m:e--> — the elementary charge, in coulombs.

(Quarks, inside protons and neutrons, carry ![plus-minus 1 over 3 e](coulombs-law.assets/eq-inline/1c2efaf475.svg)<!--m:\pm\tfrac{1}{3} e--> and ![plus-minus 2 over 3 e](coulombs-law.assets/eq-inline/9575466e63.svg)<!--m:\pm\tfrac{2}{3} e-->, but they are never
found alone. Every free particle's charge is a whole multiple of ![e](coulombs-law.assets/eq-inline/58e6b3a414.svg)<!--m:e-->.)

**The coulomb.** The SI unit of charge is the **coulomb**, symbol C. Since the 2019 redefinition of
the SI, the coulomb is fixed by giving the elementary charge an exact value:

![e equals 1.602176634 times 10 to the minus 19 coulombs, exactly](coulombs-law.assets/eq-e-value.svg)

Turn that round to see how many elementary charges make one coulomb:

![one coulomb equals one over e elementary charges, about 6.24 times 10 to the 18](coulombs-law.assets/eq-one-coulomb.svg)

So a coulomb is about six million million million electrons' worth of charge. That sounds like a lot,
and §4 shows that, measured as a force, it is very much more than it sounds.

**Conservation.** The total charge of an isolated system never changes. Charge can move from one
object to another (that is what rubbing does), and particles can be created or destroyed, but only
in combinations whose charges add to zero:

![the sum of all charges in an isolated system is the same before and after any process](coulombs-law.assets/eq-conservation.svg)

- ![q_i](coulombs-law.assets/eq-inline/e7a8ac7463.svg)<!--m:q_i--> — the charge of object number ![i](coulombs-law.assets/eq-inline/042dc4512f.svg)<!--m:i-->, in coulombs; the sum runs over every object in the system.

For example, a high-energy photon ![gamma](coulombs-law.assets/eq-inline/67833ee201.svg)<!--m:\gamma--> (no charge) can turn into an electron and a positron,
the electron's antiparticle with charge ![+e](coulombs-law.assets/eq-inline/b67b9a5e15.svg)<!--m:+e-->:

![a gamma photon becomes an electron and a positron; charge 0 equals minus e plus e](coulombs-law.assets/eq-pair.svg)

No experiment has ever seen this rule broken. It is the reason Kirchhoff's current law holds: charge
cannot appear or vanish at a junction of wires, so what flows in must flow out (see
[../electromagnetism/electromagnetism.md §1](../electromagnetism/electromagnetism.md#1-charge-current-and-the-electric-field)).

## 2 Current is charge in motion

A circuit does not care how much charge exists. It cares how fast charge **moves** past a point.
Pick a plane cutting across a wire and count the charge ![Delta Q](coulombs-law.assets/eq-inline/9fd1467576.svg)<!--m:\Delta Q--> (in coulombs) that crosses it
during a time ![Delta t](coulombs-law.assets/eq-inline/fdecfb4216.svg)<!--m:\Delta t--> (in seconds). The **average current** is the ratio:

![average current I equals delta Q over delta t](coulombs-law.assets/eq-current-avg.svg)

The current at one instant is the limit of that ratio as the counting time shrinks to zero. That
limit is, by definition, the derivative of the charge that has crossed, ![Q(t)](coulombs-law.assets/eq-inline/98c5567c66.svg)<!--m:Q(t)-->, with respect to time:

![I equals dQ by dt, the limit of delta Q over delta t as delta t goes to zero](coulombs-law.assets/eq-current-def.svg)

- ![I](coulombs-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> — the current, in amperes (A);
- ![Q](coulombs-law.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> — the charge that has crossed the plane up to time ![t](coulombs-law.assets/eq-inline/8efd86fb78.svg)<!--m:t-->, in coulombs;
- ![t](coulombs-law.assets/eq-inline/8efd86fb78.svg)<!--m:t--> — time, in seconds (s).

The unit follows directly from the definition. One ampere is one coulomb crossing the plane every
second. Read backwards, a coulomb is the charge one ampere delivers in one second, an
**ampere-second**:

![one ampere equals one coulomb per second, written 1 C s to the minus 1; so one coulomb equals one ampere second](coulombs-law.assets/eq-ampere.svg)

![Electrons drifting through a wire are counted as they cross a plane; ten packets of 0.1 coulomb in one second is one ampere](coulombs-law.assets/fig-01-anim.svg)

_Each dot is a packet of 0.1 C. Ten of them cross the plane during each one-second sweep of the
clock, so 1 C crosses per second, and that is 1 A. The animation is slowed down: real electrons in a
wire drift far more slowly still, as the worked number below shows._

**How many electrons is one ampere?** Divide the charge per second by the charge of one electron:

![at one ampere, electrons per second equals 1 over e, about 6.24 times 10 to the 18 per second](coulombs-law.assets/eq-electrons-per-second.svg)

**How fast do they move?** Surprisingly slowly. Let the wire have cross-section area ![A](coulombs-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> (in
![m^2](coulombs-law.assets/eq-inline/fd25baa3e2.svg)<!--m:\mathrm{m^2}-->), let it hold ![n](coulombs-law.assets/eq-inline/d1854cae89.svg)<!--m:n--> free electrons per cubic metre (the carrier density, in
![m^-3](coulombs-law.assets/eq-inline/3c42163f29.svg)<!--m:\mathrm{m^{-3}}-->), and let them drift along the wire at an average speed ![v](coulombs-law.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> (in
![m times s^-1](coulombs-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}-->). In a short time ![dt](coulombs-law.assets/eq-inline/f7e6632892.svg)<!--m:dt--> every carrier within a distance ![v dt](coulombs-law.assets/eq-inline/9ad9d040f1.svg)<!--m:v\,dt--> of the
plane crosses it. Those carriers fill a slab of volume ![A v dt](coulombs-law.assets/eq-inline/5942f14f20.svg)<!--m:A\,v\,dt-->, so their number ![dN](coulombs-law.assets/eq-inline/3055577a45.svg)<!--m:dN--> is

![in time dt the carriers within a length v dt of the plane cross it; their number is n A v dt](coulombs-law.assets/eq-drift-volume.svg)

Each carries charge ![e](coulombs-law.assets/eq-inline/58e6b3a414.svg)<!--m:e--> (only its size matters here), so the charge that crosses is

![the charge crossing is dQ equals e times dN equals n e A v dt](coulombs-law.assets/eq-drift-charge.svg)

Divide both sides by ![dt](coulombs-law.assets/eq-inline/f7e6632892.svg)<!--m:dt-->, which is the definition of current above:

![dividing by dt, I equals n e A v](coulombs-law.assets/eq-drift-current.svg)

Copper has about ![n = 8.5 times 10^28 m^-3](coulombs-law.assets/eq-inline/a9f77f5ba0.svg)<!--m:n = 8.5\times10^{28}\ \mathrm{m^{-3}}--> free electrons. For 1 A in a wire of
cross-section ![1 mm^2 = 10^-6 m^2](coulombs-law.assets/eq-inline/cd03296883.svg)<!--m:1\ \mathrm{mm^2} = 10^{-6}\ \mathrm{m^2}-->:

![v equals I over n e A equals 1 over 8.5 times 10 to the 28 times 1.602 times 10 to the minus 19 times 10 to the minus 6, about 7.3 times 10 to the minus 5 metres per second](coulombs-law.assets/eq-drift-worked.svg)

That is less than a tenth of a millimetre per second. An electron would take nearly four hours to
drift one metre. A lamp lights the instant the switch closes because the push travels along
the wire at close to the speed of light. Every electron in the wire starts drifting at once; no
electron has to travel from the switch to the lamp.

> **Note —** Current is defined as the flow of **positive** charge. In a metal the carriers are
> electrons, which move the opposite way to the arrow drawn for ![I](coulombs-law.assets/eq-inline/ca73ab6556.svg)<!--m:I-->. The two descriptions give the
> same current, because negative charge moving left changes the charge on each side of the plane
> exactly as positive charge moving right would. Circuit work always uses the conventional
> direction.

## 3 Coulomb's law — the force between two point charges

In 1785 Charles-Augustin de Coulomb hung a light rod from a thin fibre, put a small charged ball on
one end, and brought a second charged ball near it. The twist of the fibre measured the force
between the balls. He found that the force between two small charged bodies

- acts along the straight line joining them;
- is proportional to the product of the two charges;
- falls as the inverse square of the distance between them;
- repels when the charges are alike and attracts when they are unlike.

A **point charge** is a charged body small compared with its distance to the other charges, so that
"the distance between them" has one value. For two point charges, **Coulomb's law** gives the size
of the force as

![F equals k times the magnitude of q1 q2 over r squared](coulombs-law.assets/eq-coulomb.svg)

- ![F](coulombs-law.assets/eq-inline/e69f20e9f6.svg)<!--m:F--> — the size (magnitude) of the force each charge exerts on the other, in newtons (N);
- ![q_1](coulombs-law.assets/eq-inline/63d628baf5.svg)<!--m:q_1-->, ![q_2](coulombs-law.assets/eq-inline/bc09c3b934.svg)<!--m:q_2--> — the two charges, in coulombs, with their signs. The bars ![| times |](coulombs-law.assets/eq-inline/ca54ea32c4.svg)<!--m:|\,\cdot\,|--> take the
  size and drop the sign, because the sign tells you the **direction** (repel or attract), not the size;
- ![r](coulombs-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> — the distance between the two charges, in metres (m);
- ![k](coulombs-law.assets/eq-inline/13fbd79c3d.svg)<!--m:k--> — the **Coulomb constant**, a measured constant of nature that converts "coulombs squared per
  metre squared" into newtons.

The constant is always written in terms of a more basic one, the **permittivity of free space**
![epsilon_0](coulombs-law.assets/eq-inline/961a0cda39.svg)<!--m:\varepsilon_0--> (epsilon-nought):

![k equals 1 over 4 pi epsilon_0, about 8.988 times 10 to the 9 newton square metres per square coulomb](coulombs-law.assets/eq-k-def.svg)

![epsilon_0 equals 8.8541878188 times 10 to the minus 12 square coulombs per newton per square metre](coulombs-law.assets/eq-eps0.svg)

So in full:

![F equals 1 over 4 pi epsilon_0 times the magnitude of q1 q2 over r squared](coulombs-law.assets/eq-coulomb-full.svg)

**Working out the units.** Solve the law for ![k](coulombs-law.assets/eq-inline/13fbd79c3d.svg)<!--m:k-->:

![solve Coulomb's law for k: k equals F r squared over q1 q2](coulombs-law.assets/eq-k-units-1.svg)

Put in the units of each quantity: newtons on top, metres squared on top, coulombs squared below.

![so the unit of k is newton times square metre over square coulomb, N m squared C to the minus 2](coulombs-law.assets/eq-k-units-2.svg)

![epsilon_0](coulombs-law.assets/eq-inline/961a0cda39.svg)<!--m:\varepsilon_0--> is ![1/(4 pi k)](coulombs-law.assets/eq-inline/31fc401d8b.svg)<!--m:1/(4\pi k)-->, and ![4 pi](coulombs-law.assets/eq-inline/63f4e29993.svg)<!--m:4\pi--> is a pure number, so its unit is the reciprocal:

![and epsilon_0 equals 1 over 4 pi k, so its unit is the reciprocal, C squared N to the minus 1 m to the minus 2](coulombs-law.assets/eq-k-units-3.svg)

§9 shows that this same unit can be written as farads per metre,
![F times m^-1](coulombs-law.assets/eq-inline/8fd760381c.svg)<!--m:\mathrm{F\cdot m^{-1}}-->, which is how datasheets and the
[electromagnetism notes](../electromagnetism/electromagnetism.md#1-charge-current-and-the-electric-field)
quote it. It is the same number in a different unit.

> **Note — where the factor of four pi comes from.** The ![4 pi](coulombs-law.assets/eq-inline/63f4e29993.svg)<!--m:4\pi--> looks like clutter, but it is put there on purpose. §8 shows that
> the electric field of a charge spreads over a sphere of area ![4 pi r^2](coulombs-law.assets/eq-inline/79874193ec.svg)<!--m:4\pi r^2-->. Writing ![k](coulombs-law.assets/eq-inline/13fbd79c3d.svg)<!--m:k--> as
> ![1/(4 pi epsilon_0)](coulombs-law.assets/eq-inline/18f4af9063.svg)<!--m:1/(4\pi\varepsilon_0)--> makes the ![4 pi](coulombs-law.assets/eq-inline/63f4e29993.svg)<!--m:4\pi--> cancel in Gauss's law and in the capacitor formula
> ![C = epsilon_0 A/d](coulombs-law.assets/eq-inline/f699fc1a96.svg)<!--m:C = \varepsilon_0 A/d-->. Those are the formulas you use every day, so the ![4 pi](coulombs-law.assets/eq-inline/63f4e29993.svg)<!--m:4\pi--> is moved
> into the law you use least.

**The inverse square.** Because ![r](coulombs-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> is squared, the force is very sensitive to distance:

![F at 2r equals F at r over 4; F at 10 r equals F at r over 100](coulombs-law.assets/eq-inverse-square.svg)

![Two like charges push each other apart and two unlike charges pull together, the force growing as the gap closes](coulombs-law.assets/fig-02-anim.svg)

_Two like charges released from rest fly apart. The push is strongest at the start, when they are
close, so they gain speed fast and then coast. Two unlike charges released far apart barely move at
first, then rush together as the pull grows. The amber arrows show the force on each charge. They are
always equal and opposite, and their length follows ![1/r^2](coulombs-law.assets/eq-inline/d790d0aaad.svg)<!--m:1/r^2--> (clipped at the largest)._

![Force between two one-microcoulomb charges against their separation, falling as one over r squared](coulombs-law.assets/fig-03.svg)

_The same law as a graph, for two 1 µC charges (see §4). From 1 cm to 2 cm the force drops from 90 N
to 22 N. By 10 cm it is under 1 N. The curve never reaches zero, but it gets small quickly._

## 4 Worked numbers — how strong the electric force is

**Two electrons, 1 nm apart.** Each carries ![e = 1.602 times 10^-19 C](coulombs-law.assets/eq-inline/e82ad9559e.svg)<!--m:e = 1.602\times10^{-19}\ \mathrm{C}--> and
![1 nm = 10^-9 m](coulombs-law.assets/eq-inline/1c5ba44278.svg)<!--m:1\ \mathrm{nm} = 10^{-9}\ \mathrm{m}-->. Both charges are negative, so they repel:

![two electrons 1 nanometre apart: F equals 8.988 times 10 to the 9 times 1.602 times 10 to the minus 19 squared over 10 to the minus 9 squared, about 2.31 times 10 to the minus 10 newtons](coulombs-law.assets/eq-two-electrons.svg)

That looks tiny, but an electron has mass ![m_e = 9.109 times 10^-31 kg](coulombs-law.assets/eq-inline/75f76387d1.svg)<!--m:m_e = 9.109\times10^{-31}\ \mathrm{kg}-->. By Newton's second
law, acceleration ![a](coulombs-law.assets/eq-inline/86f7e437fa.svg)<!--m:a--> (in ![m times s^-2](coulombs-law.assets/eq-inline/8864b1cbde.svg)<!--m:\mathrm{m\cdot s^{-2}}-->) is force divided by mass:

![acceleration a equals F over m_e equals 2.31 times 10 to the minus 10 over 9.109 times 10 to the minus 31, about 2.5 times 10 to the 20 metres per second squared](coulombs-law.assets/eq-electron-accel.svg)

That is more than ![10^19](coulombs-law.assets/eq-inline/edaa4d4cbf.svg)<!--m:10^{19}--> times the acceleration of gravity.

**Electric force against gravity.** Two electrons also attract each other by gravity, with force
![G m_e^2/r^2](coulombs-law.assets/eq-inline/a936d7e7f7.svg)<!--m:G m_e^2/r^2-->. Here ![G = 6.674 times 10^-11 N times m^2 times kg^-2](coulombs-law.assets/eq-inline/845c29b396.svg)<!--m:G = 6.674\times10^{-11}\ \mathrm{N\cdot m^{2}\cdot kg^{-2}}--> is the
gravitational constant. Both forces fall as ![1/r^2](coulombs-law.assets/eq-inline/d790d0aaad.svg)<!--m:1/r^2-->, so their ratio does not depend on the distance:

![ratio of electric to gravitational force between two electrons equals k e squared over G m_e squared, about 4.2 times 10 to the 42, independent of distance](coulombs-law.assets/eq-gravity-ratio.svg)

The electric force wins by 42 orders of magnitude. Gravity dominates the everyday world only because
matter is almost perfectly neutral, so the huge electric forces cancel.

**The hydrogen atom.** A proton and an electron sit about ![5.29 times 10^-11 m](coulombs-law.assets/eq-inline/b5cc82ab92.svg)<!--m:5.29\times10^{-11}\ \mathrm{m}--> apart (the
Bohr radius). Their attraction is

![proton and electron 5.29 times 10 to the minus 11 metres apart: F about 8.2 times 10 to the minus 8 newtons](coulombs-law.assets/eq-hydrogen.svg)

**One coulomb against one coulomb, 1 m apart.** This is the number that shows how big a coulomb is:

![two charges of 1 coulomb, 1 metre apart: F equals 8.988 times 10 to the 9 times 1 times 1 over 1 squared, about 9.0 times 10 to the 9 newtons](coulombs-law.assets/eq-one-coulomb-force.svg)

Nine thousand million newtons. Divide by ![g = 9.81 m times s^-2](coulombs-law.assets/eq-inline/0c744ab060.svg)<!--m:g = 9.81\ \mathrm{m\cdot s^{-2}}-->, the acceleration of
gravity at the Earth's surface, to find the mass ![m](coulombs-law.assets/eq-inline/6b0d31c0d5.svg)<!--m:m--> that weighs as much:

![the mass whose weight equals that force: m equals F over g equals 8.988 times 10 to the 9 over 9.81, about 9.2 times 10 to the 8 kilograms](coulombs-law.assets/eq-one-coulomb-mass.svg)

About 900 000 tonnes, the weight of several large ships. Yet a 1 A lamp passes 1 C every second.
There is no contradiction: the coulomb flowing through the lamp is matched, at every instant, by an
equal positive charge in the metal it passes through. The wire stays neutral, so the enormous force
never appears. §11 shows that you cannot even hold 1 C of unbalanced charge on any object of
reasonable size.

**Two 1 µC charges, 1 cm apart.** A microcoulomb (![1 mu C = 10^-6 C](coulombs-law.assets/eq-inline/3679393ac9.svg)<!--m:1\ \mu\mathrm{C} = 10^{-6}\ \mathrm{C}-->) is about
what a rubbed balloon or a charged capacitor's plate holds in everyday static electricity:

![two charges of 1 microcoulomb, 1 centimetre apart: F equals 8.988 times 10 to the 9 times 10 to the minus 6 squared over 0.01 squared, about 90 newtons](coulombs-law.assets/eq-microcoulomb.svg)

About the weight of a 9 kg mass, from charges far smaller than anything a circuit pushes through
in a second. This is the curve plotted in Figure 93.

## 5 The vector form and superposition

A force has a direction as well as a size. Coulomb's law gives both once positions are written as
vectors (quantities with a direction, printed with an arrow). Let charge ![q_1](coulombs-law.assets/eq-inline/63d628baf5.svg)<!--m:q_1--> sit at
position ![r_1](coulombs-law.assets/eq-inline/fbe8b5cac7.svg)<!--m:\vec r_1--> and charge ![q_2](coulombs-law.assets/eq-inline/bc09c3b934.svg)<!--m:q_2--> at ![r_2](coulombs-law.assets/eq-inline/f2cd96ee4e.svg)<!--m:\vec r_2-->, both measured in metres from any fixed origin.
Define the separation vector, its length, and the **unit vector** (length 1, no unit) pointing from
charge 1 to charge 2:

![the separation vector r_12 equals r_2 minus r_1, its length r, and the unit vector r-hat_12 equals r_12 over r](coulombs-law.assets/eq-r-vec.svg)

The force on charge 2 due to charge 1 is then

![the force on charge 2 due to charge 1, F_12, equals k q1 q2 over r squared times r-hat_12](coulombs-law.assets/eq-coulomb-vector.svg)

The signs now do the work that the absolute-value bars did before:

- **like charges:** ![q_1 q_2 > 0](coulombs-law.assets/eq-inline/7002d3f6a3.svg)<!--m:q_1 q_2 > 0-->, so ![F_12](coulombs-law.assets/eq-inline/9c06de27aa.svg)<!--m:\vec F_{12}--> points along ![r_12](coulombs-law.assets/eq-inline/5bbb0af01e.svg)<!--m:\hat r_{12}-->, away from charge 1.
  That is repulsion.
- **unlike charges:** ![q_1 q_2 < 0](coulombs-law.assets/eq-inline/2c8abda5a5.svg)<!--m:q_1 q_2 < 0-->, so ![F_12](coulombs-law.assets/eq-inline/9c06de27aa.svg)<!--m:\vec F_{12}--> points against ![r_12](coulombs-law.assets/eq-inline/5bbb0af01e.svg)<!--m:\hat r_{12}-->, back toward charge 1.
  That is attraction.

Swap the labels and ![r_21 = - r_12](coulombs-law.assets/eq-inline/e2c586e23d.svg)<!--m:\hat r_{21} = -\hat r_{12}-->, so the force on charge 1 is equal and opposite,
as Newton's third law requires:

![the force on charge 1 due to charge 2 is equal and opposite: F_21 equals minus F_12](coulombs-law.assets/eq-newton3.svg)

**Superposition.** When several charges act on one charge ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->, each force is computed **as if the
others were absent**, and the forces add as vectors. Here ![F_i](coulombs-law.assets/eq-inline/472748ce35.svg)<!--m:\vec F_i--> is the force from charge ![q_i](coulombs-law.assets/eq-inline/e7a8ac7463.svg)<!--m:q_i-->,
![r_i](coulombs-law.assets/eq-inline/3953cd5715.svg)<!--m:r_i--> is the distance from ![q_i](coulombs-law.assets/eq-inline/e7a8ac7463.svg)<!--m:q_i--> to ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->, and ![r_i](coulombs-law.assets/eq-inline/255bc5f358.svg)<!--m:\hat r_i--> is the unit vector from ![q_i](coulombs-law.assets/eq-inline/e7a8ac7463.svg)<!--m:q_i--> toward ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->:

![the total force on q equals the vector sum over i of k q_i q over r_i squared times r-hat_i](coulombs-law.assets/eq-superposition.svg)

This is an experimental fact, not a mathematical necessity. The force between two charges is not
changed by a third charge nearby. It is what makes the electric field (§6) and Gauss's law (§8)
possible: anything true of one point charge, if it is linear, is true of any collection of them.

**Worked example.** Put ![q_1 = +4 mu C](coulombs-law.assets/eq-inline/4a94d85e71.svg)<!--m:q_1 = +4\ \mu\mathrm{C}--> at the origin, ![q_2 = -3 mu C](coulombs-law.assets/eq-inline/0345dce345.svg)<!--m:q_2 = -3\ \mu\mathrm{C}--> at
![(0.3, 0) m](coulombs-law.assets/eq-inline/8cb578062d.svg)<!--m:(0.3,\ 0)\ \mathrm{m}-->, and a test charge ![q = +1 mu C](coulombs-law.assets/eq-inline/683af563b7.svg)<!--m:q = +1\ \mu\mathrm{C}--> at ![(0, 0.4) m](coulombs-law.assets/eq-inline/292d83c875.svg)<!--m:(0,\ 0.4)\ \mathrm{m}-->. Then
![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> is 0.4 m from ![q_1](coulombs-law.assets/eq-inline/63d628baf5.svg)<!--m:q_1--> and, by Pythagoras, 0.5 m from ![q_2](coulombs-law.assets/eq-inline/bc09c3b934.svg)<!--m:q_2-->.

Force from ![q_1](coulombs-law.assets/eq-inline/63d628baf5.svg)<!--m:q_1--> alone. Both charges are positive, so it is repulsive and points straight up, away from ![q_1](coulombs-law.assets/eq-inline/63d628baf5.svg)<!--m:q_1-->:

![F_1 equals 8.988 times 10 to the 9 times 4 times 10 to the minus 6 times 1 times 10 to the minus 6 over 0.4 squared equals 0.225 newtons, straight up](coulombs-law.assets/eq-sup-f1.svg)

Force from ![q_2](coulombs-law.assets/eq-inline/bc09c3b934.svg)<!--m:q_2--> alone. The charges are unlike, so it is attractive and points from ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> toward ![q_2](coulombs-law.assets/eq-inline/bc09c3b934.svg)<!--m:q_2-->. That direction is
![(0.3, -0.4)/0.5 = (0.6, -0.8)](coulombs-law.assets/eq-inline/deb873e3c9.svg)<!--m:(0.3,\ -0.4)/0.5 = (0.6,\ -0.8)-->:

![F_2 equals 8.988 times 10 to the 9 times 3 times 10 to the minus 6 times 1 times 10 to the minus 6 over 0.5 squared equals 0.108 newtons, directed toward q_2 along 0.6, minus 0.8](coulombs-law.assets/eq-sup-f2.svg)

Add the components:

![components: F_x equals 0 plus 0.108 times 0.6 equals 0.065 newtons; F_y equals 0.225 minus 0.108 times 0.8 equals 0.138 newtons](coulombs-law.assets/eq-sup-components.svg)

and recombine them into a size and an angle ![theta](coulombs-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> from the ![x](coulombs-law.assets/eq-inline/11f6ad8ec5.svg)<!--m:x--> axis:

![magnitude F equals the square root of 0.065 squared plus 0.138 squared, about 0.153 newtons, at arctan of 0.138 over 0.065, about 65 degrees above the x axis](coulombs-law.assets/eq-sup-result.svg)

![Superposition: the force from each charge is found alone and the two force vectors are added tip to tail](coulombs-law.assets/fig-04.svg)

_Each force is found as if the other charge were absent, then the two arrows are added tip to tail.
The pull toward the negative charge cancels part of the push from the positive one. The result is
smaller than the 0.225 N push and tilted toward ![q_2](coulombs-law.assets/eq-inline/bc09c3b934.svg)<!--m:q_2-->._

## 6 The electric field

Coulomb's law describes one charge reaching across empty space to push another. A more useful
description, and the one modern physics uses, says that each charge sets up a **field** in the space
around it, and that another charge feels a force from the field **at the place where it sits**.

**Definition.** Place a small positive **test charge** ![q_0](coulombs-law.assets/eq-inline/6e0974cfb6.svg)<!--m:q_0--> (in coulombs) at a point. Measure the
force ![F](coulombs-law.assets/eq-inline/a763c7ce6f.svg)<!--m:\vec F--> on it. The **electric field** ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E--> at that point is the force per unit test
charge:

![the electric field E equals the force F on a small test charge q_0, divided by q_0](coulombs-law.assets/eq-efield-def.svg)

The test charge is kept small so that its own push does not move the charges that make the field.
The unit is a newton per coulomb:

![unit of E: newton per coulomb, N C to the minus 1](coulombs-law.assets/eq-efield-units.svg)

(§7 shows that this is the same as volts per metre.) Once the field is known, the force on **any**
charge ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> placed there is just

![the force on any charge q placed in a field E is F equals q E](coulombs-law.assets/eq-force-from-field.svg)

A positive charge is pushed along ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E-->, and a negative charge is pushed the opposite way.

**The field of a point charge.** Put the test charge ![q_0](coulombs-law.assets/eq-inline/6e0974cfb6.svg)<!--m:q_0--> at distance ![r](coulombs-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> from a point charge ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->.
Let ![r](coulombs-law.assets/eq-inline/5e39778ab6.svg)<!--m:\hat r--> be the unit vector pointing from ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> out toward ![q_0](coulombs-law.assets/eq-inline/6e0974cfb6.svg)<!--m:q_0-->. Coulomb's law (§5) gives the force

![put a test charge q_0 at distance r from q: the force is k q q_0 over r squared times r-hat](coulombs-law.assets/eq-point-field-1.svg)

and dividing by ![q_0](coulombs-law.assets/eq-inline/6e0974cfb6.svg)<!--m:q_0--> removes the test charge, leaving a property of ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> and of position alone:

![divide by q_0: the field of a point charge is E equals k q over r squared times r-hat](coulombs-law.assets/eq-point-field-2.svg)

For ![q > 0](coulombs-law.assets/eq-inline/b7bea721be.svg)<!--m:q > 0--> the field points radially outward, and for ![q < 0](coulombs-law.assets/eq-inline/d42ae51ad2.svg)<!--m:q < 0--> it points inward. Its strength falls
as ![1/r^2](coulombs-law.assets/eq-inline/d790d0aaad.svg)<!--m:1/r^2-->, exactly like the force. A 1 nC charge (![10^-9 C](coulombs-law.assets/eq-inline/6f04566927.svg)<!--m:10^{-9}\ \mathrm{C}-->) makes, 10 cm away,

![1 nanocoulomb at 10 centimetres: E equals 8.988 times 10 to the 9 times 10 to the minus 9 over 0.1 squared, about 899 newtons per coulomb](coulombs-law.assets/eq-point-field-worked.svg)

By superposition (§5), the field of many charges is the vector sum of their separate fields:

![the field of several charges is the vector sum of their separate fields](coulombs-law.assets/eq-field-superposition.svg)

**Field lines.** A field is a vector at every point of space, which is hard to draw. Faraday's
**field lines** are the standard picture. They follow three rules:

1. At every point the line runs in the direction of ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E-->. An arrow on the line shows the
   direction a positive test charge would be pushed.
2. Lines begin on positive charges and end on negative charges (or run off to infinity). The number
   of lines leaving a charge is proportional to its charge.
3. Where the lines are crowded the field is strong. Where they spread out it is weak. Two lines never
   cross, because the field has only one direction at each point.

![Field lines point straight out of a positive charge, straight into a negative charge, and run from the positive to the negative charge of a pair](coulombs-law.assets/fig-05.svg)

_Around a single charge the lines are straight spokes. The same number of spokes spreads over a
larger and larger sphere, which is the ![1/r^2](coulombs-law.assets/eq-inline/d790d0aaad.svg)<!--m:1/r^2--> law drawn as a picture (§8 makes this exact).
Between a positive and a negative charge the lines bend from one to the other. Each line shows the
path along which a positive charge would be pushed at that point._

![Small positive test charges are pushed outward along the field lines of a positive charge and pulled inward along those of a negative charge](coulombs-law.assets/fig-06-anim.svg)

_The force on a positive test charge points along the line, outward from ![+Q](coulombs-law.assets/eq-inline/de10a3e4cf.svg)<!--m:+Q--> and inward toward ![-Q](coulombs-law.assets/eq-inline/6422eedc12.svg)<!--m:-Q-->.
The dots are drawn drifting through a thick, viscous medium, where speed is proportional to force.
In that case they follow the lines exactly: fast near the source charge, where the lines crowd, and
slow far away. A free charge in vacuum also has momentum, so on a curved line it would drift off to
the outside of the curve. The line gives the direction of the force, not the path._

## 7 Work, potential and voltage

**Work.** A force does **work** when it moves something. For a constant force ![F](coulombs-law.assets/eq-inline/a763c7ce6f.svg)<!--m:\vec F--> moving an object
along a straight displacement ![d](coulombs-law.assets/eq-inline/219e2a95e8.svg)<!--m:\vec d--> (in metres), the work ![W](coulombs-law.assets/eq-inline/e2415cb7f6.svg)<!--m:W--> (in joules, J) is the force times the
distance moved in the direction of the force. Here ![theta](coulombs-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> is the angle between ![F](coulombs-law.assets/eq-inline/a763c7ce6f.svg)<!--m:\vec F--> and ![d](coulombs-law.assets/eq-inline/219e2a95e8.svg)<!--m:\vec d-->:

![work done by a constant force F over a straight displacement d equals F dot d equals F d cos theta; unit joule equals newton metre](coulombs-law.assets/eq-work-def.svg)

Along a curved path from point A to point B, chop the path into tiny straight steps ![d l](coulombs-law.assets/eq-inline/bcff4117de.svg)<!--m:d\vec l--> (in
metres), add up the work done on each step, and take the limit as the steps shrink:

![along a curved path from A to B, W_AB equals the integral from A to B of F dot dl](coulombs-law.assets/eq-work-integral.svg)

**Potential difference.** Let the electric field move a charge ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> from A to B, doing work
![W_AB](coulombs-law.assets/eq-inline/c43b09ea17.svg)<!--m:W_{AB}-->. That work is proportional to ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->, because ![F = q E](coulombs-law.assets/eq-inline/ad0a53f4ca.svg)<!--m:\vec F = q\vec E-->. Dividing by ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> therefore
gives a quantity that belongs to the two points alone. This is the **potential difference** between
A and B, also called the **voltage** between them:

![the potential difference V_A minus V_B equals the work W_AB done by the field on q going from A to B, divided by q](coulombs-law.assets/eq-potential-def.svg)

- ![V_A](coulombs-law.assets/eq-inline/3b4275c420.svg)<!--m:V_A-->, ![V_B](coulombs-law.assets/eq-inline/e3ff51d217.svg)<!--m:V_B--> — the **electric potential** at A and at B, in volts (V);
- ![W_AB](coulombs-law.assets/eq-inline/c43b09ea17.svg)<!--m:W_{AB}--> — the work done by the field on ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> as it goes from A to B, in joules;
- ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> — the charge carried, in coulombs.

Put ![F = q E](coulombs-law.assets/eq-inline/ad0a53f4ca.svg)<!--m:\vec F = q\vec E--> into the work integral and the ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> cancels:

![using F equals q E, V_A minus V_B equals the integral from A to B of E dot dl](coulombs-law.assets/eq-potential-integral.svg)

Equivalently, ![V_A - V_B](coulombs-law.assets/eq-inline/fc02a45568.svg)<!--m:V_A - V_B--> is the work **you** must do, per coulomb, to carry a charge slowly
back from B to A against the field. Positive charge "falls" from high potential to low, just as a
mass falls from high to low ground. A 12 V battery is a device that lifts every coulomb through 12 J.

**The volt.** The definition fixes the unit:

![one volt equals one joule per coulomb, J C to the minus 1](coulombs-law.assets/eq-volt.svg)

and with ![1 J = 1 N times m](coulombs-law.assets/eq-inline/df40a59902.svg)<!--m:1\ \mathrm{J} = 1\ \mathrm{N\cdot m}--> it also shows that the two units of field are
the same:

![volt per metre equals joule per coulomb per metre equals newton metre per coulomb per metre equals newton per coulomb](coulombs-law.assets/eq-field-units-volt.svg)

So ![N times C^-1](coulombs-law.assets/eq-inline/f25c79812d.svg)<!--m:\mathrm{N\cdot C^{-1}}--> and ![V times m^-1](coulombs-law.assets/eq-inline/b6f586582e.svg)<!--m:\mathrm{V\cdot m^{-1}}--> name the same thing. Circuits use the
second: a field is how many volts you drop per metre.

**Uniform field.** If ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E--> is the same everywhere between two points a distance ![d](coulombs-law.assets/eq-inline/3c363836cf.svg)<!--m:d--> apart
along the field, the force ![qE](coulombs-law.assets/eq-inline/1fbab2c831.svg)<!--m:qE--> is constant and points along the path. The work is force times
distance:

![in a uniform field E, carrying q a distance d along the field: W equals q E d](coulombs-law.assets/eq-uniform-work.svg)

Divide by ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->:

![so V equals W over q equals E d, and E equals V over d](coulombs-law.assets/eq-uniform-v.svg)

For example, 12 V across a 1 mm gap:

![12 volts across 1 millimetre: E equals 12 over 0.001 equals 1.2 times 10 to the 4 volts per metre](coulombs-law.assets/eq-uniform-worked.svg)

**The potential of a point charge.** Choose the zero of potential far away, at ![r = infinity](coulombs-law.assets/eq-inline/6faa163225.svg)<!--m:r = \infty-->, where
the field has died out. Carry a test charge from radius ![r](coulombs-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> straight out to infinity along a field
line. Here ![r'](coulombs-law.assets/eq-inline/72e34f6e57.svg)<!--m:r'--> is the running radius along the way, in metres:

![potential at distance r relative to infinity: V of r minus V of infinity equals the integral from r to infinity of E dr prime](coulombs-law.assets/eq-point-v-1.svg)

![substitute E equals k q over r prime squared](coulombs-law.assets/eq-point-v-2.svg)

![the antiderivative of 1 over r prime squared is minus 1 over r prime](coulombs-law.assets/eq-point-v-3.svg)

![so with V at infinity set to zero, V of r equals k q over r](coulombs-law.assets/eq-point-v-4.svg)

The step that confuses people is the sign. The field pushes a positive test charge **outward**,
so going from ![r](coulombs-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> out to ![infinity](coulombs-law.assets/eq-inline/9b97f26fbe.svg)<!--m:\infty--> the field does positive work, and the starting point ![r](coulombs-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> is at the
**higher** potential. The integral ![integral_r^ infinity dr'/r'^2](coulombs-law.assets/eq-inline/bb9a2a8f45.svg)<!--m:\int_r^\infty dr'/r'^2--> comes out as ![+1/r](coulombs-law.assets/eq-inline/bc95b37772.svg)<!--m:+1/r-->, so a positive charge
sits on a "hill" of potential ![kq/r](coulombs-law.assets/eq-inline/49644ded37.svg)<!--m:kq/r--> that rises as you approach it. The 1 nC charge of §6, at 10 cm:

![1 nanocoulomb at 10 centimetres: V equals 8.988 times 10 to the 9 times 10 to the minus 9 over 0.1, about 90 volts](coulombs-law.assets/eq-point-v-worked.svg)

**Why voltage depends only on the end points.** The Coulomb force on a test charge points straight
toward or away from the source charge. Split any path into tiny steps along the radius and tiny
steps around a circle centred on the charge. The steps around the circle are perpendicular to the
force, so they do no work. The radial steps add up to the integral above, which depends only on the
starting and ending radius. So the work, and hence the voltage, is the same for **every** path
between two points. By superposition this holds for any arrangement of static charges. It is why
"the voltage at a node" means something, and it is the content of Kirchhoff's voltage law (see
[../electromagnetism/electromagnetism.md §1](../electromagnetism/electromagnetism.md#1-charge-current-and-the-electric-field)).
A *changing* magnetic field breaks this property, which is the subject of
[Faraday's law](../faradays-law/).

**Potential energy and the electron-volt.** A charge ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> at a point of potential ![V](coulombs-law.assets/eq-inline/c9ee5681d3.svg)<!--m:V--> has potential
energy ![U](coulombs-law.assets/eq-inline/b2c7c0caa1.svg)<!--m:U--> (in joules)

![the potential energy of charge q at a point of potential V is U equals q V](coulombs-law.assets/eq-potential-energy.svg)

An electron that falls through 1 V gains one **electron-volt** (eV) of energy, the natural energy
unit for single particles:

![one electron-volt equals e times one volt equals 1.602 times 10 to the minus 19 joules](coulombs-law.assets/eq-ev.svg)

## 8 Gauss's law, derived from Coulomb's law

Coulomb's law says the field falls as ![1/r^2](coulombs-law.assets/eq-inline/d790d0aaad.svg)<!--m:1/r^2-->. The surface of a sphere grows as ![r^2](coulombs-law.assets/eq-inline/23b008c61c.svg)<!--m:r^2-->. Those two
facts cancel, and the cancellation gives Gauss's law, which is Coulomb's law rewritten in a form
that makes symmetric problems easy.

**Flux.** Give each small patch of a surface an **area vector** ![d A](coulombs-law.assets/eq-inline/ee650d7f5c.svg)<!--m:d\vec A-->. Its length is the
patch's area (in m²) and it points along the patch's normal, outward for a closed surface. The
**electric flux** ![Phi_E](coulombs-law.assets/eq-inline/ca9214b981.svg)<!--m:\Phi_E--> through a surface ![S](coulombs-law.assets/eq-inline/02aa629c8b.svg)<!--m:S--> adds up the component of ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E--> that crosses
each patch:

![electric flux Phi_E equals the integral over S of E dot dA; for uniform E across a flat area, E A cos theta](coulombs-law.assets/eq-flux-def.svg)

Here ![theta](coulombs-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> is the angle between ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E--> and the normal. A field skimming along the surface
(![theta = 90^ deg](coulombs-law.assets/eq-inline/23d451a4a2.svg)<!--m:\theta = 90^\circ-->) contributes nothing, and a field crossing it head-on contributes ![EA](coulombs-law.assets/eq-inline/07cdb47207.svg)<!--m:EA-->. In the
field-line picture, flux counts how many lines pierce the surface, with lines going out counted
positive and lines coming in counted negative. Its unit is

![unit of flux: N C to the minus 1 times square metres equals N m squared C to the minus 1, which equals volt metres](coulombs-law.assets/eq-flux-units.svg)

**A sphere centred on a point charge.** On a sphere of radius ![r](coulombs-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> around ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q-->, the field is the same
size everywhere and points straight out, along the normal (![theta = 0](coulombs-law.assets/eq-inline/5e8b7ec255.svg)<!--m:\theta = 0-->). So ![E](coulombs-law.assets/eq-inline/e0184adedf.svg)<!--m:E--> comes outside
the integral, and the integral of ![dA](coulombs-law.assets/eq-inline/f895508f50.svg)<!--m:dA--> is just the sphere's area:

![on a sphere of radius r centred on q, E is radial and the same everywhere, so the flux is E of r times the area 4 pi r squared](coulombs-law.assets/eq-gauss-sphere-1.svg)

Substitute the point-charge field from §6:

![substitute E of r equals q over 4 pi epsilon_0 r squared](coulombs-law.assets/eq-gauss-sphere-2.svg)

![the r squared and the 4 pi cancel, leaving q over epsilon_0, whatever the radius](coulombs-law.assets/eq-gauss-sphere-3.svg)

The radius has gone. Every sphere around the charge, large or small, carries exactly the same
flux. A sphere twice as big has four times the area, and the field on it is a quarter as strong. In
the picture, the same lines cross every sphere. This works **only** because the force is exactly
inverse-square. With ![1/r^2.1](coulombs-law.assets/eq-inline/11da031119.svg)<!--m:1/r^{2.1}-->, larger spheres would carry less flux.

**Any closed surface.** The same is true of a lumpy surface. Measure how big a patch looks from the
charge by its **solid angle** ![d Omega](coulombs-law.assets/eq-inline/9db14df989.svg)<!--m:d\Omega--> (in steradians, sr). This is the patch's area, tilted to face
the charge, divided by ![r^2](coulombs-law.assets/eq-inline/23b008c61c.svg)<!--m:r^2-->. The whole sphere of directions is ![4 pi](coulombs-law.assets/eq-inline/63f4e29993.svg)<!--m:4\pi--> steradians:

![solid angle d Omega equals dA cos theta over r squared; the whole sphere is 4 pi steradians](coulombs-law.assets/eq-solid-angle.svg)

The flux through one patch is then

![on any closed surface, E dot dA equals k q over r squared times dA cos theta](coulombs-law.assets/eq-gauss-any-1.svg)

![which is k q d Omega](coulombs-law.assets/eq-gauss-any-2.svg)

So each patch's flux depends only on how big it looks from the charge, not on how far away it is.
A closed surface around the charge surrounds it completely, covering all ![4 pi](coulombs-law.assets/eq-inline/63f4e29993.svg)<!--m:4\pi--> steradians once:

![summing over the whole closed surface: 4 pi k q equals q over epsilon_0](coulombs-law.assets/eq-gauss-any-3.svg)

If the charge is **outside** the surface, every cone of directions from the charge that hits the
surface hits it twice: once going in (negative flux) and once coming out (positive flux). The two
patches subtend the same solid angle, so they cancel exactly. An outside charge contributes zero net
flux.

**Gauss's law.** Add up any number of charges by superposition (§5). Each one inside contributes
![q_i/epsilon_0](coulombs-law.assets/eq-inline/2964e8183b.svg)<!--m:q_i/\varepsilon_0--> and each one outside contributes zero. Let ![Q_enc](coulombs-law.assets/eq-inline/d4eb055369.svg)<!--m:Q_{enc}--> be the total charge enclosed
by the surface, in coulombs. Then

![Gauss's law: the flux of E out of any closed surface S equals the enclosed charge Q_enc over epsilon_0](coulombs-law.assets/eq-gauss-law.svg)

![Every sphere around a charge is crossed by the same number of field lines, and a closed surface beside a charge is entered and left by the same lines](coulombs-law.assets/fig-07.svg)

_Left: the 16 lines from the charge cross the small sphere and the large sphere alike, so the flux
is the same. Right: a surface beside the charge. Each line that enters it (blue) leaves again (red),
so the net flux is zero. Only enclosed charge counts._

Gauss's law and Coulomb's law contain the same physics for static charges. Gauss's form is more
useful in two ways. It turns symmetric field problems into one line of algebra (§9). It also stays
true when charges move, which is why it appears as the first of Maxwell's four equations in
[../electromagnetism/electromagnetism.md §15](../electromagnetism/electromagnetism.md#15-the-whole-set--maxwells-four-equations).

## 9 Parallel plates and capacitance

A capacitor is two conducting plates facing each other across a gap (see
[../capacitor/capacitor.md](../capacitor/capacitor.md)). Gauss's law gives its field and its
capacitance directly.

**Surface charge density.** Spread charge ![Q](coulombs-law.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> evenly over a plate of area ![A](coulombs-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A-->. The charge per
square metre is the **surface charge density** ![sigma](coulombs-law.assets/eq-inline/69c15416b6.svg)<!--m:\sigma--> (sigma):

![surface charge density sigma equals Q over A, in coulombs per square metre](coulombs-law.assets/eq-sigma.svg)

**One large sheet.** Take a flat sheet of charge, large enough that its edges are far away. By
symmetry its field must point straight away from it on both sides (for ![sigma > 0](coulombs-law.assets/eq-inline/c83cdd1ad8.svg)<!--m:\sigma > 0-->), with the same
strength on both sides. Draw a **pillbox**, a short closed cylinder whose two flat faces of area
![A_p](coulombs-law.assets/eq-inline/08d67e7a0e.svg)<!--m:A_p--> sit on either side of the sheet. Flux leaves through both faces and none through the curved
side, which runs parallel to the field. The charge inside is ![sigma A_p](coulombs-law.assets/eq-inline/a29f0b5b81.svg)<!--m:\sigma A_p-->. Gauss's law gives

![pillbox through one sheet: flux leaves through both faces, 2 E A_p, and the charge inside is sigma A_p](coulombs-law.assets/eq-sheet-1.svg)

![so one sheet makes E equals sigma over 2 epsilon_0 on each side, independent of distance](coulombs-law.assets/eq-sheet-2.svg)

The field does not fall with distance. Close to a large sheet, every field line runs straight out
and never spreads, so the field stays the same at every distance.

**Two plates.** Now put ![+Q](coulombs-law.assets/eq-inline/de10a3e4cf.svg)<!--m:+Q--> on one plate and ![-Q](coulombs-law.assets/eq-inline/6422eedc12.svg)<!--m:-Q--> on the other, a distance ![d](coulombs-law.assets/eq-inline/3c363836cf.svg)<!--m:d--> apart, with
![d](coulombs-law.assets/eq-inline/3c363836cf.svg)<!--m:d--> much smaller than the plate's width. Each plate alone makes a field of size ![sigma/(2 epsilon_0)](coulombs-law.assets/eq-inline/e0f83f83f1.svg)<!--m:\sigma/(2\varepsilon_0)-->.
The positive plate's field points away from it. The negative plate's field points toward it.
**Between** the plates both point from + to −, so by superposition they add:

![between the plates both sheets' fields point from plus to minus and add: E equals sigma over 2 epsilon_0 plus sigma over 2 epsilon_0 equals sigma over epsilon_0 equals Q over epsilon_0 A](coulombs-law.assets/eq-plates-add.svg)

**Outside** the plates, the two fields point opposite ways and cancel:

![outside the plates the two fields point opposite ways and cancel: E equals sigma over 2 epsilon_0 minus sigma over 2 epsilon_0 equals 0](coulombs-law.assets/eq-plates-outside.svg)

As a check, draw one pillbox around a patch of the positive plate only. Its outer face is in the
zero-field region and its inner face is in the gap, so flux leaves through one face only:

![check with one pillbox around the top plate: flux leaves only through the lower face, E A_p equals sigma A_p over epsilon_0, so E equals sigma over epsilon_0](coulombs-law.assets/eq-plates-pillbox.svg)

Both routes give the same answer. The field between the plates is **uniform**: the same size and
direction at every point of the gap.

**Voltage and capacitance.** The field is uniform, so §7 gives the voltage across the gap:

![the voltage is V equals E d equals Q d over epsilon_0 A](coulombs-law.assets/eq-plates-v.svg)

The voltage is proportional to the charge. The constant of proportionality is the
**capacitance** ![C](coulombs-law.assets/eq-inline/32096c2e0e.svg)<!--m:C-->, the charge stored per volt (in farads, F):

![capacitance C equals Q over V equals epsilon_0 A over d](coulombs-law.assets/eq-capacitance.svg)

This is the relation ![Q = CV](coulombs-law.assets/eq-inline/4d85416dd9.svg)<!--m:Q = CV--> that [../capacitor/capacitor.md §1](../capacitor/capacitor.md#1-what-a-capacitor-actually-is)
starts from, now with a formula for ![C](coulombs-law.assets/eq-inline/32096c2e0e.svg)<!--m:C--> in terms of the geometry. It explains the design rules. To get
more capacitance, make the plates larger (more area ![A](coulombs-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> holds more charge at the same field), bring them
closer (smaller ![d](coulombs-law.assets/eq-inline/3c363836cf.svg)<!--m:d--> means less voltage for the same field), or fill the gap with a material that
reduces the field (below).

![Two parallel plates carrying plus Q and minus Q hold a uniform field between them; a Gaussian pillbox around the top plate gives E equals sigma over epsilon 0](coulombs-law.assets/fig-08.svg)

_Between the plates the field is uniform. Outside, the two sheets' fields cancel. The pillbox
around the top plate encloses charge ![sigma A_p](coulombs-law.assets/eq-inline/a29f0b5b81.svg)<!--m:\sigma A_p--> and loses flux only through its lower face,
which gives ![E = sigma/epsilon_0](coulombs-law.assets/eq-inline/615fd791f6.svg)<!--m:E = \sigma/\varepsilon_0-->. Only near the edges does the field bulge outward
("fringing"), which the formula ignores._

**The farad, and the permittivity in farads per metre.** The unit of capacitance is

![one farad equals one coulomb per volt, C V to the minus 1](coulombs-law.assets/eq-farad.svg)

Now check the units of ![epsilon_0 A/d](coulombs-law.assets/eq-inline/d0cee8a1e1.svg)<!--m:\varepsilon_0 A/d-->, using ![1 J = 1 N times m](coulombs-law.assets/eq-inline/df40a59902.svg)<!--m:1\ \mathrm{J} = 1\ \mathrm{N\cdot m}--> and
![1 J = 1 C times V](coulombs-law.assets/eq-inline/d0948b9163.svg)<!--m:1\ \mathrm{J} = 1\ \mathrm{C\cdot V}--> (from ![1 V = 1 J times C^-1](coulombs-law.assets/eq-inline/3a22397b12.svg)<!--m:1\ \mathrm{V} = 1\ \mathrm{J\cdot C^{-1}}-->, §7):

![units of epsilon_0 A over d: C squared N to the minus 1 m to the minus 1, which is C squared per joule, which is coulombs per volt, the farad; so epsilon_0 is in farads per metre](coulombs-law.assets/eq-eps0-farad.svg)

That is why the permittivity is usually quoted as
![epsilon_0 = 8.854 times 10^-12 F times m^-1](coulombs-law.assets/eq-inline/d16548f40b.svg)<!--m:\varepsilon_0 = 8.854\times10^{-12}\ \mathrm{F\cdot m^{-1}}-->. The number is the same as in §3; only
the unit's name has changed.

**Worked numbers.** Two plates ![10 cm times 10 cm](coulombs-law.assets/eq-inline/55ad0f8d1c.svg)<!--m:10\ \mathrm{cm}\times10\ \mathrm{cm}--> (![A = 0.01 m^2](coulombs-law.assets/eq-inline/61f1d717ee.svg)<!--m:A = 0.01\ \mathrm{m^2}-->), 1 mm apart, in vacuum:

![10 centimetre square plates, 1 millimetre apart: C equals 8.854 times 10 to the minus 12 times 0.01 over 0.001, about 88.5 picofarads](coulombs-law.assets/eq-cap-worked.svg)

Charged to 12 V, the field is ![12 kV times m^-1](coulombs-law.assets/eq-inline/34e252e911.svg)<!--m:12\ \mathrm{kV\cdot m^{-1}}--> (§7) and the charge is

![at 12 volts, Q equals C V, about 1.06 times 10 to the minus 9 coulombs, about 6.6 times 10 to the 9 electrons](coulombs-law.assets/eq-cap-worked-q.svg)

About seven thousand million electrons moved from one plate to the other. That sounds like a lot,
but it is fewer than one surface atom in ten million. How big would a **one-farad** vacuum capacitor
with a 1 mm gap have to be?

![a 1 farad vacuum capacitor with a 1 millimetre gap needs A equals C d over epsilon_0, about 1.13 times 10 to the 8 square metres](coulombs-law.assets/eq-one-farad.svg)

That is a square more than 10 km on a side. Real farad-sized capacitors use
nanometre-thin insulating layers (the electrolytic capacitor) or the porous surface of carbon (the
supercapacitor) to make ![d](coulombs-law.assets/eq-inline/3c363836cf.svg)<!--m:d--> tiny and ![A](coulombs-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> huge.

**Dielectrics.** An insulating material in the gap contains molecules whose charges shift slightly
in the field, partly cancelling it. The field for a given ![Q](coulombs-law.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> drops by the material's **relative
permittivity** ![epsilon_r](coulombs-law.assets/eq-inline/0f0fcc1c39.svg)<!--m:\varepsilon_r--> (a pure number, 1 for vacuum, about 4 to 5 for the FR-4 glass-epoxy
of a circuit board, and thousands for some ceramics), so the capacitance rises by the same factor:

![with a dielectric of relative permittivity epsilon_r filling the gap, C equals epsilon_r epsilon_0 A over d](coulombs-law.assets/eq-dielectric.svg)

**Where the energy is.** The [capacitor notes](../capacitor/capacitor.md#1-what-a-capacitor-actually-is)
show that a charged capacitor stores energy ![U = 1 over 2 C V^2](coulombs-law.assets/eq-inline/f3f032a189.svg)<!--m:U = \tfrac{1}{2} C V^2--> (in joules). Put in ![C = epsilon_0 A/d](coulombs-law.assets/eq-inline/f699fc1a96.svg)<!--m:C = \varepsilon_0 A/d-->
and ![V = Ed](coulombs-law.assets/eq-inline/478c7f00e0.svg)<!--m:V = Ed-->. The volume of the gap is ![Ad](coulombs-law.assets/eq-inline/94b6931d19.svg)<!--m:Ad-->, so the energy per cubic metre ![u](coulombs-law.assets/eq-inline/51e69892ab.svg)<!--m:u--> (in
![J times m^-3](coulombs-law.assets/eq-inline/1e7191b44c.svg)<!--m:\mathrm{J\cdot m^{-3}}-->) is

![stored energy one half C V squared equals one half times epsilon_0 A over d times E d squared equals one half epsilon_0 E squared times A d; so the energy per volume is u equals one half epsilon_0 E squared](coulombs-law.assets/eq-energy-density.svg)

The energy belongs to the field itself, spread through the gap.

## 10 Why there is no magnetic charge

Charges come in two kinds that can be separated. Pull an electron off an atom and you have a lone
negative charge and a positive ion, each with a field of its own. Magnetism looks at first as though
it should work the same way. A bar magnet has a north (N) and a south (S) pole, like poles repel, and
unlike poles attract. But the analogy fails at the first test: **cut a magnet in half and you get two
complete magnets**, each with its own N and S. Cut again and again, down to a single atom, and you
still find N and S together. Nobody has ever isolated a lone north or south pole, a **magnetic
monopole**, despite dedicated searches in cosmic rays, in moon rock and at particle colliders.

The reason is that magnetism has a different source. The **magnetic field** ![B](coulombs-law.assets/eq-inline/f2369dcd91.svg)<!--m:\vec B--> is defined,
like ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E-->, by the force it exerts. That force acts only on a charge ![q](coulombs-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> that is **moving**, with
velocity ![v](coulombs-law.assets/eq-inline/8cc30fa813.svg)<!--m:\vec v--> (in ![m times s^-1](coulombs-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}-->), and it acts sideways, perpendicular to both ![v](coulombs-law.assets/eq-inline/8cc30fa813.svg)<!--m:\vec v-->
and ![B](coulombs-law.assets/eq-inline/f2369dcd91.svg)<!--m:\vec B--> (the cross product ![times](coulombs-law.assets/eq-inline/5d2892f79a.svg)<!--m:\times-->):

![a charge q moving with velocity v in a magnetic field B feels F equals q v cross B](coulombs-law.assets/eq-lorentz-b.svg)

Its unit, the tesla (T), follows from the force law: newtons per (coulomb times metre per second).
Since ![1 C times s^-1 = 1 A](coulombs-law.assets/eq-inline/5d5758346d.svg)<!--m:1\ \mathrm{C\cdot s^{-1}} = 1\ \mathrm{A}--> (§2), this can also be written in amperes:

![one tesla equals one newton second per coulomb per metre, which is one newton per ampere per metre](coulombs-law.assets/eq-tesla.svg)

And it is **moving** charge, which means current, that makes ![B](coulombs-law.assets/eq-inline/f2369dcd91.svg)<!--m:\vec B--> in the first place. A bar
magnet's field comes from electrons inside its atoms, each of which behaves as a tiny loop of
circulating current (strictly, a quantum property called spin, which acts like one). A current loop
has a "north" face and a "south" face, but they are two sides of the same loop. They cannot be pulled
apart, because there is nothing at either face to pull: only charge flowing round. That is why the
lines of ![B](coulombs-law.assets/eq-inline/f2369dcd91.svg)<!--m:\vec B--> never start or end. They close on themselves, passing through the magnet from S to
N and returning outside from N to S. Every line that enters a closed surface also leaves it, so in
the language of §8 the magnetic flux out of **any** closed surface is zero:

![the magnetic flux out of any closed surface is zero](coulombs-law.assets/eq-gauss-b.svg)

Compare Gauss's law for ![E](coulombs-law.assets/eq-inline/bb952b27a7.svg)<!--m:\vec E-->, where the right-hand side is ![Q_enc/epsilon_0](coulombs-law.assets/eq-inline/29312f8770.svg)<!--m:Q_{enc}/\varepsilon_0-->. For ![B](coulombs-law.assets/eq-inline/f2369dcd91.svg)<!--m:\vec B-->
there is nothing to enclose.

![Electric field lines begin and end on separate charges, but magnetic field lines of a bar magnet form closed loops and cutting the magnet only makes two magnets](coulombs-law.assets/fig-09.svg)

_Electric field lines start on positive charge and end on negative charge, and the two charges can be
separated. Magnetic field lines have no ends. They loop through the magnet and back, and cutting
the magnet only makes two shorter loops, never a free pole._

So the magnetic counterpart of Coulomb's law cannot be "pole against pole". It must be a law about
how a **current** makes a magnetic field. That law is **Ampère's law**: around any closed loop, the
magnetic field adds up to a constant times the current passing through the loop. It is developed
from the force between current-carrying wires in [../amperes-law/](../amperes-law/). The two kinds
of field are then joined by [Faraday's law](../faradays-law/): a magnetic flux that **changes** with
time makes an electric field that circulates around it. That electric field is not produced by
charges, and around a closed loop it does net work, so the end-point rule of §7 does not hold for it.

## 11 What this costs you — the limits of Coulomb's law

- **Static charges only.** Coulomb's law gives the force between charges **at rest**. Once charges
  move, a magnetic force appears as well (§10). A change in one charge's position also reaches the
  other only after a delay ![r/c](coulombs-law.assets/eq-inline/1795e2d973.svg)<!--m:r/c-->, where ![c = 3.0 times 10^8 m times s^-1](coulombs-law.assets/eq-inline/8cf4bc7ad4.svg)<!--m:c = 3.0\times10^8\ \mathrm{m\cdot s^{-1}}--> is the speed of
  light. For circuit-sized distances and frequencies the correction is negligible. For antennas and
  fast PCB edges it is the whole story.
- **Point charges and ideal geometry.** The law is exact for points. Real bodies must be split into
  small pieces and summed (superposition). The parallel-plate formula ![C = epsilon_0 A/d](coulombs-law.assets/eq-inline/f699fc1a96.svg)<!--m:C = \varepsilon_0 A/d-->
  ignores fringing at the edges, which adds noticeably to the capacitance when ![d](coulombs-law.assets/eq-inline/3c363836cf.svg)<!--m:d--> is not small
  compared with the plate width.
- **Materials change the numbers.** Inside an insulator the field of a given charge is reduced by
  ![epsilon_r](coulombs-law.assets/eq-inline/0f0fcc1c39.svg)<!--m:\varepsilon_r-->. For many ceramic dielectrics ![epsilon_r](coulombs-law.assets/eq-inline/0f0fcc1c39.svg)<!--m:\varepsilon_r--> itself changes with voltage,
  temperature and age. A "10 µF" class-2 ceramic capacitor can lose most of its capacitance at its
  rated voltage.
- **Breakdown caps the field.** Air stops insulating at about ![3 MV times m^-1](coulombs-law.assets/eq-inline/86e87959d7.svg)<!--m:3\ \mathrm{MV\cdot m^{-1}}-->: free
  electrons gain enough energy between collisions to knock more electrons loose, and a spark jumps.
  This is why you cannot hold one coulomb of unbalanced charge on anything ordinary. Keep the
  surface field of a charged sphere of radius ![R](coulombs-law.assets/eq-inline/06576556d1.svg)<!--m:R--> (in metres) below the breakdown field ![E_bd](coulombs-law.assets/eq-inline/7e7e3294ee.svg)<!--m:E_{bd}-->:

![to hold 1 coulomb on a sphere in air without exceeding 3 megavolts per metre at its surface: R at least the square root of k Q over E_bd, about 55 metres](coulombs-law.assets/eq-breakdown-sphere.svg)

  A sphere bigger than a football stadium, just to hold 1 C in air. The circuit coulombs of §2 are
  always balanced by an equal opposite charge close by.
- **Quantum and nuclear scales.** At distances below about ![10^-15 m](coulombs-law.assets/eq-inline/96d18bec6f.svg)<!--m:10^{-15}\ \mathrm{m}--> the strong
  nuclear force dominates, and at atomic scales the motion of the charges needs quantum mechanics.
  The inverse-square law itself has been tested to extraordinary precision, though. Any departure in
  the exponent is smaller than about one part in ![10^16](coulombs-law.assets/eq-inline/317d861b13.svg)<!--m:10^{16}-->.
- **The coulomb is a large unit.** Everyday static electricity involves nanocoulombs to
  microcoulombs. Circuits move coulombs every second, but never hold more than a tiny unbalanced
  fraction of them anywhere.

## 12 Sources and cross-links

- **Where these results are used next:**
  [../electromagnetism/electromagnetism.md](../electromagnetism/electromagnetism.md) — §1 there
  summarises charge, current and the field. §14 uses Gauss's law on the plates (§9 here) to show that
  the displacement current equals ![C dV/dt](coulombs-law.assets/eq-inline/b40eb60abe.svg)<!--m:C\,dV/dt-->. §15 lists Gauss's law as the first of Maxwell's equations.
- **The capacitor law:** [../capacitor/capacitor.md](../capacitor/capacitor.md)
  — its §2 differentiates ![Q = CV](coulombs-law.assets/eq-inline/4d85416dd9.svg)<!--m:Q = CV--> to get ![I_C = C dV_C/dt](coulombs-law.assets/eq-inline/1fb4b645ce.svg)<!--m:I_C = C\,dV_C/dt-->, using the definition of current
  from §2 here.
- **Magnetism from current:** [../amperes-law/](../amperes-law/) — Ampère's law, the magnetic
  counterpart of §8, starting from the absence of magnetic charge (§10).
- **Changing flux makes an electric field:** [../faradays-law/](../faradays-law/) — the electric
  field that charges do *not* make, and why it breaks the end-point rule of §7.
- C.-A. de Coulomb, *Premier mémoire sur l'électricité et le magnétisme*, Histoire de l'Académie
  Royale des Sciences (1785) — the torsion-balance measurements.
- E. R. Williams, J. E. Faller and H. A. Hill, "New experimental test of Coulomb's law: a laboratory
  upper limit on the photon rest mass", *Physical Review Letters* 26, 721 (1971) — the inverse-square
  exponent confirmed to about one part in 10¹⁶.
- BIPM, *The International System of Units (SI)*, 9th edition (2019) — the exact value of ![e](coulombs-law.assets/eq-inline/58e6b3a414.svg)<!--m:e--> and
  the definitions of the coulomb and the ampere. CODATA 2022 recommended values for
  ![epsilon_0](coulombs-law.assets/eq-inline/961a0cda39.svg)<!--m:\varepsilon_0-->, ![m_e](coulombs-law.assets/eq-inline/a5006b1846.svg)<!--m:m_e--> and ![G](coulombs-law.assets/eq-inline/a36a6718f5.svg)<!--m:G-->.
- D. J. Griffiths, *Introduction to Electrodynamics*, 4th ed., chapter 2 (Coulomb's law, the field,
  Gauss's law, potential) — the standard textbook route followed here.
- E. M. Purcell and D. J. Morin, *Electricity and Magnetism*, 3rd ed., chapters 1–3 — the
  field-line and flux arguments, and the conductor and capacitor results.
- Style and figure conventions: [../../STYLE.md](../../STYLE.md).
