# uaisdraft

## Datasets

### Clinical AMR Surveillance (Access Required)
- **ATLAS** (Pfizer) – Global AMR surveillance
- **SIDERO-WT** (Shionogi)
- **KEYSTONE**

> These datasets were obtained via formal request through [vivli.org](https://vivli.org).

### Public Datasets
- World Bank Macroeconomic Indicators (GDP, health expenditure, population density)
- Kazakhstan Antibiotic Consumption (DDD/1000/day, 2017–2023)

---

## Features

- **Target**: Binary resistance (Resistant = 1, Susceptible = 0). Intermediate (I) phenotypes are discarded.
- **Core Features**:
  - Organism + Antibiotic
  - Year
  - Country-level macroeconomic indicators
  - Antibiotic consumption (where available)
- **Models**: Random Forest, XGBoost, LightGBM, CatBoost

---

## Methodology

1. Data harmonization across three global databases
2. Macroeconomic feature engineering
3. Strict temporal and geographical splits (Kazakhstan held out)
4. Model training with time-aware cross-validation
5. Probability calibration (Platt scaling + isotonic)
6. Zero-shot inference on Kazakhstan macro profile
7. Ablation study on contextual features

---

## Results

- Strong discrimination on global test set (AUC ~0.87–0.88)
- Calibrated predictions for Kazakhstan show realistic resistance rates
- Macroeconomic features significantly improve generalization

*(See notebook for detailed metrics and visualizations)*

---

## Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/amr-kazakhstan-zero-shot.git
cd amr-kazakhstan-zero-shot
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure data paths

Create a `.env` file or modify `config/config.yaml`:
```yaml
data_dir: "/path/to/your/data"
```

---

## Requirements

- Python 3.10+
- pandas, numpy, scikit-learn
- xgboost, lightgbm, catboost
- matplotlib, seaborn


## ⚠️ Important Notes

- Clinical datasets (ATLAS, SIDERO-WT, KEYSTONE) are **not public**. You must request access via Vivli.
- This project demonstrates **methodological feasibility** of macro-driven zero-shot AMR prediction.
- Predictions should be interpreted as **estimates** and validated with local surveillance data when available.



##  Acknowledgments

- Pfizer, Shionogi, and KEYSTONE teams for AMR surveillance data
- World Bank for open macroeconomic data
- Kaggle community for dataset hosting




Just say the word!
