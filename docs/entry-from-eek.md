# Entry from Engineering Execution Kit (EEK)

**You are here because:** An EEK artifact (SAD, TDD, WDD, or ORD) has passed its own validator and is ready for peer review before freeze.

**Review Points:** Architecture Review (SAD), Technical Design Review (TDD), Implementation Readiness (WDD), Code Review (ORD)

**What you must bring:**

| Review Point | Artifact | Status Required | Where to Find It |
|---|---|---|---|
| Architecture Review | System Architecture Document (SAD-{PROJECT}-{NNN}) | Validated (not yet frozen) | EEK `docs/sdlc/` in the consuming project |
| Technical Design Review | Technical Design Document (TDD-{PROJECT}-{NNN}) | Validated (not yet frozen) | EEK `docs/sdlc/` in the consuming project |
| Implementation Readiness | Work Decomposition Document (WDD-{PROJECT}-{NNN}) | Validated (not yet frozen) | EEK `docs/sdlc/` in the consuming project |
| Code Review | Operational Readiness Document (ORD-{PROJECT}-{NNN}) | Validated (not yet frozen) | EEK `docs/sdlc/` in the consuming project |

**Required and optional lenses by review point:**

| Review Point | Required Lenses | Optional Lenses |
|---|---|---|
| Architecture Review | security, reliability, performance, cost, operability, maintainability | compliance, devex |
| Technical Design Review | security, reliability, performance, maintainability, devex | cost, compliance |
| Implementation Readiness | cost, operability, business-value | maintainability |
| Code Review | security, performance, reliability, maintainability, devex | operability |

**Useful context documents:**

| Review Point | Context Documents |
|---|---|
| Architecture Review | ACF, DPRD (especially non-goals), integration constraints |
| Technical Design Review | SAD (for architectural alignment), ACF constraints |
| Implementation Readiness | SAD, TDD, execution plan, resource constraints |
| Code Review | SAD, TDD, WDD, test results |

**First step in this kit:** `docs/playbook.md` → "Step 0 — Identify Review Point and Select Lenses"

**What changes at this boundary:**

- You shift from engineering execution to multi-perspective review. The focus moves from "does this artifact satisfy its spec?" (EEK validator) to "does this artifact hold up under scrutiny from security, reliability, performance, and other perspectives?"
- Architecture Review (SAD) is the most lens-intensive review point — 6 required lenses. This reflects the high downstream impact of architectural decisions.
- Code Review (ORD) focuses on implementation quality — security, performance, reliability, maintainability, and developer experience are all required.
- Implementation Readiness (WDD) is a lighter review focused on cost, operability, and business value — confirming the work decomposition is practical and aligned.

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Reviewing artifacts out of EEK dependency order | PRK reviews each artifact independently. Review the SAD before the TDD only if the SAD is already validated. Do not wait to batch all EEK artifacts. |
| Providing a frozen artifact for review | PRK operates before freeze. If the artifact is already frozen, peer review is retrospective only. |
| Skipping Architecture Review because "we will catch issues in Code Review" | Architecture Review catches structural issues (single points of failure, missing security boundaries, cost-prohibitive designs) that are expensive to fix after implementation. |
| Not providing context documents | The SAD review benefits significantly from the ACF and DPRD as context. Without context, lenses produce shallow findings. |

**If the artifact has not passed its own validator:**

Stop. Return to EEK, complete validation, and address any blocking issues. PRK requires a validated artifact as input. An unvalidated artifact produces unreliable review findings.

---

*For the full review flow, see `docs/playbook.md`.*
