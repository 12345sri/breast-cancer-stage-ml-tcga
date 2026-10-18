# Archived notebook (provenance only)

`Breast_Cancer_Stage_Paper_original.ipynb` is the notebook as originally used during the study. It is kept unchanged for
provenance. **Use `../breast_cancer_stage_analysis.ipynb` to reproduce the manuscript.**

Known issues of the archived version, all fixed in the main notebook without changing any result:

- It switches to `imbalanced-learn`, `xgboost` and `shap` when these are installed (as in Google Colab), which gives
  results different from those reported. The manuscript results came from its fallback path: built-in SMOTE,
  `HistGradientBoostingClassifier` and permutation importance.
- It contains Colab-only cells (`!pip install`, `google.colab.files`) that fail elsewhere.
- It expects a local Word template and contains a manuscript-drafting section that is not part of the analysis.
- It imports `available_if` from a private scikit-learn module (`sklearn.utils._available_if`).
