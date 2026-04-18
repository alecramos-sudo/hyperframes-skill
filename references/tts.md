# Text-to-Speech & Sound Effects

## Preferred: ElevenLabs

ElevenLabs is the default for both voiceover (TTS) and custom sound effects (SFX). Use Kokoro only when offline or ElevenLabs is unavailable.

### Credentials

```
API Key:  sk_c1792a8112a8b090a671d1518b01563742142db146c83b6e
Header:   xi-api-key: <key>
```

### Known Voice IDs

| Project / Use        | Voice ID                   |
| -------------------- | -------------------------- |
| Brunch brand VO      | `stPpu7V2TD14ZUJwbPUb`     |

---

## ElevenLabs TTS (Voiceover)

```bash
curl -X POST "https://api.elevenlabs.io/v1/text-to-speech/{voice_id}?output_format=mp3_44100_128" \
  -H "xi-api-key: sk_c1792a8112a8b090a671d1518b01563742142db146c83b6e" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Your script here.",
    "model_id": "eleven_monolingual_v1",
    "voice_settings": { "stability": 0.5, "similarity_boost": 0.75 }
  }' \
  --output vo.mp3
```

Generate one clip per scene — lock each clip to its scene's start time so VO and visuals stay in sync.

### Multi-clip pattern (one script line per scene)

```bash
# Scene 1
curl ... -d '{"text": "Choose your slipper."}' --output vo-1.mp3

# Scene 2
curl ... -d '{"text": "Your design."}' --output vo-2.mp3
```

```html
<audio id="vo-1" data-start="3.5"  data-track-index="2" src="vo-1.mp3" data-volume="1"></audio>
<audio id="vo-2" data-start="5.0"  data-track-index="2" src="vo-2.mp3" data-volume="1"></audio>
```

**Track convention:** VO always on track 2. Space clips so they never overlap (check WAV/MP3 duration).

---

## ElevenLabs Sound Effects (SFX)

Custom SFX via the sound generation endpoint — no pre-recorded libraries needed.

```bash
curl -X POST "https://api.elevenlabs.io/v1/sound-generation" \
  -H "xi-api-key: sk_c1792a8112a8b090a671d1518b01563742142db146c83b6e" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Describe the sound in plain English",
    "duration_seconds": 1.5,
    "prompt_influence": 0.35
  }' \
  --output sfx-name.mp3
```

### Parameters

| Param               | Range    | Notes                                                    |
| ------------------- | -------- | -------------------------------------------------------- |
| `duration_seconds`  | 0.5–22   | Optional. Omit to let ElevenLabs choose natural length.  |
| `prompt_influence`  | 0–1      | 0.3–0.4 = balanced; lower = more creative interpretation |

### Prompt writing tips

Be specific and descriptive. ElevenLabs responds well to:
- Material + action: `"rapid paper swatches being flipped"`
- Qualifier: `"crisp, satisfying, clean"`
- Context: `"like thumbing through a luxury lookbook"`
- Duration feel: `"short burst"`, `"single hit"`, `"sustained"`

### Track conventions

```html
<!-- Hook / ambient SFX → track 3 -->
<audio id="sfx-hook" data-start="0" data-track-index="3" src="sfx-hook.mp3" data-volume="0.75"></audio>

<!-- Scene-transition SFX → track 5 (one per transition, non-overlapping) -->
<audio id="sfx-t1" data-start="2.88"  data-track-index="5" src="sfx-swipe.mp3" data-volume="0.55"></audio>
<audio id="sfx-t2" data-start="4.78"  data-track-index="5" src="sfx-swipe.mp3" data-volume="0.55"></audio>
```

The validator warns "duplicate_audio_track" for non-overlapping clips on the same track — this is a false positive when the clips don't actually overlap in time. Verify durations manually.

### Common SFX prompts

| Use case              | Prompt                                                                        |
| --------------------- | ----------------------------------------------------------------------------- |
| Hook / rapid cycling  | `"rapid fashion swatches flipping in quick succession, crisp paper snaps"`    |
| Scene transition      | `"single soft elegant page turn, clean quiet whoosh, luxury magazine"`        |
| CTA reveal            | `"soft gentle resonant chime, single note, luxury brand reveal"`              |
| Button click          | `"soft satisfying UI tap, minimal, clean"`                                    |
| Card reveal           | `"single card flip, crisp, satisfying"`                                       |

---

## TTS + Captions Workflow

```bash
# Generate VO
curl ... --output vo.mp3

# Transcribe for word-level timestamps
npx hyperframes transcribe vo.mp3  # → transcript.json
```

---

## Fallback: Kokoro TTS (offline only)

Use only when ElevenLabs is unavailable (no internet, key issue).

```bash
npx hyperframes tts "Your script here" --voice af_sky --speed 1.05 --output vo.wav
```

### Voice selection (Kokoro)

| Content       | Voice                 |
| ------------- | --------------------- |
| Marketing     | `af_sky`/`am_michael` |
| Warm/casual   | `af_heart`/`af_nova`  |
| Tutorial      | `am_adam`/`bf_emma`   |

**Requirements:** Python 3.8+ with `kokoro-onnx` and `soundfile` installed.
`pip3 install kokoro-onnx soundfile --break-system-packages`
