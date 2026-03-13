# Review Cost Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-COST

## Purpose

Examines an artifact through a cost perspective, identifying over-provisioned resources, unnecessary redundancy, expensive API calls, missing cost controls, resource waste, and unoptimized storage.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-cost is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (budget constraints, infrastructure specs, cost benchmarks) |

## Output

The tool produces structured output conforming to `review-cost-template.md`.

## What to Evaluate

### Resource Provisioning
- Are compute resources sized appropriately for expected load (not over-provisioned)?
- Is auto-scaling defined to avoid persistent over-provisioning?
- Are there always-on resources that could be scheduled or on-demand?
- Are development and staging environments right-sized (not production-scale)?

### Redundancy vs. Cost
- Is redundancy proportional to criticality (not uniform high redundancy)?
- Are multi-region deployments justified by availability requirements?
- Are hot standby resources justified, or would warm/cold standby suffice?
- Is there redundancy that exceeds the stated SLO requirements?

### API and Service Costs
- Are expensive external API calls minimized (batched, cached, throttled)?
- Are there pay-per-call services with no usage caps or budgets?
- Are there cheaper alternatives for non-critical operations?
- Is there awareness of cost scaling with traffic growth?

### Cost Controls and Governance
- Are budget alerts or cost caps defined?
- Are resource tagging and cost allocation defined?
- Is there a cost review cadence?
- Are there runaway-cost scenarios identified (unbounded scaling, uncontrolled data growth)?

### Storage Optimization
- Is data lifecycle management defined (archival, deletion, tiering)?
- Are storage classes appropriate (hot vs. cold vs. archive)?
- Is there unbounded data growth without retention policy?
- Are logs, metrics, and backups sized and retained appropriately?

### Operational Cost
- Are operational costs considered (monitoring, logging, alerting infrastructure)?
- Are staffing/on-call implications identified?
- Is the total cost of ownership (TCO) estimated, not just infrastructure cost?
- Are there hidden costs (license fees, support contracts, data transfer)?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Concept Review | Required — evaluate cost feasibility of the concept |
| Architecture Review | Required — evaluate cost implications of architecture |
| Implementation Readiness | Required — evaluate cost of planned work |
| Operational Readiness | Required — evaluate deployment cost |
| Post-Deployment Review | Required — evaluate actual vs. projected cost |

## Constraints

- The lens evaluates what is present in the artifact — it does not perform cost modeling or forecasting
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools, pricing, or products
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general cost assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
