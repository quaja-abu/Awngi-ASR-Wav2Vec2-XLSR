# Final model

The final Phase 2 XLS-R 300M CTC model is hosted on Hugging Face:

**https://huggingface.co/Kuaja/awngi-xlsr-300m-asr**

The model repository contains the trained weights and the configuration/tokenizer/processor files required for inference. Do not upload the raw-audio Arrow caches or intermediate training checkpoints to this GitHub repository.

## Evaluation

Held-out test results with greedy CTC decoding:

- Corpus WER: 58.07%
- Corpus CER: 19.64%
- Character accuracy (1 − CER): 80.36%

See [the model card](../MODEL_CARD.md) and [inference guide](../docs/inference.md).

The final model was trained in two phases: Phase 1 (24,500 updates) and Phase 2 (3,000 additional updates initialized from the Phase 1 step-23,000 checkpoint, with a fresh optimizer and scheduler). The combined schedule is 27,500 optimizer updates.

## Provenance

The model is the final Phase 2 checkpoint selected using validation performance. The repository's Hugging Face model page is the authoritative source for the currently released weight files.
