---
id: kz-spec-01KTV8A2N4Q6S8W0Y2Z4ABCDEF
type: spec
status: draft
project: kaizen-platform
created: 2026-06-10T15:02:00-04:00
updated: 2026-06-10T15:02:00-04:00
review_status: pending-review
authority: proposed
summary: "Milestone 7 durable-recoverability definition for Kaizen's critical local repositories and immutable operation evidence."
---

# Milestone 7 - Durable Recoverability

## Status

Proposed milestone definition. This document authorizes no backup execution, remote creation, push, archive upload, secret handling, restore, deletion, or cleanup until the owner accepts the exact audited SHA-256 and separately approves implementation task packets.

## Primary outcome

Kaizen can recover its critical local project state after loss of the current machine using an independently verified off-machine backup, without changing canonical authority, creating unauthorized Git remotes, exposing private vault content, or relying on an untested archive.

## Why this milestone is next

Milestones 1 through 6 proved governed project intelligence, exact mutation controls, implementation return, and canonical recovery from interrupted operations. They did not prove recovery from device loss.

The following remain local-only:

```text
C:\dev\kaizen\platform
C:\dev\kaizen\vault
C:\dev\kaizen\staging
```

The docs repository has an authorized remote, but platform, vault, and staging evidence do not have verified off-machine recovery.

Milestone 7 addresses that single risk before reliability work, the controlled pilot, physical database design, or production MCP architecture.

## Authority and boundary

Accepted authority:

- Kaizen Project Standard v0.2;
- Decision 0013 local-only period and explicit backup checkpoint;
- Decision 0014 backup/remote checkpoint requirements;
- accepted Roadmap v0.3 at SHA-256 `59f858b754287e987f5653351d8d187ccd7cc55f3029474275209ea8fdcc961b`;
- Result 102 owner acceptance.

Milestone 7 does not alter the system-of-record model:

```text
Markdown vault = canonical project intelligence
Git = supplementary version history
staging = noncanonical candidates plus immutable governed-operation evidence
Postgres = not implemented
Qdrant = not implemented
```

Backup copies are recovery artifacts, never new sources of truth.

## Protected roots

### Required

```text
C:\dev\kaizen\vault
C:\dev\kaizen\platform
C:\dev\kaizen\staging
```

### Verification-only

```text
C:\dev\kaizen-docs
```

`kaizen-docs` already has an authorized upstream. Milestone 7 must prove that the accepted branch and required commits can be recovered from that upstream, but it need not duplicate every Git object inside the new archive unless the selected implementation justifies doing so.

### Deferred

```text
C:\dev\kaizen-mcp
C:\dev\chatgpt-mcp
```

- `kaizen-mcp` remains a temporary non-Git proving ground and belongs to Milestone 8 reliability/migration analysis.
- `chatgpt-mcp` is development tooling, not canonical Kaizen state.

A later task packet may include their reproducibility metadata, but Milestone 7 must not expand into MCP productionization.

## Recovery classes

### R1 - Canonical project intelligence

Protect the full vault repository, including:

- tracked canonical Markdown;
- local Git history and refs;
- `_governance/promotion-log.jsonl`;
- repository configuration needed for local recovery;
- no remote creation.

### R2 - Deterministic platform implementation

Protect the full platform repository, including:

- tracked source and tests;
- local Git history and refs;
- repository configuration needed for local recovery;
- no generated virtual environment, cache, or disposable build output unless separately justified.

### R3 - Governed staging and immutable evidence

Protect staging content required to explain or recover governed operations, including:

- immutable plans;
- approvals;
- validation results;
- reviewed diffs;
- preserved prior and candidate bytes;
- result evidence;
- operation-status evidence;
- append-only evidence required for audit or recovery.

Disposable test fixtures, temporary caches, and proven-rebuildable scratch files must be explicitly classified before exclusion. Nothing may be excluded merely because it is inconvenient or large.

### R4 - Recovery metadata

Protect a small recovery manifest containing:

- backup format and version;
- creation timestamp in UTC;
- source machine identifier that does not expose secrets;
- protected roots;
- exact repository branches and full HEAD commits;
- clean/dirty state at capture;
- file counts and byte counts;
- exact archive or object hashes;
- exclusions and reasons;
- encryption method identifier;
- restore instructions;
- verification procedure;
- responsible owner;
- retention class.

The manifest must not contain passphrases, tokens, private keys, recovery keys, or raw secrets.

## Required backup posture

Milestone 7 must implement a backup design with all of the following properties:

1. off-machine;
2. encrypted before leaving the source machine;
3. owner-controlled;
4. versioned or retention-aware;
5. hash-verifiable;
6. restorable without the original working directories;
7. independent of a platform or vault Git remote;
8. explicit about secrets and excluded material;
9. recoverable through documented local tools;
10. tested through a disposable restore.

The specification is storage-provider neutral. Acceptable implementation families may include:

- encrypted archive copied to owner-controlled cloud storage;
- encrypted archive copied to removable media stored separately;
- a combination providing two independent off-machine copies.

The owner must approve the exact storage destination, visibility, encryption-key custody, and retention policy before implementation.

## Prohibited shortcuts

The following do not satisfy Milestone 7 by themselves:

- copying folders to another path on the same machine;
- merely creating a Git remote without restore proof;
- merely pushing platform or vault to a private repository;
- relying on `kaizen-docs` Git history to recreate vault or staging evidence;
- unencrypted cloud synchronization;
- an archive whose contents or hash were never verified;
- an archive that cannot restore Git history;
- screenshots, file lists, or verbal claims without bytes;
- backing up only canonical Markdown while discarding governed operation evidence;
- treating temporary caches as evidence without classification.

## Secrets boundary

Milestone 7 must inventory, but must not accidentally archive, plaintext secrets.

Required behavior:

- search protected roots for likely credential and secret files using bounded patterns;
- classify each finding as required recovery material, configuration template, false positive, or secret requiring a separate protected mechanism;
- exclude plaintext tokens, private keys, passwords, session cookies, `.env` secrets, and tunnel credentials from the general project archive unless a separately approved encrypted-secrets procedure covers them;
- include redacted templates or a secrets-recovery checklist when required for reproducibility;
- record exclusions without recording secret values.

Secrets management is not being redesigned in this milestone.

## Backup capture consistency

Each protected repository must be clean before the accepted baseline backup unless an exact owner-approved exception records the dirty paths and reason.

Required preflight:

```text
kaizen-docs: branch, full HEAD, clean state, upstream sync
kaizen-platform: branch, full HEAD, clean state, no remote unless separately approved
kaizen-vault: branch, full HEAD, clean state, no remote unless separately approved
staging: bounded integrity inventory and operation-evidence consistency
```

The backup must bind the vault, platform, and staging snapshots into one recovery set so cross-root evidence cannot silently drift.

## Restore proof

A backup is not accepted until restored into a disposable location outside all live Kaizen roots.

Recommended disposable root:

```text
C:\dev\kaizen-restore-proof\<backup-id>
```

The exact path requires implementation-packet approval.

The restore proof must verify:

### Vault

- repository opens as Git;
- expected branch and full HEAD match;
- tracked files match Git;
- canonical target hashes match the manifest;
- governance log hash and line count match;
- no remote appears unless explicitly present in the source repository;
- repository is clean.

### Platform

- repository opens as Git;
- expected branch and full HEAD match;
- tracked files match Git;
- repository is clean;
- the accepted test profile can run after environment reconstruction from committed configuration;
- no source-machine virtual environment is required.

### Staging

- expected operation directories and evidence files exist;
- exact file counts and selected or complete hashes match the manifest;
- immutable plan, approval, result, and reviewed-diff evidence remains readable;
- at least the two Packet 010F amendment operations can be inspected from restored evidence;
- no live canonical mutation is performed.

### Docs recovery verification

- clone or fetch from the existing authorized docs upstream into the disposable restore root;
- verify the accepted Roadmap v0.3, Result 102, and current docs HEAD;
- do not create any new remote.

## Destructive-test boundary

Milestone 7 must not prove recovery by deleting or corrupting live roots.

Allowed:

- restore into disposable paths;
- simulate missing files only inside the disposable restore copy;
- verify failure on a deliberately damaged disposable copy;
- delete the disposable restore tree only after evidence capture and exact approval if deletion tooling is required.

Forbidden:

- rename, remove, overwrite, reset, clean, or corrupt live docs, platform, vault, staging, MCP, or tooling roots;
- use the backup as permission to weaken existing evidence-preservation rules.

## Required failure injections

Implementation task packets must include at least:

1. archive hash mismatch;
2. wrong decryption key or unavailable key path;
3. truncated archive;
4. missing vault Git object or ref;
5. missing staging evidence file;
6. manifest/source commit mismatch;
7. restore into an existing nonempty destination;
8. secret-pattern finding that must be excluded or separately handled;
9. same-machine-only copy falsely presented as off-machine;
10. docs remote reachable but vault/platform/staging backup unavailable.

Expected behavior is deterministic failure with no live-root mutation and clear recovery guidance.

## Retention and generations

Milestone 7 must define and prove a minimal generation policy.

Recommended planning baseline:

```text
one initial accepted baseline
one subsequent incremental or replacement generation
both retained until the second generation restores successfully
```

The implementation packet must define:

- naming;
- timestamps;
- retention count or period;
- deletion authority;
- key custody;
- how superseded generations are verified before deletion;
- how backup age is reported.

No automated deletion is authorized by this milestone definition alone.

## Human-only decisions

The owner must explicitly decide:

- storage destination or destinations;
- whether cloud, removable media, or both are used;
- acceptable privacy and visibility;
- encryption mechanism;
- encryption-key custody and recovery;
- retention period or generation count;
- whether any platform or vault remote is ever created;
- who may execute backup and restore;
- when a generation may be deleted.

Agents may inspect, inventory, hash, prepare manifests, run disposable verification, and draft evidence only within accepted task-packet scope.

## Expected packet sequence

### 011A - Select backup posture and freeze contracts

- inventory roots and evidence classes;
- choose storage family, encryption, key custody, retention, and exclusions;
- define manifest and restore contracts;
- produce exact owner decision.

Documentation and read-only inspection only.

### 011B - Implement bounded backup creation

- create the recovery-set builder or exact operator procedure;
- generate encrypted backup and manifest;
- transfer to the approved off-machine destination;
- verify destination object hashes;
- no live-root mutation beyond permitted read access and new local temporary output.

### 011C - Prove disposable restore and failures

- restore into an approved disposable root;
- verify vault, platform, staging, and docs recovery;
- run required failure injections;
- preserve test evidence;
- do not touch live roots.

### 011D - Governed return and Milestone 7 closure

- record exact backup generation, hashes, storage class, retention, restore results, limitations, and owner instructions;
- update canonical Kaizen current state through governed amendment if required;
- complete closure audit;
- obtain owner closure acceptance.

The sequence may be split further only when a real security or evidence boundary requires it.

## Milestone exit criteria

Milestone 7 is complete only when:

1. exact owner-approved backup posture exists;
2. required roots and exclusions are inventoried;
3. encrypted off-machine recovery bytes exist at the approved destination;
4. archive/object and manifest hashes verify independently;
5. secrets handling is explicit and no plaintext secret leak is found;
6. a full disposable restore succeeds without using live working directories;
7. restored vault and platform Git history, branches, HEADs, and clean states match;
8. restored staging evidence supports inspection of accepted governed operations;
9. docs recovery from the existing upstream is verified;
10. all required failure injections fail safely;
11. at least two generations or the owner-approved equivalent retention proof exists;
12. live roots remain unchanged and clean;
13. no platform or vault remote or push occurs without separate explicit approval;
14. governed implementation return and closure audit pass;
15. the owner explicitly accepts the exact Milestone 7 closure evidence.

## Explicit exclusions

- physical Postgres schema or database backup;
- Qdrant backup;
- production MCP packaging or migration;
- stale loader fix or other Milestone 8 implementation;
- controlled mock-project execution;
- broad disaster-recovery automation or daemon;
- continuous synchronization service;
- generic cloud management tooling;
- new Git remotes for platform or vault unless a separate owner decision explicitly adds them;
- vault push;
- automated retention deletion;
- secrets-manager redesign;
- full-device image backup;
- CI/CD.

## Required planning return

Before implementation, return:

- verified repository and staging checkpoint;
- inventory of protected and excluded paths;
- threat and privacy summary;
- exact backup-medium options with tradeoffs;
- recommended storage and encryption posture;
- key-custody plan;
- manifest schema;
- restore procedure;
- failure-injection matrix;
- packet sequence;
- exact owner acceptance phrase bound to this specification SHA-256.
