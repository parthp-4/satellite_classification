# Deep Learning Workflow for Geo-Spatial Data Classification

> Pixel-level land cover classification of Sentinel-2 L2A satellite imagery using Random Forest, CNN, and U-Net deep learning models trained on Rajasthan and validated on Maharashtra.

**Authors:** Parth Porwal · Yashvi Solanki
**Research Internship:** BIT Mesra — Geospatial Deep Learning (2025–2026)

---

# 📌 Overview

This project implements an end-to-end land cover classification workflow for Sentinel-2 L2A satellite imagery using pseudo-label generation and deep learning techniques.

Instead of manually annotating millions of pixels, the workflow automatically generates training labels using:

* SLIC Superpixel Segmentation
* KMeans Clustering
* Texture & Spectral Feature Engineering

The generated labels are then used to train and compare:

* Random Forest
* CNN
* U-Net (Scratch)
* U-Net (Pretrained ResNet34)
* Ensemble Model

The project evaluates how well these models generalize across geographically separated regions.

---

# 🚀 Quick Start

## Main Files

| File                                             | Purpose                       |
| ------------------------------------------------ | ----------------------------- |
| `notebooks/Implementation_6.ipynb`               | Final implementation notebook |
| `notebooks/Implementation_6.html`                | Rendered notebook version     |
| `outputs/reports/classification_report.md`       | Generated analysis report     |
| `outputs/reports/class_distribution_summary.csv` | Class distribution statistics |

---

# ⚠️ GitHub Notebook Preview Notice

GitHub occasionally fails to render Jupyter notebooks even when the notebook file itself is valid.

If GitHub shows:

```text
An error occurred
Using nbformat...
```

open:

```text
notebooks/Implementation_6.html
```

or download and run:

```text
notebooks/Implementation_6.ipynb
```

locally.

---

# 🏆 Project Highlights

| Metric               | Value                                |
| -------------------- | ------------------------------------ |
| Training Region      | Rajasthan                            |
| Validation Region    | Maharashtra                          |
| Training Tiles       | 20                                   |
| Validation Tiles     | 10                                   |
| Land Cover Classes   | 7                                    |
| Models Compared      | 5                                    |
| Pseudo-Labelling     | SLIC + KMeans                        |
| Deep Learning Models | CNN, U-Net Scratch, U-Net Pretrained |
| Classical ML Model   | Random Forest                        |
| Evaluation Strategy  | Cross-region validation              |

---

# 🗺️ Land Cover Classes

| ID | Class         |
| -- | ------------- |
| 0  | Hills / Rocky |
| 1  | Crop Fields   |
| 2  | Fallow Land   |
| 3  | Water Body    |
| 4  | Sandy River   |
| 5  | Plantation    |
| 6  | Built-up      |

---

# 📁 Repository Structure

```text
geospatial-landcover-classification/
├── notebooks/
│   ├── Implementation_6.ipynb
│   └── Implementation_6.html
│
├── data/
│   ├── training_grids/
│   └── validation_grids/
│
├── models/
│   ├── .gitkeep
│   └── saved_models/
│       └── .gitkeep
│
├── outputs/
│   ├── analysis/
│   │   └── .gitkeep
│   │
│   ├── reports/
│   │   ├── class_distribution_summary.csv
│   │   ├── classification_report.md
│   │   └── pseudolabel_noise_stats.csv
│   │
│   └── visualizations/
│       └── .gitkeep
│
├── README.md
├── .gitignore
└── .gitattributes
```

---

# 🔬 Methodology

## Step 1 — Data Acquisition

Sentinel-2 L2A imagery was downloaded from the Copernicus Data Space Ecosystem.

Scenes used:

| Region                  | Tile   |
| ----------------------- | ------ |
| Jaipur-Ajmer, Rajasthan | T43RDK |
| Bikaner, Rajasthan      | T43RCM |
| Chandrapur, Maharashtra | T44QLH |

---

## Step 2 — Tiling

Large Sentinel scenes were divided into:

* 512 × 512 pixel GeoTIFF tiles
* 5 pixel overlap
* Generated using SAGA GIS

Result:

* 20 training tiles
* 10 validation tiles

---

## Step 3 — Pseudo-Label Generation

Labels are generated automatically using:

### SLIC Superpixels

* 400 segments
* Compactness = 8

### KMeans Clustering

* k = 7 classes

No manual annotation is required.

---

## Step 4 — Feature Engineering

Features extracted:

* NIR Intensity
* Sobel Gradient Magnitude
* Local Binary Patterns (LBP)
* Multi-scale Mean
* Multi-scale Standard Deviation
* GLCM Texture Features

Total Feature Dimension:

```text
13 features per pixel
```

---

## Step 5 — Model Training

| Model            | Type                 |
| ---------------- | -------------------- |
| Random Forest    | Classical ML         |
| CNN              | Patch Classification |
| U-Net Scratch    | Segmentation         |
| U-Net Pretrained | Transfer Learning    |
| Ensemble         | Majority Voting      |

---

# 📊 Key Findings

### Random Forest

* Most stable across domain shift
* Successfully identified all 7 classes

### CNN

* Moderate generalisation
* Better balance than U-Net Scratch

### U-Net Scratch

* Severe class collapse
* Predicted Hills/Rocky for most pixels

### U-Net Pretrained

* Faster convergence
* Better feature extraction
* Border artefacts due to domain mismatch

### Ensemble

* Combined strengths of all models
* Reduced prediction variance

---

# 🛠️ Dependencies

Core libraries:

```bash
tensorflow
segmentation-models
rasterio
scikit-learn
scikit-image
opencv-python
numpy
pandas
matplotlib
tqdm
tabulate
```

---

# 🚀 Running the Project

## Google Colab

1. Open `Implementation_6.ipynb`
2. Mount Google Drive
3. Select T4 GPU Runtime
4. Run all cells

---

## Local Environment

```bash
git clone https://github.com/parthp-4/geospatial-landcover-classification.git

cd geospatial-landcover-classification

pip install tensorflow rasterio scikit-image scikit-learn opencv-python segmentation-models tqdm tabulate

jupyter notebook notebooks/Implementation_6.ipynb
```

---

# 🗂️ Data Access

The following are intentionally excluded from GitHub:

```text
data/training_grids/
data/validation_grids/
models/saved_models/
outputs/visualizations/
```

Reason:

* Large GeoTIFF datasets
* Trained model checkpoints
* Generated visual outputs

These files are stored separately in Google Drive.

---

# 📄 Citation

```text
Porwal, P. & Solanki, Y. (2026).

Deep Learning Workflow for Geo-Spatial Data Classification.

BIT Mesra Geospatial Deep Learning Research Internship.

GitHub:
https://github.com/parthp-4/geospatial-landcover-classification
```

---

# 📜 License

This repository is intended for academic and research purposes.

Sentinel-2 imagery is provided by ESA under the Copernicus Open Data License.
