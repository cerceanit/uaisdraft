# Critique of commit 79597e2 — "Final draft based on the suggestions"

**Date:** 2026-06-23
**Reviewer:** ssk (automated CLI review)
**Commit under review:** `79597e2` by countryMouse

---

## What This Repo Is

This repository contains the working materials for a high-school research paper predicting antimicrobial resistance (AMR) in Kazakhstan using machine learning. The author (countryMouse) trained a CatBoost model on ~11.3 million susceptibility results from 85 countries (sourced via Vivli: ATLAS, SIDERO-WT, KEYSTONE) and applied it to Kazakhstan — a country with no national isolate-level AMR surveillance — using only macroeconomic proxies and antibiotic consumption data as transfer features. The paper targets submission to NHSJS (peer-reviewed high-school science journal) and the S.-T. Yau Science Award (competition with oral defense).

A prior commit (`2e5dc13`) by ssk provided countryMouse with an action plan identifying 8 problems to fix before submission. Commit `79597e2` is countryMouse's response: a new PDF (`amr_finaldraft.pdf`) described as the "final draft based on the suggestions."

---

## Process Note: We Cannot Review PDFs Properly in CLI

The commit contains only a binary PDF. To review it, we had to run `pdftotext` to extract the content — losing all figures, table formatting, and layout context. This is not a sustainable review workflow. A PDF is a rendered artifact, not a source document.

**We need the LaTeX sources committed to this repository.** Specifically:

- The `.tex` file(s)
- Any `.bib` / bibliography files
- Figure source files (or at minimum the PDF/PNG figures)
- A `Makefile` or build script (`latexmk` config, etc.)

If countryMouse provides the LaTeX sources, I will set up a GitHub Actions workflow that:
- Compiles the paper on every push (CI catches broken builds)
- Produces the PDF as a build artifact (reproducible output)
- Optionally runs a word-count / diff check against previous versions

This is a standing offer. Push the `.tex` and I'll have the workflow PR ready the same day.

---

## What Was Fixed (Relative to the Action Plan)

| # | Problem | Verdict | Evidence |
|---|---------|---------|----------|
| 1 | Data leakage (`bfill()`) | **Fixed in text** | Section 2.2.3: "Chronological forward-filling was applied... global mean for each feature over the training period (prior to 2018) was used for any remaining missing values, so that no validation or test data influenced the filling." |
| 2 | Too many models (was 8) | **Fixed** | Now 4 models: CatBoost, LightGBM, Random Forest, Logistic Regression. Justified hierarchy (gradient-boosted vs. ensemble vs. interpretable baseline). |
| 3 | Breakpoints not visible | **Partially fixed** | EUCAST v13.0 and CLSI M100-Ed33 cited by name. Breakpoint tables deposited on Zenodo as `reference_tables.pdf`. Not inline in the paper itself, but traceable. |
| 4 | Contradictory "Intermediate" handling | **Fixed** | Single statement: "Intermediate isolates were dropped to ensure a clear division between susceptible and resistant isolates." No contradictions elsewhere. |
| 5 | "Zero-shot" misuse | **Fixed** | Term eliminated entirely. Replaced with "cross-country generalization scenario" and "cross-country transfer." |
| 6 | No per-organism breakdown | **Fixed** | Table 5: per-organism mean predicted resistance, 95% bootstrap CIs, Spearman trend with BH and Bonferroni corrections for all 6 WHO-priority pathogens. |
| 7 | No editable paper source | **Not fixed in repo** | Only PDF committed. LaTeX source presumably exists (formatting is consistent with LaTeX output) but is not in the repository. |
| 8 | Git cleanup | **Not done** | Advisory commits, review docs, and multiple authors visible in history. Marked "do last" in action plan, so acceptable for now. |

---

## What Improved Beyond the Action Plan

- **Zenodo deposit** with DOI (10.5281/zenodo.20727165): reproducibility notebook, reference tables, prediction CSV
- **Ethics clearance** documented (Assistant Professor Dauren Ayazbayev, SDU University)
- **Face validity table** (Table 6): model predictions compared against published Kazakhstan clinical data (Strochkov et al. 2025)
- **Rolling temporal CV** added as third validation method (expanding-window, 4 folds)
- **Post-Soviet sensitivity model** explored (6 countries sharing Soviet healthcare legacy)
- **Data availability section** meeting journal submission requirements
- **28 references**, properly formatted, including 2025–2026 publications
- **Calibration thoroughly reported**: isotonic regression, ECE reduction from 0.178 to 0.012, Brier Skill Score
- **Honest limitations section**: acknowledges no local ground truth, hyperparameter tuning scope, trend significance caveats

---

## Remaining Concerns

### 1. Code–Paper Consistency (Critical)

The notebook in this repo (`final-draft-code.ipynb`, last modified May 9) still reflects the OLD pipeline — likely 8 models, possibly still containing `bfill()`, no per-organism table generation. If a reviewer or judge inspects the repository, the code will contradict the paper. The Zenodo deposit claims to have a "single reproducibility notebook implementing the full pipeline" — that notebook needs to be the one in this repo, or this repo needs to point to Zenodo explicitly.

**Action required:** Commit the updated notebook that matches the paper's described methodology.

### 2. No LaTeX Source (Blocking for NHSJS)

NHSJS requires either LaTeX source + PDF or a Word document. A standalone PDF is not accepted. This is a hard submission blocker regardless of paper quality.

**Action required:** Commit `.tex` source or `.docx` to the repository.

### 3. Statistical Nuance Worth Flagging

- The Spearman trend results (Table 5) pass BH but not Bonferroni — the paper correctly calls these "hypothesis-generating," which is appropriate. No action needed, just noting the author handled this honestly.
- DeLong test on multi-million-row data will find any difference significant — the paper acknowledges this (Section 4.3: "reflecting sensitivity at multi-million-row scale rather than a clinically meaningful gap"). Good.

### 4. Figures Not Reviewable

We cannot assess figures from `pdftotext` output. Figure 2 (confusion matrix, ROC, PR curve, SHAP beeswarm) and Figure 1 (pipeline diagram) exist but their quality and accuracy cannot be verified without viewing the actual PDF or having source files.

---

## Summary for Codex Second Opinion

The paper text is substantially improved and addresses 6 of 8 identified problems convincingly. The scientific content — methodology, validation design, statistical reporting, and honest scoping of claims — reads as submission-ready for NHSJS. The two unfixed items (LaTeX source, git cleanup) are logistical, not scientific.

The critical gap is **code–paper alignment**: the repo's notebook is stale and doesn't reflect the methodology described in the paper. This must be resolved before any submission that links to a GitHub repository.

Key questions for second opinion:
1. Is the face validity approach (Table 6, n=4 comparisons) sufficient, or does it need more explicit framing of its limitations?
2. Is the claim structure in the abstract appropriately hedged for a paper with no direct Kazakhstan ground truth?
3. Does the model selection rationale (Section 4.3) adequately justify choosing CatBoost over the marginally-higher-AUC LightGBM?

---

## Codex Second Opinion Notes

### 1. Face Validity

Table 6 is acceptable as a qualitative face-validity check, but only if it is framed very narrowly. The current wording is mostly responsible because it says the comparison is "suggestive" and "not the same as confirmation." I would still make the limitation sharper:

- It is a single published cohort.
- It is one region (Karaganda), not a national Kazakhstan sample.
- It is an inpatient pneumonia population, likely higher acuity than the national-level inference target.
- It contains only four comparable organism-antibiotic/class comparisons.
- It compares national model probabilities against a clinical subpopulation, so absolute percentage-point error should not be interpreted as calibrated Kazakhstan accuracy.

Recommendation: keep Table 6, but call it only an "external plausibility check" or "face-validity comparison," not validation. If the paper uses "retrospective validation" near this table, soften that phrase.

### 2. Abstract Claim Structure

The abstract is mostly appropriately hedged. The conclusion says the model is "hypothesis-generating, not a surveillance substitute," which is the right central caveat. The one phrase I would soften is:

> Macro-feature transfer can prioritise AMR testing in data-poor settings

Suggested replacement:

> Macro-feature transfer may help prioritise AMR testing in data-poor settings

or:

> Macro-feature transfer can provide provisional prioritisation for AMR testing in data-poor settings

Reason: without direct Kazakhstan isolate-level ground truth, the paper should avoid sounding as if the Kazakhstan rankings have been confirmed. "Flagging which combinations to test first" is a defensible claim; "predicting Kazakhstan resistance accurately" would not be.

### 3. CatBoost Versus LightGBM

The model-selection rationale is scientifically defensible. LightGBM has a marginally higher AUC, but CatBoost has higher Recall@0.2, and the paper states that recall is clinically prioritized because false negatives are the more dangerous error for an AMR screening/prioritisation tool. That is a coherent reason to choose CatBoost.

However, Section 4.3 has one wording issue. It says CatBoost and LightGBM were "not significantly different," then immediately says the DeLong test made the 0.002 AUC difference statistically significant. That should be reconciled.

Suggested wording:

> Although LightGBM had a slightly higher AUC than CatBoost (0.851 vs 0.849), the absolute difference was clinically negligible. The DeLong test identified this small difference as statistically significant because of the multi-million-row sample size. Model selection therefore followed the pre-specified clinical operating criterion, Recall@0.2, where CatBoost was higher (0.797 vs 0.782).

### 4. Practical Submission/Repository Guidance

The critique correctly identifies code-paper consistency as the most important repository problem. The local notebooks appear stale relative to the final paper; a reviewer following the repo would see older methodology and results. The cleanest recommendation is:

- Commit the exact Zenodo reproducibility notebook to this repository, replacing or clearly superseding `final-draft-code.ipynb`; or
- Make the repository README explicitly say that Zenodo DOI `10.5281/zenodo.20727165` is the authoritative reproducibility package.

The LaTeX-source point should be updated based on the author's clarification: there are no LaTeX sources. If `Antimicrobial resistance.docx` is the editable manuscript source and matches `amr_finaldraft.pdf`, then the submission path should be Word + PDF rather than LaTeX. The action item should be:

**Action required:** Confirm that `Antimicrobial resistance.docx` exactly matches `amr_finaldraft.pdf`, rename it cleanly if needed, and submit it as the editable source. Do not set up or advertise LaTeX CI unless `.tex` sources are later created.
