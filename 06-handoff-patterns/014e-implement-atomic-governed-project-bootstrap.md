---
id: kz-tp-01KXPROJECTBOOTSTRAP014E000000
type: task-packet
status: revised-pending-approval
project: kaizen-platform
created: 2026-06-15T09:40:00-04:00
updated: 2026-06-15T12:50:00-04:00
previously_approved: 2026-06-15T10:20:00-04:00
previously_approved_by: owner.local
previously_approved_source_sha256: a5b89ddef8c33ed94a5af7c95efacea8ee8283dab56ddd2a6e70bc35e1fc114b
previously_approved_platform_starting_commit: 7cbc132a508071c94e68fcd5bd206b19ac8bd61a
revision_reason: "Implementation preflight proved that governance-log event routing requires promotion_events.py, which was omitted from the approved path scope. No source change occurred."
reapproved: 2026-06-15T11:05:00-04:00
reapproved_by: owner.local
reapproved_source_sha256: e50f4ee29b1b5ea289c3176af78ebdfb7d0a10cd336ccf0b0b443c25d3e5406c
reapproved_platform_starting_commit: 7cbc132a508071c94e68fcd5bd206b19ac8bd61a
second_revision_reason: "Kaizen MCP contract tests proved that tests/test_server_tools.py hard-codes the pre-bootstrap 14-tool registry count and must be updated to 19. No edit to that unapproved path occurred."
review_status: pending
authority: proposed
primary_spec: kz-spec-01KXPROJECTBOOTSTRAPSPEC0001
related_specs:
  - kz-spec-01KXPROJECTBOOTSTRAPSPEC0001
summary: "Implement and hammer-test the Decision 0017 atomic three-note project-bootstrap operation and explicit Kaizen MCP surface."
---

# Task Packet 014E - Implement Atomic Governed Project Bootstrap

## Objective

Implement the accepted Decision 0017 `bootstrap-project` operation so a previously absent canonical project root can be created only by one immutable, human-approved bundle containing exactly:

```text
command-center.md
overview.md
current-state.md
```

The implementation must publish the complete project directory through one Windows-native no-replace atomic visibility boundary, preserve append-only intent and committed evidence, support deterministic read-only status and bounded recovery, and expose only explicit typed Kaizen MCP tools.

## Read First

```text
04-design-decisions/0017-atomic-governed-project-bootstrap.md
03-research-results/213-decision-0017-owner-acceptance.md
03-research-results/214-packet-014a-phase-4-atomic-bootstrap-reconciliation.md
05-specs/atomic-governed-project-bootstrap-spec.md
01-project-standard/kaizen-project-standard-v0.2.md
04-design-decisions/0008-v0.2-operating-conventions.md
06-handoff-patterns/014a-controlled-implementation-return-pilot-execution.md
03-research-results/211-packet-014d-independent-audit-and-project-bootstrap-reconciliation.md
```

Inspect before editing:

```text
C:\dev\kaizen\platform\src\kaizen\windows_fs.py
C:\dev\kaizen\platform\src\kaizen\atomic_install.py
C:\dev\kaizen\platform\src\kaizen\promotion_plan.py
C:\dev\kaizen\platform\src\kaizen\promotion_workflow.py
C:\dev\kaizen\platform\src\kaizen\promotion_events.py
C:\dev\kaizen\platform\src\kaizen\promotion_recovery.py
C:\dev\kaizen\platform\src\kaizen\promotion_scope.py
C:\dev\kaizen\platform\src\kaizen\validation.py
C:\dev\kaizen\platform\src\kaizen\contracts.py
C:\dev\kaizen\platform\src\kaizen\ids.py
C:\dev\kaizen\platform\schemas\promotion-event.schema.json
C:\dev\kaizen-mcp\src\kaizen_mcp\adapter.py
C:\dev\kaizen-mcp\src\kaizen_mcp\server.py
```

## Starting Checkpoints

Implementation must stop if the repositories do not match the owner-approved packet checkpoint recorded at approval time.

Current planning checkpoints are:

```text
kaizen-platform:
7cbc132a508071c94e68fcd5bd206b19ac8bd61a
branch main
clean
no remotes

kaizen-docs:
13bb5d52ec3dd2a1ac47e2680078eb48ca07edf6
branch main
clean
origin/main synchronized

kaizen-vault:
2487de669bc44ed50e54fd5dbbfdd128ce659dbb
branch main
clean
no remotes

Northstar:
bf9abb513d6f22fc5ed800301a8b3ba0b75c5189
branch main
clean
no remotes
```

The final owner approval must bind updated docs HEAD and the exact Packet 014E SHA-256.

## Scope

### Platform allowed changed paths

```text
src/kaizen/project_bootstrap_contracts.py
src/kaizen/project_bootstrap_plan.py
src/kaizen/project_bootstrap_workflow.py
src/kaizen/project_bootstrap_recovery.py
src/kaizen/project_bootstrap_status.py
src/kaizen/project_bootstrap_scope.py
src/kaizen/promotion_events.py
src/kaizen/windows_fs.py
src/kaizen/ids.py
schemas/event-type-prefixes.json
schemas/project-bootstrap-event.schema.json
tests/test_project_bootstrap_plan.py
tests/test_project_bootstrap_workflow.py
tests/test_project_bootstrap_recovery.py
tests/test_project_bootstrap_status.py
tests/test_live_project_bootstrap.py
tests/test_project_bootstrap_hammer.py
tests/test_promotion_events.py
```

### Kaizen MCP allowed changed paths

```text
src/kaizen_mcp/adapter.py
src/kaizen_mcp/server.py
tests/test_project_bootstrap_tools.py
tests/test_server_tools.py
README.md
docs/TOOL_CONTRACTS.md
```

A listed path may remain unchanged when unnecessary. Any additional path requires a reviewed packet revision before editing.

## Non-Scope

- Packet 014D implementation;
- modification or reuse of retired promotion operations;
- direct canonical Northstar writes;
- ordinary promotion or amendment semantic changes;
- generic directory creation;
- generic command execution;
- Go8 capability changes;
- Phase 5 Northstar amendment behavior;
- Postgres, Qdrant, LangGraph, Hermes, service deployment, remote creation, or push;
- cleanup or deletion of failed operation evidence;
- migration of the grandfathered `kaizen-platform` canonical project root.

## Implementation Requirements

### 1. Dedicated modules and operation root

Implement project bootstrap as a dedicated operation family rather than embedding bundle semantics into single-note promotion.

Use a dedicated staging evidence root such as:

```text
_bootstrap/operations/<operation-id>/
```

Do not overload `_promotion/operations` unless concrete compatibility evidence proves that doing so is safer. Packet 014E prefers a dedicated root.

### 2. Dedicated event schema, routing, and ID prefix

Add a dedicated project-bootstrap event schema and event ID prefix.

Update `promotion_events.py` only to route `action: bootstrap-project` records to the dedicated bootstrap validator while preserving the existing `promote` and `amend` validation path unchanged. The shared governance log must remain readable when historical promotion/amendment events and new bootstrap events coexist.

The existing promotion-event schema and historical promotion events must remain byte-compatible and valid.

Event actions and phases must be explicit:

```text
action: bootstrap-project
event_phase: intent | committed
```

### 3. Deterministic contract implementation

Implement the exact canonical JSON bundle-manifest serialization and ordered cross-note findings in the specification.

No dict insertion-order accident, filesystem enumeration order, locale, clock-derived business rule, or nondeterministic temporary value may alter hashes.

### 4. Candidate handling

Reuse authoritative parsing, note-type contracts, and individual validation without duplicating policy.

Implement bundle-specific consistency checks in the dedicated bootstrap contract layer.

The planner accepts exactly the three staging-relative paths and derives all canonical destinations.

### 5. Windows-native prepared-directory primitives

Extend `windows_fs.py` only with the bounded directory primitives required by the spec:

```text
create_new_relative_directory
open_existing_relative_directory
rename_relative_directory_no_replace
enumerate_relative_directory_exact
verify_directory_identity
```

Use handle-relative Windows native calls, no-replace rename, same-volume verification, reparse rejection, final identity checks, and create-new semantics.

Do not expose these primitives through MCP or as generic user-facing filesystem tools.

### 6. Execution ordering

Under the canonical-root mutex:

```text
verify immutable evidence and live scope
verify no prior conflicting history
append intent
create hidden prepared directory
write and fsync exact three files
verify prepared inventory and hashes
atomically rename directory no-replace
verify installed directory and files
append committed event
return committed
```

Injectable fault hooks must cover every state boundary needed by recovery tests.

### 7. Recovery

Implement every recovery class in the specification.

Recovery may complete only from exact immutable evidence. Unexpected content, mismatched files, project-root aliases, reparse objects, partial canonical roots, or committed-event disagreement must block.

Do not implement broad deletion. Preserve prepared residue as evidence unless a later approved cleanup policy exists.

### 8. Operation-bound recovery scope

Implement a dedicated bootstrap live-scope layer.

Planning and execution retain the ordinary clean platform/vault requirement.

Recovery may proceed through an intentionally dirty vault only when the complete dirty-path set is exactly explained by this operation's valid intent evidence, governance-log append, operation-owned prepared directory, or exact recoverable canonical project root. Any unrelated staged, tracked, untracked, ignored, deleted, renamed, or unexpected nested path blocks recovery.

Platform dirtiness always blocks recovery.

The recovery scope result must record expected and observed dirty paths and the evidence that authorizes each tolerated path. Do not weaken the shared ordinary promotion scope globally.

### 9. Status

Status is read-only and must work when mutation is blocked by dirty Git state.

It returns the structured states and per-note/bundle bindings defined in the specification.

It must not create operation directories, events, temporary files, locks beyond read-only inspection needs, or canonical state.

### 10. MCP tools

Expose exactly:

```text
kaizen_plan_project_bootstrap
kaizen_approve_project_bootstrap
kaizen_execute_project_bootstrap
kaizen_project_bootstrap_status
kaizen_recover_project_bootstrap
```

Schemas must be explicit and narrow.

Plan inputs:

```text
packet_id
operation_id
project_slug
command_center_relative_path
overview_relative_path
current_state_relative_path
```

Approval inputs:

```text
packet_id
operation_id
expected_plan_sha256
human_actor
approval_basis
```

Execution inputs:

```text
packet_id
operation_id
expected_plan_sha256
human_actor
confirmation
```

Recovery inputs must include exact packet, operation, actor, plan hash, and a dedicated recovery confirmation literal when mutation can occur.

No canonical destination path argument is exposed.

### 11. Backward compatibility

Existing promotion, amendment, staging, validation, recovery, and operator-console tests must remain green.

Existing schemas and event logs must not require migration.

No old operation changes state because the new code exists.

### 12. Northstar dry proof

After implementation, tests may use disposable roots and copied candidate fixtures.

Do not create a live Northstar immutable plan or mutate the live vault during implementation.

After commit and Kaizen MCP restart, Phase 4 will separately create the first live plan using the frozen candidates.

## Constraints

- Python 3.11;
- existing dependencies only;
- Windows-first native filesystem behavior;
- fixed roots only;
- no network or remote;
- no canonical mutation outside disposable test roots;
- no direct edits to canonical Northstar paths;
- exact reviewed file scope;
- no reset, stash, clean, broad delete, or evidence destruction;
- fail closed on unknown state;
- use Go8 for development and Kaizen MCP only for later governed live operations;
- Go7 is not part of the workflow.

## Validation

### Hard-gate deterministic tests

- exact three-role input requirement;
- individual note validation;
- cross-note project, ID, type, filename, stage, boundary, and checkpoint consistency;
- canonical JSON bundle hash stability;
- candidate/source drift;
- exact plan and approval binding;
- invalid project slug, case-folding collision, reserved names, trailing spaces/dots, ADS, traversal, UNC, and absolute paths;
- existing target project root, empty target directory, ignored/untracked target entry, and case alias;
- missing or reparse `projects/` directory;
- unknown contract and malformed evidence fail closed.

### Hard-gate Windows integration tests

- create hidden prepared directory relative to verified `projects/` handle;
- exact three-file inventory;
- directory no-replace atomic rename;
- directory/file identity preservation and final-path verification;
- project-root junction, symlink, mount-point, and reparse substitution;
- prepared-directory reparse substitution;
- sharing violation, access denial, collision, and antivirus-like lock interference;
- no partial final visibility at every injected crash point.

### Hard-gate recovery tests

- intent with no prepared directory;
- prepared empty, partial, and complete;
- complete canonical root without committed event;
- prepared and canonical both present;
- partial or mismatched canonical root;
- committed event with missing/mismatched root;
- unexpected extra file;
- changed source, candidate, bundle, approval, Git binding, governance log, ancestor identity, or target posture;
- idempotent retry after exact commitment;
- no broad cleanup.

### Hard-gate concurrency hammer

```text
100 rounds: same project, same bundle
100 rounds: same project, conflicting bundle
100 rounds: different projects
```

Prove exactly-once publication, deterministic loser states, no divergent root, no duplicate committed event, no cross-project contamination, and preserved evidence.

### Hard-gate live-scope tests

- dirty platform blocks mutation;
- dirty vault blocks mutation;
- read-only status remains structured;
- stale platform HEAD;
- stale vault HEAD;
- stale governance-log hash;
- stale `projects/` identity;
- externally created empty project directory;
- source/candidate evidence changed;
- packet mismatch;
- actor mismatch;
- wrong confirmation literal;
- no remotes created.

### Full verification

Run:

```text
focused bootstrap tests
existing promotion and amendment tests
full platform pytest suite
compileall
Ruff if available or record exact unavailable environment
Kaizen MCP tests
Go8 health and tool-count verification after server changes
```

## Acceptance Criteria

- Decision 0017 and the specification are implemented without weakening existing controls;
- one complete project root is atomically visible or remains absent;
- bundle evidence is deterministic and append-only;
- all recovery classes are covered;
- all hard-gate tests pass;
- hammer rounds pass with zero unexplained outcomes;
- existing platform tests remain green;
- explicit MCP tools register with the expected schemas;
- generic execution remains disabled;
- platform and Kaizen MCP changes are committed separately and cleanly;
- platform remains local-only with no remotes;
- no live Northstar plan or canonical mutation occurs;
- completion evidence identifies any unavailable lint environment honestly;
- Kaizen MCP restart is required before Phase 4 resumes.

## Completion Report

Return:

```text
starting and ending platform commits
starting and ending Kaizen MCP state or commit posture
exact changed paths by root
root-cause summary
selected evidence-root design
bundle-manifest schema and hash example
new event schema and compatibility result
new Windows primitives and proven behavior
plan/approval/execute/status/recovery tool schemas
focused test results
full platform test results
Kaizen MCP test results
compile/lint results
hammer round totals and outcome distribution
fault-injection and recovery matrix results
live-scope and dirty-state results
working-tree and remote state
known limitations
restart instructions
```

## Stop Conditions

Stop immediately on:

- checkpoint mismatch;
- need to weaken Decision 0017 or existing validation;
- need for generic mkdir or arbitrary canonical paths;
- inability to prove atomic no-partial visibility on the supported Windows filesystem;
- unexplained concurrency outcome;
- recovery requiring guessed approval or broad deletion;
- need to edit an unapproved path;
- live canonical mutation;
- schema incompatibility with historical evidence;
- dirty or remote drift.

## Authority Boundary

Packet drafting and audit do not authorize implementation.

Separate owner approval must bind:

```text
exact Packet 014E SHA-256
exact platform starting commit
exact Kaizen MCP checkpoint
```

After implementation, no live Northstar bootstrap plan may be created until the completion audit passes and Kaizen MCP is restarted.
