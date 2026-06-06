# HayderVGR — Reproducibility Package

This package was produced by a single end-to-end script that trains and
evaluates every result reported in the manuscript on the DS-SID dataset.

**Contents**
- `paper_numbers.json` — all computed metrics (per fold) + the manuscript's reported values.
- `tables.md` — Tables 3-6 regenerated from this run.
- `figures/` — ROC, confusion matrix, learning curves, error-analysis plots (300 DPI).
- `verification_raw_scores.npz` — raw genuine/impostor cosine scores (re-draw the exact ROC).

Model: VGGFace-pretrained VGG16 + BNNeck + ArcFace (no SE, no ResNet fusion).
Protocol: StratifiedKFold(seed=42); 256x256 inputs; ArcFace s=32, margins (0.20,0.25,0.25,0.35);
AdamW + warmup-cosine; MixUp(0.2) + RandAug-light; TTA(10)+flip; cosine kNN (k=5, T=0.7).
