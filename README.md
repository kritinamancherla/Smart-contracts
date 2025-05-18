# Risk Analysis for Smart Contracts using GRU Deep Learning 

Identify technical & security risks in Solidity smart contracts automatically with an end‑to‑end NLP + GRU classifier.


[![License: Apache‑2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](#)

---

## 📑 Table of Contents
- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Methodology](#methodology)
  - [Model Hyper‑Parameters](#model-hyper-parameters)
- [Quick Start](#quick-start)
- [Results](#results)
  - [Confusion Matrix](#confusion-matrix)
  - [Error Analysis](#error-analysis)
- [Web Demo](#web-demo)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

---

## Problem Statement
Smart contracts collectively safeguard billions of dollars but remain susceptible to bugs such as re‑entrancy, arithmetic overflows, and improper access control. Static analyzers flood auditors with false positives, while manual review is slow and costly.  
This project delivers a **GRU‑based neural classifier** that flags *high‑risk* contracts with >92 % accuracy, cutting review time and focusing expert effort where it matters most.

---

## Dataset
| Source | Contracts | Label Schema (0 = Low, 1 = High) | License |
|--------|-----------:|-----------------------------------|---------|
| Curated‑Bugs (ICSE 2022) | 4 154 | Vulnerable = 1 | MIT |
| SmartBugs‑Wild | 2 687 | Vulnerable = 1 | Apache‑2.0 |
| Solidity‑Samples (manual) | 120 | Safe = 0 | CC‑BY‑4.0 |
| **Total** | **6 961** | — | — |

**Class balance:** 58 % high‑risk vs 42 % low‑risk.  
Large raw archives live in an S3 bucket.

---

## Methodology

1. **Parsing** – `solidity‑parser‑antlr` converts `.sol` files into token streams.  
2. **Embedding** – Custom WordPiece trained on 100 k contracts (dim = 256).  
3. **GRU Classifier** –  
   `Embedding → GRU(128, bidirectional) → Dropout(0.3) → Dense(64) → ReLU → Dense(1) → Sigmoid`

### Model Hyper‑Parameters
```yaml
batch_size: 32
epochs: 12
optimizer: Adam
learning_rate: 1e‑3
loss_fn: BinaryCrossEntropy
class_weighting:
  low:   0.9
  high:  1.1
gradient_clip: 2.0
early_stopping: patience 3
