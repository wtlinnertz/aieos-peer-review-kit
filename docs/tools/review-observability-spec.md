# Review Lens: Observability — Spec

**Tool ID:** TOOL-REVIEW-OBSERVABILITY
Version: v1.0

---

## Purpose

The observability lens examines an artifact for gaps in structured logging, metrics instrumentation, distributed tracing, log aggregation, metric export, alert design, and observability testing. It identifies blindspots that would prevent operators from understanding system behavior in production.

---

## Preconditions

- The artifact under review has passed its own validator
- The full artifact is provided (not a summary)
- The review point is one where observability is applicable (see Applicable Review Points)

---

## Input

| Parameter | Description |
|-----------|-------------|
| `artifact` | The full text of the artifact under review |
| `review_point` | Which review point this lens is being executed for |
| `context_documents` | Optional supporting documents (SLO definitions, monitoring config, ISPEC, SAD) |

---

## Evaluation Categories

### 1. Structured Logging Standards

Are logging practices defined with sufficient structure for operational use?

- Structured format specified (JSON, key-value) rather than unstructured text
- Log levels differentiated and appropriate (not all INFO, not excessive DEBUG in production)
- Correlation IDs propagated across service boundaries
- Sensitive data redaction policy defined
- Log retention and rotation specified
- Context fields (request ID, user ID, operation) included in log entries

### 2. Metrics Instrumentation

Are key operational and business metrics defined with appropriate granularity?

- RED metrics defined (Rate, Errors, Duration) for request-serving components
- USE metrics defined (Utilization, Saturation, Errors) for resource components
- Custom business metrics identified for domain-specific health indicators
- Metric granularity appropriate (not too coarse, not too fine)
- Metric naming conventions consistent across services
- Histogram buckets or percentile definitions specified for latency metrics

### 3. Distributed Tracing and Correlation

Can operations be traced across service boundaries?

- Trace context propagation defined for inter-service communication
- Span naming conventions specified
- Critical path identification for key user journeys
- Sampling strategy defined (head-based, tail-based, or deterministic)
- Trace-to-log correlation mechanism specified

### 4. Log Aggregation and Search

Can operators find and analyze log data effectively?

- Log aggregation destination defined
- Search and filter capabilities specified for operational scenarios
- Log volume estimation and capacity planning addressed
- Cross-service log correlation supported
- Structured query patterns documented for common troubleshooting scenarios

### 5. Metric Export and Dashboards

Are metrics accessible and visualized for operational decision-making?

- Metric export pipeline defined (collection, storage, visualization)
- Dashboard requirements specified for operational and business views
- Dashboard hierarchy defined (overview → service → component drill-down)
- SLO tracking dashboards specified with burn-rate visualization
- On-call dashboard requirements defined

### 6. Alert Design

Are alerts SLO-based, actionable, and fatigue-resistant?

- Alerts defined for SLO-relevant conditions (not just threshold breaches)
- Alert thresholds specific with numeric values (not "alert on any error")
- Alert routing defined (who gets paged, escalation path)
- Alerts linked to runbooks or playbooks
- Alert fatigue risk assessed (too many non-actionable alerts)
- Multi-window or burn-rate alerting considered for SLO-based alerts

### 7. Observability Testing Plan

Is observability itself tested before production?

- Log output verification included in test plan
- Metric emission verification included in test plan
- Alert firing verification (test-fire) planned
- Dashboard population verification planned
- Trace propagation verification for multi-service scenarios

---

## Applicable Review Points

| Review Point | Status | Rationale |
|-------------|--------|-----------|
| Architecture Review (SAD) | Optional | Identify architectural decisions that enable or constrain observability |
| Technical Design Review (TDD) | Optional | Verify design includes observability instrumentation patterns |
| Code Review (ORD) | Optional | Verify implementation includes observability code |
| Operational Readiness (RP) | Required | Verify observability is operational before release |
| Post-Deployment Review (RHR) | Required | Verify observability is functioning in production |

---

## Constraints

- Evaluate only what is present in the artifact — do not test running systems
- Do not suggest new features or expand scope
- Do not redesign the artifact
- Do not reference specific vendor tools or monitoring platforms in findings
- Findings must cite specific artifact sections, decisions, or gaps
- Do not produce findings outside the observability domain

---

## Hard Gates

| # | Gate | Rule |
|---|------|------|
| 1 | `artifact_scoped` | Every finding references a specific section, decision, or gap in the artifact under review. No findings based on general assertions or assumptions. |
| 2 | `severity_classified` | Every finding has a severity: critical (operational blindness), high (significant observability gap), medium (weakness that should be addressed), low (minor improvement). |
| 3 | `evidence_grounded` | Every finding cites specific artifact content — a design decision, a component description, a missing consideration. No findings without traceable evidence. |
| 4 | `actionable_output` | Every finding has a concrete, implementable recommendation that addresses the observability gap without expanding scope. |
