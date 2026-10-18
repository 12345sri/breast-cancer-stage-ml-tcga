# Breast cancer stage classification from gene expression (TCGA-BRCA)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23456699.svg)](https://doi.org/10.5281/zenodo.23456699)

Code for the manuscript:

> **An Interpretable Machine Learning Framework for Early- versus Late-Stage Classification of Breast Cancer from Gene Expression, with Co-expression Network Analysis**
> Sriporna Biswas, Tanjidul Huda, Hema Pal and Vinod Kumar Gupta.

This repository contains the analysis notebook that produces all tables and figures reported in the manuscript, the
reference outputs from that run, and Supplementary Table S4.

> For research use only. The models described here are not clinical tools and must not be used for diagnosis or treatment decisions.

## What the analysis does

The study asks how much information about anatomical stage (AJCC Stage I–II vs III–IV) is carried by the expression of an
a priori panel of 65 cancer-related genes in primary breast tumours from TCGA-BRCA.

1. **Cohort:** primary tumours (TCGA sample type 01), one per patient; tumours with Stage X, discrepant or missing stage are
   excluded. This leaves 1,075 tumours (805 early, 270 late).
2. **Split:** stratified 80/20 train/test split (860 / 215), random seed 42.
3. **Models:** logistic regression, SVM (RBF kernel), random forest and gradient boosting (scikit-learn
   `HistGradientBoostingClassifier`).
4. **Preprocessing:** median imputation, z-scaling and SMOTE, all inside a pipeline fitted on training data only.
5. **Tuning and thresholds:** grid search with stratified 5-fold cross-validation (ROC-AUC); decision thresholds from
   out-of-fold predictions (Youden's J).
6. **Evaluation:** a single evaluation on the held-out test set, reporting ROC-AUC and balanced accuracy with 1,000-sample
   bootstrap 95% CIs, plus PR-AUC, sensitivity, specificity, F1 and MCC, and DeLong tests between models.
7. **Sensitivity analysis:** class weighting instead of SMOTE (Supplementary Table S3).
8. **Gene-level analyses:**
   - permutation importance for the gradient-boosting model;
   - Mann–Whitney differential expression with Benjamini–Hochberg FDR across all 1,075 tumours;
   - a Pearson co-expression network with hub genes, built from the 860 training tumours.

Key results, reproduced by the notebook:

| Quantity | Value |
|---|---|
| Logistic regression, test ROC-AUC (95% CI) | 0.618 (0.530–0.704) |
| Logistic regression, balanced accuracy / sensitivity / specificity | 0.578 / 0.741 / 0.416 |
| Other models, test ROC-AUC | SVM 0.595, gradient boosting 0.594, random forest 0.589 |
| DeLong p (logistic regression vs others) | 0.199–0.585 |
| Class-weighted logistic regression, test ROC-AUC (sensitivity analysis) | 0.651 |
| Genes with FDR < 0.05 | none (smallest FDR 0.057, *CD44*) |
| Top hub genes (\|r\| ≥ 0.5 network) | *REL* (11), *EP300* (10), *ESR1* (9), *GATA3* (8), *PIK3CA* (7) |

## Repository contents

```
notebooks/breast_cancer_stage_analysis.ipynb   analysis notebook (run this)
notebooks/archive/                             notebook as originally used, kept unchanged for provenance
data/README.md                                 how to obtain the input file (data are not redistributed here)
reference_results/                             CSV/JSON outputs of the verified run, for comparison
supplementary/                                 Supplementary Table S4 (xlsx with unrounded values; pdf)
requirements.txt                               pinned package versions used for the verified run
CITATION.cff, .zenodo.json                     citation and archive metadata
LICENSE                                        MIT licence for the code
```

## How to reproduce

1. Use Python 3.13. Then install the pinned packages:
   ```bash
   pip install -r requirements.txt
   ```
2. Obtain the input file `denseDataOnlyDownload-3.tsv` as described in [`data/README.md`](data/README.md). Then do one of
   the following:
   - place it in `data/`;
   - set `BRCA_DATA=/path/to/file.tsv`;
   - in Google Colab, upload it when prompted.
3. Open `notebooks/breast_cancer_stage_analysis.ipynb` and run all cells from the repository root, for example with
   `jupyter lab`, or open it in Colab.

The notebook takes about 5–10 minutes. It writes all tables and 300-dpi figures (Figures 1–8) to `results/`. Its final cell
compares the key numbers with the values reported in the manuscript and prints `ALL CHECKS PASSED` when they agree.

## Reproducibility notes

- **Verified run.** The notebook was executed end to end in Python 3.13.16 with the versions in `requirements.txt`. Every
  output table matched the manuscript run exactly, and every figure was pixel-identical. Installation of these exact
  versions in other environments (for example Google Colab) has not been tested.
- **Fixed implementation choices.** The manuscript results were produced with a built-in SMOTE implementation (in the
  notebook), scikit-learn's `HistGradientBoostingClassifier` and permutation importance. The notebook always uses these
  components, so installing `imbalanced-learn`, `xgboost` or `shap` does not change the results. Earlier development
  versions of the notebook switched to those libraries when they were installed, which would have produced different
  numbers.
- **Input file.** The notebook checks the SHA-256 checksum of the input file and warns if it differs from the file used for
  the manuscript.
- **Non-nested cross-validation.** Cross-validated AUCs are computed on the same folds used for tuning (no nested
  cross-validation), as stated in the manuscript.

## Data

The expression and clinical data are public TCGA-BRCA data obtained from the
[UCSC Xena browser](https://xenabrowser.net) (TCGA hub; expression dataset `TCGA.BRCA.sampleMap/HiSeqV2_PANCAN`, phenotype
dataset `TCGA.BRCA.sampleMap/BRCA_clinicalMatrix`). They are not redistributed in this repository; see
[`data/README.md`](data/README.md). Please cite TCGA and UCSC Xena when using the data.

## Citation

If you use this code, please cite the manuscript and this software. Citation metadata are in [`CITATION.cff`](CITATION.cff).

> Biswas S, Huda T, Pal H, Gupta VK. Code for: An Interpretable Machine Learning Framework for Early- versus Late-Stage
> Classification of Breast Cancer from Gene Expression, with Co-expression Network Analysis (v1.0.0). Zenodo; 2026.
> https://doi.org/10.5281/zenodo.23456699

## Licence

Code: MIT licence (see [`LICENSE`](LICENSE)). Supplementary Table S4 and the reference results are derived from public
TCGA data.

## Contact

Sriporna Biswas, Department of Computer Science & Engineering, Chandigarh University, Punjab, India —
sripornab13@gmail.com
