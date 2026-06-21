# MCNet-RF: Multi-Branch CNN for RF Modulation Classification

Automatic Modulation Classification (AMC) using deep learning on the RadioML 2016.10a dataset.

This project was developed as part of a CDAC machine learning training program and investigates whether a multi-branch CNN architecture combined with domain-specific signal processing features can outperform classical CNN baselines for RF modulation recognition.

## Overview

The objective is to classify wireless modulation schemes directly from raw IQ samples.

The baseline model combines three complementary signal representations:

1. Raw IQ samples
2. FFT magnitude and phase features
3. Signal envelope and instantaneous phase features

Each representation is processed by an independent CNN branch. Feature vectors are fused and passed through fully connected layers for final modulation prediction.

A second-generation architecture currently under development adds higher-order cumulant features inspired by recent AMC literature.

## Model Versions

### v1 — Multi-Branch Baseline (complete)
- Three-branch CNN fusing raw IQ, FFT (magnitude/phase), and envelope/instantaneous-phase features
- Trained and evaluated on RadioML 2016.10a
- Serves as the baseline for comparing against classical single-branch CNNs

### v2 — Cumulant-Augmented Architecture (in development)
- Extends v1 by adding a fourth branch for higher-order statistical cumulant features (e.g., C20, C21, C40, C42), which are known to carry strong discriminative information for modulation order and constellation shape
- Goal: improve classification accuracy at low SNR, where amplitude/phase-based features alone tend to degrade
- Currently being benchmarked against v1 on the same dataset splits

## Dataset

**RadioML 2016.10a**

Dataset size:

* 220,000 signal samples
* 11 modulation classes
* 20 SNR levels (-20 dB to +18 dB)
* 1000 examples per modulation/SNR pair

Input format:

```text
(220000, 2, 128)
```

## Limitations Observed

- **Synthetic dataset bias**: RadioML 2016.10a is generated using simulated channel models (e.g., GNU Radio with synthetic AWGN, fading, and clock offsets). Models trained on it may not generalize well to real-world RF captures with hardware-specific impairments.
- **Limited modulation diversity**: Only 11 modulation classes are covered, which excludes many higher-order and modern digital modulation schemes used in practical wireless systems.
- **Fixed-length input constraint**: All samples are fixed at 128 IQ samples per example, which may not reflect realistic burst lengths or continuous-stream signals.
- **Low-SNR degradation**: Performance across all branches (including the fused model) drops noticeably below 0 dB SNR, where FFT and envelope-based features become noisy and less discriminative.
- **Increased model complexity**: The multi-branch fusion design adds parameters and inference latency compared to a single-branch CNN, which may be a constraint for real-time or edge deployment.
- **No real-world validation**: All experiments to date are confined to RadioML 2016.10a; the model has not yet been tested against over-the-air captures or other independently collected RF datasets.
- **v2 cumulant branch is unverified at scale**: Early experiments with higher-order cumulant features show promise but have not yet been validated across the full SNR range or compared rigorously against v1 on held-out test splits.

## Future Scope

- **Sig53 synthetic dataset integration**: Incorporate the Sig53 dataset (TorchSig) to expand training data with a broader range of modulation types, signal impairments, and more realistic synthetic channel conditions. This is intended to test generalization beyond RadioML 2016.10a and reduce dataset-specific overfitting.
- **Cross-dataset generalization**: Train on one dataset (e.g., RadioML 2016.10a) and evaluate on another (e.g., Sig53) to quantify how well learned features transfer across simulation environments.
- **Completion and benchmarking of v2**: Finish training and evaluating the cumulant-augmented architecture, with a focus on low-SNR performance gains over v1.
- **Real-world / over-the-air evaluation**: Validate the model using SDR-captured signals to assess robustness against real hardware noise, frequency offset, and multipath effects not present in synthetic datasets.
- **Model compression for edge deployment**: Explore pruning, quantization, or lightweight branch architectures to reduce inference latency for deployment on SDR or embedded platforms.
- **Expanded modulation set**: Extend classification beyond the 11 RadioML classes to include modern digital modulation schemes relevant to current wireless standards.
- **Explainability**: Investigate which branch (IQ, FFT, envelope, or cumulant) contributes most to correct classification at different SNR levels, to better understand the value added by each feature representation.
