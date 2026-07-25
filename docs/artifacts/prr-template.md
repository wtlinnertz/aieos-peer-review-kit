# Peer Review Record

---

## 1. Document Control

| Field | Value |
|-------|-------|
| Artifact ID | PRR-{PROJECT}-{NNN} |
| Owner | {owner} |
| Date | {YYYY-MM-DD} |
| Reviewed Artifact ID | {TYPE}-{PROJECT}-{NNN} |
| Reviewed Artifact Type | {artifact type, e.g., SAD, TDD, DPRD} |
| Reviewed Artifact Status | {Validated / Frozen} |
| Review Point | {review point name from review point table} |
| Executed Lenses | {comma-separated list of lens names} |
| Status | DRAFT |
| Governance Model Version | 1.2 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions, e.g., peer-review-principles v1.0} |

---

## 2. Review Context

### Reviewed Artifact

| Field | Value |
|-------|-------|
| Artifact Type | {type} |
| Artifact ID | {ID} |
| Artifact Status | {Validated / Frozen} |
| Producing Kit | {kit name, e.g., Engineering Execution Kit} |

### Lens Selection

**Required lenses for this review point:**

{List all required lenses from the review point table}

- {lens name} — executed
- {lens name} — executed

**Optional lenses included:**

{List optional lenses that were included, with rationale}

- {lens name} — {rationale for inclusion}

**Optional lenses excluded:**

{List optional lenses that were not included}

- {lens name}

**Context documents provided:**

{List all context documents provided to lens sessions}

- {document name and ID}

---

## 3. Individual Lens Reviews

{One subsection per executed lens. Repeat this block for each lens.}

### 3.x — {Lens Name} (TOOL-REVIEW-{LENS-NAME-UPPER})

**Lens Score:** {1-10} — {one-sentence justification}

**Findings:**

| # | Severity | Title | Location | Description | Recommendation |
|---|----------|-------|----------|-------------|----------------|
| 1 | {critical/high/medium/low} | {finding title} | {section/field in reviewed artifact} | {description of the issue} | {concrete recommendation} |
| 2 | {severity} | {title} | {location} | {description} | {recommendation} |

*If no findings: "No findings. {Brief justification for why this lens found no issues.}"*

**Open Questions:**

{Questions the lens could not resolve from available context}

- {question}

---

## 4. Conflict Analysis

{Identify conflicts between lens recommendations. A conflict exists when one lens recommends an action that another lens would oppose.}

### Conflict {N}: {Brief description}

| Field | Value |
|-------|-------|
| Lenses Involved | {lens A} vs. {lens B} |
| Lens A Position | {what lens A recommends and why} |
| Lens B Position | {what lens B recommends and why} |
| Nature of Conflict | {why these positions are in tension} |
| Suggested Resolution Path | {how to resolve — may be "defer to artifact owner", "escalate to technical lead", or a specific compromise} |

*If no conflicts: "No conflicts identified. {Brief justification — e.g., lenses addressed non-overlapping concerns.}"*

---

## 5. Aggregate Assessment

### Finding Summary

| Severity | Count |
|----------|-------|
| Critical | {N} |
| High | {N} |
| Medium | {N} |
| Low | {N} |
| **Total** | **{N}** |

### High Finding Mitigation Status

{For each high finding, state whether a mitigation is documented in the lens recommendation.}

| Finding # | Lens | Title | Mitigation Documented |
|-----------|------|-------|-----------------------|
| {ref} | {lens name} | {title} | {Yes — recommendation addresses it / No — no mitigation path} |

*If no high findings: "No high severity findings."*

### Disposition

**Disposition: {PASS / FAIL}**

**Justification:**

{Explain why the disposition is PASS or FAIL, referencing the finding summary:}

- Critical findings: {count and status}
- High findings: {count, mitigation status}
- Required lenses: {all executed / any missing}
- Conflicts: {all resolved / any unresolved}

---

## 6. Required Remediations

{Required only when disposition is FAIL. List each blocking finding.}

| # | Finding Reference | Severity | Lens | What Must Change | Reviewed Artifact Location |
|---|-------------------|----------|------|------------------|---------------------------|
| 1 | {lens subsection, finding #} | {critical/high} | {lens name} | {what the artifact owner must address} | {section in reviewed artifact} |

*If disposition is PASS: "N/A — disposition is PASS."*

---

## 7. Completeness Checklist

Before freezing this record, confirm:

- [ ] All required lenses for the review point are represented in §3
- [ ] Every finding includes severity, title, location, description, and recommendation
- [ ] All conflicts between lenses are surfaced in §4
- [ ] Disposition is justified by the finding summary in §5
- [ ] Required remediations (§6) reference specific blocking findings (if FAIL)
- [ ] Reviewed artifact ID and status are accurately recorded
- [ ] Document Control is complete
