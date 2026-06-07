# Advanced Visuals — Rendering Recipes

`design-system.md` says WHICH visual a message needs. This file says HOW to render the ones that go
beyond native pptxgenjs charts — without sacrificing the editorial look or editability of text.

All packages below are installable from npm in the build environment and have been verified. Rasterize
SVG with `sharp`; generate icons with `react-icons` + `sharp` (see the build setup).

**Pattern catalog:** the **D3 Graph Gallery** (https://d3-graph-gallery.com) is the go-to reference for
Tier-C chart construction — it's organized by the same *message intent* this skill uses (Distribution,
Correlation, Ranking, Part-of-a-whole, Evolution, Map, Flow) and gives reproducible D3 for each. Use it
to recall the structure/d3 modules for a chart family, then write original code adapting the standard
d3 pattern to our SVG→rasterize→native-overlay flow and the brand ramp. (Don't copy its code verbatim;
the d3 API is the real dependency.)

---

## The three rendering tiers (pick the lowest that suffices)

**Tier A — Native pptxgenjs charts** (fully editable; prefer when adequate):
column/bar, line, area, pie/doughnut, scatter, radar, xy-bubble. Style per the design system
(single-hue, one focal highlight, neutral rest, round numbers, data labels as annotations).

**Tier B — Built from pptxgenjs primitives** (shapes + lines + text + raster icons; fully editable;
full control): big-stat callouts & stat strips, pictographs (icon arrays), 2×2 bubble/quadrant maps,
diverging/tornado bars, waterfall, funnel, semicircular gauge, timeline / Gantt, L→R process & flow
diagrams, comparison matrices/tables, roadmaps, insight rails, callout cards, emphasis banners. Most
slides are Tier A or B.

**Tier C — Generated as SVG, rasterized, placed as the hero image, with native overlays** (for geometry
pptxgenjs can't draw: maps, ribbons, packed hierarchies, circular flows). This is the general technique
below. Use it only when Tier A/B genuinely can't carry the message.

---

## The Tier-C technique (generalize this)

1. **Compute geometry** with the right d3 module (projection, sankey, treemap, chord…).
2. **Emit an SVG string** sized to the on-slide image rect's *pixel* proportions (W×H). Transparent
   background so the white canvas shows; fill marks from the **brand ramp**; thin **white strokes**;
   neutral gray for "no data". Render at ~1.5–2× the placed size for crispness.
3. **Rasterize:** `"image/png;base64," + (await sharp(Buffer.from(svg)).png().toBuffer()).toString("base64")`,
   then `addImage({data, x, y, w, h})` with **w:h equal to W:H** (no distortion).
4. **Keep text & annotations NATIVE.** Compute their positions from the **same scale/projection** you
   used for the SVG, so titles, callouts, leader lines, legends, and banners align to the image.
   Native text inherits the deck fonts and stays crisp/editable; only the marks live in the image.
5. **Obey the design system inside the SVG too** — single-hue ramp, emphasis once, neutral recede.

px→inch mapping (image placed at `mapX,mapY` sized `mapW,mapH` inches; SVG is `W×H` px):
`xIn = mapX + (px/W)*mapW ; yIn = mapY + (py/H)*mapH`

**Caveats:** the rasterized hero isn't click-editable geometry (acceptable for real data-viz — keep
title/legend/callouts/banner native). Don't bake body text into the SVG (font mismatch). Prefer
**deterministic** layouts (sankey/treemap/chord are deterministic given sorted input); for force graphs
fix iterations + seed positions so runs are reproducible.

---

## Visual → library / approach map

| Visual | Tier | Library / how |
|---|---|---|
| Bar, line, area, pie/doughnut, scatter, radar | A | pptxgenjs native charts |
| **Architecture / system diagram** | B+ | real icons (`@iconify/json`: `logos` + `devicon`) in labeled zones + connectors — see recipe |
| Stat callout / strip, pictograph, gauge, funnel, waterfall, 2×2 bubble, diverging bar, timeline/Gantt, process flow, matrix | B | pptxgenjs shapes + lines + text + raster icons |
| **Choropleth / geographic map** | C | `d3-geo` + `topojson-client` + `world-atlas` (countries) / `us-atlas` (US states) + `world-countries` (region join) |
| **Sankey / flow ribbons** | C | `d3-sankey` (layout) + `sankeyLinkHorizontal` (ribbon paths) |
| **Treemap / circle-pack / partition** (nested parts-of-whole, hierarchy) | C | `d3-hierarchy` |
| **Chord / circular flow** (matrix of A↔B relationships) | C | `d3-chord` + `d3-shape` (arc, ribbon) |
| **Network / relationship graph** | C | `d3-force` (fix iterations + seed for determinism) |
| **Custom radial arcs, smoothed/curved areas, streamgraph** | C | `d3-shape` (arc, area + curve) + `d3-scale` |
| Color scales/ramps for any of the above | — | hand-rolled brand-ramp lerp, or `d3-scale` + `d3-scale-chromatic` |

---

## Recipe: Choropleth map (the proven one)

Packages: `d3-geo topojson-client world-atlas world-countries` (US: add `us-atlas`).

1. **Load geometry:** read `world-atlas/countries-50m.json`; `topojson.feature(topo, topo.objects.countries).features`; filter out Antarctica (saves vertical space).
2. **Join data → region:** build `ccn3 → subregion` from `world-countries`; map each feature's `id`
   (numeric ISO, match via `parseInt`) to a **subregion → value** table built from REAL researched
   figures. Color via a brand-ramp lerp (define ~6 stops light→dark across the value domain); unmapped
   → neutral `noData` gray.
3. **Project & draw:** `geoNaturalEarth1().fitSize([W,H], featureCollection)`; `geoPath(proj)`; one
   `<path d=… fill=ramp stroke="#fff" stroke-width=0.6>` per feature. (US: `geoAlbersUsa` + `us-atlas`
   `states-10m`, join by state FIPS.)
4. **Rasterize** to PNG and place as the hero (Tier-C step 3).
5. **Callout cards (native):** for each highlighted country, `path.centroid(feature)` → px → inch;
   draw a leader line (a `LINE` with `flipH/flipV` set from the box→target direction) + a dot at the
   centroid + a white card with brand left-bar, bold name, big %, one-line label. Use **reported**
   country values on the cards even when the map is shaded by regional average.
6. **Legend (native):** a stepped gradient bar (≈28 thin rects across the value domain) + min/mid/max
   labels + a "no data" swatch. Caption names the metric.
7. **Headline + takeaway banner (native):** serif action title stating the takeaway; one dark band.
8. **Footnote:** state the metric, the shading basis (e.g. regional averages) and the source.

Worked example shipped in this skill's history: global generative-AI adoption (Microsoft 2025), shaded
by subregion, callouts on UAE 64.0% / Singapore 60.9% / Norway 46.4%, green sequential legend, takeaway
band. That slide is the canonical reference for this recipe.

---

## Recipe sketch: Sankey (flow ribbons)

`d3-sankey`: build `{nodes, links:[{source,target,value}]}`; `sankey().nodeWidth(…).nodePadding(…).extent([[0,0],[W,H]])(graph)`.
Draw node rects (brand ramp by column/stage) and link ribbons via `sankeyLinkHorizontal()` as SVG paths
with `stroke-width = link.width`, translucent fill. Rasterize; overlay native node labels + values +
the action title. Emphasis: widen/saturate the one focal flow, mute the rest.

## Recipe sketch: Treemap / circle-pack (nested parts-of-whole)

`d3-hierarchy`: `hierarchy(data).sum(d=>d.value).sort(...)`; `treemap().size([W,H]).padding(2)(root)`
(or `pack()`); draw a rect/circle per leaf, fill by brand ramp (lightness = value), white gutters,
labels on the larger cells only. Rasterize; native title/legend. Good when a bar chart has too many
categories or the data is hierarchical.

## Recipe sketch: Chord (A↔B matrix relationships)

`d3-chord`: `chord().padAngle(…)(matrix)`; arcs via `d3.arc()`, ribbons via `d3.ribbon()`. Outer arcs =
groups (brand ramp), ribbons = flows (translucent). Rasterize; native group labels + title. Use
sparingly — only when bilateral flows between many categories are the point.

---

## Notes
- Default to Tier A/B; Tier C is for genuine geometry gaps, not decoration.
- One Tier-C hero per slide max; never stack two rasterized visuals on a slide.
- Keep the build script's slide blocks separate so a Tier-C slide can be re-rendered alone (Phase 6b).
- New recipes get folded in here as they're proven; update the map table when you add one.

---

## Chart catalog by message intent (mirrors the D3 Graph Gallery)

When a beat's message fits one of these intents and Tier A/B can't carry it, build the family below as
Tier C (d3 → SVG → rasterize → native overlay). Reference the gallery page for the construction pattern.

| Intent | Chart families | d3 modules / how |
|---|---|---|
| **Distribution** | histogram, density, boxplot, violin, ridgeline | `d3-array` (bin/quantile) + `d3-shape` (area + curve); stack ridgelines with shared x |
| **Correlation** | scatter, connected scatter, bubble, heatmap, correlogram, 2D density | native scatter (Tier A) for plain XY; `d3-scale` + grid rects for heatmap/correlogram; `d3-contour` for 2D density |
| **Ranking** | barplot, lollipop, radar/spider, parallel coordinates, circular barplot, wordcloud | bar/lollipop = Tier B; radar/parallel/circular-bar = `d3-shape` (line/arc) + `d3-scale` (incl. `scalePoint`, `scaleRadial`) |
| **Part of a whole** | treemap, doughnut/pie, dendrogram, circular packing | `d3-hierarchy` (treemap/pack/cluster); pie/doughnut native (Tier A) |
| **Evolution** | line, area, stacked area, streamgraph | line/area native (Tier A); stacked/stream via `d3-shape.stack` + `stackOffsetWiggle` |
| **Map** | background, choropleth, bubble map, hexbin map, connection map, cartogram | `d3-geo` + topojson (+ `d3-hexbin` for hexbin; great-circle `geoPath` lines for connection maps) |
| **Flow** | sankey, chord, network, arc diagram, edge bundling | `d3-sankey`; `d3-chord`; `d3-force` (seed it); arc/bundling via `d3-shape` + `d3-hierarchy` bundle |

Notes: prefer native (Tier A) for the simple evolution/correlation/part-of-whole charts; reach for the
d3 family only when the simpler tier can't express the message. Keep marks in the SVG, text native.

## Python / Bokeh — why we keep one path

Considered and **declined for this skill.** Bokeh (and Plotly) target *interactive* HTML/JS; a .pptx is
static, so the interactivity is wasted, and static PNG export needs a headless browser + webdriver —
heavy and often network-blocked. The d3→`sharp` path is browserless, deterministic, and already proven,
and the gallery above covers the chart space. If a Python path is ever genuinely required, use
**matplotlib** (renders PNG directly, no browser) rather than Bokeh — but avoid mixing Python charts
into the Node/pptxgenjs build unless there's a specific reason; one rendering path keeps decks
consistent and reproducible.

---

## Consulting-grade chart features (the strategy-deck "chart add-in" look)

The polished waterfall / Mekko / Harvey-ball charts associated with strategy-consulting PowerPoint
add-ins are nearly all reproducible here. The chart TYPES are standard dataviz; the "smart" automation
(auto totals, labels, series connectors, difference/CAGR arrows, perfect label positioning, round-to-
100%) we reproduce by **computing it in the build script** — totals, CAGR, and connector endpoints are
just arithmetic we already control. We don't get live in-PowerPoint relayout or Excel/Tableau data
links, but the rendered slide matches. Tiers:

| Feature | Tier | How to build |
|---|---|---|
| Waterfall / bridge (EBITDA, P&L) | B | anchors + floating step bars + dashed connectors + Δ bracket (P6) |
| Mekko / Marimekko (variable-width stacked) | B | `d3-scale` for widths∝totals & heights∝share; pptx rects; recipe below |
| Stacked / clustered / 100% + totals + series connectors | A+B | native bars (Tier A) + overlay connector lines & computed total labels |
| Difference arrows & series CAGR arrows | B | arrow shape + computed Δ, or CAGR = (end/start)^(1/yrs) − 1, label piped in |
| Harvey balls / status dots | B | neutral ring + brand `PIE` wedge at 0 / ¼ / ½ / ¾ / full; recipe below |
| Gantt / calendar timeline (+ status, milestones, brackets) | B | week/month grid + phase bars + shape-coded markers (P3) |
| Tornado / butterfly | B | diverging bars from a center axis |
| Football-field (valuation ranges) | B | floating min–max range bar per row + a reference line |
| Line / area / 100% area / pie / doughnut / scatter / bubble | A | native pptxgenjs charts + Tier-B label/marker overlays |
| Process flow / chevrons / agenda / smart text boxes | B | rounded rects + chevrons + computed alignment |

Most of these are Tier B (shapes) or Tier A (native) — they do NOT need the Tier-C rasterize path. The
value isn't a new renderer; it's **computing the labels/totals/arrows precisely and placing them on a
clean grid.**

### Recipe sketch: Mekko / Marimekko
Column width ∝ column total (scale the row of totals to the chart width, no gaps between columns);
within each column, segment height ∝ share (percent axis) or value (unit axis), so each segment's AREA
∝ its absolute value. Compute x-offsets cumulatively; draw one pptx rect per segment (brand ramp by
series, consistent across columns); label segments (value / % / both), and put column-total + the
width-category name above/below each column. Roll small segments into an "Other" bucket. Use `d3-scale`
only for the arithmetic — the marks are plain editable rects (Tier B).

### Recipe: Harvey-ball scorecard (proven)
The classic consulting "scenario/option × criteria" rating matrix. Harvey balls render most reliably as
tiny **SVG wedges rasterized with sharp** (pptxgenjs's pie/arc shape is finicky), reused across cells.

Generator — white disc + green wedge (clockwise from 12 o'clock) + gray ring; special-case 0 and full:
```js
async function harvey(frac) {            // frac in {0,.25,.5,.75,1}
  const r=46, c=50, fill="#15875C";
  let wedge="";
  if (frac>=1) wedge=`<circle cx="${c}" cy="${c}" r="${r}" fill="${fill}"/>`;
  else if (frac>0){ const t=frac*2*Math.PI, x=c+r*Math.sin(t), y=c-r*Math.cos(t), big=frac>.5?1:0;
    wedge=`<path d="M${c},${c} L${c},${c-r} A${r},${r} 0 ${big} 1 ${x.toFixed(2)},${y.toFixed(2)} Z" fill="${fill}"/>`; }
  const svg=`<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100">`
    +`<circle cx="${c}" cy="${c}" r="${r}" fill="#fff"/>${wedge}`
    +`<circle cx="${c}" cy="${c}" r="${r}" fill="none" stroke="#C7CDD1" stroke-width="5"/></svg>`;
  return "image/png;base64,"+(await sharp(Buffer.from(svg)).png().toBuffer()).toString("base64");
}
```
Pre-render the five fills once (`balls[0..4]`) and `addImage` them at ~0.42" in each cell.

Matrix layout: rows = options/scenarios, columns = criteria; left cell holds the option name + a
one-line descriptor; a Harvey ball per criterion cell; an **Overall** column showing the summed score
as a small bar + number (this is the think-cell "compute it" move — the title's claim becomes provable
straight off the matrix). Highlight the recommended row (light brand tint + brand left-bar +
"RECOMMENDED" tag). Add a fit legend (empty→full = weak→strong) and footnote the ratings as a
qualitative assessment. **Canonical reference:** the SAP three-scenarios slide (Full SAP / Hybrid /
Datasphere→Azure scored on compliance, team fit, cost, RISE, speed → 12 / 18 / 15 of 20).

### Recipe sketch: series connectors
For stacked columns, draw a thin (often dashed) line from the top of segment k in column i to the top
of segment k in column i+1 — connectors make each series' growth visible across columns (a hallmark of
the consulting look). Compute endpoints from the same scale as the bars.

---

## Recipe: Architecture / system diagram (real icons, not boxes)

System & cloud architectures must use **real product/service icons grouped into labeled zones with clean
connectors** — the Azure Architecture Center / AWS-diagram look — **never plain labeled boxes**. Treat
this as its own visual type.

**Icon source — `@iconify/json` (npm, `npm i @iconify/json`).** Read sets from
`node_modules/@iconify/json/json/<set>.json`; each icon has a `body` + `width/height`. Useful sets:
- `logos` — full-color brand logos: `microsoft-azure`, `microsoft-power-bi`, `apache-spark`, `sap`, …
- `devicon` — real cloud SERVICE icons: `azuredatafactory`, `azuresqldatabase`, `microsoftsqlserver`, …
- `simple-icons` / `fa6-solid` — monochrome brand + generic glyphs (`databricks`, `plug`, `right-left`,
  `layer-group`); tint by replacing `currentColor` with a hex before rasterizing.

Helper: wrap `body` in an `<svg viewBox="0 0 W H">`, rasterize with sharp (`resize({fit:'contain',
background:transparent})`), return `{png, aspect}`. Place each icon at a fixed display height with
width = height × aspect so nothing distorts; put a short label beneath.

**Layout:** group nodes into 2–4 **labeled zone containers** (light rounded rects with a header), each
holding 1–3 icon nodes; connect zones/nodes with thin arrows. Use **semantic color**: tint a
zone/border/connector to mark the focal or broken part (e.g. red for a blocked path) and to color-code
by platform (e.g. SAP vs Azure). One emphasis; the rest calm. Labels stay short — the icons carry
recognition.

**Licensing:** brand/trademark icons are used nominatively to identify the real services. For
pixel-exact official Azure/AWS/GCP service glyphs, drop the vendor's official icon SVGs into the skill's
assets and point the helper at them; vendor icon sets carry their own usage terms.

**Canonical reference:** the SAP "original architecture" slide — SAP Source → Extraction → Azure Ingest
→ Consume zones with real ADF / Azure SQL / Power BI / Databricks / SAP icons and a red-flagged
extraction path.
