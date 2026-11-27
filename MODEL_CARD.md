---
language:
  - other
license: other
library_name: transformers
pipeline_tag: automatic-speech-recognition
tags:
  - awngi
  - automatic-speech-recognition
  - speech
  - wav2vec2
  - xls-r
  - low-resource
---

# Awngi XLS-R 300M ASR

## Model description

This model is a fine-tuned version of [facebook/wav2vec2-xls-r-300m](https://huggingface.co/facebook/wav2vec2-xls-r-300m) for automatic speech recognition (ASR) in Awngi. It uses a CTC output head and the accompanying tokenizer/processor configuration. Inference uses greedy CTC decoding; no external language model is required by the documented baseline.

## Intended use

The model is intended for research and evaluation of Awngi speech recognition, including work on low-resource speech technologies. It is not validated for legal, medical, financial, or other high-stakes transcription. Outputs may contain substantial recognition errors and should be reviewed by a fluent Awngi speaker.

## Training data

The research corpus contains 7,380 utterances (approximately 13.54 hours) from 78 speakers. It was divided into speaker-independent partitions:

| Partition | Utterances | Duration | Speakers |
|---|---:|---:|---:|
| Train | 6,598 | 12.04 h | 70 |
| Validation | 392 | 0.76 h | 4 |
| Test | 390 | 0.73 h | 4 |

No speakers overlap between the three partitions. Publicly released transcript CSV files are available in the [project GitHub repository](https://github.com/quaja-abu/Awngi-ASR-Wav2Vec2-XLSR/tree/main/data/transcripts). Original audio recordings are not included in the public release.

## Training procedure

The final model was obtained through two phases of XLS-R fine-tuning:

- Phase 1: 24,500 optimizer updates; learning rate 3e-5; warm-up 500 updates; batch size 1; gradient accumulation 4. The feature encoder was frozen for the first 3,000 updates and then unfrozen.
- Phase 2: initialized from the Phase 1 checkpoint selected at step 23,000; fresh optimizer and scheduler; learning rate 1e-5; warm-up 100 updates; 3,000 additional optimizer updates; batch size 1; gradient accumulation 4; all model parameters trainable.

The total reported training schedule is 27,500 optimizer updates across the two phases. Phase 2 was selected as the final model based on validation performance. Full software versions, seeds, and preprocessing details should be consulted in the project repository and accompanying paper; not all environment details are currently recorded in this card.

## Evaluation

The final Phase 2 model was evaluated on the held-out test partition using greedy CTC decoding:

| Metric | Result |
|---|---:|
| Corpus word error rate (WER) | 58.07% |
| Corpus character error rate (CER) | 19.64% |
| Character accuracy (1 − CER) | 80.36% |
| Mean utterance WER | 57.16% |
| Median utterance WER | 58.11% |

Corpus WER and CER are computed by aggregating edit counts over the test corpus. Mean and median utterance WER are supplementary utterance-level summaries. No statistical significance testing between phases is claimed.

## How to use

```python
import torch
import librosa
from transformers import AutoProcessor, AutoModelForCTC

model_id = "Kuaja/awngi-xlsr-300m-asr"
processor = AutoProcessor.from_pretrained(model_id)
model = AutoModelForCTC.from_pretrained(model_id)

device = "cuda" if torch.cuda.is_available() else "cpu"
model = model.to(device).eval()

audio, _ = librosa.load("sample.wav", sr=16_000, mono=True)
inputs = processor(audio, sampling_rate=16_000, return_tensors="pt")

with torch.no_grad():
    logits = model(
        inputs.input_values.to(device),
        attention_mask=inputs.get("attention_mask", None).to(device)
        if inputs.get("attention_mask") is not None else None,
    ).logits

predicted_ids = torch.argmax(logits, dim=-1)
transcription = processor.batch_decode(predicted_ids)[0]
print(transcription)
```

See the [inference guide](https://github.com/quaja-abu/Awngi-ASR-Wav2Vec2-XLSR/blob/main/docs/inference.md) for details.

## Limitations and risks

- The corpus is small and represents a limited number of speakers and recording conditions.
- Recognition quality may vary with speaker, dialect, accent, speaking style, microphone, noise, and domain.
- The reported test results describe this particular held-out split and should not be interpreted as performance on all Awngi speakers or domains.
- The model may produce inaccurate or incomplete transcripts. Human review is recommended.
- The model has not been assessed for demographic fairness or high-stakes use.
- The training audio is not publicly downloadable; independent reproduction of acoustic training requires appropriately authorized access to the recordings.

## Ethical considerations

The study received ethics approval from Injibara University IRERC. Recordings were collected before the approval letter date; written informed consent was obtained before recording. Participants consented to public sharing of anonymized transcripts. Audio recordings are not publicly released and remain subject to controlled access and applicable institutional conditions. Users must not attempt to identify speakers or redistribute restricted audio.

## License

The model repository's license must be confirmed by the model publisher before redistribution or reuse. The upstream XLS-R checkpoint is identified at its source repository; that upstream license does not by itself establish the license for this fine-tuned model or the associated dataset. Check the model repository's current license metadata and the paper for the applicable terms before use.

## Citation

A complete citation will be added when the associated paper is published. Until then, cite the project repository and model repository URLs.
