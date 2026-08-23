# circleai-voices

Voice models [CircleAI](https://github.com/bhengubv/CircleAI) ships, mirrored so
they cannot go away.

CircleAI is free AI for people on old hardware who cannot pay for anything else.
It is developed independently and earns no money.

## Why this repository exists

Every voice here was made by someone else. We host a copy for two reasons.

**A speaker of a small language should not lose their voice because someone
else's repository went away.** That is not hypothetical: the catalogue used to
point at a store where 45 of the small files it named had quietly stopped
existing, so those languages downloaded a 114 MB model and then failed on 2 KB
of settings. One address, under our control, is the whole point.

**The second reason is the hours.** Working out which model, which ONNX input
layout, and which front-end each language needs took a long time. Nobody should
have to spend it again.

Hosting a copy makes us a redistributor. Every licence below permits that and
every one requires attribution. **This file is how that obligation is met. If it
is removed, the redistribution stops being compliant.**

## Attribution

| asset | language | upstream | licence |
|---|---|---|---|
| `ne_NP-google-medium.onnx` | Nepali | [rhasspy/piper-voices](https://huggingface.co/rhasspy/piper-voices) — trained on [OpenSLR 43](http://www.openslr.org/43/), 18 speakers | CC-BY-SA-4.0 |
| `ko_KR-kss-medium.onnx` | Korean | [rhasspy/piper-voices](https://huggingface.co/rhasspy/piper-voices) — trained on the [Korean Single Speaker corpus](https://www.kaggle.com/datasets/bryanpark/korean-single-speaker-speech-dataset) | CC-BY-NC-SA-4.0 |
| `ig_IB-soro-medium.onnx` | Igbo | [Shinzmann/soro-tts-ibo](https://huggingface.co/Shinzmann/soro-tts-ibo) — trained on [WaxalNLP](https://huggingface.co/datasets/google/WaxalNLP), re-exported to ONNX by us | CC-BY-NC-4.0 |

Each `.onnx` ships with its `.onnx.json` sidecar, which carries the phoneme map,
the sample rate and the inference scales. The sidecar is part of the voice:
without it the model has no vocabulary, and with the wrong one it produces
fluent speech that is not what you asked for.

**On the NonCommercial entries.** CircleAI earns no money and is a core component
of nothing. CC-BY-NC is the licence that exists for exactly this use. The label
is a note on where a voice came from, not a liability — and nothing NonCommercial
lives in the CircleAI source repository itself.

## Layout

Assets hang off a release tag rather than the git tree, because a 77 MB model
does not belong in a clone. The catalogue addresses them as
`<tag>/<asset>` and resolves that to:

    https://github.com/bhengubv/circleai-voices/releases/download/<tag>/<asset>

Every file is pinned by SHA-256 in CircleAI's model registry and verified after
download, so a replaced or corrupted asset fails loudly instead of being spoken.

### On the Igbo voice

It is weak, and saying so is the point. Measured against its own noise floor —
the same voice reading random in-vocabulary tokens — it scores **CER 0.56 against
a 0.76 floor**. That gap is real: it follows what you give it, and `enyi` comes
back verbatim in a longer sentence. But it is a long way from the European voices
here, and it is published as the best free Igbo voice we could find rather than a
good one.

It replaces a bundle that shipped as `mms-ibo` and was not an MMS voice at all —
Meta's own list of 1077 TTS languages contains no Igbo. That bundle was
`VITS-OpenBible-Igbo`, and re-exporting it correctly proved the checkpoint itself
is broken rather than the pipeline: given "ndewo enyi m kedu ka i mere taa" it
produced fluent, well-formed Igbo with no relationship to the input, in 8 seconds
for six words. Wrong words spoken confidently are worse than no voice, so it is
gone.
