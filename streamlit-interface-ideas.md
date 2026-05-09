# Streamlit Interface Ideas for AMR Prediction Tool

**Date:** 2026-05-09

---

## Concept

A Streamlit app that exposes the trained CatBoost model (and optionally the ensemble) as an interactive tool for clinicians, public health researchers, and policymakers. The app would run inference on the saved `.cbm` model without needing to retrain.

---

## Page Ideas

### 1. Resistance Risk Explorer

**Purpose:** Given a country, organism, antibiotic, and year — return the predicted probability of resistance with a confidence indicator.

- Dropdowns for organism (6 targets), antibiotic (filtered by clinical plausibility), country, year
- Output: probability gauge (0–1), calibrated confidence band, AWaRe category flag
- Colour-coded: green (<30%), amber (30–70%), red (>70%)
- Could pull macro features automatically from a bundled CSV or a live World Bank API call

### 2. Country-Level Antibiogram Heatmap

**Purpose:** Generate a synthetic antibiogram for any country/year combination.

- Select country + year → produce organism × antibiotic heatmap of predicted resistance probabilities
- Toggle between raw probabilities and binary S/R at a user-selected threshold
- Export as PNG or CSV for local infection control use
- Side-by-side comparison: two countries or two time points

### 3. Temporal Trend Dashboard

**Purpose:** Show how resistance is predicted to evolve over time for a given organism–antibiotic pair.

- Line charts of predicted resistance probability over years (2004–2023)
- Overlay actual observed resistance rates (where available in ATLAS/SIDERO) vs model predictions
- Confidence ribbon from NGBoost uncertainty or bootstrap
- Filters: organism, antibiotic, country or region grouping

### 4. Feature Impact (SHAP) Viewer

**Purpose:** Let users understand what drives a specific prediction.

- Enter a scenario (country/organism/antibiotic/year) → run SHAP on that single instance
- Waterfall plot showing which features push toward R vs S
- Global SHAP summary (precomputed beeswarm plot) as a reference tab
- Useful for policymakers to see which levers (e.g., sanitation, health expenditure) matter most

### 5. Model Comparison Dashboard

**Purpose:** Transparently display how different models perform.

- Precomputed metrics table with bootstrap CIs (from the notebook results)
- Interactive ROC and PR curves (toggle models on/off)
- Calibration reliability diagrams per model
- DeLong p-value matrix as a heatmap

### 6. Kazakhstan LOCO Results

**Purpose:** Dedicated view for the Kazakhstan case study.

- Heatmap of predicted resistance by organism × antibiotic for each year
- K-means cluster assignments (from notebook Cell 39) visualised on a risk map
- Compare post-Soviet reference group vs structural peers vs all-countries models
- Highlight high-risk combinations flagged for clinical review

### 7. Data Coverage Explorer

**Purpose:** Show users where the training data is strong or sparse.

- World map coloured by number of isolates per country
- Bar chart of year × data source (ATLAS/SIDERO/KEYSTONE) contribution
- Flag countries/years where predictions rely heavily on imputation
- Useful for transparency — "trust this prediction less for countries with <100 isolates"

---

## Technical Considerations

### Model Serving
- Load the saved `catboost_model.cbm` at app startup via `CatBoostClassifier().load_model()`
- Bundle macro feature CSVs (GDP, health expenditure, etc.) as static data files
- Precompute SHAP values for common scenarios to avoid slow real-time computation

### Architecture Options

| Approach | Pros | Cons |
|----------|------|------|
| Single-file `app.py` | Simple to deploy | Gets unwieldy beyond 3 pages |
| Multi-page (`pages/` dir) | Clean separation, native Streamlit support | Slightly more setup |
| With FastAPI backend | Decouples model from UI, better for production | More infrastructure |

**Recommendation:** Start with Streamlit multi-page app. Migrate to FastAPI backend only if concurrent users or API access becomes a requirement.

### Data Pipeline
- Static CSV bundle for macro features (refreshed quarterly)
- Or: live World Bank API calls via `wbgapi` Python package with caching (`@st.cache_data`)
- Clinical plausibility filter reused from notebook (the `VALID_COMBINATIONS` dict)

### Deployment Options
- **Streamlit Community Cloud** — free, easy, good for demos/academic sharing
- **Docker container** — portable, good for institutional deployment
- **HuggingFace Spaces** — free GPU if needed for SHAP computation

---

## Minimum Viable Version

For a first iteration, focus on pages 1 and 2 only:

1. Single-prediction resistance risk lookup
2. Country antibiogram heatmap

This requires:
- The saved `.cbm` model file
- A bundled macro features CSV (one row per country-year)
- The `VALID_COMBINATIONS` filter dict
- ~200 lines of Streamlit code

---

## Potential File Structure

```
streamlit_app/
├── app.py                    # Entry point, page config
├── pages/
│   ├── 1_Risk_Explorer.py
│   ├── 2_Antibiogram.py
│   ├── 3_Trends.py
│   ├── 4_SHAP.py
│   └── 5_Model_Comparison.py
├── models/
│   └── catboost_model.cbm
├── data/
│   ├── macro_features.csv
│   ├── valid_combinations.json
│   └── precomputed_shap.parquet
├── utils/
│   ├── predict.py            # Model loading + inference helpers
│   ├── features.py           # Feature construction from raw inputs
│   └── plots.py              # Reusable plotting functions
└── requirements.txt
```

---

## Open Questions

- Should the app allow user-uploaded local surveillance data for custom predictions?
- Is real-time SHAP computation feasible, or should all SHAP be precomputed?
- Should there be an authentication layer for institutional deployment?
- Would a "report generation" feature (PDF export of antibiogram + trends) be valuable?
