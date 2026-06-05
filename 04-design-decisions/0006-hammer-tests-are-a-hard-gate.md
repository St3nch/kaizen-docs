# Decision 0006 - Hammer Tests Are a Hard Gate

Status: accepted
Date: 2026-06-04
Accepted: 2026-06-04
Related research: `03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md`

## Context

Kaizen is accumulating important invariants: canonical boundaries, staging-only writes, authority transitions, Qdrant rebuildability, project isolation, API-only database access, and privacy restrictions. Rules written only in documentation are not sufficient proof that the system enforces them.

## Decision

Kaizen should treat real-execution hammer tests as a hard gate before governed schema, API, retrieval, authority, or agent-tool changes are considered hardened.

Hammer tests are not a substitute for unit or integration tests. They deliberately pressure system boundaries and prove both valid and invalid paths.

## Required hammer categories

1. Persistence and atomicity
2. Contract and response behavior
3. Boundary and authorization enforcement

## Required principles

- clean independent fixtures
- exact assertions
- deliberate invalid-path coverage
- rollback verification
- project-scope isolation
- concurrency probes on high-risk creation and transition surfaces
- readable modules named by bounded concern

## First candidate hammer areas

- frontmatter completeness
- immutable IDs
- note-type registry enforcement
- authority transition enforcement
- Qdrant rebuild idempotency
- Qdrant filter isolation
- Observatory result boundary
- no raw rows in Markdown
- no private data in Markdown/Qdrant
- Hermes tool authorization

## Consequences

- New capabilities without boundary coverage are not hardened.
- Schema changes require hammer review.
- New Hermes tools require authorization hammer coverage.
- Retrieval payload changes require filter/isolation hammer coverage.

## Open questions

- Where hammer code will live.
- What environment qualifies as real execution for the vault-only phase.
- Which early static checks count as validation rather than hammer tests.
- How much of the first hammer suite can exist before Postgres/Qdrant are built.

## Related files

- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/kaizen-validation-gate-spec.md`
