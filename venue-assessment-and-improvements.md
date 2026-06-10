# Venue Assessment & Submission Readiness

**Date:** 2026-06-10
**Subject:** Readiness of AMR zero-shot prediction work for two target venues

---

## Target Venues

### 1. National High School Journal of Science (NHSJS)

**Type:** Peer-reviewed academic journal for high school students.

**Review process:** Manuscripts undergo peer and professional review by academics.

**Format requirements:**
- Maximum 20 pages total (including figures, tables, appendix)
- Font size 12, single spacing
- Abstract: 200–250 words
- Required sections (in order): Title, Authors/Affiliations, Abstract, Introduction, Methods, Results, Discussion, Acknowledgments, References
- Minimum 5 figures or tables for Original Research submissions
- Citation style: superscript numerical (e.g., "text^1") with specific reference formatting
- Submission requires 2–3 versions:
  1. Blinded version (no author identifiers) with citations and references
  2. Online publication format with complete citations in double parentheses
  3. For LaTeX: PDF + Word conversion + original .tex file
- Non-compliance risks rejection or delays

**What reviewers look for:** Scientific rigor, reproducibility, clear methodology, appropriate statistical analysis, clinical relevance of claims.

---

### 2. S.-T. Yau High School Science Award

**Type:** International research competition founded by Fields Medalist Shing-Tung Yau. Highly prestigious.

**Categories:** Mathematics, Physics, Biology, Chemistry, Computer Science, Economics Modelling.

**Relevant categories for this work:**
- Computer Science ("Algorithms, systems, theoretical CS, and applied computing research")
- Biology ("Molecular, organismal, and ecological investigations in the life sciences")

**Judging criteria (4 axes):**
1. **Originality** — Is this a novel research question/approach?
2. **Rigor** — Is the methodology sound and defensible?
3. **Presentation** — Is the paper well-written and clearly structured?
4. **Scientific merit** — Does it contribute meaningfully to the field?

**Process:** Paper submission followed by oral defense for finalists. Judges are internationally recognized scholars who will ask probing questions.

**Timeline:** Registration and submission July 1 – September 15 (annual).

**Critical note:** Finalists must orally defend their work to expert panels. The student must understand every method used — not just at a surface level but well enough to answer follow-up questions.

---

## Assessment: Current Work vs. Venue Requirements

### Strengths of the Current Work

- Genuinely novel research question (predicting AMR for a country with no local surveillance using macroeconomic proxies)
- Multiple global datasets harmonized (ATLAS, SIDERO-WT, KEYSTONE)
- Appropriate temporal split design (train ≤2018, val 2019–2020, test >2020)
- LOCO framework with multiple reference groups
- Statistical testing (DeLong + Bonferroni) for model comparison
- Isotonic calibration properly held out
- Good discrimination (AUC ~0.87–0.88)
- SHAP interpretability analysis
- Clinically relevant target (AMR in Kazakhstan)

### Issues & Their Impact by Venue

| Issue | NHSJS Impact | S.-T. Yau Impact |
|-------|-------------|------------------|
| Temporal imputation leakage | **Fatal** — peer reviewers will catch this | **Fatal** — violates "rigor" axis |
| Unverifiable EUCAST breakpoints | **Fatal** — reproducibility required | **High** — judges may probe labeling logic |
| Inconsistent Intermediate (I) handling | **High** — contradicts Methods section | **High** — internal inconsistency |
| 8 models without justification for breadth | Medium — looks unfocused | **High** — student must defend each one orally |
| No hyperparameter tuning documented | High — expected in Methods | Medium |
| No per-subgroup evaluation | Medium — limits clinical claims | Medium |
| "Zero-shot" terminology misuse | Medium — CS-literate reviewers may flag | **High** — CS judges will know the NLP/vision usage |
| Reproducibility broken (Kaggle paths, no requirements.txt) | **High** — journal may request runnable code | Low — paper-focused |
| Paper exists only as PDF (no LaTeX source) | **Blocking** — NHSJS requires structured submission formats | Low — PDF acceptable |
| Cell 26 mislabeled (EBM header, PyTorch code) | Low — if not submitted | Medium — suggests code assembly without understanding |

---

## Required Improvements

### Priority 1: Must-Fix for Either Venue

#### 1.1 Fix Temporal Imputation Leakage

**Problem:** Forward-fill and back-fill applied to the full dataset before train/test split. Future macroeconomic values (e.g., 2021 GDP) can leak into training rows (≤2018) via back-fill within a country group. Global mean fallback also uses post-2020 data.

**Fix:** Perform imputation within each temporal fold:
- Use forward-fill only (never back-fill from future)
- Compute fallback means from training data exclusively
- Or: implement a sensitivity analysis demonstrating the leakage does not materially change results

#### 1.2 Include or Reference EUCAST Breakpoint Dictionaries

**Problem:** `classify_mic_v2()` references `EUCAST_BREAKPOINTS` and `FALLBACK_BREAKPOINTS` dictionaries that are never defined in any visible cell. The labeling logic — the most critical step — is unverifiable.

**Fix:** Either:
- Include the full breakpoint dictionaries in the notebook with source comments, or
- Cite the exact EUCAST publication (year, version, table number, page) from which values were extracted

#### 1.3 Unify Intermediate (I) Category Handling

**Problem:** Three contradictory strategies exist:
- Cell 1 states I is "eliminated from the dataset"
- Cell 5's `classify_mic_v2` maps INTERMEDIATE → 0 (susceptible), citing EUCAST 2019+ S/I consolidation
- Cell 3's `parse_sir_to_binary` returns NaN for I, effectively discarding it

**Fix:** Choose one strategy, apply it consistently across all processing steps, and document the clinical justification (citing EUCAST 2019 guideline if using I→S).

#### 1.4 Reduce Model Scope

**Problem:** 8 models (CatBoost, LightGBM, XGBoost, RF, MLP, LR, NGBoost, AutoGluon) tested without clear justification for why all 8 are needed. This is difficult to defend and suggests a "throw everything at the wall" approach.

**Fix:** Focus on 3–4 models the student can fully explain:
- CatBoost (primary, handles categorical features natively)
- LightGBM or XGBoost (one gradient boosting comparator)
- Logistic Regression (interpretable baseline)
- Random Forest (ensemble baseline)

Remove or relegate the rest to a supplementary appendix if space permits.

#### 1.5 Reframe "Zero-Shot" Terminology

**Problem:** "Zero-shot" in established ML literature refers to model-level adaptation mechanisms (CLIP, GPT, prototypical networks). This work is leave-one-country-out prediction using proxy features — legitimate but not "zero-shot" in the accepted sense.

**Fix:** Either:
- Replace with "LOCO prediction with macroeconomic proxy features" throughout, or
- Include an explicit paragraph defining how "zero-shot" is used here and how it differs from the NLP/vision usage

---

### Priority 2: Must-Fix for NHSJS Specifically

#### 2.1 Produce a LaTeX or Word Manuscript

The paper must follow NHSJS structure exactly:
- Title
- Authors and affiliations
- Abstract (200–250 words, no more)
- Introduction
- Methods
- Results
- Discussion
- Acknowledgments
- References (superscript numerical format)

#### 2.2 Prepare a Blinded Version

Remove all author-identifying information for the review copy.

#### 2.3 Verify Minimum 5 Figures/Tables

Count figures and tables in the current draft. NHSJS Original Research requires a minimum of 5.

#### 2.4 Format Citations Correctly

NHSJS uses superscript numbers before punctuation. References must be:
- Numbered sequentially
- All author initials + surnames listed
- Journal format: *Journal.* Vol. **X**, pg. Y–Z, Year, DOI

---

### Priority 3: Must-Fix for S.-T. Yau Specifically

#### 3.1 Oral Defense Preparation

The student must be able to explain without notes:
- How CatBoost handles categorical features (ordered target encoding)
- What a DeLong test is and why Bonferroni correction is needed
- What isotonic calibration does and why it's preferable to Platt scaling here
- How LOCO validation works and what it proves
- How SHAP values are computed and what they mean for feature importance
- What AUC means clinically in this context

If the student cannot explain any method used, that method should be removed from the paper.

#### 3.2 Choose the Right Category

- **Computer Science:** Frame around the ML methodology (novel application of gradient boosting with proxy features for domain transfer)
- **Biology:** Frame around the AMR prediction contribution to public health surveillance

Computer Science is likely stronger given the methodological emphasis.

---

### Priority 4: Should-Fix for Either Venue

| Item | Effort | Impact |
|------|--------|--------|
| Add per-organism/antibiotic-class subgroup AUC table | Medium | Strengthens clinical claims |
| Document hyperparameter selection rationale | Low | Shows methodological awareness |
| Fix Cell 26 mislabeling (EBM → PyTorch) | Trivial | Prevents embarrassment |
| Add `requirements.txt` | Trivial | Reproducibility |
| Make data paths relative / configurable | Low | Reproducibility |
| Acknowledge reverse causality in consumption features | Low | Shows awareness of limitations |
| Add rolling-origin temporal CV for variance estimation | High | Strengthens robustness claims |

---

## Git Cleanup Plan (For Clean Submission)

The current repository contains advisory files and git history that should not be in the student's submission.

### Files to Include in Clean Submission Repo

- `final-draft-code.ipynb` (after fixes)
- Paper manuscript (LaTeX source + compiled PDF, or Word)
- `README.md` (cleaned — remove "Just say the word!" and placeholder references)
- `requirements.txt` (new)
- Data access instructions (how to request datasets from Vivli)

### Files to Exclude

- `co-authorship-risk-assessment.md`
- `code-review-methodological-rigour.md`
- `feedback-amr-introduction.md`
- `streamlit-interface-ideas.md`
- `amr_additional_features_recommendations.md`
- `amr_alternative_libraries_recommendations.md`
- `amr_publication_packaging_recommendations.md`
- `roadmap.md`
- Old notebook versions (`finall.ipynb`, `iiiiii (1).ipynb`)
- `.github/workflows/` (non-functional CI)

### Process

1. Wait until all methodological fixes are completed
2. Create a new repository (or use `git checkout --orphan clean-submission`)
3. Add only the files listed above
4. Single commit with countryMouse as sole author
5. Verify no references to mentor in any file or metadata

---

## Recommendation

**For NHSJS:** The work is not submission-ready. The imputation leakage and unverifiable breakpoints would likely result in rejection during peer review. The paper also needs to be reformatted to journal specifications (LaTeX/Word, specific citation style, structured sections). After fixes, this is a strong candidate — the research question is novel and the dataset access alone (Vivli) shows initiative.

**For S.-T. Yau:** The work has high potential due to originality and real-world relevance, but the oral defense requirement makes it essential that scope be reduced to what the student can genuinely explain. The current 8-model setup is a liability unless the student has deep understanding of each. After scope reduction and methodological fixes, this is competitive for the Computer Science category.

**Suggested strategy:** Fix the fundamentals (items 1.1–1.5), reduce scope, then submit to both. NHSJS has rolling submissions; Yau has a July–September window. The Yau deadline is more pressing.
