# MCNet-RF: Multi-Branch CNN for RF Modulation Classification

Automatic Modulation Classification (AMC) using deep learning on the RadioML 2016.10a dataset.

This project was developed as part of a CDAC machine learning training program and investigates whether a multi-branch CNN architecture combined with domain-specific signal processing features can outperform classical CNN baselines for RF modulation recognition.

---

## Overview

The objective is to classify wireless modulation schemes directly from raw IQ samples.

The baseline model combines three complementary signal representations:

1. Raw IQ samples
2. FFT magnitude and phase features
3. Signal envelope and instantaneous phase features

Each representation is processed by an independent CNN branch. Feature vectors are fused and passed through fully connected layers for final modulation prediction.

A second-generation architecture currently under development adds higher-order cumulant features inspired by recent AMC literature.

---

## Dataset

### RadioML 2016.10a

Dataset size:

- 220,000 signal samples
- 11 modulation classes
- 20 SNR levels (-20 dB to +18 dB)
- 1000 examples per modulation/SNR pair

Input format:

```text
(220000, 2, 128)
