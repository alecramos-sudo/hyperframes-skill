# Music Generation

## Preferred: Suno

Suno generates full music tracks from text prompts. Use it for background music beds. The user generates tracks directly in Suno and drops the file into the project directory.

### Workflow

1. Prompt Suno (see below) → download the track (MP3, typically 2–4 min)
2. Drop `music.mp3` in the project root
3. Reference in HTML on track 4, trim to composition duration with `data-duration`

```html
<audio id="music"
  data-start="0"
  data-track-index="4"
  src="music.mp3"
  data-volume="0.28"
  data-duration="14">
</audio>
```

**Track convention:** Music bed always on track 4 at `data-volume="0.25–0.30"` so it sits under VO and SFX without competing.

---

## Suno Prompt Writing

### Structure

```
[Genre/style], [tempo/energy], [instrumentation], [mood/feel], [no lyrics / instrumental]
```

### Tips

- Always add `"no lyrics"` or `"instrumental"` to prevent vocal generation
- Specify BPM or energy: `"upbeat 120bpm"`, `"slow and warm"`, `"building energy"`
- Name references: `"in the style of lo-fi hip-hop"`, `"Parisian café jazz"`
- Add texture descriptors: `"warm Rhodes piano"`, `"soft brushed drums"`, `"airy strings"`

### Example prompts by tone

| Tone               | Prompt                                                                                                |
| ------------------ | ----------------------------------------------------------------------------------------------------- |
| Luxury / fashion   | `"Elegant piano and soft strings, slow 70bpm, warm and sophisticated, light percussion, instrumental"` |
| Spring / lifestyle | `"Bright acoustic guitar, upbeat 100bpm, airy and optimistic, indie pop feel, no lyrics"`             |
| NYC energy         | `"Urban jazz with Rhodes piano, mid-tempo groove, NYC rooftop vibe, light drums, instrumental"`       |
| Minimal / calm     | `"Minimal ambient piano, slow and meditative, soft pads, no melody, instrumental"`                    |
| Product / modern   | `"Modern synth pop, 110bpm, clean and aspirational, light percussion, no vocals"`                     |

### Brunch brand prompt (reference)

> Elegant and warm, soft piano with light acoustic guitar, slow upbeat 85bpm, spring morning feeling, NYC sophisticated, airy and joyful, instrumental, no lyrics

---

## Volume Mixing Guide

| Track         | Index | Volume      | Notes                               |
| ------------- | ----- | ----------- | ----------------------------------- |
| VO            | 2     | `1.0`       | Always full volume                  |
| SFX (hook)    | 3     | `0.7–0.8`   | Punchy but not harsh                |
| Music bed     | 4     | `0.25–0.30` | Background — never competes with VO |
| SFX (trans.)  | 5     | `0.5–0.6`   | Subtle, accent only                 |

When VO is present, keep music at `0.25–0.28`. For music-only sections (hook, visuals only), bump to `0.35–0.45`.
