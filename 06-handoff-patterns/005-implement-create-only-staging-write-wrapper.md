---
id: kz-tp-01KTHDKAM3KDPMS474WAKXRJCA
type: task-packet
status: complete
project: kaizen-platform
summary: Implement a create-only staging-write wrapper with exact validation, provenance, idempotency, and append-only attempt evidence.
created: 2026-06-07T00:00:00Z
updated: 2026-06-07T17:15:00Z
review_status: approved
authority: accepted
primary_spec: 05-specs/staging-write-wrapper-and-promotion-recovery.md
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 005 - Implement Create-Only Staging-Write Wrapper

> Security status: owner-approved and accepted for execution. This packet may create new Markdown files only inside the trusted sibling staging root. It does not authorize canonical writes, overwrite, edit, move, rename, delete, promotion, or agent integration.

## Objective

Implement a narrowly scoped create-only staging-write wrapper in `C:\dev\kaizen\platform` that:

1. accepts only a typed request contract;
2. validates the logical path through the proven confinement layer;
3. records and flushes append-only pre-write attempt evidence before mutation;
4. creates a new staged Markdown file exactly once using fail-closed create-new semantics;
5. validates the exact bytes written in staging mode;
6. records terminal success, rejection, or failure evidence;
7. supports idempotent retries by `request_id` and content hash;
8. never touches the canonical vault.

## Read First

1. `C:\dev\kaizen-docs\04-design-decisions\0001-two-zone-agent-write-model.md`
2. `C:\dev\kaizen-docs\04-design-decisions\0002-search-before-create-and-diff-before-write.md`
3. `C:\dev\kaizen-docs\04-design-decisions\0005-api-only-structured-data-access.md`
4. `C:\dev\kaizen-docs\04-design-decisions\0006-hammer-tests-are-a-hard-gate.md`
5. `C:\dev\kaizen-docs\05-specs\staging-write-wrapper-and-promotion-recovery.md`
6. `C:\dev\kaizen-docs\05-specs\staging-and-promotion-workflow.md`
7. `C:\dev\kaizen-docs\05-specs\kaizen-validation-gate-spec.md`
8. `C:\dev\kaizen-docs\05-specs\kaizen-hammer-test-strategy.md`
9. `C:\dev\kaizen-docs\03-research-results\021-staging-path-confinement-steward-audit.md`
10. `C:\dev\kaizen\platform\AGENTS.md`
11. `C:\dev\kaizen\staging\README.md`

## Context Pack

Current state:

```text
platform: b39b3a6
vault: 3de6042
staging: README.md only
path checker: implemented and audited pass-with-notes
```

The current Python layer proves dry-run syntax, root, parent-chain, and reparse-point checks. It does not yet prove a race-resistant write primitive.

## Scope

Implement only:

1. immutable request, response, and audit-event models;
2. a single create-only library function conceptually named `create_staged_markdown`;
3. a CLI conceptually named `kaizen-stage-create`;
4. append-only JSONL attempt evidence stored outside the canonical vault;
5. pre-write event append plus flush before mutation;
6. create-new behavior that refuses any existing destination;
7. exact UTF-8 byte hashing before and after write;
8. staging-mode validation of the exact file content after creation;
9. idempotent retry behavior keyed by `request_id`;
10. terminal audit events for `created`, `rejected`, and `failed` outcomes;
11. bounded crash/recovery reconciliation for interrupted create attempts;
12. unit, integration, CLI, concurrency, and real-execution hammer tests;
13. platform-local documentation.

## Non-Scope

Do not implement:

- canonical writes;
- promotion commands, promotion state machine, or promotion logs;
- overwrite, append, edit, patch, move, rename, delete, cleanup, or archive;
- caller-selected filesystem roots;
- general filesystem access;
- cross-note resolution;
- human approval workflows;
- task-packet promotion;
- Hermes, MCP, or any agent connection;
- Postgres, Qdrant, providers, DataForSEO, marketplace logic, Tauri, Obsidian plugins, or UI work;
- ACL changes or elevated-process requirements;
- cross-volume writes;
- any write outside `C:\dev\kaizen\staging` and the approved non-canonical attempt-log location.

## Allowed Changes

Implementation changes are limited to:

```text
C:\dev\kaizen\platform
```

Runtime writes during tests and execution are limited to:

```text
C:\dev\kaizen\staging
```

and one approved append-only attempt log under the staging root, for example:

```text
C:\dev\kaizen\staging\_audit\staging-write-attempts.jsonl
```

The exact location must be fixed by trusted configuration, not caller input.

## Do Not Touch

- tracked files in `C:\dev\kaizen\vault`;
- vault Git history;
- `C:\dev\kaizen-docs` during implementation;
- global Git or Python configuration;
- sibling repositories;
- remote hosting;
- Obsidian configuration;
- Windows ACL or security policy.

## Output Location

Code and tests:

```text
C:\dev\kaizen\platform
```

Runtime staged artifacts and audit evidence:

```text
C:\dev\kaizen\staging
```

## Interfaces and Data

### Request contract

Required fields:

```text
logical_path
content
note_type
project
agent_id
model_id
session_id
request_id
search_run_id
```

Optional fields may include:

```text
expected_content_sha256
source_refs
validation_mode
```

Rules:

- `logical_path` is relative only;
- `content` is bounded UTF-8 Markdown;
- `note_type` is a registered value;
- `project` is a validated slug or approved stable project identifier;
- provenance fields are nonempty durable strings;
- `request_id` is unique and stable across retries;
- `search_run_id` is required evidence that search-before-create occurred;
- callers cannot select roots, overwrite behavior, permissions, terminal commands, or database actions.

### Response contract

Return at minimum:

```text
request_id
result: created | rejected | failed
logical_path
resolved_path
bytes_written
content_sha256
validation_status: not-run | passed | failed
validation_run_id
audit_event_id
failure_code
message
```

### Attempt evidence

Every accepted, rejected, or failed request produces append-only JSONL evidence.

Each event must include at minimum:

```text
event_id
event_time
phase: intent | committed | rejected | failed | recovery
request_id
logical_path
project
note_type
agent_id
model_id
session_id
search_run_id
expected_content_sha256
content_sha256
validation_status
validation_run_id
result
failure_code
message
```

The pre-write `intent` event must be appended and flushed before filesystem mutation. If that append or flush fails, the wrapper rejects the request and writes nothing.

## Implementation Requirements

### Create-new primitive

The implementation must use a primitive with create-new/exclusive semantics. It must fail if the target already exists.

Acceptable behavior must be demonstrated, not assumed:

- one request creates the file;
- a concurrent second request cannot overwrite it;
- an existing file is never truncated;
- a different-hash retry cannot replace it;
- a same-request same-hash retry returns the prior outcome or a proven idempotent no-op.

If Python's available standard-library or Windows bindings cannot prove this safely against the accepted path boundary, stop and return `blocked`. Do not substitute a pre-check followed by ordinary open/write as though it were race-resistant.

### Directory creation

Parent-directory creation, if implemented, must be bounded and create-only.

Rules:

- only unresolved safe descendants previously accepted by the path-confinement layer may be created;
- each created parent must remain beneath the trusted staging root;
- existing reparse points, junctions, mount points, symlinks, or ambiguous parents fail closed;
- no directory is created before the pre-write audit event is durable;
- failed file creation must not delete preexisting directories;
- cleanup of packet-created empty directories after failure may be implemented only when ownership is unambiguous and tested.

### Content and provenance

Before write:

- require bounded UTF-8 text;
- reject null bytes and undecodable data;
- require the frontmatter note type and project to match the request;
- require `agent`, `model`, and `session` frontmatter values to match request provenance;
- require `validation_status` to be present and staging-appropriate;
- reject `authority: accepted`, `review_status: approved`, or any equivalent canonical-ready claim from the agent;
- compute SHA-256 over the exact UTF-8 bytes to be written;
- compare with `expected_content_sha256` when supplied.

After write:

- reopen and read the exact file bytes;
- verify the persisted SHA-256 equals the pre-write hash;
- run `kaizen-validate --mode staging` through the library interface, not shell parsing;
- return success only when validation passes;
- record validation evidence.

A post-write validation failure is a failed create attempt. Do not silently delete or rewrite the artifact. Record the exact state for recovery and human inspection.

### Idempotency

The durable attempt log is the initial idempotency source for this milestone.

Required behavior:

- same `request_id`, same logical path, same hash, prior committed result: return the prior committed outcome without writing;
- same `request_id`, changed path or hash: reject as idempotency conflict;
- different `request_id`, existing target: reject as destination exists;
- interrupted intent with no file: recovery may mark failed or safely retry after reconciliation;
- interrupted intent with matching file hash: recovery may validate and commit the outcome;
- interrupted intent with mismatched file hash: fail closed and require human review.

### Failure codes

Use stable codes covering at least:

```text
invalid_request
invalid_provenance
search_evidence_missing
content_too_large
content_hash_mismatch
frontmatter_request_mismatch
agent_authority_forbidden
path_rejected
prewrite_audit_failed
destination_exists
create_new_failed
persisted_hash_mismatch
validation_failed
idempotency_conflict
recovery_required
recovery_hash_mismatch
audit_terminal_append_failed
internal_failure
```

Reuse existing path-confinement codes rather than inventing aliases for the same condition.

### CLI

Provide a deliberately narrow CLI suitable for human-run testing, not agent exposure.

Conceptual form:

```text
kaizen-stage-create --request request.json
```

Optional:

```text
--json
```

The request file is local UTF-8 JSON matching the typed contract. The CLI must not accept a root argument, overwrite flag, arbitrary audit-log location, or direct shell command.

Exit codes:

- `0` - created or proven idempotent prior success;
- `1` - governed rejection such as invalid request, path rejection, validation failure, destination exists, or idempotency conflict;
- `2` - trusted configuration, audit-log, usage, recovery-required, or internal failure.

## Constraints

- Python 3.11.15 unless a narrowly justified native helper is required;
- Windows 11 compatible;
- no runtime network access;
- no canonical write capability;
- no agent-facing service exposure;
- no ordinary overwrite-capable file open;
- no check-then-write sequence presented as race-resistant;
- no silent normalization of forbidden paths;
- no hidden deletion or cleanup of failed artifacts;
- no secrets or private data in fixtures;
- preserve all existing tests;
- fail closed on ambiguity.

## Documentation Updates

Update platform-local README and `AGENTS.md` only.

Document:

- the exact create-only boundary;
- audit-log location;
- retry and recovery behavior;
- CLI contract and exit codes;
- explicit absence of canonical promotion and agent authorization.

## Validation

Run and report at minimum:

```powershell
.\.venv\Scripts\python.exe -m pytest
.\.venv\Scripts\python.exe -m compileall -q src tests
.\.venv\Scripts\python.exe -m pip check
.\.venv\Scripts\kaizen-stage-create.exe --request tests\fixtures\staging-write\valid-request.json
```

Also verify:

```text
same request retry
same request changed hash
existing destination
concurrent duplicate create
pre-write audit append failure
persisted-hash mismatch simulation
staging validation failure
interrupted intent recovery with no file
interrupted intent recovery with matching file
interrupted intent recovery with mismatched file
canonical vault Git status unchanged
```

## Required Hammer Tests

At minimum:

1. safe create under nonexistent approved descendants;
2. traversal and hostile paths rejected before mutation;
3. existing destination never truncated;
4. two concurrent writers targeting the same path yield exactly one create;
5. same request and same hash is idempotent;
6. same request and changed hash is rejected;
7. different request and same destination is rejected;
8. intent-log append failure writes nothing;
9. crash after intent but before create;
10. crash after create but before validation;
11. crash after validation but before terminal event;
12. matching-file recovery;
13. mismatched-file recovery fails closed;
14. invalid staging frontmatter rejected;
15. forbidden accepted/approved authority rejected;
16. provenance mismatch rejected;
17. expected-hash mismatch rejected;
18. append-only log remains parseable after repeated attempts;
19. target file bytes equal the reported hash;
20. canonical vault remains byte-for-byte and Git clean.

## Acceptance Criteria

1. typed request, response, and audit-event models exist.
2. create-only staging function exists and has no overwrite option.
3. CLI exists and accepts only a request JSON path plus output mode.
4. pre-write audit intent is appended and flushed before mutation.
5. pre-write audit failure writes nothing.
6. create-new semantics are race-resistant and proven by concurrency tests.
7. existing files are never truncated or replaced.
8. exact content hash is verified before and after write.
9. request metadata and staged frontmatter provenance must match.
10. agent-authored accepted/approved authority is rejected.
11. staging validation runs against the exact persisted artifact.
12. all outcomes produce append-only evidence.
13. idempotent retry and conflict behavior match the packet.
14. interrupted attempts can be reconciled without guessing.
15. no canonical file, vault Git state, or vault history changes.
16. no agent integration or general filesystem API is introduced.
17. all existing tests remain green.
18. new unit, integration, concurrency, CLI, and recovery tests pass.
19. platform implementation is committed with a clean tree.
20. completion report includes commands, hashes, event samples, failure codes, concurrency results, recovery results, skips, deviations, and exact repository states.

## Dependencies and Blockers

Satisfied:

- canonical vault bootstrap complete;
- deterministic validator complete;
- staging root and dry-run path confinement complete;
- staging contains only its boundary README;
- platform and vault are clean.

Stop and report when:

- create-new semantics cannot be made race-resistant with the available implementation approach;
- the pre-write log cannot be durably flushed before mutation;
- safe parent creation requires a broader filesystem API;
- recovery cannot distinguish no-file, matching-file, and mismatched-file states;
- implementation would need canonical access;
- tests require unapproved ACL or security-policy changes.

Do not weaken the contract to avoid a blocker.

## Rollback and Recovery

Platform code rolls back through Git revert.

Runtime test artifacts must be created under packet-owned fixture paths inside staging and may be removed only when ownership is explicit and no unrelated content exists.

The staging boundary README must remain.

Never alter canonical vault content during rollback.

## Completion Report

```text
Status: complete
Platform repository and commit: C:\dev\kaizen\platform @ 36fa419
Vault status and commit: clean main @ 3de6042
Staging artifacts and audit log:
- C:\dev\kaizen\staging\README.md
- C:\dev\kaizen\staging\staging-write-attempts.jsonl
- C:\dev\kaizen\staging\projects\kaizen-platform\claims\create-only-wrapper-smoke.md
Files created:
- src/kaizen/windows_fs.py
- src/kaizen/staging_write.py
- src/kaizen/stage_create_cli.py
- tests/test_windows_fs.py
- tests/test_staging_write.py
- tests/test_stage_create_cli.py
- tests/fixtures/staging-write/valid-request.json
Files modified:
- AGENTS.md
- README.md
- pyproject.toml
- src/kaizen/__init__.py
Request/response models: immutable typed StagingWriteRequest and StagingWriteResult plus stable StagingWriteError
Failure codes: invalid_request, invalid_provenance/search evidence failures through typed request validation, content_too_large, content_hash_mismatch, frontmatter_request_mismatch, agent_authority_forbidden, existing path-confinement codes, prewrite_audit_failed, destination_exists, create_new_failed, persisted_hash_mismatch, validation_failed, idempotency_conflict, recovery_required, recovery_hash_mismatch, audit_terminal_append_failed, internal_failure, and native handle/mutex failure codes
Commands run:
- editable package install
- full pytest suite
- compileall
- pip check
- real two-process kaizen-stage-create smoke execution
- independent staging-mode validation
- SHA-256 and JSONL audit-chain verification
- cached diff, scope, text-integrity, junk-file, and Git-state checks
Tests and concurrency results: 126 passed, 0 skipped; native exclusive-create race passed; real two-process smoke produced one physical create and one idempotent replay with both processes exiting 0
Recovery scenarios: intent/no-file retry, create-before-validation recovery, validation-before-terminal-event recovery, matching-file recovery, mismatched-file fail-closed recovery, terminal-audit failure, and committed-file mismatch cases covered
CLI exit checks: 0 for created and idempotent prior success; governed rejection 1 and configuration/audit/recovery/internal failure 2 covered by tests
Representative hashes and event IDs:
- SHA-256: d6033bacf6325abf40082fdca9bf7376888e6d7d1380bca966d3fdbc8e60d42e
- intent event: 4696cfe3-ef72-4bc1-82fb-7a1f8deb7dba
- committed event: 5e6674cd-1f62-405a-9a3f-6c34d848d030
- validation run: 22dff208-44b9-4c82-a539-01fb2d200676
Acceptance criteria result: all 20 satisfied
Skipped tests with reasons: none; Developer Mode enabled full Windows symlink coverage
Deviations: none requiring doctrine changes
Unresolved issues: canonical promotion remains unimplemented; wrapper remains human-run with no Hermes or MCP exposure
Contract findings for kaizen-docs: race-resistant create-only staging writes required a Windows handle-relative FILE_CREATE primitive plus a named cross-process mutex; Python path prechecks alone were insufficient
Recommended next task: draft and security-audit Task Packet 006 for human-operated canonical promotion and recoverable promotion events; do not implement it without owner approval
Final Git statuses: docs clean before closeout; platform clean at 36fa419; vault clean at 3de6042; staging contains only the boundary README, append-only attempt log, and one validated smoke claim
```

This milestone authorizes only the human-run create-only staging boundary. It does not authorize canonical promotion, overwrite/edit/delete operations, Hermes, MCP, or a general filesystem surface.
