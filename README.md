# data-storytelling-slides
 
A reusable **Agent Skill** for generating consulting-grade, data-storytelling PowerPoint decks in an
editorial "insight-driven" visual style — bold serif action-title headlines, a single-hue color
system, minimal chrome, and charts built to carry the argument (polished executive data stories).
 
The skill is **palette-agnostic** (it derives a theme from any brand color) and ships with a built-in
fallback theme ("Editorial Mint"). Given a topic, it researches the subject, designs a storyline,
writes a detailed editable prompt spec for **every** slide, and then builds the deck **one slide at a
time, keeping you in the loop** — never as a single batch.
 
## What's inside
 
```
data-storytelling-slides/
├── SKILL.md                      # The skill: trigger description + workflow + design rules
└── references/
    ├── design-system.md          # Narrative architecture, color logic, pattern library,
    │                             #   layout principles, chart craft
    ├── advanced-visuals.md       # HOW to render beyond native charts: the SVG→rasterize→native-overlay
    │                             #   technique; recipes for maps/choropleth, Sankey, treemap, chord,
    │                             #   architecture diagrams (real product icons), Harvey balls, Mekko;
    │                             #   the D3-gallery chart catalog by intent
    └── themes.md                 # Fallback "Editorial Mint" theme + palette-agnostic derivation recipe
```
 
`SKILL.md` is the entry point. The files in `references/` are loaded on demand for deeper detail.
 
## How it works (the workflow)
 
1. **Frame** — topic, audience, goal; gather data (use provided data, or research the web for real figures).
2. **Storyline** — pick a narrative arc, break it into beats, write each as an *action title*
   (a quantified, single-sentence takeaway); QA with the so-what + horizontal/vertical-logic tests.
3. **Per-slide prompt specs** — turn the storyline into a detailed, editable spec for **every** slide
   (action title · hero visual · the real figures on it · callouts · insight rail · emphasis). Present
   the **whole story up front** and get it approved before anything is built. Principle:
   **data → visualize it; no data → structure the text** (MECE buckets, chevron/ghost-box flows, 2×2 /
   driver trees, framework blocks) — never loose bullets or plain boxes.
4. **Theme** — derive a palette from the brand color, or use the Editorial Mint fallback.
5. **Build one slide at a time (human in the loop)** — show that slide's prompt → apply edits → build
   and render just that slide → iterate to approval → move on. Pairs with a PowerPoint-rendering
   capability (e.g. `pptxgenjs`).
6. **QA** — visual + narrative checks: every page speaks for itself, one hero visual per slide, text
   budget, emphasis follows narrative, grayscale-safe, and **aspect ratios preserved** (icons/logos/
   images never stretched).
   - **6b. Edit one slide at a time** — later feedback edits that slide's spec and re-renders only it,
     while preserving deck-wide consistency.
## Visual range
 
- **Native charts** (editable): bar, line, area, pie/doughnut, scatter.
- **Shape-built consulting charts** (editable): waterfall/bridge, Mekko/Marimekko, Harvey-ball
  scorecards, Gantt timelines, football-field ranges, tornado/diverging bars, 2×2 maps, pictographs.
- **Rasterized data-viz** (SVG → image, with native text overlays): choropleth maps, Sankey, treemap,
  chord — built with the D3 ecosystem.
- **Architecture diagrams** with **real product/service icons** (via `@iconify/json`) grouped into
  labeled zones — the Azure-Architecture-Center look, not plain boxes.
## Using it across AI tools
 
This is a portable **Agent Skill** (a `SKILL.md` plus reference docs). It works anywhere that supports
the Agent Skills format:
 
- **Claude (Claude.ai / Claude Code / Cowork / API):** place this folder in your skills directory so
  it appears in `available_skills`. It triggers automatically when you ask for an analytical/data deck,
  or invoke it by name: *"Use the data-storytelling-slides skill to make a deck on X."*
- **Other agent tools:** point the tool at `SKILL.md` as a system/context document, or paste its
  contents as instructions. The references can be supplied when more detail is needed.
### Companion capabilities
- A **PowerPoint rendering** capability (e.g. a `pptx` skill / `pptxgenjs`) is needed to produce the
  actual `.pptx`. `SKILL.md` defers all rendering mechanics to it.
- Node libraries used by the advanced visuals: `pptxgenjs`, `sharp`, `react-icons`, `@iconify/json`,
  and the relevant `d3-*` modules (installed on demand).
- If you maintain a separate corporate slide style, this skill can borrow its style-neutral component
  specs (card geometry, table spec, spacing); the visual identity here stays its own.
## Changelog
 
- **v4** — per-slide prompt specs · whole-story-first, slide-by-slide human-in-the-loop build ·
  advanced-visuals reference (maps, Sankey, treemap, chord, architecture icons, Harvey balls, Mekko) ·
  D3-gallery chart catalog · data→visualize / no-data→structure principle · aspect-ratio rule.
## Status
 
Living document. The core slide archetypes and the cross-cutting systems (color, typography, narrative,
chart craft, layout) are captured, along with worked recipes for choropleth maps, Harvey-ball
scorecards, and icon-based architecture diagrams. Thin spots to graduate to full worked examples over
time: Mekko/Marimekko and Sankey on real slides.
 
## License
 
This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.
