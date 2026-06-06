# HayderVGR — Reproduced Tables (this run)

_Generated 2026-06-06 00:01_

## Table 3 — Component ablation (5-fold CV)

| Configuration | Val Acc (5-fold CV) |
| --- | --- |
| VGGFace + softmax (backbone only) | 92.54 ± 2.16 |
| + BNNeck + ArcFace (dense + TTA) | 97.45 ± 0.71 |
| + cosine-kNN re-scoring (HayderVGR) | 97.64 ± 0.0 |

## Table 4 — Quantitative summary

- Val Acc (Dense+TTA): 97.45 ± 0.71
- Val Acc (kNN): 97.64 ± 0.0
- Latency GPU mean (ms): 76.96677718000046
- Latency CPU mean (ms): 182.98
- Params / size: 14.87 M / 56.72 MB

## Table 5 — Comparison with baselines (5-fold CV; Accuracy + macro Precision/Recall/F1)

| Model | Accuracy | Precision (macro) | Recall (macro) | F1 (macro) |
| --- | --- | --- | --- | --- |
| MobileNetV3Small (kNN) | 94.43 ± 1.68 | 95.89 ± 1.06 | 94.8 ± 1.43 | 94.4 ± 1.64 |
| EfficientNetB0 (kNN) | 95.09 ± 1.45 | 96.15 ± 1.11 | 95.27 ± 1.48 | 94.94 ± 1.61 |
| InsightFace ArcFace, frozen (kNN) | 91.88 ± 1.37 | 93.07 ± 1.38 | 91.85 ± 1.26 | 91.25 ± 1.42 |
| MobileFaceNet, from scratch (kNN, 3-fold) | 83.1 ± 1.82 | 86.06 ± 2.04 | 83.22 ± 1.85 | 82.91 ± 1.96 |
| ResNet50 + BNNeck + ArcFace, corrected (3-fold) | 87.16 ± 2.27 | 88.64 ± 2.06 | 87.28 ± 2.4 | 86.08 ± 2.36 |
| HayderVGR (ours), dense+TTA | 97.45 ± 0.71 | 97.73 ± 1.28 | 97.47 ± 0.76 | 97.16 ± 1.05 |
| HayderVGR (ours), kNN | 97.64 ± 0.0 | 98.04 ± 0.49 | 97.69 ± 0.1 | 97.48 ± 0.2 |

## Table 6 — Verification

- AUC: 0.9715
- EER%: 6.02
- TAR@FAR: {'0.1': 94.74, '0.01': 90.79, '0.001': 88.16}

## LFW (external)

- HayderVGR: {'acc': 86.83, 'std': 1.47, 'auc': 0.9401}  (AUC 0.9401)
- InsightFace: {'acc': 96.75, 'std': 0.53, 'auc': 0.991}
