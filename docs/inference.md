# Inference guide

This guide runs the Awngi XLS-R CTC model on a local WAV recording. It uses greedy decoding and does not require an external language model.

## 1. Install dependencies

Python 3.9+ is recommended. Install:

```bash
pip install torch transformers librosa soundfile
```

For GPU inference, install the PyTorch build appropriate for your CUDA environment from the official PyTorch installation selector.

## 2. Run transcription

Save this as `transcribe_awngi.py`:

```python
import argparse
import torch
import librosa
from transformers import AutoProcessor, AutoModelForCTC

MODEL_ID = "Kuaja/awngi-xlsr-300m-asr"
TARGET_SAMPLE_RATE = 16_000

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("audio", help="Path to an input WAV/audio file")
    args = parser.parse_args()

    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

    processor = AutoProcessor.from_pretrained(MODEL_ID)
    model = AutoModelForCTC.from_pretrained(MODEL_ID).to(device)
    model.eval()

    # Load as mono and resample to the model's 16 kHz input rate.
    speech, _ = librosa.load(
        args.audio,
        sr=TARGET_SAMPLE_RATE,
        mono=True,
    )

    inputs = processor(
        speech,
        sampling_rate=TARGET_SAMPLE_RATE,
        return_tensors="pt",
    )

    input_values = inputs.input_values.to(device)
    attention_mask = inputs.get("attention_mask")
    if attention_mask is not None:
        attention_mask = attention_mask.to(device)

    with torch.inference_mode():
        logits = model(
            input_values=input_values,
            attention_mask=attention_mask,
        ).logits

    predicted_ids = torch.argmax(logits, dim=-1)
    text = processor.batch_decode(predicted_ids)[0]

    print("Awngi transcription:")
    print(text)

if __name__ == "__main__":
    main()
```

Run it:

```bash
python transcribe_awngi.py path/to/awngi_recording.wav
```

The first run downloads the model files from Hugging Face and caches them locally.

## 3. Input requirements

- Supply a readable audio file.
- Audio is converted to mono and resampled to 16,000 Hz by the example.
- The example performs greedy CTC decoding (argmax over frame-level logits followed by processor decoding).
- The script does not apply an external language model, punctuation restoration, or additional text normalization.

## 4. Python usage

The same model can be loaded in a notebook or Python application:

```python
from transformers import AutoProcessor, AutoModelForCTC

model_id = "Kuaja/awngi-xlsr-300m-asr"
processor = AutoProcessor.from_pretrained(model_id)
model = AutoModelForCTC.from_pretrained(model_id)
```

Preprocess the waveform to mono, 16 kHz, pass it to the processor, run the model, and decode the highest-scoring CTC token sequence as shown above.

## 5. Important limitations

This is a research baseline. Its reported test corpus WER is 58.07% and CER is 19.64%; transcription errors are therefore expected. Review outputs before relying on them. Do not use the model as the sole basis for high-stakes decisions.

## Links

- Model: https://huggingface.co/Kuaja/awngi-xlsr-300m-asr
- Code, notebook, and transcript files: https://github.com/quaja-abu/Awngi-ASR-Wav2Vec2-XLSR
