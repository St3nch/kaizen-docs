---
id: kz-tp-01KTV3H4ZP8M6N2Q5R7S9X0YAB
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-10T15:42:57Z
updated: 2026-06-10T15:42:57Z
review_status: pending-review
authority: proposed
summary: "Packet 010D - implement the minimal fixed-root local operator console for one governed amendment operation."
primary_spec: kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
related_specs:
  - kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
---

# Task Packet 010D - Implement Minimal Fixed-Root Local Operator Console

> This packet is a non-authoritative draft. Drafting and auditing it do not authorize implementation. Implementation may begin only after the owner accepts the exact audited Packet 010D SHA-256. This packet does not authorize canonical vault mutation, live staging mutation, remote creation, vault push, production MCP migration, Packet 010E or 010F work, or any broader desktop application.

## Objective

Implement the smallest platform-local human operator interface that can inspect one immutable governed amendment operation, approve its exact immutable plan, execute it exactly once, and invoke reviewed recovery while reusing accepted `kaizen-platform` enforcement.

The selected mechanism is a narrow Python command-line console installed from the existing `kaizen-platform` package. It is not Tauri, a web application, a generic terminal, or a new repository.

## Why this mechanism is sufficient

`kaizen-platform` already ships fixed-purpose Python console scripts and has no runtime UI dependency beyond Python 3.11 and PyYAML. A single bounded CLI entry point can prove the accepted human-operator contract without adding a frontend framework, application shell, server, browser, database, identity system, or packaging boundary.

The console is a typed consumer of accepted platform APIs. It does not own mutation semantics.

## Bound State

```text
kaizen-docs:
branch main
predecessor HEAD 70686c087ddb23f7afbbdba8cae435728f86399f
clean
upstream origin/main

kaizen-platform:
branch main
HEAD c56e1cc4f7ca75e060f94127930dc78b8cd7bcc0
clean
remote none

kaizen-vault:
branch main
HEAD e5e4eec1adc4ef26f9e735333dbb229b7bb59368
clean
remote none

Kaizen MCP:
service kaizen-mcp 0.1.0
runtime registry verified after Packet 010C

Go8:
service chatgpt-mcp 0.2.0
framework fastmcp-3.4.2
generic execution false
```

Authoritative standard SHA-256:

```text
5b441a9e024446d8a9acb3a58a8a24ad020772f4e7666d697c495352bebb6e90
```

Accepted Implementation Roadmap v0.2 SHA-256:

```text
1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc
```

Any branch, HEAD, cleanliness, remote, fixed-root, or accepted-contract mismatch stops implementation before editing.

## Read First

### Accepted authority and completion evidence

- `04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md`
- `05-specs/milestone-6-governed-operator-surface.md`
- `03-research-results/076-packet-010b-completion-security-steward-audit.md`
- `03-research-results/079-packet-010b1-completion-security-steward-audit.md`
- `03-research-results/082-packet-010c-completion-security-steward-audit.md`
- `06-handoff-patterns/010b-implement-read-only-operation-status-and-evidence-integrity.md`
- `06-handoff-patterns/010b1-implement-constrained-amendment-candidate-preparation.md`
- `06-handoff-patterns/010c-implement-amendment-native-mcp-adapters.md`

### Platform implementation

- `C:\dev\kaizen\platform\AGENTS.md`
- `C:\dev\kaizen\platform\pyproject.toml`
- `C:\dev\kaizen\platform\src\kaizen\operation_status.py`
- `C:\dev\kaizen\platform\src\kaizen\operation_status_cli.py`
- `C:\dev\kaizen\platform\src\kaizen\live_amendment_cli.py`
- `C:\dev\kaizen\platform\src\kaizen\amendment_plan.py`
- `C:\dev\kaizen\platform\src\kaizen\amendment_workflow.py`
- `C:\dev\kaizen\platform\src\kaizen\promotion_scope.py`
- relevant existing tests for operation status, amendments, promotion, recovery, and live-scope verification.

## Repository Placement

Implementation belongs in the existing `C:\dev\kaizen\platform` repository.

No new repository is permitted. No code is added to `kaizen-docs`, `kaizen-vault`, `kaizen-staging`, `kaizen-mcp`, or `chatgpt-mcp` under this packet.

## Console Contract

Add one installed entry point:

```text
kaizen-operator
```

The console accepts exactly one operation ID per invocation and exposes exactly four subcommands:

```text
kaizen-operator inspect
kaizen-operator approve
kaizen-operator execute
kaizen-operator recover
```

It must support deterministic human-readable text and deterministic JSON output. It must not start a persistent process, listener, local server, browser, GUI, scheduler, or daemon.

### Common bindings

Every subcommand requires:

- exact operation ID;
- exact packet ID assertion;
- fixed live roots resolved internally from accepted platform configuration;
- no caller-supplied root, repository, staging, vault, or destination path.

Every consequence-bearing subcommand must load current authoritative operation status immediately before acting and fail closed when status contains a binding or integrity mismatch relevant to the requested action.

### `inspect`

Consequence class: `inspect`.

Authoritative platform mapping:

```text
kaizen.operation_status.inspect_operation
```

Required display and JSON fields:

- packet ID;
- operation ID;
- operation type and state;
- destination;
- canonical and staging fixed-root bindings represented without caller override;
- plan, prior canonical, candidate, reviewed diff, approval, and result SHA-256 values;
- validation run ID and validation status available from the accepted status contract;
- approval actor and timestamp;
- ordered event chain and terminal outcome;
- Git branch, full HEAD, cleanliness, and expected binding data supplied by the platform status contract;
- execute eligibility;
- recovery eligibility;
- every structured mismatch code, field, expected value, actual value, and message.

Inspection is read-only. It creates no lock file, cache, evidence, temporary file, log entry, Git change, canonical mutation, or staging mutation.

### `approve`

Consequence class: `approve`.

Required caller inputs:

- operation ID;
- packet ID;
- exact plan SHA-256 copied from reviewed inspection output;
- human-supplied trusted local actor;
- non-empty approval basis.

Authoritative platform mapping:

```text
kaizen.operation_status.inspect_operation
kaizen.amendment_plan.load_ready_amendment
kaizen.amendment_plan.approve_amendment
```

Requirements:

- compare the supplied plan SHA-256 byte-for-byte with the immutable plan hash before approval;
- require current authoritative status to be `ready`, unapproved, packet-bound, and free of integrity mismatches relevant to approval;
- never infer, remember, default, normalize, or fabricate the actor;
- label the actor as a trusted local identifier, not authenticated identity;
- create approval evidence only;
- return post-action operation status so the human can verify the approval hash, actor, timestamp, and unchanged canonical state;
- no execution, canonical mutation, Git mutation, remote, or push.

### `execute`

Consequence class: `execute`.

Required caller inputs:

- operation ID;
- packet ID;
- exact plan SHA-256;
- human actor exactly matching approval evidence;
- exact confirmation literal `PROMOTE-LIVE-EXACTLY-ONCE`.

Authoritative platform mapping:

```text
kaizen.operation_status.inspect_operation
kaizen.amendment_plan.load_ready_amendment
kaizen.amendment_workflow.execute_amendment
```

Requirements:

- compare the supplied plan SHA-256 before execution;
- require exact packet, operation, actor, approval, Git, evidence, and execute-eligibility bindings;
- reject missing, altered, lowercase, whitespace-padded, or otherwise non-exact confirmation;
- execute exactly once under existing platform mutex, intent-event, terminal-event, canonical-installation, and idempotency enforcement;
- return terminal result plus post-action operation status, including installed canonical SHA-256 and event chain;
- perform no Git add, commit, reset, clean, stash, remote, or push.

### `recover`

Consequence class: `recover`.

Required caller inputs:

- operation ID;
- packet ID;
- exact plan SHA-256;
- human actor exactly matching approval evidence;
- exact recovery confirmation literal `RECOVER-LIVE-EXACTLY-ONCE`; this packet-local interface literal adds deliberate human confirmation only and must not alter platform recovery semantics.

Authoritative platform mapping:

```text
kaizen.operation_status.inspect_operation
kaizen.amendment_plan.load_ready_amendment
kaizen.amendment_workflow.recover_amendment
```

Requirements:

- expose recovery only when authoritative status reports recovery eligibility;
- compare the exact immutable plan hash before recovery;
- require the exact actor binding;
- require deliberate explicit confirmation distinct from ordinary invocation;
- preserve existing two-state recovery classification: exact prior bytes or exact candidate bytes;
- fail closed on unknown canonical state;
- never delete, rewrite, truncate, or synthesize historical events;
- never create a second successful terminal outcome;
- return recovery result plus post-action operation status.

No alternate rollback, correction, cleanup, retry, or repair semantics may be introduced.

## Implementation Shape

Preferred minimal shape:

```text
src/kaizen/operator_console.py
tests/test_operator_console.py
pyproject.toml
README.md
```

A reviewed implementation may instead replace or narrowly refactor `src/kaizen/live_amendment_cli.py` and its existing tests when that produces a smaller exact diff without weakening compatibility. It must not maintain two conflicting human amendment operators.

The implementation must share one internal status-rendering path with `operation_status_cli.py` or extract a small reusable renderer. Duplicated evidence interpretation is prohibited.

No new runtime dependency is expected or authorized. Adding one is a packet deviation requiring owner review before installation.

## Existing Behavior Reconciliation

The current `kaizen-amend-live` CLI predates the accepted Packet 010D console contract. It includes plan creation and does not require a caller-supplied exact plan SHA-256 on approval, execution, or recovery.

Packet 010D must resolve this safely by one of these bounded approaches:

1. replace `kaizen-amend-live` with the accepted four-action `kaizen-operator` surface and retain a compatibility error directing users to the new entry point; or
2. retain `kaizen-amend-live` only if its consequence-bearing paths are changed to enforce the same exact plan-hash and actor/confirmation bindings and its `plan` action is clearly excluded from the Packet 010D operator surface.

The implementation audit must reject any route that leaves a weaker live mutation path available.

Candidate preparation and planning remain available only through their already accepted fixed-purpose workflows. Packet 010D does not add them to the console.

## Exact Path Scope

Expected platform path scope:

```text
C:\dev\kaizen\platform\pyproject.toml
C:\dev\kaizen\platform\README.md
C:\dev\kaizen\platform\src\kaizen\operator_console.py
C:\dev\kaizen\platform\src\kaizen\operation_status_cli.py
C:\dev\kaizen\platform\src\kaizen\live_amendment_cli.py
C:\dev\kaizen\platform\tests\test_operator_console.py
C:\dev\kaizen\platform\tests\test_operation_status_cli.py
C:\dev\kaizen\platform\tests\test_live_amendment_cli.py
```

Only files actually required by the chosen minimal implementation may change. Missing listed files are not a requirement to create them. Any path outside this set is a deviation that must be reported and reviewed before editing.

No vault, staging, docs, MCP, schema, event-log, or remote path may change under implementation tests.

## Required Tests

All mutation-path tests use disposable roots and disposable Git repositories. Production vault and staging roots are forbidden.

### Parser and capability tests

Prove:

- exactly four subcommands exist;
- one operation ID is accepted per invocation;
- packet ID is required;
- exact plan SHA-256 is required for approve, execute, and recover;
- actor is required for approve, execute, and recover and has no default;
- approval basis is required only for approve;
- exact execute and recovery confirmations are enforced;
- no root, arbitrary path, staged path, destination, shell, command, SQL, Python, Git, remote, push, batch, queue, editor, delete, move, or generalized mutation argument exists;
- no plan or candidate-preparation action exists in `kaizen-operator`.

### Inspection tests

Prove deterministic text and JSON output for ready, approved, completed, interrupted-recoverable, invalid, packet-mismatch, dirty-Git, unavailable-Git, and historical-event cases.

Inspection must be byte-for-byte side-effect free across operation evidence, canonical destination, staging, event log, and Git worktree.

### Approval tests

Prove exact plan hash succeeds once; wrong plan, packet mismatch, or missing actor fails without side effects; reapproval follows accepted behavior; canonical bytes and event log remain unchanged; and returned status matches approval evidence.

### Execute-once tests

Prove exact approved execution succeeds once; wrong plan, actor, confirmation, packet, Git binding, dirty state, evidence hash, or candidate hash fails before canonical mutation; replay cannot create a second installation or second successful terminal event; and returned status matches installed bytes.

### Recovery tests

Prove recovery is unavailable before an interrupted eligible state; exact-prior and exact-candidate states follow accepted deterministic recovery; unknown bytes fail closed; wrong plan, actor, packet, or confirmation fails without mutation; history is never deleted or rewritten; and no second successful terminal outcome appears.

### Regression tests

Run and pass:

- focused operator-console tests;
- existing operation-status tests;
- existing candidate-preparation tests;
- existing amendment planning, approval, execution, and recovery tests;
- existing first-time promotion tests;
- event-chain and Git-binding tests;
- the complete `kaizen-platform` suite.

The baseline is 267 passing platform tests at Packet 010C closure. The implementation return must state the exact new focused-test count and exact full-suite count.

## Hammer Tests

Prove capability exclusion, exact plan-hash binding, no inferred actor, execute-once replay resistance, conservative recovery eligibility, inspection side-effect freedom, removal of any weaker legacy path, disposable-root confinement, no generic Git capability, and no regression across promotion, amendment, candidate preparation, status, recovery, Git binding, and event-chain evidence.

Packet 010E remains responsible for integrated connector-boundary proof. Packet 010D must still return enough focused evidence that Packet 010E is not asked to discover basic console-contract failures.

## Human-Only Boundaries

The console may present evidence and accept explicit inputs. It may not decide that an operation should be approved, executed, or recovered.

Human-only decisions remain the reviewed-diff verdict, trusted local actor, approval basis, execute decision, reviewed recovery decision, and every backup, remote, commit, push, production migration, or broader interface choice.

No agent, connector, default, environment variable, config file, cache, or previous invocation may populate the actor or confirmation on the human's behalf.

## Backup and Remote Posture Checkpoint

Verified repository posture at drafting:

```text
kaizen-platform: local Git repository, clean, no remote
kaizen-vault: local Git repository, clean, no remote
kaizen-docs: clean and synchronized with existing origin/main
```

Off-machine backup status has not been verified by repository evidence available to this packet. Packet 010D creates no remote and authorizes no push. Before implementation begins, the owner must explicitly accept proceeding with local-only platform and vault repositories or separately authorize a reviewed backup plan.

No remote creation is recommended inside Packet 010D because that is a separate consequential owner decision and not required to prove the local console.

## Documentation Drift

`00-entrypoint/LLM_START_HERE.md` contains a later stale sentence saying Packet 010C remains blocked even though Result 082 records completion. That is documentation drift.

The stale sentence is not part of Packet 010D platform implementation and must not be silently bundled into implementation. It should be corrected in this reviewed planning/audit docs batch or in a separate bounded docs-only correction before implementation authorization. The accepted roadmap snapshot must not be modified.

## Non-Scope

- Tauri, GUI, web server, browser UI, daemon, scheduler, watcher, or service;
- production MCP migration or Kaizen MCP changes;
- new mutation semantics or canonical action types;
- candidate preparation or planning through `kaizen-operator`;
- generalized editor, filesystem browser, path picker, shell, PowerShell, Python execution surface, SQL, or generic command execution;
- generic Git client, commit, push, reset, clean, stash, remote, or remote creation;
- generalized correction, rollback, delete, move, supersedence, or cleanup;
- multi-operation approval, queue, batch, multi-note transaction, remote execution, multi-user identity, authentication, roles, or sessions;
- Operational Postgres, Observatory, Internet Marketing Intelligence database, Qdrant, Hermes, or multi-agent orchestration;
- canonical vault mutation or live staging mutation during implementation or testing;
- Packet 010E or 010F work.

## Acceptance Criteria

Packet 010D implementation is complete only when:

1. the owner accepted the exact audited packet SHA-256 before any platform edit;
2. implementation stays inside the accepted repository and path boundary;
3. `kaizen-operator` exposes exactly inspect, approve, execute, and recover for one operation;
4. all evidence comes from authoritative platform status data;
5. approve, execute, and recover require exact plan hash and explicit human actor;
6. execute and recover require exact deliberate confirmation;
7. no weaker legacy live mutation route remains;
8. disposable-root tests and the complete suite pass without regression;
9. no new dependency, remote, push, MCP change, vault mutation, or live staging mutation occurs;
10. the return contains exact changed paths, hashes, commit, tests, deviations, and evidence needed for Packet 010E.

## Required Implementation Return

Return predecessor and final platform state; exact changed-path and hash manifest; staged-scope and diff-check evidence; focused and full test commands/results; capability-exclusion, side-effect, exact-hash, execute-once, recovery, regression, and production-root non-use evidence; every deviation; and the local platform commit after exact staged-scope review.

## Owner Gate

Approval must bind to the exact reviewed SHA-256 of this packet.

```text
I approve Kaizen Task Packet 010D at SHA-256 <EXACT_PACKET_SHA256> for implementation of the minimal fixed-root local `kaizen-operator` console in `C:\dev\kaizen\platform` only. I accept proceeding with the currently verified local-only platform and vault repository posture for this packet. I do not authorize Tauri or broad UI work, new runtime dependencies, production MCP migration, Kaizen MCP changes, canonical vault mutation, live staging mutation, remote creation, push, Packet 010E, or Packet 010F work.
```

Packets 010E and 010F remain separately gated.
