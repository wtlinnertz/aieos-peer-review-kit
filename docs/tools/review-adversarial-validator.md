# Review Lens: Adversarial — Validator

## Purpose

Evaluate the output of the adversarial review lens against the hard gates defined in `review-adversarial-spec.md`. Produce a PASS/FAIL judgment. Do not help, suggest improvements, or expand scope.

## Inputs

1. The adversarial lens output to evaluate
2. `docs/tools/review-adversarial-spec.md` — the authoritative rules
3. The artifact that was reviewed (for cross-reference)

## Evaluation Process

Evaluate each hard gate independently.

### Gate 1: `artifact_scoped`

- Every finding references a specific section, decision, or gap in the reviewed artifact
- No findings based on general assertions ("security is important") or assumptions about unspecified behavior
- "No findings" is acceptable only if the minimum_findings gate also passes (see Gate 5)

### Gate 2: `severity_classified`

- Every finding has exactly one severity: critical, high, medium, or low
- Severity assignments are consistent with the definitions in the spec
- No findings without severity classification

### Gate 3: `evidence_grounded`

- Every finding cites specific artifact content — a design decision, a stated assumption, a missing consideration
- No findings based on inference about what the artifact "probably" does
- Location field references a specific artifact section or component

### Gate 4: `actionable_output`

- Every finding has a concrete, implementable recommendation
- Recommendations address the specific adversarial concern, not general best practices
- Recommendations do not expand scope beyond the artifact's defined boundaries
- "Improve security" is not actionable — recommendations must specify what to do

### Gate 5: `minimum_findings`

- If findings count >= 3: PASS
- If findings count < 3 AND detailed per-category justification is present in the Category Coverage table (all 7 categories evaluated, each with explanation of why no findings emerged): PASS with warning "Zero/low findings flagged for human review"
- If findings count < 3 AND no per-category justification: FAIL

## Additional Checks

- Category Coverage table is present and all 7 categories are listed
- No findings outside the 7 adversarial evaluation categories
- Lens score is present and justified
- No fabricated findings (findings that do not trace to actual artifact content)

## Output Format

```json
{
  "status": "PASS | FAIL",
  "summary": "<one-sentence verdict>",
  "hard_gates": {
    "artifact_scoped": "PASS | FAIL",
    "severity_classified": "PASS | FAIL",
    "evidence_grounded": "PASS | FAIL",
    "actionable_output": "PASS | FAIL",
    "minimum_findings": "PASS | FAIL"
  },
  "blocking_issues": [
    {
      "gate": "<gate_name>",
      "description": "<what is wrong>",
      "location": "<section reference>"
    }
  ],
  "warnings": [
    {
      "description": "<non-blocking observation>",
      "location": "<section reference>"
    }
  ],
  "completeness_score": "<0-100>"
}
```

## Rules

- Do not suggest fixes or improvements to the lens output
- Do not expand scope beyond what the spec requires
- Evaluate only against the hard gates
- Do not evaluate the quality of the adversarial analysis itself — only whether the output meets structural and evidence requirements
