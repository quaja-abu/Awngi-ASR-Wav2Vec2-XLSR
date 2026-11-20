# Awngi ASR with Wav2Vec2 XLS-R

Research code and reproducibility materials for **Developing a Low-Resource Automatic Speech Recognition System for Awngi Language Using Wav2Vec2 XLS-R**.

## Repository status

This repository is private and under preparation. Files will be checked before any public release.

- Training notebook and source code: to be added and verified.
- Dataset: not included. Audio recordings and restricted participant data must not be uploaded here.
- Dataset partitions and vocabulary: pending verification against the experiments reported in the manuscript.
- Trained acoustic-model checkpoint: currently unavailable; recovery is being investigated.
- Decoder artifacts: available locally and pending inspection before inclusion.
- Experimental results: original reported results will be documented separately from any future reproduction runs.

## Project organization

```text
configs/       Training and decoding configuration files
notebooks/     Research and training notebooks
src/           Data preparation, preprocessing, training, decoding, evaluation
data/          Dataset documentation and approved non-sensitive metadata only
models/        Model/checkpoint availability notes
decoders/      Verified decoder artifacts and instructions
results/       Original reported and future reproduction results, kept separate
docs/          Data access, ethics, and reproducibility documentation
```

## Data and ethics

The audio corpus is not distributed in this repository. Access arrangements, participant-consent conditions, and availability of transcripts and anonymized metadata will be documented after checking applicable institutional approvals and permissions. Do not upload recordings, identifiable participant information, consent forms, or restricted data.

## Reproducibility

The repository will document the software environment, preprocessing, vocabulary, exact data partitions, training and decoding configurations, evaluation procedure, and available artifacts. Any new training run will be clearly distinguished from the original experiments reported in the manuscript.

## Citation

Citation details will be added after publication and finalization of the repository release.
