# Action Items Before Submission

The paper has improved substantially — the science is in good shape. These are the remaining items to address before submission, grouped by priority.

---

## Priority A — Need these before submitting

### A1. Commit editable manuscript source

We've mentioned this in previous rounds of feedback but haven't received it yet, so just a reminder: NHSJS explicitly requires either a Word document or LaTeX source (a standalone PDF is not accepted), and S.-T. Yau almost certainly has its own formatting requirements too.

Both venues should have submission templates available. If you haven't already been working in one, it's worth reformatting into the appropriate template now rather than at the last moment — reformatting often surfaces length, citation, or layout issues that are easier to fix with time in hand.

**What to do:**
- Check the submission template from your target venue (NHSJS or S.-T. Yau) and reformat if needed.
- Confirm that `Antimicrobial resistance.docx` matches `amr_finaldraft.pdf`.
- Rename it something clean (e.g., `manuscript.docx`) and commit it to the repo.
- Submit Word + PDF (or whatever the venue specifies). No LaTeX setup is needed unless the template is LaTeX-based.

### A2. Align repo code with the paper

The notebook in this repo (`final-draft-code.ipynb`, last modified May 9) appears to reflect the older pipeline (likely 8 models, possibly still `bfill()`, no per-organism table generation). If a reviewer or judge looks at the repository, there would be a mismatch with what the paper describes.

**What to do (pick one):**
- Commit the Zenodo reproducibility notebook to this repo, replacing or clearly superseding `final-draft-code.ipynb`; OR
- Add a note to the repo README stating that Zenodo DOI `10.5281/zenodo.20727165` is the authoritative reproducibility package.

---

## Priority B — Text edits in the manuscript

These are small wording adjustments that will strengthen the paper's defensibility during review or oral defense.

### B1. Soften one abstract claim

Current wording:
> Macro-feature transfer can prioritise AMR testing in data-poor settings

Suggested alternatives:
> Macro-feature transfer **may help** prioritise AMR testing in data-poor settings

or:
> Macro-feature transfer can provide **provisional** prioritisation for AMR testing in data-poor settings

Reasoning: without direct Kazakhstan isolate-level ground truth, it's safer to avoid implying the Kazakhstan rankings have been confirmed. The rest of the abstract is well-hedged already.

### B2. Reconcile the CatBoost vs. LightGBM wording (Section 4.3)

There's a small contradiction: the text says CatBoost and LightGBM were "not significantly different," then says the DeLong test found the 0.002 AUC gap statistically significant. A reviewer will catch this. Consider something like:

> Although LightGBM had a slightly higher AUC than CatBoost (0.851 vs 0.849), the absolute difference was clinically negligible. The DeLong test identified this small difference as statistically significant because of the multi-million-row sample size. Model selection therefore followed the pre-specified clinical operating criterion, Recall@0.2, where CatBoost was higher (0.797 vs 0.782).

### B3. Reframe Table 6 as a plausibility check

Table 6 is a nice addition — just make sure the surrounding text calls it an "external plausibility check" or "face-validity comparison" rather than "validation."

Worth confirming these limitations are noted near the table:
- Single published cohort from one region (Karaganda), not a national sample.
- Inpatient pneumonia patients — likely higher acuity than the national-level inference target.
- Only four organism–antibiotic/class comparisons available.
- Absolute percentage-point agreement should not be interpreted as calibrated Kazakhstan accuracy.

---

## Priority C — Housekeeping (do last)

### C1. Git cleanup

Before sharing the repo publicly with judges, tidy up advisory commits and review documents. This is cosmetic and can wait until everything above is done.

---

## Checklist

- [ ] A1 — Commit editable `.docx` source
- [ ] A2 — Align repo code with paper methodology
- [ ] B1 — Soften abstract claim ("can" → "may help")
- [ ] B2 — Fix CatBoost/LightGBM wording in Section 4.3
- [ ] B3 — Reframe Table 6 as plausibility check
- [ ] C1 — Git cleanup
