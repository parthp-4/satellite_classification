# Pixel-Level Land Cover Classification -- Report Summary

## 1. Study Overview

| Parameter | Value |
|-----------|-------|
| Satellite | Sentinel-2 L2A |
| Training region | Jaipur-Ajmer & Bikaner, Rajasthan |
| Validation region | Chandrapur, Maharashtra |
| Training tiles | 20 |
| Validation tiles | 10 |
| Image size | 512 x 512 pixels |
| Land cover classes | 7 |

## 2. Land Cover Classes

| ID | Class | RGB Colour |
|----|-------|------------|
| 0 | Hills/Rocky | (139, 69, 19) |
| 1 | Crop Fields | (34, 139, 34) |
| 2 | Fallow Land | (255, 215, 0) |
| 3 | Water Body | (0, 0, 255) |
| 4 | Sandy River | (255, 165, 0) |
| 5 | Plantation | (0, 100, 0) |
| 6 | Built-up | (255, 0, 0) |

## 3. Methodology

### Pseudo-label Generation
- SLIC superpixel segmentation (n_segments=400, compactness=8)
- KMeans clustering (k=7) on per-segment statistics (mean, std, median, percentiles, GLCM texture features)

### Models

**Random Forest** -- pixel-level classifier trained on hand-crafted features (intensity, gradient, LBP, multi-scale statistics, GLCM). 120 trees, max depth 18, balanced class weights.

**CNN Patch Classifier** -- 32x32 patch classification, 3-block encoder, 12 epochs, batch 64.

**U-Net Segmentation** -- 4-level encoder-decoder + skip connections, weighted CE loss, 15 epochs, batch 4.

**Ensemble** -- majority-vote across all trained models.

## 4. Cross-Model Pixel Agreement

| Comparison | Agreement |
|------------|-----------|
| RF vs CNN | 22.61% |
| RF vs U-Net Scratch | 20.84% |
| RF vs U-Net Pretrained | 13.87% |
| RF vs Ensemble | 35.07% |
| CNN vs U-Net Scratch | 28.86% |
| CNN vs U-Net Pretrained | 26.93% |
| CNN vs Ensemble | 50.05% |
| U-Net Scratch vs U-Net Pretrained | 23.96% |
| U-Net Scratch vs Ensemble | 75.59% |
| U-Net Pretrained vs Ensemble | 41.20% |

## 5. Class Distribution Summary (%)

| Class       |   CNN |   Ensemble |    RF |   U-Net Pretrained |   U-Net Scratch |
|:------------|------:|-----------:|------:|-------------------:|----------------:|
| Built-up    |  6.22 |       0.13 |  5.99 |               2.36 |            0    |
| Crop Fields | 20.05 |      11.5  | 10.86 |              46.41 |            0    |
| Fallow Land | 22.67 |       9.02 | 13.09 |              26.92 |            1.27 |
| Hills/Rocky | 29.29 |      75.76 | 20.91 |              24.07 |           98.73 |
| Plantation  |  8.23 |       1.36 | 23.06 |               0    |            0    |
| Sandy River |  7.79 |       1.06 |  4.9  |               0.2  |            0    |
| Water Body  |  5.75 |       1.17 | 21.18 |               0.05 |            0    |

## 6. Pseudo-Label Noise Rate Analysis

KMeans was run 7 times with different random seeds. Label permutations were aligned via the Hungarian algorithm. Per-class noise rate = 1 - average fraction of runs assigning the same label to a pixel.

| Class | Stability (%) | Noise Rate (%) | Pixel Count |
|-------|--------------|----------------|-------------|
| Hills/Rocky | 86.0 | 14.0 | 187,574 |
| Crop Fields | 91.0 | 8.9 | 236,522 |
| Fallow Land | 91.3 | 8.7 | 211,681 |
| Water Body | 89.8 | 10.2 | 123,295 |
| Sandy River | 88.7 | 11.3 | 251,914 |
| Plantation | 96.8 | 3.2 | 183,542 |
| Built-up | 91.7 | 8.3 | 116,192 |

**Most noisy class:** Hills/Rocky -- spectral ambiguity in single-band NIR causes inconsistent KMeans clustering, directly degrading model performance.
**Most stable class:** Plantation -- spectrally distinct NIR signature yields reliable pseudo-labels regardless of KMeans initialisation.

## 7. Temporal Change Detection

*Not run. To enable: add dated subfolders under data/temporal_grids/ (see Cell 7B for instructions).*

## 8. Output Artefacts

- Classified GeoTIFFs: `/content/drive/MyDrive/satellite_classification/outputs/results/classified_tifs`
- Figures:             `/content/drive/MyDrive/satellite_classification/outputs/visualizations`
- CSV summaries:       `/content/drive/MyDrive/satellite_classification/outputs/reports`
- Noise stats CSV:     `/content/drive/MyDrive/satellite_classification/outputs/reports/pseudolabel_noise_stats.csv`