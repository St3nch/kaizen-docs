---
id: kz-tp-01KTVAKMN4Q6S8W0Y2Z4ABCDEF
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-10T18:10:00-04:00
updated: 2026-06-10T18:10:00-04:00
review_status: pending-review
authority: proposed
summary: "Packet 011D - prove independent disposable restores from Google Drive and USB recovery copies and run bounded failure injections without touching live roots."
primary_spec: kz-spec-01KTV8A2N4Q6S8W0Y2Z4ABCDEF
---

# Task Packet 011D - Prove Disposable Restore and Failure Handling

> Non-authoritative draft. Implementation requires exact owner approval of the audited Packet 011D SHA-256.

## Objective

Prove that recovery generation:

```text
kz-backup-20260610T212509Z-52c691c34b21
```

can restore Kaizen independently from both approved off-machine copies:

```text
private Google Drive copy
owner-selected USB SSD copy
```

Restore only into disposable roots outside all live Kaizen paths, verify vault, platform, staging, and docs recovery, run required failure injections against disposable copies, and preserve bounded evidence.

Packet 011D must not modify any live root, delete any retained encrypted copy or identity, create platform or vault remotes, push platform or vault, create the second backup generation, or begin Packet 011E.

## Bound authority

```text
Milestone 7 specification SHA-256:
a5d9cac899fd82888201113bbc2e340030be10c6184cfa036cffcd40ff4294eb

Recovery-set contract SHA-256:
8e092ac761e60687bf8d3b8c78bdb83b7f6d68c58b47024a0ee01be987d3956b

Packet 011C implementation return SHA-256:
8019d6affd910f7c0e1332a64022c253588ac60ae9e8d85571a11c6081afbd4c

Packet 011C completion audit SHA-256:
30bc5d2e654c585a6677a54a478e381481aee1d42c675c68ad23c95df2bb71b0

Packet 011C owner completion acceptance:
Result 119
```

## Recovery object binding

```text
backup_id:
kz-backup-20260610T212509Z-52c691c34b21

encrypted filename:
kz-backup-20260610T212509Z-52c691c34b21.tar.age

encrypted SHA-256:
dfadd5b5a6014e41ef7e5d6921d9ff7a2ca4f478b798eb6c5a97643d9e296d04

encrypted size:
2,338,552 bytes

plaintext archive SHA-256:
95cac544c8e1ec4af3278a6396b14157b00bf41dcd0db77b3c8ed6867caf6c2a

public recipient:
age1ra3j0e7qc0sue0drv2dkpk0xc90exndck9uuqnwrc07d54wft40qksx8mc
```

The private identity and passphrase remain owner-controlled and must not enter chat, logs, repositories, manifests, command history, or evidence.

## Disposable restore roots

Recommended exact classes:

```text
C:\dev\kaizen-restore-proof\kz-backup-20260610T212509Z-52c691c34b21\google
C:\dev\kaizen-restore-proof\kz-backup-20260610T212509Z-52c691c34b21\usb
C:\dev\kaizen-restore-proof\kz-backup-20260610T212509Z-52c691c34b21\failures
C:\dev\kaizen-restore-proof\kz-backup-20260610T212509Z-52c691c34b21\docs-clone
```

Rules:

- each destination must not exist or must be verified empty before use;
- paths must remain outside docs, platform, vault, staging, kaizen-mcp, and chatgpt-mcp roots;
- no restore into any live directory;
- no junction, symlink, or reparse-point escape;
- no overwrite of unrelated data;
- no cleanup or deletion without separate exact owner authorization.

## Source copies

### Google Drive source

The owner manually downloads the encrypted object and verification companion into an exact temporary source folder outside live roots.

Before decryption:

- verify filename;
- verify size `2,338,552` bytes;
- verify SHA-256 `dfadd5...96d04`;
- verify companion binding;
- verify the source came from the private dedicated Kaizen recovery folder.

### USB source

Use the retained copy at the owner-confirmed mounted path, expected generation directory:

```text
<USB>:\kz-backup-20260610T212509Z-52c691c34b21
```

Before decryption:

- verify device is the backup SSD, not the identity/recovery USB;
- verify filename and companion;
- verify size and SHA-256;
- do not write to the USB source;
- do not rename, move, or delete USB evidence.

## Identity handling

The encrypted identity is supplied through an owner-controlled local path outside repositories.

Required behavior:

- owner enters the identity passphrase interactively;
- no passphrase argument, environment variable, transcript, or redirected output;
- no private identity content is printed;
- no Bitwarden automation or token access;
- identity is read only for decryption;
- failed passphrase attempts return a bounded failure without exposing details.

## Restore procedure

Implement as one narrow owner-run PowerShell procedure or two small scripts under:

```text
C:\dev\chatgpt-mcp\scripts\packet011d
```

The procedure must not be exposed as an MCP tool.

### Phase 1 - Preflight

Verify:

- Packet 011D exact approval;
- live docs/platform/vault repositories are clean at accepted HEADs;
- platform and vault still have no remotes;
- Go8 repository is clean;
- encrypted source hash and size match;
- restore root is new and empty;
- `age` v1.3.1 and `tar.exe` are available;
- identity path exists without reading or displaying its contents;
- source and destination paths are outside live roots.

Any mismatch stops before decryption.

### Phase 2 - Decrypt

For each source independently:

1. decrypt the `.tar.age` into a new disposable plaintext archive within that source's restore root;
2. verify plaintext archive SHA-256 equals:

```text
95cac544c8e1ec4af3278a6396b14157b00bf41dcd0db77b3c8ed6867caf6c2a
```

3. do not reuse the decrypted archive between Google and USB restores;
4. do not overwrite an existing archive;
5. stop on any decryption or hash mismatch.

### Phase 3 - Archive safety inspection

Before extraction:

- list every archive entry;
- reject absolute paths;
- reject drive-qualified paths;
- reject `..` traversal;
- reject entries escaping the disposable root;
- reject unsafe reparse-point or link semantics;
- verify required top-level payload classes exist:
  - `vault`;
  - `platform`;
  - `staging`;
  - `_recovery`;
- verify the internal manifest and root manifests are present.

### Phase 4 - Extract

Extract only into the selected disposable source root.

After extraction:

- verify internal manifest schema version;
- verify backup ID and recovery-contract SHA-256;
- verify every root file manifest deterministically;
- verify no unexpected files or missing files;
- record counts, bytes, and manifest hashes;
- preserve source-specific restore evidence separately.

## Vault verification

For both Google and USB restores:

```text
repository recognized by Git
branch: main
HEAD: fc4b03397d770f9b4a8a2f5e7f71e33981bcd181
working tree: clean
remote: none
```

Verify exact canonical hashes:

```text
projects/kaizen-platform/current-state.md:
5dc7608d83b91791fa1930597db6bb1a727eb3021ccc9eb0b9b356d0d13a0e70

projects/kaizen-platform/handoffs/implement-governed-amendment-support.md:
62a79e2104bf9ca5daf0c1e96a32f1b01ebc1cec290e5ab4c76ccf8d357fe0be

_governance/promotion-log.jsonl:
e17cc9f74f40b4b66ffcad432bceff16d5a583a922228db734b737863e95328f
```

Also record governance-log line count and verify tracked-file integrity through Git.

## Platform verification

For both Google and USB restores:

```text
repository recognized by Git
branch: main
HEAD: 1b8be1d6d42d768587dddb2be8415fa24b670561
working tree: clean
remote: none
```

Verify:

- tracked-file integrity through Git;
- `.python-version` and `pyproject.toml` present;
- no source-machine `.venv` exists in restored payload;
- a new disposable Python 3.11 environment can be created outside the restored repository or beneath the disposable restore root;
- dependencies install from committed metadata using the accepted local/network posture;
- full accepted platform test suite passes;
- restored source is not modified by test execution beyond accepted disposable caches;
- any generated disposable caches are recorded and remain outside live roots.

If dependency installation requires network access not separately approved in the implementation gate, stop after structural/Git verification and return the exact limitation. Do not falsely claim the test reconstruction passed.

## Staging verification

For both Google and USB restores:

- verify complete staging manifest;
- verify all expected evidence files and hashes;
- verify both Packet 010F operations exist:

```text
kz-prom-01KTV6N2A4B6C8D0E2F4G6H8J0
kz-prom-01KTV6R4B6D8F0H2J4M6N8P0R2
```

- verify candidate and reviewed-diff hashes:

```text
62a79e2104bf9ca5daf0c1e96a32f1b01ebc1cec290e5ab4c76ccf8d357fe0be
be013e4cbb4dfad67767c15915e1fd28eb019348c0e6872f60f893f3303096bf
5dc7608d83b91791fa1930597db6bb1a727eb3021ccc9eb0b9b356d0d13a0e70
4087296d345d283c7145d5a722bc26d7e3e162553a4c5fa23e13937fbc68eff9
```

- inspect evidence read-only;
- do not execute or recover any governed operation;
- do not write new staging evidence.

## Docs recovery verification

Clone the existing authorized repository into the disposable docs-clone root:

```text
https://github.com/St3nch/kaizen-docs.git
```

Verify:

- clone succeeds;
- branch `main`;
- expected current accepted docs commit at Packet 011D execution time;
- accepted Roadmap v0.3 SHA-256:
  `5feec5bef8bf48c30be36fddfe1547f47810145407d1e9cc47301e0c6dddc248`;
- Results 102, 117, 118, and 119 exist;
- repository clean;
- only the clone's normal `origin` exists;
- no new remote is created elsewhere.

## Required failure injections

Run only on disposable copies beneath the `failures` root.

### FI-01 - Encrypted object hash mismatch

Modify or substitute a disposable encrypted copy.

Expected:

```text
stop before decryption
no extraction
clear hash-mismatch result
```

### FI-02 - Wrong or unavailable identity

Use a disposable wrong identity or unavailable identity path.

Expected:

```text
decryption fails closed
no plaintext archive accepted
no secret output
```

### FI-03 - Truncated encrypted object

Create a truncated disposable encrypted copy.

Expected:

```text
decryption or integrity failure
no accepted archive
```

### FI-04 - Nonempty restore destination

Prepopulate the disposable target.

Expected:

```text
stop before decryption or extraction
no overwrite
```

### FI-05 - Unsafe archive path

Use a synthetic disposable test archive containing an absolute or traversal path.

Expected:

```text
path-safety rejection before extraction
```

Do not modify the accepted recovery archive to create this fixture.

### FI-06 - Manifest/source commit mismatch

Alter only a disposable internal manifest copy after extraction.

Expected:

```text
verification failure
restore not accepted
```

### FI-07 - Missing vault Git object or ref

Remove one Git object or ref only from a disposable restored vault copy.

Expected:

```text
Git integrity or expected-HEAD verification fails
```

### FI-08 - Missing staging evidence file

Remove one evidence file only from a disposable restored staging copy.

Expected:

```text
manifest/evidence verification fails
```

### FI-09 - Docs-only recovery falsely presented as complete

Verify docs clone succeeds while the encrypted recovery object is unavailable in a disposable scenario.

Expected:

```text
overall Kaizen recovery fails
no claim that docs alone recover vault/platform/staging
```

### FI-10 - Same-machine-only source

Use only the retained local encrypted source while marking both off-machine copies unavailable.

Expected:

```text
o off-machine recovery acceptance
```

## Evidence outputs

Create source-specific, non-secret evidence beneath the disposable root:

```text
google-restore-result.json
usb-restore-result.json
docs-recovery-result.json
failure-injection-results.json
packet011d-summary.json
```

Evidence may include:

- backup ID;
- source class;
- encrypted and plaintext archive hashes and sizes;
- restore destination class;
- manifest counts and hashes;
- Git branches, HEADs, clean state, and remotes;
- canonical file hashes;
- staging evidence hashes;
- test result counts;
- failure-injection result codes;
- timestamps;
- tool versions;
- known limitations.

Evidence must not include:

- private identity content;
- passphrase;
- Bitwarden data;
- exact sensitive physical location;
- secret values;
- raw environment dumps.

## Cleanup boundary

Packet 011D implementation does not automatically authorize deletion of:

- disposable restore roots;
- decrypted plaintext archives;
- generated test environments;
- failure fixtures;
- Google download source;
- local encrypted source;
- Google or USB copies;
- encrypted identity.

After evidence review, the owner may separately authorize exact disposable cleanup paths.

## Required implementation controls

- one bounded phase at a time;
- no giant combined command;
- exact literal paths;
- no arbitrary filesystem tool exposure;
- no live-root write access;
- no symlink or reparse-point escape;
- no overwrite;
- no secret-bearing command arguments;
- no command transcript containing passphrases;
- exact source/destination hash verification;
- stop on first mismatch;
- preserve partial evidence safely;
- no implementation work outside Packet 011D.

## Required validation of restore procedure

Before live restore:

- PowerShell parser passes;
- static scan finds no network upload, remote creation, live-root mutation, generic deletion, or secret logging;
- disposable fixture tests cover path safety, no-overwrite, hash mismatch, manifest mismatch, and redaction;
- exact changed-path review passes;
- local Go8 commit only if scripts are added;
- no Go8 MCP tool registry change.

## Acceptance criteria

Packet 011D is complete only when:

1. exact owner approval exists;
2. Google encrypted source independently verifies;
3. USB encrypted source independently verifies;
4. both sources decrypt independently;
5. both plaintext archive hashes match;
6. both archives pass path-safety checks;
7. both restores pass complete manifest verification;
8. both restored vault repositories match branch, HEAD, clean state, remote posture, and canonical hashes;
9. both restored platform repositories match branch, HEAD, clean state, remote posture, and accepted test/reconstruction evidence;
10. both restored staging sets verify and support read-only inspection of the two Packet 010F operations;
11. docs recovery from the existing upstream succeeds;
12. all ten failure injections fail safely;
13. no secret is disclosed;
14. no live root changes;
15. no retained encrypted copy or identity is deleted;
16. exact evidence return and completion audit pass;
17. second-generation retention remains explicitly open for the next closure packet.

## Stop gates

Stop when:

- source hash or size differs;
- restore destination is not empty;
- source or destination path could touch a live root;
- identity handling would expose a secret;
- archive path safety cannot be proven;
- manifest verification differs;
- restored Git state differs;
- staging evidence differs;
- platform test reconstruction requires unapproved network or system mutation;
- a failure injection would touch accepted recovery bytes or live roots;
- cleanup is requested without separate authority;
- scope drifts into second-generation creation, closure, or Milestone 8.

## Explicit non-scope

- live-root replacement or recovery;
- deletion of disposable or retained files;
- second backup generation;
- Packet 011E or Milestone 7 closure;
- platform or vault remote creation or push;
- Google Drive API integration;
- USB administration;
- identity rotation;
- automatic scheduling or retention;
- Milestone 8 reliability fixes;
- mock-project execution;
- Postgres, Qdrant, Hermes, or production MCP work.

## Required implementation return

Return:

- exact restore procedure commit and changed paths;
- exact source-copy hashes and sizes;
- Google and USB restore evidence;
- archive safety results;
- vault/platform/staging verification;
- docs clone verification;
- platform environment/test reconstruction result;
- all failure-injection results;
- zero-live-mutation proof;
- retained/decrypted/disposable path status;
- cleanup paths requiring separate approval;
- known limitations;
- second-generation requirement status;
- exact Packet 011E prerequisite state.
