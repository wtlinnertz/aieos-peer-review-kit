# Entry from Release & Exposure Kit (REK)

**You are here because:** A Release Plan (RP) has passed its own validator and is ready for peer review before freeze.

**Review Point:** Operational Readiness

**What you must bring:**

| Artifact | Status Required | Where to Find It |
|----------|----------------|-----------------|
| Release Plan (RP-{PROJECT}-{NNN}) | Validated (not yet frozen) | REK `docs/sdlc/` in the consuming project |

**Useful context documents:**

| Document | Purpose |
|----------|---------|
| Release Entry Record (RER) | Entry conditions and release scope |
| Release Configuration File (RCF) | Configuration and environment details |
| SLO definitions | Service level objectives for operational review |
| Runbooks (if available) | Operational procedure coverage |

**Required lenses:** operability, reliability, security, cost

**Optional lenses:** compliance

**First step in this kit:** `docs/playbook.md` → "Step 0 — Identify Review Point and Select Lenses"

**What changes at this boundary:**

- You shift from release planning to multi-perspective review of operational readiness. The focus is: is this release plan operationally sound from operability, reliability, security, and cost perspectives?
- The operability lens is the primary lens here — it evaluates deployment complexity, rollback plans, monitoring coverage, and alerting adequacy.
- The reliability lens evaluates whether the release plan accounts for failure scenarios during and after deployment.
- The cost lens evaluates whether the release plan has considered infrastructure costs, particularly for canary/blue-green deployments.

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Reviewing the system instead of the release plan | PRK reviews the RP artifact. Findings should reference RP sections (deployment steps, rollback procedures, monitoring plans). |
| Skipping the cost lens for incremental releases | Even small releases may have cost implications (new infrastructure, increased monitoring, additional environments). |
| Not providing RCF as context | The RP without RCF context means the operability lens cannot evaluate configuration management adequacy. |

**If the RP has not passed its own validator:**

Stop. Return to REK, complete validation, and address any blocking issues. PRK requires a validated artifact as input.

---

*For the full review flow, see `docs/playbook.md`.*
