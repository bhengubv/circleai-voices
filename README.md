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
| `af_ZA-google-nwu-low.onnx` | Afrikaans | [MycroftAI/mimic3-voices](https://github.com/MycroftAI/mimic3-voices/tree/master/voices/af_ZA/google-nwu_low) — trained on [OpenSLR 32](http://www.openslr.org/32/), the Google/North-West University South African corpus; ONNX conversion from sherpa-onnx | CC-BY-SA-4.0 |
| `ig_IB-soro-medium.onnx` | Igbo | [Shinzmann/soro-tts-ibo](https://huggingface.co/Shinzmann/soro-tts-ibo) — trained on [WaxalNLP](https://huggingface.co/datasets/google/WaxalNLP), re-exported to ONNX by us | CC-BY-NC-4.0 |
| `jsut_vits_prosody.onnx` | Japanese | [espnet/kan-bayashi_jsut_vits_prosody](https://huggingface.co/espnet/kan-bayashi_jsut_vits_prosody) — trained on the [JSUT corpus](https://sites.google.com/site/shinnosuketakamichi/publication/jsut) (University of Tokyo), re-exported to ONNX by us | CC-BY-4.0 |
| `sys.dic`, `matrix.bin`, `char.bin`, `unk.dic`, `*-id.def`, `rewrite.def` | Japanese (phonemiser) | [Open JTalk](http://open-jtalk.sourceforge.net/) dictionary `open_jtalk_dic_utf_8-1.11`, NAIST — dictionary data only, no engine code | BSD 3-clause (`COPYING`) |

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
does not belong in a clone:

    https://github.com/bhengubv/circleai-voices/releases/download/<tag>/<asset>

Every file is pinned by SHA-256 in CircleAI's model registry and verified after
download, so a replaced or corrupted asset fails loudly instead of being spoken.

**Release assets are flat, and the tag is not a directory.** The catalogue keeps
the tag on the repository — `bhengubv/circleai-voices@voices-v1` — and uses the
bundle file name for the path the file unpacks to locally. Those are two
different things, and collapsing them broke Japanese silently: naming the
dictionary `voices-v1/sys.dic` builds a correct URL, downloads 103 MB, verifies
its SHA, and lands it in a folder the phonemiser does not look in. Nothing
errors; Japanese just has no phonemiser. Only the last segment of a bundle name
is the asset here.

The nine Japanese dictionary files keep their exact upstream names because Open
JTalk opens them by name from a folder it is handed, not from a list.

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

### On the Afrikaans voice

It replaces an eleven-language South African model that measured **at its own
noise floor** for Afrikaans — cer 0.76 against a 0.76 floor, unmoved by every
encoding, language id and recogniser tried. That is not a weak voice, it is
noise wearing a language label.

The replacement measures **CER 0.48 against a 1.17 floor**. `Goeie môre` comes
back as "Goeiemor", `Die son skyn vandag mooi` as "Disone schijn van dach moui".
It is phoneme-driven and needs espeak with the `af` voice; its sidecar here is
generated from the bundle's own `tokens.txt`, because Mimic3 ships a training
config rather than a Piper sidecar.

The eleven-language model keeps its other ten languages and simply no longer
claims `af`.

### On the Japanese voice

It is the one voice here that is neither grapheme-driven nor espeak-driven, and
that is why the dictionary ships beside it. Japanese is written without spaces
and its pitch accent is not recoverable from the characters, so the text goes
through Open JTalk to full-context labels, and the accent fields in those labels
become the bracket tokens the model was trained on. Strip the brackets and it
still speaks — confidently, with flat and wrong prosody, which a recogniser will
happily transcribe as correct.

Scoring it needs care for a reason that has nothing to do with the voice.
Japanese has no single correct spelling of a spoken sentence. Reading back
`これはテスト文です`, the recogniser wrote `これは手スト分です` — /te/ as 手
rather than テ, /bun/ as 分 rather than 文. Same sounds, same word, two
characters different, and a character error rate of 0.22 for a perfect reading.
Compared as phonemes instead it is **0.00**, and 0.01–0.12 on longer sentences,
with no unknown tokens.

Both files were built and measured months before they were published; what is in
this release is byte-identical to what was measured.
