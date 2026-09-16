# DETECTION.md

> All artifacts tested here are AI-generated. See README disclosure.

Detection and provenance checks. Both hits and misses are recorded — the
misses turned out to be the more informative results.

---

## Check A: Provenance and watermark survival — COMPLETE

Performed on `artifacts/AI_GENERATED_attempt2_heygen_screencapture.mov`
using `ffprobe` for container metadata and a raw string scan for C2PA
structures.

### What HeyGen embedded

**Visible watermark: yes, and aggressively.** The export carries a
*tiled grid* of "HeyGen" marks across the entire frame, overlapping the
subject's face and body — not the single corner logo shown in the editor
preview.
→ `evidence/watermark_tiling_full_frame.png`

The preview/export discrepancy matters on its own: a user evaluating the
free tier from the editor view would reasonably conclude the marking was
unobtrusive.

### Survival test: screen recording

Because the free tier paywalled the download, the only available capture
method was a macOS ReplayKit screen recording — making this test
unavoidable rather than optional.

| Marker | Survived? |
|---|---|
| Visible tiled watermark | **Yes** — burned into pixels, fully intact |
| C2PA manifest (JUMBF box) | **No** — absent |
| C2PA claim signature | **No** — absent |
| Any `heygen` identifier in metadata | **No** — absent |
| Any `elevenlabs` identifier in metadata | **No** — absent |

Container metadata after capture consists solely of:

```
major_brand=qt
creation_time=2026-09-16T04:12:30Z
com.apple.quicktime.author=ReplayKitRecording
com.apple.quicktime.full-frame-rate-playback-intent=1
```

A raw string scan returned a single incidental `C2Pa` byte sequence with
no accompanying JUMBF container, claim signature, or manifest store —
a coincidence in compressed video data, not provenance.

**The screen recording is, to any metadata-based tool, an original Apple
screen capture.** Every machine-readable trace of both generating tools
was destroyed by a single, trivial, universally available
transformation. No specialist software, no stripping utility, no intent
to evade — just pressing record.

---

## Check B: Hive AI-Generated Content Detection — COMPLETE

**Tool:** Hive Moderation, "AI-Generated Content Detection" public demo
(`thehive.ai/demos/ai-generated-content-detection`). Free, no login.
**Date tested:** 2026-09-16

### B1 — Video submission: demo crashed

Uploading `detector_15s_854p.mp4` (15s, 854px, 429 KB) produced a
successful upload and playable preview, but the analysis panel returned
a frontend error rather than a result:

```
Cannot read properties of undefined (reading 'input')
```

with "No chart data" in the per-frame timeline below. This is a
JavaScript failure in the demo itself, not a file rejection — the video
loaded and played normally. Repeated with progressively smaller files
(down to 69 KB) with the same outcome.

**Recorded as: video analysis unavailable due to tool failure.**

### B2 — Image submission: FALSE NEGATIVE

Submitted `detector_single_frame_face.jpg` — a 768×768 crop of a single
frame at t≈12s, 24 KB.

| Field | Result |
|---|---|
| Verdict banner | **"This input is not likely to contain AI-generated or deepfake content"** |
| Likely to be AI-Generated Image | **2.9%** |
| Likely to be Deepfake | **0%** |
| Top generation source guess | cogvideos, 0.8% |
| Explained itself? | No — probability scores only, no reasoning or localization |

**This is a complete miss.** The submitted image is a frame from a
wholly synthetic video: a stock avatar, animated by a commercial
lip-sync model, driven by synthesized speech. Ground truth is 100%
machine-generated. The detector returned 2.9% and an explicit negative
verdict.

**And the HeyGen watermark is visible in the submitted image.** The
"HeyGen" mark is legible in the lower portion of the crop. The strongest
available signal that this content was machine-generated — the
generating company's own branding, present in the pixels — sat in the
frame while the model concluded the image was probably not
AI-generated. The detector is evidently analyzing generative
*artifacts* (diffusion texture, frequency signatures) and not reasoning
about content at all.

**Why the miss is plausible, and why that is the point.** This artifact
is not what deepfake detectors are built for. It is not a face swap and
not a diffusion image. It is a *real photographic recording of a real
human actor*, re-animated — so the pixels are largely genuine camera
output, and the manipulation lives in the temporal domain, in how the
mouth and head move over time. A still frame contains almost none of
the evidence. The 0% deepfake score is arguably *correct* under a narrow
definition of deepfake as identity substitution.

That is precisely the gap worth documenting. The avatar pipeline
produces content that is fully synthetic in provenance and intent while
being nearly free of the artifacts detectors search for. It defeats
detection not by being sophisticated but by being a *different category
of thing* than the detectors were trained on. And this is the
easy-to-reach, free-tier, no-credit-card path.

### B3 — The Content Credentials result, which is the most significant finding here

The same result panel reported, under Content Credentials:

```
Tool or device used:  Anthropic Files
Action taken:         Opened
```

**Read that against what the file actually is.** The chain of custody
for this image was:

1. **ElevenLabs** synthesized the speech
2. **HeyGen** generated the video
3. macOS **ReplayKit** screen-recorded it
4. **ffmpeg**, running in Claude's analysis environment, extracted and
   cropped the frame

The only cryptographic provenance credential attached to the file names
step 4 — the tool that *resized* it. Neither tool that *fabricated* it
left any signed trace at all.

So the credential is accurate and useless simultaneously. It faithfully
records the last cooperating tool to touch the file, and tells a viewer
nothing whatsoever about the file being synthetic. An investigator
trusting Content Credentials on this image would conclude it passed
through Anthropic's tooling, which is true, and would learn nothing
about ElevenLabs or HeyGen, which is what matters.

**This is a concrete demonstration of the structural problem with C2PA
as a deepfake defense.** The standard is opt-in and last-writer-wins: it
records the provenance of tools that choose to sign, and a
non-participating generator is indistinguishable from a camera. The
generating tools here were silent; an incidental post-processor was not.
The result is a provenance trail that points confidently at the wrong
link in the chain.

### Audio submission

> **Not yet run.** The demo accepts MP3. Submitting
> `AI_GENERATED_attempt1b_elevenlabs_short.mp3` would be the cleaner
> test — no watermark, and audio synthesis leaves artifacts in a domain
> detectors are better tuned for. Prediction, to be tested: audio is
> flagged where video and stills were not, because the ElevenLabs
> output is fully generated waveform rather than re-animated real
> footage.

| Field | Value |
|---|---|
| Verdict | |
| Confidence | |
| Explained? | |

---

## Check C: Deepware Scanner — ATTEMPTED, UNAVAILABLE

`deepware.ai` was unreachable during the testing window (2026-09-16).
Recorded rather than silently omitted.

Independently of the outage: Deepware is video-only and analyzes face
manipulation while explicitly ignoring voice, and a 2026 review
describes it as stale relative to current generators. Given the B2
result, it would very likely have returned the same false negative for
the same reason — this artifact is not a face swap.

---

## Check D: LLM transcript evaluation — OPTIONAL, NOT RUN

| Field | Value |
|---|---|
| Model + version | |
| Exact prompt | |
| Response | |
| What it claimed tipped it off | |
| Correct? | |

**Note on interpreting this check if run:** the script was *written* by a
human–LLM collaboration describing real analysis, and the *audio* was
synthesized. "Does this text read as AI-written" and "was this recording
synthesized" are different questions. A model conflating them produces a
right answer for a wrong reason and should be recorded as such.

---

## Overall takeaway

Three detection and provenance avenues were attempted. The results, in
order of how much they should worry anyone:

**The public detector returned a false negative on fully synthetic
content** — 2.9% AI-generated, 0% deepfake, explicit negative verdict —
with the generating company's watermark visible in the submitted frame.
It offered a probability with no reasoning, so a user had no way to
assess the verdict's basis. The avatar pipeline evades detection not
through sophistication but by producing a category of artifact the
detectors are not built for: real camera footage, re-animated, with the
manipulation living in motion rather than in pixels.

**The cryptographic provenance standard attributed the file to the wrong
tool.** C2PA credentials named the environment that cropped the frame,
while both tools that generated the content left nothing. Being opt-in
and last-writer-wins, the standard cannot distinguish a non-signing
generator from a camera — and here it produced a confident trail
pointing at an irrelevant link in the chain.

**The only marker that worked is the one that exists for commercial
reasons and damages the content.** The tiled watermark survived screen
recording, re-encoding and cropping, because it is burned into the
pixels. It survived, in other words, for exactly the reason it is
*unacceptable* to a paying user — and it is removable for $29/month. The
population most likely to produce a convincing deepfake is precisely the
population whose output carries no marking at all. The safety property
is a side effect of the pricing page.

**And one tool simply didn't work.** Hive's video analysis crashed on
every file submitted.

Set against two generation tools that worked correctly on the first
attempt, in under an hour, for free: generation is productized,
reliable, and cheap. Detection is a broken demo page, a false negative,
and a provenance standard pointing at the wrong tool. The asymmetry is
not close.
