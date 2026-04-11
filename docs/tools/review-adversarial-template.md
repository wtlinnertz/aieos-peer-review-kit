# Review Lens: Adversarial — Output

## Lens Header

| Field | Value |
|-------|-------|
| Lens | Adversarial |
| Artifact Under Review | {artifact ID} |
| Review Point | {review point name} |
| Date | {YYYY-MM-DD} |

---

## Findings

| # | Severity | Title | Location | Description | Recommendation |
|---|----------|-------|----------|-------------|---------------|
| 1 | {critical/high/medium/low} | {brief title} | {§X or component name} | {what the adversarial finding is — assumption fragility, failure cascade, boundary violation, misuse scenario, implicit dependency, invariant violation, or information asymmetry — and what impact it has} | {concrete, implementable fix} |

---

## Category Coverage

| # | Category | Evaluated | Findings Count | Notes |
|---|----------|-----------|----------------|-------|
| 1 | Assumption Fragility | {Yes/No} | {N} | {brief note on what was checked} |
| 2 | Failure Cascade Paths | {Yes/No} | {N} | {brief note on what was checked} |
| 3 | Boundary Condition Violations | {Yes/No} | {N} | {brief note on what was checked} |
| 4 | Misuse Scenarios | {Yes/No} | {N} | {brief note on what was checked} |
| 5 | Implicit Dependency Risks | {Yes/No} | {N} | {brief note on what was checked} |
| 6 | Invariant Violations | {Yes/No} | {N} | {brief note on what was checked} |
| 7 | Information Asymmetry | {Yes/No} | {N} | {brief note on what was checked} |

---

## Open Questions

{List any adversarial questions that could not be resolved from the artifact alone, or "No open questions."}

---

## Lens Score

**Score:** {1–10}

**Justification:** {One sentence. 10 = no adversarial concerns identified across all 7 categories. 1 = critical assumption fragility or uncontained failure cascades that invalidate the artifact's core premise.}
