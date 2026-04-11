# Review Maintainability Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-MAINTAINABILITY

## Purpose

Examines an artifact through a maintainability perspective, identifying tight coupling, god classes, missing abstractions, code duplication, overly complex logic, poor naming, and missing documentation.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-maintainability is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (coding standards, architecture principles) |

## Output

The tool produces structured output conforming to `review-maintainability-template.md`.

## What to Evaluate

### Coupling
- Are components tightly coupled (changes in one require changes in many)?
- Are interfaces defined between components (not direct implementation references)?
- Are there circular dependencies between modules or services?
- Is the dependency direction correct (inner layers do not depend on outer layers)?
- Are cross-cutting concerns separated from business logic?

### Cohesion
- Are components focused on a single responsibility?
- Are there god classes or god modules that handle too many concerns?
- Are related functions grouped together (high cohesion)?
- Are unrelated functions separated (not bundled for convenience)?

### Abstraction and Modularity
- Are appropriate abstractions defined for complex operations?
- Are abstractions at the right level (not too leaky, not too abstract)?
- Can components be understood in isolation?
- Can components be tested in isolation?
- Are boundaries between modules explicit?

### Duplication
- Is there structural duplication (same pattern reimplemented in multiple places)?
- Is there logic duplication (same business rules encoded in multiple locations)?
- Are shared concerns extracted into reusable components?
- Is the DRY principle applied without over-abstraction?

### Complexity
- Are there overly complex conditional chains that could be simplified?
- Are state machines explicit rather than implicit in control flow?
- Is cyclomatic complexity managed for critical paths?
- Are algorithms documented when non-obvious?
- Are magic numbers and hardcoded values extracted?

### Naming and Readability
- Are names descriptive and consistent?
- Do names reflect domain concepts (ubiquitous language)?
- Are abbreviations explained or avoided?
- Is the code/design self-documenting where possible?

### Documentation
- Are public interfaces documented?
- Are non-obvious design decisions explained?
- Are tradeoffs and alternatives documented for key decisions?
- Is there sufficient narrative documentation for module-level intent?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Architecture Review | Required — evaluate architectural maintainability |
| Technical Design Review | Required — evaluate design maintainability |
| Implementation Readiness | Optional — evaluate work plan maintainability implications |
| Code Review | Required — evaluate implementation maintainability |
| Incident Review | Optional — evaluate whether maintainability contributed to the incident |

## Constraints

- The lens evaluates what is present in the artifact — it does not perform static analysis or code metrics
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools or products
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general maintainability assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
