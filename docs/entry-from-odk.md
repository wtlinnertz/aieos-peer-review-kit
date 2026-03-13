# Entry from Operational Diagnostics Kit (ODK)

**You are here because:** A Postmortem Record (PMR) has passed its own validator and is ready for peer review before freeze.

**Review Point:** Incident Review

**What you must bring:**

| Artifact | Status Required | Where to Find It |
|----------|----------------|-----------------|
| Postmortem Record (PMR-{PROJECT}-{NNN}) | Validated (not yet frozen) | ODK `docs/sdlc/` in the consuming project |

**Useful context documents:**

| Document | Purpose |
|----------|---------|
| Incident timeline | Chronological event sequence |
| Root cause analysis | Contributing factors and root causes |
| Affected services inventory | Scope of impact |

**Required lenses:** security, reliability, operability

**Optional lenses:** performance, maintainability

**First step in this kit:** `docs/playbook.md` → "Step 0 — Identify Review Point and Select Lenses"

**What changes at this boundary:**

- You shift from incident investigation to multi-perspective review of the postmortem. The focus is: does this PMR adequately address the incident from security, reliability, and operability perspectives?
- The security lens evaluates whether the PMR identified security-related root causes and whether remediation actions address security gaps.
- The reliability lens evaluates whether the PMR captured failure modes, whether the root cause analysis is thorough, and whether preventive measures are adequate.
- The operability lens evaluates whether the PMR addresses operational process gaps (detection latency, escalation effectiveness, communication).

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Reviewing the incident instead of the PMR | PRK reviews the PMR artifact. Findings should reference PMR sections (timeline, root cause, action items), not the incident itself. |
| Skipping the security lens for non-security incidents | Many incidents have security dimensions (unauthorized access during incident response, data exposure during outage) that the security lens can surface. |
| Not providing the incident timeline as context | The PMR without timeline context means lenses cannot evaluate whether the postmortem's chronology is complete. |

**If the PMR has not passed its own validator:**

Stop. Return to ODK, complete validation, and address any blocking issues. PRK requires a validated artifact as input.

---

*For the full review flow, see `docs/playbook.md`.*
