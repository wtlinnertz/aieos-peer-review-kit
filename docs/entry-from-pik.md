# Entry from Product Intelligence Kit (PIK)

**You are here because:** A Discovery PRD (DPRD) has passed its own validator and is ready for peer review before freeze.

**Review Point:** Concept Review

**What you must bring:**

| Artifact | Status Required | Where to Find It |
|----------|----------------|-----------------|
| Discovery PRD (DPRD-{PROJECT}-{NNN}) | Validated (not yet frozen) | PIK `docs/sdlc/` in the consuming project |

**Useful context documents:**

| Document | Purpose |
|----------|---------|
| Product Brief | Background and strategic context |
| Discovery Intake | Market context and problem framing |
| Budget constraints (if available) | Cost lens input |

**Required lenses:** business-value, cost, compliance

**Optional lenses:** security, reliability

**First step in this kit:** `docs/playbook.md` → "Step 0 — Identify Review Point and Select Lenses"

**What changes at this boundary:**

- You shift from product discovery to multi-perspective review. The focus moves from "is this a good product artifact?" (PIK validator) to "does this concept hold up under business, cost, and compliance scrutiny?"
- The review operator is now the primary accountable party, not the product owner.
- Concept review is deliberately lighter than later review points — 3 required lenses rather than 6+. The DPRD is a discovery artifact; deep technical review (security, reliability) is optional here because the architecture does not yet exist.

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Reviewing a frozen DPRD | PRK operates before freeze. If the DPRD is already frozen, peer review is retrospective only — it cannot gate the freeze. |
| Expecting technical depth from concept review | The DPRD does not contain architecture or implementation details. Security and reliability lenses at this stage evaluate risk posture, not technical controls. |
| Skipping the cost lens because "it is too early" | Early cost review catches scope that is fundamentally unaffordable before investment in architecture and design. |

**If the DPRD has not passed its own validator:**

Stop. Return to PIK, complete validation, and address any blocking issues. PRK requires a validated artifact as input. An unvalidated artifact produces unreliable review findings.

---

*For the full review flow, see `docs/playbook.md`.*
