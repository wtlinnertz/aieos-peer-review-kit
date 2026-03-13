# Peer Review Kit — Documentation Index

This kit governs autonomous multi-perspective peer review of artifacts at key lifecycle points. It is Layer 14 of the AIEOS system, operating as cross-cutting governance.

---

## Start Here

| Document | Purpose |
|----------|---------|
| `playbook.md` | End-to-end process definition — read this first |
| `how-to-use-with-ai.md` | AI session setup and tool guidance |
| `how-to-adapt.md` | Organizational adoption guidance |
| `governance-model.md` | AIEOS structural rules (reference) |

---

## Artifact Governing Files

### Peer Review Record (PRR)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/prr-spec.md` | Content rules and 5 hard gates |
| Template | `artifacts/prr-template.md` | PRR structure |
| Prompt | `prompts/prr-prompt.md` | Generation instructions |
| Validator | `validators/prr-validator.md` | Pass/fail evaluation |

---

## Review Lens Tools (docs/tools/)

Each lens has four files: spec, template, prompt, validator.

| Lens | Tool ID | Files |
|------|---------|-------|
| Security | TOOL-REVIEW-SECURITY | `review-security-{spec,template,prompt,validator}.md` |
| Reliability | TOOL-REVIEW-RELIABILITY | `review-reliability-{spec,template,prompt,validator}.md` |
| Performance | TOOL-REVIEW-PERFORMANCE | `review-performance-{spec,template,prompt,validator}.md` |
| Cost | TOOL-REVIEW-COST | `review-cost-{spec,template,prompt,validator}.md` |
| Operability | TOOL-REVIEW-OPERABILITY | `review-operability-{spec,template,prompt,validator}.md` |
| Maintainability | TOOL-REVIEW-MAINTAINABILITY | `review-maintainability-{spec,template,prompt,validator}.md` |
| Compliance | TOOL-REVIEW-COMPLIANCE | `review-compliance-{spec,template,prompt,validator}.md` |
| DevEx | TOOL-REVIEW-DEVEX | `review-devex-{spec,template,prompt,validator}.md` |
| Business Value | TOOL-REVIEW-BUSINESS-VALUE | `review-business-value-{spec,template,prompt,validator}.md` |

---

## Principles

| File | Purpose |
|------|---------|
| `principles/peer-review-principles.md` | Organizational peer review policy (6 principles) |

---

## Examples

`examples/` — Worked examples including a PRR for an architecture review

---

## Boundary Briefings

| Document | Purpose |
|----------|---------|
| `entry-from-pik.md` | Boundary briefing for concept review (DPRD) |
| `entry-from-eek.md` | Boundary briefing for architecture, design, implementation, and code review (SAD, TDD, WDD, ORD) |
| `entry-from-qak.md` | Boundary briefing for integration review (QGR) |
| `entry-from-rek.md` | Boundary briefing for operational readiness review (RP) |
| `entry-from-rrk.md` | Boundary briefing for post-deployment review (RHR) |
| `entry-from-odk.md` | Boundary briefing for incident review (PMR) |

---

## Tests

`tests/kit-test-plan.md` — Structural integrity checks and flow scenarios
