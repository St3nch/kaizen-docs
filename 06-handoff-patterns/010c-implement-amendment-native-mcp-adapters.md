---
id: kz-tp-01KTPW9BNS1K6V9Z743SKVQD26
type: task-packet
status: complete
project: kaizen-platform
created: 2026-06-09T18:30:00Z
updated: 2026-06-09T19:49:31Z
review_status: approved
authority: accepted
summary: "Revised Packet 010C - implement amendment-native typed adapters in the temporary kaizen-mcp proving ground, delegating preparation to the accepted Packet 010B.1 platform API."
primary_spec: kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
related_specs:
  - kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
---

# Task Packet 010C (Revised) - Implement Amendment-Native MCP Proving-Ground Adapters

> Packet 010C was owner-approved at SHA-256 `fd09d15d0da3fc2c80d643aba47ce18ff77f9afeda9df6cfa4f9d5ed2225333d` and is complete in the temporary non-Git `kaizen-mcp` proving ground. Result 081 records authorization and Result 082 records completion. No Packet 010D through 010F, production MCP migration, Git initialization, canonical vault mutation, live staging mutation, remote creation, or console work is authorized.

## What changed from the blocked draft

The previous Packet 010C draft (audited at SHA-256
`8e81c02917163bcd16ca040546142983b54a4960e741be3b0caf47c4ce90478c`)
identified six tools and was blocked because no accepted platform
amendment-candidate-preparation API existed. Result 077 recommended
Option A: resolve the blocker by adding a separate platform
prerequisite packet.

That prerequisite (Packet 010B.1) is now complete at platform
commit `c56e1cc4f7ca75e060f94127930dc78b8cd7bcc0` with 15 new tests
and zero regression against the 252 baseline. Closure audit recorded
at `03-research-results/079-packet-010b1-completion-security-steward-audit.md`.

This revised Packet 010C:

- removes the blocker section;
- maps `kaizen_prepare_amendment_candidate` to the accepted
  `prepare_amendment_candidate` and
  `load_prepared_amendment_candidate` platform functions;
- preserves all other tool mappings, consequence classifications,
  fixed-root enforcement, mutations-disabled-by-default posture, and
  scope exclusions from the blocked draft.

No platform code changes are proposed by this packet. All six
adapters delegate to existing accepted platform APIs.

## Objective

Add narrowly typed amendment-native tools to the temporary
`C:\dev\kaizen-mcp` proving ground while preserving platform-owned
enforcement and the accepted consequence classifications.

## Scope

This packet is limited to six typed MCP proving-ground adapters, their exact server metadata, bounded disposable-root tests, connector allow/block evidence, and documentation updates within the listed `kaizen-mcp` paths. It introduces no platform semantic change, production MCP migration, console surface, canonical vault mutation, remote, or generalized command capability.

Tool surface (six tools):

```text
kaizen_amendment_status
kaizen_prepare_amendment_candidate
kaizen_plan_amendment
kaizen_approve_amendment
kaizen_execute_amendment
kaizen_recover_amendment
```

## Bound state

```text
kaizen-docs:
main
predecessor HEAD 83d20ddbb8f69d813a3f3ccc64c231785d53cd5b
upstream origin/main

kaizen-platform:
main
HEAD c56e1cc4f7ca75e060f94127930dc78b8cd7bcc0
clean
remote: none

kaizen-vault:
main
HEAD e5e4eec1adc4ef26f9e735333dbb229b7bb59368
clean
remote: none

kaizen-mcp:
non-Git temporary proving ground
service version 0.1.0
fixed roots verified by health
```

Accepted Packet 010B.1 SHA-256 (precondition for this revision):

```text
9e88e85c0595ec5cf541936366726bf3264f39df8c225c46fa7b98333d7f993b
```

Accepted roadmap SHA-256:

```text
1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc
```

Any branch, HEAD, cleanliness, root, remote, or accepted-contract
mismatch stops implementation before editing.

## Read First

### Governance and contracts

- `03-research-results/076-packet-010b-completion-security-steward-audit.md`
- `03-research-results/077-packet-010c-prerequisite-gap-and-security-steward-audit.md`
- `03-research-results/078-packet-010b1-option-a-authorization-and-security-steward-audit.md`
- `03-research-results/079-packet-010b1-completion-security-steward-audit.md`
- `04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md`
- `05-specs/milestone-6-governed-operator-surface.md`
- `06-handoff-patterns/010b1-implement-constrained-amendment-candidate-preparation.md`

### MCP proving-ground surface

- `C:\dev\kaizen-mcp\README.md`
- `C:\dev\kaizen-mcp\docs\TOOL_CONTRACTS.md`
- `C:\dev\kaizen-mcp\docs\UPSTREAM_MUTATION_GATING.md`
- `C:\dev\kaizen-mcp\src\kaizen_mcp\adapter.py`
- `C:\dev\kaizen-mcp\src\kaizen_mcp\server.py`
- `C:\dev\kaizen-mcp\tests\test_boundaries.py`

### Platform implementation (read-only delegation targets)

- `C:\dev\kaizen\platform\src\kaizen\amendment_candidate.py`
- `C:\dev\kaizen\platform\src\kaizen\amendment_plan.py`
- `C:\dev\kaizen\platform\src\kaizen\amendment_workflow.py`
- `C:\dev\kaizen\platform\src\kaizen\operation_status.py`
- `C:\dev\kaizen\platform\src\kaizen\promotion_scope.py`

## Consequence classes

Each tool maps to exactly one accepted consequence class. No tool
mixes classes.

```text
kaizen_amendment_status            inspect
kaizen_prepare_amendment_candidate prepare-candidate
kaizen_plan_amendment              plan
kaizen_approve_amendment           approve
kaizen_execute_amendment           execute
kaizen_recover_amendment           recover
```

## Implementation Requirements

All adapters delegate to accepted platform APIs through the
existing kaizen-mcp adapter pattern (`sys.path` injection of the
platform `src/` followed by import of platform modules). No
adapter reimplements normalization, continuity checks, validation,
diff generation, path confinement, idempotency, evidence
verification, or any mutation primitive.

### A. `kaizen_amendment_status`

Consequence class:

```text
inspect
```

Platform mapping:

```text
kaizen.operation_status.inspect_operation
```

Inputs:

- operation ID;
- optional packet ID assertion.

Requirements:

- return the complete accepted operation-status contract;
- no promotion mutex required for a pure reader;
- no evidence, lock, temporary file, cache, log, staging,
  canonical, or Git mutation;
- preserve structured mismatches and conservative eligibility flags.

### B. `kaizen_prepare_amendment_candidate`

Consequence class:

```text
prepare-candidate
```

Platform mapping after fixed-root scope verification:

```text
kaizen.amendment_candidate.prepare_amendment_candidate
kaizen.amendment_candidate.load_prepared_amendment_candidate
```

Inputs:

- packet ID;
- reserved operation ID;
- canonical-relative destination;
- expected prior canonical SHA-256;
- complete UTF-8 replacement Markdown payload.

Requirements:

- mutations must be disabled by default;
- fixed live roots only;
- packet-bound verified promotion scope created by the existing
  fixed-root verifier;
- the live preparation timestamp is generated internally by the
  platform; the adapter must not expose `prepared_at` to callers
  under live scope;
- the adapter must not interpret, normalize, validate, diff, or
  hash the payload itself; all such work happens inside the
  platform function;
- on success, return the typed
  `AmendmentCandidatePreparation` fields verbatim, including
  `idempotent`, every hash field, validation summary, and live
  binding;
- on failure, surface the platform `PromotionError` code unchanged.
- a parallel typed wrapper for
  `load_prepared_amendment_candidate` is exposed for read-only
  re-verification under the same fixed-root and packet-binding
  rules; it must not be a hidden mutation surface.

### C. `kaizen_plan_amendment`

Consequence class:

```text
plan
```

Platform mapping after fixed-root scope verification:

```text
kaizen.amendment_plan.create_amendment_plan
```

Inputs:

- canonical-relative destination;
- packet ID;
- reserved operation ID (must match the preparation operation ID
  when planning consumes a prepared candidate);
- change summary.

Requirements:

- mutations disabled by default;
- fixed live roots only;
- packet-bound verified promotion scope;
- planning may consume a candidate previously created by
  `kaizen_prepare_amendment_candidate`; the adapter must not
  itself locate, copy, normalize, or rewrite the candidate;
- immutable evidence creation only beneath
  `_promotion/operations/<operation-id>`;
- no approval or canonical mutation;
- return exact plan, validation, prior, candidate, and reviewed
  diff hashes.

### D. `kaizen_approve_amendment`

Consequence class:

```text
approve
```

Platform mapping:

```text
kaizen.amendment_plan.load_ready_amendment
kaizen.amendment_plan.approve_amendment
```

Inputs:

- operation ID;
- packet ID;
- exact plan SHA-256;
- human-supplied actor;
- approval basis.

Requirements:

- verify live scope and exact plan hash before approval;
- never infer or default the actor;
- approval evidence only;
- no execution or canonical mutation.

### E. `kaizen_execute_amendment`

Consequence class:

```text
execute
```

Platform mapping:

```text
kaizen.amendment_plan.load_ready_amendment
kaizen.amendment_workflow.execute_amendment
```

Inputs:

- operation ID;
- packet ID;
- exact plan SHA-256;
- human actor matching approval;
- exact confirmation literal `PROMOTE-LIVE-EXACTLY-ONCE`.

Requirements:

- fixed live roots only;
- no arbitrary filesystem or Git capability;
- execute exactly once under existing platform enforcement;
- return terminal evidence and installed hash data;
- no Git commit, remote, or push.

### F. `kaizen_recover_amendment`

Consequence class:

```text
recover
```

Platform mapping:

```text
kaizen.amendment_plan.load_ready_amendment
kaizen.amendment_workflow.recover_amendment
```

Inputs:

- operation ID;
- packet ID;
- exact plan SHA-256;
- human actor.

Requirements:

- verified recovery scope;
- exact plan-hash check before recovery;
- no alternate repair semantics;
- no event rewriting or deletion;
- preserve idempotency and terminal-event rules.

## Constraints

The implementation must preserve fixed roots, mutations-disabled-by-default behavior, exact consequence classes, packet/operation/hash/actor/confirmation bindings, non-Git `kaizen-mcp` posture, and zero generic shell, SQL, Git, remote, or arbitrary filesystem capability.

## Fixed-root enforcement

All six adapters call the existing kaizen-mcp `_live(settings)`
helper (or its equivalent extended to import the
`amendment_candidate` module) which:

- validates the fixed-root configuration;
- injects the platform `src/` into `sys.path`;
- imports the accepted platform modules;
- yields the platform-callable bundle.

No adapter accepts a root argument, environment override, or
caller-supplied platform path. No adapter calls
`RootConfig(...)` with caller-supplied paths.

## Mutations-disabled-by-default behavior

The existing kaizen-mcp `Settings` object exposes a
`mutations_enabled` flag that defaults to `false`. The five
mutation-class adapters (`kaizen_prepare_amendment_candidate`,
`kaizen_plan_amendment`, `kaizen_approve_amendment`,
`kaizen_execute_amendment`, `kaizen_recover_amendment`) must
raise an explicit, structured error before any platform mutation
call when `mutations_enabled` is false.

`kaizen_amendment_status` is read-only and is not gated on
`mutations_enabled`.

## Binding requirements

Each adapter enforces these bindings before delegating to the
platform:

| Adapter | Packet | Operation | Prior hash | Plan hash | Actor | Confirmation |
|---|---|---|---|---|---|---|
| `kaizen_amendment_status` | optional | required | n/a | n/a | n/a | n/a |
| `kaizen_prepare_amendment_candidate` | required | required (reserved) | required | n/a | n/a | n/a |
| `kaizen_plan_amendment` | required | required | n/a (verified by platform via candidate) | n/a | n/a | n/a |
| `kaizen_approve_amendment` | required | required | n/a | required | required | n/a |
| `kaizen_execute_amendment` | required | required | n/a | required | required | required |
| `kaizen_recover_amendment` | required | required | n/a | required | required | n/a |

"Required (reserved)" means the operation ID must be a freshly
reserved `kz-prom-<ULID>` produced before the call. The adapter
schema must not generate the operation ID itself.

## Connector allow/block evidence

After local adapter and server tests pass on disposable roots, the
implementation may test whether the connected ChatGPT upstream
permits each typed tool classification.

For every connector attempt, record separately:

```text
tool name
consequence class
upstream allowed or blocked
whether Kaizen received the request
pre/post operation status
pre/post approval/event/canonical/Git state
```

A connector block is not a Kaizen failure and is not an
implementation acceptance dependency.

## One-block fallback behavior

After one upstream block on a given tool route, the implementation
may not retry equivalent routes within the same session. The
record of the block is the evidence; retries would only train
upstream classifiers without producing new information.

## Disposable-root tests

All adapter, server, hammer, and binding tests run against
disposable roots created by the existing kaizen-mcp test
fixtures. Production fixed roots
(`C:\dev\kaizen\vault`, `C:\dev\kaizen\staging`) are never used by
this packet's tests.

The platform package must remain importable in the test
environment; tests do not relocate or copy platform source.

## Exact MCP path scope

Only these proving-ground paths may change unless a reviewed
deviation is approved:

```text
C:\dev\kaizen-mcp\src\kaizen_mcp\adapter.py
C:\dev\kaizen-mcp\src\kaizen_mcp\server.py
C:\dev\kaizen-mcp\tests\test_boundaries.py
C:\dev\kaizen-mcp\tests\test_amendment_adapters.py
C:\dev\kaizen-mcp\tests\test_server_tools.py
C:\dev\kaizen-mcp\README.md
C:\dev\kaizen-mcp\docs\TOOL_CONTRACTS.md
C:\dev\kaizen-mcp\docs\ROADMAP.md
C:\dev\kaizen-mcp\docs\UPSTREAM_MUTATION_GATING.md
```

`kaizen-mcp` remains non-Git. Packet 010C does not initialize Git
there.

No `kaizen-platform`, `kaizen-vault`, `kaizen-staging`, or
production MCP path may change under Packet 010C.

## Non-Scope

- implementing candidate preparation, normalization, continuity,
  validation, diff, or hashing logic in MCP;
- changing platform APIs under this packet;
- production MCP migration;
- Git initialization in `kaizen-mcp`;
- local operator console;
- broad UI work;
- canonical vault mutation for project documentation return;
- staging mutation except disposable tests after explicit packet
  approval;
- remote creation or push;
- actor identity expansion;
- generalized edit/delete/move/correct/rollback/supersede;
- multi-note transactions;
- Packets 010D through 010F.

## Validation

### Adapter unit tests

- `kaizen_amendment_status` delegates to `inspect_operation` and
  preserves structured output;
- `kaizen_prepare_amendment_candidate` delegates to
  `prepare_amendment_candidate` and returns the typed
  `AmendmentCandidatePreparation` result verbatim;
- the prepare adapter under fixed live scope does not accept a
  caller-supplied `prepared_at`;
- the prepare adapter raises before calling the platform when
  `mutations_enabled` is false;
- the prepare adapter surfaces `PromotionError` codes unchanged
  for at least: `prior_content_changed`, `amendment_no_change`,
  `validation_failed`, `idempotency_conflict`,
  `preparation_recovery_required`, `live_packet_mismatch`,
  `live_vault_drift`;
- a paired read-only loader adapter verifies prepared evidence
  without mutation;
- `kaizen_plan_amendment` delegates to `create_amendment_plan`
  with verified live scope and may consume a candidate previously
  prepared by `kaizen_prepare_amendment_candidate`;
- `kaizen_approve_amendment` verifies exact plan hash before
  calling `approve_amendment`;
- `kaizen_execute_amendment` verifies mutation enablement, plan
  hash, actor, packet, and confirmation before calling
  `execute_amendment`;
- `kaizen_recover_amendment` verifies mutation enablement,
  recovery scope, packet, actor, and plan hash before calling
  `recover_amendment`;
- mutations-disabled posture fails before any platform mutation
  API is called for all five mutation adapters;
- fixed-root validation occurs before adapter execution;
- no tool exposes arbitrary paths, shell, SQL, Git, remote, push,
  or generic filesystem access.

### Server/tool metadata tests

For each tool, verify:

- exact public name;
- exact argument schema;
- consequence-class description;
- read-only versus mutation annotation where supported by FastMCP;
- required plan, actor, packet, and confirmation language;
- prohibited capabilities are not present.

### Boundary and hammer tests

- wrong packet ID fails closed;
- wrong operation ID or path-like operation ID fails closed;
- wrong prior SHA-256 fails closed before any staging write under
  the prepare adapter;
- wrong plan SHA-256 fails closed before mutation under approve,
  execute, and recover;
- missing or inferred actor is impossible through the schema;
- wrong confirmation fails before execution;
- oversized, null-byte, and non-UTF-8 payloads to the prepare
  adapter fail closed before staging effects;
- connector/server serialization cannot drop hashes or mismatch
  fields;
- repeated status calls are idempotent;
- repeated prepare calls under identical input return
  `idempotent: true` and create no new evidence;
- repeated prepare calls under conflicting input return
  `idempotency_conflict` and preserve all evidence;
- failed adapter calls produce no platform, staging, vault,
  event, approval, or Git changes;
- adapter functions cannot import or invoke arbitrary
  shell/process helpers;
- server exposes no generic command or filesystem tool.

### Connector outcome recording

After local adapter/server tests pass, the implementation may
test whether the connected ChatGPT upstream permits each typed
tool classification, recording the fields listed in the
"Connector allow/block evidence" section.

A connector block is not a Kaizen failure and is not an
implementation acceptance dependency.

After one block, apply the one-block rule and do not retry
equivalent routes.

## Stop gates

Stop before or during implementation when:

- bound repository state or accepted hashes differ;
- owner approval does not bind an exact revised Packet 010C hash;
- platform, docs, vault, or MCP fixed-root state differs;
- an adapter would duplicate platform validation or mutation
  semantics;
- an implementation requires editing `kaizen-platform`;
- a new dependency is proposed;
- a tool schema admits arbitrary paths or commands;
- connector behavior is treated as guaranteed;
- `kaizen-mcp` would need Git initialization;
- changed paths exceed the accepted set;
- full kaizen-mcp suite regression occurs;
- the platform package becomes unimportable from the kaizen-mcp
  test environment.

## Acceptance Criteria

Revised Packet 010C is eligible for owner approval only when:

1. all six tool mappings point to accepted platform APIs and the
   `kaizen_prepare_amendment_candidate` adapter delegates to the
   accepted Packet 010B.1 surface;
2. tool consequence metadata is exact;
3. binding tables for packet, operation, prior hash, plan hash,
   actor, and confirmation are reviewable in the implementation;
4. tests prove fixed-root, packet, operation, plan, actor,
   confirmation, and mutations-disabled binding;
5. failed calls prove zero side effects on platform, vault,
   staging, MCP working tree, and Git state;
6. connector outcomes are evidence rather than acceptance
   dependencies;
7. no production MCP migration or Git initialization is included;
8. a new exact packet SHA-256 is audited.

## Implementation return requirements

An approved implementation must return:

- exact MCP working-tree state (non-Git; the proving ground
  records file paths and hashes rather than commits);
- exact changed paths;
- focused adapter and server test results;
- hammer and binding test evidence;
- mutations-disabled evidence;
- zero-platform-mutation evidence (platform commit unchanged);
- zero-canonical-mutation evidence (vault commit unchanged);
- zero-staging-mutation evidence on production fixed roots;
- connector allow/block log;
- deviations and discovered contract gaps;
- completion security/steward audit;
- no Packet 010D through 010F work.

## Completion Report

Packet 010C is complete against the exact non-Git MCP manifest recorded in Result 082.

Implemented:

- amendment status and prepared-evidence read-only tools;
- constrained amendment-candidate preparation;
- amendment planning, approval, execution, and recovery adapters;
- exact tool metadata and mutation gating;
- prepared-candidate timestamp/hash continuity into planning;
- focused adapter, server-schema, and disposable-root integration tests;
- proving-ground documentation updates.

Verification:

```text
complete kaizen-mcp suite: 20 passed
focused platform amendment/status suite: 20 passed
full kaizen-platform suite: 267 passed
Ruff: all checks passed
Python compilation: pass
```

No platform, vault, live staging, Git, remote, production-MCP, console, or Packet 010D through 010F change occurred. The live MCP process was not restarted during completion review.

## Current gate

```text
Packet 010A: complete
Packet 010B: complete
Packet 010B.1: complete at platform commit c56e1cc
Packet 010C: complete in non-Git kaizen-mcp at the Result 082 manifest
Packet 010C implementation: complete
Packets 010D through 010F: not approved
```
