# Review Lens: Accessibility — Spec

**Tool ID:** TOOL-REVIEW-ACCESSIBILITY
Version: v1.0

---

## Purpose

The accessibility lens examines an artifact for gaps in inclusive design, accessibility compliance, and assistive technology compatibility. It identifies barriers that would prevent users with disabilities from effectively using the system.

---

## Preconditions

- The artifact under review has passed its own validator
- The full artifact is provided (not a summary)
- The review point is one where accessibility is applicable (see Applicable Review Points)

---

## Input

| Parameter | Description |
|-----------|-------------|
| `artifact` | The full text of the artifact under review |
| `review_point` | Which review point this lens is being executed for |
| `context_documents` | Optional supporting documents (DPRD, ACF, SAD, etc.) |

---

## Evaluation Categories

### 1. Perceivability

Can all users perceive the information and interface components?

- Text alternatives for non-text content
- Captions and alternatives for multimedia
- Content adaptable to different presentations without losing meaning
- Sufficient color contrast and non-color-dependent information

### 2. Operability

Can all users operate the interface components and navigation?

- Keyboard accessibility for all functionality
- Sufficient time for user interactions
- No content that could cause seizures or physical reactions
- Navigation mechanisms and wayfinding aids
- Input modalities beyond keyboard (touch, voice, pointer)

### 3. Understandability

Can all users understand the information and interface operation?

- Readable and predictable content
- Consistent navigation and identification
- Input assistance and error prevention
- Clear language and instructions

### 4. Robustness

Is the content robust enough for diverse user agents and assistive technologies?

- Valid, well-formed markup
- Programmatic name, role, and value for UI components
- Status messages available to assistive technologies

### 5. Inclusive Design Patterns

Beyond compliance, does the design consider diverse users?

- Multiple interaction modalities (not mouse-only, not touch-only)
- Responsive design for varied viewport sizes
- Cognitive load considerations
- Internationalization readiness (RTL, character sets, date formats)

---

## Applicable Review Points

| Review Point | Status | Rationale |
|-------------|--------|-----------|
| Architecture Review (SAD) | Optional | Identify architectural decisions that enable or constrain accessibility |
| Technical Design Review (TDD) | Optional | Verify design includes accessible interaction patterns |
| Code Review (ORD) | Optional | Verify implementation meets accessibility requirements |
| Operational Readiness (RP) | Optional | Verify accessibility testing is part of release criteria |

This lens is optional at all review points. Organizations should make it required based on their accessibility commitments, regulatory requirements (ADA, EAA, Section 508), or user base needs.

---

## Constraints

- Evaluate only what is present in the artifact — do not test running software
- Do not suggest new features or expand scope
- Do not redesign the artifact
- Do not reference specific vendor tools or testing platforms in findings
- Findings must cite specific artifact sections, decisions, or gaps
- Do not produce findings outside the accessibility domain
- Reference WCAG 2.1 AA as the baseline standard unless the artifact specifies a different conformance level

---

## Hard Gates

| # | Gate | Rule |
|---|------|------|
| 1 | `artifact_scoped` | Every finding references a specific section, decision, or gap in the artifact under review. No findings based on general assertions or assumptions. |
| 2 | `severity_classified` | Every finding has a severity: critical (blocks users entirely), high (significant barrier), medium (usability degradation), low (minor improvement). |
| 3 | `evidence_grounded` | Every finding cites specific artifact content — a design decision, a component description, a missing consideration. No findings without traceable evidence. |
| 4 | `actionable_output` | Every finding has a concrete, implementable recommendation that addresses the accessibility gap without expanding scope. |
