# aieos-peer-review-kit

**Layer 14 of the AIEOS system — Peer Review (Cross-Cutting Governance)**

This kit governs autonomous multi-perspective peer review by running specialized review lenses against artifacts at key lifecycle points. It produces a Peer Review Record (PRR) that aggregates findings from all applicable lenses and declares a PASS/FAIL disposition that gates artifact freeze.

---

## What This Kit Does

Artifacts in the AIEOS pipeline pass through their own validators, but validators check structural compliance — they verify that the artifact satisfies its spec. Peer review adds a different dimension: does the artifact hold up under scrutiny from multiple specialized perspectives?

- **Multi-lens evaluation** — 9 specialized lenses (security, reliability, performance, cost, operability, maintainability, compliance, devex, business-value) each examine the artifact through their domain expertise
- **Conflict surfacing** — When lenses disagree (e.g., security recommends encryption that performance flags as a bottleneck), the conflict is explicitly identified with both perspectives
- **Proportional depth** — Not every lens applies at every review point; the review point determines which lenses are required and which are optional
- **Actionable findings** — Every finding includes severity, location in the artifact, and a concrete recommendation

---

## Artifact Type

| Artifact | Purpose |
|----------|---------|
| Peer Review Record (PRR) | Aggregated output of all lens reviews for a specific artifact at a specific review point, with conflict analysis and PASS/FAIL disposition |

The PRR has exactly four governing files: spec, template, prompt, validator.

---

## Review Points

| Review Point | Kit | Artifact Reviewed |
|---|---|---|
| Concept Review | PIK | DPRD |
| Architecture Review | EEK | SAD |
| Technical Design Review | EEK | TDD |
| Implementation Readiness | EEK | WDD |
| Code Review | EEK | ORD |
| Integration Review | QAK | QGR |
| Operational Readiness | REK | RP |
| Post-Deployment Review | RRK | RHR |
| Incident Review | ODK | PMR |

---

## Quick Start

1. Read `docs/playbook.md` — the complete process definition
2. Read `docs/how-to-use-with-ai.md` — session setup and AI tool guidance
3. See `examples/` — worked examples

---

## Repository Structure

```
docs/
  principles/          # Organizational peer review policy (input material)
  specs/               # Content rules and hard gates for PRR
  artifacts/           # PRR template
  prompts/             # PRR generation prompt
  validators/          # PRR validator
  tools/               # 9 review lens tools (four-file sets each)
  playbook.md          # End-to-end process definition
  index.md             # Documentation entry point
  how-to-adapt.md      # Organizational adoption guidance
  how-to-use-with-ai.md # AI tool usage guide
  governance-model.md  # AIEOS structural rules (reference)
  entry-from-*.md      # Boundary briefings from upstream kits
examples/              # Worked examples
tests/
  kit-test-plan.md     # Structural integrity checks and flow scenarios
CLAUDE.md              # AI operating instructions
```

---

## AIEOS Layer

| Layer | Kit | Category |
|-------|-----|----------|
| 2. Product Intelligence | `aieos-product-intelligence-kit` | Pipeline |
| 4. Engineering Execution | `aieos-engineering-execution-kit` | Pipeline |
| 9. Quality Assurance | `aieos-quality-assurance-kit` | Cross-Cutting |
| 5. Release & Exposure | `aieos-release-exposure-kit` | Pipeline |
| 6. Reliability & Resilience | `aieos-reliability-resilience-kit` | Pipeline |
| 8. Operational Diagnostics | `aieos-operational-diagnostics-kit` | Operational |
| **14. Peer Review** | **`aieos-peer-review-kit`** | **Cross-Cutting** |

See `aieos-governance-foundation/docs/layer-model.md` for the full layer model.
