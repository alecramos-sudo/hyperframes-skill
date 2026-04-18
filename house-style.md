# House Style

Creative direction for compositions when no `visual-style.md` is provided. These are starting points — override anything that doesn't serve the content.

## Producer Defaults

These are established preferences. Apply them on every project unless the brief explicitly overrides.

### Pacing: high energy

- Scene durations: **1.5–2.5s** per content scene — keep it moving, don't linger
- Hook scenes with rapid visual cycling: **0.10–0.16s per frame**
- If a scene has delivered its message, cut

### Transitions: clean cuts

- **Default transition:** simple opacity fade, 0.25–0.35s, `ease: linear` or `power2.inOut`
- **Never use blur + scale on transitions.** `filter: blur()` combined with scale on scene in/out is banned — looks muddy and cheap
- Hard cuts are acceptable and preferred for high-energy sequences
- **SFX at every scene transition**, not just the hook — soft page-turn/whoosh on each cut

### VO: locked to on-screen copy

- One VO clip per scene, starts **0.3–0.5s after the scene appears**
- VO script **matches on-screen text exactly** — no paraphrasing, no summarizing
- Generate with ElevenLabs (see [references/tts.md](references/tts.md))
- Split into separate files per scene (e.g. `vo-1.mp3`, `vo-2.mp3`) — never one long clip

### Audio layers

| Layer          | Track | Volume      | When                                          |
| -------------- | ----- | ----------- | --------------------------------------------- |
| VO             | 2     | `1.0`       | Always; one non-overlapping clip per scene    |
| Hook SFX       | 3     | `0.70–0.80` | Rapid visual sequences (hook scene)           |
| Music bed      | 4     | `0.25–0.30` | Always; lower when VO is present              |
| Transition SFX | 5     | `0.50–0.60` | Every scene cut — subtle accent               |

Generate all SFX with ElevenLabs sound-generation API. Generate music with Suno. See [references/music.md](references/music.md).

### Hook sequences (color/option cycling)

Opening hooks that rapid-cycle through product options or combinations:

- **Seeded PRNG (mulberry32)** for randomized order — never sequential, never `Math.random()`
- **Block consecutive same-background:** after shuffle, swap any adjacent pairs with matching bg
- **Bright/spring palette first** — drop dark and muddy colors (black, forest green, burgundy, clay) unless specifically requested; favor blush, cream, lavender, yellow, white, navy
- **Parameters:** 18–36 combos, 0.10–0.16s per frame, ~2.5–4s total
- **No freeze at end:** set `T1 = hookCombos.length * INTERVAL` exactly — zero gap between last combo and scene transition

```js
var INTERVAL = 0.16;  // seconds per combo
var T1 = hookCombos.length * INTERVAL;  // transition fires right after last combo
```

### Scene structure (social media ads)

Preferred arc for short-form ads:

1. **Hook** — rapid visual cycling of options/variety (2–4s)
2. **Step scenes** — one product dimension per scene (color → design → personalization)
3. **Product** — photo or video; Ken Burns zoom if still image (`scale: 1.06 → 1.0` over 2s)
4. **CTA** — brand + tagline + price + button + fine print (staggered entrance, 3–4s hold)

Keep it to **5–6 scenes** for social-length video (12–15s). Longer arcs need explicit justification.

---

## Before Writing HTML

1. **Interpret the prompt.** Generate real content. A recipe lists real ingredients. A HUD has real readouts.
2. **Pick a palette.** Light or dark? Declare bg, fg, accent before writing code.
3. **Pick typefaces.** Run the font discovery script in [references/typography.md](references/typography.md) — or pick a font you already know that fits the theme. The script broadens your options; it's not the only source.

## Lazy Defaults to Question

These patterns are AI design tells — the first thing every LLM reaches for. If you're about to use one, pause and ask: is this a deliberate choice for THIS content, or am I defaulting?

- Gradient text (`background-clip: text` + gradient)
- Left-edge accent stripes on cards/callouts
- Cyan-on-dark / purple-to-blue gradients / neon accents
- Pure `#000` or `#fff` (tint toward your accent hue instead)
- Identical card grids (same-size cards repeated)
- Everything centered with equal weight (lead the eye somewhere)
- Banned fonts (see [references/typography.md](references/typography.md) for full list)

If the content genuinely calls for one of these — centered layout for a solemn closing, cards for a real product UI mockup, a banned font because it's the perfect thematic match — use it. The goal is intentionality, not avoidance.

## Color

- Match light/dark to content: food, wellness, kids → light. Tech, cinema, finance → dark.
- One accent hue. Same background across all scenes.
- Tint neutrals toward your accent (even subtle warmth/coolness beats dead gray).
- **Contrast:** enforced by `hyperframes validate` (WCAG AA). Text must be readable with decoratives removed.
- Declare palette up front. Don't invent colors per-element.

## Background Layer

Every scene needs visual depth — persistent decorative elements that stay visible while content animates in. Without these, scenes feel empty during entrance staggering.

Ideas (mix and match, 2-5 per scene):

- Radial glows (accent-tinted, low opacity, breathing scale)
- Ghost text (theme words at 3-8% opacity, very large, slow drift)
- Accent lines (hairline rules, subtle pulse)
- Grain/noise overlay, geometric shapes, grid patterns
- Thematic decoratives (orbit rings for space, vinyl grooves for music, grid lines for data)

All decoratives should have slow ambient GSAP animation — breathing, drift, pulse. Static decoratives feel dead.

## Motion

See [references/motion-principles.md](references/motion-principles.md) for full rules. Quick: 0.3–0.6s, vary eases, combine transforms on entrances, overlap entries.

## Typography

See [references/typography.md](references/typography.md) for full rules. Quick: 700-900 headlines / 300-400 body, serif + sans (not two sans), 60px+ headlines / 20px+ body.

## Palettes

Declare one background, one foreground, one accent before writing HTML.

| Category          | Use for                                       | File                                                       |
| ----------------- | --------------------------------------------- | ---------------------------------------------------------- |
| Bold / Energetic  | Product launches, social media, announcements | [palettes/bold-energetic.md](palettes/bold-energetic.md)   |
| Warm / Editorial  | Storytelling, documentaries, case studies     | [palettes/warm-editorial.md](palettes/warm-editorial.md)   |
| Dark / Premium    | Tech, finance, luxury, cinematic              | [palettes/dark-premium.md](palettes/dark-premium.md)       |
| Clean / Corporate | Explainers, tutorials, presentations          | [palettes/clean-corporate.md](palettes/clean-corporate.md) |
| Nature / Earth    | Sustainability, outdoor, organic              | [palettes/nature-earth.md](palettes/nature-earth.md)       |
| Neon / Electric   | Gaming, tech, nightlife                       | [palettes/neon-electric.md](palettes/neon-electric.md)     |
| Pastel / Soft     | Fashion, beauty, lifestyle, wellness          | [palettes/pastel-soft.md](palettes/pastel-soft.md)         |
| Jewel / Rich      | Luxury, events, sophisticated                 | [palettes/jewel-rich.md](palettes/jewel-rich.md)           |
| Monochrome        | Dramatic, typography-focused                  | [palettes/monochrome.md](palettes/monochrome.md)           |

Or derive from OKLCH — pick a hue, build bg/fg/accent at different lightnesses, tint everything toward that hue.
