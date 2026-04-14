# Recommended Publication Packaging for AMR Prediction Models

This document outlines recommendations for packaging the Kazakhstan AMR prediction project for publication and reproducibility.

---

## Current State

The project exists as a Jupyter notebook (`iiiiii (1).ipynb`) which is useful for exploration but problematic for reproducibility and sharing.

---

## Interactive Web Demo

| Tool | Pros | Best For |
|------|------|----------|
| Streamlit | Easiest to build, Python-native, free hosting | Quick demos, non-technical audiences |
| Gradio | Even simpler, great for ML models | Model inference demos |
| Shiny (R or Python) | Polished, publication-quality | Academic audiences |
| Panel/Voila | Converts notebooks to apps | When you already have a notebook |

### Recommendation

**Streamlit** is the recommended choice for this project:
- Python-native, integrates easily with existing code
- Free hosting via Streamlit Community Cloud
- Users can select country/pathogen/antibiotic and see predictions with confidence intervals

---

## Reproducible Code Package

| Approach | Description |
|----------|-------------|
| Python package | Structured `src/` layout with `pyproject.toml`, installable via pip |
| CLI tool | Command-line interface, e.g. `amr-predict --country Kazakhstan --year 2023` |
| Docker container | Full environment frozen, guaranteed reproducibility |
| Conda environment.yml | Lighter than Docker, captures dependencies |

### Recommendation

Use **Docker + Python package** combination:
- Docker guarantees anyone can reproduce results exactly
- Python package enables testing and clean separation of concerns

---

## Experiment Tracking

| Tool | Description |
|------|-------------|
| MLflow | Track experiments, package models, serve predictions |
| DVC | Version control for data + models |
| Weights & Biases | Experiment tracking with nice visualizations |

### Recommendation

**MLflow** is well-suited for academic projects:
- Open source, no vendor lock-in
- Can package models for deployment
- Integrates with the proposed Streamlit demo

---

## Publication-Specific Tools

| Tool | Description |
|------|-------------|
| Quarto | Modern scientific publishing, renders to PDF/HTML/Word |
| Jupyter Book | Turn notebooks into documentation website |
| Binder | One-click runnable notebooks in browser |
| Code Ocean / Zenodo | Archival with DOI for citation |

### Recommendation

**Zenodo** for archival:
- Provides DOI for citation in the paper
- Free for open-source projects
- Integrates with GitHub for automatic releases

---

## Recommended Project Structure

```
amr-kazakhstan/
├── src/amr/           # Core prediction logic
│   ├── __init__.py
│   ├── data.py        # Data loading & harmonization
│   ├── features.py    # Feature engineering
│   ├── model.py       # Model training & inference
│   └── calibration.py # Probability calibration
├── app/               # Streamlit demo
│   └── main.py
├── notebooks/         # Exploratory analysis only
├── data/              # Or instructions to download
├── models/            # Trained model artifacts
├── tests/             # Unit tests
├── Dockerfile
├── pyproject.toml
└── README.md
```

---

## Implementation Priority

### Phase 1: Core Package
1. Refactor notebook into `src/amr/` modules
2. Create `pyproject.toml` with dependencies
3. Add basic unit tests

### Phase 2: Reproducibility
4. Create Dockerfile
5. Set up MLflow experiment tracking
6. Document data download instructions

### Phase 3: Publication
7. Build Streamlit demo app
8. Create Zenodo archive with DOI
9. Add Binder badge for one-click notebook execution

---

## References

- Streamlit: https://streamlit.io
- MLflow: https://mlflow.org
- Zenodo: https://zenodo.org
- Docker: https://www.docker.com
- Quarto: https://quarto.org
