# Research Summary 005 - Veda Database Governance and Hammer Patterns

Status: evidence summary
Source: Claude analysis of `veda-ops-dev/v-ecosystem-docs`
Date captured: 2026-06-04

## Purpose

Extract reusable governance, database, Observatory, provenance, approval, and hammer-test patterns from Veda without copying Veda-specific design into Kaizen.

This file is evidence. It is not doctrine by itself.

## Reusable Veda lessons

1. Docs define governed architecture; the database owns operational state.
2. Database access should be API-only; agents and MCP tools should not receive direct clients or arbitrary SQL.
3. Schema authority should exist as a governing document above migrations and detailed schema specs.
4. Every declared invariant should have a real-execution hammer test.
5. Raw evidence, structured observations, derived interpretations, and governing decisions must remain distinct layers.
6. Agents may identify and package authority-bearing actions but may not authorize them.
7. Paid data pulls need explicit scope, vendor, purpose, spend ceiling, and approval.
8. Supersedence and invalidation are explicit governed events, not silent metadata edits.
9. Failure, partial completion, rollback, and escalation must be represented honestly and logged.
10. JSON is for genuinely variable payloads, not unresolved schema design.

## Schema-authority implications

Before building Observatory tables, Kaizen should define:

- what Postgres is allowed to own
- what Postgres must never own
- project-scoped, intentionally global, and system-metadata table classes
- migration ownership
- JSON discipline
- provenance requirements
- state-transition rules
- retention and recovery expectations

## Hammer-test implications

Kaizen hammer tests should verify real execution and deliberate invalid use across three concerns:

- persistence and atomicity
- contract and response behavior
- boundaries and authorization

Every important capability needs valid-path and invalid-path coverage. High-risk creation and transition surfaces need concurrency probes.

## Governance implications

- Undefined agent actions are forbidden by default.
- Approve/authorize remains human-only.
- Prior approval becomes stale when scope or conditions change.
- Paid external actions are requests until separately approved.
- Supersedence proposals may be drafted by agents but executed only through human approval.
- Failure outcomes belong in append-only audit logs.

## Candidate first hammer areas

- frontmatter completeness
- ID immutability
- note-type registry enforcement
- authority transition rules
- Qdrant rebuild idempotency
- project-scope retrieval isolation
- Observatory result boundary
- no raw rows in Markdown
- no private data in Markdown/Qdrant
- Hermes tool authorization

## Related files

- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `05-specs/operational-postgres-authority.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-permission-matrix.md`
