---
name: analyze
description: Analyze the blast radius of a proposed code change before implementation. Use it to identify affected files, hidden dependencies, contracts, tests, migration needs, and compatibility risks.
argument-hint: "[change request]"
disable-model-invocation: true
context: fork
agent: blast-radius-researcher
effort: high
---

# Blast Radius Analysis

Analyze this proposed change:

> $ARGUMENTS

If the change request is empty or too vague to identify a concrete change, return a short usage example and stop.

Use repository evidence rather than assumptions.

## Investigate

1. Locate the primary implementation entry points.
2. Trace callers, callees, imports, interfaces, inheritance, registrations, generated code, and dependency-injection wiring that can be established from repository files.
3. Identify public or cross-module contracts that may change:
   - APIs and routes
   - DTOs, schemas, and serialization formats
   - database models and migrations
   - events, queues, messages, and background jobs
   - configuration and environment variables
   - CLI flags and file formats
4. Locate relevant tests, fixtures, mocks, snapshots, and test utilities.
5. Check for non-obvious failure modes:
   - backward compatibility
   - data migration or rollout ordering
   - concurrency and race conditions
   - caching and stale state
   - authorization and trust boundaries
   - error handling and retries
   - performance regressions
   - platform-specific behavior
   - feature flags and deployment configuration

Do not invent dependencies you cannot support with repository evidence. Label uncertain findings explicitly.

## Return

Produce a concise report with these sections:

### Change surface
List the files or modules most likely to require modification and why.

Use confidence labels:
- **Direct** — almost certainly changes
- **Likely** — probably changes
- **Watch** — may be affected indirectly

### Dependency chain
Show the important flow from entry point to downstream dependencies using a compact text tree or arrows.

### Contracts and compatibility
Call out API, schema, persistence, event, configuration, or behavioral contracts that must remain compatible or be migrated.

### Tests
List existing tests to run or update, plus the smallest missing tests that would meaningfully reduce risk.

### Risk register
For each material risk, give:
- severity: low / medium / high
- evidence
- mitigation

### Safe implementation order
Give the smallest practical sequence of changes. Prefer reversible steps and compatibility-preserving transitions.

### Unknowns
List only questions that repository inspection could not answer.

End with:

**Blast radius:** Small / Medium / Large  
**Confidence:** Low / Medium / High
