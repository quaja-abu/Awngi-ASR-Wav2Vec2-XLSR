# Reproducibility record

This document tracks the information required to reproduce the Awngi XLS-R experiments.

## Items to verify and document

- Exact pretrained XLS-R model identifier and revision.
- Python, PyTorch, Transformers, CUDA, and other dependency versions.
- Audio sampling rate, resampling, normalization, and preprocessing.
- Dataset manifest and exact train/validation/test partitions.
- Speaker-disjointness checks, where applicable.
- Vocabulary/tokenizer construction and vocabulary file.
- Model architecture and all training hyperparameters.
- Random seeds and deterministic settings.
- Training duration, update count, batch size, gradient accumulation, and checkpoint selection.
- CTC decoding configuration and language-model artifacts.
- Evaluation script, normalization rules, and WER/CER calculations.

## Original and reproduced experiments

The results originally reported in the manuscript must be preserved as a historical record and clearly distinguished from any future reproduction runs. Do not overwrite original results with new measurements.

## Current limitations

The final original acoustic-model checkpoint is currently unavailable and recovery is being investigated. The exact original data partitions and all supporting artifacts must be verified before claiming exact reproducibility.
