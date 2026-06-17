---
id: kz-tp-01KVBC0F4M2N7C5Y3P8T6D1F90
type: task-packet
status: ready-for-approval
project: kaizen-platform
created: 2026-06-17T18:00:00Z
updated: 2026-06-17T18:00:00Z
review_status: passed
authority: proposed
summary: "Implement the final Milestone 11 slice: encrypted local recovery generations, disposable restore proof, consistency inspection, retention dry-run planning, and closure hammer evidence."
---

# Task Packet 016F - Implement Backup, Restore, Retention Dry-Run, and Milestone 11 Closure

## Authority

Ready for exact-hash owner approval. This packet is not yet approved and may not be executed.

Governing records:

```text
Backup, restore, and retention design:
05-specs/milestone-11-backup-restore-retention-design.md
SHA-256: f5d99a1b5c908bbf807c4bb44089852d136086721ec284e8637b993e93b9d43d

Typed service contract:
05-specs/milestone-11-typed-service-contract.md
SHA-256: af5f0d00647ccdf8518adcebe05a372d75095718bba30d9350f479ef45e9fa4b

Hammer and failure-test plan:
05-specs/milestone-11-hammer-and-failure-test-plan.md
SHA-256: 8408801f99a0d9af83fe53abdbc9d94c2cb89bf407d17bcd1f07459c36686a2d

Packet 016B owner acceptance:
03-research-results/260-packet-016b-owner-acceptance-and-implementation-readiness-authorization.md
SHA-256: a1ba227ebc7729b34304090b2ce3befea64a3d541642b748ea18610ae59bb9da

Packet 016E owner completion acceptance:
03-research-results/283-packet-016e-owner-completion-acceptance.md
SHA-256: 556eaa1a27434f7421203fdb0ec46be5e27ef0692256642153b2966a4e0dcf53
```

## Objective

Implement only the final Milestone 11 recovery and closure slice:

```text
create one cryptographically bound local recovery generation
verify one recovery generation
restore one generation into disposable roots and disposable PostgreSQL
inspect database/file consistency
create deterministic retention-deletion dry-run plans
record backup and restore observations
run final backup, restore, retention, migration, isolation, and concurrency hammers
produce Milestone 11 closure evidence
```

## Starting checkpoint

```text
repository:
C:\dev\kaizen\platform

branch:
main

required starting commit:
b4e219633a9c16cd0d9e2425e33a89572754ac06

required working tree:
clean

required remotes:
none
```

Stop on checkpoint drift unless the owner approves a revised binding.

## Accepted operational policy

```text
local encrypted backup frequency:
after each accepted implementation-return freeze
and at least daily while operational writes exist

portable off-machine posture:
private Google Drive plus separately stored USB SSD
minimum two verified generations
maximum normal age 30 days
owner-only deletion after successor restore proof
```

Packet 016F implements the local encrypted recovery-generation mechanism and disposable restore proof. Manual transfer to Google Drive and USB remains owner-operated unless an exact local copy step is explicitly included. No cloud API or remote automation is authorized.

## Included scope

- logical PostgreSQL backup of `kaizen_ops` using owner-configured PostgreSQL tooling;
- immutable migration-set manifest and hash;
- deterministic inventory of referenced evidence files from accepted operational rows;
- storage-root mapping manifest using logical IDs and package-relative restore locations only;
- repository checkpoint manifest for `kaizen-platform` and the accepted migration source set;
- generation manifest with per-object SHA-256, counts, sizes, tool versions, schema version, and consistency result;
- one shared recovery-generation ID across database dump, file archive, mappings, repository checkpoints, and manifest;
- encryption using the already accepted local encryption tooling and owner-managed secret material;
- destination copy preparation for private Google Drive and separately stored USB SSD;
- independent hash verification after local copy or owner-supplied downloaded copy;
- restore into disposable filesystem root and disposable PostgreSQL database only;
- exact migration compatibility check before restore;
- database-to-file reconciliation after restore;
- typed-service read-only smoke checks against restored state;
- `inspect_consistency` read-only capability;
- `plan_retention_deletion` dry-run capability only;
- explicit retention classes, holds, blockers, counts, candidate IDs, tombstone effects, and plan digest;
- backup-generation and restore-proof metadata rows if required by accepted physical design;
- one immutable migration for narrow metadata tables, constraints, indexes, or service grants if tests prove necessary;
- final Milestone 11 hammer and failure closure.

## Explicit exclusions

- `execute_retention_deletion`;
- physical deletion of operational rows or evidence files;
- backup-generation destruction;
- automatic Google Drive or USB transfer;
- cloud API credentials or remote automation;
- PITR, WAL archiving, replication, failover, partitioning, or high availability;
- production deployment or production SLA claims;
- restoration into live `kaizen_ops`;
- promotion of restored state into canonical use;
- MCP, HTTP, RPC, queue, daemon, scheduler, Windows service, or network adapter;
- direct agent SQL or arbitrary filesystem access;
- canonical Markdown, governance JSONL, vault, Git history, immutable staging evidence, Northstar, Kaizen MCP, or Go8 mutation;
- Observatory, Internet Marketing Intelligence, Qdrant, customer data, or multi-user identity;
- platform remote creation;
- any post-Milestone-11 capability.

## Recovery generation contract

A complete generation contains:

```text
encrypted logical database backup
immutable migration-set manifest
referenced evidence-file archive
storage-root mapping manifest
repository checkpoint manifest
recovery-generation manifest
per-object SHA-256 hashes
tool and version evidence
```

A database dump without its referenced file archive is incomplete. A file archive without the matching database snapshot is incomplete.

Required manifest fields:

```text
recovery_generation_id
created_at
source_database_identity
source_schema_version
migration_set_sha256
database_backup_sha256
file_archive_sha256
storage_root_manifest_sha256
repository_checkpoint_manifest_sha256
record_counts_by_family
file_count
total_file_bytes
consistency_check_result
encryption_tool
encryption_tool_version
backup_tool
backup_tool_version
```

## Backup consistency boundary

Required sequence:

1. allocate generation ID;
2. record cutoff time and database transaction/snapshot evidence;
3. create a database-consistent logical dump;
4. freeze the referenced file inventory for that snapshot;
5. archive only those exact referenced files;
6. hash dump, archive, mappings, migration set, and repository checkpoint manifest;
7. reconcile every included database reference to one captured file or explicit allowed missing state;
8. write immutable generation manifest;
9. encrypt the generation package or cryptographically bound object set;
10. verify encrypted output hash;
11. prepare owner-operated destination copies;
12. record success only after consistency and hash verification pass.

Any mismatch fails the generation. No partial generation may be labeled verified.

## Encryption and secrets

- encryption keys and passwords remain owner-managed and process-injected;
- secrets never enter manifests, logs, Git, docs, database rows, or service responses;
- plaintext temporary files exist only under an owner-configured disposable work root;
- plaintext cleanup occurs only after encrypted output and destination hashes verify;
- cleanup failure is reported explicitly and never hidden;
- tool paths and versions may be recorded; secret arguments may not.

## Restore contract

Restore target must be disposable:

```text
disposable PostgreSQL database or instance
disposable filesystem root
disposable storage-root mapping
no live database mutation
no canonical promotion
```

Required order:

1. verify encrypted package hash;
2. decrypt into isolated temporary workspace;
3. verify internal manifest and object hashes;
4. verify migration-set availability and hash;
5. create empty disposable PostgreSQL target;
6. restore database with the documented tool path;
7. restore evidence files beneath disposable roots;
8. inject disposable storage-root mappings;
9. run database integrity checks;
10. run file existence and SHA-256 reconciliation;
11. run typed-service read-only smoke checks;
12. record exact result, mismatches, elapsed time, and RTO comparison;
13. leave restored state isolated.

Restore fails closed on missing migration source, generation mismatch, file mismatch, schema mismatch, or package corruption.

## Consistency inspection capability

`inspect_consistency` is read-only and requires one explicit project or recovery-generation scope.

Checks include:

```text
project/reference integrity
terminal timestamp consistency
artifact file existence and hash state
manifest count and inventory digest recomputation
provenance links and supersedence
missing or unmapped storage roots
expired-but-held records
orphaned records
backup-generation object/hash consistency
```

Output is bounded and deterministic. No file bytes, physical roots, credentials, SQL, or arbitrary payloads are returned.

## Retention dry-run capability

`plan_retention_deletion` creates a plan only.

Input:

```text
project_id
as_of
requested retention classes
reason
caller and correlation envelope
```

Output:

```text
candidate IDs and counts by family
retention class and expiry basis
blocked IDs and reasons
active holds
accepted authority links
retained Class A counts
project tombstone effects
file-deletion effects marked unexecuted
plan digest
```

Retention classes:

```text
Class A: permanent authority-linked lineage
Class B: project lifetime plus 12 months
Class C: future telemetry, 90 days unless held
Class D: derived read models, no independent retention
```

Holds block deletion planning eligibility:

```text
owner hold
accepted audit or completion reference
active incident
unresolved recovery
legal or security hold
pending backup or restore investigation
```

No deletion is executed. No row, file, canonical record, Git object, JSONL event, or staging evidence may change through dry-run planning.

## Backup and restore metadata

Packet 016F may add narrowly scoped metadata for:

```text
recovery_generation
recovery_generation_object
restore_proof
retention_hold
retention_plan
retention_plan_item
```

Any added family must be justified by tests and remain metadata-only. File bytes and backup archives remain external.

## Database migration posture

Packet 016F may add one immutable migration:

```text
0004_recovery_and_retention_planning.sql
```

It may contain only:

- narrowly required recovery-generation, restore-proof, hold, and retention-plan metadata;
- exact constraints, indexes, immutability triggers, and service grants;
- no destructive DDL;
- no ownership transfer;
- no delete grant for the runtime service;
- no backup archive bytes in PostgreSQL;
- no changes to migrations `0001–0003`.

## Hammer and failure requirements

At minimum:

```text
100 identical recovery-generation create requests
  expected: 1 verified generation or 1 deterministic failure record, 99 replays

100 conflicting generation requests under one idempotency key
  expected: 1 winner, changed requests conflict, no mixed generation objects

100 concurrent consistency inspections
  expected: deterministic bounded results, zero mutation

100 identical retention-plan requests
  expected: 1 plan, 99 replays

100 conflicting retention-plan requests under one key
  expected: 1 winner, changed requests conflict

100 cross-project recovery, consistency, restore, hold, and retention attempts
  expected: all rejected, zero leaked rows or paths
```

Failure injection must cover:

```text
before database dump
during database dump
after dump before file archive
during file archive
after hash before manifest
after manifest before encryption
during encryption
after encryption before destination verification
during restore database creation
during database restore
during file restore
after restore before reconciliation
after reconciliation before proof commit
after proof commit before response
```

Every profile records exact expected and observed distributions.

## Restore-proof gates

Before acceptance:

- create and verify at least two local encrypted generations;
- prove both can be independently verified;
- restore at least one current generation from an empty disposable environment;
- restore at least one older retained generation or equivalent earlier-schema fixture and apply separately approved forward migrations;
- prove one mismatched database/file generation fails closed;
- prove one corrupted object fails closed;
- reconcile database references and restored files;
- run typed-service read-only smoke checks;
- record elapsed time against the four-hour proving-ground RTO;
- preserve no plaintext secret or package material outside approved disposable roots after cleanup.

## Exact changed-path allowlist

Existing files allowed to change:

```text
src/kaizen/ops_db/migrations/manifest.json
src/kaizen/ops_service/__init__.py
src/kaizen/ops_service/contracts.py
src/kaizen/ops_service/errors.py
src/kaizen/ops_service/repository.py
src/kaizen/ops_service/service.py
src/kaizen/ops_service/serialization.py
docs/OPERATIONS_DATABASE_PROVING_GROUND.md
tests/test_ops_db_migration_runner.py
tests/test_ops_db_migration_hammer.py
tests/test_ops_service_contracts.py
tests/test_ops_service_repository.py
tests/test_ops_service_service.py
tests/test_ops_service_integration.py
tests/test_ops_service_hammer.py
```

New source paths allowed:

```text
src/kaizen/ops_recovery/__init__.py
src/kaizen/ops_recovery/contracts.py
src/kaizen/ops_recovery/generation.py
src/kaizen/ops_recovery/backup.py
src/kaizen/ops_recovery/encryption.py
src/kaizen/ops_recovery/restore.py
src/kaizen/ops_recovery/consistency.py
src/kaizen/ops_recovery/retention.py
src/kaizen/ops_db/migrations/0004_recovery_and_retention_planning.sql
```

New test paths allowed:

```text
tests/test_ops_recovery_contracts.py
tests/test_ops_recovery_generation.py
tests/test_ops_recovery_backup.py
tests/test_ops_recovery_restore.py
tests/test_ops_recovery_consistency.py
tests/test_ops_recovery_retention.py
tests/test_ops_recovery_integration.py
tests/test_ops_recovery_hammer.py
```

No other platform path may change without owner-approved packet revision.

## Required verification gates

- complete existing platform suite;
- complete Packet 016D and 016E service suites;
- complete recovery/retention unit suite;
- live disposable PostgreSQL plus disposable filesystem integration suite;
- all required recovery and retention hammer profiles;
- migration empty-state, upgrade, interruption, replay, and drift proof for migrations `0001–0004`;
- two verified encrypted local generations;
- current-generation disposable restore proof;
- older-generation or earlier-schema restore-and-forward proof;
- mismatched-generation fail-closed proof;
- corrupted-object fail-closed proof;
- consistency inspection proof;
- retention dry-run no-mutation proof;
- compileall;
- available Ruff check if configured and installed;
- package imports;
- static capability review;
- changed-text integrity review;
- secret exposure scan;
- exact changed-path verification;
- migration source-hash verification;
- retained migration apply and idempotent replay;
- retained role and ownership inspection;
- clean platform Git state after local commit;
- no platform remote;
- no vault, Northstar, Kaizen MCP, Go8, staging, or canonical mutation.

## Implementation return

Return must include:

- exact starting and ending commits;
- exact changed paths;
- migration ID and SHA-256;
- backup and encryption tool/version evidence;
- two generation IDs and object hashes;
- generation consistency and destination-copy verification;
- restore root and database isolation proof without leaking physical secrets;
- current and older-generation restore results;
- mismatch and corruption failure evidence;
- consistency inspection results;
- retention-plan inputs, counts, blockers, and digest;
- explicit no-deletion proof;
- crash-boundary results;
- hammer distributions;
- full test totals;
- compile/lint/import/integrity/secret-scan status;
- known limitations;
- explicit confirmation that deletion execution, production deployment, network adapters, Observatory, IMI, vault, and remote creation were not implemented.

## Stop conditions

Stop on:

- platform checkpoint drift;
- need for an unapproved dependency or path;
- inability to produce a database-consistent dump boundary;
- inability to bind database and file generations cryptographically;
- inability to encrypt without secret leakage;
- inability to restore into a disposable target only;
- inability to prove database/file reconciliation;
- inability to preserve Class A lineage or hold blockers in retention planning;
- any attempted deletion or live restore;
- unexplained hammer distribution;
- need for cloud automation, PITR, replication, deployment, MCP, network, Observatory, IMI, vault, or remote scope.

## Approval gate

Implementation may begin only after the owner approves this exact packet SHA-256 against platform starting commit:

```text
b4e219633a9c16cd0d9e2425e33a89572754ac06
```
