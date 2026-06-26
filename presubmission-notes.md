# Pre-Submission Notes

Final check before NHSJS submission. Items are grouped by urgency.

---

## Must fix before submitting

### 1. Remove all identifying information from both submitted files

NHSJS requires blind review. Their guidelines explicitly state:

> "Please remove all personal identifying information from this document, including author names, affiliations, acknowledgements, etc."

This applies to **both** submitted `.docx` files (both citation formats). Author information is entered separately through the NHSJS submission portal, not in the manuscript itself.

**What to do:**
- Remove author names and affiliations from both `.docx` files
- Leave the Acknowledgements section **blank** (or write "Removed for blind review") in the submitted versions
- Keep a local copy with the full Acknowledgements text (see `acknowledgements-suggestion.md`) — this gets added back after acceptance

### 2. Prepare acknowledgements for post-acceptance

The Acknowledgements text to insert after the paper is accepted is in `acknowledgements-suggestion.md`. Do **not** include it in the submission files (see above).

### 3. Align repo with paper (outstanding from previous feedback)

The repository has no code in it. Old notebooks were deleted but not replaced. Before submitting, do **one** of:
- Commit the Zenodo reproducibility notebook to this repo; OR
- Add a short `README.md` stating that the authoritative code is at Zenodo DOI `10.5281/zenodo.20727165`

If the submission form or paper links to this GitHub URL, a reviewer clicking through will currently find only review documents and `.docx` files — no pipeline code.

---

## Verified OK

- **Abstract word count:** 239 words (limit is 200–250). Pass.
- **Page count:** Estimated 15–17 pages (limit is 20). Confirm in Word, but likely fine.
- **Two citation formats:** Both files present — superscript numbered and double-parentheses inline. Correct per NHSJS guidelines.
- **Required sections:** Title, Abstract, Introduction, Methods, Results, Discussion, Acknowledgements, References — all present and in correct order.
- **Keywords:** Present after abstract.
- **B-series text edits from previous feedback:** All addressed (B1 softened differently but acceptably, B2 fixed exactly, B3 done).

---

## Minor — check in Word

- The Keywords line may have a line-break splitting "probability" from "calibration; surveillance gap; Kazakhstan." Verify it renders as a single line/paragraph in Word.
- Confirm figures and tables render correctly (we could not assess layout from plaintext extraction).
