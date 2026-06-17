---
id: kz-tp-01KV71AF4M2N7C5Y3P8T6D1F30
type: task-packet
status: ready-for-approval
project: kaizen-platform
created: 2026-06-17T16:52:00Z
updated: 2026-06-17T16:52:00Z
review_status: passed
authority: proposed
summary: "Implement bounded file consistency, artifact provenance, immutable return-manifest freeze, and expanded evidence-graph reads over the accepted Milestone 11 foundation."
---

# Task Packet 016E - Implement File Provenance Consistency and Return-Manifest Freeze

## Authority

Ready for exact-hash owner approval. This packet is not yet approved and may not be executed.

Governing records:

```text
Milestone 11 planning:
05-specs/milestone-11-operational-data-foundation-planning.md

Identity, lifecycle, and recovery contract:
05-specs/milestone-11-identity-lifecycle-recovery-contract.md
SHA-256: e1067437543ff6e084df859cc3ecdd6ea2c67a7bd935e9c2d5178dc0c394f511

Minimum PostgreSQL physical design:
05-specs/milestone-11-minimum-postgres-physical-design.md
SHA-256: c21347ffe4161c970534733fefadf76526f1aff3950784db8ea9f54cabb017f8

Typed service contract:
05-specs/milestone-11-typed-service-contract.md

Hammer and failure-test plan:
05-specs/milestone-11-hammer-and-failure-test-plan.md
SHA-256: 8408801f99a0d9af83fe53abdbc9d94c2cb89bf407d17bcd1f07459c36686a2d

Packet 016D owner completion acceptance:
03-research-results/278-packet-016d-owner-completion-acceptance.md
SHA-256: 6146a49144bad43527240f67edab4ad363d1394d64a5a67c6e90a2a90a24ae5c
```

## Objective

Implement only the typed file-evidence and immutable return-manifest slice for:

```text
resolve and hash one governed file reference
record or replay one immutable artifact byte object
record or replay one provenance assertion
recheck one artifact file-consistency state
freeze one immutable return manifest atomically
append one canonical audit or acceptance link to a frozen manifest
get one expanded execution evidence graph
```

Packet 016E completes the first-slice evidence model without implementing backup, restore, retention, deployment, MCP, or external transport.

## Starting checkpoint

```text
repository:
C:\dev\kaizen\platform

branch:
main

required starting commit:
b28fd53818b062d41613efdd250def6e48b376de

required working tree:
clean

required remotes:
none
```

Stop on checkpoint drift unless the owner approves a revised packet binding.

## Included scope

- machine-local storage-root resolution supplied through bounded service configuration;
- relative-path normalization and confinement beneath a configured root;
- file reading and SHA-256 hashing for explicitly referenced files only;
- explicit file states: `file_pending`, `file_verified`, `file_missing`, `file_hash_mismatch`, `reference_unresolvable`;
- logical-artifact creation or exact replay;
- immutable artifact byte-object creation or exact replay;
- provenance assertion creation, rejection, and supersedence without overwrite;
- immutable return-manifest freeze;
- manifest entries for present files;
- explicit missing-item rows for required missing evidence;
- deterministic inventory digest and manifest counts;
- append-only audit/acceptance links to frozen manifests;
- expanded evidence graph covering attempt, verification, manifest, entries, missing items, byte objects, provenance, and audit links;
- one immutable migration for narrow service grants or evidenced indexes and constraints;
- unit, integration, file-boundary, failure-injection, and concurrency hammer proof.

## Excluded scope

- backup creation, archive transfer, restore, or generation binding;
- retention planning, holds, deletion, or dry-run execution;
- PITR, replication, partitioning, or production deployment;
- MCP, HTTP, RPC, queue, worker, daemon, or connector adapter;
- direct agent filesystem or SQL access;
- automatic discovery of arbitrary files or directories;
- recursive unbounded directory scans;
- customer payloads, prompts, conversations, stdout/stderr bodies, or secrets;
- canonical Markdown creation or mutation;
- canonical audit verdict creation;
- owner acceptance creation;
- Observatory, connector telemetry, Internet Marketing Intelligence, Qdrant, or multi-user identity;
- vault, Northstar, Kaizen MCP, Go8, or staging mutation;
- platform remote creation;
- Packet 016F implementation.

## Trust and architecture boundary

```text
caller
-> typed in-process evidence service
-> bounded storage-root resolver and file hasher
-> fixed typed repository
-> Psycopg parameterized SQL
-> kaizen_ops
```

Only the service resolves machine-local roots and opens files. Callers provide governed storage-root IDs and normalized relative paths, never absolute paths.

The repository may persist only IDs, hashes, lengths, media type, bounded states, timestamps, counts, and scoped references. File bytes remain external.

## Required capabilities

### 1. `record_artifact_provenance`

Input:

```text
request envelope
logical artifact role
storage_root_id
relative_path
media_type optional
producer execution_attempt_id optional
producer verification_run_id optional
repository commit algorithm and digest
input_set_ref optional
normalized_command_id optional
expected_sha256 optional
supersedes_assertion_id optional
```

Required behavior:

1. validate project, caller, producer references, storage-root scope, and relative path;
2. resolve the configured machine-local root;
3. reject traversal, absolute paths, UNC paths, drive-relative paths, alternate-stream syntax, and root escape;
4. read and hash the exact file bytes with SHA-256;
5. capture byte length and bounded media type;
6. compare an optional expected hash;
7. create or replay the logical artifact, byte object, and provenance assertion atomically;
8. set `file_verified` only after successful read and exact hash calculation;
9. set or return `file_missing`, `file_hash_mismatch`, or `reference_unresolvable` explicitly when appropriate;
10. create one bounded service audit event;
11. exact retry replays the same result; changed payload under one key rejects.

Missing or mismatched files do not create a false verified byte object or active provenance assertion.

### 2. `verify_artifact_reference`

Required behavior:

- resolve and hash the referenced byte object's current storage location;
- preserve immutable byte-object identity and provenance history;
- create a new consistency observation or narrowly update only the accepted mutable consistency fields if the physical contract requires it;
- transition among explicit file-consistency states without deleting history;
- never silently convert missing or changed bytes into verified state;
- exact retry replays.

### 3. `freeze_return_manifest`

Input includes:

```text
request envelope
execution_attempt_id
contract_version
manifest_version
predecessor_manifest_id optional
repository commit algorithm and digest
required inventory specification
present file references
verification_run references
audit_record_id optional only when already canonical
```

Required behavior:

- require a terminal execution attempt;
- require project-safe verification references;
- resolve and hash every present retained file before database freeze;
- compute deterministic normalized inventory;
- compute inventory SHA-256 from canonical serialization;
- compute required, present, missing, and extra counts;
- create manifest metadata, present entries, missing-item rows, and idempotency/audit rows atomically;
- create no partial manifest on file, hash, validation, or transaction failure;
- exact retry replays the same manifest;
- conflicting inventory under one key rejects;
- concurrent freeze for the same attempt/version produces exactly one frozen manifest;
- frozen inventory is immutable;
- corrected inventory requires a new manifest version linked to a predecessor.

### 4. `link_manifest_authority_record`

Required behavior:

- accept only an already-canonical record ID supplied by an authorized owner/steward/audit caller;
- allow only `audit_pass`, `audit_fail`, `owner_acceptance`, or `completion_record`;
- append one immutable link without mutating the frozen manifest;
- exact retry replays;
- conflicting link reuse rejects;
- does not create or interpret the canonical Markdown record.

### 5. `get_execution_evidence_graph`

Expand Packet 016D's graph to return bounded, deterministic, project-safe sections for:

```text
execution attempt
verification runs
return manifests
manifest entries
manifest missing items
manifest audit links
logical artifacts
artifact byte objects
provenance assertions
```

Requirements:

- explicit project and attempt identity;
- bounded maximums per collection;
- deterministic ordering;
- no file bytes, absolute paths, arbitrary payloads, stdout/stderr, prompts, or secrets;
- explicit truncation metadata if a bounded maximum is reached;
- no portfolio-wide cross-project query.

## Storage-root configuration

Machine-local root mapping must be process-injected or loaded from an owner-controlled configuration outside canonical records.

Required mapping shape:

```text
storage_root_id -> absolute machine-local root
```

Rules:

- durable database rows store only root ID plus relative path;
- configuration rejects duplicate IDs and non-absolute roots;
- configured roots are canonicalized once;
- resolved paths must remain under the canonical root after normalization;
- symlink or junction escape must be detected where the platform can prove it;
- unavailable or unmapped roots yield `reference_unresolvable`;
- no root mapping or absolute path appears in service responses, audit rows, or persistent database fields.

## File hashing contract

- SHA-256 only for this slice;
- stream files in bounded chunks rather than loading arbitrary files wholly into memory;
- record exact byte length;
- reject directories, devices, sockets, and non-regular files;
- detect file replacement or mutation during hashing where practical through pre/post metadata checks;
- fail closed on read errors;
- do not follow a path outside the configured root;
- no caller-selectable hash algorithm;
- no caller-selectable arbitrary read range.

## Logical artifact and provenance rules

- logical artifacts are project-scoped roles, not file paths;
- exact artifact-role retry returns the existing logical artifact;
- byte objects are immutable observations of exact bytes at a scoped reference;
- identical bytes may receive multiple legitimate provenance assertions;
- at least one producer reference is required;
- producer attempt and verification references must share the project;
- supersedence creates a new assertion and preserves the prior assertion;
- no active assertion is silently overwritten;
- hash mismatch rejects active provenance creation;
- canonical links remain separate from provenance assertions unless the accepted schema explicitly supports a bounded canonical record ID.

## Manifest inventory contract

Every manifest freeze input is normalized into deterministic entries.

Present entry fields:

```text
entry_kind
storage_root_id
relative_path
sha256
byte_length
media_type optional
requirement_status
verification_run_id optional
```

Missing item fields:

```text
requirement_key
expected_kind
expected_storage_root_id optional
expected_relative_path optional
```

Rules:

- present entries are sorted deterministically before digesting and assigning ordinals;
- missing items are sorted deterministically before assigning ordinals;
- duplicate root/path entries reject;
- required missing items are represented only in missing-item rows;
- no fake hash or zero-length placeholder represents a missing file;
- manifest count fields must equal persisted child-row counts;
- inventory digest must recompute exactly from persisted normalized content;
- frozen manifests and entries are immutable.

## Error contract

At minimum:

```text
storage_root_not_found
storage_root_unmapped
reference_project_mismatch
path_invalid
path_traversal_rejected
path_escape_detected
file_not_found
file_not_regular
file_read_failed
file_changed_during_hash
file_hash_mismatch
logical_artifact_conflict
producer_reference_missing
producer_project_mismatch
provenance_conflict
supersedence_invalid
manifest_version_conflict
manifest_predecessor_invalid
manifest_inventory_conflict
manifest_already_frozen
manifest_immutable
inventory_digest_mismatch
collection_limit_exceeded
idempotency_conflict
concurrency_conflict
recovery_required
service_unavailable
```

No absolute path, OS exception text, SQLSTATE, SQL text, stack trace, credential, DSN, or file content may cross the service boundary.

## Transaction and crash boundaries

### Observe artifact file

File read and hashing occur before database mutation. The database transaction then atomically commits logical-artifact, byte-object, provenance, idempotency, and audit rows.

Crash outcomes:

```text
before file read -> no rows
mid-read -> no verified claim
post-hash before transaction -> no rows
mid-transaction -> rollback
post-commit before response -> exact retry replays
```

### Freeze manifest

All file reads and hashes complete before the database freeze transaction. The transaction atomically commits manifest metadata, entries, missing rows, idempotency, and audit.

A post-hash/pre-commit file change must be detected where feasible or represented as a later consistency incident; it may never produce an knowingly false verified claim.

## Database migration posture

Packet 016E may add one immutable migration:

```text
0003_file_provenance_and_manifest_service.sql
```

It may contain only:

- exact service-role grants for logical artifact, byte object, provenance, manifest, entry, missing-item, and audit-link operations;
- narrow constraints, indexes, triggers, or helper functions proven necessary by implementation tests;
- bounded graph-read privileges;
- no destructive DDL;
- no ownership transfer;
- no backup, retention, telemetry, deployment, or later-slice objects;
- no changes to migrations 0001 or 0002.

The migration must be applied through the accepted migration runner and bound by SHA-256 in the packaged manifest.

## Secret posture

- storage-root mapping is owner-controlled and outside Git unless it contains only non-secret disposable test roots;
- service DSNs remain process-injected;
- temporary development credentials may be used for disposable proof but are never committed;
- no credential, DSN, root absolute path, file byte content, or environment dump enters docs, Git, logs, audit rows, or service responses;
- all credential and mapping environment variables are cleared after owner-run tests.

## Exact changed-path allowlist

Existing files allowed to change:

```text
src/kaizen/ops_db/migrations/manifest.json
src/kaizen/ops_service/__init__.py
src/kaizen/ops_service/contracts.py
src/kaizen/ops_service/errors.py
src/kaizen/ops_service/repository.py
src/kaizen/ops_service/serialization.py
src/kaizen/ops_service/service.py
tests/test_ops_db_migration_runner.py
tests/test_ops_db_migration_hammer.py
tests/test_ops_service_contracts.py
tests/test_ops_service_repository.py
tests/test_ops_service_service.py
tests/test_ops_service_integration.py
tests/test_ops_service_hammer.py
docs/OPERATIONS_DATABASE_PROVING_GROUND.md
```

New source paths allowed:

```text
src/kaizen/ops_service/storage.py
src/kaizen/ops_service/hashing.py
src/kaizen/ops_service/manifest.py
src/kaizen/ops_db/migrations/0003_file_provenance_and_manifest_service.sql
```

New test paths allowed:

```text
tests/test_ops_service_storage.py
tests/test_ops_service_hashing.py
tests/test_ops_service_manifest.py
tests/test_ops_service_file_integration.py
tests/test_ops_service_file_hammer.py
```

No other platform path may change without an owner-approved packet revision.

## Required tests

### Unit and contract tests

- root mapping validation;
- path normalization and rejection matrix;
- symlink or junction escape detection where supported;
- regular-file enforcement;
- streamed SHA-256 and byte-length calculation;
- pre/post mutation detection during hashing where practical;
- strict observe, recheck, freeze, link, and graph contracts;
- deterministic inventory normalization and digest;
- manifest count calculations;
- unknown-field rejection;
- caller allowlists;
- bounded error redaction;
- graph collection limits and truncation metadata.

### Live PostgreSQL and filesystem integration tests

- verified artifact observation;
- missing file explicit state;
- hash mismatch explicit state;
- unmapped root explicit state;
- changed file recheck;
- same bytes with multiple valid provenance assertions;
- supersedence preserving prior assertion;
- producer-reference and project mismatch rejection;
- manifest freeze with present, missing, optional, and unexpected inventory;
- deterministic persisted inventory digest recomputation;
- no partial rows after injected freeze failure;
- immutable frozen manifest and entries;
- version 2 predecessor linkage;
- append-only audit/acceptance links;
- expanded bounded evidence graph;
- service-role positive and negative privilege checks;
- no absolute path or file content leakage.

## Hammer requirements

At minimum:

```text
100 identical artifact observations
  expected: 1 logical artifact as applicable, 1 byte object, 1 provenance assertion, 99 replayed

100 conflicting expected hashes under one idempotency key
  expected: 1 winner or one deterministic mismatch result, all changed payloads conflict, no false verified rows

100 identical manifest freezes
  expected: 1 frozen manifest, 99 replayed, exact entry/missing counts

100 conflicting inventories for one attempt/version
  expected: 1 winner, 99 bounded conflicts, no mixed inventory

100 supersedence contenders
  expected: exactly 1 accepted successor for the same correction slot or a documented deterministic append-only distribution, no overwritten prior assertion

100 cross-project producer, storage-root, verification, manifest, and graph attempts
  expected: all rejected, zero leaked rows or paths
```

File/database crash-boundary injection must cover:

```text
before file read
after file read before hash completion
after hash before database transaction
after database begin before inserts
after rows before commit
after commit before response
```

Every profile records exact expected and observed distributions.

## Required verification gates

- complete existing platform suite;
- complete Packet 016D service suite;
- complete Packet 016E unit and file-boundary suite;
- live disposable PostgreSQL plus disposable storage-root integration suite;
- all required Packet 016E hammer profiles;
- migration empty-state, upgrade, interruption, replay, and drift proof for migrations 0001-0003;
- compileall;
- available Ruff check if configured and installed;
- package import checks;
- static capability review;
- changed-text integrity review;
- exact changed-path verification;
- migration source-hash verification;
- retained migration apply and idempotent replay;
- retained service-role privilege inspection;
- clean platform Git state after local commit;
- no platform remote;
- no vault, Kaizen MCP, Go8, Northstar, or staging mutation.

## Implementation return

Return must include:

- starting and ending platform commits;
- exact changed paths;
- migration ID and SHA-256;
- storage-root configuration and redaction proof;
- capability inventory;
- path-confinement and hashing proof;
- file-state proof;
- provenance and supersedence proof;
- manifest atomic-freeze and immutability proof;
- deterministic inventory digest proof;
- expanded evidence-graph proof;
- project-isolation and privilege proof;
- crash-boundary evidence;
- hammer distributions;
- full test totals;
- compile/lint/import/integrity status;
- known limitations;
- explicit confirmation that backup, restore, retention, MCP, deployment, Observatory, IMI, vault, and remote work were not implemented.

## Stop conditions

Stop on:

- platform checkpoint drift;
- need for an unapproved dependency or path;
- need to alter migrations 0001 or 0002;
- inability to keep root mappings, DSNs, credentials, and absolute paths out of durable records and responses;
- inability to prove path confinement;
- inability to detect or fail closed on hash mismatch;
- inability to freeze manifest metadata and child rows atomically;
- inability to preserve immutable manifest or provenance history;
- unexplained hammer distribution;
- need to implement backup, restore, retention, telemetry, MCP, deployment, or later-slice scope;
- need to mutate vault, Northstar, Kaizen MCP, Go8, or staging.

## Approval gate

Implementation may begin only after the owner approves this exact packet SHA-256 against platform starting commit:

```text
b28fd53818b062d41613efdd250def6e48b376de
```
