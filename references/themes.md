# Themes — Fallback Theme + Palette-Agnostic Derivation

This skill is **palette-agnostic**: it assigns colors to ROLES, then fills those roles from
whatever palette applies. Use the fallback theme unless the user gives a brand color/palette.

For the full reasoning behind these roles, read `design-system.md` → "Color System Logic".

---

## The 6 color roles (every theme fills these)

| Role | Purpose |
|------|---------|
| `canvas` | Slide background. Maximally neutral. |
| `ink` | All primary text. Max contrast on canvas. |
| `brand` | The single accent hue = identity. ONE hue. |
| `ramp[1..5]` | Sequential tints/shades of `brand`, light→dark. Encodes magnitude / ordered categories. |
| `muted` | Desaturated gray. "No data", de-emphasized, secondary text, outline icons. |
| `emphasis` | Darkest/most-saturated ramp step. Used ONCE, on the single most important element. |

Plus optional **semantic** colors (only when color carries shared meaning — see below).

---

## FALLBACK THEME — "Editorial Mint" (default)

The default house style. Use when no palette is specified.

```
canvas      #FFFFFF   pure white
ink         #1A1A1A   near-black — headlines + body
muted-text  #5B6670   captions, axis labels, secondary text
muted-bg    #F4F5F6   light insight strips / panels (low-saturation, survives B&W)
muted-fill  #D9DCDF   "no data", outline-icon remainder, gridlines

brand       #2EC98B   vivid mint (ramp step 3)

ramp-1      #D8F5E8   lightest  (low values)
ramp-2      #8DE0BC
ramp-3      #2EC98B   = brand
ramp-4      #15875C
ramp-5      #0C4A37   darkest = EMPHASIS (takeaway banner, hero element)

semantic-good     #2EC98B   (reuse brand mint)
semantic-warn     #F4C24B   amber
semantic-bad      #E2503B   red
semantic-water    #2E9BD6   blue  (e.g. hydro)
semantic-sun      #F2A33C   orange (e.g. solar)
semantic-coal     #1A1A1A   black
```

### Typography (fallback)
```
headline   Georgia, BOLD, 28–32pt, ink        ← serif display (the signature move)
section    Arial, BOLD, 16–18pt, ink           ← "Key Insights", "PHASES & TASKS"
chart-title Arial, BOLD, 14–16pt, ink
chart-sub  Arial, 11–12pt, muted-text
body       Arial, 12–14pt, ink/muted-text
axis/label Arial, 10–11pt, muted-text
```
- The serif headline + sans body pairing is what makes it read "editorial," not generic corporate.
- Georgia and Arial are near-universally available, so the deck renders reliably. If nicer faces are
  installed/embeddable, upgrade: headline → a display serif (Tiempos / Fraunces / Source Serif),
  body → Inter. VERIFY availability before using non-standard fonts (see pptx skill).
- HARD RULE (from blueprints video): ONE body font size per slide; only the headline may be larger.

### Layout defaults
- 16:9 (13.33" × 7.5"), outer margin ~0.6–0.8"
- Action title top, left-aligned, NO accent bar / label / underline above it
- Insights panel LEFT, chart/visual RIGHT (the tool's answer-first convention)
- White canvas (pure `#FFFFFF`, not an off-white)

---

## Deriving a theme from ANY brand hue (palette-agnostic recipe)

Given a user's brand color (or a topic-appropriate hue), build the roles mechanically:

1. `canvas` = white (or the palette's lightest neutral)
2. `ink` = near-black (or the palette's darkest neutral)
3. `brand` = the user's primary hue
4. `ramp[1..5]` = 5 steps of `brand` from ~92% lightness down to ~22% lightness, **hue held constant,
   vary lightness only**. (Keep adjacent steps clearly distinguishable; cap at 5.)
5. `muted` = pull `brand` saturation toward 0, lightness ~88% (gray), plus a darker gray for text
6. `emphasis` = `ramp-5` (darkest, most saturated)

For data that is NOT simple magnitude:
- **Diverging** (above/below a midpoint, +/−): brand hue on one side, a complementary or palette
  secondary hue on the other, meeting at a light neutral middle.
- **Unordered categories**: distinct hues of *similar lightness* so none dominates. Prefer staying
  single-hue (lightness steps) if ≤5 categories.

### When to break single-hue (semantic exception)
Only when the color itself carries meaning the audience already shares:
- Good/bad → traffic-light red/amber/green (or palette good/bad colors)
- Category-with-a-known-color → water=blue, sun=orange/yellow, coal=black, eco=green
Never introduce extra hues for decoration. Default everything else to brand + muted.

### Accessibility guardrails (always)
- Near-black text on white; white/lightest text on emphasis/dark fills
- Don't rely on hue alone — pair with lightness so encodings survive grayscale + color-blindness
- ≤5 sequential steps; adjacent steps must be distinguishable

---

## Theme selection logic (what the skill does)
1. User specified a brand color/palette? → derive roles via the recipe above.
2. Topic has an obvious associated color and user is open? → may pick a topic hue (still ONE hue).
3. Otherwise → use **Editorial Mint** fallback.
State the chosen theme briefly to the user; keep it consistent across every slide in the deck.
