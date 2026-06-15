---
id: kz-tp-01KV6LHQ8M2N7R5C3Y9P4T6BS0
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-16T01:50:00Z
updated: 2026-06-16T01:50:00Z
review_status: pending
authority: proposed
summary: "Implement the minimum four-family operational-data foundation after physical-design acceptance."
---

# Task Packet 016C - Implement Minimum Operational Data Foundation

## Authority

Draft only. This packet is not approved and may not be executed.

## Objective

Implement the local proving-ground foundation for:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

## Prerequisites

Owner acceptance at exact hashes of the complete Packet 016B design package and audit, plus a frozen clean platform starting commit, exact path allowlist, database-version evidence, and credential-storage plan.

## Proposed sequence

### 016C.1 Database and migrations

- create local proving-ground database and least-privilege roles;
- implement immutable migrations;
- create first-slice tables, constraints, indexes, and narrow triggers;
- prove empty-state creation and interrupted migration recovery.

### 016C.2 Typed service core

- implement execution-attempt and verification-run capabilities;
- enforce project scope, lifecycle, idempotency, bounded errors, and service audit events.

### 016C.3 Manifest and provenance

- implement storage-root resolution and path confinement;
- implement exact file hashing, atomic manifest freeze, byte objects, and provenance assertions;
- implement bounded evidence-graph reads and consistency inspection.

### 016C.4 Recovery and closure

- implement coordinated backup generation and disposable restore;
- implement recovery inspection and approved interruption handling;
- implement retention dry-run only unless deletion execution receives separate approval;
- run the full hammer matrix;
- freeze implementation return and obtain independent audit.

The owner may split these into separate approved packets.

## Proposed implementation home

```text
C:\dev\kaizen\platform
```

Local database configuration and secrets remain outside Git. Exact source and test paths must be enumerated before approval. Kaizen MCP and Go8 changes are excluded unless separately authorized.

## Explicit exclusions

- governed-operation read model;
- connector telemetry;
- Observatory, ingestion, Qdrant, IMI, customer data, or multi-user identity;
- production MCP architecture or deployment;
- direct agent SQL;
- moving Markdown or artifact bytes into Postgres;
- retention deletion execution without separate approval.
