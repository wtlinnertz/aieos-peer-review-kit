# Review Lens: Resilience — Spec

**Tool ID:** TOOL-REVIEW-RESILIENCE
Version: v1.0

---

## Purpose

The resilience lens examines an artifact for gaps in compound failure survival, chaos engineering readiness, recovery velocity, SLO recovery time budgets, self-healing mechanisms, adaptive capacity, blast radius containment, and cascading failure prevention. It identifies weaknesses that would prevent the system from surviving and recovering from unexpected or multi-failure scenarios.

---

## Preconditions

- The artifact under review has passed its own validator
- The full artifact is provided (not a summary)
- The review point is one where resilience is applicable (see Applicable Review Points)

---

## Input

| Parameter | Description |
|-----------|-------------|
| `artifact` | The full text of the artifact under review |
| `review_point` | Which review point this lens is being executed for |
| `context_documents` | Optional supporting documents (SLO definitions, architecture context, incident history, chaos test results) |

---

## Evaluation Categories

### 1. Multi-Failure Scenarios

Does the design account for compound failures, not just single-component outages?

- Scenarios where two or more components fail simultaneously are identified
- Correlated failure modes documented (e.g., shared dependency, region outage)
- System behavior under multi-failure conditions is specified
- Prioritization of which failures to handle simultaneously vs. accept as beyond design limits

### 2. Chaos Engineering Readiness

Is the system designed to be safely tested under failure conditions?

- Fault injection points identified in the architecture
- Steady-state hypothesis defined for chaos experiments
- Blast radius controls for chaos tests (scope limitation, kill switches)
- Chaos test plan exists or is referenced for post-deployment verification
- Pre-production chaos testing strategy defined

### 3. Recovery Velocity

How quickly can the system return to normal operation after a failure?

- Recovery time targets defined for different failure scenarios
- Automated recovery mechanisms specified (restart, failover, re-routing)
- Manual recovery procedures documented with estimated duration
- Recovery verification steps defined (how to confirm recovery is complete)
- Recovery prioritization when multiple components fail

### 4. SLO Recovery Time Budgets

Are recovery expectations aligned with SLO commitments?

- Recovery time objectives derived from SLO error budgets
- Time-to-detect + time-to-mitigate + time-to-recover budgeted against SLO
- Recovery time budget allocated across detection, diagnosis, and remediation phases
- Budget exceeded scenario documented (what happens when recovery takes longer than budgeted)

### 5. Self-Healing Mechanisms

Can the system recover from certain failures without human intervention?

- Automatic restart or respawn for crashed processes
- Automatic failover for stateful components
- Automatic scaling for capacity-related degradation
- Self-healing scope boundaries defined (what heals automatically vs. requires human)
- Self-healing verification (how operators know self-healing occurred)

### 6. Adaptive Capacity

Can the system adjust to unexpected load or conditions beyond design parameters?

- Load shedding strategy defined for overload conditions
- Graceful degradation modes for capacity exhaustion
- Priority-based resource allocation when resources are constrained
- Backpressure mechanisms for upstream overload
- Capacity headroom assessment and growth planning

### 7. Blast Radius Containment

Are failures isolated to prevent system-wide impact?

- Failure domain boundaries defined (per-service, per-region, per-tenant)
- Bulkhead patterns isolating critical from non-critical paths
- Resource isolation preventing noisy-neighbor effects
- Data isolation preventing corruption propagation
- Deployment blast radius limited (canary, feature flags, progressive rollout)

### 8. Cascading Failure Prevention

Are mechanisms in place to prevent one failure from triggering a chain of failures?

- Dependency failure propagation paths identified
- Circuit breakers or equivalent isolation at dependency boundaries
- Timeout and retry policies that prevent retry storms
- Queue depth limits preventing unbounded backlog accumulation
- Synchronous dependency chains identified and mitigated

---

## Applicable Review Points

| Review Point | Status | Rationale |
|-------------|--------|-----------|
| Architecture Review (SAD) | Required | Architectural resilience patterns must be identified early |
| Technical Design Review (TDD) | Optional | Verify detailed design includes resilience mechanisms |
| Integration Review (QGR) | Optional | Verify integration testing covers multi-failure scenarios |
| Operational Readiness (RP) | Required | Verify resilience mechanisms are operational before release |
| Post-Deployment Review (RHR) | Required | Verify resilience in production behavior |
| Incident Review (PMR) | Required | Evaluate resilience failures exposed by the incident |

---

## Constraints

- Evaluate only what is present in the artifact — do not perform chaos engineering or fault injection
- Do not suggest new features or expand scope
- Do not redesign the artifact
- Do not reference specific vendor tools or chaos platforms in findings
- Findings must cite specific artifact sections, decisions, or gaps
- Do not produce findings outside the resilience domain

---

## Hard Gates

| # | Gate | Rule |
|---|------|------|
| 1 | `artifact_scoped` | Every finding references a specific section, decision, or gap in the artifact under review. No findings based on general assertions or assumptions. |
| 2 | `severity_classified` | Every finding has a severity: critical (system-wide failure risk with no containment), high (significant resilience gap), medium (weakness that should be addressed), low (minor improvement). |
| 3 | `evidence_grounded` | Every finding cites specific artifact content — a design decision, a component description, a missing consideration. No findings without traceable evidence. |
| 4 | `actionable_output` | Every finding has a concrete, implementable recommendation that addresses the resilience gap without expanding scope. |
