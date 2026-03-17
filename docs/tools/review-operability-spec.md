# Review Operability Lens — Specification

Version: v1.1

Tool ID: TOOL-REVIEW-OPERABILITY

## Purpose

Examines an artifact through an operability perspective, identifying missing operational testing, absent change management readiness, missing runbooks, complex deployments, no rollback plan, and insufficient health checks.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-operability is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (SLO definitions, runbooks, monitoring config) |

## Output

The tool produces structured output conforming to `review-operability-template.md`.

## What to Evaluate

### Deployment Complexity
- Is the deployment process documented step-by-step?
- Are there manual steps that could be automated?
- Is the deployment atomic or does it require multi-step coordination?
- Are deployment dependencies identified (database migrations, config changes, feature flags)?
- Is the deployment reversible?

### Rollback Plan
- Is a rollback procedure defined?
- Is the rollback tested or at least documented?
- Are rollback criteria specified (when to trigger)?
- Is rollback possible without data loss?
- Is the rollback time bounded?

### Runbook Coverage
- Are operational procedures documented for common scenarios?
- Are runbooks linked to alerts?
- Do runbooks include diagnosis steps, not just remediation?
- Are escalation paths defined in runbooks?
- Are runbooks kept current (review cadence)?

### On-Call and Incident Response
- Is on-call responsibility defined for the service?
- Are incident severity definitions consistent with organizational standards?
- Is the communication plan defined for incidents?
- Are post-incident review procedures referenced?

### Operational Testing
- Is deployment dry-run capability defined?
- Is rollback verification testing documented?
- Is configuration change testing addressed?
- Is environment promotion testing defined?
- Is operational procedure validation included?

### Change Management Readiness
- Is a change approval workflow defined?
- Are change window constraints documented?
- Is a change communication plan for operational staff included?
- Are change rollback criteria distinct from release rollback?
- Are change audit trail requirements defined?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Implementation Readiness | Required — evaluate operational readiness of work plan |
| Code Review | Optional — evaluate operational concerns in implementation |
| Integration Review | Optional — evaluate operational readiness |
| Operational Readiness | Required — evaluate deployment operational readiness |
| Post-Deployment Review | Optional — evaluate operational health |
| Incident Review | Optional — evaluate operational response |

## Constraints

- The lens evaluates what is present in the artifact — it does not test operational procedures
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools or products
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general operability assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
