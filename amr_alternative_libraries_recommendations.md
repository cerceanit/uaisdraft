# Recommended Alternative Libraries for AMR Prediction Models

This document outlines alternative machine learning libraries and approaches that could improve or complement the current AMR prediction models.

---

## Current Model Comparison

The notebook compares six ML algorithms to find the best predictor:

| Model | Type | Key Characteristics |
|-------|------|---------------------|
| Random Forest | Ensemble of decision trees | Baseline model, handles imbalanced classes via `class_weight='balanced'` |
| XGBoost | Gradient boosted trees | GPU-accelerated, histogram method, popular in ML competitions |
| LightGBM | Gradient boosted trees | Fast, memory-efficient, leaf-wise growth |
| CatBoost | Gradient boosted trees | Handles categorical features natively (no manual encoding needed) |
| MLP | Neural network | Multi-layer perceptron, learns non-linear patterns |
| Logistic Regression | Linear model | Simple baseline, highly interpretable |

**Why compare multiple models?**
- No single algorithm works best on all data ("no free lunch" theorem)
- Gradient boosted trees (XGBoost, LightGBM, CatBoost) often excel on tabular data
- CatBoost is particularly suited here because it handles categorical features (organism, antibiotic) natively
- A **DeLong test** checks if performance differences are statistically significant

**Evaluation metrics:**
- **AUC** (Area Under ROC Curve) - overall discrimination ability
- **AUPRC** (Area Under Precision-Recall Curve) - important for imbalanced data where resistant cases are rarer
- **F1, Precision, Recall** - classification performance

---

## Alternative Libraries to Consider

### Tabular-Specific Deep Learning

| Library | Description |
|---------|-------------|
| TabNet | Google's neural network designed for tabular data, has built-in feature selection |
| TabTransformer | Transformer architecture adapted for tabular data |
| FT-Transformer | Feature Tokenizer + Transformer, often beats gradient boosting |
| SAINT | Self-Attention and Intersample Attention Transformer |

### AutoML (Automated Model Selection)

| Library | Description |
|---------|-------------|
| AutoGluon | Amazon's AutoML, ensembles multiple models automatically |
| H2O AutoML | Robust AutoML with good defaults |
| FLAML | Microsoft's fast AutoML, low computational cost |
| PyCaret | Low-code ML, easy experimentation |
| Auto-sklearn | Automated sklearn pipeline optimization |

### Probabilistic / Uncertainty Quantification

| Library | Description |
|---------|-------------|
| NGBoost | Probabilistic gradient boosting, outputs confidence intervals |
| PyMC | Bayesian modeling, useful for small data or incorporating priors |
| GPyTorch | Gaussian Processes, good uncertainty estimates |

### Interpretability-Focused

| Library | Description |
|---------|-------------|
| InterpretML (EBM) | Explainable Boosting Machine, glass-box model with competitive accuracy |
| GAMI-Net | Interpretable neural network with main effects + interactions |

---

## Recommendations for AMR Prediction

| Library | Rationale |
|---------|-----------|
| **NGBoost** | Uncertainty quantification is valuable in clinical settings ("70% resistant ± 10%") |
| **AutoGluon** | Might find a better ensemble than manual model selection |
| **InterpretML's EBM** | If interpretability for clinicians is important |

---

## Validation Approaches

### LOCO (Leave-One-Country-Out)
Uses proxy countries (Bulgaria, Ukraine, Turkey, Mexico) as surrogates for Kazakhstan to validate the zero-shot generalization capability.

### 5-Fold Time-Aware Cross-Validation
Folds constructed chronologically by year quantiles with isolate ID group constraints to prevent data leakage.

---

## Target Pathogens

The model focuses on six clinically important pathogens:

1. *Staphylococcus aureus*
2. *Escherichia coli*
3. *Klebsiella pneumoniae*
4. *Pseudomonas aeruginosa*
5. *Acinetobacter baumannii*
6. *Enterococcus* species

---

## Data Sources

| Dataset | Description | Source |
|---------|-------------|--------|
| ATLAS | Global AMR surveillance | Pfizer |
| SIDERO-WT | Global AMR surveillance | Shionogi |
| KEYSTONE | Global AMR surveillance | - |
| World Bank Macro | GDP, health expenditure, population density | World Bank |
| Antibiotic Consumption | DDD/1000/day metrics | - |

---

## Zero-Shot Approach

### The Problem with Country Names

A naive approach using **country as a categorical feature** fails for unseen countries like Kazakhstan. The model has never seen `country="Kazakhstan"` and cannot make predictions.

### The Solution: Macroeconomic Proxies

Instead of country names, use the **underlying factors that cause AMR differences**:

| Naive Approach | Zero-Shot Approach |
|----------------|-------------------|
| `country="France"` | GDP per capita, health expenditure %, population density, antibiotic consumption rates |
| Model learns: "France has X resistance" | Model learns: "Countries with these economic characteristics have X resistance" |

This enables prediction for any country by plugging in its macroeconomic values—economically similar countries likely have similar AMR patterns.

---

## References

- TabNet: https://github.com/google-research/google-research/tree/master/tabnet
- AutoGluon: https://auto.gluon.ai
- NGBoost: https://github.com/stanfordmlgroup/ngboost
- InterpretML: https://interpret.ml
