# 🫒 Electronic Nose for Rapid Spice Quality & Adulteration Detection

[![Patent: Provisional Filed](https://img.shields.io/badge/Patent-Provisional%20Filed-brightgreen)](#-intellectual-property)
[![Conference: Synchron'25](https://img.shields.io/badge/Presented%20at-Synchron'25-blue)](#-achievements--recognition)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Hardware](https://img.shields.io/badge/Hardware-Arduino%20Mega-orange.svg)](https://www.arduino.cc/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An embedded electronic nose (E-Nose) system designed for non-destructive, rapid quality validation and adulteration detection in spices. By capturing volatile organic compound (VOC) fingerprints using a calibrated 5-sensor array and classifying them via tree-based machine learning ensembles, this project provides a low-cost alternative to standard chromatography testing.

---

## 📹 Hardware & System Demonstration

<!-- DRAG AND DROP YOUR DEMO VIDEO OR GIF DIRECTLY BELOW THIS LINE -->



https://github.com/user-attachments/assets/9f8b59f0-13b2-4fba-b9f6-bf040a28a9ee



> *Chamber airflow system drawing sample vapors over the 5-gas sensor array, baseline stabilization, and real-time inference.*

---

## 📌 Problem Statement

In spice markets, black pepper is commonly adulterated with papaya seeds due to morphological similarities, while red chilli powder is often mixed with non-edible adulterants like brick powder and industrial dyes. 

* **High Cost & Delay:** Conventional laboratory validation methods (e.g., GC-MS, HPLC) cost between **₹3,000 to ₹20,000 per sample** and require **3 to 5 business days**.
* **The Inspection Gap:** Ground inspectors and small-scale procurement agents lack portable tools to rapidly screen batches at source, allowing adulterated goods to enter supply chains.

---

## 💡 Solution Overview

This system serves as an on-ground screening bridge between field sampling and certified laboratory validation:
- **Cost-effective:** Built with low-cost components (~₹5,000 total BOM).
- **Fast Turnaround:** Delivers classification results in **under 2 minutes**.
- **Non-Destructive:** Analyzes head-space VOC vapors without chemical reagent consumption.

---

## 🛠️ System Architecture

### 1. Hardware Setup
- **Microcontroller:** Arduino Mega 2560 (10-bit multi-channel ADC capture).
- **Chamber Design:** Custom benchtop gas chamber with controlled airflow and exhaust manifold.
- **Gas Sensor Array:**
  - **MQ-135:** Air quality, ammonia, benzene, smoke.
  - **MQ-2:** Combustible gases, LPG, propane, hydrogen.
  - **MQ-7:** Carbon monoxide detection.
  - **TGS 2600:** Low-concentration general air contaminants/VOCs.
  - **TGS 2602:** High-sensitivity VOCs, sulfur compounds, organic solvents.

### 2. Signal Preprocessing & Anti-Drift Pipeline
Metal oxide semiconductor (MOS) sensors are prone to thermal drift and environmental variations. To stabilize readings:
- **Baseline Correction:** Captures clean air reference values ($R_0$) before each test cycle.
- **Adaptive Filtering:** Combines rolling median filtering (impulse noise rejection) with exponential smoothing.
- **Relative Transformations:** Raw voltage channels ($v_i$) are converted to relative ratios:
  $$\text{Relative Ratio } (r_i) = \frac{v_i - \text{baseline}_i}{\text{baseline}_i}$$



---

## 📊 Dataset & Feature Engineering

Because public sensor datasets for spice adulteration do not exist, the dataset was **handcrafted and recorded across real testing sessions**:

* **Feature Set:**
  - Relative responses ($r_{135}, r_2, r_{2602}, r_{2600}, r_7$)
  - Cross-sensor interaction ratios (e.g., $r_{2602} / r_{2600}$, $r_{135} / r_2$)
  - Rolling window statistical moments (Mean, Standard Deviation, Deltas)
* **Target Classes:**
  1. `Pure Pepper`
  2. `Pepper + Papaya Adulterated Mix`
  3. `Clean Air Reference`

---

## 🤖 Machine Learning Model & Performance

The classification engine relies on tree ensemble models tuned via `StratifiedKFold` cross-validation:

| Metric | Random Forest / XGBoost |
| :--- | :--- |
| **CV Macro F1-Score** | **~0.85+** |
| **Held-Out Test F1-Score** | **0.87** |
| **Response Latency** | **< 120 seconds** |
| **Evaluation Strategy** | Chronological session splits & 5-fold Stratified CV |

---



## 🏆 Achievements & Recognition

- **Symposium Presentation:** Presented at **Synchron'25** (MIT Manipal).
- **Intellectual Property:** **Provisional Patent Filed** for the multi-sensor detection design and adaptive classification pipeline.

---
