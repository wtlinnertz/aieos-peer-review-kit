# Entry from Reliability & Resilience Kit (RRK)

**You are here because:** A Reliability Health Record (RHR) has passed its own validator and is ready for peer review before freeze.

**Review Point:** Post-Deployment Review

**What you must bring:**

| Artifact | Status Required | Where to Find It |
|----------|----------------|-----------------|
| Reliability Health Record (RHR-{PROJECT}-{NNN}) | Validated (not yet frozen) | RRK `docs/sdlc/` in the consuming project |

**Useful context documents:**

| Document | Purpose |
|----------|---------|
| Release Record (RR) | Release details and deployment observations |
| SLO performance data | Measured service level indicators |
| Monitoring observations | Post-deployment telemetry |

**Required lenses:** reliability, performance, cost, operability

**Optional lenses:** security

**First step in this kit:** `docs/playbook.md` → "Step 0 — Identify Review Point and Select Lenses"

**What changes at this boundary:**

- You shift from reliability assessment to multi-perspective review of the health record. The focus is: does this RHR accurately capture the system's post-deployment health from reliability, performance, cost, and operability perspectives?
- The reliability lens at this point evaluates whether the RHR adequately covers failure modes observed or anticipated in production, not the architecture.
- The performance lens evaluates whether performance observations in the RHR are complete and whether degradation trends are identified.
- The cost lens evaluates whether the RHR captures operational cost implications (resource utilization, scaling behavior).
- The operability lens evaluates whether the RHR identifies operational gaps (missing runbooks, insufficient alerting, monitoring blind spots).

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Reviewing the system instead of the RHR | PRK reviews the RHR artifact. Findings should reference RHR sections, not the system directly. |
| Treating post-deployment review as optional | Post-deployment review catches production realities that earlier review points cannot anticipate. |
| Not providing monitoring data as context | The RHR without monitoring observations means lenses cannot evaluate whether the health assessment is complete. |

**If the RHR has not passed its own validator:**

Stop. Return to RRK, complete validation, and address any blocking issues. PRK requires a validated artifact as input.

---

*For the full review flow, see `docs/playbook.md`.*
