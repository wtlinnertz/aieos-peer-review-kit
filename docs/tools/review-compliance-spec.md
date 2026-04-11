# Review Compliance Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-COMPLIANCE

## Purpose

Examines an artifact through a compliance perspective, identifying missing audit trails, data retention gaps, privacy violations, missing consent mechanisms, regulatory non-compliance, and data sovereignty issues.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-compliance is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (compliance requirements, regulatory framework, privacy policy) |

## Output

The tool produces structured output conforming to `review-compliance-template.md`.

## What to Evaluate

### Audit Trail
- Are audit trails defined for security-sensitive and business-critical operations?
- Do audit records capture who, what, when, where, and the outcome?
- Are audit records tamper-resistant (append-only, separate storage)?
- Is audit log retention aligned with regulatory requirements?
- Are audit records accessible for compliance review without production access?

### Data Retention
- Is a data retention policy defined for each data category?
- Are retention periods aligned with regulatory requirements?
- Is data deletion (right to erasure) supported where required?
- Is there a distinction between operational data and compliance data retention?
- Are backups covered by the retention policy?

### Privacy
- Is personal data identified and classified?
- Are data processing purposes documented?
- Is data minimization applied (collecting only what is needed)?
- Are data subject rights supported (access, rectification, erasure, portability)?
- Is data processing lawful basis documented?

### Consent
- Are consent mechanisms defined where required?
- Is consent granular (per purpose, not blanket)?
- Is consent withdrawal supported?
- Are consent records maintained?
- Is default behavior privacy-preserving (opt-in, not opt-out)?

### Regulatory Requirements
- Are applicable regulations identified (GDPR, HIPAA, SOX, PCI-DSS, etc.)?
- Are regulatory controls mapped to implementation?
- Are compliance gaps explicitly documented?
- Are cross-border data transfer requirements addressed?

### Data Sovereignty
- Is data residency documented (where data is stored and processed)?
- Are cross-border data flows identified?
- Are data localization requirements met?
- Are third-party data processors identified with their jurisdictions?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Concept Review | Required — evaluate compliance risk posture |
| Architecture Review | Optional — evaluate compliance in architecture |
| Technical Design Review | Optional — evaluate compliance in design |
| Operational Readiness | Optional — evaluate compliance in deployment |

## Constraints

- The lens evaluates what is present in the artifact — it does not perform compliance auditing
- The lens does not reference specific regulatory jurisdictions unless provided in context documents
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general compliance assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
