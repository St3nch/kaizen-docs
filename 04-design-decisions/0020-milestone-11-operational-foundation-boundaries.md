---
id: kz-dec-01KV6L8Q8M2N7R5C3Y9P4T6BH0
type: decision
status: proposed
project: kaizen-platform
created: 2026-06-16T00:20:00Z
updated: 2026-06-16T00:20:00Z
review_status: pending
authority: proposed
summary: "Propose the first-slice, retention, deletion, path, trust, and deferred-family boundaries for Milestone 11."
---

# Decision 0020 - Milestone 11 Operational Foundation Boundaries

## Context

Decision 0019 accepted six conceptual operational families for later design. Packet 016A requires the owner to choose the smallest first implementation slice and settle policy boundaries before physical database design.

Results 255 and 256 show that the minimum coherent first slice is the four-family implementation-evidence chain.

## Proposed decision

### First implementation slice

Accept for Packet 016B physical design:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

Defer from the first slice:

```text
governed-operation read model
connector invocation telemetry
```

Deferral does not reject the families. It requires the first slice to prove the operational database and typed-service burden before adding them.

### Retention classes

Adopt:

```text
Class A - permanent authority-linked lineage
frozen return manifests, rejected return history, and provenance referenced by accepted audits or owner decisions

Class B - project lifetime plus 12 months
execution attempts, verification runs, and unaccepted provenance linked to retained projects

Class C - 90 days
future connector or transient diagnostic telemetry unless held by an incident or unresolved operation

Class D - rebuildable
future governed-operation read models
```

A legal, security, incident, audit, or owner hold suspends ordinary deletion.

### Connector telemetry posture

For any later connector-telemetry slice:

- retain all blocked, rejected, failed, interrupted, timed-out, and consequential mutation attempts;
- permit sampling or aggregation of successful low-risk reads;
- prohibit prompt bodies, arbitrary arguments, file contents, credentials, tokens, and private customer payloads by default;
- require a separate privacy and retention audit before implementation.

### Retry-group expiry

Future connector retry groups expire 30 days after the most recent invocation unless linked to an unresolved governed operation or active incident.

### Project deletion

Adopt governed deletion with retained lineage:

- deletion requires an explicit approved plan;
- ordinary Class B records may be removed after retention and hold checks;
- Class A records remain;
- canonical decisions, governance events, rejected and accepted implementation lineage, and authority-linked provenance are never cascade-deleted merely because a project is archived or removed;
- retained lineage references a durable project tombstone or project identity.

### Path references

Durable operational records use:

```text
scoped storage root ID
repository-relative or storage-root-relative path
```

Absolute local paths are prohibited as durable identity. They may appear only in ephemeral local diagnostics and must not be exposed through ordinary agent-facing tools.

### Stable external identity

Use typed prefixed ULIDs as stable external IDs at service boundaries. Internal database keys may exist later but may not replace or leak as canonical external identity.

Retries, reruns, corrected manifests, and superseding provenance assertions receive new IDs with explicit predecessor or supersedence relationships.

### Trust boundary

- no agent, MCP client, or generic script receives direct SQL or database credentials;
- only an approved typed service may mutate first-slice records;
- every write is project-scoped and validates legal lifecycle transitions;
- ordinary reads are project-bounded;
- human audit and owner acceptance remain Markdown authority;
- artifact and evidence bytes remain files;
- Git, governance JSONL, and immutable staging evidence keep their current authority.

### Transaction and correction posture

- execution terminal states are immutable;
- timeout or connector blocking is not recorded as a failed test;
- return-manifest freeze atomically binds the exact evidence inventory and hashes;
- frozen manifests are immutable and versioned;
- provenance corrections supersede prior assertions;
- unknown recovery state remains recovery-required;
- idempotency keys prevent silent duplicate mutation.

### Backup and recovery posture

Packet 016B must design:

- encrypted local backup objects;
- off-machine and removable-media compatibility with accepted Kaizen backup doctrine;
- disposable restore proof;
- database-and-file consistency verification;
- migration and restore ordering;
- explicit recovery point and recovery time targets;
- behavior when database and artifact-file generations do not match.

### Hammer posture

Physical design must include concurrency, duplicate, lifecycle, isolation, migration interruption, backup/restore, crash-boundary, stale-reference, and idempotency hammer tests before implementation acceptance.

## Consequences

### Positive

- first implementation scope is small and evidence-backed;
- execution state is separated from verification and human acceptance;
- evidence completeness and provenance become queryable;
- existing canonical authorities remain intact;
- absolute Windows paths do not become durable identity;
- retention and deletion are explicit before tables exist.

### Costs

- the typed service and recovery model add implementation work;
- immutable manifests require versioning rather than convenient edits;
- file/database consistency must be proved;
- governance read-model and connector-telemetry benefits remain deferred.

## Explicit non-authorization

This decision does not authorize:

- Postgres creation;
- physical schemas, tables, columns, keys, indexes, constraints, migrations, SQL, roles, or credentials;
- services, APIs, MCP tools, source code, deployment, or implementation;
- Packet 016B execution without exact-hash owner approval;
- Observatory, ingestion, Qdrant, IMI, customer-data, or multi-user work.

## Acceptance condition

This decision becomes active only after exact-hash owner acceptance following the Phase 11A readiness audit.