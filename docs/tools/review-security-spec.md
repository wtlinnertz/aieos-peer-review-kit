# Review Security Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-SECURITY

## Purpose

Examines an artifact through a security perspective, identifying authentication gaps, authorization weaknesses, secrets exposure, injection risks, missing encryption, insecure defaults, overprivileged access, and supply chain risk.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-security is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens (e.g., Architecture Review) |
| `context_documents` | No | Supporting documents (ACF, threat model, security principles) |

## Output

The tool produces structured output conforming to `review-security-template.md`.

## What to Evaluate

### Authentication and Identity
- Are authentication mechanisms specified for all entry points?
- Is session management defined (token lifecycle, rotation, revocation)?
- Are service-to-service authentication patterns specified?
- Are there unauthenticated paths that should be authenticated?

### Authorization and Access Control
- Is authorization defined at appropriate granularity (resource, operation, field)?
- Are role definitions and permission boundaries explicit?
- Is the principle of least privilege applied?
- Are there overprivileged default roles or permissions?
- Is there separation of duties where required?

### Secrets and Credential Management
- Are secrets stored outside the codebase (not hardcoded)?
- Is secrets rotation defined?
- Are default credentials eliminated?
- Is there a key management strategy?

### Input Validation and Injection
- Is input validation defined for all external inputs?
- Are parameterized queries or equivalent protections specified?
- Is output encoding defined to prevent XSS?
- Are file upload restrictions specified?

### Encryption and Data Protection
- Is encryption at rest defined for sensitive data?
- Is encryption in transit (TLS) required for all communication?
- Are cryptographic algorithm choices appropriate (not deprecated)?
- Is data classification defined (what is sensitive)?

### Attack Surface
- Are unnecessary endpoints, services, or features disabled?
- Is the external attack surface minimized?
- Are debug/diagnostic endpoints disabled in production?
- Is there a dependency security posture (known vulnerabilities)?

### Insecure Defaults
- Are default configurations secure (not permissive)?
- Are error messages non-revealing (no stack traces, internal paths)?
- Are CORS, CSP, and security headers defined?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Concept Review | Optional — evaluate security risk posture of the concept |
| Architecture Review | Required — evaluate security architecture decisions |
| Technical Design Review | Required — evaluate security in technical design |
| Code Review | Required — evaluate security in implementation |
| Integration Review | Required — evaluate security testing adequacy |
| Operational Readiness | Required — evaluate security in deployment plan |
| Post-Deployment Review | Optional — evaluate security in production health |
| Incident Review | Required — evaluate security dimensions of the incident |

## Constraints

- The lens evaluates what is present in the artifact — it does not perform penetration testing or code scanning
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools or products
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general security assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
