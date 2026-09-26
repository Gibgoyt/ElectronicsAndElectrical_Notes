# Documentation & figure style guide

How every document in this tree is written. It mirrors the Proqmed `docs/*/STYLE.md`
guide — the engine-agnostic house style — adapted for an electronics/maths subject where
equations and graphs carry as much of the argument as the prose.

## 1. The prose

- **Thesis in one line** — a blockquote near the top stating the single idea the document
  argues, e.g. "voltage across an inductor is proportional to how fast its current changes;
  everything else is integration."
- **Numbered Contents** list linking to sections (`## N Title` → `#n-title`).
- **Narrative, not spec-sheet.** Sections *explain* — what a thing is, why it exists, what
  breaks without it — in complete sentences. Bullets carry enumerable facts only.
- **Numbers are load-bearing.** Constants and results are stated verbatim with units:
  `112.5 µH`, `f_sw = 100 kHz`, not "large".
- **Callouts** — a blockquote whose first line is a bold emoji label, so it renders on GitHub,
  GitLab, and the local markdown-it viewer alike (GitHub's `> [!NOTE]` admonition syntax does
  **not** render on GitLab or markdown-it, so it is not used here). Three kinds:
  `> **📝 Note —** context that reframes`, `> **⚠️ Watch out —** footguns and costs`, and
  `> **💡 Tip —** takeaways and rules to carry forward`.
- **Honest costs.** Every strong claim gets its price stated ("what this costs you").
- **Address the confusion.** Where a step is genuinely easy to get wrong (the classic one
  here: differentiating `Q = C·V`), stop and prove it slowly rather than asserting it.
- **Sources / cross-links** at the end: link to the sibling documents each mechanism connects
  to (`[[../capacitor/capacitor.md]]`-style relative links).

## 2. Maths — three tools, pick by job

This tree deliberately uses **three** ways of showing maths. Use the one that fits:

1. **Display and inline equations — KaTeX/LaTeX syntax.** Wrap display maths in `$$…$$` and
   inline maths in `$…$`. This renders on GitHub, GitLab, and the local Chrome *Markdown Viewer*
   (with its **mathjax** content option enabled) alike, as well as VS Code and Obsidian. Use it
   for the *statement* of a law or a final result:

   ```
   $$V_L(t) = L\,\frac{dI_L}{dt}$$
   ```

   > **📝 Note —** If a viewer does not render `$$…$$`, the raw LaTeX is still readable. Every
   > equation is also restated in prose or in an ASCII derivation nearby, so no meaning is lost.

2. **Step-by-step derivations — aligned ASCII code fences.** When the point is to show *every
   algebraic move* (the parts that are easy to get lost in), put the derivation in a plain
   fenced block with the `=` signs lined up. These read identically in every viewer and match
   how you write them by hand:

   ```text
   V_L        = L · dI/dt                  the defining law
   dI/dt      = V_L / L                    rearranged for the rate
   ∫ dI       = ∫ (V_L/L) dt               integrate both sides over 0..t
   I(t)−I(0)  = (V_L/L) · t                V_L, L constant → pull out of the integral
   ```

3. **Graphs, waveforms, schematics — SVG figures** (see §3–§4).

> **⚠️ Watch out —** **Never animate a graph.** Axes-and-curves figures (`V` vs `t`, `I` vs `t`) are always
> static — motion on a plotted quantity is misleading. Animation is allowed *only* on
> component diagrams (charge moving through a coil, charge piling on a plate), because there
> the motion depicts a physical flow, not a measured value.

## 3. The figures — file convention

- `<FILE>.md` pairs with a sibling directory `<FILE>.assets/` holding `fig-01.svg`,
  `fig-02.svg`, … in section order. Topic-level figures live in
  `<topic>/<topic>.assets/`.
- Embed as `![<short alt>](<FILE>.assets/fig-NN.svg)` followed immediately by one *italic*
  caption line — the caption says *why it matters*; the figure itself carries *what it is*.
  The alt text **equals** the SVG's `aria-label`.
- Density: roughly a figure per major section. A reader who only looks at the figures should
  still get the whole story.

## 4. The figures — SVG skeleton (non-negotiable)

Every figure copies the same skeleton (the `<style><![CDATA[ … ]]></style>` block plus the
six arrowhead `<marker>`s plus the `h2m-bg` rect), taken from the Proqmed
`uSockets_philosophy.assets/fig-01.svg`. Only three things change per figure: the `viewBox`
height `H`, the `aria-label`, and the body. This is what makes the figures re-theme themselves
for dark, light, and print automatically — **never inline a colour in the body; every colour
resolves through a class.**

Class families (same letter, same hue, same meaning):

| Hue | Node / arrow / label classes | Means, in this tree |
|---|---|---|
| blue | `.nb .ab .lb .plot-b .fx-b` | the mechanism under focus; the **output node**; the effect quantity of a ramp (I for an inductor, V for a capacitor) |
| green | `.ng .ag .lg .plot-g .fx-g` | the correct/favoured side; the boost result; "cap charging" |
| red | `.nr .ar .lr .plot-r .fx-r` | cost/danger; the ON (low) interval of a boost; "cap discharging feeds load" |
| amber | `.na .aa .la .plot-a .fx-a` | machinery & accounting; the **switch node**; the constant *drive* on top of a ramp graph; the bottom-line takeaway |
| purple | `.np .ap .lp .plot-p .fx-p` | data/payload; the capacitor's own current `i_C` |
| neutral | `.n .a .l .wire .axis .grid` | wires, axes, gridlines, enumerated items |

Helper classes added for this subject (still theme-driven): `.wire` (schematic wire),
`.axis` / `.grid` (graph axes and gridlines), `.plot-*` (a plotted curve in the family
colour), `.fill-g` / `.fill-a` (translucent area fills for "charge = area" arguments).

### Emoji net labels

Borrowed from the onvif breadboard docs: when prose refers to a coloured point in a figure,
tag it with the matching emoji so the eye can jump between text and picture. The mapping in
this tree:

- 🟠 **switch node** (amber) — the chopped point after the switch
- 🔵 **output node** (blue) — `V_out`, at the capacitor/load
- 🟡 **PWM drive** (amber) — the gate signal
- ⚫ **ground rail** (neutral)

## 5. Conformance checklist

1. Skeleton copied verbatim; only `H`, `aria-label`, body changed; `h2m-bg` height matches `H`.
2. `xmllint --noout` passes on every SVG; `prefers-color-scheme` present (theming intact).
3. No inline colours in figure bodies; every colour is a class.
4. Graphs are static; only component diagrams may animate.
5. One centred takeaway line near the bottom of each figure (`.la`, at about `y = H − 20`).
6. Markdown embed: alt == aria-label, italic caption follows, callout after if there is a
   consequence.
7. Prose: thesis blockquote, numbered Contents, honest-cost callout, Sources/cross-links.
8. Every quantity verbatim with units; results shown in a display equation *and* an ASCII
   derivation.
9. **Portable everywhere.** Renders correctly on GitHub, GitLab, and the local markdown-it
   viewer: use emoji-label blockquotes (not `> [!NOTE]`), keep inline `$…$` on one line with no
   space just inside the delimiters and no backticks inside, and reference figures by relative
   path so they resolve in every host.
