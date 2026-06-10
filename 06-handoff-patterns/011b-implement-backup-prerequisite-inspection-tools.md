---
id: kz-tp-01KTV90AN2Q4S6W8Y0Z2ABCDEF
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-10T16:22:00-04:00
updated: 2026-06-10T16:22:00-04:00
review_status: pending-review
authority: proposed
summary: "Packet 011B - implement the narrow read-only staging evidence inventory and redaction-safe secret classifier required before backup creation."
primary_spec: kz-spec-01KTV8Y4N6Q8S0W2Y4Z6ABCDEF
---

# Task Packet 011B - Implement Backup-Prerequisite Inspection Tools

> Non-authoritative draft. Implementation requires exact owner approval of the audited packet SHA-256.

## Objective

Implement and prove two narrow read-only Go8 capabilities required before Kaizen may create a backup generation:

```text
staging_evidence_inventory
classify_secret_exposure
```

Packet 011B must not create backup bytes, encryption keys, archives, uploads, restores, remotes, pushes, or deletions.

## Why this packet exists

Packet 011A found two evidence gaps:

1. Go8 has no staging root, so immutable governed-operation evidence cannot yet be inventoried safely.
2. Existing text search returns matching content and is unsuitable for secret classification because it could disclose secret values.

These are security prerequisites, not Milestone 8 reliability work and not backup implementation.

## Bound authority

```text
Milestone 7 specification SHA-256:
a5d9cac899fd82888201113bbc2e340030be10c6184cfa036cffcd40ff4294eb

Recovery-set contract SHA-256:
8e092ac761e60687bf8d3b8c78bdb83b7f6d68c58b47024a0ee01be987d3956b

Packet 011A inventory/result SHA-256:
ad8d0a567d7b19cb8c3053af8e335d6b7f52fb360d16a0289dc1ad72a6a5d11a

Prerequisite-tools specification SHA-256:
a6b57edfd63f469468659416eb15897f8470f935f7bfea6c20bcb2e6c2f98dbb

Owner posture selection:
Result 109
```

## Repository placement

Implementation root:

```text
C:\dev\chatgpt-mcp
```

Reasons:

- Go8 already owns fixed-root read-only inspection tools;
- staging is a backup-operator inspection concern here, not canonical mutation logic;
- temporary Kaizen MCP remains deferred;
- kaizen-platform must not absorb ChatGPT connector tooling;
- no new repository is needed.

Documentation and audits return to:

```text
C:\dev\kaizen-docs
```

## Expected predecessor state

Verify independently before editing:

```text
chatgpt-mcp:
branch main
working tree clean
remote posture unchanged
service version and tool count recorded

kaizen-docs:
branch main
working tree clean
upstream origin/main synchronized

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

Any mismatch stops before implementation.

## Read first

- `05-specs/milestone-7-backup-prerequisite-tools.md`;
- `05-specs/milestone-7-recovery-set-contract.md`;
- Results 107 through 109;
- Go8 root configuration, path-confinement, manifest, search, integrity, and test implementations;
- current Go8 tool registry and version contract;
- staging directory structure read-only through an exact owner-run inspection only if necessary to implement fixtures, without exposing live secret values.

## Authorized implementation scope

### Root configuration

Add one fixed logical root:

```text
staging -> C:\dev\kaizen\staging
```

The root is available only to the two accepted tools unless a later packet separately approves more.

Do not automatically enable existing generic read, write, Git, process, Python, or search tools for staging.

### Tool 1

Implement exactly:

```text
staging_evidence_inventory
```

Contract:

- accepts exact operation IDs or an explicit bounded all-operations mode;
- returns relative paths, sizes, SHA-256 hashes, evidence classes, counts, and findings;
- returns no file contents;
- deterministic sorted output;
- rejects traversal and malformed IDs;
- does not follow reparse points outside staging;
- reports unknown evidence classes rather than excluding them;
- read-only and side-effect free.

### Tool 2

Implement exactly:

```text
classify_secret_exposure
```

Contract:

- fixed roots only;
- explicit paths or accepted bounded manifest scope;
- fixed rule profile `backup-preflight-v1`;
- returns path, rule ID, line number, class, confidence, and treatment only;
- never returns matched text, context, captured values, prefixes, suffixes, decoded bytes, environment values, or secrets in exceptions/logs;
- no caller-supplied regex or pattern;
- no network, shell, subprocess, password-manager, environment dump, mutation, or quarantine.

## Required implementation controls

- optimistic exact-file edits;
- typed input and output schemas;
- explicit maximum items, files, and bytes;
- stable error codes;
- no broad path parameters;
- no file-content return from staging inventory;
- no secret value in any serialized output;
- no secret value in test names, snapshots, logs, or assertion messages;
- fake test secrets only;
- capability comments and tool descriptions accurately mark both tools read-only;
- Go8 version increments according to existing policy;
- health/tool registry reflects the two new tools and staging root without claiming generic staging access.

## Required tests

### Staging inventory focused tests

- one operation;
- both Packet 010F operation IDs through disposable fixtures or safe live inspection;
- all-operations bounded mode;
- nonexistent and malformed IDs;
- traversal;
- reparse-point escape;
- unreadable file;
- unknown evidence class;
- truncation;
- deterministic ordering;
- no content leakage;
- no mutation.

### Secret-classifier focused tests

- fake token;
- fake private-key block;
- `.env` filename;
- false positive;
- binary file;
- oversized file;
- unreadable file;
- traversal;
- staging scope;
- exception redaction;
- log redaction;
- serialized output redaction;
- caller pattern rejection;
- no mutation.

### Integration and hammer tests

- repeated inventory calls yield identical results;
- repeated classification calls yield identical findings;
- concurrent read-only calls do not mutate or deadlock;
- file changes between manifest and scan produce explicit drift findings;
- tool registry contains exactly the expected additions;
- no existing root gains new unintended access;
- complete Go8 suite passes;
- static capability scan shows no new shell, subprocess, network, archive, upload, deletion, or credential access.

## Live verification

After disposable tests pass, run bounded live read-only verification:

1. inventory the two Packet 010F operation IDs;
2. record counts, sizes, hashes, and evidence classes;
3. classify the accepted vault/platform/staging inclusion manifests using redaction-safe output;
4. review every finding without revealing values;
5. record proposed inclusion/exclusion treatment;
6. verify no live file changed.

If a likely live secret is found:

- do not display it;
- do not move or delete it;
- record redacted finding metadata;
- stop backup planning until the owner chooses treatment.

## Explicit non-scope

- archive creation;
- `age` installation or execution;
- key or identity generation;
- Google Drive access;
- USB discovery, formatting, or writing;
- restore-root creation;
- platform or vault remote creation;
- platform or vault push;
- Kaizen MCP changes;
- kaizen-platform changes;
- Milestone 8 reliability fixes;
- generic staging browser or read tool;
- generic secret scanning;
- backup generation;
- deletion.

## Required evidence return

Return:

- exact Go8 commit;
- exact changed paths;
- version and tool registry before/after;
- focused, integration, hammer, and full-suite counts;
- static capability scan;
- live staging inventory summary without content;
- redaction-safe secret findings and treatment candidates;
- proof of zero mutation in docs/platform/vault/staging;
- docs completion audit;
- revised Packet 011C prerequisite checkpoint.

## Acceptance criteria

Packet 011B is complete only when:

1. both tools satisfy the accepted specification;
2. staging root is exposed only to those tools;
3. all required tests pass;
4. no secret value appears anywhere in output or evidence;
5. both Packet 010F operations are inventoried live;
6. secret findings are classified without mutation;
7. Go8 authority does not widen beyond exact scope;
8. platform, vault, staging, and docs content remain unchanged except authorized docs audit files;
9. Go8 commit is clean and reviewable;
10. completion audit passes;
11. owner separately approves Packet 011C before backup creation.

## Stop gates

Stop when:

- predecessor state differs;
- live secret value appears in output;
- staging cannot be confined safely;
- implementation requires generic path/file access;
- tests require real credentials;
- output redaction cannot be proven;
- a reparse-point escape cannot be blocked;
- any live protected file changes;
- backup creation or external storage access becomes necessary;
- scope drifts into Milestone 8.
