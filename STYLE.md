# Documentation & figure standards

The authoritative standard for every document in this tree. It exists because these notes must
render **identically** on GitLab, GitHub, the local Chrome *Markdown Viewer*, VS Code, and fully
offline — with no plugin, no toggle, no surprises. The rules below are what guarantee that.

> **The one rule that drives everything**
>
> **All mathematics and all diagrams are pre-rendered SVG images.** No markdown math delimiters
> (`$…$`, `$$…$$`, ``$`…`$``), no ASCII-art equations, no `` `code` ``-as-maths, and no emoji ever
> appear in a committed `.md`. Every symbol you see is a generated SVG. This is not a preference —
> it is the only form that renders the same everywhere (see §2 for the evidence).

## Contents

1. [The prose](#1-the-prose)
2. [Why everything is an SVG — the portability evidence](#2-why-everything-is-an-svg--the-portability-evidence)
3. [Writing mathematics](#3-writing-mathematics)
4. [Callouts (no emoji)](#4-callouts-no-emoji)
5. [Figures — the arXiv standard](#5-figures--the-arxiv-standard)
6. [The figure colour system](#6-the-figure-colour-system)
7. [The toolchain and the build](#7-the-toolchain-and-the-build)
8. [Conformance checklist](#8-conformance-checklist)

---

## 1. The prose

- **Thesis in one line** — a blockquote near the top stating the single idea the document argues.
- **Numbered Contents** linking to sections (`## N Title` → anchor `#n-title`).
- **Narrative, not spec-sheet.** Sections *explain* — what a thing is, why it exists, what breaks
  without it — in full sentences. Bullets carry enumerable facts only.
- **Numbers are load-bearing.** Constants and results are stated verbatim with units (rendered as
  maths where they are maths).
- **Honest costs.** Every strong claim states its price (a "What this costs you" section).
- **Address the confusion.** Where a step is genuinely easy to get wrong (e.g. differentiating
  `Q = C·V`), stop and prove it slowly rather than asserting it.
- **Sources / cross-links** at the end, as relative links to sibling documents.

## 2. Why everything is an SVG — the portability evidence

Inline maths was tested on the two renderers this project actually targets:

| Form | GitLab | Chrome *Markdown Viewer* (mathjax on) |
|---|---|---|
| plain `$V_C$` | unreliable (docs favour the backtick form) | renders |
| backtick ``$`V_C`$`` | renders | **does not render** (its regex forbids backticks inside `$…$`) |
| **SVG image** `![...](eq-….svg)` | **renders** | **renders** |

The two renderers **conflict** on every markdown math delimiter. The only form that renders on
both — plus GitHub and offline — is a plain image. GitHub's `> [!NOTE]` admonitions likewise fail
on GitLab and markdown-it, and emoji render as tofu boxes in the user's font. Hence the one rule:
**pre-render maths and diagrams to SVG; use no emoji; use bold-label callouts.**

## 3. Writing mathematics

You author maths in readable source; the toolchain converts it to SVG. Three cases:

**a. Display equations** — write them in a `<doc>.eqns` source file beside the `.md`:

```
name: law
tex: V_L(t) = L\frac{dI_L}{dt}
---
```

Running the toolchain emits `eq-<name>.svg` into `<doc>.assets/`. Embed it with a words-only alt:

```
![V_L equals L times dI_L by dt](inductor.assets/eq-law.svg)
```

Multi-step derivations are one equation each, or a single `\Longrightarrow`-chained line — the
algebra is typeset, never ASCII art.

**b. Inline maths in prose** — write it with ordinary `$…$` delimiters directly in the `.md`:

```
Suppose $V_L$ is held constant, so $dI_L/dt = V_L/L$ is fixed.
```

The `inline_math.js` pass converts each `$…$` span to an SVG embed, rewriting the line to a
round-trippable marker form:

```
Suppose ![V_L](inductor.assets/eq-inline/HASH.svg)<!--m:V_L--> is held constant, …
```

The image comes **first** and the HTML comment trails it. A line must never *start* with `<!--`:
CommonMark treats such a line as a raw HTML block, so an image later on the same line would show as
literal `![…](…)` text (this broke the local viewer before the order was swapped). The comment preserves the LaTeX source so the pass is **idempotent** and you can keep editing
the maths. Rules for authoring inline maths:
- Multi-letter subscripts get braces: write `V_{out}`, `f_{sw}`, `I_{load}` (not `V_out`).
- English words inside maths use `\text{…}` (e.g. `\text{rise} = \text{fall}`).
- Units use `\mathrm{…}` (e.g. `100\,\mathrm{kHz}`, `112.5\,\mu\mathrm{H}`).
- **Compound units take negative exponents and a centre dot, never a slash.** Write
  `1\,\mathrm{A} = 1\,\mathrm{C\cdot s^{-1}}`, not `C/s`; likewise `\mathrm{V\cdot s\cdot A^{-1}}`,
  `\mathrm{A\cdot s^{-1}}`, `\mathrm{J\cdot C^{-1}}`, `\mathrm{N\cdot C^{-1}}`, `\mathrm{Wb\cdot A^{-1}}`,
  `\mathrm{F\cdot m^{-1}}`, `\mathrm{H\cdot m^{-1}}`, `\mathrm{V\cdot m^{-1}}`, `\mathrm{W\cdot m^{-2}}`,
  `\mathrm{rad\cdot s^{-1}}`. With a prefix that is a maths symbol, keep the exponent on the whole
  unit: `\mathrm{V}\cdot\mu\mathrm{s}^{-1}`. In prose, a compound unit is always maths
  (`$\mathrm{C\cdot s^{-1}}$`) — never a raw Unicode superscript or a slash in plain text. The rule
  is for **units only**: a ratio of quantities such as `dI/dt` or `V/L` keeps its slash. Figure
  labels follow the same rule (`H\;(\mathrm{A\cdot m^{-1}})`).
- Never put maths in a heading — headings stay plain text.

**c. Figures** — see §5. Maths inside a figure is typeset by the same engine via `mathText`.

**Never** commit `$…$`, `$$…$$`, ``$`…`$``, ASCII `=`-aligned blocks, or `` `code` ``-wrapped
equations. Genuine non-maths inline code (a literal `ON`/`OFF`, a filename) may stay as code.

## 4. Callouts (no emoji)

A callout is a blockquote whose first line is a **bold text label** — no emoji, ever:

```
> **Note —** context that reframes.
> **Tip —** a takeaway to carry forward.
> **Watch out —** a footgun or a cost.
```

These render identically on every target. Do not use `> [!NOTE]` admonitions (GitLab/markdown-it
do not render them) and do not use emoji labels (they show as tofu boxes).

## 5. Figures — the arXiv standard

Figures are **generated** by `toolchain/figures/*.js` using the `Fig` builder in
`toolchain/drawing_to_svg.js`; never hand-authored. They must read like a research paper:

- **Every label is typeset maths** — titles, axis titles, tick labels that are quantities, and all
  in-plot annotations go through `mathText`/`plot().ticks(...tex...)`. **No monospace plaintext
  maths, no non-maths-rendered symbols, ever.**
- **Framed, ticked axes.** A plot has a rectangular frame, real tick marks with typeset numeric
  labels, a subtle grid, and **axis titles as maths with units** — `$t$`, `$V_L\;(\mathrm{V})$`,
  `$I_L\;(\mathrm{A})$`. Use `plot({x,y,w,h,xlim,ylim})` and draw in data coordinates.
- **Caption band.** Every figure ends with `f.caption("Y vs. X — descriptive name")`, which renders
  a rule line and a bold `Figure N:` prefix. Figure numbers are global and stable across the tree;
  each topic owns a block, and new topics take the next free block:
  - fundamentals: inductor 1–3, 90 (the Faraday derivation, file `fig-04.svg`) and 135–136
    (animated: the right-hand rule `fig-05-anim.svg`, the step-by-step derivation `fig-06-anim.svg`),
    capacitor 4–6, electromagnetism 13–19, signals 20–29 (edges and Fourier 20–26, AC and RMS
    27–29), transformer 30–39, Coulomb's law 91–99, Ampère's law 100–113, Faraday's law 115–134;
  - dc-dc-converters: buck 7–9, boost 10–11, buck/boost comparison 12, start-up 50–55;
  - filters: LC filter 56–65;
  - pwm: 66–70;
  - rectifiers: 80–89;
  - dc-ac-inverters: H-bridge 40–49, SPWM 71–79;
  - later additions to an existing topic whose block is full take the next free number after 89
    (90 onward): inductor 90, then 135–136.
  - **unused numbers** (allocated, never drawn; free for the topic that owns them): 114 (Ampère),
    137–139 (inductor additions), 140–144 (electromagnetism additions). The next new topic starts
    at 145.

  The *file* names are local to each document — `fig-01.svg`, `fig-02.svg`, … inside its
  `<doc>.assets/` — while the caption carries the global number.
- **Serif type**, generous margins, dark theme kept.
- **Embedding in the `.md`**: `![alt](<file>.assets/fig-NN.svg)` where the alt **equals** the SVG's
  `aria-label`, followed immediately by one *italic* interpretive caption line in the prose (says
  *why it matters*; the figure itself carries *what it is*).
- **Animation rule:** never animate a graph — motion on a plotted quantity is misleading. Animation
  is allowed *only* on component diagrams (charge moving through a coil, charge on a plate), where
  it depicts a physical flow, not a measured value.
- **How to animate: prefer CSS `@keyframes`.** Both SMIL (`<animate>`, `<animateMotion>`,
  `<animateTransform>`) and CSS animations run inside an SVG embedded as an image, but only CSS
  animations can be stopped by `@media (prefers-reduced-motion: reduce)` — CSS cannot pause SMIL.
  CSS animation also leaves each element's own attributes as the static frame that `rsvg-convert`,
  print and reduced-motion viewers show, so draw a meaningful first frame in plain markup and let a
  class animate it. Use the clock and helpers in `toolchain/parts/animation.js` (`animator`,
  `dotStream`, `circulation`, `bead`), which emit the reduced-motion rule for you, and check single
  frames with `FIG_FREEZE_AT=<seconds>` (see [../toolchain/README.md](../toolchain/README.md)). Name
  an animated file `fig-NN-anim.svg`, make the loop seamless and the timing deterministic.
  - CSS keyframes (reduced-motion safe): Figures 100, 101, 104, 106 (Ampère), 135, 136 (inductor).
  - SMIL with a reduced-motion fallback (the animated group is hidden and a static copy shown):
    Figures 118, 121, 123, 125, 130, 132 (Faraday).
  - SMIL that keeps moving under reduced motion (older or pre-dates this rule; convert to CSS when
    next touched): Figures 91, 92, 96 (Coulomb), 2 (inductor, current dots), 5 (capacitor).

## 6. The figure colour system

Colours are **classes**, never inlined — this is what lets each figure re-theme itself for dark
(default), light (`prefers-color-scheme: light`), and print. Same hue letter = same meaning:

| Hue | Classes | Means in this tree |
|---|---|---|
| blue | `plot-b` `fx-b` `nb` `flow-b` `dotb` | the mechanism under focus; the output node; the effect quantity of a ramp |
| green | `plot-g` `fx-g` `ng` `flow-g` `fill-g` | the favoured/correct side; the boost result; charging |
| red | `plot-r` `fx-r` `nr` `flow-r` | cost/danger; the ON (low) interval of a boost; discharging |
| amber | `plot-a` `fx-a` `na` `flow-a` `fill-a` `dota` | machinery/accounting; the switch node; the constant drive on a ramp |
| purple | `plot-p` `fx-p` `np` | data/payload; the capacitor's own current (`i_C`) |
| neutral | `wire` `axis` `grid` `frame` `tickline` `n` | wires, axes, gridlines, frames, enumerated items |

Typeset maths inside a figure inherits colour from `mathText(..., {hue})` (→ `fx-*`) or the default
`mathfill` class. Both set `color` and `fill` so MathJax's `currentColor` paths resolve per theme.

## 7. The toolchain and the build

All generation is one command — see [../toolchain/README.md](../toolchain/README.md):

```
node toolchain/build_all.js
```

It runs three passes in order: display equations (`.eqns` → `eq-*.svg`), inline maths (`$…$` in
every `.md` → `eq-inline/*.svg` embeds), then figures (`figures/*.js` → `fig-NN.svg`). It is
deterministic and idempotent — a second run changes nothing. Generation needs `mathjax-full`;
viewers need nothing (the SVGs are self-contained).

## 8. Conformance checklist

1. No `$…$`/`$$…$$`/``$`…`$``, no ASCII-maths, no `` `code` ``-as-maths, **no emoji** in any `.md`.
2. Every equation is a generated SVG (`eq-*.svg` display, `eq-inline/*.svg` inline); regenerated
   from source by the toolchain; `build_all.js` is idempotent.
3. Callouts are bold-label blockquotes (`> **Note —**`, `> **Tip —**`, `> **Watch out —**`).
4. Figures: all labels typeset maths; framed ticked axes; maths axis titles with units; a
   `Figure N: Y vs. X — name` caption; dark theme; no inlined colours; graphs never animate.
5. `xmllint --noout` passes on every SVG; each contains `prefers-color-scheme` (theming intact).
6. Figure embed: alt == `aria-label`, one italic caption line follows.
7. Prose: thesis blockquote, numbered Contents, honest-cost section, Sources/cross-links.
8. Renders identically on GitLab, GitHub, the local viewer, and offline — verified by rendering
   through markdown-it with zero literal `$`, zero `[!`, zero emoji outside code blocks, and every
   `<img>` resolving.
