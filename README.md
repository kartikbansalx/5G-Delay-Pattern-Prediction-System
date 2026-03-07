# 5G Delay Pattern Prediction System

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)
![TensorFlow](https://img.shields.io/badge/TensorFlow-LSTM-orange?logo=tensorflow)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-purple)

> **Forecasting 5G network latency (IP Delay) using machine learning — with a hybrid multi-step recursive LSTM approach for long-term prediction accuracy.**

---

## 📌 Overview

This project focuses on forecasting **5G network latency**, specifically **IP Delay**, using time-series machine learning models. The dataset consists of **40,000 data packets** collected from the **ExPECA testbed at KTH, Sweden**.

By studying packet traces across the IP, RLC, MAC, and Physical layers, the project identifies the primary cause of the triangular pattern observed in Delay vs. SN (Sequence Number) graphs — **Frame Alignment Delay in the RLC layer** — and builds predictive models to forecast this behavior.

---

## 🔬 Research Findings

- Analyzed packet traces and built understanding of protocol concepts across **IP, RLC, MAC, and Physical layers**.
- Concluded that the main reason for the **triangular pattern in Delay vs. SN graphs** is due to **Frame Alignment Delay in the RLC layer**.
- Constructed and compared various neural network models: **RNN**, **LSTM**, and **ARIMA**.
- Evaluated prediction strategies: **Single-Step**, **Multi-Step**, and a **Hybrid** approach.
- **Best results** were achieved using an **LSTM-based Hybrid Multi-Step Recursive approach**.

---

## 🧠 Model Architecture

| Component       | Details                     |
|----------------|-----------------------------|
| Model Type      | LSTM (Long Short-Term Memory) |
| LSTM Units      | 50                          |
| Output Layer    | Dense (1 unit)              |
| Optimizer       | Adam                        |
| Loss Function   | Mean Squared Error (MSE)    |
| Sequence Length | 30 timesteps                |
| Normalization   | Min-Max Scaling             |

---

## ⚙️ Hybrid Multi-Step Recursive Approach

The core innovation of this project is a **Hybrid Recursive Forecasting Strategy** that balances prediction realism with long-term stability.

```
┌─────────────────────────────────────────────────────────────┐
│           Hybrid Multi-Step Recursive Prediction            │
│                                                             │
│  Step 1: Feed 30 real (ground truth) values as input        │
│                        │                                    │
│  Step 2: Predict next 10 values recursively                 │
│          → Predict 1st value                                │
│          → Append to sequence, drop oldest                  │
│          → Repeat × 10                                      │
│                        │                                    │
│  Step 3: RESET — use next 30 real values as new input       │
│                        │                                    │
│  Step 4: Repeat for entire dataset                          │
└─────────────────────────────────────────────────────────────┘
```

### Why Hybrid?

| Approach | Problem |
|---|---|
| Pure Recursive | Error accumulates → phase shift, large drift |
| Hybrid (this work) | Predicts in small blocks of 10, resets with real data regularly → **reduced long-term drift** |

> **Key concept:** Predict small future blocks → Reset with real data → Repeat.

---

## 📁 Dataset

- **Source:** ExPECA Testbed, KTH Royal Institute of Technology, Sweden
- **Size:** ~40,000 data packets
- **Features:** IP Delay, RLC layer timing, sequence numbers, packet traces
- **Files:** `TR1.csv` (training), `TE1.csv` (testing)

---

## 📈 Results Summary

The LSTM-based Hybrid Multi-Step Recursive model outperformed standard RNN, ARIMA, and plain recursive LSTM approaches in long-term forecasting, particularly in:

- Reducing prediction **phase shift**
- Minimizing **error accumulation** over long sequences
- Accurately capturing the **triangular Delay vs. SN pattern**

---

