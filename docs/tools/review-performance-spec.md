# Review Performance Lens — Specification

Version: v1.0

Tool ID: TOOL-REVIEW-PERFORMANCE

## Purpose

Examines an artifact through a performance perspective, identifying N+1 query patterns, unbounded lists, missing pagination, synchronous operations where async is needed, missing caching strategy, poor algorithm choices, and resource leaks.

## Preconditions

- The artifact under review has passed its own validator
- The artifact type matches a review point where review-performance is required or selected as optional
- The artifact is provided in full (not summarized)

## Input

| Field | Required | Description |
|-------|----------|-------------|
| `artifact` | Yes | The full validated artifact under review |
| `review_point` | Yes | The review point triggering this lens |
| `context_documents` | No | Supporting documents (performance SLOs, load projections, ACF constraints) |

## Output

The tool produces structured output conforming to `review-performance-template.md`.

## What to Evaluate

### Query and Data Access Patterns
- Are there N+1 query patterns (iterating and querying per item)?
- Are batch operations used where appropriate?
- Are database indexes specified for high-frequency query paths?
- Are connection pools sized appropriately?
- Are read-heavy paths separated from write-heavy paths where beneficial?

### Pagination and Bounding
- Are list endpoints paginated?
- Are result sets bounded (max page size, max total results)?
- Are there unbounded iterations or aggregations?
- Are streaming or cursor-based patterns used for large data sets?

### Synchronous vs. Asynchronous
- Are long-running operations handled asynchronously?
- Are blocking I/O calls identified and handled appropriately?
- Are message queues or event-driven patterns used where synchronous processing creates bottlenecks?
- Is there unnecessary synchronous coupling between services?

### Caching Strategy
- Is there a caching strategy for frequently accessed, infrequently changed data?
- Are cache invalidation rules defined?
- Are cache TTLs appropriate (not stale, not too short)?
- Is cache size bounded?
- Are cache miss patterns identified?

### Algorithm and Data Structure Choices
- Are algorithm complexities appropriate for expected data volumes?
- Are there O(n^2) or worse patterns operating on large data sets?
- Are data structures chosen for the access patterns (hash maps for lookups, sorted structures for range queries)?
- Are there unnecessary sorting, filtering, or transformation passes?

### Resource Management
- Are connections, file handles, and memory allocations properly bounded and released?
- Are there potential memory leaks (unbounded caches, growing collections)?
- Are thread/goroutine/async task pools bounded?
- Is resource cleanup defined for error paths?

### Latency and Throughput
- Are latency budgets defined for critical paths?
- Are there serial dependency chains that could be parallelized?
- Are hot paths identified and optimized?
- Are there unnecessary network round trips?

## Applicable Review Points

| Review Point | Relevance |
|---|---|
| Architecture Review | Required — evaluate performance architecture |
| Technical Design Review | Required — evaluate performance in design |
| Code Review | Required — evaluate performance in implementation |
| Integration Review | Required — evaluate performance testing adequacy |
| Post-Deployment Review | Required — evaluate performance in production |
| Incident Review | Optional — evaluate performance aspects of the incident |

## Constraints

- The lens evaluates what is present in the artifact — it does not perform load testing or profiling
- The lens does not suggest new features or scope beyond what the artifact covers
- The lens reports findings — it does not redesign the artifact
- The lens does not reference specific vendor tools or products
- Findings must cite specific artifact sections, not make general assertions

## Hard Gates

| Gate | Rule |
|------|------|
| `artifact_scoped` | Every finding references a specific section, decision, or gap in the reviewed artifact |
| `severity_classified` | Every finding has a severity level: critical, high, medium, or low |
| `evidence_grounded` | Every finding cites specific content from the artifact — not general performance assertions |
| `actionable_output` | Every finding includes a concrete, implementable recommendation |
