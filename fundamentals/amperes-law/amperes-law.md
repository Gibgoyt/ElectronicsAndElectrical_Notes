# Ampère's law — how a current makes a magnetic field

Every inductor, transformer, motor and relay in this tree works because **a current is wrapped in
a magnetic field**. This document builds that fact from nothing. It starts with what a magnetic
field *is* (the force it puts on a moving charge), then Oersted's discovery that a current makes
one, and the right-hand grip rule that gives its direction. Then come the two laws that give its
size. The **Biot–Savart law** adds up the field piece by piece. **Ampère's circuital law** gets
the same answer in one line whenever the geometry is symmetric enough. Both are used, step by step,
on the long straight wire, the inside of a conductor, the solenoid, the toroid and the coaxial
cable. Then come the H-field and magnetic cores, the force between two wires (which used to define
the ampere), and Maxwell's correction, which completes the law. Every result has worked numbers.

**Contents**

1. [The magnetic field B and the force on a moving charge](#1-the-magnetic-field-b-and-the-force-on-a-moving-charge)
2. [Oersted's discovery — a current makes a magnetic field](#2-oersteds-discovery--a-current-makes-a-magnetic-field)
3. [The right-hand grip rule](#3-the-right-hand-grip-rule)
4. [The Biot-Savart law](#4-the-biot-savart-law)
5. [The long straight wire by Biot-Savart](#5-the-long-straight-wire-by-biot-savart)
6. [Ampere's circuital law](#6-amperes-circuital-law)
7. [The straight wire again by Ampere's law](#7-the-straight-wire-again-by-amperes-law)
8. [Inside the conductor — B grows in proportion to r](#8-inside-the-conductor--b-grows-in-proportion-to-r)
9. [The solenoid](#9-the-solenoid)
10. [The toroid](#10-the-toroid)
11. [The coaxial cable](#11-the-coaxial-cable)
12. [H, permeability and ferromagnetic cores](#12-h-permeability-and-ferromagnetic-cores)
13. [The force between parallel wires and the old ampere](#13-the-force-between-parallel-wires-and-the-old-ampere)
14. [Maxwell's correction — displacement current](#14-maxwells-correction--displacement-current)
15. [What this costs you](#15-what-this-costs-you)
16. [Sources and cross-links](#16-sources-and-cross-links)

> **The thesis in one line**
>
> A current is wrapped in a magnetic field that circles it. Walk once around any closed loop,
> adding up the field along your path, and the total depends only on the current that threads the
> loop. Nothing else matters: not the loop's shape, and not currents outside it.

![closed line integral of B dot dl around C equals mu_0 times I enclosed](amperes-law.assets/eq-ampere.svg)

**What you need first.** Electric charge, the coulomb, the electric field ![E](amperes-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> and the
permittivity of free space ![epsilon_0](amperes-law.assets/eq-inline/961a0cda39.svg)<!--m:\varepsilon_0--> are built in [../coulombs-law/](../coulombs-law/). This
document uses them only in §1 (one line) and §14. The partner law, in which a *changing* magnetic
field makes a voltage, is [../faradays-law/](../faradays-law/). Read it after this one.

---

## 1 The magnetic field B and the force on a moving charge

**Charge and current.** Charge ![q](amperes-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> is measured in **coulombs** (C) (see
[../coulombs-law/](../coulombs-law/)). A **current** ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> is the rate at which charge ![Q](amperes-law.assets/eq-inline/c3156e00d3.svg)<!--m:Q--> flows past a
point. Its unit is the **ampere** (A), and one ampere is one coulomb per second:

![I equals dQ by dt; one ampere equals one coulomb per second](amperes-law.assets/eq-current.svg)

By convention the direction of a current is the direction *positive* charge would move. This is
called **conventional current**. In a metal wire the carriers are electrons, which carry negative
charge and drift the other way, but every rule in this document uses conventional current.

**Vectors.** An arrow over a symbol marks a vector, a quantity with a size and a direction:
![v](amperes-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> is the velocity of a charge (in ![m times s^-1](amperes-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}-->), and ![v](amperes-law.assets/eq-inline/7a38d8cbd2.svg)<!--m:v--> without the arrow is
its size. A hat marks a **unit vector**, an arrow of length exactly one that carries a direction
only.

**What a magnetic field is.** Put a charge ![q](amperes-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> near a magnet or near a wire carrying current. If the
charge sits still, the magnet does nothing to it. If it *moves*, it feels a force, and that force
has three strange properties. It is proportional to the speed. It vanishes for one particular
direction of motion. And it always points sideways, at right angles to the motion. A vector field
![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->, the **magnetic field** (also called the *magnetic flux density*), sums up all three. Its
direction at each point is the direction of motion that feels no force. Its size sets how strong
the force is. The force is

![F equals q v cross B](amperes-law.assets/eq-force-moving-charge.svg)

- ![F](amperes-law.assets/eq-inline/b0682d270b.svg)<!--m:\vec{F}--> — the force on the charge, in newtons (N);
- ![q](amperes-law.assets/eq-inline/22ea1c649c.svg)<!--m:q--> — the charge, in coulombs (C), with its sign;
- ![v](amperes-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> — the charge's velocity, in ![m times s^-1](amperes-law.assets/eq-inline/54b190bdce.svg)<!--m:\mathrm{m\cdot s^{-1}}-->;
- ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> — the magnetic field where the charge is, in **tesla** (T, defined below);
- ![times](amperes-law.assets/eq-inline/5d2892f79a.svg)<!--m:\times--> — the **cross product**, defined next.

This equation is the *definition* of ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->: the magnetic field is whatever vector makes this
force law come out right at every point.

**The cross product.** For two vectors ![a](amperes-law.assets/eq-inline/1e37c650a8.svg)<!--m:\vec{a}--> and ![b](amperes-law.assets/eq-inline/71fa108edb.svg)<!--m:\vec{b}--> with an angle ![theta](amperes-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> between them,
![a times b](amperes-law.assets/eq-inline/06bae3770c.svg)<!--m:\vec{a}\times\vec{b}--> is a third vector:

![the size of a cross b equals a b sin theta; b cross a equals minus a cross b](amperes-law.assets/eq-cross.svg)

- **Size:** ![ab sin theta](amperes-law.assets/eq-inline/4bdf0397fd.svg)<!--m:ab\sin\theta-->. It is largest when the two vectors are at right angles and zero when
  they are parallel.
- **Direction:** perpendicular to *both* ![a](amperes-law.assets/eq-inline/1e37c650a8.svg)<!--m:\vec{a}--> and ![b](amperes-law.assets/eq-inline/71fa108edb.svg)<!--m:\vec{b}-->. Of the two directions that are
  perpendicular to both, the **right-hand rule for the cross product** picks one. Point the
  fingers of your right hand along ![a](amperes-law.assets/eq-inline/1e37c650a8.svg)<!--m:\vec{a}-->, curl them toward ![b](amperes-law.assets/eq-inline/71fa108edb.svg)<!--m:\vec{b}--> through the smaller angle,
  and your thumb points along ![a times b](amperes-law.assets/eq-inline/06bae3770c.svg)<!--m:\vec{a}\times\vec{b}-->.
- **Order matters:** swapping the two reverses the result.

Applied to ![F = q v times B](amperes-law.assets/eq-inline/3d6a2c73fd.svg)<!--m:\vec{F} = q\vec{v}\times\vec{B}-->, with ![theta](amperes-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> the angle between the velocity and the
field:

![F equals q v B sin theta; when v is perpendicular to B, F equals q v B](amperes-law.assets/eq-force-magnitude.svg)

Two consequences matter later. First, a charge moving *along* the field (![theta = 0](amperes-law.assets/eq-inline/5e8b7ec255.svg)<!--m:\theta = 0-->) feels
nothing, which is how the direction of ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> was defined. Second, the force is always at right
angles to the velocity, so it can bend a path but never speed a charge up. The power the force
delivers, force dotted with velocity (the **dot product** ![F times v = Fv cos alpha](amperes-law.assets/eq-inline/7889c40682.svg)<!--m:\vec{F}\cdot\vec{v} = Fv\cos\alpha-->, where
![alpha](amperes-law.assets/eq-inline/f7c665b459.svg)<!--m:\alpha--> is the angle between them), is zero:

![P equals F dot v equals q times v cross B dot v equals 0](amperes-law.assets/eq-no-work.svg)

A static magnetic field does no work on a free charge. (Energy enters and leaves magnetic fields
through the electric field that a *changing* magnetic field makes, which is
[../faradays-law/](../faradays-law/).) Add the electric force ![q E](amperes-law.assets/eq-inline/8c5db5737d.svg)<!--m:q\vec{E}--> from
[../coulombs-law/](../coulombs-law/) and you have the complete **Lorentz force** on a charge:

![F equals q times E plus v cross B](amperes-law.assets/eq-lorentz.svg)

**The tesla.** Read the unit of ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> off the force law, with ![v](amperes-law.assets/eq-inline/39a3a59a8f.svg)<!--m:\vec{v}--> perpendicular to ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> so
that ![F = qvB](amperes-law.assets/eq-inline/3401a534e1.svg)<!--m:F = qvB-->:

![B equals F over q v, so the unit of B is newton per coulomb per metre per second, which is newton times coulomb per second to the minus one times metre to the minus one](amperes-law.assets/eq-tesla-1.svg)

A coulomb per second is an ampere, so

![coulomb per second equals ampere, so one tesla equals one newton per ampere per metre](amperes-law.assets/eq-tesla-2.svg)

That is the first form of the tesla: **a field of one tesla pushes with one newton on each metre of
wire carrying one ampere across it** (the wire force is derived just below). For the second form,
rewrite the newton. Work is force times distance, so a newton is a joule per metre. A volt is a
joule per coulomb, so a joule is a volt times a coulomb:

![one newton equals one joule per metre equals one volt coulomb per metre](amperes-law.assets/eq-tesla-3.svg)

Substitute, then collect the coulomb and the ampere. A coulomb per ampere is a second, because an
ampere is a coulomb per second:

![one tesla equals one volt coulomb per metre per ampere per metre, equals one volt times coulomb per ampere per square metre, equals one volt second per square metre](amperes-law.assets/eq-tesla-4.svg)

So ![1 T = 1 N times A^-1 times m^-1 = 1 V times s times m^-2](amperes-law.assets/eq-inline/148b5fdb9d.svg)<!--m:1\ \mathrm{T} = 1\ \mathrm{N\cdot A^{-1}\cdot m^{-1}} = 1\ \mathrm{V\cdot s\cdot m^{-2}}-->. The second form says a
tesla is *volt-seconds spread over an area*. That is the form that matters in Faraday's law and in
transformer design ([../faradays-law/](../faradays-law/)).

For scale: the Earth's field is about ![50 mu T](amperes-law.assets/eq-inline/2cd225cb1f.svg)<!--m:50\ \mu\mathrm{T}--> (25 to 65 depending on where you stand), a
fridge magnet about ![5 mT](amperes-law.assets/eq-inline/b8943ccedb.svg)<!--m:5\ \mathrm{mT}-->, a ferrite transformer core runs at ![0.1](amperes-law.assets/eq-inline/180505679c.svg)<!--m:0.1--> to ![0.3 T](amperes-law.assets/eq-inline/29dad7b555.svg)<!--m:0.3\ \mathrm{T}-->, the
surface of a neodymium magnet about ![1 T](amperes-law.assets/eq-inline/7a94d47bc3.svg)<!--m:1\ \mathrm{T}-->, and a hospital MRI scanner ![1.5](amperes-law.assets/eq-inline/aa8f289ebe.svg)<!--m:1.5--> to ![3 T](amperes-law.assets/eq-inline/5175206ddf.svg)<!--m:3\ \mathrm{T}-->.

**The force on a wire.** A wire carrying current is full of moving charge, so a magnetic field
pushes on it. Take a short piece of wire ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}-->: a vector whose length ![dl](amperes-law.assets/eq-inline/b612af11ef.svg)<!--m:dl--> is the length of the
piece (m) and whose direction is the direction of the current. The charge ![dq](amperes-law.assets/eq-inline/3bf4ab708f.svg)<!--m:dq--> that crosses this
piece in time ![dt](amperes-law.assets/eq-inline/f7e6632892.svg)<!--m:dt--> moves with velocity ![v = d l/dt](amperes-law.assets/eq-inline/21c46c00e6.svg)<!--m:\vec{v} = d\vec{l}/dt-->. The key step is to regroup:

![dq v equals dq times dl by dt equals dq by dt times dl equals I dl](amperes-law.assets/eq-wire-force-1.svg)

So the force on the piece is ![dq v times B = I d l times B](amperes-law.assets/eq-inline/fe73c51456.svg)<!--m:dq\,\vec{v}\times\vec{B} = I\,d\vec{l}\times\vec{B}-->. For a straight wire of
length ![L](amperes-law.assets/eq-inline/d160e0986a.svg)<!--m:L--> (m) at right angles to a uniform field, every piece pushes the same way and the forces
add:

![dF equals dq v cross B equals I dl cross B; for a straight perpendicular wire in a uniform field, F equals B I L](amperes-law.assets/eq-wire-force-2.svg)

*Worked number.* A motor has ![0.5 T](amperes-law.assets/eq-inline/790c25363e.svg)<!--m:0.5\ \mathrm{T}--> in its air gap, and ![5 cm](amperes-law.assets/eq-inline/2ae14f3384.svg)<!--m:5\ \mathrm{cm}--> of conductor in that gap
carries ![10 A](amperes-law.assets/eq-inline/c2205c3089.svg)<!--m:10\ \mathrm{A}-->:

![F equals 0.5 tesla times 10 amperes times 0.05 metres equals 0.25 newtons](amperes-law.assets/eq-wire-force-worked.svg)

The same 10 A wire, 1 m long, across the Earth's ![50 mu T](amperes-law.assets/eq-inline/2cd225cb1f.svg)<!--m:50\ \mu\mathrm{T}--> feels only ![0.5 mN](amperes-law.assets/eq-inline/744b41b060.svg)<!--m:0.5\ \mathrm{mN}-->. This is why
motors need strong fields.

**Field lines.** A magnetic field is drawn as **field lines**. The line's direction at each point is
the direction of ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}-->, and the lines are drawn closer together where the field is stronger.
Magnetic field lines never start or end: they close on themselves. (This is the statement that
there are no magnetic "charges", or *monopoles*, written formally as Gauss's law for magnetism in
[../electromagnetism/electromagnetism.md §15](../electromagnetism/electromagnetism.md#15-the-whole-set--maxwells-four-equations).)

## 2 Oersted's discovery — a current makes a magnetic field

Until 1820, electricity and magnetism were thought to be separate subjects. In April of that year
Hans Christian Oersted, lecturing in Copenhagen, ran a current from a battery through a wire laid
over a compass, along the north–south line. The needle swung away from north. When he broke the
circuit, it swung back. Reversing the current swung the needle the other way. A wire laid over the
compass east–west did nothing visible. He published the result in July 1820 as a four-page Latin
pamphlet. Within weeks André-Marie Ampère in Paris showed that two currents push on each other (§13),
and Jean-Baptiste Biot and Félix Savart measured how the field falls off with distance (§4).

![Oersted experiment: a compass needle under a north-south wire swings toward east when the current is switched on](amperes-law.assets/fig-02-anim.svg)

_The needle under the wire is pushed by two fields at once: the Earth's, pointing north, and the
wire's, pointing sideways. It settles along their sum, which is why it turns part of the way and
not all of it. §7 works out the 33.7° in the figure._

What Oersted's needle says, and what careful later measurement confirmed:

- **A current makes a magnetic field.** No magnet is needed: moving charge is enough. (A permanent
  magnet's field turns out to come from the same source, the circulating charge of its electrons,
  §12.)
- **The field circles the wire.** Its lines are closed circles centred on the wire, in planes at
  right angles to it. Under the wire the field points one way across the needle; over the wire it
  points the other way.
- **The field is proportional to the current.** Double the current and you double the field.
  Reverse the current and the field reverses.
- **The field weakens with distance from the wire.** §5 and §7 show it falls as one over the
  distance.

![Current flows up a straight wire while its magnetic field circulates around it in closed rings](amperes-law.assets/fig-01-anim.svg)

_The field is not a fluid that flows round the wire. The moving arrowheads only show which way the
field points around each circle. The rings are the same at every height along a long straight
wire._

## 3 The right-hand grip rule

The field circles the wire, so we need a rule for *which way* it circles. First, a way of drawing a
current that points straight into or out of the page.

**The dot and the cross.** Think of the current as an arrow. If the arrow points *out of* the page,
toward you, you see its point: a dot in a circle, ![](amperes-law.assets/eq-inline/c8e2d1a0bf.svg)<!--m:\odot-->. If it points *into* the page, away from you,
you see the cross of its tail feathers: a cross in a circle, ![](amperes-law.assets/eq-inline/72166555a6.svg)<!--m:\otimes-->. The circle itself is the rim of
the wire.

![Current out of the page drawn as a dot in a circle with an anticlockwise field; current into the page drawn as a cross in a circle with a clockwise field](amperes-law.assets/fig-03.svg)

_Every field circle is centred on the wire. Around a current coming toward you the field runs
anticlockwise; around a current going away it runs clockwise. The field is strongest on the
innermost circle and weakens as one over the radius._

**The rule.** Grip the wire with your **right** hand, thumb pointing along the conventional current.
Your fingers curl around the wire in the direction of the magnetic field.

![Right-hand grip rule: grip the wire with the right thumb along the current and the fingers curl the way the magnetic field goes; for a coil, fingers along the current in the turns and the thumb gives the field inside](amperes-law.assets/fig-04.svg)

_Panel (a): thumb along the current, fingers along the field. Panel (b) is the same hand used the
other way round for a coil. The current circulates in the turns, so the fingers follow it, and the
thumb gives the field down the middle and the coil's north end. The thumb always takes the
straight thing; the fingers always take the thing that goes round._

Check it against the symbols. Point your right thumb at your own face (current out of the page,
![](amperes-law.assets/eq-inline/c8e2d1a0bf.svg)<!--m:\odot-->): your fingers curl anticlockwise as you look at them. Point it away (![](amperes-law.assets/eq-inline/72166555a6.svg)<!--m:\otimes-->): clockwise. Those
are the two directions drawn in the figures.

![Animated cross-sections: the field circles anticlockwise around current out of the page and clockwise around current into the page](amperes-law.assets/fig-05-anim.svg)

_The same two cross-sections, with the direction of circulation shown moving. Use it to check the
grip rule: thumb toward you, fingers anticlockwise._

**For a coil (solenoid).** Curl the fingers of your right hand the way the current runs around the
turns. Your thumb then points along the field *inside* the coil, which is also the direction the
field leaves the coil: the coil's **north** end. This is the same physics. Apply the
straight-wire rule to any short piece of any turn and you find that every piece pushes field the
same way through the middle (§9).

> **Watch out —** The rule is for **conventional** current, the direction positive charge would
> move. Electrons in a wire move the other way. If you follow the electrons with your right hand you
> get every field backwards. (Some textbooks teach a "left-hand rule" for electron flow; it is the
> same rule with the sign flipped. Pick conventional current and the right hand, and never mix.)

**A quick check.** A vertical wire carries current upward. A compass sits due east of it. Which way
does the wire's field point at the compass? Thumb up: seen from above, your fingers run
anticlockwise, so on the east side of the wire they point north. The wire's field at the compass
points north.

> **Note —** Why the *right* hand? Nature does not prefer one. The right-hand choice is built into
> the definition of the cross product (§1). Use it consistently in ![F = q v times B](amperes-law.assets/eq-inline/3d6a2c73fd.svg)<!--m:\vec{F} = q\vec{v}\times\vec{B}-->
> and in the field laws below, and every prediction comes out right. Use the left hand everywhere
> and every ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> would point the other way. Every force would still come out the same, because
> each force has two cross products and the sign cancels. This is why ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> is called a
> *pseudovector*.

## 4 The Biot-Savart law

Oersted's observations say *that* a current makes a field. The Biot–Savart law says *how much*. The
idea is to cut the circuit into tiny pieces. Each piece ![I d l](amperes-law.assets/eq-inline/e3b4a4319c.svg)<!--m:I\,d\vec{l}--> contributes a small field ![d B](amperes-law.assets/eq-inline/32b0df1dba.svg)<!--m:d\vec{B}--> at
the point of interest, and the total field is the sum of all of them:

![dB equals mu_0 over 4 pi times I dl cross r-hat over r squared, where r-hat equals r vector over r](amperes-law.assets/eq-biot-savart.svg)

Every symbol:

- ![d B](amperes-law.assets/eq-inline/32b0df1dba.svg)<!--m:d\vec{B}--> — the field contributed by this one piece, in tesla;
- ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> — the current in the piece, in amperes;
- ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> — the piece itself: a vector along the wire in the direction of the current, whose
  length ![dl](amperes-law.assets/eq-inline/b612af11ef.svg)<!--m:dl--> (m) is the piece's length;
- ![r](amperes-law.assets/eq-inline/e8436e4526.svg)<!--m:\vec{r}--> — the vector *from the piece to the point* where you want the field (m);
- ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> — the length of ![r](amperes-law.assets/eq-inline/e8436e4526.svg)<!--m:\vec{r}-->, the distance from piece to point (m);
- ![r](amperes-law.assets/eq-inline/e954d16a9b.svg)<!--m:\hat{r}--> — the unit vector along ![r](amperes-law.assets/eq-inline/e8436e4526.svg)<!--m:\vec{r}-->, which carries the direction only;
- ![times](amperes-law.assets/eq-inline/5d2892f79a.svg)<!--m:\times--> — the cross product of §1, which makes ![d B](amperes-law.assets/eq-inline/32b0df1dba.svg)<!--m:d\vec{B}--> perpendicular to both the wire and the
  line of sight;
- ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> — the **permeability of free space**, a constant of nature (below).

Taking sizes, with ![theta](amperes-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> the angle between ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> and ![r](amperes-law.assets/eq-inline/e8436e4526.svg)<!--m:\vec{r}-->:

![dB equals mu_0 over 4 pi times I dl sin theta over r squared](amperes-law.assets/eq-biot-savart-mag.svg)

and the field of the whole circuit is the sum (the integral) of every piece. The circle on the
integral sign means *all the way around the closed circuit*. A steady current always flows in a
closed loop.

![B equals mu_0 over 4 pi times the closed integral of I dl cross r-hat over r squared](amperes-law.assets/eq-biot-savart-total.svg)

Read the law in words. Each piece contributes a field that:

- grows with the current and with the length of the piece;
- falls as **one over the square** of the distance, like Coulomb's law for charges
  ([../coulombs-law/](../coulombs-law/));
- is zero for a point straight ahead of the piece (![theta = 0](amperes-law.assets/eq-inline/5e8b7ec255.svg)<!--m:\theta = 0-->) and largest for a point off to the
  side (![theta = 90^ deg](amperes-law.assets/eq-inline/23d451a4a2.svg)<!--m:\theta = 90^\circ-->);
- points around the wire, never toward or away from it. This is the right-hand grip rule of §3
  written as a cross product.

**The constant.** In the SI, ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> has the value

![mu_0 equals 4 pi times 10 to the minus 7 henries per metre, about 1.2566 times 10 to the minus 6 tesla metres per ampere](amperes-law.assets/eq-mu0.svg)

Its unit follows from the law itself. Solve for ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> and put in the unit of each symbol:

![mu_0 equals 4 pi r squared dB over I dl, so the unit of mu_0 is square metre tesla per ampere metre, which is tesla metre per ampere](amperes-law.assets/eq-mu0-units-1.svg)

The datasheet unit is the **henry per metre**. The henry (H) is the unit of inductance: one weber of
flux per ampere. A **weber** (Wb) is one tesla across one square metre, the unit of magnetic
**flux** (the field times the area it crosses, built in [../faradays-law/](../faradays-law/)). So:

![one henry equals one weber per ampere equals one tesla square metre per ampere, so one henry per metre equals one tesla metre per ampere equals one newton per ampere squared](amperes-law.assets/eq-mu0-units-2.svg)

The last form uses ![1 T = 1 N times A^-1 times m^-1](amperes-law.assets/eq-inline/d2cebbfcc2.svg)<!--m:1\ \mathrm{T} = 1\ \mathrm{N\cdot A^{-1}\cdot m^{-1}}--> from §1.

> **Note —** Until 2019, ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> was *exactly* ![4 pi times 10^-7](amperes-law.assets/eq-inline/fd648cedba.svg)<!--m:4\pi\times10^{-7}-->, because the ampere was defined
> through the force between two wires (§13). Since the 2019 redefinition of the SI the elementary
> charge is fixed instead, and ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> became a measured quantity. The CODATA value agrees with
> ![4 pi times 10^-7](amperes-law.assets/eq-inline/fd648cedba.svg)<!--m:4\pi\times10^{-7}--> to better than one part in a billion, so for all engineering purposes it is
> still ![4 pi times 10^-7 H times m^-1](amperes-law.assets/eq-inline/ed33d40c80.svg)<!--m:4\pi\times10^{-7}\ \mathrm{H\cdot m^{-1}}-->.

![Biot-Savart geometry: a current element dl on a straight wire, the separation vector s to the field point P at perpendicular distance r, and the angles theta and phi](amperes-law.assets/fig-06.svg)

_The set-up for the next section. One piece ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> of the wire, the arrow ![s](amperes-law.assets/eq-inline/6a16290a6f.svg)<!--m:\vec{s}--> from it to the
point ![P](amperes-law.assets/eq-inline/511993d3c9.svg)<!--m:P-->, and the two angles. Pieces above and below ![O](amperes-law.assets/eq-inline/08a914cde0.svg)<!--m:O--> both push their contribution into the
page at ![P](amperes-law.assets/eq-inline/511993d3c9.svg)<!--m:P-->, so nothing cancels._

## 5 The long straight wire by Biot-Savart

This is the first real use of the law: the field at distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> from a long straight wire
carrying current ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I-->. It is a long calculation, and §7 gets the same answer in three lines. It is
worth doing once, because it shows that the ![1/r](amperes-law.assets/eq-inline/525108fcf9.svg)<!--m:1/r--> law for a wire really is the sum of many ![1/r^2](amperes-law.assets/eq-inline/d790d0aaad.svg)<!--m:1/r^2-->
pieces.

> **Note —** In this section the letter ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> means the *perpendicular distance from the wire to the
> point*, because that is what the final answer is written in. So the piece-to-point distance,
> called ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> in §4, is renamed ![s](amperes-law.assets/eq-inline/a0f1490a20.svg)<!--m:s--> here, with ![s](amperes-law.assets/eq-inline/6a16290a6f.svg)<!--m:\vec{s}--> the vector and ![s](amperes-law.assets/eq-inline/66a6bd5762.svg)<!--m:\hat{s}--> its unit vector.

**Step 1 — set up.** Lay the wire along a ![z](amperes-law.assets/eq-inline/395df8f7c5.svg)<!--m:z-->-axis with the current flowing toward ![+z](amperes-law.assets/eq-inline/81e5266b98.svg)<!--m:+z--> (up the page
in Figure 105). Put the point ![P](amperes-law.assets/eq-inline/511993d3c9.svg)<!--m:P--> at perpendicular distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> from the wire, level with the
origin ![O](amperes-law.assets/eq-inline/08a914cde0.svg)<!--m:O-->. Take a piece ![dz](amperes-law.assets/eq-inline/57f378cca8.svg)<!--m:dz--> of wire at height ![z](amperes-law.assets/eq-inline/395df8f7c5.svg)<!--m:z-->. Then ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> points up with length ![dz](amperes-law.assets/eq-inline/57f378cca8.svg)<!--m:dz-->, and
the piece-to-point distance is ![s = sqrt r^2 + z^2](amperes-law.assets/eq-inline/29defa21ac.svg)<!--m:s = \sqrt{r^2 + z^2}--> (Pythagoras on the right-angled triangle
of Figure 105).

**Step 2 — direction.** ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> points up, and ![s](amperes-law.assets/eq-inline/6a16290a6f.svg)<!--m:\vec{s}--> points from the piece across to ![P](amperes-law.assets/eq-inline/511993d3c9.svg)<!--m:P-->. By
the cross-product rule, ![d l times s](amperes-law.assets/eq-inline/3cea2eff24.svg)<!--m:d\vec{l}\times\hat{s}--> points into the page at ![P](amperes-law.assets/eq-inline/511993d3c9.svg)<!--m:P-->. That is true for *every*
piece, above ![O](amperes-law.assets/eq-inline/08a914cde0.svg)<!--m:O--> and below it. So every contribution points the same way, and the total is just the
sum of the sizes. (This is the step that makes the calculation easy. In most problems the
contributions point in different directions and must be added as vectors.)

![dB equals mu_0 over 4 pi times I dz sin theta over s squared, with s equal to the square root of r squared plus z squared](amperes-law.assets/eq-bsw-1.svg)

**Step 3 — remove the angle.** In the right-angled triangle of Figure 105, the side opposite
![theta](amperes-law.assets/eq-inline/cb005d76f9.svg)<!--m:\theta--> is ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> and the hypotenuse is ![s](amperes-law.assets/eq-inline/a0f1490a20.svg)<!--m:s-->, so ![sin theta = r/s](amperes-law.assets/eq-inline/9bb30799c8.svg)<!--m:\sin\theta = r/s-->:

![sin theta equals r over s, so dB equals mu_0 I over 4 pi times r dz over s cubed, equals mu_0 I over 4 pi times r dz over r squared plus z squared to the three halves](amperes-law.assets/eq-bsw-2.svg)

**Step 4 — add up every piece.** A long wire runs from ![z = - infinity](amperes-law.assets/eq-inline/ccf8888ba9.svg)<!--m:z = -\infty--> to ![z = + infinity](amperes-law.assets/eq-inline/58b5340dc0.svg)<!--m:z = +\infty-->. Pull the
constants out of the integral:

![B equals mu_0 I r over 4 pi times the integral from minus infinity to plus infinity of dz over r squared plus z squared to the three halves](amperes-law.assets/eq-bsw-3.svg)

**Step 5 — change variable.** Measure each piece by the angle ![phi](amperes-law.assets/eq-inline/44294dbd19.svg)<!--m:\varphi--> at which ![P](amperes-law.assets/eq-inline/511993d3c9.svg)<!--m:P--> sees it
(Figure 105) rather than by its height. Then ![z = r tan phi](amperes-law.assets/eq-inline/29bc987155.svg)<!--m:z = r\tan\varphi-->. Running ![z](amperes-law.assets/eq-inline/395df8f7c5.svg)<!--m:z--> from ![- infinity](amperes-law.assets/eq-inline/18787d835d.svg)<!--m:-\infty--> to ![+ infinity](amperes-law.assets/eq-inline/bdef054374.svg)<!--m:+\infty--> means
running ![phi](amperes-law.assets/eq-inline/44294dbd19.svg)<!--m:\varphi--> from ![- pi/2](amperes-law.assets/eq-inline/2adf81a422.svg)<!--m:-\pi/2--> to ![+ pi/2](amperes-law.assets/eq-inline/583916068f.svg)<!--m:+\pi/2-->. Differentiating gives ![dz](amperes-law.assets/eq-inline/57f378cca8.svg)<!--m:dz-->, and the denominator simplifies:

![z equals r tan phi; dz equals r sec squared phi d phi; r squared plus z squared equals r squared times 1 plus tan squared phi, which equals r squared sec squared phi](amperes-law.assets/eq-bsw-4.svg)

Here ![phi = 1/cos phi](amperes-law.assets/eq-inline/f8d2ce37de.svg)<!--m:\sec\varphi = 1/\cos\varphi-->, and ![d( tan phi )/d phi =^2 phi](amperes-law.assets/eq-inline/da04a61b7b.svg)<!--m:d(\tan\varphi)/d\varphi = \sec^2\!\varphi-->. The step that confuses people
is ![1 + tan^2 phi =^2 phi](amperes-law.assets/eq-inline/e835b8709a.svg)<!--m:1 + \tan^2\!\varphi = \sec^2\!\varphi-->. It is just Pythagoras: divide ![sin^2 phi + cos^2 phi = 1](amperes-law.assets/eq-inline/b7027ffe55.svg)<!--m:\sin^2\!\varphi + \cos^2\!\varphi = 1--> through by
![cos^2 phi](amperes-law.assets/eq-inline/f6d01e5d2b.svg)<!--m:\cos^2\!\varphi-->:

![sin squared phi plus cos squared phi equals 1; dividing by cos squared phi gives tan squared phi plus 1 equals sec squared phi](amperes-law.assets/eq-bsw-identity.svg)

**Step 6 — simplify the integrand.** The power ![3/2](amperes-law.assets/eq-inline/d3c4aa234c.svg)<!--m:3/2--> of ![r^2^2 phi](amperes-law.assets/eq-inline/d1b47aea0b.svg)<!--m:r^2\sec^2\!\varphi--> is ![r^3^3 phi](amperes-law.assets/eq-inline/39918b770c.svg)<!--m:r^3\sec^3\!\varphi-->:

![dz over r squared plus z squared to the three halves equals r sec squared phi d phi over r cubed sec cubed phi, which equals cos phi d phi over r squared](amperes-law.assets/eq-bsw-5.svg)

**Step 7 — integrate.**

![the integral from minus pi over 2 to plus pi over 2 of cos phi d phi equals sin phi evaluated between the limits, equals 1 minus minus 1, equals 2](amperes-law.assets/eq-bsw-6.svg)

**Step 8 — assemble.**

![B equals mu_0 I r over 4 pi times 2 over r squared, which equals mu_0 I over 2 pi r](amperes-law.assets/eq-bsw-7.svg)

That is the field of a long straight wire. It is proportional to the current, falls as **one over
the distance**, and circles the wire.

**Where the field comes from.** If you stop the integral at ![phi_1](amperes-law.assets/eq-inline/447d1b956d.svg)<!--m:\varphi_1--> and ![phi_2](amperes-law.assets/eq-inline/b7d358fa00.svg)<!--m:\varphi_2--> instead of ![plus-minus pi/2](amperes-law.assets/eq-inline/b62c1d88dd.svg)<!--m:\pm\pi/2-->,
you get the field of a *finite* straight segment, seen from a point at perpendicular distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r-->
from its line:

![B equals mu_0 I over 4 pi r times sin phi_2 minus sin phi_1](amperes-law.assets/eq-bsw-finite.svg)

Take just the stretch of wire within ![plus-minus r](amperes-law.assets/eq-inline/59e473d661.svg)<!--m:\pm r--> of ![O](amperes-law.assets/eq-inline/08a914cde0.svg)<!--m:O-->, so ![phi](amperes-law.assets/eq-inline/44294dbd19.svg)<!--m:\varphi--> runs from ![-45^ deg](amperes-law.assets/eq-inline/e5902f0b8d.svg)<!--m:-45^\circ--> to ![+45^ deg](amperes-law.assets/eq-inline/24008984ec.svg)<!--m:+45^\circ-->:

![the field from the part with absolute z less than r, as a fraction of the total, equals sin of pi over 4 minus sin of minus pi over 4, over 2, which is root 2 over 2, about 0.71](amperes-law.assets/eq-bsw-near.svg)

About 71 % of the field comes from the length of wire within one distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> of the nearest point.
A wire counts as "long" once it runs a few times ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> either side of you.

**Worked number — 10 A at 1 cm.**

![B equals mu_0 I over 2 pi r equals 4 pi times 10 to the minus 7, times 10, over 2 pi times 0.01, equals 2 times 10 to the minus 4 tesla, which is 200 microtesla](amperes-law.assets/eq-wire-worked.svg)

That is four times the Earth's field, from an ordinary 10 A wire one centimetre away. It is worth
remembering the constant in front:

![mu_0 over 2 pi equals 2 times 10 to the minus 7 tesla metres per ampere, so B equals 2 times 10 to the minus 7 times I over r](amperes-law.assets/eq-wire-shortcut.svg)

At ![10 cm](amperes-law.assets/eq-inline/05048e1c9c.svg)<!--m:10\ \mathrm{cm}--> the same wire gives ![20 mu T](amperes-law.assets/eq-inline/da116a2863.svg)<!--m:20\ \mu\mathrm{T}-->, and at ![1 m](amperes-law.assets/eq-inline/75c737356d.svg)<!--m:1\ \mathrm{m}--> it gives ![2 mu T](amperes-law.assets/eq-inline/5995d95d53.svg)<!--m:2\ \mu\mathrm{T}-->.

## 6 Ampere's circuital law

Biot–Savart always works, but the integral in §5 was for the easiest shape there is. Ampère's
circuital law says the same physics in a form that, for symmetric shapes, makes the answer almost
free:

![closed line integral of B dot dl around C equals mu_0 times I enclosed](amperes-law.assets/eq-ampere.svg)

- ![C](amperes-law.assets/eq-inline/32096c2e0e.svg)<!--m:C--> — a closed loop that **you choose**, called the **Amperian loop**. It is imaginary: a path in
  space, not a wire. You also choose which way round to walk it.
- ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> — a small step along the loop, in the direction you are walking, with length ![dl](amperes-law.assets/eq-inline/b612af11ef.svg)<!--m:dl--> (m).
- ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> — the magnetic field at that step (T). The total field, from every current anywhere.
- ![B times d l](amperes-law.assets/eq-inline/08375c2981.svg)<!--m:\vec{B}\cdot d\vec{l}--> — the dot product: the part of the field *along* your step, times the
  step length.
- ![loop integral_C](amperes-law.assets/eq-inline/1f02c1df6a.svg)<!--m:\oint_C--> — add up ![B times d l](amperes-law.assets/eq-inline/08375c2981.svg)<!--m:\vec{B}\cdot d\vec{l}--> over every step, once around the loop. The units are
  ![T times m](amperes-law.assets/eq-inline/7eb58ecf67.svg)<!--m:\mathrm{T\cdot m}-->.
- ![I_enc](amperes-law.assets/eq-inline/7406b82902.svg)<!--m:I_{enc}--> — the **enclosed current**: the net current passing through the loop, counted with a
  sign (below), in amperes.
- ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> — the same constant as in Biot–Savart.

**The line integral, slowly.** The symbol ![loop integral_C B times d l](amperes-law.assets/eq-inline/4bc2170798.svg)<!--m:\oint_C \vec{B}\cdot d\vec{l}--> is a recipe for a sum. Walk
around ![C](amperes-law.assets/eq-inline/32096c2e0e.svg)<!--m:C--> in ![K](amperes-law.assets/eq-inline/a7ee38bb7b.svg)<!--m:K--> short steps. Step number ![k](amperes-law.assets/eq-inline/13fbd79c3d.svg)<!--m:k--> is a little arrow ![Delta l_k](amperes-law.assets/eq-inline/07c6624d36.svg)<!--m:\Delta\vec{l}_k--> along the path. At
that step the field is ![B_k](amperes-law.assets/eq-inline/395510e74e.svg)<!--m:\vec{B}_k-->, at some angle ![alpha_k](amperes-law.assets/eq-inline/867086074b.svg)<!--m:\alpha_k--> to the step. Only the part of the field
along the step counts:

![B dot delta l equals B delta l cos alpha](amperes-law.assets/eq-dot.svg)

A field along your step counts fully (![cos 0 = 1](amperes-law.assets/eq-inline/7f28a78900.svg)<!--m:\cos 0 = 1-->). A field across it counts nothing
(![cos 90^ deg = 0](amperes-law.assets/eq-inline/e23270f327.svg)<!--m:\cos 90^\circ = 0-->). A field against it counts negative (![cos 180^ deg = -1](amperes-law.assets/eq-inline/8df939f3c7.svg)<!--m:\cos 180^\circ = -1-->). Add the
contributions of all ![K](amperes-law.assets/eq-inline/a7ee38bb7b.svg)<!--m:K--> steps:

![the sum over k from 1 to K of B_k dot delta l_k equals B_1 dot delta l_1 plus B_2 dot delta l_2 plus and so on up to B_K dot delta l_K](amperes-law.assets/eq-line-sum.svg)

Now make the steps shorter and more numerous. The sum settles to a fixed number, and that limit is
what the integral sign means:

![the closed line integral of B dot dl around C equals the limit as K goes to infinity of the sum of B_k dot delta l_k](amperes-law.assets/eq-line-int.svg)

![Animated Amperian loop: a step vector dl walks once around a circle centred on a wire, always parallel to the magnetic field, while the traced part of the loop grows](amperes-law.assets/fig-07-anim.svg)

_The integral as a walk. On a circle centred on the wire, the field at every step points exactly
along the step, so every step contributes ![B dl](amperes-law.assets/eq-inline/ed1d30e43f.svg)<!--m:B\,dl--> and the total is ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> times the circumference._

*A toy check.* Take a uniform field ![B_0](amperes-law.assets/eq-inline/7c42ee326e.svg)<!--m:B_0--> pointing along ![x](amperes-law.assets/eq-inline/11f6ad8ec5.svg)<!--m:x--> (no current anywhere) and a square
loop of side ![a](amperes-law.assets/eq-inline/86f7e437fa.svg)<!--m:a--> with two sides along ![x](amperes-law.assets/eq-inline/11f6ad8ec5.svg)<!--m:x-->. Walk it. The bottom side runs with the field, ![+B_0a](amperes-law.assets/eq-inline/6bc5061586.svg)<!--m:+B_0a-->. The
top side runs against it, ![-B_0a](amperes-law.assets/eq-inline/2786fbe650.svg)<!--m:-B_0a-->. The two vertical sides are at right angles to the field and give
zero:

![the integral equals B_0 a times plus 1, plus 0, plus B_0 a times minus 1, plus 0, equals 0](amperes-law.assets/eq-uniform-square.svg)

Zero, as the law demands for a loop with no current through it. Note that zero total does **not**
mean zero field along the loop. The field here is ![B_0](amperes-law.assets/eq-inline/7c42ee326e.svg)<!--m:B_0--> everywhere; the contributions cancel.

**The enclosed current.** Imagine a soap film stretched across the loop. Any surface whose edge is
the loop will do. Every wire that pierces the film carries current through the loop. Add these
currents up, with signs:

- **The sign comes from the right hand.** Curl the fingers of your right hand along the direction
  you walk the loop. Current crossing the film in the direction of your thumb counts **plus**;
  current crossing the other way counts **minus**.
- **A current outside the loop counts zero.** It still changes the field at every point of the
  loop, but its contributions to the integral cancel exactly (proved below).
- **A wire that passes through the loop several times counts every time.** A coil of ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns
  threading the loop contributes ![NI](amperes-law.assets/eq-inline/364aa96a89.svg)<!--m:NI-->. This is what makes coils strong (§9).

![Left: an Amperian loop enclosing a 5 A current out of the page and a 3 A current into the page, with a 4 A current outside; the enclosed current is 2 A. Right: for any loop around a wire, B dot dl equals mu_0 I over 2 pi times d phi, so only the angle swept counts](amperes-law.assets/fig-08.svg)

_Panel (a): walking anticlockwise means out of the page counts plus. The 5 A counts ![+5](amperes-law.assets/eq-inline/af64fe7e1f.svg)<!--m:+5-->, the 3 A
going into the page counts ![-3](amperes-law.assets/eq-inline/def03a29bf.svg)<!--m:-3-->, and the 4 A outside the loop counts nothing. Panel (b): why the
loop's shape cannot matter._

*Worked enclosed current (panel a):*

![I enclosed equals plus 5 amperes plus minus 3 amperes equals plus 2 amperes, so the closed line integral equals mu_0 times 2 amperes, about 2.51 microtesla metres](amperes-law.assets/eq-enc-example.svg)

**Why the shape of the loop does not matter.** Prove it for a single long wire, using the field
from §5. Look at panel (b). Any short step ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> on the loop can be split into a part pointing
straight away from the wire and a part going around it. The field of a straight wire points
purely *around* (§3), so only the second part counts in ![B times d l](amperes-law.assets/eq-inline/08375c2981.svg)<!--m:\vec{B}\cdot d\vec{l}-->. If the step moves
the angle seen from the wire by ![d phi](amperes-law.assets/eq-inline/47ed26625f.svg)<!--m:d\varphi-->, that around-part has length ![r d phi](amperes-law.assets/eq-inline/eea6105152.svg)<!--m:r\,d\varphi--> (arc length is
radius times angle). So:

![B dot dl equals B times r d phi, equals mu_0 I over 2 pi r times r d phi, equals mu_0 I over 2 pi times d phi](amperes-law.assets/eq-shape-proof-1.svg)

The distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> has cancelled. A far-away step has a weaker field but sweeps the same angle with a
longer path, and the two effects balance exactly. So the integral only counts the total angle
swept:

![the closed line integral equals mu_0 I over 2 pi times the closed integral of d phi, which equals mu_0 I if the loop encircles the wire and 0 if it does not](amperes-law.assets/eq-shape-proof-2.svg)

A loop that goes once around the wire sweeps a full turn, ![2 pi](amperes-law.assets/eq-inline/0833718ca4.svg)<!--m:2\pi-->. A loop that does not go around the
wire swings out and back and sweeps zero. That is Ampère's law for a straight wire and any loop at
all. For a field made by many currents, add the fields: each current contributes ![mu_0 I](amperes-law.assets/eq-inline/fb628ff051.svg)<!--m:\mu_0 I--> if it is
enclosed and zero if not. (The general proof for any shape of steady current, starting from
Biot–Savart, is in Griffiths §5.3; see §16.)

**Why the choice of loop matters.** Ampère's law is true for *every* loop, but it hands you the
field only when you can pull ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> out of the integral. That needs a loop on which, piece by piece,
one of three things holds:

1. the field points along the loop and has the **same size** all along that piece, so the piece
   contributes ![B times](amperes-law.assets/eq-inline/44c68db74e.svg)<!--m:B\times--> its length;
2. the field is **at right angles** to the loop on that piece, so it contributes zero;
3. the field is **zero** on that piece.

Symmetry is what tells you this in advance. Take a square loop around a straight wire instead of a
circle. Ampère's law still says the integral is ![mu_0 I](amperes-law.assets/eq-inline/fb628ff051.svg)<!--m:\mu_0 I-->. But along each side the field changes
size and angle from point to point, so you cannot pull ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> out and the law tells you nothing
about ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B-->. The useful geometries are the symmetric ones: the long straight wire and round conductor
(§7, §8), the long solenoid (§9), the toroid (§10) and the coaxial cable (§11). For everything
else, such as a short coil, a square loop or a PCB track, you are back to Biot–Savart or to a
computer.

> **Watch out —** This form of the law is for **steady** currents (magnetostatics), currents that
> do not change with time and that flow around closed circuits. For a current that is changing,
> or one that stops at a capacitor plate, it gives the wrong answer. §14 shows the term Maxwell
> added to fix it.

## 7 The straight wire again by Ampere's law

Now the payoff. The field of a long straight wire, by Ampère's law.

**Symmetry first.** Before computing anything, decide what the field must look like.

- **It has no part pointing along the wire.** In Biot–Savart, ![d l times s](amperes-law.assets/eq-inline/3cea2eff24.svg)<!--m:d\vec{l}\times\hat{s}--> is perpendicular to
  ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}-->, which points along the wire. No piece can push field along the wire.
- **It has no part pointing away from or toward the wire.** If it had, field lines would stream out
  of (or into) the wire all along its length. But magnetic field lines never start or end (§1).
- **Its size depends only on the distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r-->.** Rotate the long wire about its own axis, or slide it
  along itself, and nothing changes, so the field cannot change either.

So the field points *around* the wire and has the same size everywhere on a circle of radius ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r-->
centred on the wire. Choose that circle as the Amperian loop, walked in the direction of the
right-hand grip rule. It meets condition 1 of §6 at every point.

**Step 1.** The field is parallel to every step, so the dot product is just the product of sizes:

![closed integral of B dot dl equals closed integral of B dl, because B is parallel to dl everywhere on the circle](amperes-law.assets/eq-amp-wire-1.svg)

**Step 2.** ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> is the same at every point of the circle, so it comes out of the integral:

![closed integral of B dl equals B times closed integral of dl, because B is the same at every point of the circle](amperes-law.assets/eq-amp-wire-2.svg)

**Step 3.** Adding up the step lengths around a circle gives its circumference, ![2 pi r](amperes-law.assets/eq-inline/45b3fba1a4.svg)<!--m:2\pi r-->. The only
current through the circle is the wire's, ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I-->:

![B times closed integral of dl equals B times 2 pi r equals mu_0 I, so B equals mu_0 I over 2 pi r](amperes-law.assets/eq-amp-wire-3.svg)

The same answer as eight steps of Biot–Savart in §5.

| | Biot–Savart (§5) | Ampère (§7) |
|---|---|---|
| What you do | add up every piece of the wire | walk one loop |
| Steps for the straight wire | eight, including a trigonometric substitution | three |
| Needs symmetry? | no — always works | yes — otherwise it is true but useless |
| Gives the field of a finite segment? | yes | no |
| Gives the direction too? | yes, from the cross product | no — that comes from the symmetry argument and the grip rule |
| Best for | short coils, loops, odd shapes | long wires, solenoids, toroids, coax |

**Worked numbers.** At ![1 cm](amperes-law.assets/eq-inline/1eef359098.svg)<!--m:1\ \mathrm{cm}--> from ![10 A](amperes-law.assets/eq-inline/c2205c3089.svg)<!--m:10\ \mathrm{A}--> the field is ![200 mu T](amperes-law.assets/eq-inline/d7d933dc42.svg)<!--m:200\ \mu\mathrm{T}--> (§5). For
Oersted's compass in Figure 101, ![2 A](amperes-law.assets/eq-inline/7ccc626d65.svg)<!--m:2\ \mathrm{A}--> flows south along a wire ![3 cm](amperes-law.assets/eq-inline/c0152843c5.svg)<!--m:3\ \mathrm{cm}--> above the
needle. Assume the horizontal part of the Earth's field is ![20 mu T](amperes-law.assets/eq-inline/da116a2863.svg)<!--m:20\ \mu\mathrm{T}--> (it ranges from almost
zero near the magnetic poles to about ![40 mu T](amperes-law.assets/eq-inline/681670ac0c.svg)<!--m:40\ \mu\mathrm{T}--> near the equator):

![B wire equals 2 times 10 to the minus 7 times 2 over 0.03, equals 13.3 microtesla; tan theta equals 13.3 over 20, so theta equals 33.7 degrees](amperes-law.assets/eq-compass-worked.svg)

The direction follows from the grip rule. Thumb pointing south along the wire, the fingers pass
*under* the wire, where the compass is, heading east. The needle settles ![33.7^ deg](amperes-law.assets/eq-inline/a09a021476.svg)<!--m:33.7^\circ--> east of north,
along the sum of the two fields.

## 8 Inside the conductor — B grows in proportion to r

A real wire has a radius. What is the field inside the copper? Take a round wire of radius ![R](amperes-law.assets/eq-inline/06576556d1.svg)<!--m:R-->
carrying a steady (DC) current ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> spread evenly over its cross-section. The current per square
metre of cross-section, the **current density** ![J](amperes-law.assets/eq-inline/58668e7669.svg)<!--m:J-->, is then

![J equals I over pi R squared, in amperes per square metre](amperes-law.assets/eq-current-density.svg)

The symmetry argument of §7 still holds inside, so take a circle of radius ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> *smaller* than ![R](amperes-law.assets/eq-inline/06576556d1.svg)<!--m:R-->
as the loop. It now encloses only the current flowing through its own area, ![pi r^2](amperes-law.assets/eq-inline/ace26b4800.svg)<!--m:\pi r^2-->:

![I enclosed equals J pi r squared, equals I over pi R squared times pi r squared, equals I r squared over R squared](amperes-law.assets/eq-inside-enc.svg)

The left side of Ampère's law is ![B(2 pi r)](amperes-law.assets/eq-inline/e90c722155.svg)<!--m:B(2\pi r)-->, exactly as before:

![B times 2 pi r equals mu_0 I r squared over R squared, so B equals mu_0 I r over 2 pi R squared, for r up to R](amperes-law.assets/eq-inside-B.svg)

Inside, the field grows **in proportion to** ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r-->: zero on the axis, largest at the surface. At ![r = R](amperes-law.assets/eq-inline/b42fd0293b.svg)<!--m:r = R-->
the inside formula gives ![mu_0 I/(2 pi R)](amperes-law.assets/eq-inline/4a49a2124b.svg)<!--m:\mu_0 I/(2\pi R)-->, the same value as the outside formula, so the field is
continuous through the surface of the wire. Reference charts quote this pair as
![B_in = mu_0 I r/(2 pi R^2)](amperes-law.assets/eq-inline/ff83010406.svg)<!--m:B_{in} = \mu_0 I r/(2\pi R^2)--> and ![B_out = mu_0 I/(2 pi r)](amperes-law.assets/eq-inline/81eb205b37.svg)<!--m:B_{out} = \mu_0 I/(2\pi r)-->.

![Magnetic flux density versus distance from the axis of a 1 mm radius wire carrying 10 A: rising linearly inside to 2 mT at the surface, then falling as one over r outside](amperes-law.assets/fig-09.svg)

_The field is largest at the surface of the wire, not at its centre. The dashed line shows what
![1/r](amperes-law.assets/eq-inline/525108fcf9.svg)<!--m:1/r--> would do if all the current sat on the axis: it would grow without limit. A real wire never
does, because a smaller loop encloses less current._

*Worked numbers.* ![10 A](amperes-law.assets/eq-inline/c2205c3089.svg)<!--m:10\ \mathrm{A}--> in a wire of radius ![R = 1 mm](amperes-law.assets/eq-inline/2c26c21884.svg)<!--m:R = 1\ \mathrm{mm}-->:

![B at R equals 2 times 10 to the minus 7 times 10 over 0.001, equals 2 millitesla; B at R over 2 equals B at 2R equals 1 millitesla](amperes-law.assets/eq-inside-worked.svg)

The field halfway to the axis equals the field one radius outside the surface.

> **Watch out —** "Spread evenly" is a DC assumption. At high frequency the current crowds toward
> the surface of the conductor (the **skin effect**, a consequence of [../faradays-law/](../faradays-law/)),
> and the field inside falls toward zero. The depth the current occupies, the skin depth ![delta](amperes-law.assets/eq-inline/3a6a16552e.svg)<!--m:\delta-->,
> depends on the copper's resistivity ![rho](amperes-law.assets/eq-inline/c77a25750c.svg)<!--m:\rho--> (![1.68 times 10^-8 Omega times m](amperes-law.assets/eq-inline/c00c792925.svg)<!--m:1.68\times10^{-8}\ \mathrm{\Omega\cdot m}-->) and the frequency ![f](amperes-law.assets/eq-inline/4a0a19218e.svg)<!--m:f-->.
> At ![50 kHz](amperes-law.assets/eq-inline/46ea3f1829.svg)<!--m:50\ \mathrm{kHz}--> in copper:
>
> ![delta equals the square root of rho over pi f mu_0, which for copper at 50 kilohertz is about 0.29 millimetres](amperes-law.assets/eq-skin-depth.svg)
>
> A 2 mm diameter wire at 50 kHz carries most of its current in the outer 0.3 mm. This is why
> switching-converter windings use thin strands (Litz wire) or copper foil.

## 9 The solenoid

A **solenoid** is a wire wound into a helix: ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns spread evenly along a length ![l](amperes-law.assets/eq-inline/07c342be6e.svg)<!--m:l-->. Call the
number of turns per metre ![n](amperes-law.assets/eq-inline/d1854cae89.svg)<!--m:n-->:

![n equals N over l, turns per metre, in inverse metres](amperes-law.assets/eq-turns-density.svg)

"Long" means much longer than its radius. For a long solenoid, the field is strong and uniform
inside, along the axis, and almost zero outside. The grip rule says why the field is strong inside.
In the cross-section of Figure 109, the top of every turn carries current out of the page (![](amperes-law.assets/eq-inline/c8e2d1a0bf.svg)<!--m:\odot-->),
and its field runs anticlockwise around it, which is rightward just below it, inside the coil. The
bottom of every turn carries current into the page (![](amperes-law.assets/eq-inline/72166555a6.svg)<!--m:\otimes-->), and its clockwise field also runs
rightward just above it, inside the coil. Inside, all the contributions add. Outside, the fields of
the top and bottom rows point in opposite directions and nearly cancel.

Ampère's law makes both claims exact for a long solenoid:

- **Uniform inside.** Draw a rectangle entirely *inside* the coil with two sides parallel to the
  axis. It encloses no current, so the integral is zero. The field has no part across the
  rectangle's short sides, so the two long sides must cancel. That means the field is the same on
  both, whatever their distance from the axis.
- **Zero outside.** Do the same with a rectangle entirely *outside*. The field is the same at every
  distance from the coil, and far away it must be zero. So it is zero everywhere outside.

Now take the rectangle ![abcd](amperes-law.assets/eq-inline/81fe8bfe87.svg)<!--m:abcd--> of Figure 109. The side ![ab](amperes-law.assets/eq-inline/da23614e02.svg)<!--m:ab--> of length ![h](amperes-law.assets/eq-inline/27d5482eeb.svg)<!--m:h--> runs inside the coil along
the field, and the side ![cd](amperes-law.assets/eq-inline/034778198a.svg)<!--m:cd--> runs outside. Walk it ![a to b to c to d](amperes-law.assets/eq-inline/358a4c211b.svg)<!--m:a\to b\to c\to d-->, which is anticlockwise in the
figure, so currents out of the page count plus.

![Solenoid cut along its axis: current out of the page along the top row and into the page along the bottom row, a strong uniform field inside, and a rectangular Amperian loop abcd with side ab inside the coil](amperes-law.assets/fig-10.svg)

_The rectangle straddles one row of wires. Only its inside side sees any field, and it is pierced by
every turn between ![a](amperes-law.assets/eq-inline/86f7e437fa.svg)<!--m:a--> and ![b](amperes-law.assets/eq-inline/e9d71f5ee7.svg)<!--m:b-->. In a real, finite coil the field returns around the outside, weak and
spread out, as the dashed lines show._

**Step 1 — split the loop into its four sides.**

![the closed integral around abcd equals the integral from a to b plus b to c plus c to d plus d to a](amperes-law.assets/eq-sol-1.svg)

**Step 2 — each side.** Along ![ab](amperes-law.assets/eq-inline/da23614e02.svg)<!--m:ab--> the field is uniform and along the path: ![Bh](amperes-law.assets/eq-inline/bc3566fc11.svg)<!--m:Bh-->. Along ![bc](amperes-law.assets/eq-inline/5b2505039a.svg)<!--m:bc--> and
![da](amperes-law.assets/eq-inline/cdd4f87409.svg)<!--m:da--> the part inside the coil is at right angles to the field, and the part outside is in zero
field, so both give zero. Along ![cd](amperes-law.assets/eq-inline/034778198a.svg)<!--m:cd--> the field is zero:

![integral from a to b equals B h; integrals from b to c and d to a are zero because B is perpendicular to dl or zero; integral from c to d is zero because B is zero](amperes-law.assets/eq-sol-2.svg)

**Step 3 — enclosed current.** The rectangle is ![h](amperes-law.assets/eq-inline/27d5482eeb.svg)<!--m:h--> long, so it is pierced by ![nh](amperes-law.assets/eq-inline/6eb03d8896.svg)<!--m:nh--> turns, each
carrying ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> out of the page:

![I enclosed equals n h times I](amperes-law.assets/eq-sol-3.svg)

**Step 4 — solve.**

![B h equals mu_0 n h I, so B equals mu_0 n I, which equals mu_0 N I over l](amperes-law.assets/eq-sol-4.svg)

The rectangle's length ![h](amperes-law.assets/eq-inline/27d5482eeb.svg)<!--m:h--> cancels, so any rectangle gives the same ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B-->. That is a check that the
field inside really is uniform. The field depends only on the **ampere-turns per metre**, ![nI](amperes-law.assets/eq-inline/a8200d5dee.svg)<!--m:nI-->, not on
the radius of the coil. A thin coil and a fat one with the same winding density have the same field
inside.

> **Watch out —** Many reference charts write the solenoid field as
> ![B = mu_0 N I](amperes-law.assets/eq-inline/cc591bf0a5.svg)<!--m:B = \mu_0 N I-->. There, ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> means turns **per unit length**, the ![n](amperes-law.assets/eq-inline/d1854cae89.svg)<!--m:n--> used here. With ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> as the
> total number of turns, as in this tree, the field is ![mu_0 N I/l](amperes-law.assets/eq-inline/3c450cb161.svg)<!--m:\mu_0 N I/l-->. Check which one a formula
> means before you put numbers in: the two differ by a factor of the coil's length.

**Worked number — 100 turns over 5 cm.** At ![I = 1 A](amperes-law.assets/eq-inline/66a0aa4c95.svg)<!--m:I = 1\ \mathrm{A}-->:

![n equals 100 over 0.05 metres equals 2000 per metre; B equals 4 pi times 10 to the minus 7 times 2000 times 1 ampere, about 2.51 millitesla](amperes-law.assets/eq-sol-worked.svg)

About fifty times the Earth's field, from 1 A, in air. At ![2 A](amperes-law.assets/eq-inline/7ccc626d65.svg)<!--m:2\ \mathrm{A}--> it is ![5.03 mT](amperes-law.assets/eq-inline/28466fa743.svg)<!--m:5.03\ \mathrm{mT}-->. Compare one
straight 1 A wire, which gives this field only ![80 mu m](amperes-law.assets/eq-inline/2d3cc0fe91.svg)<!--m:80\ \mu\mathrm{m}--> from its centre. Coiling the wire
concentrates its field.

**How long is "long"?** Ampère's law needs the coil to be infinitely long. To see how much a real
coil falls short, go back to Biot–Savart. Start with one circular turn of radius ![a](amperes-law.assets/eq-inline/86f7e437fa.svg)<!--m:a--> (m) and find
the field on its axis, a distance ![x](amperes-law.assets/eq-inline/11f6ad8ec5.svg)<!--m:x--> (m) from its centre. Every piece ![d l](amperes-law.assets/eq-inline/8d7f60aa83.svg)<!--m:d\vec{l}--> of the ring is at the
same distance ![s = sqrt a^2 + x^2](amperes-law.assets/eq-inline/b2476341ff.svg)<!--m:s = \sqrt{a^2 + x^2}--> and at right angles to ![s](amperes-law.assets/eq-inline/6a16290a6f.svg)<!--m:\vec{s}-->, so ![sin theta = 1](amperes-law.assets/eq-inline/42f6a8311f.svg)<!--m:\sin\theta = 1-->. The parts of
![d B](amperes-law.assets/eq-inline/32b0df1dba.svg)<!--m:d\vec{B}--> across the axis cancel between opposite sides of the ring. The part along the axis is a
fraction ![a/s](amperes-law.assets/eq-inline/50524724b7.svg)<!--m:a/s--> of each contribution:

![the axial part of dB equals mu_0 over 4 pi times I dl over s squared times a over s, with s equal to the square root of a squared plus x squared](amperes-law.assets/eq-ring-1.svg)

Everything except ![dl](amperes-law.assets/eq-inline/b612af11ef.svg)<!--m:dl--> is the same all round the ring, and the pieces add up to the circumference
![2 pi a](amperes-law.assets/eq-inline/642025ba7e.svg)<!--m:2\pi a-->:

![B ring equals mu_0 I a over 4 pi s cubed times the closed integral of dl, equals mu_0 I a over 4 pi s cubed times 2 pi a, equals mu_0 I a squared over 2 times a squared plus x squared to the three halves](amperes-law.assets/eq-ring-2.svg)

A solenoid is a stack of such rings, ![n dx](amperes-law.assets/eq-inline/474a7fd07e.svg)<!--m:n\,dx--> of them in each slice ![dx](amperes-law.assets/eq-inline/4a2b94f8a9.svg)<!--m:dx-->. At the centre of a coil of
length ![l](amperes-law.assets/eq-inline/07c342be6e.svg)<!--m:l-->, add the rings from ![x = -l/2](amperes-law.assets/eq-inline/67cc67dc2c.svg)<!--m:x = -l/2--> to ![+l/2](amperes-law.assets/eq-inline/fd45f72739.svg)<!--m:+l/2-->. The integral is the one from §5, with ![a](amperes-law.assets/eq-inline/86f7e437fa.svg)<!--m:a--> in
place of ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r-->. The substitution ![x = a tan phi](amperes-law.assets/eq-inline/276d27f16e.svg)<!--m:x = a\tan\varphi--> gives ![sin phi/a^2](amperes-law.assets/eq-inline/5fcb35956d.svg)<!--m:\sin\varphi/a^2-->, and ![sin phi = x/sqrt a^2 + x^2](amperes-law.assets/eq-inline/05aa0353f3.svg)<!--m:\sin\varphi = x/\sqrt{a^2 + x^2}-->:

![B centre equals the integral from minus l over 2 to plus l over 2 of mu_0 n dx I a squared over 2 times a squared plus x squared to the three halves, equals mu_0 n I a squared over 2 times x over a squared root of a squared plus x squared, between the limits](amperes-law.assets/eq-fin-1.svg)

Putting in the limits, and doing the same from ![x = 0](amperes-law.assets/eq-inline/11c20ca2d1.svg)<!--m:x = 0--> to ![x = l](amperes-law.assets/eq-inline/51ce4738b0.svg)<!--m:x = l--> for a point on the axis at one
end of the coil:

![B centre equals mu_0 n I times l over 2 over the square root of l over 2 squared plus a squared; B end equals mu_0 n I over 2 times l over the square root of l squared plus a squared](amperes-law.assets/eq-fin-2.svg)

As ![l](amperes-law.assets/eq-inline/07c342be6e.svg)<!--m:l--> grows much larger than ![a](amperes-law.assets/eq-inline/86f7e437fa.svg)<!--m:a-->, the centre factor goes to 1 (the ideal result) and the end factor
goes to one half. For the 100-turn, 5 cm coil wound with radius ![a = 1 cm](amperes-law.assets/eq-inline/0a7ecf13bf.svg)<!--m:a = 1\ \mathrm{cm}-->:

![B centre equals 2.51 millitesla times 2.5 over the square root of 2.5 squared plus 1 squared, about 2.33 millitesla; B end equals 2.51 millitesla over 2 times 5 over the square root of 5 squared plus 1, about 1.23 millitesla](amperes-law.assets/eq-fin-worked.svg)

The ideal formula is 7 % high at the centre of this stubby coil, and the field at its ends is
about half the centre value.

## 10 The toroid

A **toroid** is a solenoid bent round until its ends meet: ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns wound evenly around a ring.
In Figure 110 the ring has inner radius ![a](amperes-law.assets/eq-inline/86f7e437fa.svg)<!--m:a--> and outer radius ![b](amperes-law.assets/eq-inline/e9d71f5ee7.svg)<!--m:b-->. The problem has rotational
symmetry about the ring's centre, so the field circles that centre and has the same size at every
point a distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> from it. So take circles of radius ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> about the centre as Amperian loops. On
each one, ![loop integral B times d l = B (2 pi r)](amperes-law.assets/eq-inline/2ae43ef40b.svg)<!--m:\oint\vec{B}\cdot d\vec{l} = B\,(2\pi r)-->. Only the enclosed current changes from region to
region.

![Toroid seen from above: turns wound around a ring core, the field circling inside the core, and three circular Amperian loops: in the hole, inside the core, and outside, with field zero, mu_0 N I over 2 pi r, and zero](amperes-law.assets/fig-11.svg)

_Every turn passes up through the hole (![](amperes-law.assets/eq-inline/c8e2d1a0bf.svg)<!--m:\odot--> on the inner edge) and back down outside (![](amperes-law.assets/eq-inline/72166555a6.svg)<!--m:\otimes--> on the
outer edge). A loop in the core encloses all ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> upward passes. A loop in the hole encloses nothing.
A loop outside encloses ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> up and ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> down, so the net is zero._

**Loop (1), in the hole, ![r < a](amperes-law.assets/eq-inline/34a3ef1d06.svg)<!--m:r < a-->.** No wire passes through it:

![for r less than a, B times 2 pi r equals mu_0 times 0, so B equals 0](amperes-law.assets/eq-tor-1.svg)

**Loop (2), inside the core, ![a < r < b](amperes-law.assets/eq-inline/87fa969224.svg)<!--m:a < r < b-->.** Every one of the ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> turns passes through it once, the
same way:

![for r between a and b, B times 2 pi r equals mu_0 N I, so B equals mu_0 N I over 2 pi r](amperes-law.assets/eq-tor-2.svg)

**Loop (3), outside, ![r > b](amperes-law.assets/eq-inline/b7ad5842c2.svg)<!--m:r > b-->.** Each turn passes through the loop's disc twice, once up through the
hole and once down outside:

![for r greater than b, B times 2 pi r equals mu_0 times N I minus N I, equals 0, so B equals 0](amperes-law.assets/eq-tor-3.svg)

The field exists only inside the windings. In the core it falls as ![1/r](amperes-law.assets/eq-inline/525108fcf9.svg)<!--m:1/r-->, being strongest at the
inner edge. Reference charts often write ![B = mu_0 N I/(2 pi R)](amperes-law.assets/eq-inline/7971206303.svg)<!--m:B = \mu_0 N I/(2\pi R)--> with ![R](amperes-law.assets/eq-inline/06576556d1.svg)<!--m:R--> the mean radius. For a
thin ring, ![2 pi R](amperes-law.assets/eq-inline/f969d5632d.svg)<!--m:2\pi R--> is the length of the core's centre line, and the toroid formula becomes the
solenoid formula ![mu_0 N I/l](amperes-law.assets/eq-inline/3c450cb161.svg)<!--m:\mu_0 N I/l--> with ![l = 2 pi R](amperes-law.assets/eq-inline/1945a0b434.svg)<!--m:l = 2\pi R-->.

*Worked numbers.* ![N = 100](amperes-law.assets/eq-inline/3822a6daa4.svg)<!--m:N = 100--> turns at ![I = 1 A](amperes-law.assets/eq-inline/66a0aa4c95.svg)<!--m:I = 1\ \mathrm{A}--> on a ring with ![a = 1 cm](amperes-law.assets/eq-inline/0a7ecf13bf.svg)<!--m:a = 1\ \mathrm{cm}--> and
![b = 1.5 cm](amperes-law.assets/eq-inline/4c8cba4acd.svg)<!--m:b = 1.5\ \mathrm{cm}-->:

![B at a equals 2 times 10 to the minus 7 times 100 times 1 over 0.010, equals 2.00 millitesla; B at b equals the same over 0.015, equals 1.33 millitesla](amperes-law.assets/eq-tor-worked.svg)

At the mean radius, ![1.25 cm](amperes-law.assets/eq-inline/774fe48973.svg)<!--m:1.25\ \mathrm{cm}-->, it is ![1.6 mT](amperes-law.assets/eq-inline/bc567f4be3.svg)<!--m:1.6\ \mathrm{mT}-->. The field varies by ![plus-minus 20 %](amperes-law.assets/eq-inline/89314ea910.svg)<!--m:\pm 20\,\%-->
across this fairly fat ring. A thinner ring is more uniform.

> **Tip —** This is why so many power inductors and current transformers are toroids. Their field
> stays inside the core, so they neither spray field onto neighbouring circuits nor pick it up.
> (Strictly, the winding also creeps once around the ring as it goes, which adds the field of one
> single turn of radius ![R](amperes-law.assets/eq-inline/06576556d1.svg)<!--m:R--> outside. It is ![N](amperes-law.assets/eq-inline/b51a60734d.svg)<!--m:N--> times weaker than the field inside.)

## 11 The coaxial cable

A **coaxial cable** has a solid inner conductor of radius ![a](amperes-law.assets/eq-inline/86f7e437fa.svg)<!--m:a--> carrying current ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> one way, and a
tubular outer conductor (inner radius ![b](amperes-law.assets/eq-inline/e9d71f5ee7.svg)<!--m:b-->, outer radius ![c](amperes-law.assets/eq-inline/84a516841b.svg)<!--m:c-->) carrying the same current back. Assume
each current is spread evenly over its own conductor (DC). The symmetry is the same as for the
wire, so take circles of radius ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> centred on the axis. Every loop gives ![B(2 pi r)](amperes-law.assets/eq-inline/e90c722155.svg)<!--m:B(2\pi r)-->; only the
enclosed current changes.

**Region (1), inside the centre conductor.** This is §8 with ![R = a](amperes-law.assets/eq-inline/6d3a22c548.svg)<!--m:R = a-->:

![for r less than a, B equals mu_0 I r over 2 pi a squared](amperes-law.assets/eq-coax-1.svg)

**Region (2), in the gap.** The loop encloses all of the inner current and none of the outer:

![for r between a and b, B equals mu_0 I over 2 pi r](amperes-law.assets/eq-coax-2.svg)

**Region (3), inside the outer conductor.** The loop encloses the inner current minus the part of
the return current that flows inside radius ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r-->. The return current is spread over the area
![pi (c^2 - b^2)](amperes-law.assets/eq-inline/e44bf75a99.svg)<!--m:\pi(c^2 - b^2)-->, and the loop takes in the area ![pi (r^2 - b^2)](amperes-law.assets/eq-inline/9266bd39b3.svg)<!--m:\pi(r^2 - b^2)--> of it:

![for r between b and c, I enclosed equals I minus I times pi r squared minus b squared over pi c squared minus b squared, equals I times c squared minus r squared over c squared minus b squared](amperes-law.assets/eq-coax-3a.svg)

![B equals mu_0 I over 2 pi r times c squared minus r squared over c squared minus b squared](amperes-law.assets/eq-coax-3b.svg)

**Region (4), outside.** The two currents are equal and opposite:

![for r greater than c, I enclosed equals I minus I equals 0, so B equals 0](amperes-law.assets/eq-coax-4.svg)

![Coaxial cable: cross-section with inner conductor carrying current out of the page and outer conductor carrying it back, and a plot of B versus r that rises inside, falls as one over r in the gap, drops to zero across the outer conductor, and is zero outside](amperes-law.assets/fig-12.svg)

_All of the field is trapped inside the cable. With ![a = 1 mm](amperes-law.assets/eq-inline/4891919f35.svg)<!--m:a = 1\ \mathrm{mm}-->, ![b = 3 mm](amperes-law.assets/eq-inline/fbb0784689.svg)<!--m:b = 3\ \mathrm{mm}-->,
![c = 3.5 mm](amperes-law.assets/eq-inline/801622ac48.svg)<!--m:c = 3.5\ \mathrm{mm}--> and ![10 A](amperes-law.assets/eq-inline/c2205c3089.svg)<!--m:10\ \mathrm{A}-->, the field peaks at ![2 mT](amperes-law.assets/eq-inline/7ecabd5f5f.svg)<!--m:2\ \mathrm{mT}--> at the surface of the centre
conductor, is ![0.67 mT](amperes-law.assets/eq-inline/2b04744060.svg)<!--m:0.67\ \mathrm{mT}--> at the inside of the shield, and is zero outside._

**What it buys you.** A cable with no external field neither radiates magnetic interference nor
picks it up. A changing outside field threads no flux between its conductors. The same idea, done
less perfectly, is behind twisted pairs and behind running every supply wire right next to its
return (§15).

**Inductance per metre.** The field in the gap gives the cable's inductance. The **inductance** ![L](amperes-law.assets/eq-inline/d160e0986a.svg)<!--m:L-->
of a circuit is the magnetic flux ![Phi](amperes-law.assets/eq-inline/b51f9a1a7f.svg)<!--m:\Phi--> it links per ampere, ![L = Phi/I](amperes-law.assets/eq-inline/494f2f8c16.svg)<!--m:L = \Phi/I-->, in henries. (It is
developed in [../inductor/](../inductor/) and
[../electromagnetism/electromagnetism.md §9](../electromagnetism/electromagnetism.md#9-self-inductance--deriving-the-inductor-law).)
Count the flux crossing a strip one metre long, running radially from the inner conductor to the
outer one. A slice of that strip of width ![dr](amperes-law.assets/eq-inline/05c77e49f6.svg)<!--m:dr--> has area ![1 m times dr](amperes-law.assets/eq-inline/3b1de6bd9f.svg)<!--m:1\ \mathrm{m}\times dr-->, and the field crosses
it at right angles, so the flux per metre of cable, ![Phi '](amperes-law.assets/eq-inline/38fb958fbc.svg)<!--m:\Phi'--> in ![Wb times m^-1](amperes-law.assets/eq-inline/fcc06493ae.svg)<!--m:\mathrm{Wb\cdot m^{-1}}-->, is

![Phi prime equals the integral from a to b of B dr, equals mu_0 I over 2 pi times the integral of dr over r, equals mu_0 I over 2 pi times the natural log of b over a](amperes-law.assets/eq-coax-flux.svg)

![L prime equals Phi prime over I, equals mu_0 over 2 pi times ln of b over a, equals 2 times 10 to the minus 7 times ln 3, about 220 nanohenries per metre](amperes-law.assets/eq-coax-L.svg)

This counts only the flux in the gap; the small amount inside the conductors is ignored. The answer
is close to the roughly ![250 nH times m^-1](amperes-law.assets/eq-inline/281dc4b836.svg)<!--m:250\ \mathrm{nH\cdot m^{-1}}--> quoted for common 50-ohm cable. Note the logarithm.
A fatter cable with the same ratio ![b/a](amperes-law.assets/eq-inline/42da8cad59.svg)<!--m:b/a--> has the same inductance per metre.

## 12 H, permeability and ferromagnetic cores

Everything so far was in air or vacuum. Put iron or ferrite inside a coil and the field grows by a
factor of hundreds to thousands. That changes Ampère's law in a way that needs new bookkeeping.

**Why materials respond.** Every atom carries tiny circulating currents: its electrons orbit and
spin, and each one is a microscopic current loop with its own field. In most materials these loops
point every which way and cancel. In an applied field they line up a little: slightly with the
field in *paramagnetic* materials such as aluminium, slightly against it in *diamagnetic* ones such
as copper. In **ferromagnetic** materials (iron, nickel, cobalt, and ferrites, ceramics made with
iron oxide), neighbouring atoms line up together in regions called *domains*. A modest applied
field swings whole domains into line, and the material's own field becomes far larger than the
applied one. This is also where a permanent magnet's field comes from.

**Free and bound current.** Ampère's law with ![B](amperes-law.assets/eq-inline/84dd0d2d09.svg)<!--m:\vec{B}--> counts *every* current through the loop,
including those atomic loops. They are called **bound** currents, and you cannot connect a meter
to them. The currents you control, in the wires, are called **free** currents. To keep them apart,
describe how strongly the material is magnetised by its **magnetisation** ![M](amperes-law.assets/eq-inline/285e310d23.svg)<!--m:\vec{M}-->, the atomic
magnetic moment per cubic metre, in ![A times m^-1](amperes-law.assets/eq-inline/8d73c714f8.svg)<!--m:\mathrm{A\cdot m^{-1}}-->. Then define a second field, ![H](amperes-law.assets/eq-inline/badac9158b.svg)<!--m:\vec{H}-->, by

![B equals mu_0 times H plus M](amperes-law.assets/eq-b-h-m.svg)

![H](amperes-law.assets/eq-inline/badac9158b.svg)<!--m:\vec{H}--> is called the **magnetic field strength** (or simply the H-field), in ![A times m^-1](amperes-law.assets/eq-inline/8d73c714f8.svg)<!--m:\mathrm{A\cdot m^{-1}}-->. With
this definition the bound currents drop out of Ampère's law, and ![H](amperes-law.assets/eq-inline/badac9158b.svg)<!--m:\vec{H}--> obeys a version that counts
only the free current (see Griffiths §6.3 for the proof):

![closed line integral of H dot dl around C equals the free current enclosed](amperes-law.assets/eq-h-ampere.svg)

So **![H](amperes-law.assets/eq-inline/7cf184f4c6.svg)<!--m:H--> is set by your wires alone**, whatever material is around them, and its unit, amperes per
metre, says so: ampere-turns per metre of path. **![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> is what results** once the material has
responded. It is ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> that pushes on charges, saturates cores and appears in Faraday's law.

**Permeability.** In many materials, as long as the field is not too strong, the magnetisation is
proportional to ![H](amperes-law.assets/eq-inline/7cf184f4c6.svg)<!--m:H-->: ![M = chi_m H](amperes-law.assets/eq-inline/5c7debe7f5.svg)<!--m:\vec{M} = \chi_m\vec{H}-->, where the pure number ![chi_m](amperes-law.assets/eq-inline/306e1d43c5.svg)<!--m:\chi_m--> is the
*magnetic susceptibility*. Then

![M equals chi_m H, so B equals mu_0 times 1 plus chi_m times H, which equals mu_0 mu_r H, which equals mu H; H equals B over mu](amperes-law.assets/eq-b-mu-h.svg)

- ![mu_r = 1 + chi_m](amperes-law.assets/eq-inline/28cbd06d7d.svg)<!--m:\mu_r = 1 + \chi_m--> — the **relative permeability**, a pure number;
- ![mu = mu_0 mu_r](amperes-law.assets/eq-inline/62cb256de9.svg)<!--m:\mu = \mu_0\mu_r--> — the **permeability** of the material, in ![H times m^-1](amperes-law.assets/eq-inline/94372b463a.svg)<!--m:\mathrm{H\cdot m^{-1}}-->;
- ![H = B/mu](amperes-law.assets/eq-inline/7f199a74d3.svg)<!--m:H = B/\mu--> — the same relation solved for ![H](amperes-law.assets/eq-inline/7cf184f4c6.svg)<!--m:H-->, valid only in the linear range.

| Material | Relative permeability |
|---|---|
| vacuum, air | 1 (air: 1.0000004) |
| copper (diamagnetic) | 0.99999 |
| aluminium (paramagnetic) | 1.00002 |
| power ferrite (MnZn) | about 2000–3000 |
| silicon transformer steel | several thousand |
| nickel-iron alloys (mu-metal) | up to about 100 000 |

**Worked number — the same coil on a core.** Wind the 100 turns of §9 on a closed ring of ferrite
whose centre line is ![l = 5 cm](amperes-law.assets/eq-inline/9381537411.svg)<!--m:l = 5\ \mathrm{cm}--> long, and pass ![1 A](amperes-law.assets/eq-inline/4bd22980e0.svg)<!--m:1\ \mathrm{A}-->. Walk the H-field version of
Ampère's law around the centre line of the core. The loop is pierced by all 100 turns:

![H times l equals N I, so H equals N I over l, equals 100 times 1 ampere over 0.05 metres, equals 2000 amperes per metre](amperes-law.assets/eq-core-h.svg)

The same ![H](amperes-law.assets/eq-inline/7cf184f4c6.svg)<!--m:H--> as the air-cored solenoid. Now the material decides ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B-->. Take ![mu_r = 2000](amperes-law.assets/eq-inline/e09d61eab4.svg)<!--m:\mu_r = 2000-->:

![B in air equals mu_0 H, about 2.51 millitesla; B in ferrite equals mu_0 mu_r H, equals 2000 times 2.51 millitesla, about 5.0 tesla, as asked for](amperes-law.assets/eq-core-b.svg)

"Asked for" is deliberate. No ferrite can deliver 5 T. Its domains are all lined up by roughly
![0.35](amperes-law.assets/eq-inline/ff0e46b0b0.svg)<!--m:0.35--> to ![0.4 T](amperes-law.assets/eq-inline/5d55849b40.svg)<!--m:0.4\ \mathrm{T}-->. Beyond that it **saturates**, its permeability collapses toward that of air,
and ![B = mu H](amperes-law.assets/eq-inline/032478bfa3.svg)<!--m:B = \mu H--> with a constant ![mu](amperes-law.assets/eq-inline/3a4e56595d.svg)<!--m:\mu--> stops being true. On this core the current that reaches a safe
![0.3 T](amperes-law.assets/eq-inline/29dad7b555.svg)<!--m:0.3\ \mathrm{T}--> is only

![the current for 0.3 tesla equals B l over mu_0 mu_r N, equals 0.3 times 0.05 over 4 pi times 10 to the minus 7 times 2000 times 100, about 60 milliamperes](amperes-law.assets/eq-core-sat.svg)

A core multiplies the field a coil makes by thousands, but it also caps it. The B–H curve, the
hysteresis loop, and the air gaps that stop a core saturating are covered in
[../electromagnetism/electromagnetism.md §5 and §12](../electromagnetism/electromagnetism.md#5-h-permeability-and-why-the-core-matters).

> **Watch out —** The factor ![mu_r](amperes-law.assets/eq-inline/de4a3aca4d.svg)<!--m:\mu_r--> applies only when the field's whole path is inside the
> material, as in a closed ring. A straight ferrite rod inside a solenoid gains far less, because
> the field must still return through the air outside, and the air path dominates. This is the
> same reason a tiny air gap in a ring core matters so much (electromagnetism §5, reluctance).

## 13 The force between parallel wires and the old ampere

Ampère's first experiment after hearing of Oersted's was to show that two currents push on each
other. With §1 and §7 the force follows in two lines. Two long parallel wires a distance ![d](amperes-law.assets/eq-inline/3c363836cf.svg)<!--m:d--> apart
carry currents ![I_1](amperes-law.assets/eq-inline/d572d898ae.svg)<!--m:I_1--> and ![I_2](amperes-law.assets/eq-inline/982743db73.svg)<!--m:I_2-->. Wire 2 sits in the field of wire 1, which is at right angles to wire
2 (it circles wire 1). So a length ![L](amperes-law.assets/eq-inline/d160e0986a.svg)<!--m:L--> of wire 2 feels the straight-wire force of §1:

![B_1 equals mu_0 I_1 over 2 pi d; the force on wire 2 equals I_2 L B_1](amperes-law.assets/eq-par-1.svg)

Substituting, the force per metre of wire, in ![N times m^-1](amperes-law.assets/eq-inline/d8aacfac92.svg)<!--m:\mathrm{N\cdot m^{-1}}-->, is

![F over L equals mu_0 I_1 I_2 over 2 pi d](amperes-law.assets/eq-par-2.svg)

The same expression comes out for the force on wire 1 in the field of wire 2, equal and opposite,
as Newton's third law requires.

![Two parallel wires: currents in the same direction attract, currents in opposite directions repel; each wire sits in the other wire field and feels a force I L cross B](amperes-law.assets/fig-13.svg)

_Use ![d F = I d l times B](amperes-law.assets/eq-inline/a9b3e0f691.svg)<!--m:d\vec{F} = I\,d\vec{l}\times\vec{B}--> on each wire with the other wire's field. Currents in the
same direction attract. Opposite currents repel. Opposite charges attract; parallel currents attract.
Electricity and magnetism have opposite sign rules here._

**The old definition of the ampere.** Put ![1 A](amperes-law.assets/eq-inline/4bd22980e0.svg)<!--m:1\ \mathrm{A}--> in each wire, ![1 m](amperes-law.assets/eq-inline/75c737356d.svg)<!--m:1\ \mathrm{m}--> apart:

![F over L equals 4 pi times 10 to the minus 7 times 1 times 1 over 2 pi times 1, equals 2 times 10 to the minus 7 newtons per metre](amperes-law.assets/eq-par-worked.svg)

From 1948 until 20 May 2019, this *was* the definition of the ampere:

> the constant current which, if maintained in two straight parallel conductors of infinite length,
> of negligible circular cross-section, and placed 1 metre apart in vacuum, would produce between
> these conductors a force equal to ![2 times 10^-7](amperes-law.assets/eq-inline/46e6cd4c0f.svg)<!--m:2\times10^{-7}--> newton per metre of length.

That definition is why ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> was exactly ![4 pi times 10^-7 H times m^-1](amperes-law.assets/eq-inline/ed33d40c80.svg)<!--m:4\pi\times10^{-7}\ \mathrm{H\cdot m^{-1}}-->: the number was chosen,
not measured. Since 2019 the SI fixes the elementary charge instead,
![e = 1.602 176 634 times 10^-19 C](amperes-law.assets/eq-inline/1b88d0c6e0.svg)<!--m:e = 1.602\,176\,634\times10^{-19}\ \mathrm{C}--> exactly, and defines ![1 A = 1 C times s^-1](amperes-law.assets/eq-inline/1ac70fbaf0.svg)<!--m:1\ \mathrm{A} = 1\ \mathrm{C\cdot s^{-1}}-->
from it. The wire force is now a measurement of ![mu_0](amperes-law.assets/eq-inline/7cb4a998a7.svg)<!--m:\mu_0--> rather than a definition (§4).

**Where the force matters.** On a circuit board it does not: ![20 A](amperes-law.assets/eq-inline/4a8da715d9.svg)<!--m:20\ \mathrm{A}--> in two tracks ![1 mm](amperes-law.assets/eq-inline/9b7a2cdcb9.svg)<!--m:1\ \mathrm{mm}-->
apart gives ![0.08 N times m^-1](amperes-law.assets/eq-inline/6815ac348a.svg)<!--m:0.08\ \mathrm{N\cdot m^{-1}}-->. In switchgear it matters a great deal. The force grows as the
*square* of the current, and in a short circuit the current can briefly reach tens of kiloamperes.
Two busbars ![5 cm](amperes-law.assets/eq-inline/2ae14f3384.svg)<!--m:5\ \mathrm{cm}--> apart carrying a ![20 kA](amperes-law.assets/eq-inline/0eeecec6c5.svg)<!--m:20\ \mathrm{kA}--> fault current:

![F over L equals 2 times 10 to the minus 7 times 2 times 10 to the 4 squared over 0.05, equals 1600 newtons per metre](amperes-law.assets/eq-par-fault.svg)

That is about 160 kg-force on every metre of bar, applied suddenly. Busbar supports and transformer
windings are braced for exactly this.

## 14 Maxwell's correction — displacement current

Ampère's law as written so far has a hole, and a capacitor shows it. Charge a capacitor through a
wire, and draw an Amperian loop ![C](amperes-law.assets/eq-inline/32096c2e0e.svg)<!--m:C--> around the wire. The law says to count the current through
*any* surface bounded by ![C](amperes-law.assets/eq-inline/32096c2e0e.svg)<!--m:C-->. Take the flat disc ![S_1](amperes-law.assets/eq-inline/8d89fb9718.svg)<!--m:S_1-->: the wire pierces it, so ![I_enc = I](amperes-law.assets/eq-inline/756ede3034.svg)<!--m:I_{enc} = I-->. Now
stretch the surface into a bag ![S_2](amperes-law.assets/eq-inline/ce8d6ef281.svg)<!--m:S_2--> that passes between the capacitor plates. No charge crosses
the gap, so ![I_enc = 0](amperes-law.assets/eq-inline/50a33e5018.svg)<!--m:I_{enc} = 0-->. Same loop, same field along it, two different answers.

![A charging capacitor: one Amperian loop around the wire bounds a flat surface pierced by the current and a bulging surface that passes between the plates, where only the changing electric field crosses](amperes-law.assets/fig-14.svg)

_Something does cross ![S_2](amperes-law.assets/eq-inline/ce8d6ef281.svg)<!--m:S_2-->: the electric field between the plates, which grows as charge builds
up. Maxwell counted its rate of change as a current._

In 1861–65 James Clerk Maxwell added a term that counts a changing **electric flux** as if it were
a current. This is the **Ampère–Maxwell law**:

![closed line integral of B dot dl around C equals mu_0 times I enclosed plus epsilon_0 d Phi_E by dt, where Phi_E is the surface integral of E dot dA](amperes-law.assets/eq-ampere-maxwell.svg)

- ![E](amperes-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> — the electric field, in ![V times m^-1](amperes-law.assets/eq-inline/b6f586582e.svg)<!--m:\mathrm{V\cdot m^{-1}}--> ([../coulombs-law/](../coulombs-law/));
- ![Phi_E](amperes-law.assets/eq-inline/ca9214b981.svg)<!--m:\Phi_E--> — the **electric flux** through the surface, the normal part of ![E](amperes-law.assets/eq-inline/140990525e.svg)<!--m:\vec{E}--> added up over the
  surface's area ![A](amperes-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A-->, in ![V times m](amperes-law.assets/eq-inline/18961962e1.svg)<!--m:\mathrm{V\cdot m}-->;
- ![epsilon_0](amperes-law.assets/eq-inline/961a0cda39.svg)<!--m:\varepsilon_0--> — the permittivity of free space, ![8.854 times 10^-12 F times m^-1](amperes-law.assets/eq-inline/2c4d1e7472.svg)<!--m:8.854\times10^{-12}\ \mathrm{F\cdot m^{-1}}-->
  ([../coulombs-law/](../coulombs-law/));
- ![epsilon_0 d Phi_E/dt](amperes-law.assets/eq-inline/1a47231470.svg)<!--m:\varepsilon_0\,d\Phi_E/dt--> — the **displacement current**, in amperes. Check the unit, using
  ![1 F = 1 C times V^-1](amperes-law.assets/eq-inline/08f453b938.svg)<!--m:1\ \mathrm{F} = 1\ \mathrm{C\cdot V^{-1}}--> (a farad stores one coulomb per volt):

![the unit of epsilon_0 d Phi_E by dt is farads per metre times volt metres per second, equals farad volts per second, equals coulombs per second, equals amperes](amperes-law.assets/eq-disp-units.svg)

**It fixes the capacitor exactly.** Between parallel plates of area ![A](amperes-law.assets/eq-inline/6dcd4ce23d.svg)<!--m:A--> carrying charge ![plus-minus Q](amperes-law.assets/eq-inline/5492734c68.svg)<!--m:\pm Q-->, Gauss's
law ([../coulombs-law/](../coulombs-law/); worked for plates in
[../electromagnetism/electromagnetism.md §14](../electromagnetism/electromagnetism.md#14-displacement-current--why-a-capacitor-passes-ac))
gives a uniform field ![E = Q/( epsilon_0 A)](amperes-law.assets/eq-inline/460b1fcf3b.svg)<!--m:E = Q/(\varepsilon_0 A)-->. So the flux
through ![S_2](amperes-law.assets/eq-inline/ce8d6ef281.svg)<!--m:S_2--> is ![Q/epsilon_0](amperes-law.assets/eq-inline/2910d2f7e3.svg)<!--m:Q/\varepsilon_0-->, and the displacement current equals the rate at which charge
arrives on the plate. That is the wire current:

![E equals Q over epsilon_0 A, so Phi_E equals E A equals Q over epsilon_0, so epsilon_0 d Phi_E by dt equals dQ by dt equals I](amperes-law.assets/eq-disp-plate.svg)

Through ![S_1](amperes-law.assets/eq-inline/8d89fb9718.svg)<!--m:S_1--> the count is ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> of conduction current. Through ![S_2](amperes-law.assets/eq-inline/ce8d6ef281.svg)<!--m:S_2--> it is ![I](amperes-law.assets/eq-inline/ca73ab6556.svg)<!--m:I--> of displacement current.
The two surfaces now agree. A ![100 nF](amperes-law.assets/eq-inline/52343c37ff.svg)<!--m:100\ \mathrm{nF}--> capacitor whose voltage slews at ![1 V](amperes-law.assets/eq-inline/9593822b8f.svg)<!--m:1\ \mathrm{V}--> per microsecond
carries ![0.1 A](amperes-law.assets/eq-inline/d18c0e44c5.svg)<!--m:0.1\ \mathrm{A}--> through its leads and the same ![0.1 A](amperes-law.assets/eq-inline/d18c0e44c5.svg)<!--m:0.1\ \mathrm{A}--> of displacement current across its
gap (the capacitor law, [../capacitor/](../capacitor/)).

**The bigger consequence.** A changing magnetic field makes an electric field
([../faradays-law/](../faradays-law/)). Maxwell's term says a changing electric field makes a
magnetic field. Together they let a disturbance in the two fields sustain itself and travel
through empty space at a speed fixed by the two constants:

![c equals 1 over the square root of mu_0 epsilon_0, equals 1 over the square root of 1.2566 times 10 to the minus 6 times 8.854 times 10 to the minus 12, about 2.998 times 10 to the 8 metres per second](amperes-law.assets/eq-light.svg)

That is the speed of light. Maxwell's correction is how we know light is an electromagnetic wave.
In power electronics the term matters less for magnetics than for interference: a fast switching
edge, through the stray capacitance of a heatsink or a transformer, is a displacement current
that has to return somewhere. The full set of four field laws is in
[../electromagnetism/electromagnetism.md §14–§15](../electromagnetism/electromagnetism.md#14-displacement-current--why-a-capacitor-passes-ac).

## 15 What this costs you

- **Ampère's law only solves symmetric problems.** It is always true, but it gives you ![B](amperes-law.assets/eq-inline/ae4f281df5.svg)<!--m:B--> only for
  infinite wires, long solenoids, toroids and coax. A real coil is finite: the 5 cm coil of §9 is
  7 % below the ideal formula at its centre and about half of it at its ends. Short coils, square
  loops and PCB tracks need Biot–Savart or a field-solver program.
- **A single wire's field never occurs alone.** Every circuit has a return path, and the return
  current's field cancels most of the go current's. For a go-and-return pair of wires a distance ![d](amperes-law.assets/eq-inline/3c363836cf.svg)<!--m:d-->
  apart, at a distance ![r](amperes-law.assets/eq-inline/4dc7c9ec43.svg)<!--m:r--> along the line through both wires, the two ![1/r](amperes-law.assets/eq-inline/525108fcf9.svg)<!--m:1/r--> fields subtract:

  ![B equals mu_0 I over 2 pi times 1 over r minus d over 2 minus 1 over r plus d over 2, equals mu_0 I over 2 pi times d over r squared minus d squared over 4, approximately mu_0 I d over 2 pi r squared](amperes-law.assets/eq-pair-far.svg)

  The pair's field falls as ![1/r^2](amperes-law.assets/eq-inline/d790d0aaad.svg)<!--m:1/r^2-->, not ![1/r](amperes-law.assets/eq-inline/525108fcf9.svg)<!--m:1/r-->, and it is proportional to the spacing. With ![10 A](amperes-law.assets/eq-inline/c2205c3089.svg)<!--m:10\ \mathrm{A}-->,
  ![d = 2 mm](amperes-law.assets/eq-inline/6c7d5847ab.svg)<!--m:d = 2\ \mathrm{mm}--> and ![r = 10 cm](amperes-law.assets/eq-inline/00772e5333.svg)<!--m:r = 10\ \mathrm{cm}-->:

  ![B pair is about 2 times 10 to the minus 7 times 10 times 0.002 over 0.1 squared, equals 0.4 microtesla; B single equals 2 times 10 to the minus 7 times 10 over 0.1, equals 20 microtesla](amperes-law.assets/eq-pair-worked.svg)

  Fifty times less. Routing every current right next to its return, so the loop it encloses is
  small, is the cheapest magnetic shielding there is.
- **Steady currents only.** The law in the form ![mu_0 I_enc](amperes-law.assets/eq-inline/991262962d.svg)<!--m:\mu_0 I_{enc}--> is magnetostatics. Fast edges need
  Maxwell's term (§14), and at high enough frequency the circuit radiates.
- **B is proportional to I only in linear materials.** Magnetic cores saturate (§12), and above
  saturation more current buys almost no more field.
- **"Spread evenly" is a DC idea.** The skin effect (§8) changes the field inside conductors at
  switching frequencies and raises their resistance.
- **Forces grow as the square of the current.** Harmless at signal level, destructive at fault
  level (§13).
- **Direction errors are easy.** Use conventional current and the right hand, every time (§3).

## 16 Sources and cross-links

**In this tree**

- **Charge, the electric field, ε₀ and Gauss's law:** [../coulombs-law/](../coulombs-law/).
- **The partner law, in which a changing field makes a voltage:**
  [../faradays-law/](../faradays-law/).
- **The whole field picture: flux, Faraday, Lenz, inductance, B–H curves and Maxwell's four
  equations:** [../electromagnetism/electromagnetism.md](../electromagnetism/electromagnetism.md),
  especially §4 (the short version of this document), §5 (H and reluctance), §12 (saturation) and
  §14–§15.
- **The inductor**, whose field is the solenoid's of §9:
  [../inductor/inductor.md](../inductor/inductor.md). Its right-hand-rule sentence is illustrated by
  §3 here.
- **The capacitor**, whose current crosses the gap as displacement current (§14):
  [../capacitor/capacitor.md](../capacitor/capacitor.md).
- **The transformer**, two windings sharing a core's flux: [../transformer/](../transformer/).
- Style rules: [../../STYLE.md](../../STYLE.md).

**References**

- D. J. Griffiths, *Introduction to Electrodynamics*, 4th ed., Cambridge University Press, 2017:
  §5.1 (Lorentz force), §5.2 (Biot–Savart, the straight wire and the ring), §5.3 (Ampère's law,
  its proof from Biot–Savart, solenoid and toroid), §6.3 (H and free current), §7.3 (displacement
  current).
- E. M. Purcell and D. J. Morin, *Electricity and Magnetism*, 3rd ed., Cambridge University Press,
  2013, ch. 6 (the magnetic field) and ch. 11 (matter in magnetic fields).
- R. P. Feynman, R. B. Leighton and M. Sands, *The Feynman Lectures on Physics*, vol. II,
  ch. 13–14 (magnetostatics) and ch. 18 (the Maxwell equations).
- H. C. Ørsted, *Experimenta circa effectum conflictus electrici in acum magneticam*, Copenhagen,
  21 July 1820.
- J.-B. Biot and F. Savart, "Note sur le magnétisme de la pile de Volta", *Annales de chimie et de
  physique* 15, 1820.
- A.-M. Ampère, *Mémoire sur la théorie mathématique des phénomènes électrodynamiques uniquement
  déduite de l'expérience*, Paris, 1826.
- J. C. Maxwell, "A Dynamical Theory of the Electromagnetic Field", *Philosophical Transactions of
  the Royal Society* 155, 1865.
- BIPM, *The International System of Units (SI)*, 9th ed., 2019: the 1948 ampere definition
  (historical appendix) and the 2019 redefinition fixing *e*.
- CODATA recommended values of the fundamental physical constants (NIST): the vacuum magnetic
  permeability.
- Science Facts, "Magnetic Field from Ampere's Law" (sciencefacts.net), a one-page chart of the
  straight wire, the inside of a conductor, the solenoid and the toroid. All four are derived here,
  in §5–§10.
