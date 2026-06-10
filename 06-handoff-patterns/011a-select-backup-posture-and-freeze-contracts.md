---
id: kz-tp-01KTV8G8N0Q2S4W6Y8Z0ABCDEF
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-10T15:18:00-04:00
updated: 2026-06-10T15:18:00-04:00
review_status: pending-review
authority: proposed
summary: "Packet 011A - select the Milestone 7 backup posture and freeze the inventory, manifest, restore, secrets, and retention contracts."
primary_spec: kz-spec-01KTV8A2N4Q6S8W0Y2Z4ABCDEF
---

# Task Packet 011A - Select Backup Posture and Freeze Contracts

> This packet is a non-authoritative draft. It authorizes read-only inspection, bounded inventory, threat/privacy analysis, option comparison, and documentation only. It does not authorize archive creation, encryption, upload, restore, remote creation, push, deletion, secret movement, or any live-root mutation.

## Objective

Produce the exact owner decision package required before Kaizen creates its first durable off-machine recovery set.

Packet 011A must answer:

1. exactly which files and Git objects must be protected;
2. which material is rebuildable or safely excluded;
3. which likely secrets require exclusion or separate handling;
4. which storage posture best fits the owner's privacy, cost, access, and recovery needs;
5. which encryption and key-custody posture is practical and recoverable;
6. what immutable manifest binds vault, platform, and staging into one recovery set;
7. how restore success and failure are proven;
8. how generations are named, retained, and eventually deleted;
9. which exact decisions remain owner-only before Packet 011B.

## Bound predecessor state

```text
Milestone 6: closed
Milestone 7 specification: accepted
Milestone 7 specification SHA-256:
a5d9cac899fd82888201113bbc2e340030be10c6184cfa036cffcd40ff4294eb

Roadmap v0.3 SHA-256:
5feec5bef8bf48c30be36fddfe1547f47810145407d1e9cc47301e0c6dddc248

Milestone 7 owner acceptance:
Result 104

kaizen-docs:
branch main
HEAD 0138a1fb54f888012cf9e72ab8c06cc26a3b54b2
clean
upstream origin/main

kaizen-platform:
branch main
HEAD 1b8be1d6d42d768587dddb2be8415fa24b670561
clean
remote none

kaizen-vault:
branch main
HEAD fc4b03397d770f9b4a8a2f5e7f71e33981bcd181
clean
remote none
```

Any mismatch stops Packet 011A before new evidence is written.

## Read first

- `ROADMAP_V0.3.md`;
- `05-specs/milestone-7-durable-recoverability.md`;
- Results 097, 102, 103, and 104;
- Decisions 0003, 0004, 0013, and 0014;
- canonical vault and platform current-state records;
- staging operation-evidence layout and the two Packet 010F amendment operations;
- repository ignore rules and committed environment/configuration files;
- existing docs remote and recovery instructions.

Search before assuming paths or filenames.

## Authorized actions

### Allowed

- inspect fixed roots and repository state;
- generate bounded file-tree manifests and counts;
- hash explicit files or bounded path sets;
- inspect Git branches, full HEADs, refs, remotes, and cleanliness;
- inspect `.gitignore`, configuration templates, package manifests, and restore prerequisites;
- search bounded roots for likely secret-bearing filenames and text markers;
- classify findings without recording secret values;
- inspect staging evidence structure read-only;
- compare backup, storage, encryption, and key-custody options;
- draft manifest, restore, retention, and operator contracts;
- create and audit Packet 011A planning evidence in `kaizen-docs`;
- commit and push only the reviewed docs planning batch to existing `origin/main`.

### Prohibited

- create an archive, bundle, encrypted object, or backup generation;
- copy protected roots to another storage destination;
- upload anything;
- restore anything;
- create or modify platform or vault remotes;
- push platform or vault;
- read or expose secret values;
- move secrets;
- delete or clean files;
- initialize Git;
- modify platform, vault, staging, Kaizen MCP, or Go8;
- begin Packet 011B, 011C, or 011D;
- begin Milestone 8 or later work.

## Workstream A - Protected-root inventory

### Vault inventory

Record:

- branch, full HEAD, clean state, remotes;
- tracked-file count and byte count;
- Git object and repository metadata requirements;
- canonical Markdown count by major project area;
- governance-log path, size, line count, and SHA-256;
- untracked, ignored, generated, backup, temporary, malformed, or suspicious files;
- configuration required to reopen the repository locally;
- any files whose exclusion would break canonical interpretation or governed recovery.

### Platform inventory

Record:

- branch, full HEAD, clean state, remotes;
- tracked-file count and byte count;
- Git object and repository metadata requirements;
- source, tests, packaging, and environment-reconstruction inputs;
- virtual environments, caches, compiled files, coverage, and other rebuildable outputs;
- local-only configuration or scripts required to run accepted test profiles;
- any files whose exclusion would prevent deterministic reconstruction.

### Staging inventory

Record, without mutation:

- top-level evidence classes;
- operation count;
- per-operation file count and byte count;
- exact inventory for the two Packet 010F amendment operations;
- immutable versus disposable evidence classes;
- candidate exclusions and justification;
- whether any operation evidence points to files outside staging;
- whether current restore verification can inspect plans, approvals, results, diffs, and preserved bytes without live mutation.

If the current tools cannot inspect staging with sufficient boundedness, record the tooling gap. Do not replace it with broad arbitrary execution.

### Docs recovery inventory

Record:

- existing upstream;
- branch and full HEAD;
- exact accepted roadmap and Result 102 hashes;
- minimum clone/fetch instructions;
- any local-only ignored file required for recovery;
- whether docs recovery depends on credentials not covered by the project archive.

## Workstream B - Secrets and privacy classification

Search only bounded protected roots for likely secret material using filename and text-pattern classes such as:

```text
.env
*.pem
*.key
id_rsa
id_ed25519
api_key
token
password
secret
ngrok auth
credential
cookie
session
```

For each finding record only:

```text
path or redacted path class
finding class
true positive / false positive / unknown
required for recovery: yes / no / unknown
proposed treatment
```

Never copy secret values into the report, logs, manifest, or chat.

Treatment classes:

- exclude from general archive;
- include redacted template only;
- document manual recreation;
- protect through separately approved encrypted-secrets mechanism;
- false positive requiring no action.

## Workstream C - Backup-posture options

Compare at least these postures:

### Option A - Encrypted cloud object only

One encrypted recovery object in owner-controlled cloud storage.

Assess:

- cost;
- convenience;
- off-machine assurance;
- account lockout risk;
- provider visibility;
- key-separation quality;
- recovery from a new machine;
- retention and versioning;
- mobile accessibility if relevant.

### Option B - Encrypted removable media only

One encrypted recovery object on removable media stored separately from the computer.

Assess:

- cost;
- physical loss or damage;
- update friction;
- ransomware separation;
- key custody;
- off-site practicality;
- restore speed.

### Option C - Encrypted cloud plus removable media

Two independent off-machine copies from the same verified recovery set.

Assess:

- strongest recoverability;
- extra operational effort;
- cost;
- generation synchronization;
- key custody;
- deletion and retention complexity.

### Option D - Another owner-proposed posture

Evaluate only if it meets every accepted Milestone 7 property. Do not present a Git remote alone as an equivalent to the required encrypted recovery set.

## Workstream D - Encryption and key custody

Compare practical local encryption families based on:

- authenticated encryption;
- mature implementation;
- Windows availability;
- scriptability through narrow operator commands;
- ability to encrypt before upload;
- deterministic verification metadata;
- recovery on a replacement machine;
- no secret in command history or manifest;
- owner comprehension and recoverability.

The report must distinguish:

```text
archive format
encryption format
storage destination
key or passphrase custody
recovery instructions
```

Required key-custody options:

1. owner-held passphrase in an existing password manager;
2. printed or offline recovery copy stored separately;
3. separate key file protected outside the project archive;
4. combined approach where loss of one device does not lose both data and key.

Do not generate a passphrase, key, or encrypted file in Packet 011A.

## Workstream E - Recovery-set manifest contract

Draft a machine-readable manifest schema with at least:

```text
schema_version
backup_id
created_at_utc
created_by
source_machine_id
protected_roots
source_root_id
source_path_class
repository_flag
branch
head_commit
clean_state
remote_posture
file_count
byte_count
content_manifest_sha256
git_verification
staging_operation_summary
exclusions
secret_handling_summary
archive_format
encryption_format
storage_class
archive_sha256
encrypted_object_sha256
retention_class
previous_backup_id
restore_contract_version
```

Rules:

- paths must be portable or path-classified rather than assuming the original drive layout;
- no secret value may appear;
- hashes must identify exactly which bytes they bind;
- unknown values are explicit, not omitted silently;
- manifest changes require versioning;
- one manifest binds all protected roots in the recovery generation;
- the encrypted object hash is taken after encryption;
- storage-provider object identifiers may be recorded only when they reveal no credential.

Packet 011A must define whether the manifest is:

- inside the encrypted object;
- outside as a non-sensitive verification companion;
- both, with exact consistency rules.

## Workstream F - Restore contract

Draft the exact Packet 011C restore procedure, including:

1. acquire approved recovery object;
2. verify encrypted-object hash before decryption;
3. obtain key through the owner-approved custody path;
4. decrypt into a new disposable recovery workspace;
5. verify archive hash and manifest consistency;
6. restore each root into a unique nonempty-safe destination;
7. verify Git repositories, refs, branches, HEADs, tracked files, and cleanliness;
8. verify canonical hashes and governance log;
9. verify staging operation evidence;
10. recover docs from existing upstream;
11. reconstruct platform environment from committed inputs;
12. run accepted verification profiles;
13. record results without changing live roots;
14. preserve restore evidence until closure.

Define deterministic failure behavior for every required injection in the Milestone 7 specification.

## Workstream G - Retention contract

Draft a minimal policy covering:

- backup IDs and naming;
- full versus incremental or replacement generations;
- minimum two-generation proof;
- creation cadence after Milestone 7;
- maximum acceptable backup age;
- retention count or period;
- superseded-generation verification;
- owner-only deletion authority;
- no deletion before replacement restore proof;
- handling of failed or partial generations;
- how storage-provider versioning interacts with Kaizen retention.

Packet 011A must not choose automatic deletion.

## Required decision package

Return one owner decision table with exact options and a recommendation for:

| Decision | Required choice |
|---|---|
| Primary storage | cloud / removable / both / accepted alternative |
| Secondary storage | none / removable / cloud / accepted alternative |
| Visibility | private encrypted object only |
| Encryption | selected format and implementation |
| Key custody | password manager / offline copy / key file / combined |
| Key recovery | exact replacement-machine procedure |
| Retention | generation count or duration |
| Backup cadence | manual trigger and maximum age |
| Staging exclusions | exact classified list |
| Secret treatment | exact exclusions and separate-handling decisions |
| Platform remote | remain none / separately proposed later |
| Vault remote | remain none / separately proposed later |
| Deletion authority | owner only |
| Restore destination | approved disposable path class |

The recommendation must explain why it is proportionate for one owner and one machine today.

## Required documentation outputs

Packet 011A may create only:

- one root/evidence inventory result;
- one backup-posture options and recommendation result;
- one manifest/restore/retention contract document or proposed specification amendment;
- one Packet 011A security/steward audit;
- one owner-decision record after explicit acceptance;
- narrowly necessary roadmap or entrypoint status updates after approval.

Search before creating files and avoid document duplication.

## Acceptance criteria

Packet 011A is complete only when:

1. predecessor hashes and repository state match;
2. vault and platform inventories are bounded and reproducible;
3. staging immutable evidence is inventoried or an exact tooling gap is documented;
4. secret findings are classified without exposing values;
5. at least three viable storage postures are compared;
6. encryption and key-custody options are compared honestly;
7. manifest, restore, and retention contracts are complete enough for Packet 011B/011C planning;
8. a single recommended posture is presented with tradeoffs;
9. platform/vault remotes remain unchanged;
10. no backup, upload, restore, deletion, or live-root mutation occurred;
11. exact docs paths, hashes, diff checks, commit, and push evidence are returned;
12. the owner explicitly accepts the exact posture and contracts before Packet 011B begins.

## Stop gates

Stop when:

- any protected repository is dirty unexpectedly;
- a protected-root or staging path cannot be inspected safely;
- a secret value appears in output or evidence;
- a recommendation depends on unsupported software or unavailable recovery access;
- storage cost, visibility, or retention cannot be determined honestly;
- encryption key recovery depends solely on the source machine;
- the proposed posture creates a platform or vault remote without separate authority;
- the packet would need to create, encrypt, upload, restore, or delete bytes;
- the owner has not selected the exact posture.

## Non-scope

- implementation of backup tooling;
- archive or encryption execution;
- cloud account configuration;
- removable-media formatting;
- remote creation or push;
- secrets-manager migration;
- live restore;
- destructive live-root tests;
- MCP work;
- Postgres or Qdrant;
- controlled pilot execution;
- automated schedules or daemons;
- Packet 011B through 011D implementation.

## Owner gate after Packet 011A completion

The completion audit must provide a phrase bound to:

- exact Packet 011A SHA-256;
- selected storage posture;
- selected encryption format;
- exact key-custody arrangement;
- retention and cadence;
- secret and staging exclusions;
- manifest contract SHA-256;
- restore contract SHA-256;
- explicit confirmation that platform and vault remotes remain unchanged unless separately approved.
