---
id: kz-aud-01KTHXBNVNY1NBS75VR4K33FBR
type: audit
status: complete
project: kaizen-platform
summary: Final steward audit of the Packet 006B human-operated first-time promotion workflow implementation.
created: 2026-06-07T20:46:01Z
updated: 2026-06-07T20:46:01Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-validation-gate-spec.md
---

# Research Result 031 - Packet 006B Final Implementation Steward Audit

## Scope

Audit the complete Packet 006B implementation across platform commits:

```text
032fa63 Implement Packet 006B promotion core
667f304 Harden Packet 006B promotion evidence
703d532 Complete Packet 006B hammer hardening
```

The audit determines whether Packet 006B is complete as a disposable-root implementation dependency. It does not authorize live `_governance` creation or a real canonical promotion.

## Final implementation surface

```text
schemas/promotion-event.schema.json
src/kaizen/promotion_contracts.py
src/kaizen/promotion_plan.py
src/kaizen/promotion_events.py
src/kaizen/promotion_workflow.py
src/kaizen/promotion_cli.py
src/kaizen/atomic_install.py
src/kaizen/windows_fs.py
src/kaizen/root_config.py
tests/test_promotion_workflow.py
```

## Proven workflow

The implementation provides a human-operated first-time `promote` workflow with:

- immutable create-new candidate, validation, plan, and approval evidence;
- distinct staged-source and canonical-candidate SHA-256 bindings;
- `kz-val-<ULID>` validation-run evidence;
- `kz-prom-<ULID>` operation and event IDs;
- human-only approval, chronology, actor, and reviewed-diff binding;
- deterministic allowlisted canonical normalization;
- global note-ID, relationship, body-link, and staging-backlink checks;
- strict schema-versioned event validation;
- fail-closed malformed or locked promotion-log handling;
- durable intent append and flush before canonical temporary-file creation;
- verified destination-parent handles held through intent, temp creation, install, verification, and terminal append;
- destination-parent rename blocking through DELETE access without delete sharing;
- handle-relative temporary-file verification;
- handle-relative Packet 006A no-replacement installation;
- canonical file identity, size, and hash verification;
- append-only committed and recovered terminal evidence;
- retained staged source;
- idempotent execute and recovery outcomes;
- exact live-root refusal and a fixed disposable CLI sandbox.

## Final test evidence

```text
Focused Packet 006B suite: 45 passed
Repeated focused runs: 3 consecutive runs, 45 passed each
Same-operation independent-process races: 20 passed
Competing-operation same-destination races: 20 passed
Full platform suite: 190 passed
Failed: 0
Skipped: 0
Xfailed: 0
Diff check: clean
Control-character scan: clean
Backup/temp sweep: clean
```

Direct Windows evidence includes:

- source-file symlink escape rejection;
- source-directory junction escape rejection;
- governance-directory junction rejection;
- promotion-log file-symlink rejection;
- destination-parent junction rejection;
- source incompatible-share interference;
- promotion-log incompatible-share interference;
- destination-parent rename attempts after intent and immediately before terminal append;
- temp tampering after initial verification;
- destination appearance after temp verification and before install;
- terminal-event append interruption after canonical installation;
- abrupt `os._exit(91)` after intent, after temp creation, and after canonical install;
- recovery convergence for all interruption states;
- representative intent, committed, failed, and recovered event records checked against the schema file and runtime contract.

## Native-handle audit

### Destination parent

The destination-parent chain is opened relative to the verified canonical root. Every segment rejects reparse points. The final directory handle requests DELETE access while withholding delete sharing, which blocks rename/replacement during the full critical section.

The same verified handle remains open through:

```text
intent append
temporary-file create
temporary-file hash verification
no-replacement rename
canonical hash/identity verification
terminal-event append
```

### Atomic install

`install_first_time_atomic_with_parent_handle` accepts only the already-verified parent handle and sibling leaf names. It verifies source hash and same-volume identity, calls `NtSetInformationFile(FileRenameInformation)` with replacement disabled, and verifies the installed file by handle.

No path-based copy/delete fallback, overwrite option, or existing-destination update exists.

### Governance log

Governance paths must already exist and must be regular non-reparse paths. Event history is fully parsed and validated before mutation. Appends are canonical JSONL, append-only, and flushed.

### Recovery

Recovery is evidence-driven and append-only. It never guesses, overwrites canonical content, rewrites event history, or deletes unknown temporary files.

## Live-root preservation

The live vault remained clean throughout Packet 006B testing.

The live staging inventory and hashes remained unchanged:

```text
README.md
  bytes: 330
  sha256: 52e5f276e3b0259d31babbc09138e09e0f3d41133b563333113dcdd3ab9ff216

staging-write-attempts.jsonl
  bytes: 1486
  sha256: 35e2e1308a32015493881c25ede6d3ddefbcf527de7a09e39ddce77fd825af80

projects/kaizen-platform/claims/create-only-wrapper-smoke.md
  bytes: 1027
  sha256: d6033bacf6325abf40082fdca9bf7376888e6d7d1380bca966d3fdbc8e60d42e
```

## Result 025 disposition

Packet 006B implementation closes the remaining Gate B findings:

```text
F-09  source and canonical-candidate hash binding
F-10  complete packet contract
F-11  editorial/integrity review
F-12  split-gate implementation sequence
F-13  immutable plan and approval storage
F-15  promotion-event IDs
F-18  normalized canonical event paths
F-19  schema-versioned records
F-27  explicit nonzero Windows reparse constant
```

The separate live-vault bootstrap finding remains open:

```text
F-14  owner-approved _governance bootstrap before first real promotion
```

## Limitations

- The workstation exposes one writable NTFS volume, so a physical cross-volume integration test remains unavailable.
- The workflow proves process-interruption and namespace behavior, not storage-device power-loss durability.
- Packet 006B supports first-time `promote` only; no amendment, supersedence, correction, overwrite, delete, or general rollback command exists.
- The CLI remains restricted to a fixed disposable sandbox.

## Contract assessment

The implementation remains inside the approved boundary:

- no live `_governance` creation;
- no live canonical promotion;
- no staged-source deletion;
- no agent, Hermes, or MCP promotion surface;
- no network, database, Qdrant, UI, or Git mutation by the workflow;
- no caller-selected roots;
- no overwrite-capable API.

## Steward verdict

**pass with documented limitations**

Packet 006B is complete as a disposable-root implementation and recovery dependency.

## Next action

Draft and security-audit a separate owner-controlled task packet for bootstrapping only:

```text
_governance/README.md
_governance/promotion-log.jsonl
```

in the live canonical vault. Do not perform the bootstrap or a real promotion until that packet is explicitly owner-approved.
