# Peer Review Kit — Test Plan

This document contains the structural integrity checks and flow scenario tests for the Peer Review Kit. These tests verify that the kit is complete, internally consistent, and capable of producing valid artifacts.

---

## Structural Integrity Checks

Structural checks verify that the kit's files are present, properly named, and internally consistent. These checks do not require AI — they are verifiable by inspection.

### S-01: Four-File Completeness — PRR Artifact

**Check:** The PRR artifact type has exactly four files: spec, template, prompt, validator.

| File | Location | Status |
|------|----------|--------|
| Spec | docs/specs/prr-spec.md | Required |
| Template | docs/artifacts/prr-template.md | Required |
| Prompt | docs/prompts/prr-prompt.md | Required |
| Validator | docs/validators/prr-validator.md | Required |

**Expected result:** All four files present.

---

### S-02: Four-File Completeness — Lens Tools

**Check:** Each of the 9 lens tools has exactly four files: spec, template, prompt, validator.

| Lens | Spec | Template | Prompt | Validator |
|------|------|----------|--------|-----------|
| review-security | docs/tools/review-security-spec.md | docs/tools/review-security-template.md | docs/tools/review-security-prompt.md | docs/tools/review-security-validator.md |
| review-reliability | docs/tools/review-reliability-spec.md | docs/tools/review-reliability-template.md | docs/tools/review-reliability-prompt.md | docs/tools/review-reliability-validator.md |
| review-performance | docs/tools/review-performance-spec.md | docs/tools/review-performance-template.md | docs/tools/review-performance-prompt.md | docs/tools/review-performance-validator.md |
| review-cost | docs/tools/review-cost-spec.md | docs/tools/review-cost-template.md | docs/tools/review-cost-prompt.md | docs/tools/review-cost-validator.md |
| review-operability | docs/tools/review-operability-spec.md | docs/tools/review-operability-template.md | docs/tools/review-operability-prompt.md | docs/tools/review-operability-validator.md |
| review-maintainability | docs/tools/review-maintainability-spec.md | docs/tools/review-maintainability-template.md | docs/tools/review-maintainability-prompt.md | docs/tools/review-maintainability-validator.md |
| review-compliance | docs/tools/review-compliance-spec.md | docs/tools/review-compliance-template.md | docs/tools/review-compliance-prompt.md | docs/tools/review-compliance-validator.md |
| review-devex | docs/tools/review-devex-spec.md | docs/tools/review-devex-template.md | docs/tools/review-devex-prompt.md | docs/tools/review-devex-validator.md |
| review-business-value | docs/tools/review-business-value-spec.md | docs/tools/review-business-value-template.md | docs/tools/review-business-value-prompt.md | docs/tools/review-business-value-validator.md |

**Expected result:** All 36 tool files present (9 lenses x 4 files each).

---

### S-03: Hard Gate Count Alignment

**Check:** Each spec's declared hard gate count matches the validator's gate checks.

| Type | Spec Hard Gates | Validator Gates |
|------|----------------|----------------|
| PRR | 5 | 5 |
| Each lens tool | 4 | 4 |

**Expected result:** Counts match for PRR and all 9 lens tools.

---

### S-04: Hard Gate Name Alignment

**Check:** Gate names in specs match gate names in validators (exact string match for JSON output field names).

| Type | Spec Gate Names | Validator Gate Names |
|------|----------------|---------------------|
| PRR | lens_coverage, finding_actionability, conflict_surfacing, artifact_traceability, disposition_justified | lens_coverage, finding_actionability, conflict_surfacing, artifact_traceability, disposition_justified |
| Each lens | artifact_scoped, severity_classified, evidence_grounded, actionable_output | artifact_scoped, severity_classified, evidence_grounded, actionable_output |

**Expected result:** All gate names match exactly.

---

### S-05: Prompt-to-Spec Reference Integrity

**Check:** The PRR generation prompt references the correct spec and template. No prompt inlines content rules.

| Prompt | References Spec | References Template | Inlines Rules? |
|--------|----------------|--------------------|----|
| prr-prompt.md | docs/specs/prr-spec.md | docs/artifacts/prr-template.md | No |

**Expected result:** Prompt references correct spec and template; no inlined rules.

---

### S-06: Validator-to-Spec Reference Integrity

**Check:** The PRR validator references its spec as the source of truth. Validator does not reference prompts.

| Validator | References Spec | References Prompt? |
|-----------|-----------------|-------------------|
| prr-validator.md | docs/specs/prr-spec.md | No |

**Expected result:** Validator references correct spec; does not reference prompts.

---

### S-07: Lens Tool Spec Version Headers

**Check:** Each lens tool spec has a `Version: v1.0` header and a Tool ID.

| Lens | Version Header | Tool ID |
|------|---------------|---------|
| review-security | v1.0 | TOOL-REVIEW-SECURITY |
| review-reliability | v1.0 | TOOL-REVIEW-RELIABILITY |
| review-performance | v1.0 | TOOL-REVIEW-PERFORMANCE |
| review-cost | v1.0 | TOOL-REVIEW-COST |
| review-operability | v1.0 | TOOL-REVIEW-OPERABILITY |
| review-maintainability | v1.0 | TOOL-REVIEW-MAINTAINABILITY |
| review-compliance | v1.0 | TOOL-REVIEW-COMPLIANCE |
| review-devex | v1.0 | TOOL-REVIEW-DEVEX |
| review-business-value | v1.0 | TOOL-REVIEW-BUSINESS-VALUE |

**Expected result:** All lens specs have version and tool ID.

---

### S-08: PRR Template Section Alignment

**Check:** The PRR template's section headings match the required sections listed in the spec.

| Spec Required Sections | Template Sections |
|----------------------|-------------------|
| Document Control, Review Context, Individual Lens Reviews, Conflict Analysis, Aggregate Assessment, Required Remediations, Completeness Checklist | §1-§7 (all present) |

**Expected result:** All template sections match spec required sections.

---

### S-09: Review Point Table Consistency

**Check:** The review point table in the PRR spec matches the review point table in the playbook. Both contain the same 9 review points with the same required and optional lens assignments.

**Expected result:** Tables are identical.

---

### S-10: Governance Model Sync

**Check:** `docs/governance-model.md` matches `aieos-governance-foundation/governance-model.md` exactly.

**Expected result:** Files are identical (byte-for-byte).

---

## Flow Scenario Tests

Flow scenarios verify that the kit's artifacts, when produced in order with appropriate inputs, pass validation. These tests require AI execution.

---

### F-00: Architecture Review — All PASS, No Conflicts

**Scenario:** Receive a validated SAD with no issues → execute 6 required lenses (all produce no findings or low findings only) → aggregate into PRR → disposition PASS.

**Setup:**
- Provide: a validated SAD with complete security, reliability, performance, cost, operability, and maintainability coverage
- Execute 6 lenses, each returning no findings or low-severity only
- Generate PRR → validate → freeze

**Expected outcomes:**
- All 6 lens outputs: 4 gates PASS each
- PRR: all 5 gates PASS; disposition is PASS; no conflicts; no required remediations

**Key gate to verify:** PRR Gate 1 (lens_coverage) — confirm all 6 required lenses present.

---

### F-01: Architecture Review — PASS with Conflict

**Scenario:** Security and performance lenses produce conflicting recommendations → PRR surfaces the conflict → disposition PASS because conflict has resolution path and no critical/unmitigated high findings.

**Setup:**
- Execute security lens: high finding recommending encryption
- Execute performance lens: high finding flagging encryption latency
- Both have documented mitigations
- Generate PRR with conflict analysis

**Expected outcomes:**
- PRR: all 5 gates PASS; conflict documented in §4 with both perspectives; disposition PASS; all high findings have mitigations
- Conflict has a resolution path documented

**Key gate to verify:** PRR Gate 3 (conflict_surfacing) — confirm conflict identified with both perspectives and resolution path.

---

### F-02: Code Review — FAIL with Critical Finding

**Scenario:** Security lens produces a critical finding (hardcoded secrets) → PRR disposition is FAIL → required remediations listed.

**Setup:**
- Execute security lens: critical finding (hardcoded API key in ORD)
- Execute remaining required lenses: no critical findings
- Generate PRR

**Expected outcomes:**
- PRR: all 5 gates PASS; disposition is FAIL; §6 Required Remediations lists the critical finding
- Required remediations reference the specific security lens finding

**Key gate to verify:** PRR Gate 5 (disposition_justified) — confirm FAIL is the only valid disposition when a critical finding exists. A PASS with a critical finding would fail this gate.

---

## Notes

- All structural checks (S-01 through S-10) should be verified before running flow scenarios.
- Flow scenarios F-00 through F-02 cover PASS without conflict, PASS with conflict, and FAIL.
- Additional scenarios may be added as new patterns are identified in production use.
