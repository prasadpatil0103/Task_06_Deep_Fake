# EVALUATION.md

> All artifacts assessed here are AI-generated. See README disclosure.

Critical evaluation of each artifact. The aim throughout is language more
precise than "sounds real" or "looks fake" — every claim below points at
a specific, nameable behaviour, with timestamps or frame evidence.

## Working vocabulary

Terms used below, defined up front. The first five were anticipated; the
last four were surfaced by these particular experiments.

- **Numeric flattening** — figures read at uniform pace and pitch,
  without the grouping or stress a speaker applies to numbers that belong
  together semantically.
- **Breath absence** — no audible inhalation before long clauses; the
  voice never seems to need air.
- **Terminal uniformity** — every sentence closing on the same falling
  pitch contour regardless of rhetorical function.
- **Lip-sync drift** — mouth movement and phoneme onset diverging,
  usually accumulating across a clip.
- **Temporal instability** — frame-to-frame inconsistency in hair,
  background, teeth, or head position.
- **Emphasis inversion** — stress placed on the wrong member of a
  sequence relative to how a speaker would build a point.
- **Register flatness** — emotional temperature held constant across
  passages whose content calls for it to change.
- **Affective decoupling** — the facial performance bearing no
  relationship to the semantic content of the speech it accompanies.
  Distinct from a merely *wrong* expression: the face is not misreading
  the content, it is not reading it at all.
- **Blink suppression** — blink rate far below the human baseline of
  roughly one every 2–4 seconds.
- **Gaze off-axis** — eyeline directed consistently away from the lens,
  producing the impression of addressing someone out of frame.

---

## Artifact 1: `AI_GENERATED_attempt1_elevenlabs.mp3`

**Tool:** ElevenLabs, Eleven Multilingual v2, stock voice "Roger"
**Pipeline:** text → speech, no visual component
**Duration:** 1:35

### Where it holds up

**Numbers are handled correctly — the expected failure point, and it
wasn't one.** The script is unusually dense with spoken figures and the
hypothesis going in was that this would break. It didn't. *"We finished
sixteen and six"* is delivered as a single semantic unit with the stress
contour of a sporting record, not as two independent numerals. That
distinction carries the meaning: getting it wrong would strip the phrase
of its sense. The model got it right with no markup. The remaining
figures — *"just over seventeen goals"*, *"under ten"*, *"just under
thirteen"*, *"three goals or fewer"* — are all clean.

**Breath artifacts are present, not absent.** Audible inhalation occurs
between clauses. Worth flagging specifically because breath absence is
among the most commonly cited TTS tells, and it does not apply here. The
model is simulating respiration rather than producing an unbroken stream.
A listener scanning for the "no breathing" giveaway would not find it.

**Pacing is natural.** No robotic evenness; delivery speeds and slows
roughly as conversational speech does.

### Where it fails

**Emphasis inversion in the closing sequence.** The script ends with
three short sentences built as a deliberate crescendo: *"Offense first.
Build around Tyrrell. That's where I'd start."* A speaker lands the final
line hardest — it is the resolution the two fragments set up. The
synthesis does the reverse: "Offense first" carries noticeably more
weight than "That's where I'd start," which tapers. The model appears to
apply a default sentence-level contour without modelling the rhetorical
arc *across* sentences. It can stress a sentence; it cannot stage a
paragraph.

**Register flatness on rhetorical repetition.** The line *"six losses is
six losses"* is a device — the repetition exists to convey frustration,
and the second instance should carry different weight than the first.
Both are delivered at the same emotional temperature. The words are
correct; the intent behind them is not conveyed. This is the clearest
instance of the model reading text rather than performing an argument.

### Would a casual listener be fooled?

Probably yes. Both failures are failures of *performance*, not
*articulation* — nothing is mispronounced, nothing stutters, nothing is
obviously synthetic at the surface. A listener hearing this once, at
normal attention, in a context where a coach's audio memo would be
unsurprising, has little to catch on. Notably, the two tells a
non-expert would most likely be listening for — flat robotic numbers and
absent breathing — are precisely the two things this model does well.

### Would someone who works with this technology be fooled?

Less likely, but not because any single artifact is damning. The
emphasis inversion is the strongest tell because it is a structural
limitation rather than a random glitch: it recurs predictably wherever a
script builds across multiple sentences.

### The creator-vs-listener gap

The failures above were **not audible on a first pass.** The initial
impression was that the artifact was clean. Both surfaced only on a
second, *directed* listen with specific questions held in mind — "do
these three sentences build?", "does the repetition carry frustration?"
That gap is itself a finding: the difference between listening *to*
something and listening *for* something is the difference between
accepting the artifact and catching it.

### Anything refused or degraded?

Nothing. No content warnings, no filters, no silent degradation, no
rewording required. The script names a real athlete and discusses a real
team's season without tripping anything. No voice-clone consent gate was
encountered because cloning is unavailable on the free tier.

---

## Artifact 2: `AI_GENERATED_attempt2_heygen_screencapture.mov`

**Tool:** HeyGen, Avatar IV, stock avatar "Vernon Lounge Side 2"
**Audio:** the Attempt 1b ElevenLabs file, unmodified
**Duration:** 38 seconds of video (capture runs 56s including end screen)
**Capture:** macOS ReplayKit screen recording, 60 fps, 3420×2214 —
download was paywalled, see PROCESS_LOG

### Method note

Evaluation was done on extracted frames rather than by eye alone. Two
dense samples were pulled: sixteen frames at 0.4-second intervals across
14.0–20.0s (the passage describing the losses), and twelve frames at
0.6-second intervals across 31.0–37.6s (the closing directive). Both
grids are in `evidence/`.

Frames were used because **the most significant failure below is
invisible in any single frame** and becomes apparent only when frames
from semantically opposite passages are placed side by side.

### Where it holds up

**Everything static.** Skin texture retains pores and specular variation
rather than the waxy flatness of earlier avatar generations. Lighting on
the subject is consistent with the room's practical sources. The
background — staircase, dried arrangement, framed print, sofa, flowers —
is stable across all 38 seconds with no flicker at the hairline, no
warping at the ear, no jitter in the furniture. Clothing folds move with
the body.

**Gesture is well-formed.** Open-palm presentational gestures around 28s
are the kind of thing a person actually does while explaining a number,
and sampled frames show distinct hand configurations rather than a
repeating cycle — not looped.

On a single frame, this artifact is not distinguishable from a real
recording of a man standing in a room.

### Where it fails

**Affective decoupling — the dominant failure.** Unambiguous in the
evidence.

At 14.0–20.0s the audio says *"In our wins, we averaged just over
seventeen goals a game. In our losses, that fell to ten."* Across sixteen
sampled frames the avatar is **broadly smiling, teeth visible, corners
raised**, for nearly the entire window.
→ `evidence/failure_expression_mismatch_losses_passage.png`

At 31.0–37.6s the audio delivers *"Offense first. Build around Tyrrell.
That's where I'd start."* Across twelve sampled frames the avatar wears
**the same broad smile at substantially the same intensity.**
→ `evidence/failure_expression_mismatch_closing_passage.png`

Place the two grids side by side and a coach reporting a losing streak is
facially indistinguishable from a coach issuing a directive. The model is
animating *speech* — mouth shapes driven by phonemes — and generating
pleasant affect as a constant decorative layer on top. It is not
animating *meaning*. This is structural, not a glitch: it will recur on
any script whose emotional register varies, which is most scripts worth
synthesizing.

**Blink suppression.** No blink visible across sixteen frames spanning
six continuous seconds. Human baseline is roughly one blink every two to
four seconds, so that window should contain two or three. The classic
tell, present.

**Gaze off-axis.** Eyeline directed consistently up and to the subject's
left, never at the lens, across both sampled passages. The effect is of a
man addressing someone standing beside the camera. For content framed as
speaking *to* a team, a mismatch — a coach delivering this assessment
would make eye contact.

**Mouth rarely at rest.** Across the dense sample the mouth is open in
the large majority of frames, with few complete closures. English at
conversational rate should produce regular bilabial closures on *p*, *b*
and *m*; the script's *"number"*, *"blowouts"* and *"build"* all require
them. Some closures are present, but the overall impression is of a mouth
continuously articulating rather than forming and releasing consonants.
The 186 wpm delivery rate measured in SCRIPT.md is a plausible
contributing factor.

### Would a casual viewer be fooled?

**In this state, no — but for a reason that has nothing to do with the
deepfake.** The tiled watermark covering the entire frame, face included,
makes the artifact unmistakably machine-generated at a glance. That is
the free tier's doing, not the model's.

Strip the watermark and the answer likely flips to yes for a single
viewing at speed. The smile reads as warmth rather than as error unless
you are holding the audio content in mind simultaneously. Blink absence
and off-axis gaze are the kind of thing that registers as vague oddness
rather than identifiable fault.

### Would someone who works with this technology be fooled?

No, and affective decoupling is why. Blink rate can be patched; gaze can
be corrected. A face holding one expression across semantically opposite
content is a failure of the model's architecture rather than its tuning,
and it is visible to anyone who thinks to compare two passages instead of
watching one.

### Anything refused or degraded?

No content refusals. Degradation was entirely commercial: length capped
at 60 seconds, resolution at 720p, watermark non-removable, and the
finished file withheld behind a $29–49/month paywall. The rendered
artifact existed and was viewable but not retrievable.

---

## Cross-artifact comparison

| Dimension | Artifact 1 (audio) | Artifact 2 (video) |
|---|---|---|
| Spoken numbers | **Strong** — no flattening | Inherited from A1 |
| Breath simulation | **Present** | Inherited from A1 |
| Pacing | Natural | Natural |
| Cross-sentence emphasis | **Weak — inverted** | Inherited, and now *contradicted* by the visual |
| Emotional register | **Weak — flat** | **Worse — decoupled entirely** |
| Static realism | N/A | **Strong** |
| Blink / gaze | N/A | **Weak** |
| Temporal stability | N/A | **Strong** |
| Provenance marking | None observed | Tiled visible watermark; no C2PA |
| Time to output | ~10 min | ~35 min |
| Retrievable? | Yes | **No — screen capture required** |

### Did the video make the audio's problems better or worse?

**Worse** — and this is the most interesting result of running the same
audio through both pipelines.

Artifact 1's weakness was register flatness: *"six losses is six losses"*
delivered without the frustration the repetition implies. On its own that
reads as a slightly under-performed line, easy to miss.

Add a face and the same flatness becomes an active contradiction. The
voice is neutral where it should be frustrated; the face is *smiling*.
Two independent channels now disagree with the words and with each other.
The visual layer did not mask the audio's shortcoming — it amplified it,
because an audience tolerates an under-inflected voice far more readily
than it tolerates a smile over bad news.

The practical implication: **adding modalities does not average out
weaknesses, it multiplies the surfaces on which a mismatch can be
detected.** A convincing audio deepfake is easier to produce than a
convincing video one, not merely because video is harder to render, but
because video hands the audience a second channel to cross-check
against.

### The creator-vs-viewer gap, revisited

Artifact 1's failures surfaced only on a *directed* second listen.
Artifact 2 extends this: its central failure was invisible in any single
frame and became apparent only when frames from two passages were
extracted and compared. The artifact survived casual viewing, survived
careful viewing of any one moment, and failed only under a comparison
the viewer has to deliberately construct.

This is the substantive conclusion from both artifacts together.
**Detecting current synthetic media is not a perceptual task, it is an
analytical one.** Watching more attentively does not reliably help;
knowing which comparison to run does. That is a meaningfully harder thing
to ask of a general audience than "look closely."
