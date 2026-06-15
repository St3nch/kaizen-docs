---
id: kz-tp-01KV6LAQ8M2N7R5C3Y9P4T6BK0
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-16T00:40:00Z
updated: 2026-06-16T00:40:00Z
review_status: pending
authority: proposed
summary: "Design the minimum physical operational-data foundation for the four-family first slice without implementing it."
---

# Task Packet 016B - Design Minimum Operational Data Foundation

## Objective

Produce an implementation-ready physical design for the four-family Milestone 11 first slice without creating a database, writing executable migrations, or implementing source code.

## Prerequisite authority

Packet 016B may begin only after exact-hash owner acceptance of:

```text
Decision 0020
Milestone 11 identity, lifecycle, and recovery contract
Phase 11A readiness audit
```

## In scope

Physical design for:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

Design-only supporting concerns:

```text
project and repository references
scoped storage roots
idempotency records
typed service contracts
bounded machine-audit observations
migration metadata
backup and restore design
```

## Out of scope

```text
governed-operation read model
connector invocation telemetry
Observatory
ingestion
Qdrant
IMI
customer data
multi-user identity
production MCP architecture
implementation or deployment
```

## Required design outputs

### 1. Database posture

Specify:

- minimum supported Postgres version;
- local proving-ground deployment class;
- database and logical schema naming;
- extension policy;
- encoding, collation, and timezone posture;
- ownership and migration roles;
- direct-access prohibition;
- development reset and disposable-test posture.

### 2. Physical schema design

Define reviewed, non-executable design for:

- tables;
- columns and data types;
- primary and alternate keys;
- foreign keys;
- unique constraints;
- check constraints;
- indexes justified by accepted queries;
- immutable and append-only enforcement;
- project-isolation enforcement;
- idempotency bindings;
- storage-root and relative-path representation;
- audit timestamps and actor/service references.

Do not use generic JSON to avoid governing repeatedly queried fields.

### 3. Migration design

Specify:

- migration ownership;
- file layout and naming;
- schema version tracking;
- empty-state creation;
- forward upgrade;
- failed-migration recovery;
- rollback versus fail-forward policy;
- compatibility windows;
- seed/reference-data rules;
- migration test matrix;
- backup requirement before destructive migration.

Executable SQL remains prohibited until a later implementation packet.

### 4. Typed service design

Define exact service contracts for:

```text
reserve/start/finish execution attempt
record verification run
freeze return manifest
record and verify artifact provenance
get one evidence graph
list bounded project attempts
run consistency inspection
perform governed retention/deletion
```

For every capability define:

- input and output types;
- caller class;
- consequence class;
- authorization and project scope;
- lifecycle preconditions;
- transaction boundary;
- idempotency behavior;
- bounded error taxonomy;
- privacy filtering;
- audit observation;
- retry safety.

No MCP tool design is authorized in this packet. The service must stand independently of MCP.

### 5. File and storage-reference design

Define:

- storage-root registry authority;
- relative-path normalization;
- path traversal rejection;
- file hash verification;
- missing and mismatch states;
- artifact registration and reconciliation;
- file/database generation consistency;
- behavior when a root moves to another machine.

### 6. Retention and deletion design

Apply Decision 0020 classes and define:

- eligibility computation;
- legal/audit/incident holds;
- tombstone behavior;
- dry-run deletion plan;
- exact affected-count preview;
- authority-reference checks;
- transaction boundaries;
- deletion-result evidence;
- restore implications.

### 7. Backup and restore design

Specify:

- encrypted database backup format;
- coordination with artifact/evidence-file backups;
- generation manifest binding database backup and file archive;
- local, cloud, and removable-media compatibility;
- disposable restore root;
- migration-before-restore or restore-before-migration ordering;
- consistency checks after restore;
- mismatched-generation handling;
- recovery point and recovery time targets;
- minimum two-generation retention unless owner changes accepted backup doctrine.

### 8. Hammer and failure-test design

At minimum design deterministic tests for:

```text
100 concurrent attempt reservations for one idempotency key
100 concurrent distinct attempts in one project
100 cross-project isolation attempts
100 duplicate verification submissions
100 manifest-freeze races
100 duplicate provenance assertions
illegal lifecycle-transition matrix
migration interruption at each phase
service crash before and after commit
file created but database write fails
database write commits but file disappears
hash mismatch and stale storage root
backup during active writes
restore with mismatched file generation
retention deletion with active hold
retention deletion with canonical reference
```

Exact round counts may increase during audit but may not be reduced without evidence.

### 9. Security and privacy design

Define:

- least-privilege database roles;
- service-only credentials;
- secret storage and rotation;
- local transport security posture;
- logging redaction;
- denial-of-service and unbounded-query limits;
- project-scope authorization;
- prohibited payload fields;
- data-at-rest backup encryption;
- incident evidence preservation.

### 10. Implementation packet decomposition

Propose the smallest safe implementation sequence, likely:

```text
016C - migration and database proving ground
016D - typed service and repository boundary
016E - file/provenance consistency and return-manifest freeze
016F - backup, restore, retention, and hammer closure
```

This naming and sequence remain proposals until audited.

## Required audit questions

1. Does the design enforce Decision 0020 rather than merely describe it?
2. Does every physical field have an evidenced purpose?
3. Are artifact bytes and human verdicts still outside Postgres?
4. Can all legal transitions be enforced atomically?
5. Are exact retries idempotent and conflicting retries rejected?
6. Can a database backup be matched to the correct file generation?
7. Can the system recover without guessing terminal state?
8. Are absolute paths absent from durable identity?
9. Are all ordinary reads project-bounded?
10. Can the first slice be removed or rebuilt without harming canonical Markdown and governance authority?
11. Is the design small enough for one local operator to understand and maintain?
12. Does the design avoid preparing unused Observatory, ingestion, telemetry, or read-model infrastructure?

## Deliverables

Recommended:

```text
05-specs/milestone-11-minimum-postgres-physical-design.md
05-specs/milestone-11-typed-service-contract.md
05-specs/milestone-11-backup-restore-retention-design.md
05-specs/milestone-11-hammer-and-failure-test-plan.md
03-research-results/<next>-packet-016b-physical-design-audit.md
```

## Completion gate

Packet 016B is complete only when the physical design is independently audited and accepted at exact hashes. Completion authorizes neither database creation nor implementation. A separate implementation packet and exact owner approval are required.