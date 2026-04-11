# Entry from Quality Assurance Kit (QAK)

**You are here because:** A Quality Gate Record (QGR) has passed its own validator and is ready for peer review before freeze.

**Review Point:** Integration Review

**What you must bring:**

| Artifact | Status Required | Where to Find It |
|----------|----------------|-----------------|
| Quality Gate Record (QGR-{PROJECT}-{NNN}) | Validated (not yet frozen) | QAK `docs/sdlc/` in the consuming project |

**Useful context documents:**

| Document | Purpose |
|----------|---------|
| Verification Plan (VP) | Test scope and acceptance criteria |
| Test Campaign Records (TCRs) | Execution evidence |
| SAD integration points | Cross-component verification context |

**Required lenses:** reliability, security, performance

**Optional lenses:** operability

**First step in this kit:** `docs/playbook.md` → "Step 0 — Identify Review Point and Select Lenses"

**What changes at this boundary:**

- You shift from quality verification to multi-perspective review of the quality disposition itself. The focus is: does the QGR's quality judgment hold up under reliability, security, and performance scrutiny?
- The reliability lens at this point evaluates whether the test campaign adequately covered failure modes, not the system architecture itself.
- The security lens evaluates whether security testing was sufficient, not whether the system is secure (that is the SCK's domain).
- The performance lens evaluates whether performance testing was adequate for the stated acceptance criteria.

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Reviewing the system instead of the QGR | PRK reviews the QGR artifact. Lens findings should reference QGR sections, not the system under test directly. |
| Duplicating QAK validation | PRK does not re-validate the QGR against its spec. It reviews the QGR through reliability, security, and performance perspectives. Different questions. |
| Skipping integration review for PASS-disposition QGRs | Even a PASS QGR may have blind spots visible to peer review (e.g., missing failure mode coverage, insufficient security test scope). |

**If the QGR has not passed its own validator:**

Stop. Return to QAK, complete validation, and address any blocking issues. PRK requires a validated artifact as input.

---

*For the full review flow, see `docs/playbook.md`.*
