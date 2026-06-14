---
id: kz-tp-01KX4D7PROJECTDIR0000000000
type: task-packet
status: proposed
project: kaizen-platform
created: 2026-06-14T21:05:00Z
updated: 2026-06-14T21:05:00Z
review_status: pending
authority: proposed
summary: "Allow one approved first promotion to create its exact missing canonical project directory without generic mkdir authority."
---

# Task Packet 014D - Governed First-Project Directory Creation

## Objective

Add the smallest safe capability required for first promotion of a new canonical project root.

The current planner can approve a destination such as:

```text
projects/northstar-stockroom/overview.md
```

but execution fails when `projects/northstar-stockroom/` does not already exist:

```text
destination_path_invalid: Approved destination parent does not exist
```

The correction must allow the approved first-promotion operation itself to create exactly one missing direct child directory beneath `projects/`, while preserving fixed-root confinement, exact plan binding, reparse-point rejection, recovery safety, and no generic directory-creation surface.

## Triggering evidence

```text
Packet 014A Phase 4
approved operation:
kz-prom-01KX4E70000000000000000001

approved plan SHA-256:
913105abba8bf89b5b57d1f57920fe42d10558b3de9c59abc99ccf7a1cf3dbbd

execution result:
destination_path_invalid

canonical intent event:
none

canonical write:
none

governance-log mutation:
none
```

The approved operation is retired and must not be reused after platform changes.

## Read First

```text
src/kaizen/promotion_plan.py
src/kaizen/promotion_workflow.py
src/kaizen/promotion_events.py
src/kaizen/windows_fs.py
src/kaizen/path_confinement.py
tests/test_promotion_workflow.py
tests/test_live_promotion.py
01-project-standard/kaizen-project-standard-v0.2.md
04-design-decisions/0008-v0.2-operating-conventions.md
06-handoff-patterns/006b-implement-human-operated-first-promotion.md
03-research-results/209-packet-014c-implementation-completion.md
```

## Scope

Allowed changed paths:

```text
src/kaizen/promotion_plan.py
src/kaizen/promotion_workflow.py
src/kaizen/promotion_events.py
src/kaizen/windows_fs.py
tests/test_promotion_workflow.py
tests/test_live_promotion.py
```

A listed file may remain unchanged when unnecessary.

## Non-Scope

- no generic mkdir tool;
- no direct Go8 write beneath the canonical vault;
- no arbitrary parent creation outside `projects/<project-slug>/`;
- no recursive earned-folder creation;
- no schema weakening;
- no amendment behavior change;
- no command-center or current-state content change;
- no approval bypass;
- no execution of the retired overview operation;
- no canonical promotion during implementation of this packet.

## Required Design

### Plan binding

When planning a first promotion, record explicit destination-parent posture in the immutable plan:

```text
destination_parent_path
parent_existed_at_plan
parent_creation_authorized
```

`parent_creation_authorized` may be true only when all conditions hold:

1. destination is exactly `projects/<project-slug>/<root-note>.md`;
2. `<root-note>` is one of:
   - `command-center.md`
   - `overview.md`
   - `current-state.md`;
3. `projects/` exists as a real directory beneath the canonical root;
4. the exact project directory is absent at planning time;
5. no ancestor is a symlink, junction, mount point, or other forbidden reparse object;
6. the plan remains a first-time promotion with absent destination.

Any deeper or unrelated missing parent must fail closed.

### Execution

Under the canonical promotion mutex:

1. reverify the approved plan, live binding, governance log, source, candidate, and approval;
2. reverify `projects/` and all ancestors through safe handles;
3. if the approved parent was absent at plan time and remains absent, create exactly that one project directory;
4. reject if the parent now exists as a symlink, junction, mount point, or mismatched object;
5. append intent evidence that records whether the parent existed or was created by the operation;
6. continue the existing temp-file, atomic-install, identity, hash, and committed-event protocol;
7. return committed only after destination and terminal event verification.

No unapproved directory may be created.

### Recovery

Recovery must safely classify:

```text
approved parent created, destination absent, intent present
approved parent created, canonical candidate present, committed absent
parent replaced by reparse object
unexpected pre-existing content in the project directory
terminal event with missing canonical destination
```

An empty project directory created by the operation may remain as preserved evidence after a failed attempt; recovery must not perform broad deletion. The directory must not be treated as authority to create unrelated files.

## Security Requirements

- exact fixed canonical root only;
- exact normalized destination path from the immutable plan;
- direct child of canonical `projects/` only;
- no path traversal, absolute path, UNC path, alternate data stream, symlink, junction, mount point, or case-folding escape;
- no TOCTOU widening between validation and creation;
- no creation before exact approval and execution confirmation;
- no destination overwrite;
- no generic filesystem capability exposed through Kaizen MCP;
- failed or stale plans remain immutable evidence and are never rewritten.

## Validation

Required tests:

1. first promotion to missing `projects/<slug>/overview.md` creates the exact project directory and commits successfully;
2. current-state and command-center root-note destinations receive the same bounded behavior;
3. missing parent outside the direct project-root pattern fails;
4. missing deeper earned folder fails;
5. project directory junction/symlink/mount-point substitution fails;
6. `projects/` reparse substitution fails;
7. unexpected pre-existing content does not widen allowed writes;
8. two contenders cannot create divergent project roots or duplicate canonical writes;
9. retry after committed event is idempotent;
10. approved plan becomes stale on platform, vault, governance-log, source, or parent-state drift;
11. existing first-promotion and amendment suites remain green;
12. no operation event or canonical write occurs when the parent contract fails.

Run:

```text
focused promotion and recovery tests
live-promotion tests
full platform suite
compile checks
available lint checks
```

## Acceptance Criteria

- new-project overview promotion can be planned, approved, and executed through Kaizen MCP without manual vault directory creation;
- only the exact direct project directory is created;
- plan and event evidence state whether parent creation was authorized and performed;
- no generic mkdir or direct canonical workaround exists;
- exact changed paths remain within packet scope;
- platform repository is clean, local-only, and committed;
- Kaizen MCP is restarted before Phase 4 resumes;
- fresh promotion IDs and plan approval are required after the correction.

## Completion Report

Return:

```text
starting commit
ending commit
exact changed paths
root cause
plan schema additions
execution and recovery behavior
focused tests
full tests
compile/lint results
security regressions
clean-state and remotes
known limitations
```

## Authority Boundary

Drafting and auditing Packet 014D do not authorize platform source changes. Separate owner approval bound to the exact packet SHA-256 is required.
