---
id: kz-tp-01KTPP2DJ3SGW9H7BDC7H82DKA
type: task-packet
status: blocked
project: kaizen-platform
created: 2026-06-09T17:14:40Z
updated: 2026-06-09T17:14:40Z
review_status: pending
authority: proposed
summary: "Implement amendment-native typed adapters in the temporary Kaizen MCP proving ground, subject to resolution of the missing platform candidate-preparation API."
primary_spec: 05-specs/milestone-6-governed-operator-surface.md
related_specs:
  - 04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/promotion-event-schema.md
---

# Task Packet 010C - Implement Amendment-Native MCP Proving-Ground Adapters

> Packet 010C is a blocked proposed implementation packet. It does not authorize MCP changes, platform changes, candidate preparation, planning, approval, execution, recovery, connector mutation attempts, vault or staging mutation, remote creation, or work under Packets 010D through 010F.

## Objective

Add narrowly typed amendment-native tools to the temporary `C:\dev\kaizen-mcp` proving ground while preserving platform-owned enforcement and the accepted consequence classifications.

Candidate tool surface:

```text
kaizen_amendment_status
kaizen_prepare_amendment_candidate
kaizen_plan_amendment
kaizen_approve_amendment
kaizen_execute_amendment
kaizen_recover_amendment
```

## Current blocker

Packet 010C is not ready for owner approval because the accepted platform currently exposes amendment APIs for:

```text
status
plan
approve
execute
recover
```

but does not expose an amendment-specific constrained candidate-preparation API.

Verified platform functions include:

```text
inspect_operation
create_amendment_plan
load_ready_amendment
approve_amendment
execute_amendment
recover_amendment
```

No `prepare_amendment_candidate` or equivalent platform function was found.

The generic create-only staging writer is not an acceptable substitute because:

- it is designed for new agent-authored staged notes;
- it forbids staged content claiming accepted authority or approved review status;
- an amendment candidate must preserve the existing canonical note's `id`, `type`, `project`, `created`, `review_status`, and `authority` fields;
- it does not bind one canonical destination, expected prior SHA-256, reserved amendment operation ID, and reviewed diff evidence as required by the accepted operator-surface specification.

Implementing candidate preparation directly in `kaizen-mcp` would create independent mutation semantics outside `kaizen-platform` and violate accepted Decision 0014.

## Required blocker resolution

Before Packet 010C may be approved, the owner must choose one reviewed path:

### Option A - Add a platform candidate-preparation primitive first

Draft, audit, and separately approve a narrow platform packet that implements a fixed-root amendment-candidate preparation API with:

- one packet ID;
- one reserved operation ID;
- one canonical destination;
- expected prior canonical SHA-256;
- reviewed full replacement payload or separately accepted constrained patch contract;
- immutable identity and authority continuity checks;
- canonical and operation-context validation;
- candidate SHA-256, prior SHA-256, and reviewed diff SHA-256 output;
- staging-only side effects;
- no plan, approval, event, canonical, or Git mutation.

After that platform primitive is accepted, Packet 010C can adapt all six tools.

### Option B - Narrow Packet 010C to five existing platform APIs

Amend the accepted operator-surface specification and Packet 010C scope to omit `kaizen_prepare_amendment_candidate` from this slice. Candidate preparation would remain human/local until a later accepted platform packet.

This option requires an explicit reviewed contract amendment. It cannot be inferred from convenience.

## Bound state

```text
kaizen-docs:
main
HEAD 2acf04604f9597998f6beec4da1d09a78fd01044
clean
upstream origin/main
+0 / -0

kaizen-platform:
main
HEAD 1d3dabf5a49bc818b8c4830733b6fb23b9e76ee7
clean
remote none

kaizen-vault:
main
HEAD e5e4eec1adc4ef26f9e735333dbb229b7bb59368
clean
remote none

kaizen-mcp:
non-Git temporary proving ground
service version 0.1.0
fixed roots verified by health
```

Accepted roadmap SHA-256:

```text
1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc
```

## Read first

- `03-research-results/076-packet-010b-completion-security-steward-audit.md`
- `04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md`
- `05-specs/milestone-6-governed-operator-surface.md`
- `C:\dev\kaizen-mcp\README.md`
- `C:\dev\kaizen-mcp\docs\TOOL_CONTRACTS.md`
- `C:\dev\kaizen-mcp\docs\UPSTREAM_MUTATION_GATING.md`
- `C:\dev\kaizen-mcp\src\kaizen_mcp\adapter.py`
- `C:\dev\kaizen-mcp\src\kaizen_mcp\server.py`
- `C:\dev\kaizen-mcp\tests\test_boundaries.py`
- `C:\dev\kaizen\platform\src\kaizen\operation_status.py`
- `C:\dev\kaizen\platform\src\kaizen\amendment_plan.py`
- `C:\dev\kaizen\platform\src\kaizen\amendment_workflow.py`
- `C:\dev\kaizen\platform\src\kaizen\promotion_scope.py`

## Implementable adapter scope after blocker resolution

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
- no promotion mutex is required for a pure reader unless platform evidence proves otherwise;
- no evidence, lock, temporary file, cache, log, staging, canonical, or Git mutation;
- preserve structured mismatches and conservative eligibility flags.

### B. `kaizen_prepare_amendment_candidate`

Consequence class:

```text
prepare-candidate
```

Status:

```text
blocked - no accepted platform API exists
```

The MCP must not implement this behavior independently.

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

- staging-relative candidate path;
- canonical-relative destination;
- packet ID;
- reserved operation ID;
- change summary.

Requirements:

- mutations must be disabled by default;
- fixed live roots only;
- packet-bound verified promotion scope;
- immutable evidence creation only;
- no approval or canonical mutation;
- return exact plan, validation, prior, candidate, and reviewed diff hashes.

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

## Expected MCP paths after approval

Only these proving-ground paths may change unless a reviewed deviation is approved:

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

`kaizen-mcp` remains non-Git. Packet 010C does not initialize Git there.

No `kaizen-platform`, `kaizen-vault`, `kaizen-staging`, or production MCP path may change under Packet 010C.

## Required tests after blocker resolution

### Adapter unit tests

- status delegates to `inspect_operation` and preserves structured output;
- plan delegates to `create_amendment_plan` with verified live scope;
- approval verifies exact plan hash before calling `approve_amendment`;
- execution verifies mutation enablement, plan hash, actor, packet, and confirmation before calling `execute_amendment`;
- recovery verifies mutation enablement, recovery scope, packet, actor, and plan hash before calling `recover_amendment`;
- mutations-disabled posture fails before platform mutation APIs are called;
- fixed-root validation occurs before adapter execution;
- no tool exposes arbitrary paths, shell, SQL, Git, remote, push, or generic filesystem access.

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
- wrong plan SHA-256 fails before mutation;
- missing or inferred actor is impossible through the schema;
- wrong confirmation fails before execution;
- connector/server serialization cannot drop hashes or mismatch fields;
- repeated status calls are idempotent;
- failed adapter calls produce no platform, staging, vault, event, approval, or Git changes;
- adapter functions cannot import or invoke arbitrary shell/process helpers;
- server exposes no generic command or filesystem tool.

### Connector outcome recording

After local adapter/server tests pass, the implementation may test whether the connected ChatGPT upstream permits each typed tool classification.

For every attempt record separately:

```text
tool name
consequence class
upstream allowed or blocked
whether Kaizen received the request
pre/post operation status
pre/post approval/event/canonical/Git state
```

A connector block is not a Kaizen failure and is not an implementation acceptance dependency.

After one block, apply the one-block rule and do not retry equivalent routes.

## Non-scope

- implementing candidate preparation in MCP;
- changing platform APIs under this packet;
- production MCP migration;
- Git initialization in `kaizen-mcp`;
- local operator console;
- broad UI work;
- canonical vault mutation for project documentation return;
- staging mutation except disposable tests after explicit packet approval;
- remote creation or push;
- actor identity expansion;
- generalized edit/delete/move/correct/rollback/supersede;
- multi-note transactions;
- Packets 010D through 010F.

## Stop gates

Stop before implementation when:

- the candidate-preparation blocker is unresolved;
- owner approval does not bind an exact revised Packet 010C hash;
- platform, docs, vault, or MCP fixed-root state differs;
- an adapter would duplicate platform validation or mutation semantics;
- an implementation requires editing `kaizen-platform`;
- a new dependency is proposed;
- a tool schema admits arbitrary paths or commands;
- connector behavior is treated as guaranteed;
- `kaizen-mcp` would need Git initialization;
- changed paths exceed the accepted set.

## Acceptance criteria

Packet 010C cannot be approved in its current blocked state.

It becomes eligible for renewed audit and owner approval only when:

1. Option A or Option B is explicitly reviewed and accepted;
2. the packet is revised to match that decision;
3. all tool mappings point to accepted platform APIs;
4. tool consequence metadata is exact;
5. tests prove fixed-root, packet, operation, plan, actor, and confirmation binding;
6. failed calls prove zero side effects;
7. connector outcomes are evidence rather than acceptance dependencies;
8. no production MCP migration or Git initialization is included;
9. a new exact packet SHA-256 is audited.

## Current gate

```text
Packet 010A: complete
Packet 010B: complete
Packet 010C: blocked draft
Packet 010C implementation: prohibited
Packets 010D through 010F: not approved
```
