---
id: kz-spec-01KTV8R6N8Q0S2W4Y6Z8ABCDEF
type: spec
status: draft
project: kaizen-platform
created: 2026-06-10T15:50:00-04:00
updated: 2026-06-10T15:50:00-04:00
review_status: pending-owner-decision
authority: proposed
summary: "Proposed Milestone 7 recovery-set manifest, restore, retention, and failure contracts selected by Packet 011A."
---

# Milestone 7 Recovery-Set Contract

## Status

Proposed contract produced by Packet 011A. It authorizes no backup creation, encryption, transfer, restore, deletion, remote creation, or push.

The contract becomes implementation authority only after:

1. exact owner posture selection;
2. security/steward audit;
3. exact owner acceptance of this file's SHA-256;
4. separate approval of Packet 011B.

## Recovery-set model

One backup generation is one logically atomic recovery set containing:

```text
vault repository and working tree
platform repository and working tree
staging immutable governed-operation evidence
internal full manifest
restore instructions
```

The generation is packaged into one portable archive, encrypted locally before transfer, and copied unchanged to each approved destination.

The encrypted object is not canonical authority. It is a recovery artifact bound to the canonical source state by exact manifests and hashes.

## Selected planning posture

Recommended for owner acceptance:

```text
primary storage: owner-selected private cloud object
secondary storage: removable media stored separately from the laptop
encryption: age recipient encryption
identity: age identity file protected separately
platform Git remote: none
vault Git remote: none
minimum retained verified generations: 2
automated deletion: prohibited
```

Fields requiring owner-specific completion before Packet 011B:

```text
cloud_provider:
cloud_account_or_folder_class:
removable_medium_class:
removable_medium_storage_location_class:
password_manager:
offline_recovery_location_class:
```

No secret or exact sensitive physical location may appear in the repository copy of this contract.

## Backup ID

Format:

```text
kz-backup-YYYYMMDDTHHMMSSZ-<12 lowercase hex>
```

Rules:

- timestamp is UTC;
- suffix is derived from cryptographically secure random bytes during implementation;
- ID is unique;
- ID is not reused after a failed generation;
- failed generation IDs remain recorded with terminal status.

## Source-root identities

Stable logical IDs:

```text
kz-root-vault
kz-root-platform
kz-root-staging
```

Source absolute paths are capture-time metadata and must not become restore requirements.

## Internal full manifest

Proposed filename:

```text
kaizen-recovery-manifest.json
```

The internal manifest is inside the encrypted archive and may contain non-secret exact relative path inventories.

Required top-level fields:

```json
{
  "schema_version": "kz-recovery-manifest-v1",
  "backup_id": "...",
  "created_at_utc": "...",
  "created_by": "owner.local",
  "source_machine_id": "non-secret stable label",
  "recovery_contract_sha256": "...",
  "protected_roots": [],
  "staging_operation_summary": {},
  "exclusions": [],
  "secret_handling_summary": {},
  "archive": {},
  "encryption": {},
  "storage": {},
  "retention": {},
  "previous_backup_id": null,
  "restore_contract_version": "kz-restore-contract-v1"
}
```

### Protected-root entry

```json
{
  "root_id": "kz-root-vault",
  "source_path_class": "kaizen-vault",
  "repository": true,
  "branch": "main",
  "head_commit": "40 lowercase hex",
  "clean_state": true,
  "remote_posture": "none",
  "tracked_file_count": 0,
  "tracked_byte_count": 0,
  "working_tree_file_count": 0,
  "working_tree_byte_count": 0,
  "git_bundle_or_repository_mode": "full-repository-copy",
  "content_manifest_path": "manifests/kz-root-vault.files.jsonl",
  "content_manifest_sha256": "64 lowercase hex",
  "critical_hashes": {}
}
```

### Content manifest

One JSON object per line, sorted by normalized relative path:

```json
{"path":"relative/path","kind":"file","size_bytes":123,"sha256":"..."}
```

Rules:

- relative paths only;
- UTF-8 JSONL;
- deterministic lexical path order;
- directories may be omitted unless empty-directory preservation is required;
- symlinks or special files must be represented explicitly if encountered;
- no secret value or file content enters the manifest;
- Git internals are verified separately by repository checks and archive coverage.

## Public verification companion

Proposed filename:

```text
<backup-id>.verify.json
```

Stored beside each encrypted object. It contains only non-sensitive verification metadata:

```json
{
  "schema_version": "kz-recovery-verify-v1",
  "backup_id": "...",
  "created_at_utc": "...",
  "encrypted_object_filename": "...",
  "encrypted_object_size_bytes": 0,
  "encrypted_object_sha256": "...",
  "encryption_format": "age-v1",
  "recovery_contract_sha256": "...",
  "storage_copy": "primary-cloud|secondary-removable",
  "copy_verified_at_utc": "..."
}
```

It must not contain:

- full path inventory;
- usernames beyond approved non-secret actor label;
- secret values;
- identity data;
- cloud credentials;
- exact sensitive physical location;
- canonical document contents.

Both copies must have the same encrypted-object SHA-256.

## Archive contract

Packet 011B must select and freeze one portable archive format before implementation.

Required properties:

- preserves relative paths and full Git repositories;
- deterministic inclusion list;
- supports long Windows paths used by the protected roots;
- no encryption responsibility inside the archive layer when `age` is selected;
- archive can be verified after decryption;
- implementation records exact tool name and version;
- archive excludes only accepted classified paths.

Recommended first implementation evaluation:

```text
portable tar archive generated from a frozen inclusion manifest
```

A 7z container without built-in encryption is an acceptable alternative only if path, metadata, and restore behavior are proven. The archive and encryption layers remain conceptually separate.

## Encryption contract

Recommended format:

```text
age v1 recipient encryption
```

Required rules:

- encryption occurs locally before transfer;
- encrypt to the owner-approved public recipient;
- private identity never enters the archive;
- identity path and passphrase never enter command history, logs, manifests, or repository files;
- implementation must avoid overwriting an existing output;
- output is hashed after encryption;
- decryption proof uses the owner-approved recovery path;
- exact age version and binary hash or trusted installation source are recorded;
- any generated identity requires separate owner handling and evidence without exposing the key.

Key custody contract:

- primary identity is recoverable independently of the source laptop;
- passphrase is stored in the owner-approved password manager;
- one offline recovery record is stored separately from the laptop and removable backup copy;
- the public recipient may be recorded in non-secret recovery documentation;
- loss of one device or one account must not lose both data and key.

## Secrets treatment contract

General archive rule:

```text
plaintext secrets are excluded unless a separately accepted secrets procedure explicitly includes them
```

Required redaction-safe preflight fields:

```text
finding_id
rule_id
redacted_path_class
line_number_or_filename_only
classification
recovery_requirement
selected_treatment
```

Allowed treatments:

```text
exclude
include redacted template
manual recreation checklist
separate encrypted-secrets mechanism
false positive
```

No matched secret value may be emitted.

## Staging inventory prerequisite

Packet 011B cannot begin until a narrow read-only capability returns:

- staging-relative paths;
- file sizes;
- SHA-256 hashes;
- operation IDs;
- evidence-class labels;
- total counts and bytes;
- no file contents by default;
- no mutation.

The inventory must explicitly include the two Packet 010F operations:

```text
kz-prom-01KTV6N2A4B6C8D0E2F4G6H8J0
kz-prom-01KTV6R4B6D8F0H2J4M6N8P0R2
```

No staging exclusion is accepted until this inventory is reviewed.

## Capture preflight contract

Before creating a generation:

1. verify accepted Packet 011B SHA-256;
2. verify vault and platform branches, full HEADs, cleanliness, and remote posture;
3. verify docs current branch/upstream for recovery reference;
4. run bounded staging inventory;
5. run redaction-safe secret classification;
6. freeze inclusion and exclusion manifests;
7. verify output workspace is outside live roots and empty;
8. verify approved storage destinations are reachable without copying data yet;
9. verify encryption public recipient and owner recovery path;
10. record source timestamp and backup ID.

Any mismatch stops before archive creation.

## Capture atomicity contract

A generation passes only when:

1. one frozen inclusion set binds all protected roots;
2. archive creation completes;
3. archive hash verifies;
4. encryption completes to a new file;
5. encrypted-object hash verifies;
6. primary copy transfer completes and remote/object hash or byte verification passes;
7. secondary copy transfer completes and verification passes;
8. public verification companions match the encrypted object;
9. local temporary plaintext archive is securely handled according to the accepted task packet;
10. generation result evidence is written.

Packet 011B must define safe handling of the temporary plaintext archive. This contract does not authorize deletion; deletion requires explicit implementation-packet scope and verified encrypted copies.

## Restore contract

Version:

```text
kz-restore-contract-v1
```

### Preflight

- use a new empty disposable destination outside all live roots;
- verify it contains no existing files;
- record source recovery copy selected;
- verify companion manifest and encrypted-object SHA-256;
- obtain identity through the owner-approved path without logging it;
- do not access or modify live roots.

### Decryption and extraction

1. decrypt to a new temporary archive path;
2. verify decrypted archive SHA-256 against internal expected metadata where available;
3. inspect archive paths for traversal, absolute paths, or unsafe entries before extraction;
4. extract only into the disposable destination;
5. load and validate the internal manifest;
6. verify all content manifests and root inventories;
7. retain evidence of every check.

### Vault verification

- repository recognized by Git;
- branch and full HEAD match manifest;
- no unexpected remote;
- working tree clean;
- tracked file verification passes;
- governance-log SHA-256 and line count match;
- current-state and implementation-return hashes match.

### Platform verification

- repository recognized by Git;
- branch and full HEAD match manifest;
- no unexpected remote;
- working tree clean;
- tracked file verification passes;
- Python version contract can be satisfied;
- environment reconstructed from committed metadata;
- accepted verification test profile passes without source-machine `.venv`.

### Staging verification

- complete evidence manifest matches;
- required operation directories exist;
- plans, approvals, results, diffs, and preserved bytes are hash-valid;
- both Packet 010F operations can be inspected read-only;
- no canonical mutation is executed.

### Docs verification

- clone from the existing authorized upstream into the disposable root;
- verify expected branch and current accepted commit;
- verify accepted Roadmap v0.3 and Result 102/104/106 evidence as applicable;
- no new remote is created beyond the clone's existing origin.

### Terminal result

Restore result is one of:

```text
verified
failed-integrity
failed-key-access
failed-archive
failed-path-safety
failed-git
failed-staging-evidence
failed-docs-recovery
failed-test-reconstruction
```

Partial success is not represented as verified.

## Required failure injections

| Injection | Expected result |
|---|---|
| Encrypted object hash mismatch | stop before decryption |
| Wrong or unavailable identity | `failed-key-access`; no fallback bypass |
| Truncated encrypted object/archive | deterministic integrity failure |
| Missing vault Git object/ref | `failed-git` |
| Missing staging evidence file | `failed-staging-evidence` |
| Manifest/source commit mismatch | stop before acceptance |
| Nonempty restore destination | stop before extraction |
| Unsafe archive path traversal | `failed-path-safety` |
| Secret finding lacking accepted treatment | stop before capture |
| Same-machine-only copy | generation cannot be accepted as off-machine |
| Docs clone succeeds but recovery object unavailable | overall restore fails |
| One destination hash differs | generation incomplete; no acceptance |

All injections run only on disposable test copies or fixtures.

## Retention contract

Initial proof:

```text
minimum verified generations: 2
generation type: full baseline or accepted full successor
```

Recommended cadence after closure:

```text
trigger: milestone closure or consequential canonical/platform change
maximum target age: 30 days
```

Rules:

- owner approves every deletion;
- no generation is deleted before a successor restores successfully;
- failed generations are retained as evidence until reviewed;
- primary and secondary copy state is recorded separately;
- provider-side versioning supplements but does not replace Kaizen generation records;
- automated deletion is prohibited;
- retention changes require contract amendment.

## Evidence contract

Each generation must return:

- backup ID;
- source root states;
- inclusion/exclusion manifest hashes;
- secret classification summary;
- archive format/tool/version;
- archive hash;
- encryption format/tool/version;
- public recipient fingerprint or non-secret identifier;
- encrypted-object hash and size;
- primary and secondary copy verification;
- restore proof result;
- failure-injection results;
- retention state;
- known limitations;
- exact owner acceptance gate.

## Explicit exclusions

- database backup;
- Qdrant snapshots;
- MCP packaging;
- broad cloud administration;
- continuous synchronization;
- automated scheduler or deletion;
- platform or vault remotes;
- live-root destructive testing;
- secrets-manager migration;
- controlled pilot work.
