# PROCESS_LOG.md

> All artifacts referenced here are AI-generated. See README disclosure.

Running log of every attempt, written as the work happened rather than
reconstructed afterward.

**Source script:** [`../script/SCRIPT.md`](../script/SCRIPT.md)

---

## Attempt 1: ElevenLabs (web app, free plan) — Eleven Multilingual v2

**Category:** Voice only. No visual component.

**Free-tier constraints in effect:**
- 10,000 characters/month. The long script was ~1,750 characters, so no
  quota limit was hit and substantial headroom remained.
- No commercial usage rights on Free; attribution required.
- **Voice cloning unavailable on Free** — it requires Starter ($5/mo) or
  above. This attempt therefore uses a stock voice. Noted because it
  means the one safeguard most relevant to deepfake misuse (consent for
  cloning) was never tested: the tier that gates it is also the tier that
  sells it.

**Date:** 2026-09-15
**Time spent:** ~10 minutes, start to downloaded file. No retries — the
first generation was usable.

### Settings

| Setting | Value |
|---|---|
| Voice | Roger — "Laid-Back, Casual, Resonant" (stock) |
| Model | Eleven Multilingual v2 |
| Speed | Slightly above center |
| Stability | ~35–40% (toward "more variable") |
| Similarity | High, ~85% |
| Style Exaggeration | None (far left) |
| Panning | Centered |

**Voice selection rationale:** the script is written in a coach's voice
addressing his team. A laid-back, resonant register fits that framing
better than a newsreader voice would. The goal was synthesis in a
*plausible* speaking context — a voice/content mismatch would have made
the "would this fool anyone" question turn on casting rather than on the
synthesis.

**Script used:** full ~300-word version, verbatim, with the em dash and
line-wrap normalization described in SCRIPT.md.

### What happened

Generated cleanly first pass. No errors, no retries, no truncation, no
content warnings, no refusals — notable given the script names a real
athlete and discusses a real team's season.

**Output:** `artifacts/AI_GENERATED_attempt1_elevenlabs.mp3` (1:35)

### First impressions

Better than expected, and the surprise was *where*. The going-in
hypothesis was that the script's density of spoken figures ("sixteen and
six", "just over seventeen goals", "under thirteen") would be the weak
point — numeric flattening is a well-documented TTS failure mode. It
wasn't. "Sixteen and six" came out as a single semantic unit with the
stress pattern of a sporting record, not two independently-read numerals,
with no SSML markup or hinting. Audible breath intake is present between
clauses rather than absent.

The failures that did surface sit in emphasis and emotional register
rather than in pronunciation or pacing. Detailed in EVALUATION.md.

---

## Attempt 1b: ElevenLabs re-cut — forced by a downstream constraint

**Why this attempt exists:** HeyGen's free plan caps output at **1
minute**. The Attempt 1 audio was 1:35 and was rejected at HeyGen's
generate step with a message that the video exceeded the free plan limit
and to upgrade or shorten the script. Rather than pay, the script was cut
to 118 words and re-synthesized.

**Settings:** identical to Attempt 1 — same voice, same model, same
slider positions. Held constant deliberately so the shorter audio stays
comparable rather than introducing a second variable.

**Date:** 2026-09-15
**Time spent:** ~5 minutes

**Output:** `artifacts/AI_GENERATED_attempt1b_elevenlabs_short.mp3` (0:38)

**Measured rate:** 118 words in 38 seconds ≈ 186 wpm, faster than
conversational norm despite a near-default speed setting.

**Finding:** the constraint that shaped this artifact was not a
limitation of synthesis *quality* but of a downstream tool's **business
model**. The audio engine would have produced two minutes without
complaint. Pipeline composition, not model capability, set the ceiling.

---

## Attempt 2: HeyGen (web app, free plan) — Avatar IV motion engine

**Category:** Audio-plus-avatar → lip-synced video.

**Free-tier constraints in effect:**
- 60-second maximum output (hit; forced Attempt 1b)
- 720p export ceiling; 1080p and 4K paid
- HeyGen watermark non-removable
- 3 videos/month; this consumed one
- **The rendered file could not be downloaded at all.** The download
  dialog resolved to an upgrade wall (Creator $29/mo, Pro $49/mo) with
  the visible "Download" panel functioning as promotional artwork rather
  than a working control, alongside "29 people just upgraded in the last
  20 minutes" as social pressure. The artifact was captured by **screen
  recording** instead — which conveniently doubles as the provenance
  survival test the assignment asks for.

**Dates:** video generated 2026-09-15, 8:14 PM. Screen capture made
2026-09-16, ~12:12 AM.

**Time spent:** ~35 minutes, including the dead end at the length cap,
the re-cut, the re-upload, the render, and working out that the download
was paywalled.

### Settings

| Setting | Value |
|---|---|
| Avatar | Vernon, look "Vernon Lounge Side 2" (public/stock) |
| Motion Engine | Avatar IV |
| Voice | **Overridden** — uploaded ElevenLabs MP3, not HeyGen's "Vernon - Lifelike" |
| Avatar Background | Color (scene retained as shot) |
| Layout | Original |
| Radius / Zoom | 0 px / default |
| Aspect | Landscape 16:9 |
| Project title | `AI_GENERATED_attempt2_heygen` |

**Avatar selection rationale:** "Vernon Lounge Side 2" was chosen over
the four available Office looks because a subject in blazer and tie
delivering locker-room content would have made the plausibility question
turn on costuming rather than on the synthesis. Of the eight looks
offered, this was the only standing, casually-dressed, mid-gesture
option.

**Why the audio override matters methodologically:** holding the audio
constant from Attempt 1b means the visual layer is the only new
variable. Any artifact observed in Attempt 2 is attributable to the
lip-sync and animation model rather than to a different voice engine.

### What happened

1. First generate attempt **rejected** — exceeded the 60-second cap.
2. Audio re-cut (Attempt 1b), re-uploaded.
3. Second generate attempt succeeded → 38-second video.
4. Download **blocked** by upgrade interstitial. Captured via macOS
   ReplayKit screen recording: 60 fps, 3420×2214, 56 seconds total
   including ~18 seconds of post-video end screen.

No content filters fired. No consent gate encountered — notably, HeyGen
presents "Clone a real person" from video footage as a standard option in
its onboarding flow, with voice cloning as step 2 of account setup, and
no verification step was observed at any point.

**Output:** `artifacts/AI_GENERATED_attempt2_heygen_screencapture.mov`

Derived files prepared for detector upload (progressively compressed to
meet free-demo file limits):
- `AI_GENERATED_attempt2_heygen_28s_for_detector.mp4` (28s, 2.0 MB)
- `detector_15s_854p.mp4` (15s, 429 KB)
- `detector_8s_640p_noaudio.mp4` (8s, 69 KB)
- `detector_single_frame_face.jpg` (768×768, 24 KB)

**Note on generational loss:** these derived files are third-generation
copies — HeyGen render → screen capture → re-encode and crop. Each step
degrades the signal a detector would rely on. This is relevant to
interpreting the detection results in `detection/DETECTION.md`.

### First impressions

The still frames are convincing; the moving sequence is not, for a
specific reason rather than a general one. Skin texture, lighting, hand
position and the room all hold up under inspection. What does not hold up
is the relationship between the face and the content of the speech.

Also immediately apparent: the exported watermark is **far more
aggressive than the editor preview implied.** The preview showed a single
corner logo; the export is a tiled grid of "HeyGen" marks across the
entire frame, overlapping the subject's face and body. Evidence:
`evidence/watermark_tiling_full_frame.png`. A user judging the free
tier's usability from the editor view would have reached a different
conclusion than the export supports.

---

## Attempted but unavailable

**Deepware Scanner** — unreachable during the testing window
(2026-09-16). Recorded here rather than silently omitted. Detection was
carried out with Hive's AI-Generated Content Detection demo instead; see
`detection/DETECTION.md`.

---

## Time-cost summary

| Attempt | Tool | Wall-clock | Free-tier cost | Outcome |
|---|---|---|---|---|
| 1 | ElevenLabs, Multilingual v2 | ~10 min | ~1,750 / 10,000 chars | Usable first pass, 1:35 |
| 1b | ElevenLabs, same settings | ~5 min | ~700 chars | 0:38 re-cut, forced by HeyGen cap |
| 2 | HeyGen, Avatar IV | ~35 min | 1 of 3 monthly videos; download paywalled | Rendered; capturable only by screen recording |

**Cost-in-hours observation:** the audio tool was dramatically cheaper in
effort — ten minutes to a usable result, no retries. The video tool
consumed roughly three and a half times that, and most of it was not
creative iteration but discovering and routing around free-tier
restrictions: first the length cap, then the download paywall. The
*generation* was fast. The *friction* was slow. Anyone estimating how
long it takes to produce a convincing synthetic video should budget for
the second, not the first.

## What I'd do differently starting over

Establish the **downstream** tool's constraints before generating the
upstream asset. Knowing about HeyGen's 60-second cap first would have
made the initial ElevenLabs render 38 seconds and saved an entire
generation cycle. Generalizing: in a multi-tool pipeline, work backward
from the tightest constraint in the chain.
