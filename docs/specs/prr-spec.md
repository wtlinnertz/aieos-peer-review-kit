# Peer Review Record — Specification

Version: v1.0

The Peer Review Record (PRR) is the governed artifact of the Peer Review Kit. It aggregates the outputs of all review lenses executed against a specific artifact at a specific review point, surfaces conflicts between lenses, and declares a PASS/FAIL disposition that gates artifact freeze.

---

## What This Artifact Is Not

- **Not a validator result.** The PRR does not re-validate the artifact against its spec. That is the artifact's own validator's job.
- **Not a design document.** The PRR identifies issues and recommends mitigations; it does not redesign the artifact.
- **Not a risk register.** The PRR documents review findings relevant to freeze readiness; it is not a comprehensive risk management artifact.

---

## Purpose

The PRR serves three roles:

1. **Finding aggregation** — Combines findings from all executed lenses into a single record with consistent severity classification
2. **Conflict surfacing** — Identifies cases where lens recommendations conflict and documents both perspectives with a resolution path
3. **Freeze gate** — Provides a PASS/FAIL disposition that determines whether the reviewed artifact may proceed to freeze

---

## Upstream Dependencies

- All validated lens outputs for the review point (from Step 1 of the playbook)
- The validated artifact under review (for traceability)
- The review point name and selected lens list (from Step 0 of the playbook)

---

## Required Sections

1. Document Control
2. Review Context
3. Individual Lens Reviews
4. Conflict Analysis
5. Aggregate Assessment
6. Required Remediations (if FAIL)
7. Completeness Checklist

---

## Content Rules

### Document Control
**Rules**
- PRR ID must be present (format: PRR-{PROJECT}-{NNN})
- Date must be present
- Reviewed artifact ID must be present with its type and status
- Review point name must be stated
- All executed lens IDs must be listed
- Spec Version and Principles Version must be present

**Failure Examples**
- Missing PRR ID
- Reviewed artifact ID not present
- Review point not named
- Lens list incomplete or missing

### Review Context
**Rules**
- Reviewed artifact type and ID must be stated
- Review point must be identified
- Required lenses for this review point must be listed (from the review point table)
- Optional lenses included must be listed with rationale for inclusion
- Optional lenses excluded must be acknowledged
- Context documents provided must be listed

**Failure Examples**
- Review point not identified
- Required lenses not listed
- Optional lenses included without rationale
- Context documents not listed

### Individual Lens Reviews
**Rules**
- One subsection per executed lens
- Each subsection must include: lens name, lens tool ID, findings table, lens score (1-10 with justification)
- The findings table must include for each finding: severity (critical/high/medium/low), title, location in reviewed artifact, description, recommendation
- If a lens produced no findings, the subsection must explicitly state "no findings" with a brief justification
- Findings must be transcribed accurately from the lens output — the PRR must not reinterpret, soften, or amplify lens findings
- All required lenses for the review point must have a subsection

**Failure Examples**
- Required lens missing from Individual Lens Reviews
- Finding without severity classification
- Finding without location in the reviewed artifact
- Finding without recommendation
- Lens findings reinterpreted or contradicted by PRR
- Empty subsection without "no findings" statement

### Conflict Analysis
**Rules**
- When two or more lenses produce recommendations that conflict, each conflict must be identified
- Each conflict must name: the lenses involved, the specific recommendations that conflict, why they conflict
- Each conflict must include a suggested resolution path (which may be "defer to the artifact owner" or "escalate to technical lead")
- If no conflicts exist, this section must explicitly state "no conflicts identified" with brief justification

**Failure Examples**
- Conflicting recommendations exist in lens outputs but no conflict documented
- Conflict identified without naming both perspectives
- Conflict without a resolution path
- Empty section without "no conflicts" statement

### Aggregate Assessment
**Rules**
- Total finding count by severity must be stated
- Disposition must be one of: PASS, FAIL
- Disposition must be explicitly supported by the finding summary
- PASS requires: no critical findings AND no unmitigated high findings AND all required lenses executed AND all conflicts have a resolution path
- FAIL requires: one or more critical findings exist, OR one or more high findings lack mitigation, OR required lenses are missing, OR conflicts remain unresolved without a stated path
- The disposition justification must reference specific findings or their absence

**Failure Examples**
- Disposition not stated
- PASS with critical findings present
- PASS with unmitigated high findings
- FAIL without identifying blocking findings
- Disposition contradicts the finding summary

### Required Remediations (if FAIL)
**Rules**
- Required only when disposition is FAIL
- Must list each blocking finding with: finding reference, severity, what must change in the reviewed artifact, which lens produced the finding
- Must not include medium or low findings in the required remediation list (those are advisory)
- If disposition is PASS, this section must state "N/A — disposition is PASS"

**Failure Examples**
- FAIL disposition but no remediation list
- Remediation list includes medium/low findings as blocking
- PASS disposition with non-empty remediation list

---

## Format Requirements

- PRR ID must follow the standard format
- Disposition must be in bold and clearly visible
- Findings tables must use consistent column format across all lens subsections
- Severity must use the standard values: critical, high, medium, low

---

## Completeness Rules

- All required lenses for the review point are represented in Individual Lens Reviews
- All findings from all lenses are included (none dropped or omitted)
- All conflicts between lenses are surfaced
- Disposition is justified by the aggregate finding summary
- Required remediations (if FAIL) reference specific blocking findings

---

## Relationship Rules

- The PRR gates freeze of the reviewed artifact (when PRK is adopted)
- A frozen PRR with PASS disposition clears the reviewed artifact for freeze
- A frozen PRR with FAIL disposition blocks the reviewed artifact from freeze until findings are addressed
- The PRR does not replace the artifact's own validator — validation and peer review are complementary processes

---

## Review Point Mapping

This table defines which lenses are required at each review point. It is the authoritative source for the lens_coverage hard gate.

| Review Point | Kit | Artifact Reviewed | Required Lenses | Optional Lenses |
|---|---|---|---|---|
| Concept Review | PIK | DPRD | business-value, cost, compliance | security, reliability |
| Architecture Review | EEK | SAD | security, reliability, performance, cost, operability, maintainability | compliance, devex |
| Technical Design Review | EEK | TDD | security, reliability, performance, maintainability, devex | cost, compliance |
| Implementation Readiness | EEK | WDD | cost, operability, business-value | maintainability |
| Code Review | EEK | ORD | security, performance, reliability, maintainability, devex | operability |
| Integration Review | QAK | QGR | reliability, security, performance | operability |
| Operational Readiness | REK | RP | operability, reliability, security, cost | compliance |
| Post-Deployment Review | RRK | RHR | reliability, performance, cost, operability | security |
| Incident Review | ODK | PMR | security, reliability, operability | performance, maintainability |

---

## Hard Gates

1. **lens_coverage** — All required lenses for the stated review point were executed and have a subsection in Individual Lens Reviews; the review point is valid (exists in the review point mapping table)
2. **finding_actionability** — Every finding across all lens subsections includes severity (critical/high/medium/low), description, location in the reviewed artifact, and a recommended mitigation; no finding is missing any of these four fields
3. **conflict_surfacing** — When lens recommendations conflict, each conflict is explicitly identified with both perspectives and a suggested resolution path; or "no conflicts" is explicitly stated with justification; no detectable conflict is left unaddressed
4. **artifact_traceability** — PRR identifies the exact artifact ID, artifact type, and status (validated/frozen) of the document under review in both Document Control and Review Context
5. **disposition_justified** — Overall disposition (PASS/FAIL) is justified: PASS requires no critical findings and no unmitigated high findings; FAIL identifies all blocking findings in the Required Remediations section; disposition does not contradict the finding summary
