# Review Lens: Observability — Validator

## Purpose

Evaluate the output of the observability review lens against the hard gates defined in `review-observability-spec.md`. Produce a PASS/FAIL judgment. Do not help, suggest improvements, or expand scope.

## Inputs

1. The observability lens output to evaluate
2. `docs/tools/review-observability-spec.md` — the authoritative rules
3. The artifact that was reviewed (for cross-reference)

## Evaluation Process

Evaluate each hard gate independently.

### Gate 1: `artifact_scoped`

- Every finding references a specific section, decision, or gap in the reviewed artifact
- No findings based on general assertions ("observability is important") or assumptions about unspecified behavior
- "No findings" is acceptable if justified (e.g., artifact scope does not include operational concerns)

### Gate 2: `severity_classified`

- Every finding has exactly one severity: critical, high, medium, or low
- Severity assignments are consistent with the definitions in the spec
- No findings without severity classification

### Gate 3: `evidence_grounded`

- Every finding cites specific artifact content — a design decision, a component description, a missing consideration
- No findings based on inference about what the artifact "probably" does
- Location field references a specific artifact section or component

### Gate 4: `actionable_output`

- Every finding has a concrete, implementable recommendation
- Recommendations address the specific observability gap, not general best practices
- Recommendations do not expand scope beyond the artifact's defined boundaries
- "Add observability" is not actionable — recommendations must specify what to do

## Additional Checks

- If the lens output claims "no findings," verify there is a justification
- No findings outside the observability domain (security findings, cost concerns, etc.)
- Lens score is present and justified

## Output Format

```json
{
  "status": "PASS | FAIL",
  "summary": "<one-sentence verdict>",
  "hard_gates": {
    "artifact_scoped": "PASS | FAIL",
    "severity_classified": "PASS | FAIL",
    "evidence_grounded": "PASS | FAIL",
    "actionable_output": "PASS | FAIL"
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
- Do not evaluate the quality of the observability analysis itself — only whether the output meets structural and evidence requirements
