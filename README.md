# HayderVGR

**Face-Pretrained VGG16 with BNNeck and ArcFace Angular-Margin Learning for Student Face Recognition**

HayderVGR is a compact face-recognition model for closed-set student identification from small image sets. It pairs a **face-domain pretrained (VGGFace) VGG16** backbone with **angular-margin metric learning** (BNNeck + ArcFace), and refines predictions at inference with **test-time augmentation (TTA)** and **cosine k-nearest-neighbour (kNN) re-scoring**.

> Research code accompanying the manuscript *"HayderVGR: Face-Pretrained VGG16 with BNNeck and ArcFace Angular-Margin Learning for Student Face Recognition"* (under review).

---

## Method at a glance

```
Input (256x256x3)
  -> VGG16 backbone (VGGFace-pretrained, 5 conv stages, progressive unfreezing)
  -> Global Average Pooling -> FC(256, ReLU)
  -> BNNeck (BatchNorm, scale=False)          # 256-D embedding
  -> ArcFace head (CosineDense, s=32, margin 0.20 -> 0.35)
Inference refinement: 10x TTA  +  cosine kNN (k=5, T=0.7) on the BNNeck embedding
```

Training: AdamW, warmup + cosine learning-rate schedule, MixUp (alpha=0.2), light RandAugment, four-phase progressive unfreezing with an adaptive ArcFace margin.

---

## Results (DS-SID, five-fold stratified cross-validation)

| Configuration | Validation accuracy |
|---|---|
| VGGFace + softmax (backbone only) | 95.56% ± 0.91% |
| + BNNeck + ArcFace (dense) | 97.17% ± 0.75% |
| + test-time augmentation (TTA) | 97.36% ± 0.65% |
| + cosine kNN (full HayderVGR) | **97.45% ± 0.98%** |

**Verification:** AUC = 0.994, EER = 2.41%, TAR@FAR = 98.03% / 96.71% / 91.45% (FAR = 0.1 / 0.01 / 0.001).
**Single-image latency:** ~68 ms (batch size 1, NVIDIA T4).

**Baselines (identical protocol):**

| Model | Validation accuracy |
|---|---|
| MobileNetV3Small | 93.48% ± 0.49% |
| EfficientNetB0 | 94.15% ± 1.45% |
| InsightFace ArcFace (off-the-shelf, frozen + kNN) | 91.98% ± 1.60% |
| ResNet50 + BNNeck + ArcFace (corrected margin warmup) | 88.57% ± 1.94% |

**External public benchmark (LFW verification):** HayderVGR embedding 85.07% ± 1.88% (AUC 0.921); InsightFace ArcFace 96.75% ± 0.53% (AUC 0.991) under the same pipeline. HayderVGR is optimized for the closed-set institutional task, so its LFW score reflects embedding transfer to unseen identities rather than its target objective.

A component ablation shows the metric-learning head (BNNeck + ArcFace) is the principal source of improvement; an auxiliary squeeze-and-excitation block was found unnecessary and removed.

---

## Repository contents

```
.
├── v6_2_Hyper_VGG16_VGGFace.ipynb   # full pipeline (train, CV, verification, comparisons, figures)
├── requirements.txt
├── LICENSE
├── .gitignore
└── README.md
```

The notebook cells:
- **0** — model + training (build, four-phase fine-tuning).
- **1** — five-fold stratified cross-validation (mean ± std).
- **2** — verification metrics (AUC, EER, TAR@FAR) + ROC.
- **3** — dedicated face-model comparison (InsightFace ArcFace) on DS-SID.
- **4** — LFW public-benchmark verification.
- **5** — ResNet50 baseline re-validation.
- **6** — learning curves + quantitative error analysis (confusion matrix, distributions).

---

## Setup

Designed for **Google Colab** (GPU, e.g. NVIDIA T4). For a local run:

```bash
pip install -r requirements.txt
```

The VGGFace VGG16 weights are downloaded automatically at runtime via `tf.keras.utils.get_file`.

---

## How to run (Colab)

1. Open `v6_2_Hyper_VGG16_VGGFace.ipynb` in Colab and select a GPU runtime.
2. Mount Google Drive and set `DRIVE_BASE_DIR` and the dataset path in cell 0.
3. Run cell 0 (training), then cells 1–6 as needed (each prints its results).

---

## Dataset (DS-SID)

DS-SID is an institutional student-face dataset (100 enrolled identities; experiments use the available classes after excluding low-quality samples; 8–12 images per identity).

**The dataset is not redistributed** for privacy and consent reasons (images were collected under informed consent for research use, stored with restricted access, and labelled numerically). To reproduce with your own data, arrange images as one folder per identity:

```
DS-SID/
├── 1/  img001.jpg img002.jpg ...
├── 2/  ...
└── N/  ...
```

---

## Reproducibility

Fixed random seeds and five-fold stratified cross-validation are used throughout; per-fold results and standard deviations are reported by the notebook.

---

## Ethics

Face images of students were collected with informed consent for research purposes, stored under restricted access, and identified by numeric labels only. No identifiable face images are released in this repository or in the associated paper.

---

## Citation

```bibtex
@article{haydervgr2026,
  title   = {HayderVGR: Face-Pretrained VGG16 with BNNeck and ArcFace Angular-Margin Learning for Student Face Recognition},
  author  = {HayderVGR Authors},
  journal = {Under review},
  year    = {2026}
}
```

## License

Code released under the MIT License (see `LICENSE`). The DS-SID dataset is **not** covered by this license and is not included.
