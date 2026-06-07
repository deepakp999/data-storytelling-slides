---
name: data-storytelling-slides
description: "Create consulting-grade, data-storytelling PowerPoint decks in an editorial 'insight-driven' visual style (bold serif action-title headlines, a single-hue color system, minimal chrome, charts that carry the argument). Use this skill WHENEVER the user wants a presentation, slide deck, pitch, board/executive update, or any .pptx that should look like a polished executive data story — especially when the request is just a TOPIC (e.g. 'make a deck on X', 'present our Q3 numbers', 'build slides explaining the EV market'). The skill researches the topic, designs a storyline, writes a DETAILED EDITABLE PROMPT SPEC FOR EACH SLIDE (confirmed with the user before building), picks the right chart/slide type per message, then BUILDS ONE SLIDE AT A TIME — showing the prompt and re-confirming each slide with the user before moving on (human in the loop), never batching the whole deck. Where data exists it visualizes it; where there is none it structures the text into frameworks rather than bullets. It is palette-agnostic (derives a theme from any brand color) with a built-in fallback theme. This is the DEFAULT style for data-driven / analytical / narrative decks. Use it even if the user doesn't say 'data storytelling' — if they want a polished analytical deck with charts and a clear argument, use this."
---

# Data Storytelling Slides

Build decks where every slide is a small argument: an **action-title headline** stating the
takeaway, **evidence** (a chart/visual built to prove exactly that takeaway), and **synthesis**
(what it means). The look is editorial and minimal — bold serif headlines, one accent hue, white
canvas, no decorative chrome.

**The one idea that makes or breaks these decks: the VISUAL carries the information; text is a thin
top layer.** Any fact you can draw — a magnitude, trend, share, flow, gap, or date — belongs in the
visual as a mark, label, or annotation, never as a sentence. Dense research is compressed *into the
visuals*, not poured onto the slide as prose. A finished slide is information-rich and visually calm
at once. If a slide's body is mostly text boxes, it has failed regardless of how clean it looks. The
section "Slide anatomy & density" below is the core discipline — read it first.

**This skill is the brain (what to build and why). Companion capabilities are the hands:**
- **A PowerPoint rendering capability** (e.g. a `pptx` skill / `pptxgenjs`) — HOW to render: the
  generation API, fonts, charts, converting to images, the QA loop. Read it before building; it is
  authoritative for rendering mechanics. This skill defers all rendering details to it.
- **An optional separate corporate style** (e.g. a card-based, single-accent "playbook" look) — if one
  is available and the user asks for that instead, use it. When a blueprint here needs a style-neutral
  component spec (card geometry, table spec, image container, spacing), borrow it from there; for
  anything visual, THIS skill's identity wins.

**Deep reference (read as needed):**
- `references/design-system.md` — the full system: narrative architecture, color logic (10
  principles), pattern library, layout principles, chart craft. This is the source of truth; consult
  it whenever you need detail beyond this SKILL.md.
- `references/themes.md` — the fallback "Editorial Mint" theme + the palette-agnostic derivation
  recipe. Read before choosing colors.
- `references/advanced-visuals.md` — HOW to render visuals beyond native charts (maps, Sankey,
  treemap, chord…): the SVG-→-rasterize-→-native-overlay technique, the visual→library map, and the
  concrete choropleth recipe. Read when a spec's hero visual isn't a native pptxgenjs chart.

---

## Slide anatomy & density — the core discipline

What separates an editorial data story from a generic "bullets in boxes" deck. Internalize this before
the workflow.

### Every content slide has ONE hero visual
A single dominant chart / diagram / map / framework occupies the **majority of the slide body
(~55–70%)**. If a slide has no hero visual, it isn't finished — it's a list. If you can't picture a
visual for a beat, the beat is mis-scoped (too vague, or two beats fused). Only the bookends (cover,
divider, pure executive-summary) are exempt — and even those should lean visual.

### The anatomy of a content slide
1. **Action title** — serif, the quantified takeaway, 1–2 lines.
2. **Hero visual** — dominant; built title-first so the element proving the title is emphasized and
   everything else recedes.
3. **Insight rail** — a thin column/strip of **3–4 micro-points**, each a **bold lead phrase + a
   ≤~8-word clause** (e.g. "**22.5% CAGR** — fastest of any packaging segment"). This is the synthesis
   layer. NOT paragraphs, NOT a bullet dump, NOT a grid of cards.
4. **On-visual annotations** — endpoint callouts, a bracket+number on the gap, a highlighted point, a
   "$57B by 2033" tag pinned to the curve. Most detailed numbers live here, on the picture.
5. **At most one emphasis element** (a takeaway band or a hero number) — only if it earns it.

### Text budget (hard guide)
Outside the action title and the visual's own labels/annotations, aim for **≤ ~40 words of body text
per slide.** Over budget = you're narrating the visual instead of letting it speak. Move those facts
onto the chart as labels/annotations, or cut them. One idea per slide.

### Compress the research INTO the visuals
A 10-slide deck must carry all the research. The move is to encode density into the visuals, never to
thicken the text:
- Turn a table of numbers into ONE chart; let bars/points/positions carry the values.
- Put supporting figures on the chart as **data labels, axis ticks, endpoint callouts, annotations**.
- Use **small multiples** or a **stat strip** (a row of 3–5 big-number callouts) to pack many figures
  into one calm visual frame.
- Encode categories by **position / color / size**, not a written list.
- Push secondary detail into **insight-rail micro-points** (bold lead + short clause = high
  information-per-word).
- Truly secondary material → an appendix slide, not the body.

### Visual-first ≠ sparse
Cutting text is not an empty slide with one number. Pair every hero visual with the insight rail AND
on-visual annotations so the slide still delivers the full argument. A bare chart + title is as wrong
as a wall of bullets. Aim for the middle: **dominant visual, dense annotation, tight insight rail.**

---

## The Workflow (follow in order)

When the user gives a topic (or topic + data), run these phases. Don't skip the storyline phase —
picking slide layouts before the storyline exists is the #1 way decks turn into a pile of charts. The
pivot point is **Phase 3: a detailed, editable prompt spec for every slide, confirmed before any
rendering** — and then the deck is **built one slide at a time, showing the prompt and re-confirming
each slide before moving on (human in the loop)**, never as a single batch. Research → storyline →
per-slide specs for ALL slides (approve the whole story first) → build slide-by-slide with approval at each → QA → later edits.

### Phase 1 — Frame: topic, audience, goal, data
1. Identify the **topic**, the **audience**, and the **decision/goal** the deck serves. If audience
   or goal is unclear and it materially changes the story, ask ONE concise question; otherwise make
   a sensible assumption and state it.
2. **Get the data.**
   - If the user supplied data/files, use that as the spine.
   - Otherwise **search the web** for current, real figures relevant to the topic (market sizes,
     growth rates, shares, timelines, named players). Use multiple targeted searches. Prefer recent,
     primary sources. This skill is for REAL data stories — do not invent numbers; if a figure can't
     be found, say so and either search differently or mark it clearly as illustrative.
3. Note the units and rough magnitudes you'll be charting.

### Phase 2 — Storyline: arc + action titles (the outline)
Read `design-system.md` → "Narrative Architecture" for the full method. In short:
1. Climb to **synthesis**: facts → summary → *synthesis* (the so-what). The deck must deliver the
   so-what, not just restate data.
2. Pick a **narrative arc** fitting the goal (SCQA, Pyramid/answer-first, Chronological,
   Problem-Solution, Compare-Contrast).
3. Break it into **beats** — one beat = one slide = one single message.
4. Write each beat's headline as an **ACTION TITLE**: a complete sentence stating the conclusion,
   specific and quantified, concise. Climb the ladder: *full sentence → specific (name real
   entities) → quantified (number in the title) → concise.*
5. **For each beat, name its HERO VISUAL and the 2–3 figures that will live on it** (as labels /
   annotations). Deciding the visual at outline time is what forces visual-first instead of
   text-first. If a beat has no natural visual, re-scope it.
6. **Compression pass.** If the research holds more facts than ~10 slides can carry, do NOT thicken
   slides with text — compress: combine numbers into one chart, add a stat strip, move figures into
   annotations, and push genuinely secondary detail to an appendix. Every research fact should map to
   a visual mark, a label, an annotation, or an insight-rail micro-point — almost never a sentence.
7. **QA the outline before building:**
   - *So-what test:* every slide has a real implication. If a slide has no so-what, cut or restructure it.
   - *Horizontal logic:* reading only the action titles top-to-bottom narrates the whole argument.
   - *Vertical logic:* each title is provable by what will be on its page; it claims no more.
   - *Visual test:* every content beat has a named hero visual (not "some bullets").
8. **Hand the confirmed beats to Phase 3 (per-slide prompt specs)** — that is where the user
   confirms, not here. Keep this phase to the titles + hero-visual + evidence note.

### Phase 3 — Per-slide prompt specs (the editable blueprint + the confirm gate)
Turn each beat into a **detailed, structured, human-editable prompt** — one per slide. This is the
single most important checkpoint: it is far cheaper to steer the deck here than after rendering, and
each spec doubles as the durable instruction for regenerating that one slide later (Phase 6b).

Write the specs **after research** so every figure named in a spec is a real, sourced number — never
a placeholder to be invented at build time. Use this template per slide:

```
Slide N — <Section / role>
  Action title:  <the quantified takeaway sentence>
  Hero visual:   <type + what it encodes>  (e.g. "area chart, market $B 2024→2033")
  Data on it:    <the actual figures/series + units + source>  (real numbers from research)
  Callouts/annotations: <points/brackets/tags pinned to the visual, with their values>
  Insight rail:  <3–4 micro-points: bold lead + ≤~8-word clause>
  Emphasis:      <the one element to highlight> | Semantic colors: <only if meaning-bearing>
  Notes:         <layout / comparison / anything specific>
```

Keep each spec tight (it's a brief, not prose). Then:
1. **Present the COMPLETE set of specs for EVERY slide at once — the whole story, end to end.** The user
   sees the entire plan (all action titles read top-to-bottom = the narrative, plus each slide's hero
   visual, figures, and rail) and can edit anything — reword a title, swap a visual, change figures,
   add/cut/reorder slides — and **approves the full plan before ANY slide is built.** "Whole story
   first" is non-negotiable: starting to build before the complete plan is approved is how decks drift
   off-plan.
2. The **approved specs are the source of truth.** The deck is then built **ONE SLIDE AT A TIME** in
   Phase 5, re-showing each slide's spec just before that slide is built — never as a single batch.
   Batching all slides at once is exactly what produces a pile of uneven, off-plan slides.

Choosing the hero visual per spec: use the beat→type table below. If a spec needs data you don't
have, loop back to Phase 1. Vary visual types across consecutive slides for rhythm.

| The message is about… | Use… |
|---|---|
| A trend over time | Annotated line chart (highlight the focal series) |
| Comparing magnitudes across categories | Bar/column chart |
| Geographic distribution | Choropleth map + callouts |
| A schedule / project plan | Gantt with phase bands + shape-coded markers |
| One dramatic number | Big-stat callout (hero number in brand hue) |
| Many KPIs at a glance | Dashboard grid (mix of stats, gauge, pictograph) |
| A ratio / proportion | Icon-array / pictograph |
| A rating bad→good | Gauge (semantic colors) |
| Parts of a whole | Donut with center total |
| A part's share AND its internals | Donut + exploded-slice drill-down |
| Cause/effect, bridge between two totals | Waterfall (anchors vs steps) |
| Two distributions / disproportion | Stacked-bar flow (Sankey-style ribbons) |
| Positioning across two axes | 2×2 bubble map (bubble size = a third measure) |
| A staged conceptual framework | Funnel diagram |
| Options × criteria | Comparison matrix / table + callouts |
| Weighing pros vs cons | Diverging bar (tornado) |
| The recommendation | Answer-first summary / staged roadmap |
| Deck open / close | Cover · section divider · executive summary |

**Data → visualize; no data → STRUCTURE (never loose bullets or plain boxes).** Decide per beat:
- **Has numbers/data** → the hero is a chart/visual that *carries* those numbers (table above). Pull the
  figures onto the visual as labels/annotations.
- **No data** (qualitative, conceptual, process, options) → do NOT fall back to bullets or a row of
  plain text boxes with a paragraph in each (this is what makes a slide feel "below average"). Group the
  content into a deliberate STRUCTURE whose layout shows the logic:
  - **Grouped buckets (pyramid / MECE):** 2–4 titled groups, each with 2–3 tight sub-points — horizontal
    logic the reader scans in one pass.
  - **Process flow (chevrons / ghost boxes):** sequential or staged steps as connected chevrons/boxes,
    with a labeled transition between them — not just boxes-and-arrows with text inside.
  - **2×2 matrix / driver tree:** when the point is positioning, trade-offs, or cause → effect.
  - **Framework blocks:** situation → complication → resolution, or current → shift → target.
  The STRUCTURE does the explaining; text still obeys the budget. Even the cover and "background" slides
  get a deliberate visual structure, not a title plus a paragraph.

### Phase 4 — Theme
Read `references/themes.md`. If the user gave a brand color/palette, derive the 6 roles from it; else
use the **Editorial Mint** fallback. State the chosen theme in one line. Keep it consistent deck-wide.

### Phase 5 — Build ONE SLIDE AT A TIME (human in the loop)
**Do NOT batch-build the whole deck.** Building all slides at once is what produces a pile of uneven,
off-plan slides. Build incrementally, keeping the user in the loop at every slide. Structure the
pptxgenjs script so each slide is its own function/block, then loop, in storyline order:

For each slide:
1. **Show the user that slide's prompt spec** (from Phase 3) — action title, hero visual, the real
   figures, callouts, insight rail. Just this one slide's spec, not the deck.
2. **Wait for edits/approval**, then apply any changes to the spec.
3. **Build + render only that slide** and show the rendered image.
4. **Iterate on that one slide** until the user is happy with it.
5. **Then move to the next slide** — only proceed once the current one is approved.

Only build several at once if the user explicitly says "just build it all" — and even then, list each
slide's spec first and render every slide for review.

Read the `pptx` skill before the first build. **If a spec's hero visual is not a native pptxgenjs chart**
(map, Sankey, treemap, chord, network, custom radial/curved geometry), read
`references/advanced-visuals.md` and use the right library + the SVG→rasterize→native-overlay technique;
default to native charts (Tier A) or shape-built visuals (Tier B) whenever they suffice. Build each
chart **title-first**: decide what the action title claims, then construct the visual so the element
proving it is emphasized and everything else recedes (see "Emphasis follows narrative" below). Apply the
design rules below — including **data → visualize / no-data → structure** (Phase 3) — to every slide.

### Phase 6 — QA (visual + narrative)
Run the `pptx` skill's visual QA loop (render to images, inspect with fresh eyes, fix real defects).
ADD these story-specific checks:
- **Spec fidelity:** each slide matches its confirmed spec (title, visual, figures, callouts).
- **Hero-visual check:** every content slide has ONE dominant visual; none is a card/bullet grid.
- **Text budget:** body text ≤ ~40 words/slide; facts live on the visual (labels/annotations), not in
  prose; the insight rail is micro-points (bold lead + short clause), not paragraphs.
- **Banner discipline:** ≤ ~2 emphasis bands in the whole deck.
- **Every page speaks for itself:** each slide's action title + visual convey the message with no
  narration needed.
- **Emphasis present:** on each chart, the element proving the title is visibly emphasized.
- **Horizontal logic holds** across the final titles.
- **Grayscale survives:** encodings rely on lightness, not hue alone.
- **Aspect ratios preserved:** every icon, logo, image, and rasterized visual keeps its native
  width:height — compute the aspect, set one dimension, derive the other; never set width and height
  independently (it stretches the art). Applies especially to architecture-diagram icons.
- **One body font size per slide;** no text overflow; clean left-aligned grid.

### Phase 6b — Edit one slide at a time (iteration loop)
Decks are refined slide by slide. When the user gives feedback on a specific slide:
1. **Edit that slide's spec** (Phase 3) first — the spec stays the source of truth.
2. Re-run and re-render **only that slide** (or the whole deck if a global token changed); inspect it.
3. **Preserve deck consistency** — re-read the global rules, not just the one spec: same theme/tokens,
   the one-hero-visual rule, the ≤2-banner budget, consistent card geometry and fonts. A local fix
   must not break deck-wide uniformity.
4. Don't silently restyle other slides. If a change should propagate (e.g. the user likes a new card
   size), say so and offer to apply it across the deck.

---

## Design Rules (apply to every slide)

These are the distilled, non-negotiable rules. Full rationale is in `design-system.md`.

### Headline = action title
Bold **serif**, top of slide, left-aligned, 1–2 lines. A complete sentence stating the takeaway,
quantified. **No accent bar, no kicker label, no underline above it** (those belong to other
styles, not this one).

### Layout: "every page is a table"
Lay every slide on an implicit column/row grid — it's why these decks look clean. Default content
split: **insights panel LEFT, chart/visual RIGHT** (answer-first; the classic [method|chart|
implications] arrangement with implications on the RIGHT is an allowed alternative). Charts are
tables too (axis-label + legend row, plot, footnote row).

### Color: single-hue discipline + role system
Use ONE accent hue rendered as a light→dark **ramp**; lightness encodes magnitude/order. Neutral
gray for "no data"/de-emphasis (it recedes). **Peak intensity appears exactly once**, on the single
most important element (the takeaway). Break single-hue ONLY for semantic colors (good/bad
traffic-light; water=blue, sun=orange, coal=black). Pick the right colormap for the data: sequential
(ordered), diverging (has a midpoint), categorical (unordered). See `themes.md` + catalog.

### Emphasis follows narrative (the key chart rule)
The same data can tell different stories — you choose by what you emphasize. Once the action title
states the claim, the chart MUST visually highlight the exact element that proves it (bold line,
saturated bar, exploded slice, widened ribbon, bracket/arrow on the gap) and mute everything else.

### Connective visuals for relationships
When the message is a relationship or disproportion (X% = Y%, a gap, A→B), draw a connector that
carries the eye between the two quantities (flow ribbons, leader lines + bracket, waterfall
connectors). The connector IS the argument.

### Chart craft
Right chart type for the message; toned-down colors with one focal highlight; round numbers (no
excess digits); both a neutral chart title (with units) AND the action title; difference
arrows/brackets for change (pipe the number into the title); explain mixed categories with small
labels; reuse one grouping dimension across the deck; add an implications/insights panel.

### The AVOID list (consulting "bad style")
- **No card-and-bullet slides.** A grid of text cards or a bulleted list is NOT a hero visual — it's
  the default that makes decks look generic and text-heavy. If the slide body is mostly text boxes,
  redesign it around a real visual.
- **No emphasis banner on every slide.** The dark takeaway band is for the deck's biggest moments
  (~1–2 per deck, e.g. the recommendation). Repeating it every slide is filler and reads generic.
- **No repeated layout back-to-back** — vary the hero-visual type slide to slide for rhythm.
- **Don't narrate the visual in prose** — if the text restates what the chart already shows, delete it
  and let an annotation carry any number that matters.
- No color fields behind body text (low-saturation light strips for panels are OK; survive B&W)
- No 3D or heavy shadows — EXCEPTION: subtle dimensionality only when it reinforces a physical
  metaphor (funnel, pipeline, layered stack); everything else flat
- No SmartArt — build frameworks from shapes/icons for full control
- No decorative/background images (only images that add real value)
- One body font size per slide (headline may be larger)

### Components available (recipes in the catalog)
Action-title headline · insight panel (numbered / bulleted / icon+heading+body variants) · annotated
line chart · choropleth + sequential legend · leader-line callout · emphasis takeaway banner (dark or
light-panel variant) · Gantt (phase bands + shape markers) · big-stat callout · icon-array/pictograph
· gauge · dashboard grid · donut + center total · exploded-slice drill-down · waterfall · flow
connector · stacked-bar flow/Sankey · funnel · aligned icon+description rows · comparison matrix.

---

## Notes
- This skill targets REAL data stories — research first, build on facts, reach synthesis.
- The catalog is a living document; new slides/templates get folded in over time. When new
  blueprints are added there, this SKILL.md's mapping table should be updated to match.
- Gaps still thin on examples (use principled defaults, refine when examples arrive): 2×2 matrix,
  L→R process flow, driver/issue tree, cover/divider/exec-summary bookends.
