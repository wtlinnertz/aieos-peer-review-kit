# Peer Review Kit — Examples

This directory contains worked examples demonstrating the Peer Review Kit in action.

## Available Examples

### 01 — Architecture Review PRR

`01-prr-architecture-review.md` — A complete Peer Review Record for an Architecture Review of a payment service SAD. Demonstrates:

- Execution of 6 required lenses (security, reliability, performance, cost, operability, maintainability) plus 1 optional lens (devex)
- Realistic findings across multiple lenses
- A conflict between security and performance lenses (TLS overhead vs. latency requirements)
- An overall PASS disposition with medium-severity advisory findings

## How to Use These Examples

1. Read the example PRR to understand the expected output format and quality level.
2. Compare the example against the PRR spec (`docs/specs/prr-spec.md`) to verify it would pass all hard gates.
3. Use the example as a reference when reviewing your own PRR outputs.
