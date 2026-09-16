# Task_06_Deep_Fake

> ## ⚠️ SYNTHETIC MEDIA DISCLOSURE
>
> **Every audio and video file in `artifacts/` is AI-generated.** None of
> it is a recording of a real person speaking. No real, identifiable
> person is depicted or impersonated: the voice is a stock synthetic
> voice and the on-screen figure is a stock avatar provided by the
> generating tool. All artifact filenames are prefixed
> `AI_GENERATED_`. The frame grids in `evidence/` are extracted from
> synthetic video.

Task 6 of the iSchool research sequence. Takes the written,
ground-truth-backed narrative produced in Task 5 and translates it into
synthetic audio and video using free-tier AI tools, then evaluates
critically where the output holds up, where it breaks, and whether
detection and provenance tools catch it.

The goal was a **documented experiment, not a polished product.**

## Project structure

```
Task_06_Deep_Fake/
├── script/
│   └── SCRIPT.md               <- source narrative, both versions, with rationale
├── artifacts/                  <- all AI_GENERATED_* audio and video
├── logs/
│   └── PROCESS_LOG.md          <- every attempt: tools, versions, settings, failures
├── evidence/                   <- extracted frame grids showing specific failure modes
├── detection/
│   └── DETECTION.md            <- detector results and provenance survival testing
├── EVALUATION.md               <- critical assessment of each artifact + comparison
└── README.md
```

## Source material

The script is adapted from the Phase B advisory answer in
**Task_05_Descriptive_Stats** — the "should I focus on offense or
defense, and which player should I build around" question. Every figure
spoken (16-6 record, 17.19 vs 10.00 goals in wins vs losses, Emma
Tyrrell's team-leading points and game-winning goals) traces back to that
project's `ground_truth.py`, not to anything invented here.

Keeping the content real mattered more than expected. A synthesized voice
delivering an actual analytical argument surfaces problems a voice
reading filler text would not — in particular, the script's density of
spoken figures and its deliberate shifts in emotional register both
turned out to be diagnostic.

Script: [`script/SCRIPT.md`](script/SCRIPT.md)

## Tools used

| Tool | Role | Free tier as encountered (Sept 2026) |
|---|---|---|
| ElevenLabs | Text-to-speech | 10,000 chars/month; no commercial rights; **voice cloning paywalled** (Starter $5/mo+) |
| HeyGen | Avatar video | 3 videos/month; **60-second cap**; 720p; non-removable tiled watermark; **download paywalled** ($29–49/mo) |
| Hive Moderation | AI/deepfake detection | Free public demo, no login; file size and duration limits |
| Deepware Scanner | Deepfake detection | Attempted; site unreachable during testing |
| ffmpeg / ffprobe | Frame extraction, metadata and C2PA inspection | Open source |

## Reproducing this

```
1. Read script/SCRIPT.md — note that the 118-word version is the one
   actually synthesized, and why.
2. ElevenLabs free tier, stock voice "Roger", Eleven Multilingual v2.
   Exact slider values are in logs/PROCESS_LOG.md, Attempt 1.
3. HeyGen free tier, public avatar "Vernon Lounge Side 2", Avatar IV
   motion engine, with the ElevenLabs MP3 uploaded as the audio track
   rather than using HeyGen's own voice. This keeps audio constant so
   the visual layer is the only new variable.
4. Frame analysis: ffmpeg frame extraction at fixed intervals across
   two semantically opposite passages; see EVALUATION.md method note.
5. Provenance: ffprobe for container metadata, raw string scan for C2PA
   JUMBF structures. Commands and output in detection/DETECTION.md.
```

Free tiers change frequently and limits were noticeably tighter than
published summaries suggested, so exact reproduction may not be possible.
Tool versions and dates are recorded throughout so the constraints in
effect at the time are at least legible.

## Artifact naming

```
AI_GENERATED_attempt<N>_<tool>.<ext>
```

## What this experiment found

**1. The expected failure mode wasn't the real one.** Going in, the
assumption was that dense spoken figures would break the TTS — numeric
flattening is a well-documented artifact. It didn't happen. "Sixteen and
six" came out as a single semantic unit with the stress of a sporting
record, and audible breathing was present. The two tells a non-expert
would most likely listen for are the two things the model does well.

**2. What broke instead was performance, not articulation.** The audio
failures are emphasis inversion (a three-sentence crescendo delivered
with the weight on the *first* line instead of the last) and register
flatness (the rhetorical repetition "six losses is six losses" delivered
without frustration). The model can stress a sentence; it cannot stage a
paragraph.

**3. The video's dominant failure is affective decoupling.** Frame
analysis across two semantically opposite passages shows the avatar
wearing the same broad smile while reporting a losing streak and while
issuing a closing directive. The model animates *speech* — phoneme-driven
mouth shapes — and layers constant pleasant affect on top. It does not
animate *meaning*. Blink suppression (no blink in six continuous seconds)
and consistently off-axis gaze are also present, but those are tuning
problems; this one is architectural.

**4. Adding a modality made things worse, not better.** The same audio
weakness that reads as merely under-performed in isolation becomes an
active contradiction once there is a face: neutral voice, smiling face,
bad news. Adding modalities does not average out weaknesses — it
multiplies the surfaces on which a mismatch can be detected. A
convincing audio fake is easier to make than a convincing video one for
reasons beyond rendering difficulty.

**5. The public detector returned a false negative.** Hive's
AI-Generated Content Detection demo, given a frame from the synthetic
video, reported **2.9% AI-generated, 0% deepfake** and the explicit
verdict *"not likely to contain AI-generated or deepfake content."* The
HeyGen watermark was visible in the submitted frame. The detector
missed fully synthetic content while the generating company's branding
sat in the pixels. Its video analysis path crashed outright on every
file submitted. The reason the miss is plausible is the substantive
point: this artifact is real camera footage of a real actor,
re-animated, so the manipulation lives in motion rather than in pixel
statistics. The avatar pipeline evades detection not by being
sophisticated but by being a category of thing the detectors were not
built for — and it is the free, no-credit-card path.

**6. C2PA credentials attributed the file to the wrong tool entirely.**
The only cryptographic provenance found on the tested frame read
*"Tool or device used: Anthropic Files"* — the environment that cropped
it for upload. Neither ElevenLabs nor HeyGen left any signed trace. The
standard is opt-in and last-writer-wins, so a non-signing generator is
indistinguishable from a camera, and an incidental post-processor
becomes the apparent origin. The credential was accurate and useless at
the same time.

**7. The only marker that worked is the one that exists for commercial
reasons and damages the content.** The tiled watermark survived screen
recording, re-encoding and cropping because it is burned into the
pixels — it survived for precisely the reason a paying user would find
it unacceptable. It is removable for $29/month, meaning unmarked output
is available to exactly the population motivated enough to pay. The
safety property is a side effect of the pricing page.

**8. The friction was commercial, not technical.** Generation was fast
and worked first time in both tools. What consumed the time was
discovering and routing around free-tier restrictions: a 60-second cap
that forced the script to be re-cut, then a download paywall that forced
screen capture. Anyone estimating the effort to produce a convincing
synthetic video should budget for the second, not the first.

**9. The overall conclusion: the asymmetry is not close.** Both
generation tools worked correctly on the first attempt, in under an
hour, for free. On the detection side: one tool was unreachable, one
crashed on video and returned a false negative on stills, and the
cryptographic provenance standard named the wrong tool. Meanwhile every
failure in the artifacts themselves was invisible on first exposure —
the audio problems surfaced only on a directed second listen, and the
video's central problem was invisible in any single frame and required
extracting and comparing frames from two different passages. "Look
closely" is not adequate advice. Knowing *which comparison to run* is
the actual skill, and it is a much harder thing to ask of a general
audience than of a researcher who already suspects the answer.

## Ethics note

No deepfake of any real named person was created for this task, in any
form. The voice is a stock synthetic voice; the on-screen figure is a
tool-provided stock avatar.

It is worth recording where the consent safeguards actually sat. Voice
cloning was unavailable on ElevenLabs' free tier not for safety reasons
but because it is a paid feature. HeyGen presented "Clone a real
person" from video footage as a standard onboarding option, with voice
cloning as step 2 of account setup, and no verification step was
observed at any point. In both tools, the guardrails most relevant to
misuse were positioned by commercial logic rather than by risk.

Every artifact in this repository is labeled as synthetic in its
filename and in this README. The ethics task follows this one; this
repository is the hands-on groundwork for it.
