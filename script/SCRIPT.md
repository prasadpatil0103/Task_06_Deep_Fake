# SCRIPT.md

**Source:** Adapted from the Phase B "coach advisory" answer in
`Task_05_Descriptive_Stats/PROMPT_LOG.md` (Q9) and `FINDINGS.md`, built on
ground truth computed by that project's `ground_truth.py`.

Every figure spoken below is verifiable against that script's output
(`logs/ground_truth_output.txt` in the Task 5 repo). Nothing was invented
for this task.

---

## Version 2 — the script actually synthesized (118 words)

This is the version used for both artifacts. See the note below on why it
is shorter than the original.

> Here's my read on this season. We finished sixteen and six — a good
> record, but six losses is six losses, and I wanted to know what actually
> broke down.
>
> In our wins, we averaged just over seventeen goals a game. In our
> losses, that fell to ten. The goals we allowed only went up by about
> three. So the offense dropped roughly five goals below its own average
> in the games we lost — almost double the defensive slip.
>
> And five of our six losses were decided by three goals or fewer. These
> weren't blowouts.
>
> Offense first. Build around Tyrrell. That's where I'd start.

**Measured delivery:** 118 words → 38 seconds of synthesized audio, i.e.
roughly **186 words per minute**. That is faster than typical
conversational pace (150–160 wpm) despite the speed slider sitting near
default, and it is a plausible contributing factor to one of the visual
findings in `EVALUATION.md` (the avatar's mouth rarely reaching a closed
rest position).

## Version 1 — the original, longer script (~300 words)

The first version ran ~300 words and synthesized to 1 minute 35 seconds.
It was **not** used for the video, because HeyGen's free plan caps output
at 60 seconds. The full text is preserved in
`logs/PROCESS_LOG.md` under Attempt 1.

**Why this matters as a finding rather than a footnote:** the script was
not shortened for editorial reasons. It was shortened because a
*downstream* tool's pricing tier would not accept the length that an
*upstream* tool was perfectly willing to produce. In a multi-tool
synthetic media pipeline, the most restrictive free tier in the chain
sets the specification for everything before it.

## A note on input normalization

Two changes were made to the markdown source before pasting into the TTS
engine:

1. `--` replaced with real em dash characters (`—`)
2. Hard line wraps removed, restoring full paragraphs

Both were deliberate. TTS engines sometimes vocalize or stumble over
double hyphens, and a line-wrap artifact would have produced a
*formatting* failure rather than a genuine synthesis failure. The intent
was to give the engine clean input so that any artifact observed is
attributable to the model rather than to input hygiene.
