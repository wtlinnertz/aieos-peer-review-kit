# Review DevEx Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-DEVEX

## Purpose

Examines an artifact through a developer experience perspective, identifying poor API design, missing examples, confusing configuration, bad error messages, and friction in development workflow.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-devex is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (API docs, developer guides, onboarding materials) |

## Output

The tool produces structured output conforming to `review-devex-template.md`.

## What to Evaluate

### API Design
- Are API contracts clear and consistent (naming, versioning, error formats)?
- Are APIs intuitive (follows principle of least surprise)?
- Is API versioning strategy defined?
- Are breaking changes handled gracefully (deprecation, migration path)?
- Are API boundaries well-defined (not leaking implementation details)?

### Error Handling and Messages
- Are error responses structured and consistent?
- Do errors provide actionable information (not just "error occurred")?
- Are error codes documented and stable?
- Do errors distinguish between client errors and server errors?
- Are error messages developer-friendly without exposing internals?

### Configuration
- Is configuration documented and discoverable?
- Are configuration defaults sensible?
- Is configuration validated at startup (fail fast, not at runtime)?
- Are environment-specific configurations separated?
- Is there a configuration reference or schema?

### Development Workflow
- Is the local development setup documented?
- Can a new developer get started quickly (setup complexity)?
- Are dependencies managed and reproducible?
- Is the build/test/deploy cycle documented?
- Are there development shortcuts (hot reload, test isolation)?

### Documentation and Examples
- Are APIs documented with examples?
- Are common use cases covered with code samples?
- Is there a getting-started guide?
- Are architectural decisions documented for contributors?
- Are there runnable examples or a sandbox?

### Testing Experience
- Is the test strategy clear (what to test, how to test)?
- Are test utilities and helpers provided?
- Can tests run in isolation without external dependencies?
- Is test feedback fast (reasonable test execution time)?
- Are test patterns consistent and documented?

### SDK and Client Libraries
- Are client libraries provided for major languages (if applicable)?
- Are SDKs idiomatic for their target language?
- Is authentication handled by the SDK (not left to the developer)?
- Are SDK versions aligned with API versions?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Architecture Review | Optional — evaluate developer-facing architecture |
| Technical Design Review | Required — evaluate design from developer perspective |
| Code Review | Required — evaluate implementation developer experience |

## Constraints

- The lens evaluates what is present in the artifact — it does not conduct user research or surveys
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools or products
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general devex assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
