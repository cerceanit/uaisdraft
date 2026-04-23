# Feedback on AMR Introduction

## Overall Assessment

The introduction is well-structured and academically competent. The core elements—problem significance, regional disparity, research gap, and proposed solution—are present and logically organized. However, the writing could be tightened and made more direct, and the motivation for the solution needs strengthening.

---

## Motivation

**Why this matters:** Motivation is the foundation of any research paper. Without a compelling "why," readers—and reviewers—will not invest in understanding the "how." A weak motivation signals that the research may be a solution in search of a problem. Strong motivation answers: "Why should anyone care about this work?"

### What's There

- AMR death statistics (compelling)
- "Silent pandemic" framing (effective)
- Disparity affecting low/middle-income countries (establishes need)
- Methodological gap (no zero-shot framework for Central Asia)

### What's Missing

The introduction motivates the **problem** (AMR is deadly, data is sparse) but weakly motivates the **solution**. The reader might ask:

1. **"So what?"** — If you successfully predict AMR in Kazakhstan, what changes? What decisions does this enable? Who acts on these predictions and how?

2. **Why zero-shot specifically?** — The practical value of not needing local data is mentioned ("requires years of institutional investment") but could be stronger. What can clinicians/policymakers do *now* with this approach that they couldn't do before?

3. **Why Kazakhstan/Central Asia?** — Beyond "they lack data," is there something strategically important about this region? Trade routes? Emerging resistance patterns? Geopolitical relevance?

4. **Urgency** — The 2050 projection is mentioned, but why act now rather than wait for better local data?

### Suggestion

Add a sentence or two connecting the technical contribution to tangible outcomes:

> "Accurate resistance estimates could guide empirical prescribing decisions, inform national formulary policies, and prioritize surveillance investments—interventions that currently lack an evidence base in these regions."

---

## Novelty and Contribution

**Why this matters:** An explicit "Our Contributions" statement is often the first thing reviewers look for. It tells them exactly what is new and why the paper deserves publication. Without it, reviewers must hunt for the novelty—and may conclude there isn't any. A clear contributions list differentiates your work from incremental or derivative research and provides a checklist reviewers can use to evaluate the paper.

### What's There

- "A zero-shot framework...has not been applied in any study"
- "there are no methodological approaches that assess model reliability in regions where clinical data are limited"

### Issues

1. **Buried and negative framing** — The novelty appears in paragraph 3, framed as what *hasn't* been done ("has not been applied") rather than what this paper *does*

2. **No explicit contribution list** — Many papers state "We contribute: (1)..., (2)..., (3)..." This makes the novelty scannable and memorable

3. **Methodological contributions underplayed** — The LOCO analysis, temporal validation, and three-strategy reliability assessment are mentioned as methods, not as contributions themselves

4. **Unclear differentiation** — How does this differ from other ML-based AMR prediction work beyond the geographic focus? What's technically novel?

### Suggestion

Add an explicit contributions paragraph before the research question:

> "This study makes three contributions. First, we develop the first zero-shot AMR prediction framework for Central Asia, trained entirely on global surveillance data. Second, we propose a validation methodology for data-sparse settings using LOCO analysis and rolling temporal cross-validation. Third, we demonstrate that publicly available socioeconomic features can substitute for unavailable microbiological data without sacrificing predictive accuracy."

---

## Previous Work

**Why this matters:** A thorough discussion of previous work is essential for establishing credibility and positioning your contribution. It demonstrates that you understand the field, shows reviewers you've done your homework, and—critically—*proves* the gap exists rather than merely asserting it. Reviewers who work in ML-AMR will immediately notice if key papers are missing. Inadequate coverage of prior work is one of the most common reasons for rejection.

### What's There

- General citations for AMR statistics (Naghavi, O'Neill, Semenova)
- Background citations for biological mechanisms (Davies & Davies)
- One vague clause: "despite the broad application of machine learning in AMR prediction"

### Issues

1. **ML-AMR literature dismissed in one clause** — "the broad application of machine learning in AMR prediction" is mentioned but no specific papers are cited or discussed

2. **No positioning against competing approaches** — What have others tried? Why didn't it work? The reader doesn't learn the state of the art

3. **Gap asserted, not demonstrated** — The paper claims no zero-shot framework exists, but doesn't show what *does* exist and why it falls short

4. **Missing comparisons:**
   - Other ML-AMR prediction models (what features do they use? what data do they need?)
   - Transfer learning / domain adaptation approaches in health
   - Other attempts to address data-sparse regions

5. **No "why previous approaches fail" argument** — Beyond "they need local data," why can't existing models generalize?

### Suggestion

Add a paragraph surveying prior ML-AMR work:

> "Machine learning approaches to AMR prediction have proliferated in recent years, including [examples]. However, these models typically require local microbiological data for training or fine-tuning, limiting their applicability in resource-constrained settings. Transfer learning approaches such as [examples] have shown promise in related domains but have not been adapted for AMR surveillance. Our zero-shot framework addresses this limitation by relying exclusively on globally available features."

---

## Roadmap

**Why this matters:** A roadmap paragraph signals professionalism and helps busy reviewers navigate the paper efficiently. Reviewers often skim to find specific sections (methods, results, limitations); a roadmap lets them do this quickly. It also demonstrates that the paper has a clear, logical structure—which builds confidence in the overall quality of the work.

### What's There

The introduction ends with the research question but provides no overview of the paper's structure.

### Issue

Many academic papers end the introduction with a roadmap paragraph that orients readers. Its absence can make the paper harder to navigate, especially for reviewers skimming for specific sections.

### Consideration

Whether to include a roadmap depends on the target venue:
- Many journals and CS conferences expect one
- Some consider it formulaic and discourage it
- Medical/health informatics venues often include them

### Suggestion

If appropriate for the target venue, add a brief roadmap after the research question:

> "The remainder of this paper is organized as follows. Section 2 reviews related work on ML-based AMR prediction. Section 3 describes our data sources and zero-shot framework. Section 4 presents our validation methodology. Section 5 reports results, and Section 6 discusses implications and limitations."

---

## Structure

### Strengths

1. **Strong opening** — Opens with compelling statistics (4.71 million associated deaths, 1.14 million direct deaths in 2021) that immediately establish the significance of AMR

2. **Well-cited** — Consistent use of citations throughout supports credibility

3. **Clear logical flow** — Moves from global problem → regional disparity → research gap → proposed solution

4. **Research gap clearly articulated** — Explicitly states that no zero-shot framework for Central Asian countries exists despite broad ML applications in AMR prediction

5. **Clear research question** — The final paragraph states the central question directly

### Areas for Improvement

1. **Length and scope** — The introduction is quite detailed. The methodology description (data sources, validation strategies, LOCO analysis) typically belongs in the Methods section, not the introduction.

2. **Redundancy** — The regional data gaps are mentioned multiple times; paragraphs 2 and 3 overlap in content.

3. **Transition** — The shift from describing the problem to presenting the solution could be smoother.

### Structural Recommendations

- Move detailed methodology (paragraph 4) to the Methods section
- Condense the redundant discussion of data gaps in paragraphs 2 and 3
- Add a brief transitional sentence before introducing the proposed framework

---

## Style

### Nominalization: Convert Nouns to Verbs

| Current (noun-heavy) | Suggested (verb-driven) |
|---------------------|------------------------|
| "the introduction of antibiotics" | "antibiotics were introduced" |
| "the development of interventions" | "developing interventions" |
| "capacity for cross-border dissemination" | "capacity to disseminate across borders" |
| "the lack of financial investment in healthcare" | "governments invest little in healthcare" |
| "the broad application of machine learning in AMR prediction" | "machine learning has been broadly applied to predict AMR" |
| "Training of the model includes data" | "The model trains on data" |
| "collecting local clinical data requires years of institutional investment into medical infrastructure" | "collecting local clinical data requires institutions to invest in medical infrastructure for years" |

### Passive Voice Overuse

| Current (passive) | Suggested (active) |
|------------------|-------------------|
| "A zero-shot framework...has not been applied" | "No study has applied a zero-shot framework..." |
| "these features can be obtained by the public" | "the public can obtain these features" |
| "three complementary strategies are utilized" | "we utilize three complementary strategies" |

### Wordiness

| Current | Suggested |
|---------|-----------|
| "the fact that collecting local clinical data requires..." | "because collecting local clinical data requires..." |
| "meaning these are not only theoretical concerns but also tangible public health failures" | "making these tangible public health failures, not theoretical concerns" |
| "Due to the absence of ground-truth data" | "Without ground-truth data" |

### Hedging That Weakens Claims

| Current | Suggested |
|---------|-----------|
| "Such a framework may offer" | "This framework offers" (if confident) |

### Sentence Complexity

Some sentences are overly long and should be split:

**Original:**
> "Consequences of these gaps can be delayed outbreak detection, the lack of access to therapeutics, and inappropriate antibiotic prescribing, meaning these are not only theoretical concerns but also tangible public health failures."

**Revised:**
> "These gaps delay outbreak detection, limit access to therapeutics, and encourage inappropriate antibiotic prescribing. These are tangible public health failures, not theoretical concerns."

### Awkward Phrasing

| Current | Suggested |
|---------|-----------|
| "Heavy reliance on microbiological inputs makes predictive models inapplicable in these regions due to the systematic absence of such data." | "Predictive models that rely on microbiological inputs cannot be applied in these regions because such data are systematically absent." |

---

## Example Revision

**Original paragraph:**
> "For low- and middle-income countries, the lack of financial investment in healthcare and science indicates that this disparity is not incidental but structural, meaning isolated technical solutions are not sufficient."

**Revised:**
> "In low- and middle-income countries, governments underinvest in healthcare and science, indicating this disparity is structural, not incidental. Isolated technical solutions will not suffice."

---

## Summary of Recommendations

1. **Structure:** Move methodology details to Methods section; reduce redundancy between paragraphs 2–3
2. **Style:** Convert nominalizations to verbs for more direct prose
3. **Voice:** Reduce passive constructions; use active voice where appropriate
4. **Concision:** Eliminate filler phrases ("the fact that," "Due to the absence of")
5. **Clarity:** Break up long sentences; simplify complex constructions
