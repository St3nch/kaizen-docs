---
id: kz-tp-01KTVA78N0Q2S4W6Y8Z0ABCDEF
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-10T17:18:00-04:00
updated: 2026-06-10T17:18:00-04:00
review_status: pending-review
authority: proposed
summary: "Packet 011C - create, encrypt, verify, and transfer one Kaizen recovery generation to private Google Drive and a separately stored USB SSD."
primary_spec: kz-spec-01KTV8R6N8Q0S2W4Y6Z8ABCDEF
---

# Task Packet 011C - Create and Transfer Encrypted Recovery Generation

> Non-authoritative draft. No implementation is authorized until the owner accepts the exact audited Packet 011C SHA-256.

## Objective

Create one complete, portable, encrypted Kaizen recovery generation containing:

```text
C:\dev\kaizen\vault
C:\dev\kaizen\platform
C:\dev\kaizen\staging
internal recovery manifest
restore instructions
```

Then copy the exact encrypted object unchanged to:

```text
primary: private Google Drive storage
secondary: owner-selected small USB SSD stored separately from the laptop
```

Packet 011C proves capture, encryption, and transfer integrity only. Disposable restore and destructive failure injections remain Packet 011D.

## Bound authority

```text
Milestone 7 specification SHA-256:
a5d9cac899fd82888201113bbc2e340030be10c6184cfa036cffcd40ff4294eb

Recovery-set contract SHA-256:
8e092ac761e60687bf8d3b8c78bdb83b7f6d68c58b47024a0ee01be987d3956b

Packet 011A inventory/result SHA-256:
ad8d0a567d7b19cb8c3053af8e335d6b7f52fb360d16a0289dc1ad72a6a5d11a

Packet 011B implementation return SHA-256:
db4d340ef787ec03b966c04d4a7fc699028ac19d6c560f4d94633dd7ccd52ab3

Packet 011B completion audit SHA-256:
202d89a0e7069f6f8e3b53a123ffd2e40eb28e040f8fc92fcd3629e5cb0f7ea0

Owner posture selection:
Result 109

Packet 011B owner completion acceptance:
Result 113
```

## Selected owner posture

```text
primary storage: private Google Drive
secondary storage: small USB SSD stored separately from source laptop
recovery object: identical encrypted portable archive at both destinations
encryption: age v1 recipient encryption
passphrase custody: Bitwarden
offline recovery record: separate encrypted USB, not colocated with laptop or backup SSD
retention: minimum 2 verified generations
maximum target backup age: 30 days
platform remote: none
vault remote: none
deletion authority: owner only after successor restore proof
restore root class: C:\dev\kaizen-restore-proof\<backup-id>
```

## Packet outcome

One Packet 011C generation is successful only when:

1. the exact source state is frozen and recorded;
2. the accepted inclusion and exclusion sets are generated;
3. the internal manifest is complete and self-consistent;
4. one portable archive is created outside live roots;
5. the archive hash verifies;
6. the archive is encrypted locally with `age`;
7. the encrypted-object hash verifies;
8. the identical encrypted object reaches private Google Drive;
9. the identical encrypted object reaches the selected USB SSD;
10. both destination copies independently match the source encrypted-object SHA-256 and byte size;
11. non-sensitive verification companions are present at both destinations;
12. no live root, remote, canonical file, or governed evidence is mutated;
13. temporary plaintext handling follows the exact owner-approved cleanup boundary;
14. Packet 011D remains separately gated.

## Repository and tool placement

No new repository is created.

### Documentation

```text
C:\dev\kaizen-docs
```

### Source roots

```text
C:\dev\kaizen\vault
C:\dev\kaizen\platform
C:\dev\kaizen\staging
```

### Temporary generation workspace

Recommended class:

```text
C:\dev\kaizen-backup-work\<backup-id>
```

The exact path must be empty, outside every live Kaizen root, and owner-approved in the implementation gate.

### Operator implementation

Prefer one narrow, reviewable local script or typed operator procedure under:

```text
C:\dev\chatgpt-mcp
```

It may orchestrate only the approved capture, manifest, archive, hash, encryption, and local copy steps. It must not expose generic archive, cloud, USB, shell, deletion, or credential-management tools through MCP.

An owner-run exact PowerShell procedure is acceptable when it is safer and simpler than adding persistent Go8 authority.

## Pre-implementation owner confirmations

Before Packet 011C implementation begins, the owner must confirm:

- Google Drive transfer method:
  - manual browser upload; or
  - Google Drive for desktop into an exact private folder;
- exact non-sensitive Google Drive folder class;
- exact USB SSD drive letter or mounted path for that session;
- USB SSD has sufficient free space and is not the offline recovery-key USB;
- `age` installation method;
- whether an age identity already exists or must be generated;
- exact Bitwarden item class used for the identity passphrase or recovery note;
- offline recovery-record creation method;
- whether temporary plaintext archive deletion is authorized after both encrypted copies verify;
- exact local temporary workspace;
- exact operator actor label.

No secret, Bitwarden content, private identity bytes, or sensitive physical location may be placed in the packet, chat, repository, logs, or manifest.

## Required implementation phases

## Phase 1 - Verify predecessor state

Verify independently:

```text
kaizen-docs:
branch main
clean
upstream origin/main synchronized

chatgpt-mcp:
HEAD a64d0164857d56eedeb1d36a31e16268a5c51791
clean
remote none
Go8 v0.4.0
44 tools

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

Run the accepted Packet 011B tools:

- exact staging inventory;
- redaction-safe platform inclusion scan;
- redaction-safe vault inclusion scan;
- redaction-safe staging inclusion scan.

Any new secret finding, dirty state, changed root commit, missing evidence, or unexpected remote stops before creating a backup ID.

## Phase 2 - Establish trusted local tools

### `age`

Requirements:

- obtain from an owner-approved official or trusted package source;
- record exact version;
- record binary path;
- record binary SHA-256 when practical;
- verify executable help/version locally;
- do not download through an unreviewed script pipe;
- do not install a broad package manager or persistent service solely for this packet unless separately approved.

### Archive tool

Preferred first implementation:

```text
Windows tar / bsdtar producing a portable tar archive
```

Requirements:

- record exact tool and version;
- verify long-path and Git-directory preservation;
- no built-in archive encryption when `age` is selected;
- no caller-controlled arbitrary source path beyond the frozen inclusion manifest;
- reject unsafe absolute or traversal paths.

If tar behavior cannot preserve the required repository bytes exactly, stop and revise the packet rather than silently switching formats.

## Phase 3 - Establish age identity and recovery custody

### Identity generation

When no suitable owner identity exists:

- generate one age identity locally through the exact reviewed command;
- never print the private identity into chat or documentation;
- do not commit it;
- do not place it inside the recovery archive;
- protect it according to the owner-approved age/Bitwarden method;
- record only the public recipient and a non-secret fingerprint or identifier;
- verify one local disposable encrypt/decrypt fixture before project archive use.

### Bitwarden custody

The owner performs Bitwarden entry or update manually.

Evidence records only:

```text
Bitwarden custody confirmed: yes/no
item class: non-sensitive description
confirmed by: owner
confirmed at: timestamp
```

No Bitwarden secret, master password, item contents, session token, or recovery code is requested or recorded.

### Offline recovery record

The owner creates or updates the separate encrypted recovery USB manually.

Evidence records only confirmation that:

- it exists;
- it is not the backup SSD;
- it is not colocated with the laptop or backup SSD;
- it contains sufficient recovery instructions or identity material;
- a disposable identity-access check succeeded without exposing contents.

Packet 011C does not reveal or document the physical location.

## Phase 4 - Freeze the recovery generation

Generate one backup ID using the accepted format:

```text
kz-backup-YYYYMMDDTHHMMSSZ-<12 lowercase hex>
```

Create a new empty temporary workspace.

Freeze:

- protected source root identities;
- source absolute path classes;
- branches and full HEAD commits;
- remote posture;
- clean state;
- exact inclusion paths;
- exact exclusions and reasons;
- staging inventory;
- secret-classification dispositions;
- tool versions;
- recovery contract SHA-256;
- previous backup ID or null.

The two platform test-fixture findings are recorded as reviewed false positives and included.

No new exclusion may be introduced without owner review.

## Phase 5 - Build deterministic content manifests

For each protected root, create one sorted JSONL file manifest containing:

```text
relative path
kind
size in bytes
SHA-256
```

Requirements:

- relative paths only;
- deterministic lexical order;
- UTF-8;
- no secret contents;
- no ignored/rebuildable files unless explicitly included;
- complete Git repository coverage for platform and vault;
- complete accepted staging evidence coverage;
- exact counts and byte totals;
- manifest SHA-256 recorded in the internal recovery manifest.

### Vault inclusion

Include:

- complete `.git/` repository data;
- all tracked canonical files;
- governance log;
- `.gitignore`;
- accepted non-ignored vault configuration, if any.

Exclude current ignored workspace, cache, trash, and OS metadata classes.

### Platform inclusion

Include:

- complete `.git/` repository data;
- tracked source, schemas, tests, docs, and configuration;
- exact committed environment-reconstruction metadata.

Exclude:

```text
.venv/
__pycache__/
.pytest_cache/
coverage output
build output
distribution output
editor caches
```

Tracked files remain included even when they resemble generated metadata.

### Staging inclusion

Include all accepted staging evidence discovered by the live bounded inventory unless the owner separately accepts an exact exclusion.

Unknown evidence classes remain included.

## Phase 6 - Build internal manifest and archive

Create:

```text
kaizen-recovery-manifest.json
restore-instructions.md
manifests/<root-id>.files.jsonl
```

The internal manifest must satisfy the accepted recovery-set contract.

Then create one portable archive from the frozen inclusion set.

Rules:

- archive path is new and outside live roots;
- no overwrite;
- no live-root mutation;
- no archive encryption at this layer;
- path traversal and absolute-entry checks pass;
- archive contents are listed and compared against the frozen manifest;
- archive SHA-256 and byte size are recorded;
- archive is readable before encryption;
- a failed archive remains quarantined in the temporary workspace and is not transferred.

## Phase 7 - Encrypt with age

Encrypt the verified plaintext archive to a new file using the owner-approved public recipient.

Rules:

- no overwrite;
- identity/private key is not required for encryption;
- recipient is verified against the accepted non-secret identifier;
- no passphrase, private identity, or secret environment variable appears in process arguments or logs;
- output filename binds the backup ID and archive type;
- encrypted object SHA-256 and byte size are recorded;
- the encrypted file is read-only during transfer;
- one local decryption-integrity check may be performed only against a disposable output and without full restore semantics;
- full restore remains Packet 011D.

## Phase 8 - Create verification companions

Create one non-sensitive verification companion for each destination:

```text
<backup-id>.verify.json
```

It may contain only:

- schema version;
- backup ID;
- creation timestamp;
- encrypted filename;
- encrypted size;
- encrypted SHA-256;
- encryption format;
- recovery-contract SHA-256;
- destination class;
- copy verification timestamp;
- copy verification result.

It must not contain private paths, secret values, identity bytes, Google credentials, Bitwarden data, or physical location.

## Phase 9 - Transfer to private Google Drive

Preferred first-slice transfer:

```text
manual owner upload through the browser or owner-confirmed Google Drive desktop folder
```

Do not build Google Drive API integration.

Required checks:

- destination is private and owner-controlled;
- no public or link-wide sharing;
- exact encrypted object and matching verification companion are uploaded;
- local source encrypted SHA-256 and size are recorded;
- destination copy is re-downloaded or otherwise byte-verified through an exact owner-approved method;
- destination encrypted SHA-256 equals the local source hash;
- verification companion records success;
- screenshots may supplement but never replace byte/hash verification.

If Google Drive cannot provide independent byte verification without an unsafe workflow, stop and return the limitation before claiming success.

## Phase 10 - Transfer to USB SSD

Required preflight:

- owner confirms exact mounted path;
- device is the backup SSD, not the offline recovery USB;
- sufficient free space;
- destination directory is new or empty;
- no unrelated files are overwritten;
- filesystem supports the encrypted object size;
- device is not inside any live Kaizen root.

Copy the encrypted object and its verification companion.

Then:

- flush writes through the operating system;
- hash the destination encrypted object from the USB SSD;
- compare byte size and SHA-256 to the local source;
- record success;
- do not format, repartition, rename, or otherwise administer the device;
- do not delete prior generations unless separately authorized.

## Phase 11 - Temporary plaintext handling

The plaintext archive is more sensitive than the encrypted copies.

Default posture:

```text
retain only until:
- local archive verification passes;
- age encryption passes;
- Google Drive copy hash verifies;
- USB SSD copy hash verifies;
- Packet 011C evidence is reviewed.
```

Deletion of the exact temporary plaintext archive and any disposable local decryption fixture requires explicit owner authorization in the Packet 011C implementation approval.

No secure-erasure claim may be made for SSD storage. The packet may delete the exact temporary files and verify absence, but must not claim forensic erasure.

The encrypted local source object may be retained until Packet 011D restore proof completes unless the owner selects another exact retention posture.

## Required failure injections

Packet 011C implementation must use disposable fixtures or pre-transfer checks to prove:

1. dirty platform or vault stops before backup ID creation;
2. changed staging evidence stops before capture;
3. unresolved secret finding stops before archive creation;
4. nonempty temporary workspace stops;
5. unsafe archive path stops;
6. archive manifest mismatch stops;
7. wrong age recipient identifier stops;
8. encrypted object hash mismatch stops transfer acceptance;
9. Google Drive destination is not private;
10. Google Drive destination copy cannot be byte-verified;
11. USB path is missing or ambiguous;
12. USB free space is insufficient;
13. USB destination file already exists;
14. USB copy hash differs;
15. one destination succeeds while the other fails;
16. attempted use of the offline recovery USB as backup media stops;
17. attempt to delete plaintext before both copies verify stops.

No failure injection may modify live protected roots or destroy accepted backup evidence.

## Required tests and validation

When code is added:

- focused unit tests for manifest, inclusion, archive command construction, output naming, and verification companions;
- path-confinement and traversal tests;
- no-overwrite tests;
- secret-redaction tests;
- simulated Google/USB transfer verification using disposable directories;
- hash mismatch and partial-copy tests;
- deterministic manifest tests;
- full Go8 suite if Go8 code changes;
- Ruff and compilation;
- static capability scan;
- exact changed-path and diff verification.

When an owner-run procedure is used instead of code:

- every command is exact and separately reviewed;
- each phase stops for evidence review;
- no giant all-in-one command;
- no command includes secrets;
- output is captured into bounded non-secret evidence.

## Required implementation return

Return:

- backup ID;
- exact source repository states;
- staging inventory counts and hash summary;
- secret-classification summary and dispositions;
- tool versions and trusted sources;
- age public recipient identifier only;
- inclusion/exclusion manifest hashes;
- internal manifest SHA-256;
- plaintext archive SHA-256 and size;
- encrypted object SHA-256 and size;
- Google Drive destination class and independent hash verification;
- USB SSD destination class and independent hash verification;
- verification companion hashes;
- temporary plaintext status;
- local encrypted source status;
- failure-injection results;
- zero-live-mutation proof;
- known limitations;
- exact Packet 011D prerequisite state.

## Acceptance criteria

Packet 011C is complete only when:

1. predecessor evidence and repository states match;
2. owner-specific transfer and custody choices are confirmed;
3. trusted local tool versions are recorded;
4. age identity custody is confirmed without secret disclosure;
5. all protected roots are covered by deterministic manifests;
6. staging includes all accepted evidence;
7. reviewed fake-secret fixtures are included and no unresolved secret finding remains;
8. one portable archive is created and verified;
9. one age-encrypted object is created and verified;
10. Google Drive holds an independently verified identical encrypted object;
11. USB SSD holds an independently verified identical encrypted object;
12. verification companions match;
13. platform and vault remain clean, remote-less, and unchanged;
14. staging remains unchanged;
15. no public sharing or unauthorized remote is created;
16. plaintext temporary handling matches exact owner authority;
17. failure injections pass safely;
18. documentation return and completion audit pass;
19. Packet 011D receives separate owner approval before restore begins.

## Stop gates

Stop when:

- any accepted predecessor hash differs;
- protected repository state is dirty or changed;
- staging evidence differs unexpectedly;
- secret classification has unresolved findings;
- identity custody cannot survive source-machine loss;
- a command would expose a secret;
- Google Drive privacy cannot be verified;
- Google copy cannot be independently byte-verified;
- USB identity/path is ambiguous;
- any operation would overwrite unrelated data;
- archive format cannot preserve complete Git repositories;
- any destination hash differs;
- the packet would need API integration, broad cloud tooling, device administration, remote creation, or full restore;
- deletion authority is missing.

## Explicit non-scope

- disposable full restore;
- destructive restore failure testing;
- Packet 011D or 011E implementation;
- Google Drive API integration;
- background synchronization daemon;
- automated scheduling;
- automatic retention deletion;
- platform or vault Git remotes;
- platform or vault push;
- database or Qdrant backup;
- Kaizen MCP production migration;
- Milestone 8 work;
- mock-project execution;
- secrets-manager migration;
- full-device image backup.

## Owner implementation gate requirements

The audit must provide an exact acceptance phrase bound to:

- Packet 011C SHA-256;
- selected Google Drive transfer method;
- selected USB mounted-path class;
- selected age installation source and version policy;
- age identity generation/custody posture;
- exact temporary workspace class;
- explicit temporary plaintext deletion authority or retention;
- explicit local encrypted-source retention posture;
- exact actor label;
- confirmation that Packet 011D remains separate.
