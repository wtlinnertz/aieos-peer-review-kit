# Review Operability Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-OPERABILITY

## Purpose

Examines an artifact through an operability perspective, identifying missing logging, absent metrics, inadequate alerting, missing runbooks, complex deployments, no rollback plan, and insufficient health checks.

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

### Observability — Logging
- Is structured logging defined for key operations?
- Are log levels appropriate (not all INFO, not excessive DEBUG in production)?
- Are correlation IDs propagated across service boundaries?
- Are sensitive data redacted from logs?
- Is log retention and rotation defined?

### Observability — Metrics
- Are key business and technical metrics identified?
- Are latency, error rate, and throughput (RED/USE) metrics defined?
- Are custom metrics defined for domain-specific health indicators?
- Is metric granularity appropriate (not too coarse, not too fine)?

### Observability — Alerting
- Are alerts defined for SLO-relevant conditions?
- Are alert thresholds specific (not "alert on any error")?
- Is alert routing defined (who gets paged, escalation path)?
- Are alerts actionable (linked to runbooks or playbooks)?
- Is there alert fatigue risk (too many non-actionable alerts)?

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

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Architecture Review | Required — evaluate operational architecture |
| Implementation Readiness | Required — evaluate operational readiness of work plan |
| Code Review | Optional — evaluate operational concerns in implementation |
| Integration Review | Optional — evaluate operational testing |
| Operational Readiness | Required — evaluate deployment operational readiness |
| Post-Deployment Review | Required — evaluate operational health |
| Incident Review | Required — evaluate operational response |

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
