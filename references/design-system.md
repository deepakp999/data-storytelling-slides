# Data Storytelling — Design System

The complete design system for building insight-driven, editorial data decks. Each slide is a small
argument: a headline that states the takeaway, a visual built to prove it, and a synthesis of what it
means. This document is the source of truth; `SKILL.md` is the operational summary.

Contents:
1. Narrative Architecture (how to turn content into a storyline)
2. Color System Logic (palette-agnostic)
3. Typography & Tokens
4. Pattern Library (slide/chart blueprints)
5. Layout & Production Principles
6. Chart Craft
7. Slide Anatomy & Information Density (the visual-first / compression discipline)

---

## 1. Narrative Architecture (the storytelling layer)

A deck is not a collection of slides — it is a narrative arc. Build the story first, then choose the
visual for each beat. Slide-type selection is content-driven, never random.

### Step 0 — Climb from facts to synthesis
Three levels of insight; the deck must reach level 3:
1. **Facts** — raw, unordered data points (the spreadsheet).
2. **Summary** — an organized, condensed restatement of the facts. Many decks stop here and leave the
   reader to infer the meaning.
3. **Synthesis** — what the summary MEANS: the implications, how it informs a decision. Always deliver
   the synthesis.

Strong data storytelling needs three things together: **visualization + narrative + audience.** The
same data becomes a different story for a different audience, so always begin by asking who the
audience is and what decision they face.

### Step 1 — Choose a narrative arc

| Arc | Shape | Use when |
|-----|-------|----------|
| **SCQA** | Situation → Complication → Question → Answer | Persuading / recommending |
| **Answer-first (pyramid)** | Answer up front → supporting arguments → evidence | Executive audiences |
| **Chronological** | Past → present → future | Performance reviews, trends over time |
| **Problem-Solution** | Problem → impact → solution → results | Pitches, proposals |
| **Compare-Contrast** | Option A vs B vs C → recommendation | Decisions, trade-offs |

### Step 2 — Break the story into beats
Each beat = one slide = one single message. The slide headline IS that message. A beat answers:
"What is the ONE thing the audience should take away here?"

### Step 2b — Write each headline as an ACTION TITLE
The headline is the single most important element of a slide. An action title is the header that
summarizes the slide in a complete sentence stating its conclusion.

Rules:
- **State the takeaway, not the topic.** "Energy mix by source" is a label; "Fossil fuels still supply
  three-quarters of demand while renewables stall below the target" is an action title.
- **A complete sentence** that makes a claim — not a few-word category name.
- **Specific and quantified** — put the actual number in the title; the figure makes it concrete.
- **One message per slide** — if the title needs an "and" joining two unrelated claims, it's two slides.
- **Concise** — the fewest words that carry the message.

The **refinement ladder** (climb it for every title):
1. ❌ Label, not a sentence — "Findings on regional expansion" (doesn't say what was found)
2. 🔸 Full sentence but generic — "Expanding into new regions looks attractive"
3. 🔸 Specific — "Expanding into the North and Coastal regions looks attractive"
4. ✅ Quantified — "Expanding into the North and Coastal regions adds an estimated $40M in profit"
5. ⭐ Quantified + concise — "North and Coastal expansion adds ~$40M profit potential"
(Example figures above are illustrative.)

**The "so what?" test (also a slide-existence test):** if you cannot write a strong action title, the
slide probably lacks a "so what" — a clear implication. Cut or restructure any slide with no so-what.

**Two logics the title set must satisfy:**
- *Horizontal logic:* reading ONLY the titles, top to bottom, narrates the whole argument.
- *Vertical logic:* each title is faithfully proven by what's on its page and claims no more.

**Format:** 1–2 lines, left-aligned, top of slide, bold serif display face, no kicker label / accent
bar / underline above it.

### Step 3 — Map each beat to the right slide type

| If the message is about… | Use… |
|---|---|
| A trend over time | Annotated line chart (highlight the focal series) |
| Comparing magnitudes across categories | Bar/column chart |
| Geographic distribution | Choropleth map + callouts |
| A schedule / project plan | Gantt with phase bands + shape-coded markers |
| One dramatic number | Big-stat callout |
| Many KPIs at a glance | Dashboard grid (stats, gauge, pictograph) |
| A ratio / proportion | Icon-array / pictograph |
| A rating on a bad→good scale | Gauge with semantic colors |
| Parts of a whole | Donut with center total |
| A part's share AND its internal breakdown | Donut + exploded-slice drill-down |
| Cause/effect, bridge between two totals | Waterfall (anchors vs steps) |
| Two distributions / disproportion | Stacked-bar flow (Sankey-style ribbons) |
| A staged conceptual framework | Funnel diagram |
| Trade-offs across options | Comparison matrix / table + callouts |
| Qualitative reasoning | Insight panel + supporting visual |
| The recommendation | Answer-first summary slide |
| Deck open / close | Cover · section divider · executive summary |

Vary slide types across consecutive slides for visual rhythm.

### Step 3b — Emphasis follows narrative (the key chart rule)
The same data can support different stories — you choose which by what you emphasize. Identical data
can be framed as "the leader's runaway rise" (bold + color the leader's series) or "a challenger
holding share despite the leader" (emphasize the challenger), depending on the audience and the
action title. The data does not change; the emphasis does.

Rule: once the action title states the claim, the chart must visually emphasize the exact element that
proves it (bold line, saturated bar, exploded slice, widened ribbon, bracket/arrow on the gap) and let
everything else recede into muted neutral. Build charts title-first.

### Step 4 — Sequence & connect
- Each headline logically leads to the next (the "so what" chain).
- Open with the storyline/agenda or the answer (depending on arc); close with the recommendation.
- Vary dense and breathing slides; don't stack two of the same layout back-to-back.

### Step 4b — Make relationships visible (connective visuals)
When the message is a relationship or disproportion (X% of base = Y% of value, a gap between series, A
bridges to B), don't make the viewer compute it from separate numbers. Connect the two quantities with
a visual that carries the eye between them — flow ribbons, leader lines + bracket, waterfall
connectors. The connector IS the argument; pick the chart type that lets you draw it.

### Generation workflow (when a deck is requested)
1. Understand content + audience + goal; gather/research data.
2. Choose the arc.
3. Draft ALL action titles as the outline; run the so-what + horizontal-logic + ladder checks.
4. (Optionally confirm the outline with the user.)
5. Map each title → slide type; loop back to research if a chart needs data you lack.
6. Build using this design system.
7. QA the visuals AND the narrative flow.

---

## 2. Color System Logic (PALETTE-AGNOSTIC — the "why")

The goal is not a fixed color — it is a role system that produces a great deck in any palette. Colors
are assigned to ROLES; swap the hue, keep the logic. (Concrete fallback values and the derivation
recipe live in `themes.md`.)

### The 6 color roles
| Role | What it does |
|------|--------------|
| **Canvas** | The stage. Maximally neutral so content pops. |
| **Ink** | All primary text. Maximum contrast on canvas. |
| **Brand hue** | The single chosen accent hue = identity. ONE hue only. |
| **Data ramp** | Sequential tints/shades of the brand hue. Encodes magnitude / order. |
| **Muted/neutral** | Desaturated gray. "No data", de-emphasized, secondary, outline icons. |
| **Emphasis** | Darkest/most-saturated brand step. Used ONCE, on the single most important element. |

### Why it works (principles)
1. **Single-hue discipline.** Vary one hue by lightness/saturation rather than using rainbow colors.
   Ordered data calls for an ordered visual variable; lightness is perceptually ordered, hue is not.
2. **Lightness = magnitude.** Darker reads as "more"; a sequential ramp is legible without constant
   legend checks.
3. **Saturation = importance; neutral recedes.** Desaturated gray drops behind meaningful colored data.
4. **Peak intensity appears exactly once,** on the most important element (the takeaway).
5. **Neutral containers, accent identifiers.** Keep boxes/cards neutral; tie them to the system with a
   small dab of brand hue (e.g., a thin left bar), so many callouts don't create color chaos.
6. **Single-hue quasi-categorical.** For a few (≤5) categories, use distinct lightness STEPS of the
   ramp as the category colors; separation comes from lightness, not new hues.
7. **Encode extra categories by SHAPE, not more color.** When categorical events sit on a colored chart
   (e.g., milestones on a timeline), distinguish them by shape (triangle/diamond/circle).
8. **Semantic-color exception.** Break single-hue ONLY when a color carries meaning the audience
   already shares: traffic-light red/amber/green for bad/caution/good.
9. **Semantic categorical colors.** Also for categories with widely-shared color associations
   (water→blue, sun→orange/yellow, fuel/coal→black, vegetation/eco→green). Meaning-driven, never decorative.
10. **Direction-aware encoding.** Before using diverging +/− colors, check the data moves both ways. If
    all steps go one direction (e.g., all costs), encode "totals vs steps" (brand vs neutral-dark) instead.

### Choosing the colormap for the data type
| Data type | Colormap | Construction |
|-----------|----------|--------------|
| Ordered / quantitative | **Sequential** | One hue, light→dark = low→high |
| Has a meaningful midpoint (+/−) | **Diverging** | Two hues meeting at a neutral middle |
| Unordered categories | **Categorical** | Distinct hues of similar lightness (or lightness steps if ≤5) |

### Guardrails
- Near-black text on canvas; white/lightest text on emphasis/dark fills.
- Don't rely on hue alone — pair with lightness so encodings survive grayscale and color-blindness.
- ≤5 sequential steps; adjacent steps must be distinguishable.

---

## 3. Typography & Tokens

**Font pairing (the signature move):** a bold **serif** display headline paired with a clean
**sans-serif** for everything else (labels, body, chart text, legends). The serif/sans split gives the
deck an editorial, "published article" feel rather than a generic corporate look.

| Element | Style |
|---------|-------|
| Action-title headline | Serif, bold, ~28–32pt, ink. 1–2 lines, left-aligned, no chrome above. |
| Section label | Sans, bold, ~16–18pt, ink (e.g. "Key Insights"). |
| Chart title | Sans, bold, ~14–16pt, ink (neutral description + units). |
| Chart subtitle | Sans, ~11–12pt, muted. |
| Body | Sans, ~12–14pt, ink / muted. |
| Axis / labels | Sans, ~10–11pt, muted. |

**Hard rule:** ONE body font size per slide; only the headline may be larger.

**Spacing & grid**
- 16:9 (13.33" × 7.5"); outer margin ~0.6–0.8" on all sides.
- **Dominant visual + thin insight rail** (not a 50/50 split): the hero visual takes ~55–70% of the
  body; the insight rail is a narrow column of micro-points (answer-first default: rail LEFT, visual
  RIGHT — or visual full-bleed with the rail as a slim overlay/footer). See §7 for the spec.
- Card radius consistent (~12px); subtle shadows only.
- Leave ~30% of each slide as breathing room.

---

## 4. Pattern Library (slide/chart blueprints)

Each entry: the message it serves + how it's built. Mix variants; don't repeat a layout on consecutive
slides.

### P1 — Annotated line chart (trend over time)
Insight panel left, line chart right. Multiple series; the focal series is bold/saturated, others
muted. Annotate the key relationship with a bracket + number (e.g., the gap between two series). Legend
compact, near the plot.

### P2 — Choropleth map (geographic distribution)
Full-bleed map; regions filled by a sequential ramp (light→dark = low→high), absent regions in muted
gray. Sequential legend with the metric definition. Leader-line callout boxes spotlight specific
geographies (white box, thin brand-hue left bar, bold label + short note). Optional emphasis banner
with the overall takeaway.

### P3 — Gantt / timeline (schedule, project plan)
Left task column; right week/month grid. Phase header rows banded in light gray group their tasks.
Duration bars colored by phase using distinct lightness steps of the brand hue. Milestones as
shape-coded markers (e.g., triangle = deliverable, diamond = meeting) with inline labels. Connector/key
legend at the bottom.

### P4 — KPI dashboard (many metrics at a glance)
Multi-column masonry of cards (varied heights), dashed dividers between columns. Card types:
- **Big-stat callout** — hero number in brand hue, small label above, caption below.
- **Icon-array / pictograph** — repeated icons, filled+brand = achieved portion, outline+gray = remainder.
- **Gauge** — semicircular arc on a bad→good scale (semantic colors), needle to the value.
- **Icon-card** — brand-hue icon + bold heading + short body (the qualitative card).

### P5 — Donut + drill-down (parts of a whole, with one part expanded)
Donut ring sized by share, hole shows the grand total. Segments use the ramp; a semantically-colored
category (e.g., fuel = black) is allowed. Pull the slice of interest OUT and connect it with dashed
leaders to a secondary bar chart that decomposes it (with its own total). Answers "how big is it" and
"what's inside it" together.

### P6 — Waterfall (cause/effect, bridge between two totals)
Anchor bars (start total, end total) on the baseline; intermediate bars float. Dashed connectors link
bar tops to show the running total. Value labels above bars; first & last x-labels bold (the totals).
Color: brand hue for the totals/anchors, neutral-dark for the step bars (when all steps go one way).
Bracket annotation quantifies the cumulative change.

### P7 — Stacked-bar flow / Sankey (two distributions, disproportion)
Two 100%-stacked bars side by side, each with its own total label. Same categories in both. Translucent
flow ribbons connect each category between the bars; ribbon width tracks share, so a small slice on one
side visibly balloons on the other. The ribbons make the disproportion self-evident.

### P8 — Funnel (staged conceptual framework)
Stacked trapezoid segments narrowing top→bottom, single-hue ramp encoding stage depth, white centered
labels. Left: aligned icon + description rows mapping to each stage. Right: a light summary panel
(check icon + heading + bullets). Subtle dimensionality is acceptable here because a funnel is a
physical metaphor (see the 3D exception in §5).

### Cross-cutting components
- **Action-title headline** (every content slide).
- **Insight panel**, three interchangeable variants: numbered circles (ranked/sequential), bullets
  (parallel peers, optional inline-bold), icon + heading + body (each insight gets a mini-title).
- **Leader-line callout box** · **emphasis takeaway banner** (dark high-drama variant OR light tinted-panel
  variant) · **flow connector** (chevron in a brand-hue circle between panels) · **chart header**
  (icon + title + subtitle) · **aligned icon+description rows**.

### Blueprints to expand (thin on examples — use principled defaults for now)
Column/list (3–4 text columns), comparison matrix (options × criteria), 2×2 quadrant, L→R process flow,
driver/issue tree, and the deck bookends (cover, section divider, executive summary).

### Rendering visuals beyond native charts
For heroes that pptxgenjs can't draw natively — **choropleth maps, Sankey/flow ribbons, treemap /
circle-pack, chord/circular flow, network graphs, custom radial or smoothed geometry** — see
`advanced-visuals.md`. It defines the general technique (compute geometry with a d3 module → emit SVG →
rasterize with sharp → place as the hero image → overlay native title/callouts/legend/banner aligned to
the same scale) and ships a concrete, proven **choropleth** recipe (d3-geo + topojson + world-atlas +
world-countries, with centroid-anchored callout cards and a sequential legend). Default to native (Tier
A) or shape-built (Tier B) visuals whenever they suffice; reserve rasterized heroes for real geometry
gaps.

---

## 5. Layout & Production Principles

### "Every page is a table"
Think of every slide as an (often invisible) grid of columns and rows. You don't redesign each page —
you ask "what's my information and how does it fit a table?" A common base form: a TITLE column (names
the items) + a DETAIL column (specifics per item). Charts are tables too (axis-label + legend row, plot,
footnote row). With the table in your head, you spend effort on content, not layout.

### "Every page speaks for itself"
A reader who sees ONE page in isolation must grasp its message with nobody there to explain it. This
requires (1) minimum design quality — aligned, consistent — and (2) self-contained content: the action
title + visual carry the message alone. This is a per-slide QA gate.

### The universal data-analysis layout
For an "we analyzed X" slide, lay out left→right because people read left→right: optional methodology
(left) → chart/evidence (middle) → implications (right). NOTE the convention choice: the classic
arrangement puts implications on the RIGHT; the answer-first default in this system puts the insight
panel on the LEFT with the chart on the right. Both are valid since the action title already states the
answer; pick one and stay consistent.

### The AVOID list
1. **No color fields behind body text** — hurts contrast and fails in black-and-white printing. Small,
   low-saturation light strips/panels for titles or one highlight are acceptable.
2. **No 3D, minimal shadows** — heavy effects read as unprofessional. EXCEPTION: subtle dimensionality
   only when it reinforces a physical/spatial metaphor (funnel, pipeline, layered stack); else flat.
3. **No auto-generated diagram widgets** — build frameworks from individual shapes + icons for full
   control over fonts and proportions.
4. **No decorative/background images** — only add an image if it adds real value.
5. **One body font size per page** — the action title may be larger; everything else is one size.

### Grayscale / print check
Slides are often printed black-and-white: a good slide stays readable in grayscale. This is why the
color system never relies on hue alone. Add to QA: glance at a grayscale version — do contrasts and
encodings survive?

---

## 6. Chart Craft (data-viz cleanup techniques)

Concrete moves that turn a raw chart into a storytelling chart:
1. **Pick the chart type for the message,** not the default (a 100%-stacked column of share-over-time
   reads worse than a line chart).
2. **Tone down colors;** reserve saturation for the one focal element.
3. **Highlight the focus series** — bold it, give it the one distinct color, add a marker/logo if apt;
   mute everything else. (This is "emphasis follows narrative" made operational.)
4. **Round the numbers** — no excess digits; round to thousands/millions.
5. **Add both a chart title and an action title** — neutral "what you're looking at" (with units) plus
   the so-what narrative on top of the slide.
6. **Use difference arrows / brackets for change** and pipe the computed number into the title.
7. **Explain mixed categories** with small labels/boxes — never leave a mixed grouping implicit.
8. **Reuse one grouping dimension** across the deck so the story sticks.
9. **Left-align labels and elements** for a clean grid.
10. **Distinguish positives from negatives** with color only when the sign matters (semantic exception).
11. **Add an implications/insights panel** beside the chart — what it MEANS, not random points.

---

## 7. Slide Anatomy & Information Density (the visual-first / compression discipline)

The single biggest failure mode for these decks is the "generic template": every slide a grid of text
cards or bullet lists, an emphasis banner on each one, no real picture. It looks tidy and says nothing.
This section is the antidote. Core principle: **the visual carries the information; text is a thin top
layer.** Dense research is compressed *into* the visuals, not poured onto the slide as prose.

### 7.1 Canonical content-slide anatomy
Every content slide = four layers (plus an optional fifth):
1. **Action title** — serif, quantified takeaway, 1–2 lines, top, left-aligned.
2. **Hero visual** — ONE dominant chart/diagram/map/framework, ~55–70% of the body area, built
   title-first (the element proving the title is emphasized; the rest recedes).
3. **Insight rail** — a thin column (or footer strip) of **3–4 micro-points**. Each micro-point is a
   **bold lead phrase + a ≤~8-word clause** ("**66% WTP** — urban Indians, NielsenIQ"). This is the
   synthesis layer: the so-what the picture can't say alone. Never paragraphs, never a card grid.
4. **On-visual annotations** — endpoint callouts, bracket+number on a gap, a highlighted point, a tag
   pinned to a curve, direct data labels. The bulk of the detailed numbers live here, on the picture.
5. *(optional)* **one emphasis element** — a takeaway band or hero number, only if it earns it.

### 7.2 Reference layout — "hero visual + insight rail" (16:9, 13.33×7.5)
A reliable default (answer-first; mirror L/R for the classic arrangement):
- Title block: x 0.6, y 0.4, full width, ~0.95" tall.
- Insight rail: x 0.6, width ~3.6–4.0", from y ~2.0 down; a big hero stat at top, a hairline divider,
  then 3–4 micro-points evenly spaced.
- Hero visual: x ~4.5, width ~8.2", y ~2.0, height ~4.6" — the dominant element.
- Footnote (source / units): x 0.6, y ~6.95, small italic muted.
Alternative: hero visual full-bleed under the title with the insight rail as a slim left overlay or a
bottom strip of stat callouts. Either way the visual dominates and the text is a rail, not a panel.

### 7.3 Text budget
Outside the action title and the visual's own labels/annotations: **≤ ~40 words of body text per
slide.** If over, you're narrating the picture — move facts into annotations or cut. One idea per slide.

### 7.4 Compression techniques (fit ALL the research into ~10 slides, visually)
- **One chart beats a table** — let bars/points/positions carry every value.
- **Annotate, don't write** — supporting figures become data labels, axis ticks, endpoint callouts,
  brackets, pinned tags ON the chart.
- **Stat strip** — a row of 3–5 big-number callouts packs many figures into one calm frame (good for a
  "demand / market signals" slide).
- **Small multiples** — repeat a tiny chart across categories instead of describing each in text.
- **Encode, don't list** — categories by position/color/size; order by a ramp; status by semantic color.
- **Micro-points** — secondary detail goes in the insight rail (bold lead + short clause), the highest
  information-per-word text form.
- **Appendix** — genuinely secondary material is a back-of-deck slide, not body weight.

### 7.5 Visual-first ≠ sparse
The opposite error: a near-empty slide with one giant number and three words. That under-delivers. The
fix is always the same balance — **dominant visual + dense on-visual annotation + tight insight rail** —
so the slide is simultaneously calm and information-rich. Calibrate to the middle, not either extreme.

### 7.6 Insight-rail micro-point & stat-strip (component specs)
- **Micro-point:** a small brand-hue tick/square (~0.13") + text where the **lead phrase is bold ink**
  and the rest is muted; ~11pt; 3–4 per rail, generous vertical spacing. No bullets-in-a-box.
- **Stat strip / stat callout:** big serif number (brand-dark, ~26–34pt) + a 1-line muted caption; a
  thin brand left-bar; group 3–5 across a row. Use for KPI/market-signal slides instead of bullet lists.

### 7.7 "Is this slide generic?" self-check
Fail any → redesign before moving on:
- Is the body mostly text boxes / bullets with no dominant visual? → generic; add a hero visual.
- Could a reader get the so-what from the title + visual alone (rail covered)? → if no, the visual
  isn't doing the work.
- Is there an emphasis banner here AND on most other slides? → cut; reserve for ~1–2 per deck.
- Does the body text restate what the visual shows? → delete it; annotate instead.
- Same layout as the previous slide? → vary the hero-visual type.

---

## 8b. Per-slide prompt specs (worked example)

The Phase-3 spec is a structured brief — the editable contract for each slide and the instruction used
to regenerate it later. Written AFTER research, so every figure is real and sourced. Two examples:

```
Slide 3 — The prize (market)
  Action title:  India's sustainable-packaging market is on track to grow ~6× to $57B by 2033
  Hero visual:   Area chart, market size US$B, 2024→2033 (dominant, right ~65%)
  Data on it:    $9.0B (2024) → $57B (2033) at 22.5% CAGR; waypoint ~$20B (2028). Source: IMARC
  Callouts:      "$9.0B" at start; dark "$57B by 2033" tag pinned to the 2033 end; "~$20B by 2028" dot
  Insight rail:  "22.5% CAGR — fastest packaging segment" · "APAC ~21% / ~43% of revenue" ·
                 "Biodegradable = regulation-driven core" · "India under-penetrated"
  Emphasis:      The 2033 endpoint. Semantic colors: none (single green ramp)
  Notes:         Footnote names metric + that biodegradable is a premium subset

Slide 7 — Where to play (competition)
  Action title:  Crowded in bagasse tableware but wide open in compostable flexible packaging
  Hero visual:   2×2 bubble map — x = competition (low→crowded), y = opportunity (pull × volume);
                 bubble size = segment volume; top-left quadrant tinted "WHITE SPACE"
  Data on it:    Bubbles: Compostable flexible (low comp / high opp, highlighted, "← target");
                 Bagasse tableware (high comp / mid); Molded fiber/rigid (mid/mid); Bioplastic resins
  Callouts:      "4,078 players · ~30 funded (Tracxn)" corner note
  Insight rail:  "Tableware = price war" · "Flexible barely served" · "Target the open quadrant"
  Emphasis:      The highlighted "Compostable flexible" bubble. Semantic colors: none
  Notes:         Name a few real leaders as labels (Ecoware, Pakka/CHUK, Pappco)
```

Present all specs together; invite edits to any field; the edited specs become the build contract.
For single-slide iteration (Phase 6b), edit only that slide's spec and re-render only that slide,
while re-checking the global rules (theme, one-hero-visual, ≤2 banners, consistent card geometry).
