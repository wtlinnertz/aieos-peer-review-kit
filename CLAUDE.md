# CLAUDE.md — Peer Review Kit

## What This Repository Is

This is the **Peer Review Kit** — an AIEOS kit that governs autonomous multi-perspective peer review of artifacts at key lifecycle points. It is Layer 14 of the AIEOS system, operating as a cross-cutting governance kit that interacts with multiple layers rather than occupying a single sequential position in the pipeline.

PRK runs specialized review lenses against artifacts after they pass their own validator but before freeze. The aggregated output of all applicable lenses is a Peer Review Record (PRR).

## Repository Structure

```
docs/
  principles/          # Organizational peer review policy (input material)
  specs/               # Content rules and hard gates per artifact type
  artifacts/           # Templates
  prompts/             # AI generation prompts
  validators/          # Quality gate definitions
  tools/               # 13 review lens tools (four-file sets)
  playbook.md          # End-to-end process definition
  index.md             # Documentation entry point
  how-to-adapt.md      # Organizational adoption guidance
  how-to-use-with-ai.md # AI tool usage guide
  governance-model.md  # AIEOS structural rules (reference)
  entry-from-pik.md    # Boundary briefing from Product Intelligence Kit
  entry-from-eek.md    # Boundary briefing from Engineering Execution Kit
  entry-from-qak.md    # Boundary briefing from Quality Assurance Kit
  entry-from-rek.md    # Boundary briefing from Release & Exposure Kit
  entry-from-rrk.md    # Boundary briefing from Reliability & Resilience Kit
  entry-from-odk.md    # Boundary briefing from Operational Diagnostics Kit
examples/              # Worked examples
tests/                 # Structural integrity checks
```

## Artifact Type

This kit produces one governed artifact type:

- **Peer Review Record (PRR)** — Aggregated output of all applicable review lenses executed against a specific artifact at a specific review point. Contains individual lens reviews, conflict analysis, and an aggregate PASS/FAIL disposition.

The PRR has exactly four governing files: spec, template, prompt, validator.

## Review Lens Tools (13 tools in docs/tools/)

Each lens is a governed tool with four files (spec, template, prompt, validator):

1. **review-security** — Authentication, authorization, secrets, injection, attack surface
2. **review-reliability** — Single-failure resilience, retry handling, timeouts, circuit breakers, health checks, backup and recovery
3. **review-performance** — Latency, scaling, resource efficiency, bottlenecks
4. **review-cost** — Infrastructure cost, operational cost, TCO implications
5. **review-operability** — Deployment complexity, rollback plans, runbook coverage, operational testing, change management readiness
6. **review-maintainability** — Coupling, cohesion, tech debt, modularity, readability
7. **review-compliance** — Regulatory controls, audit requirements, data sovereignty
8. **review-devex** — Developer workflow friction, API ergonomics, documentation gaps
9. **review-business-value** — ROI, scope alignment, feasibility, user value
10. **review-accessibility** — Perceivability, operability, understandability, robustness, inclusive design (WCAG 2.1 AA baseline)
11. **review-observability** — Structured logging, metrics, tracing, alerting, dashboards, observability testing
12. **review-resilience** — Multi-failure scenarios, chaos readiness, blast radius containment, cascading failure prevention, self-healing, recovery velocity
13. **review-adversarial** — Assumption fragility, failure cascades, boundary conditions, misuse scenarios, implicit dependencies, invariant violations, information asymmetry (5 hard gates including minimum findings requirement)

## Review Points (Trigger Mapping)

| Review Point | Kit | Artifact Reviewed | Required Lenses | Optional Lenses |
|---|---|---|---|---|
| Concept Review | PIK | DPRD | business-value, cost, compliance | security, reliability |
| Architecture Review | EEK | SAD | security, reliability, resilience, performance, cost, operability, maintainability | compliance, devex, accessibility, observability, adversarial |
| Technical Design Review | EEK | TDD | security, reliability, performance, maintainability, devex | cost, compliance, accessibility, observability, resilience, adversarial |
| Implementation Readiness | EEK | WDD | cost, operability, business-value | maintainability |
| Code Review | EEK | ORD | security, performance, reliability, maintainability, devex | operability, accessibility, observability, adversarial |
| Integration Review | QAK | QGR | reliability, security, performance | operability, resilience, adversarial |
| Operational Readiness | REK | RP | operability, observability, reliability, security, cost | compliance, accessibility, resilience, adversarial |
| Post-Deployment Review | RRK | RHR | reliability, observability, performance, cost, operability | security, resilience |
| Incident Review | ODK | PMR | security, reliability, resilience, operability | performance, maintainability, observability |

## Key Rules

- **Specs are the source of truth** — prompts and validators reference specs, never inline rules
- **Validators judge, they do not help** — no suggestions, no redesign
- **Lens independence** — each lens evaluates independently with no cross-lens influence during execution
- **Separate generation and validation** — different AI sessions to prevent self-validation bias
- **No scope expansion** — lenses evaluate what exists; they do not suggest new scope
- **No inferred information** — mark missing information explicitly, do not fill gaps
- **Cross-cutting triggers** — PRRs are triggered at different pipeline points, not in a single linear flow
- **Governance model sync** — `docs/governance-model.md` is a synchronized copy of `aieos-governance-foundation/governance-model.md` (canonical authority). Do not edit kit copy directly; update `aieos-governance-foundation` first, then sync all kit copies to match exactly. See governance-model.md §15 for versioning and change protocol.
- **Engagement Record** — PRK maintains the Layer 14 section of the project's ER. Add artifact IDs as they freeze. See `docs/playbook.md §Maintaining the Engagement Record` and `aieos-governance-foundation/docs/engagement-record-spec.md`.

## Artifact Flow

```
Step 0: Identify review point + select required and optional lenses
Step 1: Execute each lens tool against the validated artifact
Step 2: Aggregate lens outputs into PRR → generate PRR
Step 3: Validate PRR → freeze
        → Artifact may now be frozen (when PRK is adopted)
```

## Boundary Contracts

- **Upstream:** Receives validated (not yet frozen) artifacts from PIK (DPRD), EEK (SAD, TDD, WDD, ORD), QAK (QGR), REK (RP), RRK (RHR), ODK (PMR). The artifact must have passed its own validator before PRK is invoked.
- **Downstream:** Produces a frozen PRR that gates artifact freeze. A PRR with PASS disposition allows the reviewed artifact to proceed to freeze. A PRR with FAIL disposition requires the artifact owner to address findings before freeze.

## File Naming

| Type | Pattern |
|------|---------|
| Spec | `{type}-spec.md` |
| Template | `{type}-template.md` |
| Prompt | `{type}-prompt.md` |
| Validator | `{type}-validator.md` |
| Tool files | `{tool-name}-{role}.md` in `docs/tools/` |

## When Working on This Kit

- Read the playbook (`docs/playbook.md`) for the full process definition
- Read the governance model (`docs/governance-model.md`) for structural rules
- Check `docs/how-to-use-with-ai.md` for session setup instructions
- Reference `examples/` for worked examples

## Building or Auditing AIEOS Kits

- `aieos-governance-foundation/docs/kit-structure-standard.md` — compliance checklist for building and auditing kits
- `aieos-governance-foundation/docs/philosophy.md` — design rationale for governance model decisions
- `aieos-governance-foundation/docs/layer-model.md` — layer model and kit registry
