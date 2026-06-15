---
id: kz-spec-01KXPROJECTBOOTSTRAPSPEC0001
type: spec
status: proposed
project: kaizen-platform
created: 2026-06-15T09:25:00-04:00
updated: 2026-06-15T09:25:00-04:00
review_status: pending
authority: proposed
related_decisions:
  - kz-dec-01KXPROJECTBOOTSTRAP0017
summary: "Specify immutable planning, atomic Windows publication, evidence, status, recovery, and concurrency for governed three-note project bootstrap."
---

# Atomic Governed Project Bootstrap Specification

## Goal

Provide one deterministic, human-approved `bootstrap-project` operation that creates a previously absent canonical project root and atomically publishes exactly:

```text
command-center.md
overview.md
current-state.md
```

The operation must preserve Decision 0017's complete-project invariant, fixed-root confinement, exact plan binding, append-only evidence, fail-closed recovery, Windows reparse safety, and local Git checkpoint discipline.

## Scope

This specification covers:

- staging inputs for the three root notes;
- bundle validation and deterministic hashing;
- immutable planning;
- human approval;
- exact execution confirmation;
- hidden prepared-directory creation beneath canonical `projects/`;
- atomic no-replace publication of the complete prepared directory;
- bundle-level intent and committed events;
- read-only status;
- recovery of nonterminal operations;
- idempotent retry after verified commitment;
- stale-binding, concurrency, untracked-state, and Windows filesystem controls;
- Kaizen MCP tools for plan, approve, execute, status, and recover;
- local vault commit boundary after successful execution.

## Non-Goals

- generic directory creation;
- direct Go8 canonical writes;
- bootstrap of partial project roots;
- migration of the grandfathered `kaizen-platform` project;
- earned-folder creation beyond existing Decision 0008 behavior;
- amendments to existing root notes;
- deletion, rollback, or broad cleanup of canonical evidence;
- repository remote creation or push;
- Phase 5 Northstar amendment behavior;
- production service deployment.

## Requirements

### 1. Operation identity and fixed roots

The operation uses a fresh Kaizen operation ID and packet ID.

All inputs and outputs are confined to the configured fixed roots:

```text
staging root:
C:\dev\kaizen\staging

canonical root:
C:\dev\kaizen\vault

platform root:
C:\dev\kaizen\platform
```

The canonical project destination is derived only from the validated project slug:

```text
projects/<project-slug>
```

No arbitrary destination path is accepted from an MCP caller.

### 2. Required staging inputs

The planner accepts exactly three staging-relative paths:

```text
command_center_relative_path
overview_relative_path
current_state_relative_path
```

Each path must:

- resolve beneath the fixed staging root;
- identify a regular UTF-8 Markdown file;
- contain no symlink, junction, mount point, alternate data stream, traversal, absolute path, or forbidden reparse segment;
- remain byte-identical from planning through execution;
- pass the authoritative note-type validator.

### 3. Candidate normalization

Normalization uses the accepted note-type contracts.

For command-center, overview, and current-state:

```text
review_status: not-required
authority: none
```

No review-status or authority transition is invented for these note types.

Normalization may update only fields explicitly required by the bootstrap contract, including an approved-at `updated` timestamp when the existing promotion conventions require it. The exact normalized bytes and SHA-256 of each note are frozen in the plan.

The source bytes remain preserved separately from normalized canonical candidate bytes.

### 4. Individual validation

Each normalized candidate must pass its note-type contract with zero errors.

Warnings are permitted only when the existing validator classifies them as nonblocking and the immutable plan records them. The Northstar bootstrap requires zero warnings.

### 5. Cross-note validation

Cross-note validation produces a deterministic ordered finding list.

Hard failures include:

```text
bootstrap_project_mismatch
bootstrap_duplicate_note_id
bootstrap_note_type_mismatch
bootstrap_destination_mismatch
bootstrap_pipeline_stage_mismatch
bootstrap_source_boundary_conflict
bootstrap_do_not_touch_conflict
bootstrap_repository_checkpoint_conflict
bootstrap_required_section_conflict
bootstrap_project_root_already_exists
bootstrap_project_slug_invalid
bootstrap_casefold_collision
bootstrap_candidate_validation_failed
```

Checks must prove:

- all three `project` values are the same normalized slug;
- note IDs are unique and use the correct type prefixes;
- note types are exactly command-center, overview, and current-state;
- destinations are exactly the three required root filenames;
- command-center and current-state use the same initial `pipeline_stage`;
- repository checkpoints do not contradict each other where stated;
- source-of-truth and do-not-touch boundaries are compatible;
- no candidate claims that the project is already canonically present;
- the canonical project root is absent, including case-insensitive aliases;
- there is no untracked or ignored entry at the target project-root name;
- `projects/` exists and is a verified non-reparse directory.

Findings sort by code, then note role, then field/path.

### 6. Deterministic bundle manifest and hash

The planner creates an immutable bundle manifest encoded as canonical UTF-8 JSON:

- object keys sorted lexicographically;
- separators `,` and `:` with no insignificant whitespace;
- LF only;
- Unicode emitted directly, not ASCII-escaped unless required by JSON;
- arrays retained in this exact role order:
  1. command-center
  2. overview
  3. current-state.

Minimum manifest fields:

```text
schema_version
action = bootstrap-project
operation_id
packet_id
project_slug
project_root_relative_path
canonical_root_identity
platform_git_binding
vault_git_binding
required_governance_log_sha256
projects_directory_identity
project_root_expected_state = absent
notes[]:
  role
  note_id
  note_type
  source_relative_path
  source_sha256
  canonical_relative_path
  canonical_candidate_sha256
  validation_run_id
cross_note_validation_sha256
```

The bundle SHA-256 is the SHA-256 of the exact canonical JSON manifest bytes.

The plan SHA-256 is calculated using the existing immutable-plan convention after the bundle manifest SHA-256 and all live bindings are present.

### 7. Immutable plan

The planner writes create-new operation evidence beneath a dedicated bootstrap operation directory.

Required files include:

```text
plan.json
bundle-manifest.json
cross-note-validation.json
command-center-candidate.md
overview-candidate.md
current-state-candidate.md
review-diff.md or deterministic per-note review artifacts
```

The plan records:

- exact source and normalized candidate hashes;
- exact bundle and validation hashes;
- exact project-root expected absence;
- canonical root and `projects/` directory identities;
- vault branch, HEAD, clean state, and remote posture;
- platform branch, HEAD, clean state, and remote posture;
- governance-log SHA-256;
- packet binding;
- created timestamp;
- review context.

Planning performs no canonical mutation.

### 8. Approval

Approval requires:

- exact operation ID and packet ID;
- exact expected plan SHA-256;
- valid human actor;
- explicit approval basis;
- review binding for all three normalized candidates and cross-note findings.

Approval evidence is create-new and immutable.

The approving actor must equal the executing actor.

### 9. Exact execution confirmation

Live execution requires a dedicated confirmation literal distinct from ordinary promotion, for example:

```text
BOOTSTRAP-PROJECT-LIVE-EXACTLY-ONCE
```

The exact literal becomes part of the public tool contract and tests.

### 10. Prepared-directory publication design

Execution uses one hidden operation-owned sibling directory beneath canonical `projects/`:

```text
projects/.<operation-id>.bootstrap.tmp
```

The temporary name is derived only from the operation ID and validated as a single leaf name.

Safe execution order under the canonical-root mutex:

1. reload and verify plan, validation, bundle manifest, candidates, and approval;
2. verify live Git, platform, governance-log, source, candidate, ancestor, and target-absence bindings;
3. verify no terminal or nonterminal event history conflicts;
4. append the bundle-level intent event;
5. create the hidden prepared directory with create-new semantics beneath a verified `projects/` handle;
6. create exactly the three candidate files inside it with create-new semantics;
7. flush and reopen each file; verify exact SHA-256 and file identity;
8. verify the prepared directory contains exactly the three approved filenames and no extras;
9. atomically rename the prepared directory to `<project-slug>` using native no-replace directory rename relative to the verified `projects/` handle;
10. reopen the installed project directory by handle and verify directory identity, non-reparse posture, exact inventory, and all three file hashes;
11. append the committed event;
12. return the committed result.

The atomic visibility boundary is step 9.

No note becomes visible at the final canonical project path before the complete prepared directory is published.

### 11. Windows filesystem primitives

The implementation extends the existing native Windows layer with bounded directory-specific primitives rather than using broad `Path.mkdir` and `Path.rename` calls.

Required primitives include:

```text
create_new_relative_directory
open_existing_relative_directory
rename_relative_directory_no_replace
enumerate_relative_directory_exact
verify_directory_identity
```

Primitives must:

- operate relative to verified handles;
- open reparse points themselves rather than following them;
- reject every reparse-tagged directory or file in the controlled path;
- use case-insensitive Windows collision semantics;
- reject colon, NUL, separators, dot segments, trailing spaces/dots, reserved device names, and invalid project slugs;
- share access only as required by the proven state machine;
- return stable volume and file-index identities;
- distinguish collision, missing path, sharing violation, access denial, and native rename failure.

The directory rename must be native, same-volume, and no-replace.

### 12. Intent event

The intent event is appended before the prepared directory is created so any operation-owned filesystem residue has prior governed intent evidence.

Minimum fields:

```text
schema_version
event_id = operation_id
operation_id
packet_id
event_phase = intent
action = bootstrap-project
project_slug
project_root_relative_path
bundle_sha256
plan_sha256
approval_sha256
approved_by
approved_at
note records with IDs, roles, destinations, and hashes
projects_directory_identity
project_root_expected_state = absent
prepared_directory_leaf
basis
```

### 13. Committed event

The committed event is appended only after the published directory and all three files verify.

Minimum additional fields:

```text
intent_event_id
installed_project_root_identity
installed_project_root_volume_identity
installed_note identities and sizes
verified note SHA-256 values
verified exact directory inventory
published_bundle_sha256
```

A committed event with missing or mismatched canonical state is `repository_blocked`, not success.

### 14. Status model

Read-only status returns structured state without mutation.

States include:

```text
absent
planned
approved
intent-recorded
prepared-empty
prepared-partial
prepared-complete
published-uncommitted
committed
recovery-required
blocked
```

Status includes:

- plan/approval/event presence and hashes;
- live Git and governance-log posture;
- project-root presence and identity;
- prepared-directory presence and exact inventory;
- per-note source, candidate, prepared, and canonical hash matches;
- recommended next action;
- mutation performed: false.

### 15. Recovery matrix

Recovery runs under the canonical-root mutex and revalidates all immutable bindings.

Required cases:

#### No intent, no prepared directory, no project root

Return planned or approved state. No mutation.

#### Intent present, no prepared directory, no project root

Recovery may recreate the exact prepared bundle and continue only when approval and all live bindings remain exact.

#### Prepared directory empty or partial, project root absent

Recovery verifies operation ownership and exact allowed inventory. It may create only missing approved files or replace no files. Any unexpected file, mismatched approved file, reparse point, or identity conflict blocks recovery.

#### Prepared bundle complete, project root absent

Recovery may verify and atomically publish it.

#### Prepared directory present and project root present

Blocked unless exact evidence proves a narrow crash state that can be classified without deletion or overwrite. No competing root is replaced.

#### Project root complete, committed event absent

Recovery verifies exact directory identity, inventory, candidate hashes, plan, approval, and intent. It may append the committed event only when every binding is exact.

#### Project root partial or mismatched

Blocked for human investigation. Recovery must not complete, overwrite, merge, or delete it.

#### Committed event present

Return idempotent committed only when canonical state exactly matches. Otherwise return repository blocked.

Recovery never broadly deletes the prepared directory or canonical project root. Cleanup requires a later explicit evidence-preserving policy.

#### Recovery live-scope exception

Normal planning and execution require clean platform and vault repositories.

After an intent event, a crash may leave the vault intentionally dirty because the append-only governance log changed and an operation-owned prepared or published project directory exists. Recovery must therefore use a narrow, operation-bound live-scope verifier rather than the ordinary clean-repository gate.

The recovery exception may tolerate only:

```text
an append to the exact governance log consistent with this operation's intent evidence
projects/.<operation-id>.bootstrap.tmp and its exact approved inventory
projects/<project-slug> only when its exact approved bundle state is recoverable
```

It must reject:

- any unrelated tracked, untracked, ignored, deleted, renamed, or staged path;
- any unexpected file within the prepared or canonical project directory;
- any governance-log bytes not explained by valid append-only operation evidence;
- platform repository dirtiness;
- source or candidate drift;
- another nonterminal operation for the same project.

The verifier must return the exact allowed and observed dirty-path sets in status and recovery evidence. This is not a generic dirty-repository bypass.

### 16. Concurrency

The canonical-root mutex serializes the entire live bootstrap state machine.

Required outcomes:

- same project, same bundle contenders: exactly one commit; loser returns idempotent committed or a deterministic conflict;
- same project, different bundle contenders: exactly one commit; loser fails with project conflict or stale approval;
- different project contenders: both may succeed serially without cross-contamination;
- stale pre-created plans fail after any vault HEAD, governance-log, platform HEAD, project-root, `projects/` identity, source, candidate, or bundle drift;
- a nonterminal same-project operation blocks a new operation until recovery classification;
- an externally created empty directory, file, ignored path, case alias, or reparse object blocks execution.

At least 100 same-project conflicting rounds and 100 different-project rounds are required in the hammer suite.

### 17. Git boundary

The bootstrap operation mutates canonical files and the governance log but does not run Git commands.

After committed execution, the steward must:

1. inspect exact vault changes;
2. verify only these paths changed:
   - `projects/<slug>/command-center.md`
   - `projects/<slug>/overview.md`
   - `projects/<slug>/current-state.md`
   - the append-only governance log;
3. stage exact paths;
4. commit locally;
5. verify clean tree and no remotes.

A dirty vault blocks planning and execution under existing live-scope rules.

### 18. Kaizen MCP surface

Add explicit typed tools:

```text
kaizen_plan_project_bootstrap
kaizen_approve_project_bootstrap
kaizen_execute_project_bootstrap
kaizen_project_bootstrap_status
kaizen_recover_project_bootstrap
```

The tools accept no arbitrary canonical destination paths.

They expose exact packet, operation, actor, plan-hash, confirmation, and staging-path fields only.

Generic execution remains disabled.

### 19. Northstar binding

The first live bundle after implementation uses:

```text
project_slug:
northstar-stockroom

command-center source SHA-256:
768ea1b6f28975ed3a1db1d1a452f5b9ebda14c0279a2f2262a790b955baf150

overview source SHA-256:
0c507c393900c64e6b600ff756387d387a307ba5cdafd16f5e768bf7c02465f7

current-state source SHA-256:
6da2149fdda7e09123d3d3c5e7350937fa221cd30581bb40f53f65761250afbb
```

These hashes are planning evidence, not permanent authorization. Any candidate change requires fresh validation, manifest, plan, and approval.

## Constraints

- Windows-first local implementation;
- Python 3.11 compatibility;
- standard library plus existing project dependencies only;
- no network, database, service, remote, credentials, or deployment;
- no canonical mutation during tests except disposable fixed-root live fixtures;
- no weakening of existing promotion, amendment, Git cleanliness, or path-confinement controls;
- no reuse of retired operation IDs or plans;
- no direct Go8 canonical bootstrap;
- no broad cleanup or inferred approval.

## Acceptance Criteria

1. The three individually valid root-note candidates produce one deterministic bundle manifest and plan.
2. Planning performs no canonical mutation.
3. Exact plan-hash approval is required.
4. Execution publishes no partial final project root.
5. The final project root appears atomically with exactly three approved notes.
6. Intent precedes any prepared-directory residue.
7. Committed evidence follows complete canonical verification.
8. Every defined crash state is deterministically classified.
9. Recovery never guesses, overwrites unexpected content, or broadly deletes evidence.
10. Same-project concurrency commits exactly one approved bundle.
11. Different-project contention remains safe.
12. Empty/untracked directories and case/reparse aliases are detected beyond Git state.
13. Existing promotion and amendment tests remain green.
14. Full platform tests, compile checks, available lint, security regressions, and live connector tests pass.
15. Kaizen MCP exposes only the five explicit bootstrap tools.
16. The platform repository is committed locally, clean, and has no remotes.
17. After MCP restart, the unchanged Northstar candidates can produce an immutable plan without canonical mutation.

## Open Questions

No doctrine question remains open.

Implementation must still prove, not assume:

- the exact native directory-create and no-replace rename flags;
- directory enumeration and identity behavior under Windows sharing and antivirus interference;
- the exact event-schema compatibility strategy;
- whether bundle operation evidence reuses the promotion operation root or uses a dedicated `_bootstrap/operations` root.

The implementation packet must choose one evidence-root design and test backward compatibility. A dedicated bootstrap operation root is preferred to avoid overloading single-note promotion semantics.
