# Peer Review Record — Validator

This validator evaluates a generated Peer Review Record (PRR) against `docs/specs/prr-spec.md`. It is used in a separate AI session from the one that generated the PRR.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The generated PRR (full document)
2. `docs/specs/prr-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: lens_coverage

Check:
- A review point is stated in §2 Review Context
- The stated review point exists in the review point mapping table in the spec
- All required lenses for that review point have a subsection in §3 Individual Lens Reviews
- Each lens subsection includes: lens name, tool ID, findings table (or "no findings" statement), and lens score

**Pass:** All required lenses present with complete subsections; review point is valid.
**Fail:** Required lens missing; review point not stated or not in mapping table; lens subsection incomplete.

---

### Gate 2: finding_actionability

Check:
- Every finding across all lens subsections in §3 has all four required fields: severity (critical/high/medium/low), description, location in the reviewed artifact, and recommendation
- Severity uses the standard values (critical, high, medium, low) — no other values
- Location references a specific section, field, or area of the reviewed artifact — not a generic reference
- Recommendation is concrete and actionable — not vague ("consider improving")
- If a lens reports "no findings," the statement includes a brief justification

**Pass:** All findings have all four fields with substantive content; severity values are standard; locations are specific.
**Fail:** Finding missing severity, description, location, or recommendation; non-standard severity value; vague location or recommendation; "no findings" without justification.

---

### Gate 3: conflict_surfacing

Check:
- §4 Conflict Analysis is present and non-empty
- If conflicts are documented: each conflict names the lenses involved, describes both perspectives, explains why they conflict, and includes a suggested resolution path
- If "no conflicts" is stated: justification is provided
- Cross-check §3 lens findings for detectable conflicts that are not documented in §4:
  - Look for opposing recommendations (one lens says add X, another says remove X or avoid X)
  - Look for resource tension (one lens recommends more resources, another recommends fewer)
  - Look for tradeoff tension (one lens prioritizes one quality attribute at the expense of another)

**Pass:** All detectable conflicts are documented with both perspectives and resolution paths; or "no conflicts" with justification and no detectable conflicts in §3.
**Fail:** Conflict exists in §3 findings but not documented in §4; conflict documented without both perspectives; conflict without resolution path; empty §4 without statement.

---

### Gate 4: artifact_traceability

Check:
- §1 Document Control includes: Reviewed Artifact ID (format: TYPE-PROJECT-NNN), Reviewed Artifact Type, Reviewed Artifact Status
- §2 Review Context includes: Artifact Type, Artifact ID, Artifact Status, Producing Kit
- Artifact ID in §1 and §2 are consistent (same value)
- Artifact status is one of: Validated, Frozen

**Pass:** Artifact ID, type, and status present in both §1 and §2; consistent; status is valid.
**Fail:** Artifact ID missing from §1 or §2; inconsistency between §1 and §2; status not stated or invalid.

---

### Gate 5: disposition_justified

Check:
- §5 Aggregate Assessment includes a disposition (PASS or FAIL)
- Disposition is in bold
- Finding summary by severity is present
- PASS disposition: verify no critical findings exist in the finding summary; verify all high findings have documented mitigations (check High Finding Mitigation Status table)
- FAIL disposition: verify §6 Required Remediations lists all blocking findings (critical + unmitigated high); verify each remediation references a specific finding
- Disposition does not contradict the finding summary

**Pass:** Disposition stated and justified; no contradiction with findings; PASS has no critical/unmitigated high; FAIL lists all blocking findings in §6.
**Fail:** Disposition not stated; PASS with critical findings; PASS with unmitigated high findings; FAIL without §6 remediations; disposition contradicts findings.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "lens_coverage": "PASS | FAIL",
    "finding_actionability": "PASS | FAIL",
    "conflict_surfacing": "PASS | FAIL",
    "artifact_traceability": "PASS | FAIL",
    "disposition_justified": "PASS | FAIL"
  },
  "blocking_issues": [
    {
      "gate": "<gate_name>",
      "description": "<what specifically failed>",
      "location": "<section or field where the failure is>"
    }
  ],
  "warnings": [
    {
      "description": "<non-blocking observation>",
      "location": "<section>"
    }
  ],
  "completeness_score": "<0-100>"
}
```

**Interpretation rules:**
- Any gate failure → `"status": "FAIL"`
- `blocking_issues` lists exactly the failures — no additional content
- `warnings` are non-blocking; they do not affect status
- `completeness_score` is advisory; it does not override gate results
- If all gates pass, `blocking_issues` is an empty array

---

## Validator Constraints

- Do not suggest how to fix failures
- Do not redesign or improve the PRR
- Do not evaluate content quality beyond what the spec requires
- Do not accept inferred information as equivalent to explicit content
- Evaluate only what is present in the document
