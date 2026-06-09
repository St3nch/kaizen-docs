---
id: kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
type: spec
status: draft
project: kaizen-platform
created: 2026-06-09T16:10:17Z
updated: 2026-06-09T16:10:17Z
review_status: pending
authority: proposed
summary: "Defines the proposed Milestone 6 operation-status contract, typed tool metadata, local console boundary, evidence checks, and follow-on packet contracts."
---

# Spec - Milestone 6 Governed Operator Surface

## Purpose

Define the smallest client-neutral contract for inspecting and operating one governed amendment without widening Kaizen's accepted mutation model.

This specification is proposed. It does not authorize implementation.

## Authority and dependencies

This specification is subordinate to:

- Kaizen Project Standard v0.2;
- accepted Decisions 0012 and 0013;
- proposed Decision 0014;
- existing implemented-baseline validation, staging, promotion, recovery, event, and hammer specifications;
- the accepted Implementation Roadmap v0.2.

## Repository ownership

### Platform

`C:\dev\kaizen\platform` owns authoritative status construction, evidence verification, operation eligibility, fixed-root enforcement, execution, recovery, console behavior, and tests.

### MCP proving ground

`C:\dev\kaizen-mcp` may expose typed adapters to accepted platform functions. It owns no independent mutation semantics and remains non-Git unless separately authorized.

### Docs

`C:\dev\kaizen-docs` owns this contract, decisions, task packets, audits, and acceptance evidence.

### Vault and staging

The vault remains canonical. Staging remains non-canonical candidate and immutable-operation evidence storage.

## Consequence classes

Every exposed operation must declare exactly one class:

| Class | Consequence | Canonical mutation allowed |
|---|---|---|
| `inspect` | Read-only status and evidence verification | No |
| `prepare-candidate` | Create or replace one constrained staged amendment candidate | No |
| `plan` | Create immutable operation evidence | No |
| `approve` | Create exact-plan-bound approval evidence | No |
| `execute` | Install the approved canonical result and terminal evidence | Yes |
| `recover` | Reconcile an incomplete reviewed operation | Conditional |

Tool metadata must include:

- consequence class;
- fixed-root scope;
- required human involvement;
- required packet, operation, plan, actor, destination, and confirmation bindings;
- possible side effects;
- returned evidence;
- prohibited capabilities;
- idempotency or replay behavior;
- recovery posture.

## Operation-status contract

A typed read-only operation status result must include:

```yaml
operation_type: promote | amend
operation_id: string
packet_id: string
state: absent | incomplete | ready | approved | committed | recovered | failed | invalid
destination: path
roots:
  platform: path
  staging: path
  vault: path
plan_sha256: string | null
prior_sha256: string | null
candidate_sha256: string | null
reviewed_diff_sha256: string | null
approval_sha256: string | null
result_sha256: string | null
validation_run_id: string | null
approved_actor: string | null
approved_at: ISO-8601 UTC | null
events:
  - event_id: string
    phase: intent | committed | failed | recovered
    action: promote | amend
    timestamp: ISO-8601 UTC
    sha256: string | null
git:
  branch: string
  head: full commit hash
  clean: boolean
  expected_branch: string | null
  expected_head: full commit hash | null
eligibility:
  execute: boolean
  recover: boolean
mismatches:
  - code: string
    field: string | null
    expected: string | null
    actual: string | null
    message: string
```

### State rules

- `absent`: no operation evidence exists for the requested operation ID.
- `incomplete`: evidence exists but the immutable plan or required supporting evidence is incomplete.
- `ready`: complete immutable plan and reviewed evidence exist; no approval exists.
- `approved`: exact-plan approval exists and all execution preconditions still match.
- `committed`: exactly one successful committed terminal event exists and the installed canonical/result hash verifies.
- `recovered`: exactly one recovered terminal event exists and recovery evidence verifies.
- `failed`: a failed terminal event exists and no accepted successful terminal result exists.
- `invalid`: contradictory, replayed, drifted, hash-mismatched, root-mismatched, Git-mismatched, or otherwise non-actionable evidence exists.

The status reader must remain backward-compatible with already persisted historical event shapes. It must not rewrite evidence.

### Eligibility rules

`eligibility.execute` is true only when:

- state is `approved`;
- exact plan, candidate, prior, diff, validation, actor, packet, destination, fixed roots, Git branch, Git HEAD, and clean-state bindings verify;
- no terminal event exists;
- no changed-since-review condition exists.

`eligibility.recover` is true only when:

- an intent or partial operation state exists;
- the accepted recovery contract identifies one deterministic reviewed path;
- recovery will not create a second successful terminal event;
- fixed-root, plan, approval, destination, event, filesystem, and Git checks pass.

## Proposed MCP tool contracts

### `kaizen_amendment_status`

- class: `inspect`;
- inputs: operation ID; optional packet ID for additional binding;
- output: complete operation-status contract;
- side effects: none;
- prohibited: evidence creation or mutation.

### `kaizen_prepare_amendment_candidate`

- class: `prepare-candidate`;
- inputs: packet ID, reserved operation ID, canonical destination, expected prior SHA-256, reviewed replacement payload or constrained patch;
- output: staged candidate path, candidate SHA-256, prior SHA-256, reviewed diff SHA-256, intrinsic and operation-context validation summaries;
- side effects: one constrained staging candidate and reviewed diff evidence only;
- prohibited: canonical mutation, generic editing, arbitrary path selection, Git mutation.

### `kaizen_plan_amendment`

- class: `plan`;
- inputs: packet ID, operation ID, expected candidate/prior/diff hashes, destination, expected Git and root bindings;
- output: immutable plan path and SHA-256, validation run ID, canonical candidate SHA-256, complete reviewed summary;
- side effects: immutable staging operation evidence only;
- prohibited: approval, execution, canonical mutation.

### `kaizen_approve_amendment`

- class: `approve`;
- inputs: packet ID, operation ID, exact plan SHA-256, human-supplied trusted local actor, basis;
- output: approval path and SHA-256, approved actor and timestamp;
- side effects: approval evidence only;
- prohibited: inferred actor, plan regeneration, canonical mutation.

### `kaizen_execute_amendment`

- class: `execute`;
- inputs: packet ID, operation ID, exact plan SHA-256, approved actor, exact confirmation literal;
- output: terminal result, event IDs, installed SHA-256, changed paths, Git evidence required for human commit;
- side effects: one approved canonical replacement plus required intent/terminal evidence;
- prohibited: arbitrary filesystem or Git actions, push, remote creation, replay, multi-note mutation.

### `kaizen_recover_amendment`

- class: `recover`;
- inputs: packet ID, operation ID, exact plan SHA-256, approved actor, exact recovery confirmation literal;
- output: recovered terminal result, event IDs, installed/result hashes, recovery evidence;
- side effects: only the deterministic recovery effects permitted by the existing recovery contract;
- prohibited: alternate mutation design, cleanup by deletion, history rewriting, second successful terminal event.

## Minimal local console contract

The console is platform-local and loads one operation ID.

### Required displays

- packet and operation IDs;
- destination and fixed roots;
- operation state;
- all operation hashes;
- validation run;
- actor and approval timestamp;
- exact event chain;
- Git branch, full HEAD, cleanliness, and expected bindings;
- execute/recovery eligibility;
- explicit mismatch reasons.

### Allowed actions

```text
Inspect
Approve exact plan
Execute exactly once
Recover reviewed operation
```

### Prohibited interface capabilities

- free-form Markdown editing;
- arbitrary path browsing or selection;
- shell, PowerShell, Python, or SQL console;
- generic filesystem explorer;
- generic Git staging, commit, reset, clean, stash, remote, or push;
- multi-operation queue approval;
- batch execution;
- generalized mutation types;
- remote or multi-user operation.

### Technology constraint

Packet 010A does not select Tauri, a frontend framework, or a new repository. Packet 010D must choose the smallest platform-compatible local mechanism after tests and packaging constraints are known.

## Actor identity contract

Milestone 6 uses a trusted local actor string.

- The human supplies it at the approval or execution boundary.
- Agents cannot infer or fill it automatically.
- It is preserved in approval and event evidence.
- Interfaces label it as trusted local identity, not authentication.
- Account systems, roles, sessions, remote authentication, identity providers, and multi-user authorization remain deferred.

## Connector boundary

- Read-only inspection and validation are preferred connector uses.
- Typed mutation tools may be attempted when upstream classification permits them.
- Upstream connector allowance is not an acceptance criterion.
- After one block, verify zero side effects and switch to the local operator.
- Record connector outcome separately from Kaizen operation outcome.

## Backup and remote checkpoint

Before approval of a mutation-capable implementation packet, planning must present:

- platform and vault current branch, full HEAD, cleanliness, and remote state;
- current off-machine backup status;
- proposed restore verification;
- repository visibility recommendation if a remote is proposed;
- exact owner authorization phrase for any remote creation or push.

No remote mutation is authorized by this spec.

## Evidence-integrity checks

### E-01 Active roadmap pointer integrity

- input: entrypoint and README active-roadmap references;
- rule: exactly one accepted active roadmap snapshot is referenced, with no contradictory pending/not-accepted label;
- pass: all current pointers identify the accepted roadmap and preserve its hash-bound status;
- failure: file, line, expected label, actual label;
- fixture: stale entrypoint labels found after roadmap acceptance;
- placement: docs-oriented deterministic check implemented in platform test tooling only if Packet 010B proves reuse value.

### E-02 Current-state versus closure evidence

- input: one project current-state note plus accepted completion/closure evidence;
- rule: current state must not report an earlier milestone as active after accepted closure evidence exists;
- pass: state and latest accepted closure agree;
- failure: conflicting note IDs, statuses, milestone labels, and evidence dates;
- fixture: Milestone 4 completion followed by governed current-state amendment;
- placement: platform read-only evidence check.

### E-03 Duplicate live-state detection

- input: canonical notes for a project and type where one active record is required;
- rule: no more than one live current-state record exists;
- pass: zero or one according to project-bootstrap expectations;
- failure: all conflicting IDs and paths;
- fixture: synthetic duplicate current-state fixture derived from the accepted one-record rule;
- placement: platform validation/evidence check.

### E-04 Task-packet status versus closure evidence

- input: task packet plus accepted completion and closure audits;
- rule: a packet with accepted closure evidence cannot remain represented as pending implementation in active navigation or current-state surfaces;
- pass: packet, navigation, and closure posture agree;
- failure: exact contradictory paths and fields;
- fixture: Packet 009B completion history;
- placement: platform read-only evidence check plus docs fixture.

### E-05 Changed-path and staged-scope reporting

- input: Git status and exact authorized path set;
- rule: changed/staged paths equal the reviewed set and no unstaged drift exists before commit;
- pass: exact set equality;
- failure: unexpected, missing, staged, and unstaged path lists;
- fixture: Result 068 one-file frontmatter correction and Packet 010A docs-only batch;
- placement: platform Git evidence helper.

### E-06 Operation evidence and event-chain consistency

- input: operation evidence directory, approval, event log, canonical destination, Git state;
- rule: all hashes and bindings agree; exactly one allowed successful terminal event exists; result bytes match terminal evidence;
- pass: complete consistent chain;
- failure: structured mismatch list;
- fixture: Packet 009B waves and governed amendment return operations;
- placement: authoritative platform status API.

No continuous watcher, scheduler, daemon, or generalized automation framework is specified.

## Existing-spec amendment map

### `kaizen-validation-gate-spec.md`

Proposed amendment:

- define operation-status verification as a read-only consumer of intrinsic and operation-context validation evidence;
- add structured mismatch output expectations;
- add fixtures E-02, E-03, and E-06;
- do not change canonical mutation authorization rules.

### `kaizen-hammer-test-strategy.md`

Proposed amendment:

- add hammer categories for tool consequence classification, adapter argument binding, replay resistance, execute-once, recovery eligibility, connector-block zero-side-effect verification, and console capability exclusion;
- preserve all existing promotion and amendment regression gates.

### `staging-and-promotion-workflow.md`

No semantic mutation-model change required.

Proposed clarification:

- cross-reference the operator-surface spec for typed status and human interfaces;
- state that UI or MCP clients are consumers of the existing governed workflow and cannot bypass two-gate approval.

### `staging-write-wrapper-and-promotion-recovery.md`

Proposed clarification:

- define the constrained candidate-preparation adapter as a caller of the existing same-path amendment primitive;
- define status/recovery eligibility as read-only interpretation of existing evidence;
- prohibit cleanup-oriented recovery shortcuts.

### `promotion-event-schema.md`

No event-field change is currently justified.

Proposed clarification:

- document that the status contract reads both current v0.2 event phases and historical persisted shapes;
- do not rewrite historical events or add UI-specific event fields.

### MCP proving-ground documentation

Proposed update after implementation approval:

- tool names and consequence classes;
- exact platform API mapping;
- connector block recording;
- one-block rule;
- no production-doctrine claim.

## Follow-on packet contracts

### Packet 010B - Platform inspection and evidence-integrity checks

- implement only read-only operation status and E-01 through E-06 checks that Packet 010A accepts;
- no new canonical mutation action;
- exact fixtures, unit/integration tests, and no-regression suite;
- requires separate owner approval.

### Packet 010C - Amendment-native MCP proving-ground adapters

- implement typed adapters to accepted platform APIs;
- metadata must match consequence classes;
- connector allow/block outcomes are evidence, not acceptance dependencies;
- no arbitrary commands and no production migration;
- requires separate owner approval.

### Packet 010D - Minimal fixed-root local operator console

- one operation at a time;
- display the exact required evidence;
- expose only inspect, exact approve, execute-once, and reviewed recovery;
- reuse platform enforcement;
- technology selected only within accepted dependency constraints;
- requires separate owner approval and backup/remote posture checkpoint.

### Packet 010E - Integrated hammer and connector-boundary proof

- prove exact bindings, replay resistance, execute-once, recovery, capability exclusion, connector block recording, and no regression;
- no broad UI or production deployment;
- requires separate owner approval.

### Packet 010F - Governed implementation return and Milestone 6 closure

- return exact commits, tests, deviations, audits, task-packet completion amendment, current-state amendment, scoped vault commits, and closure audit;
- requires separate owner approval and final closure acceptance.

## Acceptance criteria

This specification is ready for owner acceptance only when:

- Decision 0014 and this spec are hash-bound and audited;
- the existing-spec amendment map is verified against current text;
- no hidden implementation authorization exists;
- repository boundaries and consequence classes are unambiguous;
- actor, connector, backup, console, and evidence-check boundaries are proportionate;
- follow-on packets remain separately gated.
