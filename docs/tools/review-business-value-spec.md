# Review Business Value Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-BUSINESS-VALUE

## Purpose

Examines an artifact through a business value perspective, identifying scope creep, misaligned features, missing user value, poor ROI, feasibility concerns, and over-engineering.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-business-value is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (DPRD, product brief, business case, market context) |

## Output

The tool produces structured output conforming to `review-business-value-template.md`.

## What to Evaluate

### Scope Alignment
- Does the artifact stay within the scope defined by upstream artifacts (DPRD, PRD)?
- Are there features or components that are not traceable to stated goals?
- Are non-goals from upstream artifacts respected (no violations)?
- Is there gold-plating (building more than what was requested)?

### User Value
- Is the connection between implementation and user value clear?
- Are the most valuable features prioritized?
- Is there work that does not contribute to user-facing outcomes?
- Are user scenarios addressed by the artifact complete?

### ROI and Feasibility
- Is the effort proportional to the expected value?
- Are there simpler alternatives that deliver equivalent value?
- Is the timeline realistic for the scope?
- Are resource requirements identified and available?
- Is the technical approach feasible with current capabilities?

### Over-Engineering
- Are there unnecessary abstractions or generalizations?
- Is the solution more complex than the problem requires?
- Are there "future-proofing" decisions that add cost without addressing a stated requirement?
- Is there premature optimization?

### Risk to Value Delivery
- Are there dependencies or unknowns that could block value delivery?
- Are there scope items with unclear acceptance criteria?
- Is there a risk of delivering the artifact but not the intended value?
- Are phasing or prioritization decisions sound?

### Market and Competitive Context
- Does the artifact reflect current market understanding (if applicable)?
- Are competitive considerations addressed where relevant?
- Is the timing appropriate (not too early, not too late)?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Concept Review | Required — evaluate business value of the concept |
| Implementation Readiness | Required — evaluate business alignment of work plan |

## Constraints

- The lens evaluates what is present in the artifact — it does not perform market research or financial modeling
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools or products
- Findings must cite specific artifact sections, not make general assertions
- The lens evaluates alignment with stated goals, not the validity of the goals themselves

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general business assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
