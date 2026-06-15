---
id: kz-spec-01KV6LFQ8M2N7R5C3Y9P4T6BQ0
type: specification
status: proposed
project: kaizen-platform
created: 2026-06-16T01:30:00Z
updated: 2026-06-16T01:30:00Z
review_status: pending
authority: proposed
summary: "Define backup, restore, consistency, retention, deletion, and disaster-recovery design for the Milestone 11 first slice."
---

# Milestone 11 Backup, Restore, and Retention Design

## Scope

Design only. No backup command, database, credential, SQL, or deletion implementation is authorized.

## Recovery object

One verified recovery generation contains:

```text
encrypted logical database backup
immutable migration-set manifest
artifact/evidence archive reference or encrypted archive
storage-root mapping manifest
repository checkpoint manifest
recovery-generation manifest
per-object SHA-256 hashes
creation tool and version evidence
```

The database backup and file archive must share one generation ID. Neither is considered a complete recovery generation alone.

## Generation manifest

Conceptual fields:

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
record_counts by family
file_counts and total bytes
consistency_check_result
encryption_tool and version
```

## Backup posture

Align with accepted Kaizen recoverability doctrine:

- encrypted portable archive;
- copy unchanged to private Google Drive and separately stored USB SSD;
- minimum two verified generations;
- maximum normal backup age 30 days unless later operational evidence requires shorter;
- owner-only deletion after successor restore proof;
- no database or vault remote creation.

Database operational frequency may later be shorter than the 30-day full portable archive target. Packet 016C must decide whether daily local encrypted backups are needed after measured write volume exists.

## Consistent capture sequence

Preferred design:

1. record backup start and generation ID;
2. establish a database-consistent snapshot or logical dump boundary;
3. freeze the list of referenced artifact/evidence files for that database snapshot;
4. archive exact files and storage-root mapping;
5. calculate hashes;
6. run database-to-file consistency checks;
7. write immutable generation manifest;
8. encrypt database backup, file archive, and manifest as one recovery package or cryptographically bound set;
9. transfer and independently verify destination hashes.

A backup generation fails if database references cannot be matched to the captured file generation.

## Restore order

To disposable root only:

1. verify encrypted package hashes;
2. decrypt into isolated temporary workspace;
3. verify internal generation manifest and object hashes;
4. create disposable Postgres instance at supported version;
5. install the exact approved migration baseline required by the backup format;
6. restore database using the documented restore path;
7. restore storage roots under disposable locations;
8. apply storage-root mapping for the disposable environment;
9. run database integrity checks;
10. run file existence and hash reconciliation;
11. run typed-service read-only smoke tests;
12. record restore result and exact mismatches;
13. do not promote restored state into live use without separate recovery authority.

## Migration and restore compatibility

Every backup records schema version and migration-set hash.

Restore policy:

- restore first into a database compatible with the captured schema version;
- then apply separately approved forward migrations;
- never silently restore into a newer incompatible schema;
- migration rollback is not assumed possible;
- destructive migrations require a verified pre-migration backup;
- migration and restore tests must cover at least the oldest retained generation.

## Mismatched generations

### Database newer than files

Mark referenced artifacts as missing or generation-mismatch. Do not claim verified provenance. Recovery remains incomplete.

### Files newer than database

Files without database provenance remain unregistered. Do not import automatically.

### Missing migration set

Restore blocked. Required migration source and hashes are part of the recovery evidence.

### Hash mismatch

Restore fails closed and preserves the mismatched objects for investigation.

## Recovery objectives

Initial design targets:

```text
RPO:
30 days for portable off-machine disaster recovery;
local operational backup target to be chosen in Packet 016C based on measured write frequency

RTO:
4 hours for local disposable restore proof of the first-slice database and referenced evidence subset
```

These are proving-ground targets, not production SLA promises.

## Backup while writes are active

The design must use a database-consistent snapshot. File references created after the snapshot boundary are excluded from that generation. Files referenced before the boundary must be included and hash-verified.

Service capabilities must expose or record a backup cutoff marker sufficient to explain inclusion and exclusion.

## Retention policy

### Class A

Permanent authority-linked lineage:

- frozen return manifests;
- rejected return history;
- provenance linked by accepted audit, owner acceptance, or completion records.

### Class B

Project lifetime plus 12 months:

- ordinary execution attempts;
- verification runs;
- unaccepted provenance;
- idempotency records through required retry and audit horizons.

### Class C

Future telemetry: 90 days unless incident or operation hold applies.

### Class D

Derived read models: no independent retention.

## Holds

Deletion is blocked by:

```text
owner hold
accepted audit or completion reference
active incident
unresolved recovery
legal or security hold
pending backup or restore investigation
```

Holds are explicit records with actor, reason, creation time, scope, and release authority.

## Deletion planning

Deletion always begins with deterministic dry-run evidence:

- candidate IDs and counts;
- retention class and expiry basis;
- blocked IDs and reasons;
- authority-linked records retained;
- project tombstone effect;
- file deletion effects, if separately authorized;
- plan digest.

The database metadata deletion plan does not automatically authorize deletion of artifact or evidence bytes.

## Deletion execution

Future implementation requirements:

- exact approved plan digest;
- optimistic live-state recheck;
- transactionally delete only eligible operational metadata;
- preserve Class A lineage and tombstones;
- emit deletion result with exact counts;
- retain failure evidence;
- verify no canonical Markdown, Git, governance JSONL, or immutable staging evidence changed.

## Backup of deleted state

A backup generation may retain records later deleted from live operational storage. Backup retention and destruction require owner authority and must account for privacy obligations and accepted evidentiary holds.

## Secret and encryption posture

- credentials and encryption keys remain owner-managed;
- backup packages use accepted encryption tooling and record version/hash evidence;
- secrets are never embedded in manifests;
- temporary plaintext is deleted only after destination hashes verify and owner-approved cleanup;
- logs redact credentials and local sensitive paths.

## Restore proof cadence

Before implementation acceptance:

- restore from empty disposable environment;
- verify at least two generations;
- prove database and file consistency;
- prove one older schema generation can be restored and upgraded;
- prove a mismatched generation fails closed;
- record elapsed time against RTO target.

## Design conclusion

The first-slice database is not recoverable unless its referenced evidence files, migration history, storage-root mappings, and repository checkpoints are cryptographically bound into the same recovery generation.