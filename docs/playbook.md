# Peer Review Kit — Playbook

This playbook defines the end-to-end process for the Peer Review Kit (Layer 14). It covers how to move from a validated artifact to a frozen Peer Review Record that gates artifact freeze.

---

## Artifact Flow

```
Upstream: Validated artifact from any kit (PIK, EEK, QAK, REK, RRK, ODK)
         The artifact must have passed its own validator but NOT yet been frozen.
         │
         ▼
Step 0: Identify Review Point + Select Lenses
         │ Determine which review point applies
         │ Select required and optional lenses from the review point table
         ▼
Step 1: Execute Lens Tools
         │ Run each selected lens against the validated artifact
         │ Each lens produces independent output per its tool template
         ▼
Step 2: Aggregate into Peer Review Record (PRR) — generated
         │ Combine all lens outputs, perform conflict analysis
         │ Validate → Freeze
         ▼
Artifact may now proceed to freeze (when PRK is adopted)
```

---

## Upstream Interface

**Upstream:** Any kit at a defined review point
**Input:** A validated (not frozen) artifact, plus supporting context documents

PRK operates at defined review points across the AIEOS pipeline. The trigger is: an artifact has passed its own validator and is ready for freeze. When PRK is adopted, the artifact must receive a PRR PASS before freeze is permitted.

### Review Point Table

| Review Point | Kit | Artifact Reviewed | Required Lenses | Optional Lenses |
|---|---|---|---|---|
| Concept Review | PIK | DPRD | business-value, cost, compliance | security, reliability |
| Architecture Review | EEK | SAD | security, reliability, performance, cost, operability, maintainability | compliance, devex |
| Technical Design Review | EEK | TDD | security, reliability, performance, maintainability, devex | cost, compliance |
| Implementation Readiness | EEK | WDD | cost, operability, business-value | maintainability |
| Code Review | EEK | ORD | security, performance, reliability, maintainability, devex | operability |
| Integration Review | QAK | QGR | reliability, security, performance | operability |
| Operational Readiness | REK | RP | operability, reliability, security, cost | compliance |
| Post-Deployment Review | RRK | RHR | reliability, performance, cost, operability | security |
| Incident Review | ODK | PMR | security, reliability, operability | performance, maintainability |

### Context Documents by Review Point

Each review point benefits from additional context beyond the artifact itself:

| Review Point | Useful Context |
|---|---|
| Concept Review | Product brief, market context, budget constraints |
| Architecture Review | ACF, DPRD non-goals, integration constraints |
| Technical Design Review | SAD (for architectural alignment), ACF constraints |
| Implementation Readiness | SAD, TDD, execution plan, resource constraints |
| Code Review | SAD, TDD, WDD, test results |
| Integration Review | VP, TCRs, SAD integration points |
| Operational Readiness | RER, RCF, SLO definitions, runbooks |
| Post-Deployment Review | RR, SLO data, monitoring observations |
| Incident Review | Incident timeline, RCA, affected services |

---

## Step 0 — Identify Review Point and Select Lenses

### Purpose

Before any lens executes, the review operator identifies which review point applies and which lenses will run. This is a human decision, not an AI decision.

### Process

1. Identify the artifact under review and which kit produced it.
2. Look up the review point in the Review Point Table above.
3. All **required lenses** for that review point must be executed. There are no exceptions.
4. **Optional lenses** may be included based on the operator's judgment about the artifact's risk profile, organizational requirements, or specific concerns.
5. Document the selected lenses before proceeding. This selection is recorded in the PRR's Review Context section.

### Lens Selection Rationale

When including or excluding optional lenses, the operator should consider:
- Is there a known risk in this domain for this initiative?
- Has this lens surfaced findings in previous review points for the same initiative?
- Does the organization's principles file require this lens for this artifact type?

---

## Step 1 — Execute Lens Tools

**Tool location:** `docs/tools/review-{lens-name}-*.md`
**One lens per session** to maintain lens independence.

### Purpose

Each lens tool examines the validated artifact through a specialized perspective and produces structured findings. Lenses operate independently — no lens sees the output of any other lens during execution.

### Process

1. For each selected lens, open a new AI session.
2. Provide to the session:
   - The validated artifact under review (full document)
   - Relevant context documents for this review point (see Context Documents table above)
   - The lens tool spec: `docs/tools/review-{lens-name}-spec.md`
   - The lens tool template: `docs/tools/review-{lens-name}-template.md`
   - The lens tool prompt: `docs/tools/review-{lens-name}-prompt.md`
3. The AI executes the lens and produces output conforming to the lens template.
4. Validate the lens output in a separate session using the lens validator (`docs/tools/review-{lens-name}-validator.md`).
5. On PASS: retain the lens output for PRR aggregation.
6. On FAIL: address the issue (usually missing evidence or scope violations in the lens output) and re-execute the lens.

### Lens Independence Rule

Lenses must execute independently. Do not provide one lens's output to another lens's session. Cross-lens analysis happens only during PRR aggregation (Step 2). This prevents groupthink and ensures each perspective is authentic.

### Parallel Execution

Lenses may execute in parallel because they are independent. All lens sessions for a given review point can run simultaneously without ordering constraints.

For operational guidance on how an orchestrating agent should package context for each lens sub-agent, track completion across all parallel sessions, and reconverge validated outputs into PRR generation, see [`sub-agent-orchestration.md` Pattern 1: Independent Lens Parallelism](../../aieos-governance-foundation/docs/sub-agent-orchestration.md#pattern-1-independent-lens-parallelism-prk).

---

## Step 2 — Generate Peer Review Record

**Artifact:** Peer Review Record (PRR)
**Type:** Generated
**Spec:** `docs/specs/prr-spec.md` (5 hard gates)
**Template:** `docs/artifacts/prr-template.md`
**Prompt:** `docs/prompts/prr-prompt.md`
**Validator:** `docs/validators/prr-validator.md`

### Purpose

The PRR aggregates all lens outputs into a single record. It identifies conflicts between lenses, provides an aggregate assessment, and declares a PASS/FAIL disposition. The PRR is the artifact that gates freeze.

### Inputs

- All validated lens outputs from Step 1
- The validated artifact under review (for traceability)
- The review point name and selected lenses (from Step 0)
- `docs/specs/prr-spec.md`
- `docs/artifacts/prr-template.md`

### Process

1. Collect all validated lens outputs from Step 1.
2. In a new AI session, provide: all lens outputs + the reviewed artifact + review context + `docs/specs/prr-spec.md` + `docs/artifacts/prr-template.md`. Use the PRR prompt (`docs/prompts/prr-prompt.md`).
3. Review the generated PRR. Confirm:
   - All required lenses are included
   - Findings are accurately transcribed from lens outputs
   - Conflicts between lenses are surfaced
   - Disposition is justified by the findings
4. Validate the PRR in a separate session using `docs/validators/prr-validator.md` and the spec.
5. On PASS: the review operator reviews and freezes the PRR.
6. On FAIL: address blocking issues and re-generate or correct; re-validate.

### Disposition Logic

**PASS** — No critical findings. No unmitigated high findings. All required lenses executed. Any conflicts have a resolution path identified.

**FAIL** — One or more critical findings exist, OR high findings lack mitigation, OR required lenses are missing, OR conflicts remain unresolved without a stated path.

### Freeze Points

- PRR must be Frozen before the reviewed artifact can be frozen (when PRK is adopted)
- A frozen PRR is immutable; changes require the re-entry protocol

---

## Step 3 — Artifact Freeze (Downstream)

After the PRR is frozen with a PASS disposition:

1. The reviewed artifact may proceed to its normal freeze process (human review and approval).
2. The PRR ID is recorded in the ER alongside the reviewed artifact's ID.
3. Any findings with severity "medium" or "low" are advisory — they do not block freeze but should be tracked.

After the PRR is frozen with a FAIL disposition:

1. The reviewed artifact may NOT proceed to freeze.
2. The artifact owner must address all critical findings and all unmitigated high findings.
3. After modifications, the artifact must be re-validated by its own validator.
4. A new PRR is generated (re-entry into PRK at Step 0).

For autonomous correction based on PRR findings without human intervention at each iteration, see [`review-convergence-loop.md`](../../aieos-governance-foundation/docs/review-convergence-loop.md) Pattern B. Pattern B takes PRR §6 Required Remediations as correction constraints, re-validates the artifact, and re-executes only affected lenses. Max 3 iterations; escalation to human if convergence fails.

---

## Downstream Handoff

PRK does not hand off to a specific downstream kit. Instead, PRK gates the freeze of artifacts within other kits. The PRR disposition is recorded in the ER and the artifact's review history.

---

## Freeze Points Summary

| Artifact | When Frozen | What It Gates |
|----------|------------|---------------|
| PRR | After PRR validation PASS | Freeze of the reviewed artifact |

---

## Session Discipline

Follow these rules in every AI session:

| Rule | Why |
|------|-----|
| One lens per session | Maintains lens independence; prevents cross-contamination |
| Separate generation and validation sessions | Prevents self-validation bias |
| Include full artifact under review | Do not summarize; the AI needs complete context |
| Include spec and template for generation sessions | The AI generates against the spec, not from memory |
| Include spec and validator for validation sessions | The validator judges against the spec, not against the generated content |
| Do not share lens outputs between lens sessions | Preserves independent perspective |

Do not ask the AI that generated a lens output to also validate it in the same session.

---

## Re-Entry Protocol

When a frozen PRR must be corrected:

1. Identify what changed (new information, artifact modification, lens error).
2. Determine which lenses are affected by the change.
3. Re-execute affected lenses against the updated artifact.
4. Generate a new PRR version aggregating the updated lens outputs.
5. Validate the new PRR.
6. Freeze the new PRR; the old PRR version is superseded.
7. Document the change in the new version's Document Control section.

When a reviewed artifact is modified after PRR freeze:

1. Assess whether the modification affects any PRR findings.
2. If the modification addresses PRR findings: re-execute affected lenses to confirm resolution. Generate a new PRR.
3. If the modification is unrelated to PRR findings: re-execute all required lenses (the artifact has changed, so all lenses must re-evaluate). Generate a new PRR.

---

## Amendment Procedure

A frozen PRR may be corrected in place without re-validation when **all** of the following criteria are met:

1. The correction does not affect any field evaluated by a hard gate.
2. The correction does not change findings, severities, dispositions, or conflict analysis.
3. The correction does not affect the aggregate PASS/FAIL disposition.

**Procedure:** Make the correction and add an Amendment Log entry to the PRR's Document Control section: date, what changed, materiality criterion cited, and who authorized the change. No re-validation is required.

**If there is any ambiguity** about whether a change is non-material, treat it as material and issue a new PRR version. The amendment path must not become a workaround for the re-entry protocol.

---

## Escalation Paths

### PRR FAIL — Disputed Findings

If the artifact owner disputes a PRR finding:

1. The artifact owner documents the dispute with specific reasoning for each disputed finding.
2. The review operator evaluates the dispute against the lens output and artifact content.
3. If the dispute is valid (the finding was based on a misreading of the artifact), re-execute the affected lens with clarified context.
4. If the dispute is invalid (the finding is accurate), the finding stands.
5. Generate a new PRR reflecting the resolution of disputes.

### Lens Conflict — No Resolution Path

If a conflict between lenses cannot be resolved within the PRR:

1. Document the conflict explicitly in the PRR's Conflict Analysis section.
2. Escalate to the initiative owner or technical lead for a decision.
3. Record the decision in the PRR as the resolution path.
4. The PRR may still PASS if the conflict is acknowledged and a resolution path is documented, even if the resolution is deferred.

---

## Deprecation

When an initiative is cancelled or the reviewed artifact is deprecated:

1. Any in-progress PRR is marked as Abandoned.
2. Any frozen PRR remains frozen for audit purposes.
3. No new lens executions are performed.

---

## Maintaining the Engagement Record

The Engagement Record (ER) is a project-level artifact that lives in the consuming project at `docs/engagement/er-{initiative}.md`. It spans all AIEOS layers and is maintained by each kit's operators as work passes through. The ER spec and format are defined in `aieos-governance-foundation/docs/engagement-record-spec.md`.

**PRK maintains the Layer 14 section of the ER.**

### What to Update During Peer Review Operations

| Trigger | ER Update |
|---------|-----------|
| PRR generated for any review point | Add PRR ID to artifact table; note review point and reviewed artifact ID |
| PRR frozen with PASS | Update PRR status to Frozen; note PASS disposition |
| PRR frozen with FAIL | Update PRR status to Frozen; note FAIL disposition and required remediations |
| PRR FAIL resolved and new PRR frozen with PASS | Add new PRR ID; note supersedes previous PRR |
| Key decision: lens conflict resolution | Record decision in ER key decisions section |
| Key decision: disputed finding resolution | Record decision in ER key decisions section |
