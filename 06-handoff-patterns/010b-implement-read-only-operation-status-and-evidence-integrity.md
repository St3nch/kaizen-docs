---
id: kz-tp-01KTPKB379Z4WB0YC26HB1ET68
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-09T16:29:15Z
updated: 2026-06-09T16:29:15Z
review_status: pending
authority: proposed
summary: "Implement read-only platform operation status and bounded evidence-integrity checks without adding any mutation capability."
primary_spec: 05-specs/milestone-6-governed-operator-surface.md
related_specs:
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-hammer-test-strategy.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/promotion-event-schema.md
---

# Task Packet 010B - Implement Read-Only Operation Status and Evidence Integrity

> Packet 010B is a proposed read-only implementation packet. Drafting and auditing it do not authorize implementation. It does not authorize candidate preparation, planning, approval, execution, recovery, MCP work, console work, vault mutation, staging mutation, remote creation, or work under Packets 010C through 010F.

## Objective

Implement the smallest platform-owned read-only inspection slice required by accepted Decision 0014 and the accepted Milestone 6 Governed Operator Surface specification.

The packet must provide:

1. a typed operation-status API for existing promotion and amendment evidence;
2. a read-only CLI that renders that status deterministically;
3. six bounded evidence-integrity checks E-01 through E-06;
4. structured mismatch reporting;
5. fixtures and tests proving zero mutation and no regression.

No new governed mutation action is introduced.

## Authorization boundary

The owner accepted:

```text
Decision 0014 pre-transition SHA-256:
1944972938a72815a5e787f4b74c6851f25e05f2aa59bbe21bac9da1d3dff9a8

Milestone 6 Governed Operator Surface specification pre-transition SHA-256:
7a4e7dd77798a390a5c7596ecd1308209897a98957ca275bc1726fc90ccf16eb
```

and authorized drafting and auditing Packet 010B only.

Packet 010B remains proposed until separately approved at an exact audited SHA-256.

## Read first

### Docs contracts

- `03-research-results/073-decision-0014-and-operator-surface-specification-owner-acceptance.md`
- `04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md`
- `05-specs/milestone-6-governed-operator-surface.md`
- `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/promotion-event-schema.md`

### Platform implementation

- `src/kaizen/amendment_plan.py`
- `src/kaizen/amendment_workflow.py`
- `src/kaizen/promotion_plan.py`
- `src/kaizen/promotion_workflow.py`
- `src/kaizen/promotion_events.py`
- `src/kaizen/promotion_scope.py`
- `src/kaizen/root_config.py`
- `src/kaizen/validation.py`
- `src/kaizen/contracts.py`
- `src/kaizen/errors.py`
- existing promotion and amendment tests.

## Bound state

```text
kaizen-docs accepted roadmap SHA-256:
1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc

kaizen-docs checkpoint before Packet 010B draft:
e243ac87e3eaef0dbd2539ebd30316d655ead66f

kaizen-platform branch:
main

kaizen-platform HEAD:
845d65f356bd684c2f858f36aef54d0344791e43

kaizen-platform message:
Implement governed amendment support

kaizen-platform remote:
none

kaizen-vault HEAD:
e5e4eec1adc4ef26f9e735333dbb229b7bb59368

kaizen-vault remote:
none

full platform suite at inherited checkpoint:
230 passed
```

Any branch, HEAD, cleanliness, root, remote, or accepted-contract mismatch stops implementation before editing.

## Exact implementation scope

### A. Typed operation-status model

Create:

```text
src/kaizen/operation_status.py
```

It must expose a typed immutable result model and one read-only status function for an operation ID.

Required result fields follow the accepted specification:

```text
operation_type
operation_id
packet_id
state
destination
fixed roots
plan/prior/candidate/diff/approval/result hashes
validation run ID
approved actor and timestamp
event chain
Git branch, full HEAD, clean state, expected bindings
execute eligibility
recovery eligibility
structured mismatches
```

Required states:

```text
absent
incomplete
ready
approved
committed
recovered
failed
invalid
```

The implementation must:

- inspect both existing promotion and amendment operation evidence;
- use existing plan, event, root, validation, and Git-binding rules;
- remain backward-compatible with historical persisted event shapes;
- never rewrite, normalize, repair, or delete evidence;
- return `invalid` plus mismatch details rather than guessing when evidence conflicts;
- avoid calling approval, execution, recovery, append-event, atomic-install, atomic-replace, or staging-write functions.

### B. Read-only status CLI

Create:

```text
src/kaizen/operation_status_cli.py
```

Add one project script:

```text
kaizen-operation-status
```

The CLI may accept only:

```text
operation ID
optional packet ID assertion
optional output format: json | text
```

It must:

- load fixed roots through existing root configuration;
- call the read-only status API;
- emit deterministic UTF-8 output;
- return a nonzero exit code for `invalid` or a supplied packet-ID mismatch;
- have no confirmation literal because it cannot mutate;
- expose no path override, actor input, shell command, Git action, repair flag, or mutation option.

### C. Evidence-integrity checks

Create:

```text
src/kaizen/evidence_integrity.py
```

Implement only accepted checks E-01 through E-06.

#### E-01 Active roadmap pointer integrity

Inputs:

- explicitly supplied docs root;
- expected accepted roadmap filename;
- optional expected roadmap SHA-256.

Rules:

- active entrypoint and README pointers must identify exactly one accepted active roadmap snapshot;
- contradictory `proposed`, `not accepted`, or pending labels for that active roadmap fail;
- the check reads only the supplied docs root and must not assume `C:\dev\kaizen-docs` internally.

#### E-02 Current-state versus closure evidence

Inputs:

- explicitly supplied current-state note;
- explicitly supplied accepted closure evidence set.

Rules:

- an accepted closure cannot coexist with current-state text that reports the closed milestone as active or incomplete;
- the result reports exact conflicting paths and phrases;
- no semantic inference beyond registered deterministic phrases or fields.

#### E-03 Duplicate live-state detection

Inputs:

- explicit canonical project subtree;
- target note type or registered live-state role.

Rules:

- detect more than one active current-state record where one is required;
- return all IDs and paths;
- do not delete, merge, select, or rank records.

#### E-04 Task-packet status versus closure evidence

Inputs:

- one task packet;
- explicitly supplied accepted completion/closure evidence;
- optional navigation/current-state files.

Rules:

- detect a completed packet still represented as pending implementation;
- return exact conflicting paths and fields/phrases;
- do not transition status automatically.

#### E-05 Changed-path and staged-scope reporting

Inputs:

- explicit Git repository root;
- exact authorized path set.

Rules:

- report branch, full HEAD, clean state, staged paths, unstaged paths, untracked paths, missing authorized paths, and unexpected paths;
- pass only on exact set equality at the requested gate;
- use read-only Git commands only;
- never stage, reset, clean, stash, commit, push, or alter Git configuration.

#### E-06 Operation evidence and event-chain consistency

Inputs:

- operation ID;
- optional packet ID assertion;
- fixed root configuration.

Rules:

- reuse the operation-status API;
- verify all hashes and bindings;
- verify terminal-event cardinality and result bytes;
- return structured mismatches;
- never recover or repair.

### D. Typed integrity result

The evidence-integrity module must return immutable typed results containing:

```text
check ID
passed boolean
input scope
structured findings
exact paths
expected values
actual values
human-readable message
```

No check may return only prose when structured mismatch data is available.

### E. Public exports

Update only as needed:

```text
src/kaizen/__init__.py
```

Exports must remain narrow. Mutation functions must not be re-exported through the new read-only surface.

### F. Packaging and documentation

Update only:

```text
pyproject.toml
README.md
```

Changes are limited to:

- registering `kaizen-operation-status`;
- documenting the read-only API/CLI and evidence checks;
- explicitly stating that Packet 010B adds no mutation capability.

## Expected implementation paths

The approved implementation packet may change only these paths unless a reviewed deviation is approved before writing:

```text
src/kaizen/operation_status.py
src/kaizen/operation_status_cli.py
src/kaizen/evidence_integrity.py
src/kaizen/__init__.py
pyproject.toml
README.md
tests/test_operation_status.py
tests/test_operation_status_cli.py
tests/test_evidence_integrity.py
tests/test_read_only_inspection_hammer.py
tests/fixtures/operation_status/**
tests/fixtures/evidence_integrity/**
```

Existing source modules may be read and imported but not edited unless implementation evidence proves a narrowly required read-only extraction. Any proposed edit to an existing promotion, amendment, event, root, validation, filesystem, or workflow module stops implementation for review.

## Test requirements

### Unit tests

Test every operation state:

- absent;
- incomplete;
- ready;
- approved;
- committed;
- recovered;
- failed;
- invalid.

Test structured mismatches for:

- packet mismatch;
- root mismatch;
- destination mismatch;
- plan hash mismatch;
- prior/candidate/diff/approval/result hash mismatch;
- Git branch/HEAD/cleanliness mismatch;
- changed-since-review condition;
- duplicate or contradictory terminal events;
- historical event compatibility.

### CLI tests

Test:

- deterministic JSON output;
- deterministic text output;
- invalid operation exit code;
- packet assertion failure;
- missing operation;
- no mutation arguments exist;
- no path override or actor option exists.

### Evidence-integrity tests

Each E-01 through E-06 check requires:

- one passing fixture;
- at least one failing fixture;
- exact structured finding assertions;
- proof that input files and Git state remain byte-for-byte unchanged.

Fixtures must include real-shape examples derived from:

- stale roadmap labels repaired after v0.2 acceptance;
- the Result 068 one-file correction;
- Packet 009B completion evidence;
- one committed promotion operation;
- one committed amendment operation;
- one recoverable or recovered operation fixture;
- synthetic duplicate current-state records.

### Hammer tests

Create:

```text
tests/test_read_only_inspection_hammer.py
```

Required hammer cases:

- monkeypatch or guard every known mutation primitive so any call fails the test;
- snapshot staging, vault, docs fixtures, event logs, and Git state before and after inspection;
- hostile operation IDs and path-like inputs cannot escape expected evidence directories;
- symlink/reparse and root-confusion cases fail closed;
- malformed JSON/YAML/evidence cannot trigger repair or normalization;
- repeated status calls are idempotent and produce identical results;
- concurrent readers do not create locks, temp files, or evidence;
- CLI help exposes no mutation, actor, confirmation, repair, path override, Git, remote, or push option.

## Regression gates

Before completion:

- all focused Packet 010B tests pass;
- the inherited full suite passes with at least `230 passed` and no regression;
- all existing promotion and amendment hammer tests pass;
- no event, plan, approval, canonical, staging, Git, or filesystem mutation occurs during new tests except inside isolated disposable test fixtures created by the tests themselves;
- production fixed roots remain untouched.

## Security constraints

- All production functions added by Packet 010B are read-only.
- No new function may call `append_event`, `approve_amendment`, `execute_amendment`, `recover_amendment`, promotion execution/recovery, atomic install/replace, staging write, or Git mutation.
- No generic command execution is added.
- No arbitrary filesystem traversal is added.
- Explicit roots and operation IDs are validated using existing confinement rules.
- Status output must not expose secrets; current evidence contains no secret contract.
- Errors and mismatch outputs must not include unbounded file contents.
- Readers must fail closed on ambiguous or contradictory evidence.

## Non-scope

- candidate preparation;
- amendment planning;
- approval evidence creation;
- canonical execution;
- recovery execution;
- MCP adapters;
- connector testing;
- local operator console;
- actor identity changes;
- backup implementation or remote creation;
- vault or staging mutation;
- spec amendments beyond documentation returned after implementation evidence;
- broad lint framework, daemon, watcher, scheduler, service, database, or UI;
- Packets 010C through 010F.

## Stop gates

Stop before editing or continue only after renewed review when:

- bound branches, HEADs, roots, remotes, or accepted contract hashes differ;
- an existing mutation module appears to require modification;
- a check cannot be deterministic without semantic or probabilistic inference;
- a proposed API needs actor, confirmation, approval, execution, recovery, repair, or write parameters;
- a test touches production vault or staging roots;
- the implementation creates temp files, locks, caches, logs, or evidence outside disposable test roots;
- a new dependency is proposed;
- full-suite regression occurs;
- changed paths exceed the authorized set.

## Implementation return requirements

An approved Packet 010B implementation must return:

- exact platform commit;
- exact changed paths;
- focused test results;
- full-suite result;
- hammer-test evidence;
- zero-mutation evidence;
- discovered deviations;
- security/steward completion audit;
- updated proposed spec amendments based only on implementation evidence;
- no vault or staging amendment until a later separately approved return packet.

## Acceptance criteria

Packet 010B is ready for owner approval only when:

1. it remains read-only and introduces no mutation capability;
2. exact expected files and stop gates are reviewable;
3. all six E-checks have deterministic contracts and fixtures;
4. platform and vault production roots remain outside implementation test effects;
5. no new dependency is required;
6. existing mutation modules are read-only dependencies, not edit targets;
7. hammer tests prove zero side effects and capability exclusion;
8. a separate security/steward audit passes;
9. the exact packet SHA-256 is presented for owner approval.

## Current gate

```text
Decision 0014: accepted
Milestone 6 Governed Operator Surface specification: accepted
Packet 010A: complete
Packet 010B: proposed, not approved
Packet 010B implementation: prohibited
Packets 010C through 010F: not approved
```
