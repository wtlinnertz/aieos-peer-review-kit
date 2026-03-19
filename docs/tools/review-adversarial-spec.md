# Review Lens: Adversarial — Spec

**Tool ID:** TOOL-REVIEW-ADVERSARIAL
Version: v1.0

---

## Purpose

Attack the artifact's assumptions, probe for failure cascades, test boundary conditions, and identify misuse scenarios. Unlike domain-specific lenses that evaluate within a specialty, the adversarial lens evaluates across domains with a skeptical posture. Zero findings requires justification.

---

## Preconditions

- The artifact under review has passed its own validator
- The full artifact is provided (not a summary)
- The review point is one where adversarial review is applicable (see Applicable Review Points)

---

## Input

| Parameter | Description |
|-----------|-------------|
| `artifact` | The full text of the artifact under review |
| `review_point` | Which review point this lens is being executed for |
| `context_documents` | Optional supporting documents (upstream artifacts, principles files, prior lens outputs from other review points) |

---

## Evaluation Categories

### 1. Assumption Fragility

What if key assumptions are wrong? Identify assumptions the artifact treats as settled that are actually uncertain, untested, or dependent on external factors outside the initiative's control.

### 2. Failure Cascade Paths

What chain reactions can one failure trigger? Trace paths where a single component failure, data corruption, or dependency outage cascades into broader system failure without containment.

### 3. Boundary Condition Violations

What happens at limits? Examine behavior at zero, maximum, overflow, empty, concurrent, and timeout boundaries. Identify boundary conditions that are unspecified or have no defined handling.

### 4. Misuse Scenarios

How could users, operators, or attackers break this? Consider both intentional misuse (adversarial actors) and unintentional misuse (confused users, misconfigured operators, automated systems with stale state).

### 5. Implicit Dependency Risks

What undeclared dependencies exist? Identify dependencies on network availability, clock synchronization, ordering guarantees, configuration consistency, or third-party behavior that are assumed but not documented.

### 6. Invariant Violations

What "impossible" states could actually occur? Identify states the artifact assumes cannot happen (duplicate IDs, negative counts, null references in non-nullable fields) and evaluate whether the design prevents them.

### 7. Information Asymmetry

What does the artifact assume readers know? Identify knowledge gaps between the artifact's authors and its downstream consumers that could lead to misinterpretation, incorrect implementation, or missed constraints.

---

## Applicable Review Points

| Review Point | Status | Rationale |
|-------------|--------|-----------|
| Architecture Review (SAD) | Optional | Attack architectural assumptions and failure cascades |
| Technical Design Review (TDD) | Optional | Probe boundary conditions and implicit dependencies |
| Code Review (ORD) | Optional | Test for misuse scenarios and invariant violations |
| Integration Review (QGR) | Optional | Identify cross-component failure cascades |
| Operational Readiness (RP) | Optional | Probe deployment and rollback assumptions |

---

## Constraints

- Evaluate only what is present in the artifact — do not test running systems
- Do not suggest new features or expand scope
- Do not redesign the artifact
- Do not reference specific vendor tools or platforms in findings
- Findings must cite specific artifact sections, decisions, or gaps
- Do not produce findings outside the adversarial evaluation categories

---

## Hard Gates

| # | Gate | Rule |
|---|------|------|
| 1 | `artifact_scoped` | Every finding references a specific section, decision, or gap in the artifact under review. No findings based on general assertions or assumptions. |
| 2 | `severity_classified` | Every finding has a severity: critical (assumption failure invalidates artifact premise, uncontained cascade, data corruption), high (production boundary condition with no handling, significant misuse with no mitigation), medium (untested assumption with no validation plan, partial boundary handling), low (minor assumption weakness, theoretical edge case). |
| 3 | `evidence_grounded` | Every finding cites specific artifact content — a design decision, a stated assumption, a missing consideration. No findings without traceable evidence. |
| 4 | `actionable_output` | Every finding has a concrete, implementable recommendation that addresses the identified risk without expanding scope. |
| 5 | `minimum_findings` | At least 3 findings required. Zero findings acceptable ONLY with detailed justification demonstrating all 7 evaluation categories were attempted and the artifact is genuinely robust. The validator flags zero-finding outputs for human review regardless of justification quality. |
