# Methodological Rigour Review: `final-draft-code.ipynb`

**Reviewer:** sk
**Date:** 2026-05-09
**Commit reviewed:** ffebd69 (uais — "final draft of code")

---

## Overview

This document evaluates the methodological rigour of the AMR (Antimicrobial Resistance) prediction pipeline implemented in `final-draft-code.ipynb`. The notebook covers data integration from multiple surveillance databases (ATLAS, SIDERO-WT, KEYSTONE), macro-economic feature engineering, multi-model training and comparison, calibration, and application to Kazakhstan via a Leave-One-Country-Out (LOCO) framework.

---

## Strengths

### 1. Temporal Split Design

- Train ≤2018, Validation 2019–2020, Test >2020.
- Appropriate for time-series AMR surveillance data — prevents future-to-past information leakage in the primary split.
- LOCO for Kazakhstan correctly excludes the target country from training.

### 2. Multi-Model Comparison with Statistical Testing

- 8+ models compared: CatBoost, LightGBM, XGBoost, MLP, Logistic Regression, Random Forest, NGBoost, AutoGluon.
- DeLong test with Bonferroni correction for pairwise AUC significance.
- Stratified bootstrap confidence intervals (n=1000) for AUC, AUPRC, and Brier score.

### 3. Calibration Assessment

- Expected Calibration Error (ECE) and Hosmer-Lemeshow test reported.
- Isotonic recalibration fitted on validation set, applied to test set — proper held-out calibration procedure.

### 4. Clinical Plausibility Filter

- EUCAST-derived organism–antibiotic validity matrix prevents biologically nonsensical predictions (e.g., Vancomycin against gram-negatives filtered out).
- Transparent reporting of rows removed.

### 5. Class Imbalance Handling

- `scale_pos_weight` computed per training group, not globally.
- Applied consistently across all CatBoost runs.

### 6. LOCO Framework for Generalizability

- Multiple reference groups tested: post-Soviet, structural peers, all-except-KZ.
- Two-stage training (early stopping then full retrain) prevents overfitting to validation set iteration count.

---

## Concerns

### 1. Imputation Before Train/Test Split (Significant)

```python
full_df[col] = full_df.groupby('country')[col].transform(lambda x: x.ffill().bfill())
full_df[col] = full_df[col].fillna(full_df[col].mean())
```

Forward-fill and back-fill is applied on the entire dataset before splitting into train/val/test. This means future macro-economic values (e.g., 2021 GDP) can leak into training rows (≤2018) via back-fill within a country group. The global mean fallback also uses post-2020 data.

**Recommendation:** Perform imputation within each temporal fold. At minimum, use forward-fill only (never back-fill from future) and compute fallback means from training data exclusively.

### 2. MIC Breakpoint Dictionaries Not Defined in Notebook (Moderate)

The `classify_mic_v2()` function references `EUCAST_BREAKPOINTS` and `FALLBACK_BREAKPOINTS` dictionaries that are not defined in any visible cell. This makes the labeling logic — arguably the most critical step — unverifiable from the notebook alone.

**Recommendation:** Include the full breakpoint dictionaries in the notebook or reference the exact EUCAST publication year and table from which they were extracted.

### 3. Intermediate (I) Category — Inconsistent Handling

- Cell 1 states: "I (intermediate) was eliminated from the dataset."
- Cell 5's `classify_mic_v2` maps `'INTERMEDIATE'` → 0 (susceptible), citing EUCAST 2019+ S/I consolidation.
- Cell 3's `parse_sir_to_binary` returns `NaN` for I, effectively discarding it.

These are two different strategies applied in different parts of the pipeline. If EUCAST 2019+ I→S is the intended approach, it should be applied consistently. If I is discarded, `classify_mic_v2` should return NaN for I.

**Recommendation:** Unify the I-handling strategy and document the clinical justification.

### 4. No Hyperparameter Tuning Documented

CatBoost consistently uses `depth=7, learning_rate=0.03, l2_leaf_reg=3` with no documented justification. No grid search, random search, or Bayesian optimization is shown.

**Recommendation:** Either document a tuning procedure (even a coarse grid search) or provide literature-based justification for the chosen values. Report sensitivity to these choices.

### 5. No Cross-Validation for Variance Estimation

The primary model comparison relies on a single temporal test split. No repeated or rolling temporal cross-validation is performed to estimate variance of performance metrics.

**Recommendation:** Consider a rolling-origin temporal CV (e.g., train on ≤2016/test 2017, train on ≤2017/test 2018, etc.) to demonstrate stability across time periods.

### 6. Reverse Causality in Consumption Features

Antibiotic consumption (`consumption_did_per_class`) is merged by country, year, and antibiotic class. Consumption patterns are often driven by local resistance rates (doctors prescribe based on susceptibility), creating endogeneity. This may inflate predictive performance without reflecting causal mechanisms.

**Recommendation:** Acknowledge this explicitly as a limitation. Consider lagging consumption features by 1–2 years, or conducting a sensitivity analysis with these features removed.

### 7. No Per-Subgroup Evaluation in Main Comparison

The global model comparison evaluates only aggregate test-set metrics. No breakdown by organism, antibiotic class, or geographic region is provided in the main comparison cell. Performance could collapse for minority organisms or rare drug–bug combinations.

**Recommendation:** Report AUC/AUPRC stratified by at least the top-5 organisms and major antibiotic classes.

### 8. Cell 26 Labeling Error

Cell 26 is labeled `# EBM` (Explainable Boosting Machine) but contains PyTorch neural network code, not `interpret.glassbox.ExplainableBoostingClassifier`. This suggests code was copied or cells reordered without updating headers.

**Recommendation:** Correct the label or replace with actual EBM implementation from the `interpret` library.

### 9. Simplified MIC Thresholds in Initial Processing

Cell 3's `parse_mic_to_binary()` uses a single threshold per antibiotic regardless of organism (e.g., Meropenem: 4 for all species). Real breakpoints differ by organism (e.g., EUCAST Meropenem: S≤2/R>8 for Enterobacterales vs S≤2/R>8 for Pseudomonas but different for Acinetobacter).

This may be superseded by the later `classify_mic_v2()` relabeling step, but if any rows retain labels from this initial pass, they carry incorrect classifications.

**Recommendation:** Verify that the relabeling step in Cell 6 covers 100% of MIC-derived labels and that no rows retain the simplified Cell 3 labels.

---

## Summary

| Criterion | Rating | Notes |
|-----------|--------|-------|
| Temporal integrity | Partial | Split correct; imputation leaks across time |
| Label quality | Partial | Organism-specific breakpoints attempted but unverifiable |
| Feature engineering | Adequate | Log transforms, class mappings, multiple macro sources |
| Model comparison | Good | DeLong, Bonferroni, bootstrap CIs |
| Calibration | Good | ECE + isotonic properly held out |
| Generalizability | Good | LOCO with multiple reference groups |
| Reproducibility | Partial | Key dicts undefined; Kaggle paths not portable |
| Hyperparameter justification | Missing | No tuning documented |
| Subgroup evaluation | Missing | No per-organism breakdown in main comparison |

---

## Priority Actions

1. **Fix imputation temporal leakage** — highest impact on validity claims.
2. **Include or reference EUCAST breakpoint dictionaries** — needed for reproducibility.
3. **Unify Intermediate (I) handling** — currently contradictory.
4. **Add per-organism subgroup evaluation** — needed for clinical relevance claims.
5. **Document hyperparameter selection rationale.**
