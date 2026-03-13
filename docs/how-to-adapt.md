# How to Adapt This Kit

This kit provides the structure, rules, and prompts for multi-perspective peer review governance. Adapting it to your organization means filling in content specific to your context — without modifying the governance structure.

---

## What to Adapt

### Organizational Principles

**Directory:** `docs/principles/`

The principles file contains 6 peer review principles. Review these and adjust the emphasis to match your organization's review culture. You may add principles that reflect your organization's specific requirements. Example additions:

- **regulatory-review-policy.md** — Which regulatory domains require mandatory compliance lens review? What evidence standards apply?
- **architecture-review-standards.md** — What architecture review criteria does your organization enforce beyond the default lenses?
- **risk-appetite-policy.md** — What is your organization's threshold for accepting high-severity findings without remediation?

### Review Point Customization

The default review point table covers 9 review points across 6 kits. Your organization may need:

- **Additional review points** — If your pipeline includes additional artifacts not covered by the default table, add review points to the playbook.
- **Modified lens requirements** — If your organization always requires the compliance lens (e.g., in regulated industries), move compliance from optional to required at all review points.
- **Removed review points** — If your organization does not use a particular kit or artifact type, remove the corresponding review point.

### Lens Customization

The 9 default lenses cover common review perspectives. Your organization may need:

- **Additional lenses** — Create new lens tools following the four-file pattern in `docs/tools/`. Register them in the review point table.
- **Modified lens focus** — Adjust the "what to look for" content in lens prompts to reflect your organization's standards. For example, the security lens prompt could be updated to emphasize your specific threat model categories.
- **Severity definitions** — If your organization uses different severity levels or definitions, update the lens specs and PRR spec to reflect them.

### Adoption Depth

PRK can be adopted incrementally:

- **Full adoption** — PRR required before every artifact freeze at every review point
- **Selective adoption** — PRR required only at high-risk review points (e.g., Architecture Review, Code Review, Operational Readiness)
- **Advisory adoption** — PRR produced but not required for freeze; findings are advisory

---

## What Not to Adapt

### Specs

The specs define what makes an artifact valid. Do not soften hard gates to make PRRs easier to produce. If a hard gate is failing consistently, that usually means the lens outputs are incomplete — not that the gate is wrong.

If you genuinely need to add a hard gate (your organization requires something the spec does not check), add it. Do not remove existing hard gates.

### Validator Logic

Validators evaluate against specs. If a validator is producing unexpected results, check whether the spec accurately captures your requirements — and adjust the spec if needed, not the validator.

### Governance Model

`docs/governance-model.md` is a synchronized copy of the canonical governance model. Do not edit it. If you believe the governance model should change, update `aieos-governance-foundation/governance-model.md` and sync all kit copies.

### Lens Independence

Do not modify the process to share lens outputs between lens execution sessions. Lens independence is a core principle. Cross-lens analysis occurs only during PRR aggregation.

---

## Adding Lens Tools

If your organization needs additional review lenses, follow the four-file system:

1. Write the spec first — define the hard gates, preconditions, postconditions
2. Write the validator — this forces you to verify the spec is evaluable
3. Write the template — output structure only, no content rules
4. Write the prompt — invocation behavior, references spec and template

Register the new lens in the review point table in the playbook. Update the PRR spec if the new lens has special aggregation requirements.

---

## Tool Bindings

This kit is tool-agnostic. If your organization uses specific code analysis tools, security scanners, or performance profilers that can supplement lens execution, create bindings:

```
docs/bindings/
  security-scanner-mapping.md    # Maps security lens to your SAST/DAST tools
  performance-profiler-mapping.md # Maps performance lens to your profiling tools
```

Bindings are not governed artifacts — they have no spec, validator, or prompt. Update them when your tooling changes without touching the governed files.

---

## First-Time Setup Checklist

- [ ] Read `docs/playbook.md` fully before beginning
- [ ] Review and adjust `docs/principles/peer-review-principles.md` for your organization
- [ ] Decide adoption depth (full, selective, or advisory)
- [ ] Identify which review points apply to your current initiative
- [ ] Customize lens requirements in the review point table if needed
- [ ] Run a pilot: execute a PRR for one review point and evaluate the findings quality
- [ ] Iterate: adjust lens prompts based on pilot findings before full adoption
