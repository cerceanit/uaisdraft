# How This Work Compares to Accepted Submissions

**Date:** 2026-06-10
**Subject:** Benchmarking the AMR prediction project against published/awarded work at both target venues

---

## NHSJS: Published Papers in Computer Science / ML (2024–2026)

### Paper 1: "Predictive Modelling of a Chess Player's Style using Machine Learning" (2024)

| Dimension | Chess Paper | AMR Paper (Ours) |
|-----------|-------------|------------------|
| Models used | 2 (Logistic Regression, Neural Network) | 8 (CatBoost, LightGBM, XGBoost, RF, MLP, LR, NGBoost, AutoGluon) |
| Statistical tests | None — simple accuracy comparison | DeLong with Bonferroni, bootstrap CIs |
| Dataset | 4,500 games, single source | 3 global surveillance databases + World Bank + consumption data |
| Validation strategy | 70/30 random split | Temporal split + LOCO geographic holdout |
| Figures/tables | 4 | Likely 10+ (SHAP, calibration, clustering, etc.) |
| Scope | Narrow, well-bounded | Very broad, arguably over-scoped |
| Clinical/real-world relevance | Entertainment | Public health (AMR prediction) |
| Sophistication | Undergraduate intro-course level | Graduate-level methods |

**Takeaway:** The AMR work is substantially more sophisticated than this accepted paper. The chess paper succeeds because it's focused, clearly written, and honest about its scope. It uses 2 models and explains them well rather than deploying 8 without justification.

---

### Paper 2: "Analyzing Acoustic Features for Speech Emotion Classification" (2026)

| Dimension | Emotion Paper | AMR Paper (Ours) |
|-----------|--------------|------------------|
| Models used | 6 (LR, RF, SVC, Gradient Boosting, MLP, CNN-LSTM) | 8 |
| Statistical tests | None — no CIs or significance tests | DeLong + Bonferroni, bootstrap CIs |
| Validation | Leave-One-Speaker-Out (proper, no leakage) | Temporal split (but imputation leaks) |
| Dataset | 360 samples from 6 actors | Hundreds of thousands of isolates from global databases |
| Key strength | Rigorous leakage prevention, systematic ablation | Scale, real-world significance |
| Key weakness | Tiny dataset, modest accuracy (~41%) | Imputation leakage undermines validity |
| Sophistication | Upper undergrad / early Master's | Graduate-level methods |

**Takeaway:** This paper has lower accuracy and simpler statistics, but its validation is airtight — no leakage by design. The AMR paper's imputation leak is a problem precisely because NHSJS reviewers will see the contrast: sophisticated stats (DeLong, Bonferroni) sitting on top of a leaky foundation. Fix the leak and the AMR paper is clearly stronger.

---

### Paper 3: "Deep Neural Networks for Breast Cancer Ultrasound Classification" (2026)

| Dimension | Cancer Paper | AMR Paper (Ours) |
|-----------|-------------|------------------|
| Models used | 9+ (LR, RF, XGBoost, LightGBM, CatBoost, DNN, CNN, MobileNet, U-Net) | 8 |
| Statistical tests | Nested cross-validation, 50 evaluations per model | DeLong + Bonferroni, bootstrap (n=1000) |
| Dataset | 256 images (explicitly data-constrained) | Large-scale global surveillance |
| Validation | Nested stratified K-fold (50 metric evaluations) | Single temporal split |
| Key strength | Honest about limitations, proper variance estimation | Novel research question, real clinical target |
| Scope | Multi-modal (tabular + image + segmentation) | Multi-model + multi-database + geographic transfer |
| Sophistication | Graduate-level | Graduate-level |

**Takeaway:** This is the closest comparator. It was accepted with 9+ models because each serves a clear purpose (tabular vs image vs multi-modal vs segmentation). The AMR paper's 8 models feel less justified — they're all solving the same tabular classification problem. The cancer paper also has proper variance estimation through nested CV, while the AMR paper relies on a single temporal split.

---

### Paper 4: "Robotic Waste Sorting System Using ML Classification" (2026)

| Dimension | Waste Sorting Paper | AMR Paper (Ours) |
|-----------|--------------------|--------------------|
| Models used | 1 (Google Teachable Machine) | 8 |
| Statistical tests | None — accuracy, precision, recall only | DeLong + Bonferroni, bootstrap CIs |
| Dataset | 10,000 images, 4 classes | Hundreds of thousands of isolates, 6 organisms |
| Novelty | Systems integration (ML + robotics) | Methodological (cross-country AMR transfer) |
| Validation | 80/10/10 split, no CV | Temporal + geographic holdout |
| Key strength | End-to-end prototype, economic analysis | Novel question with public health impact |
| Sophistication | High school level | Graduate level |

**Takeaway:** NHSJS accepts work across a wide range of sophistication. This paper used a no-code ML platform and was published because it solved a real problem end-to-end with clear practical value. The AMR work is far more technically sophisticated — the risk isn't being "too simple" but rather being so complex that reviewers question whether the author understands it all.

---

### Paper 5: "Fairness Regularization in CNNs for Demographic Bias" (2026)

| Dimension | Fairness Paper | AMR Paper (Ours) |
|-----------|---------------|------------------|
| Models used | 3 (ResNet-50, ResNet-101, INV-REG ResNet-50) | 8 |
| Statistical tests | None — single split, no CIs | DeLong + Bonferroni |
| Dataset | 108,000 images (FairFace) | Large-scale AMR surveillance |
| Novelty | Applies existing technique (INV-REG) to new context | Applies ML to novel geographic prediction problem |
| Key weakness | Single split, no confidence intervals, no repeated trials | Imputation leakage, unverifiable breakpoints |
| Honesty about limitations | Excellent — explicitly acknowledges all gaps | Unclear — limitations section in PDF not verified |

**Takeaway:** This paper was accepted despite having no confidence intervals and only a single split — because it was transparent about limitations. If the AMR paper explicitly acknowledges and bounds its imputation issue (or fixes it), it would be held to the same forgiving standard.

---

## S.-T. Yau 2025 USA Regional Medalists: Computer Science

| Medal | Student | Project |
|-------|---------|---------|
| **Gold** | Omar Graia (Acton Boxborough) | "High-Efficiency Geometric Adaptation (HEGA): A Geometry-Aware First-Order Optimizer with Strong Nonconvex Performance" |
| **Silver** | Albert Lu (Phillips Exeter) | "Alcatraz: Secure Remote Computation via Sequestered Encryption in Minimally Trusted Hardware" |
| **Bronze** | Santosh Patapati (Panther Creek) | "CLARGA: Adaptive Residual Graph Attention for Contrastive Multimodal Representation Learning" |
| **Bronze** | Zirui Wang (Princeton Intl School of Math) | "Optimization Application of PPO Algorithm in Reinforcement Learning in Drone Attitude Balance" |
| **HM** | Mythreya Dharani (Bergen County Academies) | Metabolomics markers for Alzheimer's disease stratification |
| **HM** | Yijun Wang (Tonbridge School) | Fairness in multimodal language models and bias mitigation |

### Analysis: What Yau CS Winners Look Like

The Gold and Silver winners share characteristics:
- **Novel algorithmic contribution** (a new optimizer; a new encryption scheme) — not applications of existing methods
- **Theoretical depth** — these sound like papers that could appear at ICML or security conferences
- **Tight scope** — one core contribution, deeply explored

The Bronze winners are closer to our work:
- Applied ML (graph attention networks, reinforcement learning)
- Existing methods adapted to new problems
- Less theoretical novelty, more empirical contribution

### Where the AMR Paper Sits in This Hierarchy

| Criterion | AMR Paper | Yau Gold/Silver | Yau Bronze |
|-----------|-----------|-----------------|------------|
| Novelty of question | **Strong** — nobody has done LOCO AMR prediction with macro features for Kazakhstan | Very strong — new algorithms | Moderate — applying known methods |
| Algorithmic contribution | **None** — uses off-the-shelf models | Core contribution | Minor adaptations |
| Depth of understanding required | High (8 models, stats tests) | Very high (proofs, theory) | Moderate |
| Real-world impact | **Strong** — public health relevance | Theoretical | Applied |
| Data access difficulty | **Strong** — Vivli access required formal applications | Varies | Varies |
| Defensibility in oral exam | **Risky** — 8 models, DeLong, isotonic, LOCO, SHAP... | Focused, deep | More defensible |

**Verdict for Yau:** The AMR paper would compete at the **Bronze / Honorable Mention** level. Its strength is the novel applied question and real-world significance. Its weakness is the lack of algorithmic novelty (no new method proposed) and the breadth-over-depth problem. Gold requires theoretical contribution; Bronze requires solid applied work that the student can fully defend.

---

## Summary: Competitive Positioning

### For NHSJS

The AMR work is **more sophisticated than most accepted CS papers** at this venue. The risk is not rejection for being too weak — it's rejection for:
1. Methodological flaws (imputation leakage) that reviewers will catch
2. Scope that raises credibility questions about a single high-school author
3. Missing reproducibility requirements (breakpoints, paths, requirements.txt)

**If the fundamentals are fixed**, this paper would be among the stronger CS publications in recent NHSJS history. The combination of multiple real-world datasets, temporal validation, and public health relevance is genuinely above the median.

### For S.-T. Yau

The work is competitive at the **Bronze/HM tier** in Computer Science. To reach Silver/Gold would require:
- A novel methodological contribution (not just applying CatBoost)
- Theoretical justification for why macroeconomic features should predict AMR
- Tighter scope (fewer models, deeper analysis)

The strongest play for Yau is to **lean into the applied novelty**: "nobody has predicted AMR for a data-scarce country using economic proxies before." That's a genuine contribution even without algorithmic novelty. But the student must be able to defend every method used in the oral round — which means reducing scope to what they truly understand.

---

## Concrete Recommendations Based on This Analysis

### 1. Reduce model count (critical for both venues)

NHSJS accepted papers with 1–9 models, but the ones with many models (breast cancer paper) justify each one serving a different purpose. The AMR paper has 8 models all solving the same tabular binary classification.

**Recommendation:** Keep 3–4:
- CatBoost (primary — handles categoricals, best performer)
- Logistic Regression (interpretable baseline)
- Random Forest (ensemble baseline)
- One of LightGBM/XGBoost (gradient boosting comparator)

Frame the comparison as "do we need complex models, or do simpler ones suffice?" rather than "here are 8 models."

### 2. Fix the imputation leak (critical for NHSJS)

The emotion classification paper was accepted partly because its validation was leakage-free by design. The AMR paper's statistical sophistication (DeLong, Bonferroni) actually makes the imputation leak *more* embarrassing — it shows awareness of rigorous practice in one area while violating it in another.

### 3. Emphasize the novel question, not the methods (critical for Yau)

Yau Gold winners propose new algorithms. The AMR paper doesn't. What it does have is a **genuinely novel applied question** — "can we predict AMR in a country with zero surveillance data using only economic proxies?" — which is original and impactful. The paper should lead with this, not with "we compared 8 models."

### 4. Scope and honesty win at NHSJS

The waste sorting paper was accepted using Google Teachable Machine. The fairness paper was accepted with no confidence intervals. Both were transparent about limitations. The AMR paper's breadth of methods needs to be matched with equally honest acknowledgment of its gaps (or better: fixing them).

### 5. Oral defense prep determines Yau outcome

If the student can explain:
- Why CatBoost over XGBoost for this specific data structure
- What isotonic calibration does geometrically
- Why LOCO is more appropriate than standard cross-validation here
- What the SHAP values for macroeconomic features mean clinically

...then Bronze is realistic. If they cannot, Honorable Mention at best.
