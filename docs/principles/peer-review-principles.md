# Peer Review Principles

Version: v1.0

These principles define the organizational policy for how peer review operates within the AIEOS framework. They are input material for PRR generation and lens execution — not hard gates themselves. Hard gates are defined in specs.

---

## Principle 1: Lens Independence

Each review lens evaluates the artifact independently. No lens sees the output of any other lens during its execution. Cross-lens analysis occurs only during PRR aggregation.

**Rationale:** Independent evaluation prevents groupthink. If a security lens knows the performance lens flagged TLS overhead, it may self-censor its encryption recommendations. Authentic perspectives require isolation.

**Enforcement:** Lens execution sessions must not include outputs from other lenses. Violation is detectable by reviewing session inputs.

---

## Principle 2: Actionable Findings

Every finding must be specific enough to act on. A finding includes: what is wrong, where it is in the artifact, why it matters (severity), and what to do about it (recommendation).

**Rationale:** Vague findings ("consider improving security") waste the artifact owner's time and provide no clear path to resolution. Peer review adds value only when findings are concrete.

**Enforcement:** Lens specs require severity, location, description, and recommendation for every finding. The PRR spec requires finding actionability as a hard gate.

---

## Principle 3: Conflict Transparency

When two or more lenses produce recommendations that conflict with each other, the conflict must be surfaced explicitly in the PRR. Neither lens recommendation is silently dropped or subordinated.

**Rationale:** Real engineering involves tradeoffs. Security may recommend encryption that performance flags as latency overhead. Cost may recommend smaller instances that reliability flags as insufficient capacity. These tensions are valuable — hiding them produces false consensus.

**Enforcement:** The PRR spec requires a Conflict Analysis section. The conflict_surfacing hard gate verifies that conflicting recommendations are identified with both perspectives.

---

## Principle 4: Proportional Depth

Lens depth scales with artifact significance. A concept-level DPRD receives a lighter review than a production architecture SAD. The review point table defines which lenses are required at each point, and optional lenses are added based on risk profile.

**Rationale:** Applying the full weight of 9 lenses to every artifact at every stage is wasteful and produces noise. Early-stage artifacts benefit from broad-but-shallow review; later-stage artifacts benefit from deep-and-focused review.

**Enforcement:** The review point table in the playbook defines required vs. optional lenses per review point. Step 0 of the playbook requires explicit lens selection with rationale.

---

## Principle 5: Evidence Over Opinion

Findings must cite specific content from the artifact under review. General assertions ("this architecture is fragile") without pointing to specific sections, decisions, or gaps in the artifact are not valid findings.

**Rationale:** Peer review that relies on general impressions rather than artifact evidence is not reproducible and not actionable. Citing specific content ensures the finding is grounded and verifiable.

**Enforcement:** Lens specs require the evidence_grounded hard gate — findings must reference specific artifact sections. The PRR spec inherits this through finding_actionability.

---

## Principle 6: Review Scope Discipline

Lenses evaluate what exists in the artifact. They do not suggest new features, new scope, or new requirements. A lens may identify that something is missing (e.g., "no retry strategy is defined for the payment service"), but it must not prescribe what the missing thing should be beyond a high-level recommendation.

**Rationale:** Peer review is not design. If a lens starts designing solutions, it blurs the boundary between review and generation. The artifact owner retains design authority. Lenses provide observations and recommendations, not blueprints.

**Enforcement:** Lens specs require the artifact_scoped hard gate — findings must reference the reviewed artifact. Lens prompts explicitly instruct: do not suggest new scope, do not rewrite the artifact.
