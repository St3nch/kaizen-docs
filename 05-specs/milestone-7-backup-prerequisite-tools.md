---
id: kz-spec-01KTV8Y4N6Q8S0W2Y4Z6ABCDEF
type: spec
status: draft
project: kaizen-platform
created: 2026-06-10T16:16:00-04:00
updated: 2026-06-10T16:16:00-04:00
review_status: pending-review
authority: proposed
summary: "Proposed narrow read-only staging inventory and redaction-safe secret-classifier prerequisites for Milestone 7 backup creation."
---

# Milestone 7 Backup Prerequisite Tools

## Purpose

Define the two smallest missing read-only capabilities required before Packet 011B may create any backup bytes:

1. a bounded staging evidence inventory;
2. a redaction-safe secret classifier.

This specification authorizes no implementation, backup creation, key generation, archive creation, encryption, upload, restore, deletion, remote creation, or push.

## Placement recommendation

Default placement:

```text
C:\dev\chatgpt-mcp
```

Reason:

- Go8 already provides fixed-root read-only repository and integrity tools;
- the prerequisites are operator inspection tools, not Kaizen canonical mutation semantics;
- placing them in `kaizen-platform` would mix backup-operator inspection with governed project-intelligence runtime logic;
- placing them in temporary `kaizen-mcp` would expand a proving ground that remains deferred to Milestone 8;
- Go8 is already the constrained ChatGPT development interface and can expose an additional fixed staging root without granting arbitrary path access.

This recommendation remains proposed until exact packet approval.

## Required root addition

Add one fixed root:

```text
staging -> C:\dev\kaizen\staging
```

Rules:

- root is read-only for both new tools;
- no generic file read tool is automatically enabled for staging;
- no write, replace, delete, move, Git, process, command, Python, or archive capability is granted for staging;
- staging cannot be selected through arbitrary absolute paths;
- path traversal and reparse-point escape remain prohibited.

## Tool 1 - `staging_evidence_inventory`

### Objective

Return a deterministic bounded inventory of governed staging evidence without returning file contents.

### Input

```json
{
  "operation_ids": ["optional exact operation IDs"],
  "include_all_operations": false,
  "include_hashes": true,
  "max_items": 5000
}
```

### Input rules

- at least one exact operation ID is required unless `include_all_operations` is explicitly true;
- operation IDs must pass the accepted Kaizen ID grammar;
- `include_all_operations` requires an explicit bounded maximum;
- no caller-supplied path is accepted;
- no glob, regex, shell expression, or relative traversal is accepted.

### Output

```json
{
  "root_id": "staging",
  "operation_count": 0,
  "file_count": 0,
  "byte_count": 0,
  "operations": [
    {
      "operation_id": "...",
      "relative_path": "...",
      "file_count": 0,
      "byte_count": 0,
      "evidence_classes": {},
      "files": [
        {
          "relative_path": "...",
          "evidence_class": "plan|approval|result|validation|reviewed-diff|prior-bytes|candidate-bytes|other",
          "size_bytes": 0,
          "sha256": "64 lowercase hex"
        }
      ]
    }
  ],
  "truncated": false,
  "findings": []
}
```

### Evidence classification

Classification uses exact accepted filenames and directory placement where available. Unknown files are returned as:

```text
other
```

Unknown evidence is never silently excluded.

### Safety properties

- read-only;
- no content return;
- SHA-256 and metadata only;
- bounded output;
- fixed root;
- deterministic sorted paths;
- hidden and binary files may be hashed but not rendered;
- symlinks, junctions, and reparse points are reported and not followed outside root;
- unreadable files produce structured findings rather than partial success;
- no archive or backup file is created.

### Required tests

- exact inventory of one valid operation;
- inventory of both Packet 010F operations;
- all-operations bounded inventory;
- nonexistent operation;
- malformed operation ID;
- traversal attempt;
- reparse-point escape;
- unreadable file;
- unknown evidence class;
- output truncation;
- deterministic ordering;
- zero file-content leakage;
- zero mutation.

## Tool 2 - `classify_secret_exposure`

### Objective

Inspect exact fixed-root file paths or bounded manifests for likely secret exposure while never returning matched values.

### Allowed roots

```text
docs
platform
vault
staging
```

Default Packet 011B scope:

```text
platform
vault
staging
```

### Input

```json
{
  "root": "platform|vault|staging|docs",
  "paths": ["explicit relative paths"],
  "manifest_scope": false,
  "rule_profile": "backup-preflight-v1",
  "max_files": 5000,
  "max_bytes_per_file": 2000000
}
```

### Input rules

- explicit paths are preferred;
- `manifest_scope` may inspect only the bounded fixed-root manifest produced by accepted inventory logic;
- no arbitrary absolute path;
- no regex supplied by caller;
- no caller-controlled secret pattern;
- no binary decoding attempt beyond safe classification;
- no archive extraction.

### Rule profile

`backup-preflight-v1` includes bounded filename and content-shape rules for likely:

- `.env` files;
- private keys;
- PEM blocks;
- SSH identities;
- API tokens;
- bearer tokens;
- passwords assigned in common configuration formats;
- cloud credentials;
- ngrok credentials;
- session cookies;
- known secret-bearing filename classes.

The exact rules are versioned and testable.

### Output

```json
{
  "root": "...",
  "rule_profile": "backup-preflight-v1",
  "files_scanned": 0,
  "bytes_scanned": 0,
  "findings": [
    {
      "finding_id": "...",
      "relative_path": "...",
      "rule_id": "...",
      "line_number": 0,
      "finding_class": "filename|content-shape|binary|unreadable",
      "confidence": "high|medium|low",
      "matched_value_returned": false,
      "recommended_treatment": "exclude|template-only|manual-recreation|separate-secret-mechanism|review"
    }
  ],
  "truncated": false
}
```

### Redaction contract

The tool must never return:

- matched text;
- surrounding lines;
- captured groups;
- token prefix or suffix;
- entropy sample;
- private-key bytes;
- decoded secret values;
- environment variable values.

Only path, rule ID, line number, class, confidence, and proposed treatment may be returned.

Logs and exceptions follow the same rule.

### Safety properties

- read-only;
- fixed roots;
- explicit bounded paths;
- no secret value in normal output, error output, logs, tests, snapshots, or debug traces;
- no network calls;
- no password-manager access;
- no environment-variable dump;
- no subprocess or shell;
- no mutation or quarantine;
- binary files classified without rendering bytes.

### Required tests

- known fake token fixture produces finding without value;
- PEM fixture produces rule ID and line only;
- `.env` filename finding without reading value into output;
- false positive fixture;
- binary fixture;
- oversized file;
- unreadable file;
- traversal attempt;
- staging-root scan;
- exception message redaction;
- logs contain no fixture secret;
- serialized output contains no fixture secret;
- caller cannot supply patterns;
- zero mutation.

## Consequence classification

Both tools are:

```text
read-only inspection
```

They are not:

- backup tools;
- secrets managers;
- validators that approve inclusion automatically;
- archive creators;
- mutation tools;
- generalized file browsers.

A finding does not automatically exclude a file. The owner or accepted packet selects treatment.

## Packet 011B dependency

Packet 011B backup implementation cannot begin until:

1. both tools are implemented under a separately approved prerequisite packet;
2. focused and full Go8 tests pass;
3. the live staging inventory covers both Packet 010F operations;
4. redaction-safe classification is run against the exact accepted inclusion set;
5. findings and exclusions are reviewed;
6. no secret value appears in returned evidence;
7. the Go8 tool registry and version are updated and verified;
8. no unrelated Go8 authority is widened.

## Exclusions

- no backup creation;
- no `age` installation or execution;
- no key generation;
- no Google Drive integration;
- no USB discovery or write;
- no restore root creation;
- no platform or vault remote;
- no Kaizen MCP change;
- no Milestone 8 reliability work;
- no generic staging read tool;
- no generic secret search.
