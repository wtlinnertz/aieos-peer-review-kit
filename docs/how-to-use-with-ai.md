# How to Use This Kit with AI

This guide explains how to set up AI sessions for each step in the Peer Review Kit workflow. Follow the session setup instructions precisely — incorrect session setup is the most common cause of poor review quality.

---

## Core Discipline

**One lens per session.** Do not execute multiple lenses in the same session. Lens independence requires isolation.

**Separate generation and validation.** Always validate in a new session. Never ask the AI that generated a lens output or PRR to validate it — this produces self-validation bias.

**Include full documents.** Do not summarize the artifact under review. Provide the complete document.

**Do not share lens outputs between lens sessions.** Cross-lens analysis happens only during PRR aggregation.

---

## Lens Execution Sessions

Execute one session per lens. All lens sessions for a review point can run in parallel.

**Session setup (per lens):**
```
Documents to provide:
1. The validated artifact under review (full document)
2. Context documents for this review point (see playbook §Context Documents)
3. docs/tools/review-{lens-name}-spec.md
4. docs/tools/review-{lens-name}-template.md

Prompt:
"Execute the {lens-name} review lens against the provided artifact.
Follow the prompt in docs/tools/review-{lens-name}-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Reference specific sections of the artifact for every finding.
Do not suggest new scope. Do not rewrite the artifact.
Mark any areas where you lack sufficient context with
[INSUFFICIENT CONTEXT: reason]. Output pure Markdown."
```

**After execution:** Review the lens output. Confirm:
- Findings reference specific sections of the reviewed artifact
- Every finding has severity, description, location, and recommendation
- The lens stayed within its domain (no cross-domain findings)
- No new scope was suggested

**Validation session setup (per lens):**
```
Documents to provide:
1. The lens output (full document)
2. docs/tools/review-{lens-name}-spec.md

Prompt:
"Validate this lens output against the review-{lens-name} spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in
docs/tools/review-{lens-name}-validator.md."
```

---

## PRR Generation Session

After all lens outputs are validated, aggregate into a PRR.

**Session setup:**
```
Documents to provide:
1. All validated lens outputs (full documents)
2. The validated artifact under review (full document)
3. Review point name and selected lenses list (from Step 0)
4. docs/specs/prr-spec.md
5. docs/artifacts/prr-template.md

Prompt:
"Generate a Peer Review Record using the provided lens outputs.
Follow the prompt in docs/prompts/prr-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Identify all conflicts between lens recommendations.
Justify the disposition based on finding severities.
Do not add findings beyond what the lenses produced.
Output pure Markdown."
```

**After generation:** Review the PRR. Confirm:
- All required lenses are included in the Individual Lens Reviews section
- Findings are accurately transcribed from lens outputs (not reinterpreted)
- Conflicts between lenses are explicitly surfaced in the Conflict Analysis section
- Disposition is justified by the evidence (no critical findings for PASS; no unmitigated high findings for PASS)

**Validation session setup:**
```
Documents to provide:
1. The generated PRR (full document)
2. docs/specs/prr-spec.md

Prompt:
"Validate this Peer Review Record against the PRR spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/prr-validator.md."
```

---

## Troubleshooting

**Lens produces no findings**
This is valid — it means the lens found nothing concerning in its domain. The lens output should explicitly state "no findings" with a brief justification. An empty findings table without explanation fails the evidence_grounded gate.

**Lens produces findings outside its domain**
Re-execute the lens with a clearer prompt emphasizing its domain boundaries. A security lens producing performance findings indicates the session context was contaminated or the prompt was not followed.

**PRR shows conflicts that are not real conflicts**
Review the lens outputs directly. If lenses are not actually in conflict (they address different aspects), the PRR aggregation may have misinterpreted the findings. Correct the PRR and re-validate.

**PRR disposition seems too harsh**
Review the disposition logic in the PRR spec. FAIL requires critical findings OR unmitigated high findings. If neither exists, the disposition should be PASS. If the findings are real but the disposition is FAIL, check whether all high findings have documented mitigations.

**Multiple review points needed for the same artifact**
This is unusual. If an artifact triggers more than one review point (e.g., a combined SAD/TDD document), select the review point with the broader required lens set and add any additional required lenses from the other review point as optional.
