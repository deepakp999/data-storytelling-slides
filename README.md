# data-storytelling

A reusable **Agent Skill** for generating consulting-grade, data-storytelling PowerPoint decks in an
editorial "insight-driven" visual style — bold serif action-title headlines, a single-hue color
system, minimal chrome, and charts built to carry the argument (polished executive data stories).

The skill is **palette-agnostic** (it derives a theme from any brand color) and ships with a built-in
fallback theme ("Editorial Mint"). Given a topic, it researches the subject, designs a storyline,
maps each message to the right chart/slide type, and builds the full deck.

## What's inside

```
data-storytelling-slides/
├── SKILL.md                      # The skill: trigger description + 6-phase workflow + design rules
└── references/
    ├── design-system.md          # Full system: narrative architecture, color logic, pattern library,
    │                             #   layout principles, chart craft
    └── themes.md                 # Fallback "Editorial Mint" theme + palette-agnostic derivation recipe
```

`SKILL.md` is the entry point. The two files in `references/` are loaded on demand for deeper detail.

## How it works (the workflow)

1. **Frame** — topic, audience, goal; gather data (use provided data, or research the web for real figures).
2. **Storyline** — pick a narrative arc, break it into beats, write each as an *action title*
   (a quantified, single-sentence takeaway), QA with the so-what + horizontal/vertical-logic tests.
3. **Map** — match each beat to the slide/chart type that best carries it.
4. **Theme** — derive a palette from the brand color, or use the Editorial Mint fallback.
5. **Build** — render the deck (pairs with a PowerPoint-rendering capability such as `pptxgenjs`).
6. **QA** — visual + narrative checks (every page speaks for itself, emphasis follows narrative, grayscale-safe).

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
- If you maintain a separate corporate slide style, this skill can borrow its style-neutral component
  specs (card geometry, table spec, spacing); the visual identity here stays its own.

## Status

Living document. The core slide archetypes and the cross-cutting systems (color, typography,
narrative, chart craft, layout) are captured. Thin spots to refine over time: 2×2 matrix, L→R process
flow, driver/issue tree, and the cover / section-divider / executive-summary bookends.

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.
