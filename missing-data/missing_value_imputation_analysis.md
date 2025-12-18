> 📓 **Companion Notebook**  
> [View Jupyter Notebook](Handling_MCAR.ipynb)

 # Handling Missing Vlaues of MCAR type and Stability Analysis

This document summarizes the comparison of multiple **MCAR-compatible imputation techniques** applied to a randomly sampled **Income** dataset containing missing values. The goal is to identify the **most stable and usable imputation method** based on distribution preservation.

---

## Dataset Context
- Feature: **Income** (numeric)
- Missingness: **MCAR (assumed)**
- Evaluation metrics: **Mean** and **Median after imputation**

---

## Results Summary

| Method | Mean after Imputation | Median after Imputation | Stability Observation |
|---|---:|---:|---|
| Original Income | 62006.55 | 66576.00 | Baseline distribution |
| Mean Imputation | 62006.55 | 62006.55 | Median collapses to mean ❌ |
| Median Imputation | 62691.97 | 66576.00 | Median preserved ✅ |
| Random Sampling | 65503.27 | 70932.00 | Large shift, unstable ❌ |
| SimpleImputer (mean) | 62006.55 | 62006.55 | Same as mean imputation ❌ |
| SimpleImputer (median) | 62691.97 | 66576.00 | Same as median imputation ✅ |

---

## Interpretation

### Mean Imputation
- Preserves the mean but **distorts the median**.
- Repeated insertion of a constant value reduces variance.
- Not ideal for skewed income data.

### Median Imputation (Recommended)
- **Preserves the median**, which is critical for income distributions.
- Slight mean shift is acceptable.
- Robust to outliers and skewness.

### Random Sampling Imputation
- Intended to preserve distribution.
- In this dataset, it **oversampled higher values**, inflating both mean and median.
- Unstable for small/medium datasets.

### SimpleImputer
- Pipeline-safe wrapper.
- Behavior identical to manual mean/median strategies.

---

## Final Verdict

 **Most Stable & Usable Method:**

> **Median Imputation** (`SimpleImputer(strategy="median")`)

**Reasons:**
- Income data is right-skewed.
- Median is resistant to outliers.
- Preserves the true center of the distribution.
- Works well under MCAR and weak MAR assumptions.

---

## Key Rule of Thumb

> If `mean ≠ median`, the data is skewed --- **prefer median over mean**.


---

## Reproducibility Notes
- `SimpleImputer(strategy='median')` is recommended for production pipelines.
- Always validate impact using distribution plots (KDE/boxplots) and cross-validation.

---




