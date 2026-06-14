---
id: kz-tp-01KV3EGEN3BACKUP000000000
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-14T16:40:00Z
updated: 2026-06-14T16:40:00Z
review_status: pending
authority: proposed
summary: "Create and independently restore post-Milestone-8 backup Generation 3 before the first canonical Northstar mutation."
---

# Task Packet 013E - Post-Milestone-8 Backup Generation 3

## Purpose

Create one new encrypted Kaizen recovery generation from the current clean pre-pilot state, retain Generations 1 and 2 unchanged, transfer identical encrypted Generation 3 bytes to private Google Drive and the owner-selected USB SSD, independently restore the Google Drive copy, and verify exact repository, manifest, archive, and test continuity.

This packet plans a Tier 3 backup and restore operation. Execution requires separate owner approval bound to the exact audited packet SHA-256 and the owner-supplied USB destination path.

## Accepted predecessor

```text
Generation 2 backup_id:
kz-backup-20260611T181735Z-64f1ccd3800c

Generation 2 encrypted SHA-256:
38531f8f944db1ebd49234562be29ecf369aabdb865bf1fd14d9ff03a2ac60a1

Generation 1 backup_id:
kz-backup-20260610T212509Z-52c691c34b21
```

Generation 3 must bind:

```text
previous_backup_id: kz-backup-20260611T181735Z-64f1ccd3800c
```

## Current protected checkpoints

```text
kaizen-docs HEAD:
e2d317d5f61ef6e6f00df87069075d82b46cf76d

kaizen-platform HEAD:
ba3b5feca90e4fb5cb02e34981dc7ed86942962f

kaizen-vault HEAD:
2487de669bc44ed50e54fd5dbbfdd128ce659dbb

Go8 HEAD:
312704a8b8505bdb64f28cc557171c10de8bd5bc

canonical current-state SHA-256:
e50fc7d1d88257ecf6a818a47673775a9f8960e999a6a843872306227dd2b8c7
```

Any checkpoint or clean-state mismatch before capture stops execution.

## Accepted tooling bindings

Reuse the existing owner-run scripts only:

```text
C:\dev\chatgpt-mcp\scripts\packet011c\Prepare-KaizenRecovery.ps1
SHA-256: 5d811b3d86b7201279fed352f662a1271f751eac428717f705b16f24cc8cd7fe

C:\dev\chatgpt-mcp\scripts\packet011c\Verify-KaizenRecoveryCopies.ps1
SHA-256: f51c5f33998514df8a223fc0f07a688c916c9754d2a3389eeaea60117cd5c67e

C:\dev\chatgpt-mcp\scripts\packet011d\Restore-KaizenRecovery.ps1
SHA-256: 6740978c9e4ca16a7fce79df7562e6a37f07f306da82a0465f045821fb0d406f

C:\dev\chatgpt-mcp\scripts\packet011d\Test-RestoredPlatform.ps1
SHA-256: 5824ddeb2a55c4e104058de6e071281fbcb87fecb3e17f44df2b87c9d3fc6be0
```

No script edit is presumed. If one is required, stop and return to planning with exact evidence.

## Owner-supplied execution binding

Before execution the owner must provide the exact mounted USB SSD destination root. The packet may not guess a drive letter or path.

Google Drive remains a manual browser transfer to the existing private dedicated recovery folder. No cloud API, synchronization agent, public sharing, or link-wide access is authorized.

## Preflight

Before capture:

1. verify docs, platform, vault, and Go8 exact HEADs and clean states;
2. verify docs upstream is synchronized;
3. verify platform, vault, and Go8 have no remotes;
4. verify Kaizen MCP remains non-Git;
5. inventory staging without mutation and record operation/file counts;
6. run redaction-safe secret classification over the protected inclusion set;
7. verify the four script hashes above;
8. verify age v1 executable/version/hash against the accepted recovery record;
9. verify owner custody of the existing Kaizen age identity without exposing it;
10. verify both prior generations remain present in Google Drive and on USB;
11. verify the Generation 2 encrypted hash at the retained destinations;
12. verify the Generation 3 workspace and restore destination do not already exist or are empty as required.

Stop on any mismatch, unresolved secret finding, missing prior generation, unavailable identity, ambiguous USB path, dirty source, or overwrite risk.

## Generation 3 capture

Use a new unique backup ID:

```text
kz-backup-<UTC timestamp>-<random suffix>
```

Temporary workspace:

```text
C:\dev\kaizen-backup-work\<generation-3-backup-id>
```

Requirements:

- bind `previous_backup_id` to Generation 2 exactly;
- include the accepted protected roots and manifests;
- preserve existing inclusion/exclusion policy;
- record source Git branches, HEADs, clean states, and remotes;
- record plaintext archive SHA-256 and size;
- encrypt locally with the existing Kaizen-specific age recipient;
- record encrypted SHA-256 and size;
- transfer no plaintext off the laptop;
- overwrite nothing;
- delete nothing;
- perform no automatic cleanup.

## Transfer and independent verification

Transfer the identical encrypted object and verification companion to:

```text
private Google Drive recovery folder
owner-selected USB SSD Generation 3 directory
```

Verify:

- Generations 1, 2, and 3 remain present;
- Google Drive re-download matches the local encrypted SHA-256 and size;
- USB copy matches the local encrypted SHA-256 and size;
- Generation 1 and 2 encrypted hashes remain unchanged;
- no prior generation or unrelated USB path was overwritten;
- Google Drive remains private with no public or link-wide sharing.

## Independent restore proof

Restore from the Google Drive re-download into:

```text
C:\dev\kaizen-restore-proof\<generation-3-backup-id>\google
```

Verify:

### Archive and manifest

- encrypted SHA-256 and size;
- plaintext archive SHA-256;
- safe archive paths;
- internal recovery manifest;
- exact Generation 2 predecessor binding;
- protected-root manifests with no unexpected or missing files.

### Vault

- valid Git repository;
- branch `main`;
- HEAD `2487de669bc44ed50e54fd5dbbfdd128ce659dbb`;
- clean state;
- no remotes;
- tracked-file integrity;
- governance-log integrity;
- current-state SHA-256 `e50fc7d1d88257ecf6a818a47673775a9f8960e999a6a843872306227dd2b8c7`.

### Platform

- valid Git repository;
- branch `main`;
- HEAD `ba3b5feca90e4fb5cb02e34981dc7ed86942962f`;
- clean state;
- no remotes;
- fresh Python 3.11 environment;
- dependencies reconstructed only from committed metadata;
- complete current platform test suite passes;
- tracked restored content remains unchanged.

### Staging

- complete manifest;
- governed operation evidence through the current-state amendment remains present;
- no operation is executed during restore verification.

### Docs

- fresh clone from the accepted existing upstream;
- exact docs HEAD `e2d317d5f61ef6e6f00df87069075d82b46cf76d`;
- Roadmap v0.3, Packet 013D, and current-state-alignment evidence present;
- no unrelated remote or mutation.

## Regression checks

Generation 3 must prove at least:

1. incorrect encrypted hash stops before decryption;
2. nonempty restore destination stops;
3. incorrect `previous_backup_id` stops;
4. missing Generation 1 or 2 at either retained destination blocks retention acceptance;
5. attempted overwrite or deletion of any prior generation stops;
6. current protected-root HEAD mismatch stops capture;
7. restored current-state hash mismatch fails verification.

Do not repeat the entire Packet 011D ten-injection matrix unless a changed script or recovery procedure invalidates prior proof.

## Evidence and retention

Record in one compact implementation/completion result:

- Generation 3 backup ID and predecessor;
- plaintext and encrypted hashes and sizes;
- source repository states and manifests;
- secret-classification result without matched values;
- Google Drive and USB verification;
- prior-generation retention checks;
- restore root and restored hashes;
- platform environment and full test result;
- seven regression results;
- exact retained files and cleanup posture;
- deviations or failures.

Retain:

- local encrypted Generation 3 source;
- Google Drive copy and re-download;
- USB copy;
- plaintext archive until both destination hashes verify and the owner separately approves cleanup;
- disposable restore and test evidence until Packet 013E closure acceptance;
- Generations 1 and 2 unchanged;
- encrypted identity under existing owner custody.

Deletion remains owner-only after successor restore proof. No cleanup is authorized by this packet.

## Git and source-change boundary

No source repository change is expected. The capture and restore operation must not commit, push, reset, clean, stash, or create remotes.

If a script defect is found, stop. Do not patch recovery tooling inside the live backup operation.

## Authorization boundary

Drafting, deterministic auditing, and committing this packet in `kaizen-docs` are authorized under the active Tier 1 workflow.

Backup creation, encryption, transfer, USB writing, Google Drive handling, decryption, restore, test execution in the disposable restore root, and regression execution require separate owner approval bound to:

- the exact audited Packet 013E SHA-256;
- the exact current repository checkpoints;
- the exact Generation 2 predecessor ID;
- the owner-supplied USB destination root.

## Explicit exclusions

- deletion or cleanup;
- prior-generation mutation;
- public Drive sharing;
- cloud API or automatic synchronization;
- source or script changes;
- canonical or staging mutation beyond read-only inventory;
- vault push or remote creation;
- Milestone 9 artifact creation or execution;
- Postgres, Qdrant, LangGraph, Hermes, UI, provider, or deferred work.
