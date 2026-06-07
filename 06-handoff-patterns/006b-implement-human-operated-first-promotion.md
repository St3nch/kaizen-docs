---
id: kz-tp-01KTHV0M3N7Q8R9S1T2VVWXYZ0
type: task-packet
status: active
project: kaizen-platform
summary: Implement one human-operated, hash-bound, recoverable first-time canonical promotion workflow after separate governance bootstrap.
created: 2026-06-07T20:10:00Z
updated: 2026-06-07T20:30:19Z
review_status: approved
authority: accepted
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 006B - Implement Human-Operated First Promotion

> Implementation status: owner-approved Packet 006B core implemented at platform commits `032fa63` and `667f304`; checkpoint audit Result 030 is partial with the remaining hammer gate open. This still authorizes no governance bootstrap, live canonical promotion, agent-triggered promotion, amendment, supersedence, correction, rollback execution, or Hermes/MCP exposure.

## Objective

Implement and hammer-test one human-operated workflow that can plan, approve, execute, inspect, and recover a **first-time `promote` action only** for one reviewed staged Markdown artifact.

Human approval must bind the exact staged-source SHA-256, the exact normalized canonical-candidate SHA-256, one approved canonical vault-relative destination, one successful promotion-validation result, one immutable plan, and one human actor/timestamp.

Canonical installation must use the Packet 006A primitive at platform commit `26271ce`.

This packet is not authority to perform a live promotion. Implementation and tests remain confined to disposable roots until a later explicit owner go/no-go authorizes first real execution.

## Read First

1. `C:\dev\kaizen-docs\03-research-results\025-implementation-checkpoint-audit-steward-reconciliation.md`
2. `C:\dev\kaizen-docs\03-research-results\028-packet-006a-implementation-steward-audit.md`
3. `C:\dev\kaizen-docs\05-specs\staging-and-promotion-workflow.md`
4. `C:\dev\kaizen-docs\05-specs\staging-write-wrapper-and-promotion-recovery.md`
5. `C:\dev\kaizen-docs\05-specs\promotion-event-schema.md`
6. `C:\dev\kaizen-docs\05-specs\kaizen-validation-gate-spec.md`
7. `C:\dev\kaizen-docs\05-specs\kaizen-id-and-prefix-registry.md`
8. `C:\dev\kaizen-docs\05-specs\kaizen-hammer-test-strategy.md`
9. `C:\dev\kaizen-docs\06-handoff-patterns\006a-prove-windows-first-time-atomic-install.md`
10. `C:\dev\kaizen\platform\AGENTS.md`
11. `C:\dev\kaizen\platform\src\kaizen\atomic_install.py`
12. `C:\dev\kaizen\platform\src\kaizen\validation.py`
13. `C:\dev\kaizen\platform\src\kaizen\root_config.py`

## Context Pack

```text
platform: 26271ce
full platform suite: 145 passed, zero skipped reported
Packet 006A: complete and steward-audited pass-with-documented-limitations
canonical vault: separate human-controlled Git repository
live staging: preserved create-only evidence; not a Packet 006B test target
combined Packet 006: retired
canonical promotion: not implemented or authorized
_governance bootstrap: not authorized or performed
```

## First-Slice Boundary

Packet 006B supports exactly:

```text
action: promote
destination before execution: absent
destination after successful execution: present
```

It does not support:

```text
amend
supersede
correct
general rollback execution
overwrite
replace existing canonical content
delete canonical content
```

Recovery events may describe a failed or recovered first-time promotion. They do not create a general rollback command.

## Approved Architecture

### Human-operated roles

```text
planner   - reads staging/canonical state; writes immutable staging-local operation evidence
approver  - explicit human command that writes one immutable approval record
executor  - explicit human command that appends events and may install one absent canonical file
recoverer - explicit human command that reconciles one incomplete operation
inspector - read-only operation status
```

No role is exposed to Hermes, MCP, or another agent surface.

### Operation evidence root

Use the configured sibling staging root:

```text
_promotion/operations/<operation_id>/
```

Exact files:

```text
canonical-candidate.md
validation.json
plan.json
approval.json
```

Rules:

- `operation_id` equals the intent event ID and matches `kz-prom-<ULID>`;
- operation directory creation is create-new;
- `canonical-candidate.md` is written and flushed first;
- `validation.json` is written and flushed second;
- `plan.json` is written and flushed last and is the immutable ready marker;
- planning failure before `plan.json` may remove only packet-owned incomplete files after confinement proof;
- once `plan.json` exists, plan, candidate, and validation evidence are immutable;
- `approval.json` is create-new and immutable;
- no evidence file is edited in place;
- changed source content requires a new operation ID and new plan.

### Plan contract

Required `plan.json` fields:

```text
schema_version
operation_id
created_at
action
project
note_id
note_type
source_path
source_sha256
canonical_candidate_sha256
destination_path
prior_review_status
new_review_status
prior_authority
new_authority
validation_run_id
validation_tool_version
validation_result_sha256
canonical_candidate_path
required_governance_log_path
required_governance_log_sha256
packet_006a_platform_commit
review_context
plan_sha256
```

Rules:

- `schema_version` is `1.0`;
- `action` is `promote` only;
- `source_path` resolves inside the configured staging root;
- `destination_path` is vault-relative, uses forward slashes, contains no `.` or `..`, and never targets `_governance/`;
- `source_sha256` binds exact staged bytes;
- `canonical_candidate_sha256` binds exact normalized canonical bytes;
- `validation_result_sha256` binds canonical JSON serialization of `validation.json`;
- `required_governance_log_sha256` binds the observed pre-existing promotion log state;
- `packet_006a_platform_commit` is `26271ce` unless a later reviewed amendment changes it;
- `review_context` is scoped to relevant governing IDs, repo/ref, reviewed paths/interfaces, versions, and hammer evidence;
- `plan_sha256` is computed over canonical JSON excluding the `plan_sha256` field.

### Validation evidence

The current validator does not generate a durable validation-run ID. Packet 006B must add promotion-validation evidence.

Required `validation.json` fields:

```text
schema_version
validation_run_id
validated_at
tool_version
source_path
source_sha256
canonical_candidate_sha256
destination_path
passed
error_count
warning_count
issues
normalized_metadata
relationship_resolution
link_resolution
duplicate_checks
private_data_checks
transition_checks
```

Rules:

- `validation_run_id` matches `kz-val-<ULID>`;
- validation runs against the exact staged source and exact canonical candidate;
- all stable-ID relationships must resolve in the disposable canonical root;
- body links must resolve and canonical content may not link into staging;
- duplicate stable ID, normalized destination collision, exact title/summary collision, and conflicting initial promotion fail;
- canonical candidate must not contain staging-only `validation_status`;
- durable provenance must be preserved;
- authority/review transition must be legal for the note type;
- warnings are preserved exactly;
- serialized evidence is stable and hashable.

### Canonical-candidate normalization

Allowed transformations only:

- remove staging-only `validation_status`;
- set the planned approved `review_status`;
- set planned accepted `authority` only where note type permits it;
- update `updated` to the planned canonical timestamp;
- normalize line endings to LF;
- normalize body links to portable relative Markdown syntax only when deterministic and validated;
- preserve `id`, `type`, `project`, `created`, durable provenance, summary, body content, and stable-ID relationships unless a deterministic canonical-only representation change is explicitly recorded.

Planner must render a human-readable staged-to-canonical diff. Any other content change blocks planning.

### Approval contract

Required `approval.json` fields:

```text
schema_version
operation_id
plan_sha256
source_sha256
canonical_candidate_sha256
destination_path
validation_run_id
approved_by
approved_at
approval_basis
reviewed_diff_sha256
approval_sha256
```

Rules:

- `approved_by` is a non-agent human actor identifier supplied through explicit invocation;
- `approved_at` is UTC and after validation/plan creation;
- approval repeats and exactly matches critical plan bindings;
- `reviewed_diff_sha256` binds the exact diff shown to the human;
- `approval_sha256` excludes itself from its canonical JSON hash;
- approval is create-new and immutable;
- no `--yes`, auto-approve, environment-variable approver, piped approval, or agent-supplied approval exists;
- execution requires explicit operation ID and human actor ID;
- any bound change makes approval stale.

### Governance bootstrap boundary

Packet 006B implementation must not create canonical governance structure.

Before execution, the canonical root must already contain an owner-approved and Git-committed:

```text
_governance/README.md
_governance/promotion-log.jsonl
```

Promotion log must be a regular non-reparse file containing zero or more valid JSONL events. Missing bootstrap returns `governance_not_bootstrapped` before canonical mutation.

### Promotion events

Create:

```text
schemas/promotion-event.schema.json
```

Every event requires:

```text
schema_version: 1.0
event_id: kz-prom-<ULID>
operation_id: kz-prom-<ULID>
event_phase: intent | committed | failed | recovered
action: promote
note_id
note_type
project
destination_path
content_sha256
approved_by
approved_at
validation_run_id
basis
```

Intent additionally includes source path/hash, canonical-candidate hash, prior/new review and authority, plan hash, approval hash, and review context.

Committed additionally includes intent event ID, installed file identity/size, and verified canonical hash.

Failed additionally includes intent event ID, failure code/summary, source/temp/canonical states, and observed canonical hash.

Recovered additionally includes intent event ID, recovery action, resulting states/hash, recovering human, and timestamp.

All canonical paths are vault-relative with forward slashes. Every line has a unique event ID; `operation_id` remains the intent ID.

### Event append rules

- append through verified `_governance` and regular-file handles;
- reject reparse points and path-replacement ambiguity;
- use canonical compact JSON, stable key ordering, and one LF;
- hold a named canonical-root mutex across classification and append;
- append and flush intent before canonical temp creation;
- append and flush committed only after canonical identity/hash verification;
- never edit/truncate existing bytes;
- malformed/truncated JSONL returns `recovery_required` before canonical mutation;
- detect duplicate event and operation IDs;
- duplicate terminal with matching evidence is idempotent; mismatched evidence is rejected.

### Execution protocol

1. Verify immutable plan, validation, candidate, and approval files.
2. Recompute every file and internal canonical-JSON hash.
3. Re-read staged source and require `source_sha256` equality.
4. Verify approval freshness and exact binding.
5. Verify canonical root, governance paths, log, and destination parent through handles.
6. Verify destination absent and note ID unused.
7. Acquire canonical-root promotion mutex.
8. Re-scan log/destination under mutex.
9. Append and flush intent.
10. Create and flush one operation-owned temp file in destination directory.
11. Verify temp hash and identity.
12. Install with Packet 006A primitive.
13. Verify canonical identity, size, and candidate hash.
14. Append and flush committed.
15. Return committed only after canonical and terminal event verification.
16. Retain staged source; do not archive/delete automatically.

### Recovery protocol

Recovery is human-operated for one explicit operation ID.

```text
intent absent, canonical absent
  -> no canonical mutation; operation remains planned/approved

intent present, canonical absent, temp absent
  -> append failed terminal; retain source; new plan required for retry

intent present, canonical absent, owned temp present with approved hash
  -> remove only verified operation-owned temp; append recovered; retain source

intent present, canonical present with approved hash, terminal absent
  -> append committed/recovered terminal without rewriting canonical

intent present, canonical present with different hash
  -> append failed when possible; promotion_conflict; never overwrite

committed terminal present, canonical present with approved hash
  -> idempotent committed result

terminal present but canonical absent or mismatched
  -> repository_blocked; no automatic repair
```

Recovery appends history only.

### Staged-source retention

Retain staged source after commit. A retained source is not promotable again automatically. Duplicate initial promotion is rejected through log and canonical-state checks.

## Stable Failure Codes

```text
invalid_promotion_request
operation_exists
operation_incomplete
plan_not_ready
plan_hash_mismatch
validation_evidence_invalid
validation_failed
source_changed
canonical_candidate_changed
approval_missing
approval_hash_mismatch
approval_stale
human_actor_invalid
destination_path_invalid
destination_exists
note_id_exists
governance_not_bootstrapped
governance_reparse_forbidden
promotion_log_invalid
promotion_log_append_failed
duplicate_event_id
duplicate_operation_id
promotion_mutex_failed
canonical_temp_create_failed
canonical_temp_hash_mismatch
atomic_install_failed
canonical_verification_failed
terminal_event_append_failed
recovery_required
promotion_conflict
repository_blocked
internal_failure
```

## CLI Surface

```text
kaizen-promote plan <staged-path> --destination <vault-relative-path>
kaizen-promote inspect <operation-id>
kaizen-promote approve <operation-id> --human-actor <actor-id>
kaizen-promote execute <operation-id> --human-actor <actor-id>
kaizen-promote recover <operation-id> --human-actor <actor-id>
```

Rules:

- `plan`/`inspect` do not write canonical content;
- `approve` writes only immutable staging-local approval evidence;
- `execute`/`recover` are the only canonical-mutating commands;
- roots come from trusted configuration, never caller input;
- stable JSON and human-readable output are required;
- no overwrite flag and no additional business action exist.

## Allowed Changes

```text
C:\dev\kaizen\platform\schemas\promotion-event.schema.json
C:\dev\kaizen\platform\src\kaizen\promotion_*.py
C:\dev\kaizen\platform\src\kaizen\validate_cli.py
C:\dev\kaizen\platform\src\kaizen\validation.py
C:\dev\kaizen\platform\src\kaizen\windows_fs.py
C:\dev\kaizen\platform\src\kaizen\root_config.py
C:\dev\kaizen\platform\src\kaizen\cli.py
C:\dev\kaizen\platform\tests\
C:\dev\kaizen\platform\README.md
C:\dev\kaizen\platform\pyproject.toml
```

Changes outside this list require steward review.

## Do Not Touch

- live `C:\dev\kaizen\vault` contents or Git history;
- live `C:\dev\kaizen\staging` contents or evidence;
- `C:\dev\kaizen-docs` during implementation;
- global Windows, Git, ACL, registry, service, Python, or Defender configuration;
- remotes;
- Hermes, MCP, Postgres, Qdrant, providers, Tauri, Obsidian plugins, or UI code.

## Implementation Requirements

- Separate pure contracts/hashing, normalization/diff, validation evidence, plan/approval persistence, event append, execution, recovery, and CLI rendering.
- Pure classification/validation functions must be independently testable.
- Assert `stat.FILE_ATTRIBUTE_REPARSE_POINT` exists and is nonzero on supported Windows; no zero fallback.
- Persist platform version, validator version, Packet 006A commit `26271ce`, schema `1.0`, ID prefixes, and canonical JSON rules.
- Planning may create only operation evidence beneath staging.
- Approval may create only `approval.json` in a ready operation.
- Execution/recovery may mutate only promotion log, one owned temp, and one absent approved destination.

## Constraints

- Python 3.11.15 and Windows first slice.
- Same-volume promotion only.
- One operation at a time per canonical root via named mutex.
- Human-operated local CLI only.
- No network/database/Qdrant/Git mutation.
- No live-root tests.
- No power-loss durability claim.
- No automatic canonical rollback.
- No staged-source deletion.
- No existing canonical destination support.

## Required Tests

At minimum:

1. valid plan creation with immutable ready marker;
2. duplicate operation ID;
3. failed planning leaves no ready plan;
4. source outside staging root;
5. source reparse/junction escape;
6. invalid or governance destination;
7. allowlisted candidate normalization only;
8. source/candidate hash binding;
9. deterministic repeated candidate generation;
10. `kz-val-<ULID>` validation run;
11. unresolved relationship;
12. broken body link;
13. staging backlink;
14. duplicate canonical note ID;
15. destination collision;
16. invalid review/authority transition;
17. private-data/secret failure;
18. warning preservation;
19. plan canonical-JSON/hash stability;
20. plan tamper detection;
21. candidate tamper detection;
22. validation-evidence tamper detection;
23. approval create-new semantics;
24. human actor requirement;
25. approval binds plan/source/candidate/destination/validation/diff;
26. stale approval after source change;
27. stale approval after candidate or plan change;
28. missing governance bootstrap refusal;
29. governance-directory reparse;
30. promotion-log reparse;
31. empty valid promotion log;
32. malformed/truncated JSONL;
33. duplicate event ID;
34. duplicate operation ID;
35. event schema validation;
36. forward-slash destination in every event;
37. intent flushed before temp creation;
38. temp create-new in destination directory;
39. temp hash mismatch;
40. destination appears before native install;
41. valid first-time promotion in disposable roots;
42. committed event has new event ID/same operation ID;
43. canonical identity/size/hash verification;
44. staged source retained;
45. repeated execute idempotency;
46. duplicate initial promotion rejected;
47. source locked or changed before execute;
48. destination-parent replacement probe;
49. destination-parent junction/reparse;
50. promotion-log lock/share interference;
51. two independent executes for same operation;
52. two operations racing for same destination;
53. abrupt exit after intent;
54. abrupt exit after temp write;
55. abrupt exit after canonical install before terminal;
56. terminal append failure after install;
57. recover intent/no canonical/temp;
58. recover removes only approved owned temp;
59. recovery preserves unknown/mismatched temp;
60. recover missing terminal when canonical hash matches;
61. recovery conflict on different canonical hash;
62. recovery idempotency;
63. terminal present but canonical absent blocks repository;
64. unrelated canonical files unchanged;
65. unrelated staging files unchanged;
66. live roots untouched;
67. full suite passes with zero unexplained skips.

Every mutating test uses disposable roots and asserts event bytes, evidence, source/temp/canonical state, identities, hashes, result code, and unrelated-file preservation.

## Validation

1. Run schema tests.
2. Run pure plan/approval/event/recovery unit tests.
3. Run focused integration tests repeatedly.
4. Run independent-process execute races repeatedly.
5. Run abrupt-exit subprocess tests.
6. Run full platform suite.
7. Report passed/failed/skipped/warnings.
8. Run `git diff --check`.
9. Scan changed files for control characters/malformed JSON.
10. Scan for backup/temp debris.
11. Verify live vault status/commit unchanged.
12. Verify live staging inventory/hashes unchanged.
13. Review every canonical-write path and handle lifetime.
14. Review every JSONL append/flush path.
15. Review operation-evidence cleanup ownership.
16. Produce a steward-reviewable completion report.

## Acceptance Criteria

1. Planning binds exact source/candidate hashes.
2. Validation evidence is durable, ID-bearing, and hash-bound.
3. Plan/approval are create-new and tamper-evident.
4. Approval cannot be automated or agent-supplied.
5. Governance is separately prebootstrapped.
6. Event schema enforces version and promotion IDs.
7. Canonical paths use forward slashes.
8. Intent is durable before canonical mutation.
9. Installation uses Packet 006A primitive.
10. Destination replacement is impossible.
11. Canonical file and terminal event verified before success.
12. Staged source retained.
13. Execute/recovery idempotent for matching evidence.
14. Conflicting state blocks rather than guesses.
15. Recovery appends and never rewrites history.
16. Unrelated files unchanged.
17. All tests pass with zero unexplained skips.
18. Live roots untouched.
19. No amend/supersede/correct/general rollback surface.
20. No Hermes/MCP/network/database/UI/Git mutation.
21. Implementation/evidence committed in platform repo.
22. Steward audit decides whether a separate first-real-promotion execution packet may be drafted.

## Stop Conditions

Stop and report `blocked` when:

- approval cannot bind both hashes;
- operation evidence cannot be immutable/tamper-evident;
- validation evidence cannot get a stable run ID;
- malformed event history cannot fail closed;
- intent cannot be flushed before canonical mutation;
- Packet 006A primitive must be broadened;
- a collision can modify canonical content;
- post-install/pre-terminal state cannot be classified deterministically;
- execution requires automatic governance bootstrap;
- tests require live roots;
- implementation requires agent/network/database/ACL/registry/prohibited surfaces;
- any failure modifies unrelated files.

Do not weaken the contract to avoid a blocker.

## Rollback and Recovery

Code rollback is through Git revert.

Runtime recovery is append-only and evidence-driven. No automatic canonical rollback is authorized. Conflicting canonical state blocks for human review.

Incomplete planning files may be removed only before `plan.json` exists and only after ownership/confinement proof. Ready operation evidence is immutable.

## Completion Report

Return:

```text
Status: complete | partial | blocked
Platform repository and commit:
Files created:
Files modified:
Commands run:
Schema results:
Focused test results:
Repeated race results:
Full-suite results:
Skipped tests and reasons:
Operation evidence layout:
Plan and approval hash rules:
Validation-run evidence:
Event schema and event-ID results:
Intent-before-mutation evidence:
Canonical install results:
Staged-source retention result:
Crash and recovery results:
Lock/share-interference results:
Reparse/junction results:
Unrelated-file verification:
Live vault status and commit:
Live staging inventory/hash verification:
Deviations:
Unresolved risks:
Recommendation for first-real-promotion execution packet:
```
