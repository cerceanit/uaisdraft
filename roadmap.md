#  Research Roadmap: Model Comparison & Statistical Rigor

This roadmap outlines the systematic extension of our Antimicrobial Resistance (AMR) prediction pipeline. The goal is to move beyond simple performance metrics toward a statistically robust, clinically interpretable, and publication-ready framework.

---

##  Phase 1: Model Library Extension
*Expanding the ensemble to include uncertainty, automation, and interpretability.*

- [ ] **NGBoost (Natural Gradient Boosting)**
    * **Goal:** Capture aleatoric uncertainty.
    * **Implementation:** Wrap to handle `predict_proba` via distribution parameters (`.dist.params`).
    * **Output:** Extract mean prediction and standard deviation ($\sigma$) per isolate for "Uncertainty Bands."
- [ ] **AutoGluon (Automated ML)**
    * **Goal:** Benchmark against state-of-the-art AutoML.
    * **Implementation:** Run as a standalone block (Independent Evaluation) on the temporal test set to avoid nested CV conflicts.
- [ ] **InterpretML EBM (Explainable Boosting Machine)**
    * **Goal:** Glass-box interpretability.
    * **Implementation:** Integrate into `GroupKFold` loop.
    * **Output:** Generate per-feature shape functions for clinical validation.

---

##  Phase 2: Statistical Rigor & Significance
*Moving from "Model A is higher" to "Model A is significantly better."*

- [ ] **Wilcoxon Signed-Rank Test**
    * Perform paired testing on per-fold AUC scores across all model pairs.
    * Account for non-normal distribution of fold results.
- [ ] **Multiple Comparison Correction**
    * Apply **Bonferroni correction** to p-values to control the family-wise error rate.
- [ ] **Significance Matrix**
    * Construct a table/heatmap showing $W$-statistics and adjusted $p$-values.

---

##  Phase 3: Advanced Uncertainty Quantification
*Ensuring our confidence intervals are honest and consistent.*

- [ ] **Bootstrap CIs:** Calculate 95% Confidence Intervals from the held-out temporal test set.
- [ ] **CV-based CIs:** Calculate Mean $\pm 1.96 \times SD$ from the `GroupKFold` distribution.
- [ ] **Consistency Audit:** Compare Bootstrap vs. CV intervals. Divergence indicates potential overfitting or data leakage.
- [ ] **Aleatoric vs. Epistemic:** Isolate NGBoost’s predicted uncertainty from the general bootstrap variance.

---

##  Phase 4: Supplementary Figure Checklist
*The visual evidence required for a high-impact submission.*

| Figure | Title | Description |
| :--- | :--- | :--- |
| **S1** | **Performance Summary** | Grouped bar chart (AUC, AUPRC, F1, Brier) with 95% Bootstrap error bars. |
| **S2** | **Statistical Heatmap** | Matrix of Wilcoxon p-values between model pairs. |
| **S3** | **Uncertainty Dist.** | Violin plot of NGBoost $\sigma$ split by True Labels (Resistant vs. Susceptible). |
| **S4** | **EBM Shape Functions** | Marginal contribution plots for Antibiotic, Organism, and Country. |
| **S5** | **AutoGluon Leaderboard** | Internal ranking of sub-models and base learners. |
| **S6** | **Calibration Curves** | Reliability diagrams comparing predicted vs. actual resistance fractions. |

---

##  Phase 5: Deep Research & Clinical Context
*Addressing reviewer concerns and ensuring real-world applicability.*

- [ ] **Temporal Drift Analysis**
    * Train on pre-2018 data; evaluate year-by-year (2018–2022).
    * *Question:* Are AMR patterns shifting faster than the model can adapt?
- [ ] **Subgroup Heatmaps**
    * Break down performance by `Organism × Antibiotic` pairs.
    * Identify "blind spots" in the model's predictive capability.
- [ ] **Data Harmonization Audit**
    * Quantify the impact of dropping "Intermediate" (I) isolates per database.
- [ ] **Kazakhstan-Specific Validation**
    * Focused LOCO (Leave One Country Out) analysis specifically for the KZ target population.
- [ ] **Feature Consistency Check**
    * Compare **SHAP** values (CatBoost) against **Shape Functions** (EBM) to ensure cross-model agreement on top features.
