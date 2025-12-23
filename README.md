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

### Download Dataset
The **NASA C-MAPSS Dataset** is publicly available. You can download it from either of the following sources:
* [NASA Open Data Portal (Official)](https://data.nasa.gov/dataset/cmapss-jet-engine-simulated-data)
* [Kaggle Dataset Mirror](https://www.kaggle.com/datasets/behrad3d/nasa-cmaps)



### Directory Structure
Place the raw `.txt` files into the `data/` directory as follows:
```text
nasa-cmapss-hpc-feature-eng/
├── data/
│   ├── train_FD001.txt
│   ├── test_FD001.txt
│   └── RUL_FD001.txt
├── notebooks/
│   ├── 01_Baseline_model.ipynb
│   └── 02_Proposed_Ablation_study.ipynb
├── requirements.txt
└── README.md

```


### Installation
```text
**Clone the repository:**
   ```bash
   git clone [https://github.com/HarshRastogi-git/nasa-cmapss-hpc-feature-eng.git](https://github.com/HarshRastogi-git/nasa-cmapss-hpc-feature-eng.git)
   cd nasa-cmapss-hpc-feature-eng
```
### Create Environment (Ensures isolation)
```text
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### Install dependencies:
```text
 pip install -r requirements.txt
```
## ⚠️ Important: Action Required for Reproduction

> **Note:** To reproduce the results in this repository, you must manually update the data loading paths within the Jupyter Notebooks to match your local environment.

### 🛠️ Path Configuration Instructions

1.  **Open the Notebooks:** Navigate to the `notebooks/01_Baseline_model.ipynb` and `notebooks/02_Proposed_Ablation_study.ipynb` files.
2.  **Locate Data Loading Cells:** Find the initial cells where the NASA C-MAPSS `.txt` files are loaded (typically using `pd.read_csv`).
3.  **Update Absolute Paths:** Replace the existing hardcoded local paths with the **absolute path** to where you stored the dataset on your machine.
    * *Example:* Change `C:/Users/Harsh/.../train_FD001.txt` to `/your/local/path/data/train_FD001.txt`.
4.  **Parquet I/O Optimization:** In `02_Proposed_Ablation_study.ipynb`, ensure the `to_parquet` and `read_parquet` paths are also updated to a valid directory on your system. This is required to enable the **HPC-aware speedup** discussed in the paper.
