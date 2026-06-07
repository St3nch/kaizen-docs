---
id: kz-tp-01KTHGXA9XM65AFP5HSSWP8FQM
type: task-packet
status: archived
project: kaizen-platform
summary: Retired combined promotion draft preserved as source material for the required Packet 006A and 006B split.
created: 2026-06-07T17:30:00Z
updated: 2026-06-07T17:30:00Z
review_status: pending
authority: proposed
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 006 - Implement Human-Operated Canonical Promotion

> Security status: retired combined draft. This file is preserved only as source material for the required Packet 006A and 006B split. It is not eligible for owner approval and authorizes no implementation.

## Objective

Implement a narrowly scoped human-operated command in `C:\dev\kaizen\platform` that promotes one reviewed and validated staged Markdown artifact into the canonical vault through a recoverable state machine.

The implementation must:

1. bind human approval to the exact staged source hash, exact normalized canonical-candidate hash, and destination;
2. re-run deterministic validation against the exact staged bytes;
3. append and flush a unique promotion intent before any canonical mutation;
4. write and flush a uniquely named temporary file in the canonical destination directory;
5. verify the temporary file hash against the approved canonical-candidate hash;
6. install the canonical file through a same-volume atomic operation;
7. append and flush a committed event using the same operation ID;
8. verify canonical file plus terminal event;
9. retain or finalize staging only according to explicit policy;
10. deterministically recover interrupted operations without guessing.

## Read First

1. `C:\dev\kaizen-docs\04-design-decisions\0001-two-zone-agent-write-model.md`
2. `C:\dev\kaizen-docs\04-design-decisions\0002-search-before-create-and-diff-before-write.md`
3. `C:\dev\kaizen-docs\04-design-decisions\0003-raw-markdown-is-canonical.md`
4. `C:\dev\kaizen-docs\04-design-decisions\0005-api-only-structured-data-access.md`
5. `C:\dev\kaizen-docs\04-design-decisions\0006-hammer-tests-are-a-hard-gate.md`
6. `C:\dev\kaizen-docs\05-specs\staging-write-wrapper-and-promotion-recovery.md`
7. `C:\dev\kaizen-docs\05-specs\staging-and-promotion-workflow.md`
8. `C:\dev\kaizen-docs\05-specs\promotion-event-schema.md`
9. `C:\dev\kaizen-docs\05-specs\kaizen-validation-gate-spec.md`
10. `C:\dev\kaizen-docs\03-research-results\023-create-only-staging-write-wrapper-steward-audit.md`
11. `C:\dev\kaizen\platform\AGENTS.md`
12. `C:\dev\kaizen\vault\README.md`

## Context Pack

Current proven state:

```text
platform: 36fa419
vault: 3de6042
staging writer: implemented, audited pass-with-notes
platform tests: 126 passed, 0 skipped
real staging smoke artifact: present and independently valid
canonical promotion: not implemented
```

## Scope

Implement only:

1. immutable promotion request, response, approval, and event models;
2. one first-time `promote` operation;
3. one human-run CLI conceptually named `kaizen-promote`;
4. strict approval binding to staged path, staged-source hash, normalized canonical-candidate hash, canonical destination, validation run, approver, approval timestamp, and basis;
5. canonical destination preflight and unrelated-content conflict rejection;
6. global note-ID uniqueness and destination-placement checks required by the current registries;
7. canonical-content normalization required by the workflow:
   - remove staging-only `validation_status`;
   - preserve durable agent provenance;
   - apply only the approved `review_status` and `authority` transition;
8. append-only promotion events at `_governance/promotion-log.jsonl`;
9. state-machine phases P0 through P5 from the governing spec;
10. same-directory temporary-file creation and exact hash verification;
11. same-volume atomic first-time installation;
12. deterministic scan and reconciliation of incomplete intents;
13. unit, integration, CLI, concurrency, interruption, recovery, file-lock, and real-execution hammer tests;
14. platform-local documentation.

## Non-Scope

Do not implement:

- agent-triggered or unattended promotion;
- Hermes or MCP access;
- amendment of an existing canonical note;
- supersedence, correction, or governed rollback execution;
- silent overwrite or replace of an existing canonical destination;
- general canonical filesystem APIs;
- arbitrary source or destination roots;
- cross-volume copy-and-delete behavior;
- Git commit, push, merge, branch, or remote operations;
- automatic deletion of staging after promotion;
- promotion of non-Markdown artifacts;
- Postgres, Qdrant, providers, DataForSEO, marketplace logic, Tauri, Obsidian plugins, or UI work;
- ACL changes, elevated-process requirements, or Windows policy changes.

## Allowed Changes

Implementation files:

```text
C:\dev\kaizen\platform
```

Runtime mutation during approved implementation tests is limited to disposable fixtures plus one explicitly approved real promotion target inside:

```text
C:\dev\kaizen\vault
```

The implementation must not mutate unrelated vault files.

## Do Not Touch

- Git history of the vault or platform;
- remote hosting;
- `C:\dev\kaizen-docs` during implementation;
- global Git, Python, or Windows configuration;
- unrelated staging artifacts;
- Obsidian configuration;
- databases or provider credentials.

## Request Contract

Required fields:

```text
source_path
source_content_sha256
destination_path
action: promote
note_id
note_type
project
approved_by
approved_at
validation_run_id
basis
prior_review_status
new_review_status
prior_authority
new_authority
request_id
```

Rules:

- `source_path` must resolve to an existing file beneath the trusted staging root;
- `destination_path` is canonical-vault-relative, forward-slash normalized, and caller-independent from root selection;
- `source_content_sha256` must match the exact staged bytes before normalization;
- `canonical_candidate_sha256` must match the exact deterministic canonical bytes produced after removing staging-only fields and applying only the approved review/authority transition;
- `approved_by` must identify a human actor and must not contain an agent/model identity;
- approval timestamp must not precede the successful validation run;
- initial legal authority transition is `pending -> approved` plus `proposed -> accepted` where the note type supports accepted authority;
- `request_id` is stable across retries;
- no request field may authorize overwrite or arbitrary actions.

## Approval Boundary

A promotion cannot begin unless all are true:

- exact staged bytes revalidate successfully;
- stable relationships and canonical links resolve;
- no prohibited private/customer data is detected;
- note ID is globally unique;
- destination placement matches the note-type registry;
- destination is absent;
- exact staged-source hash, exact canonical-candidate hash, destination, and authority transition are human-approved;
- validation run ID and validator version are recorded;
- staging and canonical destination are on the same volume;
- the human-run command is the sole canonical writer.

Any change to staged bytes, normalized canonical bytes, destination, validation evidence, approver, basis, or authority transition invalidates approval and requires a new plan plus approval.

## Event Contract

Use schema version `1.0` and the fields required by `05-specs/promotion-event-schema.md`.

Rules:

- every JSONL line has a unique promotion-event ID;
- intent event ID equals the shared `operation_id`;
- later events use unique event IDs and copy the operation ID;
- intent, committed, failed, and recovered phases are append-only;
- existing log lines are never edited or deleted;
- destination paths stored in events are vault-relative;
- an intent append and flush is mandatory before canonical temporary-file creation;
- if intent append/flush fails, no canonical file or temporary file may be created.

## State Machine

### P0 - approved

The exact staged-source hash, exact canonical-candidate hash, destination, validation evidence, human approver, and transition are approved.

### P1 - intent recorded

Append and flush the complete intent event. The intent event ID becomes `operation_id`.

### P2 - canonical temporary file written

Create a uniquely named temporary file inside the destination directory using create-new semantics. Write the exact approved canonical-candidate bytes, flush, reopen, hash, and verify.

### P3 - canonical file installed

Install the temporary file through a Windows same-volume atomic rename suitable for a destination that must not already exist. Fail closed on destination collision, file lock, reparse point, volume mismatch, or any unexpected filesystem state.

### P4 - completion recorded

Append and flush a committed event with a new event ID, shared operation ID, intent reference, and verified final hash.

### P5 - staging finalized

For this milestone, default retention policy is **retain staged source after success**. Record that retention choice. Do not delete or archive staging automatically.

## Recovery Rules

Implement a deterministic recovery scan for intents without terminal events.

Required classifications:

1. **Canonical absent**
   - inspect/remove packet-owned orphan temporary files when safe;
   - append failed or recovered evidence;
   - retain staged source;
   - allow a new human-reviewed retry.

2. **Canonical present with approved canonical-candidate hash**
   - do not rewrite canonical content;
   - append missing committed/recovered evidence;
   - verify staging-retention policy.

3. **Canonical present with different hash**
   - append failure evidence;
   - stop and require human review;
   - do not overwrite or delete either artifact.

4. **Committed event exists and staging remains**
   - treat retained staging as valid under this milestone's retention policy;
   - return prior committed result idempotently.

5. **Duplicate retry**
   - same request/operation, same staged-source hash, same canonical-candidate hash, and same destination: prior committed result or governed idempotent recovery;
   - changed hash, destination, action, approval, or transition: reject.

Recovery must itself be idempotent.

## Native Filesystem Requirements

Reuse the proven Windows handle-relative boundary where appropriate.

The implementation must demonstrate:

- trusted canonical-root handle acquisition;
- reparse-point and junction refusal;
- destination-directory containment;
- no delete-sharing while critical directory/file handles are held;
- create-new temporary-file semantics;
- flush before install;
- same-volume verification;
- first-time atomic rename that refuses an existing destination;
- deterministic behavior under antivirus/file-lock interference.

If the available Windows primitive cannot prove first-time atomic installation without silent replacement, stop as `blocked`. Do not substitute check-then-rename and call it race-resistant.

## CLI

Provide a human-run command conceptually shaped as:

```text
kaizen-promote --request promotion-request.json
kaizen-promote --recover
```

The `--plan` phase deterministically renders the exact canonical candidate and hashes without mutating the vault. Human approval binds the resulting immutable plan. The `--execute` phase refuses any request whose staged bytes, candidate bytes, destination, validation evidence, or approval fields differ from that plan.

The CLI must not accept:

- canonical root overrides;
- overwrite flags;
- arbitrary event-log paths;
- agent credentials;
- shell commands;
- broad cleanup flags.

Exit codes:

- `0` - committed, recovered, or proven idempotent prior success;
- `1` - governed rejection such as invalid approval, stale hash, destination collision, validation failure, or unsupported transition;
- `2` - trusted configuration, durable log, recovery-required/conflict, native filesystem, or internal failure.

## Stable Failure Codes

Cover at minimum:

```text
invalid_request
invalid_human_approval
stale_approval
validation_failed
validation_run_mismatch
source_hash_mismatch
canonical_content_hash_mismatch
note_id_conflict
destination_conflict
destination_reparse_forbidden
cross_volume_unsupported
promotion_intent_append_failed
temporary_create_failed
temporary_flush_failed
temporary_hash_mismatch
atomic_install_failed
canonical_verification_failed
promotion_commit_append_failed
idempotency_conflict
recovery_required
recovery_hash_mismatch
recovery_log_failed
file_lock_interference
internal_failure
```

Reuse existing root/path/native failure codes rather than duplicating equivalent meanings.

## Required Tests

At minimum:

1. valid first-time promotion;
2. staged hash differs from approved hash;
3. staged content changes after approval;
4. normalized canonical candidate differs from the approved candidate hash;
5. failed or stale validation run;
5. non-human approver rejected;
6. invalid authority transition rejected;
7. note ID already exists elsewhere;
8. destination already exists;
9. destination parent is a reparse point or junction;
10. destination parent replacement/race probe;
11. unsupported cross-volume attempt;
12. intent append failure creates no temporary or canonical file;
13. crash after intent;
14. crash after temporary write;
15. crash after canonical install before committed event;
16. temporary-file hash mismatch;
17. canonical verification mismatch;
18. committed event append failure;
19. antivirus/file-lock interference;
20. duplicate same-request retry;
21. changed-hash or changed-destination retry;
22. recovery scan with canonical absent;
23. recovery scan with matching canonical file;
24. recovery scan with mismatched canonical file;
25. recovery scan idempotency;
26. append-only log remains parseable;
27. unrelated canonical and staging files remain untouched;
28. no automatic staging deletion;
29. platform tests remain green;
30. real execution uses an explicitly approved disposable or smoke target only.

## Acceptance Criteria

1. typed promotion request, response, approval, and event models exist;
2. only first-time `promote` is supported;
3. command is human-run only;
4. approval binds exact staged-source hash, exact canonical-candidate hash, destination, transition, validation, approver, and basis;
5. intent is appended and flushed before canonical mutation;
6. canonical temporary file is create-new, same-directory, flushed, and hash-verified;
7. first-time install is same-volume, atomic, and refuses existing destinations;
8. committed event is appended and flushed after canonical verification;
9. file and event phases are never described as one transaction;
10. incomplete operations are visible and deterministically recoverable;
11. recovery is idempotent and fails closed on mismatch;
12. unrelated canonical content is never overwritten or removed;
13. staged source is retained by default;
14. no amend/supersede/correct/rollback execution is introduced;
15. no Hermes/MCP or agent-triggered path is introduced;
16. all required static, unit, integration, interruption, recovery, concurrency, and file-lock tests pass;
17. real execution evidence records hashes, event IDs, operation ID, validation run, approver, destination, and resulting states;
18. vault/platform repositories are clean at completion;
19. implementation is committed in the platform repository;
20. completion report discloses skips, deviations, unresolved risks, and exact repository states.

## Stop Conditions

Stop and report `blocked` when:

- a first-time atomic install primitive cannot be proven to refuse existing destinations;
- canonical and staging roots are on different volumes;
- approval cannot be bound to exact normalized canonical bytes;
- promotion intent cannot be durably flushed before mutation;
- canonical hash or destination state is ambiguous;
- recovery cannot classify the current state deterministically;
- implementation requires agent access, broad filesystem access, or unapproved Git operations;
- tests require unapproved ACL or policy changes.

Do not weaken the contract to avoid a blocker.

## Rollback and Recovery

Platform code rolls back through Git revert.

Runtime promotion state is reconciled through append-only failed/recovered events and the deterministic recovery scanner. Never edit or delete existing promotion events.

Do not delete or rewrite unrelated canonical or staged content during recovery.

## Completion Report

Return:

```text
Status: complete | partial | blocked
Platform repository and commit:
Vault status and commit:
Staging source and retention state:
Canonical destination:
Promotion-log events:
Operation ID:
Intent event ID:
Committed/recovery event IDs:
Approved staged-source, canonical-candidate, and installed hashes:
Validation run and validator version:
Human approver and basis:
Files created:
Files modified:
Commands run:
Tests and hammer tests:
Interruption and recovery results:
File-lock and cross-volume results:
CLI exit checks:
Acceptance criteria result:
Skipped tests with reasons:
Deviations:
Unresolved issues:
Contract findings for kaizen-docs:
Recommended next task:
Final Git statuses:
```

Do not mark complete with failing tests, a dirty platform or vault tree, unexplained partial promotion state, unauthorized canonical mutation, missing terminal evidence, or unreviewed scope expansion.
