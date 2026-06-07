---
name: data-storytelling-slides
description: "Create consulting-grade, data-storytelling PowerPoint decks in an editorial 'insight-driven' visual style (bold serif action-title headlines, a single-hue color system, minimal chrome, charts that carry the argument). Use this skill WHENEVER the user wants a presentation, slide deck, pitch, board/executive update, or any .pptx that should look like a polished executive data story — especially when the request is just a TOPIC (e.g. 'make a deck on X', 'present our Q3 numbers', 'build slides explaining the EV market'). The skill researches the topic, designs a storyline, picks the right chart/slide type per message, and builds the full deck. It is palette-agnostic (derives a theme from any brand color) with a built-in fallback theme. This is the DEFAULT style for data-driven / analytical / narrative decks. Use it even if the user doesn't say 'data storytelling' — if they want a polished analytical deck with charts and a clear argument, use this."
---

# Data Storytelling Slides

Build decks where every slide is a small argument: an **action-title headline** stating the
takeaway, **evidence** (a chart/visual built to prove exactly that takeaway), and **synthesis**
(what it means). The look is editorial and minimal — bold serif headlines, one accent hue, white
canvas, no decorative chrome.

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

---

## The Workflow (follow in order)

When the user gives a topic (or topic + data), run these phases. Don't skip the storyline phase —
picking slide layouts before the storyline exists is the #1 way decks turn into a pile of charts.

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
5. **QA the outline before building:**
   - *So-what test:* every slide has a real implication. If a slide has no so-what, cut or restructure it.
   - *Horizontal logic:* reading only the action titles top-to-bottom narrates the whole argument.
   - *Vertical logic:* each title is provable by what will be on its page; it claims no more.
6. **Present the outline (action titles + one-line evidence note per slide) to the user for a quick
   confirm** before building the full deck, unless they asked you to just produce it.

### Phase 3 — Map each beat to a blueprint (slide type)
For each action title, choose the slide/chart type that best *carries* that message. Use the
beat→type table in `design-system.md` → "Step 3". Quick map:

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
| A staged conceptual framework | Funnel diagram |
| Options × criteria | Comparison matrix / table + callouts |
| Qualitative reasoning | Insight panel + supporting visual |
| The recommendation | Answer-first summary slide |
| Deck open / close | Cover · section divider · executive summary |

If the chosen blueprint needs data you don't have yet, **go back and search** for it (Phase 1 loop).
Vary blueprint types across consecutive slides for rhythm.

### Phase 4 — Theme
Read `references/themes.md`. If the user gave a brand color/palette, derive the 6 roles from it; else
use the **Editorial Mint** fallback. State the chosen theme in one line. Keep it consistent deck-wide.

### Phase 5 — Build
Read the `pptx` skill, then build with pptxgenjs. Apply the design rules below to every slide.
Build each chart **title-first**: decide what the action title claims, then construct the chart so
that the element proving it is emphasized and everything else recedes (see "Emphasis follows
narrative" below).

### Phase 6 — QA (visual + narrative)
Run the `pptx` skill's visual QA loop (render to images, inspect with fresh eyes, fix real defects).
ADD these story-specific checks:
- **Every page speaks for itself:** each slide's action title + visual convey the message with no
  narration needed.
- **Emphasis present:** on each chart, the element proving the title is visibly emphasized.
- **Horizontal logic holds** across the final titles.
- **Grayscale survives:** encodings rely on lightness, not hue alone.
- **One body font size per slide;** no text overflow; clean left-aligned grid.

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
