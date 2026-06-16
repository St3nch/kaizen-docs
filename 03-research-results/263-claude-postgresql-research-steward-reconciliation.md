---
id: kz-result-01KV6LPQ8M2N7R5C3Y9P4T6BZ0
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-16T03:25:00Z
updated: 2026-06-16T03:25:00Z
review_status: steward-reviewed
authority: proposed
summary: "Reconcile Claude's PostgreSQL foundation research against the live Kaizen repository and identify the exact surviving Packet 016C changes."
---

# Research Result 263 - Claude PostgreSQL Research Steward Reconciliation

## Verdict

```text
RESEARCH ACCEPTED AS EXTERNAL EVIDENCE
BROAD PACKET 016C REDESIGN NOT REQUIRED
POSTGRESQL 18 REMAINS THE PROVING-GROUND TARGET
LIVE PREFLIGHT REMAINS THE IMPLEMENTATION BLOCKER
TWO NARROW PREFLIGHT HARDENINGS ACCEPTED
```

## Verified repository checkpoint

```text
kaizen-docs branch: main
starting HEAD: c4432464f3c2e9de9bd3017c29778b7c6074bea9
working tree: clean
upstream: origin/main synchronized
```

The steward inspected the actual accepted decisions, Milestone 11 specifications, Packet 016C, and the local PostgreSQL preflight rather than relying on the research prompt summary.

## Finding-by-finding reconciliation

### PostgreSQL version

Claude's original report preferred PostgreSQL 17 and allowed 18 after a clean Windows preflight. The repository already binds Packet 016C to PostgreSQL 18 and fails closed if the service or server version cannot be verified.

Decision:

```text
keep PostgreSQL 18 as the local proving-ground target
retain PostgreSQL 16 as the minimum supported design baseline
stop and revise rather than silently fall back if the live preflight fails
```

Reasons:

- the first implementation is a disposable, heavily tested local proving ground;
- PostgreSQL 18 is already installed or expected locally and is explicitly preflight-gated;
- checksums-on-by-default, current authentication posture, and future UUIDv7 availability are useful even though Linux-only io_uring is irrelevant on Windows;
- no production or customer-data claim follows from the proving-ground version choice.

Disposition: keep.

### External and physical identifiers

Claude's original report treated text primary keys as the highest-severity pre-SQL issue and recommended native UUIDv7 storage.

The live repository shows:

- Decision 0020 already accepts typed prefixed ULIDs as stable external identities;
- existing Markdown, task-packet, governance, staging, and future retrieval references depend on that identity form;
- the physical design permits later internal database keys but forbids them from replacing or leaking as canonical external identity;
- the first-slice workload is low-volume and evidence-oriented, so text-ULID index overhead is not yet an evidenced operational problem.

Decision:

```text
keep prefixed ULID text keys in the first slice
preserve compact internal keys as a measured-volume optimization watchlist item
revisit native UUID or bigint internal keys for high-volume Observatory or IMI workloads
```

Disposition: defer optimization; no Packet 016C blocker.

### Core database naming

Claude's repository-aware follow-up flagged a possible conflict between conceptual `kaizen_core` in Decision 0010 and implemented `kaizen_ops` in Milestone 11.

The live Decision 0010 explicitly states:

```text
the name is conceptual
final naming may change
exact database names are not approved by that decision
```

Packet 016B later accepted `kaizen_ops` as the concrete first-slice database name.

Decision:

```text
keep kaizen_ops
record that it is the concrete operational database within the Kaizen Core Operational authority domain
```

Disposition: no conflict and no rename.

### Observatory and Internet Marketing Intelligence boundary

Decision 0010 remains authoritative:

- Internet Marketing Intelligence is a separate database boundary from Core Operational Postgres;
- both may initially share one PostgreSQL deployment;
- Observatory remains an analytical capability whose later workloads may live primarily in the intelligence database;
- exact Observatory and IMI physical schemas remain deferred until their own workload-design gates.

Decision:

```text
no Observatory or IMI tables, extensions, partitions, or scaffolding in Packet 016C
```

Disposition: preserve separation.

### Project isolation and RLS

Claude warned against treating row-level security as the sole isolation mechanism.

The accepted physical design already:

- carries project IDs on child records;
- requires composite project-safe relationships;
- requires typed-service project filters;
- defers RLS unless later evidence shows it adds useful defense in depth.

Disposition: already satisfied.

### Status representation

Claude recommended text plus checks rather than PostgreSQL native enums.

The accepted design already uses text fields with allowlists and constraints.

Disposition: already satisfied.

### Durable paths

Claude recommended scoped storage-root identifiers plus relative paths and rejected absolute Windows paths as durable identity.

Decision 0020 and the physical design already mandate that exact posture.

Disposition: already satisfied.

### Git commit identity

Claude warned against universal 40-character SHA-1 assumptions.

Packet 016C already supersedes fixed commit fields with:

```text
commit_algorithm
commit_digest
```

The first implementation accepts `sha1` with a matching 40-character digest and fails closed on unknown algorithms.

Disposition: already satisfied.

### Database and file recovery generations

Claude emphasized that a database backup without matching evidence files is incomplete.

The accepted backup design already cryptographically binds database backups, file archives, migration sets, storage-root mappings, repository checkpoints, counts, and hashes into one recovery generation.

Disposition: already satisfied.

### Migration runner

Claude endorsed the small Kaizen-owned migration runner if it remains narrow and enforces source hashes, deterministic ordering, one migration lock, transactional application, and interruption recovery.

Packet 016C already requires those controls and prohibits an arbitrary SQL execution API.

Disposition: keep, with `yoyo-migrations` only as a future fallback if the custom runner grows beyond its bounded contract.

### Extensions and premature scale machinery

Claude recommended no first-slice extension, partitioning, TimescaleDB, pgvector, PITR, streaming replication, connector telemetry, or governance-read-model schema.

This matches current Packet 016C scope.

Disposition: already satisfied.

## Surviving improvements

Two low-cost hardenings should be added to the live PostgreSQL preflight before implementation approval:

1. verify `data_checksums` is enabled for the selected PostgreSQL 18 cluster;
2. verify `password_encryption` is `scram-sha-256` for newly created role credentials.

These checks strengthen evidence-integrity and authentication posture without widening implementation scope.

## Packet 016C impact

```text
broad redesign: no
version change: no
identifier redesign: no
database rename: no
schema-family change: no
dependency change: no
new extension: no
preflight revision: yes, checksums and SCRAM verification only
Packet 016C wording revision: yes, bind those two preflight requirements
implementation authorization: still absent
```

## Final disposition

Packet 016C remains conditionally implementation-ready. The final gate is the owner-run local PostgreSQL preflight proving:

- PostgreSQL 18 service and server;
- exact `psql` binary and hash;
- server port, database, encoding, and timezone;
- data checksums enabled;
- SCRAM password-encryption posture;
- bounded role privileges;
- Python 3.11;
- owner-selected secret source and process-only injection;
- clean platform state and no secret leakage.

After the revised preflight passes and its non-secret evidence is frozen, Packet 016C may return for exact-hash implementation approval.