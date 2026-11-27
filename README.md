# Awngi Automatic Speech Recognition with XLS-R

This repository provides research artifacts for **“Developing a Low-Resource Automatic Speech Recognition System for Awngi Language Using Wav2Vec2 XLS-R.”**

The project fine-tunes the multilingual XLS-R 300M speech representation model for Awngi automatic speech recognition (ASR), using a speaker-independent train/validation/test split.

## Released resources

- **Fine-tuned model:** [Kuaja/awngi-xlsr-300m-asr on Hugging Face](https://huggingface.co/Kuaja/awngi-xlsr-300m-asr)
- **Source code and experiment notebook:** this repository's [notebooks](notebooks/)
- **Transcript files and split documentation:** [data/transcripts](data/transcripts/)
- **Inference instructions:** [docs/inference.md](docs/inference.md)
- **Reproducibility notes:** [docs/reproducibility.md](docs/reproducibility.md)
- **Model card:** [MODEL_CARD.md](MODEL_CARD.md)

## Dataset summary

The corpus contains 7,380 transcribed utterances from 78 speakers, with approximately 13.54 hours of speech.

| Partition | Utterances | Duration | Speakers |
|---|---:|---:|---:|
| Train | 6,598 | 12.04 h | 70 |
| Validation | 392 | 0.76 h | 4 |
| Test | 390 | 0.73 h | 4 |
| **Total** | **7,380** | **13.54 h** | **78** |

The partitions are speaker-independent; no speakers are shared across train, validation, and test. The repository contains transcript CSV files, not the original audio recordings. Audio is not publicly hosted here; access is subject to the study's data-governance and consent conditions. Do not attempt to reconstruct or infer participant identities from released files.

## Final model and evaluation

The final model is the Phase 2 continuation of the Phase 1 XLS-R fine-tuning run. Phase 2 was initialized from the Phase 1 checkpoint selected at step 23,000, then trained for a further 3,000 updates with a fresh optimizer and learning-rate schedule. The combined training schedule therefore comprises 27,500 optimizer updates (24,500 in Phase 1 plus 3,000 in Phase 2; Phase 2 starts from the Phase 1 step-23,000 checkpoint).

Held-out test results for the final Phase 2 model, using greedy CTC decoding:

| Metric | Test result |
|---|---:|
| Corpus WER | 58.07% |
| Corpus CER | 19.64% |
| Character accuracy (1 − CER) | 80.36% |
| Mean utterance WER | 57.16% |
| Median utterance WER | 58.11% |

The Phase 1 and Phase 2 results are experimental comparisons; no statistical significance claim is made. See the model card for further details and limitations.

## Quick inference

Install the dependencies:

```bash
pip install torch transformers soundfile librosa
```

Then follow [the inference guide](docs/inference.md). The example loads the model and processor from Hugging Face and performs greedy CTC decoding on a local WAV file.

## Ethical use and data access

The study received ethics approval from Injibara University IRERC. Written informed consent was obtained before recording. Participants consented to sharing anonymized transcripts publicly. The audio recordings are not publicly released and remain subject to controlled access and applicable institutional conditions.

The transcript files should be used only for legitimate research and in accordance with the consent and ethical conditions. Do not re-identify speakers, redistribute audio, or combine the data with other sources to identify participants.

## Citation

If you use this work, please cite the associated paper:

> [Full bibliographic citation to be added after publication.]

## Repository contents

- `notebooks/`: experiment notebook(s)
- `tokenizer/`: tokenizer and feature-extractor configuration files
- `data/transcripts/`: train, validation, and test transcript CSV files
- `docs/`: inference and reproducibility documentation
- `decoders/`: decoder documentation and related artifacts

## Status and reproducibility

The public model repository provides the final model weights and configuration. The notebook and documentation provide the available experimental workflow. Exact bit-for-bit reproduction may depend on the original software environment, random seeds, audio preprocessing, and access to the corresponding audio corpus. See [docs/reproducibility.md](docs/reproducibility.md) for items that still require version-level verification.
