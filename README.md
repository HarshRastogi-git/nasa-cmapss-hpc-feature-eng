# HPC-Aware Feature Engineering for Predictive Maintenance (NASA C-MAPSS)

[![Paper Status](https://img.shields.io/badge/Status-Under--Review-orange)](./paper/768_MEDCOM2025.pdf)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## 📌 Project Overview
This repository hosts a high-performance machine learning pipeline for predicting the **Remaining Useful Life (RUL)** of turbofan engines using the NASA C-MAPSS dataset. 

The core contribution is an **HPC-aware feature engineering framework** that prioritizes both predictive precision and computational throughput. By moving beyond standard rolling statistics and implementing parallelized extraction, this pipeline achieves state-of-the-art efficiency for sensor-stream data.

### 🚀 Key Results (FD001 Dataset)
* **Accuracy:** **23.1% Reduction in RMSE** compared to standard statistical baselines.
* **Efficiency:** **88% Reduction in Wall-Clock Time** for feature extraction via multi-core parallelization.
* **Architecture:** Optimized for high-throughput I/O using **Apache Parquet** and memory-efficient `float32` precision.

---

## 🔬 Scientific Contribution

### 1. Mathematical Innovation ($f_{slope}$ & $LF-ER$)
We introduce two novel window-based features designed to capture non-linear engine degradation patterns:
* **$f_{slope}$ (Normalized Trend Slope):** A robust estimator of the local degradation rate, normalized to account for varying engine lifespans.
* **$LF-ER$ (Low-Frequency Spectral Energy Ratio):** Utilizes Fast Fourier Transform (FFT) to quantify the energy shift in the low-frequency spectrum, identifying early-stage mechanical fatigue.

### 2. HPC-Aware Design Patterns
* **Per-Run Parallelism:** Leverages `joblib` to process independent engine trajectories across all available CPU cores.
* **Columnar Caching:** Replaced traditional CSV-based I/O with Parquet storage, significantly reducing latency for high-dimensional feature matrices.



---

## 📊 Experimental Performance
| Pipeline | Test RMSE | Test MAE | Preprocessing (Serial) | Preprocessing (Parallel) |
| :--- | :---: | :---: | :---: | :---: |
| **Baseline (Rolling Stats)** | 29.01 | 21.45 | 1.0x | N/A |
| **Proposed (HPC-Aware)** | **22.35** | **15.92** | 1.2x | **8.3x Speedup** |

---

## 📜 Publication Status
The methodology, rigorous validation (using `GroupKFold`), and findings are detailed in the technical report: 

**"HPC-Aware Feature Engineering for Predictive Maintenance Using NASA C-MAPSS FD001"** *Harsh Rastogi, et al. (2025)* **Submitted to MEDCOM 2025 (Pending Publication)**

📄 [**Read the Full Technical Report (PDF)**](./paper/768_MEDCOM2025.pdf)

---

## 🛠️ Reproduction & Installation

### Prerequisites
* Python 3.8+
* Conda or Virtualenv (recommended)

### Download
Download the dataset from the [NASA Open Data Portal](https://data.nasa.gov/Aerospace/CMAPSS-Jet-Engine-Simulated-Data/699s-3j6v).

### Directory Structure
Place the raw `.txt` files into the `data/` directory as follows:
nasa-cmapss-hpc-feature-eng/
├── data/
│   ├── train_FD001.txt
│   ├── test_FD001.txt
│   └── RUL_FD001.txt

```text

### Installation
1. **Clone the repository:**
   ```bash
   git clone [https://github.com/HarshRastogi-git/nasa-cmapss-hpc-feature-eng.git](https://github.com/HarshRastogi-git/nasa-cmapss-hpc-feature-eng.git)
   cd nasa-cmapss-hpc-feature-eng
   pip install -r requirements.txt
