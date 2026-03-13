# Review Reliability Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-RELIABILITY

## Purpose

Examines an artifact through a reliability perspective, identifying single points of failure, missing retry logic, absent timeout handling, lack of circuit breakers, missing health checks, no graceful degradation, and missing backup strategies.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-reliability is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (SLO definitions, architecture context, monitoring config) |

## Output

The tool produces structured output conforming to `review-reliability-template.md`.

## What to Evaluate

### Single Points of Failure
- Are there components with no redundancy whose failure would cause system-wide outage?
- Are there database instances without replication or failover?
- Are there network paths without redundancy?
- Are there external dependencies with no fallback?

### Retry and Timeout Handling
- Are retry policies defined for transient failure scenarios?
- Are retries bounded (max attempts, exponential backoff)?
- Are timeouts defined for all external calls (HTTP, database, queue)?
- Are timeout values appropriate (not infinite, not too aggressive)?
- Is retry behavior idempotent-safe?

### Circuit Breakers
- Are circuit breaker patterns defined for external dependencies?
- Are circuit breaker thresholds specified (failure rate, timeout count)?
- Is the half-open state defined (how does the circuit test recovery)?
- Is there a fallback behavior when the circuit is open?

### Health Checks
- Are liveness and readiness probes defined?
- Do health checks verify actual dependency connectivity (not just process alive)?
- Are health check intervals and thresholds appropriate?
- Is there a distinction between health check failure modes (degraded vs. down)?

### Graceful Degradation
- Is there a defined behavior for partial outages?
- Can the system continue with reduced functionality when dependencies fail?
- Are degradation modes documented and tested?
- Is there a recovery path from degraded state?

### Backup and Recovery
- Is there a backup strategy for persistent data?
- Is recovery time objective (RTO) defined?
- Is recovery point objective (RPO) defined?
- Is the backup strategy tested?

### Failure Mode Analysis
- Are known failure modes documented?
- Is the blast radius of each failure mode bounded?
- Are cascading failure scenarios identified?
- Are failure detection mechanisms defined?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Concept Review | Optional — evaluate reliability risk posture |
| Architecture Review | Required — evaluate architectural reliability |
| Technical Design Review | Required — evaluate reliability in technical design |
| Code Review | Required — evaluate reliability in implementation |
| Integration Review | Required — evaluate reliability testing adequacy |
| Operational Readiness | Required — evaluate reliability in deployment plan |
| Post-Deployment Review | Required — evaluate reliability in production |
| Incident Review | Required — evaluate reliability failures in the incident |

## Constraints

- The lens evaluates what is present in the artifact — it does not perform chaos engineering or fault injection
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools or products
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general reliability assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
