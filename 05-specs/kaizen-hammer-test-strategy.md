# Spec - Kaizen Hammer-Test Strategy

Status: implemented baseline
Date: 2026-06-04
Updated: 2026-06-09
Related decision: `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
Related decision: `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`

## Purpose

Define how Kaizen will prove important invariants through real execution and deliberate boundary pressure.

## Test layers

### Level 1 - Static validation

Run frequently and before promotion:

- frontmatter parse
- required fields
- enum validity
- note-type structure
- ID/reference integrity
- prohibited content patterns
- link checks

### Level 2 - Promotion/reference validation

Run before staged content becomes canonical:

- all source and related-note IDs resolve
- Observatory result IDs resolve through approved API
- authority transitions have human approval evidence
- supersedence targets and rationale exist
- protected files were not modified by unauthorized actors

### Level 3 - Hammer tests

Run before governed schema, API, retrieval, authority, or agent-tool changes:

- real writes and rollback
- exact response shapes
- negative-path behavior
- scope isolation
- concurrency on high-risk creation/transition paths
- audit-log completeness

## First 10 candidate hammer tests

### HT-01 Frontmatter completeness

Compliant notes pass; each missing required field fails with a specific error.

### HT-02 ID immutability

Changing an existing note's stable prefixed ULID fails validation and does not silently create a new identity. Reusing an archived or deleted ID also fails.

### HT-03 Note-type registry enforcement

Unknown types and missing type-specific sections fail explicitly.

### HT-04 Review and authority transitions

Agent-authored notes cannot become approved, accepted, or implementation-ready without human approval evidence.

### HT-05 Qdrant rebuild idempotency

Two rebuilds from unchanged Markdown produce identical point identities and payloads; one changed note changes only its own points.

### HT-06 Qdrant filter isolation

Project/domain filters prevent cross-scope retrieval; superseded/rejected content is excluded by default; unsupported filters fail fast.

### HT-07 Observatory result boundary

Agents can retrieve governed result summaries but cannot retrieve arbitrary raw observation rows or mutate published results.

### HT-08 No raw rows in Markdown

Oversized tables, bulk JSON, crawl payloads, and observation dumps are blocked from canonical notes.

### HT-09 No private data in Markdown/Qdrant

Known private/customer/secret patterns block promotion and indexing and create escalation records.

### HT-10 Hermes authorization boundary

Hermes has reliable canonical read/search access but no unapproved canonical mutation, verdict, promotion, Git, SQL, or Qdrant-write capability. The staging-create wrapper enforces create-new behavior, final-path confinement, operating-system permissions, and append-only evidence. Relative traversal, absolute paths, UNC and extended-length forms, symbolic links, Windows junctions, overwrite attempts, and sibling-repository writes are rejected and logged.

### HT-11 Document-contract enforcement

Source evidence and interpretation remain separated; claims contain one core assertion; audits reject agent-issued verdicts; task packets reject missing primary specs, validation, or completion-report sections.

### HT-12 Scoped approval staleness

A material change inside reviewed paths or interfaces stales the approval. An unrelated repository commit and a formatting-only change outside the reviewed boundary do not automatically stale it.

### HT-13 Promotion interruption and recovery

Crash or failure is injected after intent append, temporary write, canonical installation, and completion append. Recovery produces one explicit committed, rollback, conflict, or recovery-required result without silently losing the staged artifact or overwriting divergent canonical content.

### HT-14 Windows path confinement

The staging wrapper rejects traversal, absolute, drive-relative, UNC, device, extended-length, symbolic-link, junction, mixed-separator, overwrite, and parent-path replacement probes. Final resolved targets remain strict descendants of the configured staging root.
## Hammer module rules

- independently runnable from clean fixtures
- explicit setup and teardown
- exact expected errors/statuses
- valid and invalid path coverage
- no hidden ordering dependency
- concurrency probes for duplicate creation and promotion
- append-only result/report output

## Build order

1. Implement static Markdown validation.
2. Implement reference/promotion validation.
3. Create early file-based hammer fixtures.
4. Add Qdrant hammer modules when retrieval exists.
5. Add Observatory hammer modules before external agent access.
6. Add tool-boundary hammer modules before any Hermes write/API expansion.

## Open questions

- Hammer implementation language and repository location.
- Whether tests run in this docs repo or a future Kaizen tooling repo.
- Exact private-data scanning posture and false-positive handling.
- What minimum environment qualifies as real execution.
- How hammer results are stored and reviewed.

## Implemented-baseline evidence

This contract was exercised by the Milestones 1 through 4 platform and canonical-vault work, including deterministic validation, six ordered promotions, two governed amendments, hammer tests, and Result 062 closure. Remaining open questions are future refinements rather than blockers to the implemented first-slice baseline.

## Related files

- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/operational-postgres-authority.md`
- `07-hermes/hermes-permission-matrix.md`
