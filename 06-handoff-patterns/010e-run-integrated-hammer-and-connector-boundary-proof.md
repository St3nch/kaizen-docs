---
id: kz-tp-01KTV5E2H7M9Q3R6S8W0Y1ZABC
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-10T11:55:00-05:00
updated: 2026-06-10T11:55:00-05:00
review_status: pending-review
authority: proposed
summary: "Packet 010E - run integrated hammer and connector-boundary proof for the accepted Milestone 6 operator surface."
primary_spec: kz-spec-01KTPJBRH0RQTCNRVQYTZJPVVG
---

# Task Packet 010E - Run Integrated Hammer and Connector-Boundary Proof

> This packet is a non-authoritative draft. It authorizes no test execution, code change, connector mutation, canonical vault mutation, live staging mutation, production deployment, Packet 010F work, or Milestone 6 closure until the owner accepts the exact audited Packet 010E SHA-256.

## Objective

Prove that the completed Milestone 6 surface behaves correctly across the platform, local operator console, temporary Kaizen MCP proving ground, and ChatGPT connector boundary.

Packet 010E is an evidence and adversarial-test packet. It must not add product features, broaden schemas, introduce new mutation semantics, migrate the MCP server to production, or weaken a control merely to make a connector call succeed.

## Bound predecessor state

```text
kaizen-docs:
branch main
HEAD f76d593ceb513be86abd0fe4f9e5429329e0750c
clean
upstream origin/main
ahead/behind 0/0

kaizen-platform:
branch main
HEAD 1b8be1d6d42d768587dddb2be8415fa24b670561
clean
remote none
full suite 276 passed

kaizen-vault:
branch main
HEAD e5e4eec1adc4ef26f9e735333dbb229b7bb59368
clean
remote none

Go8:
service chatgpt-mcp
version 0.3.0
tool count 42
generic execution false

Kaizen MCP:
temporary non-Git proving ground
runtime health and exact tool registry must be reverified before execution
```

Packet 010D authority:

```text
Packet SHA-256: e0f0ab4e25de8d94be0fd6ace77f76d8fccaf27257a034a4d9d3393832500552
Owner approval: Result 084
Completion audit: Result 085
Platform commit: 1b8be1d6d42d768587dddb2be8415fa24b670561
```

Any mismatch in branch, HEAD, cleanliness, remote posture, fixed roots, tool registry, accepted contract, or predecessor evidence stops execution before editing or connector mutation.

## Read first

- `IMPLEMENTATION_ROADMAP_V0.2.md`
- `04-design-decisions/0014-milestone-6-governed-operator-surface-boundary.md`
- `05-specs/milestone-6-governed-operator-surface.md`
- `03-research-results/082-packet-010c-completion-security-steward-audit.md`
- `03-research-results/085-packet-010d-completion-security-steward-audit.md`
- `06-handoff-patterns/010c-implement-amendment-native-mcp-adapters.md`
- `06-handoff-patterns/010d-implement-minimal-fixed-root-local-operator-console.md`
- relevant platform amendment, promotion, status, recovery, event-chain, Git-binding, and hammer tests;
- relevant `kaizen-mcp` adapter, boundary, and exact-schema tests.

## Execution posture

Use Go8 for repository inspection, exact edits, bounded test profiles, parser inspection, capability scans, integrity checks, staging, and local platform commits.

Use Kaizen MCP only for its typed governed tools and read-only health/status inspection.

Connector allow/block outcomes are evidence, not acceptance dependencies. Apply the one-block rule:

```text
one upstream connector block
-> verify no side effect
-> record exact tool, arguments class, timestamp, and upstream outcome
-> do not retry cosmetic variants
-> continue through local deterministic evidence
```

A connector block must never be treated as permission to bypass platform controls or weaken tool schemas.

## Authorized repositories and path scope

Packet 010E may change only test or test-support files needed to close a demonstrated evidence gap.

Expected platform scope:

```text
tests/test_operator_console.py
tests/test_amendment_hammer.py
tests/test_amendment_workflow.py
tests/test_operation_status.py
tests/test_operation_status_cli.py
```

Expected temporary Kaizen MCP scope:

```text
tests/test_amendment_adapters.py
tests/test_boundaries.py
tests/test_server_tools.py
```

Only files actually required by a proven gap may change. Production modules, schemas, entry points, runtime dependencies, docs outside the execution return, vault files, and live staging files are excluded.

Any required production-code change is a Packet 010E blocker. Stop and return a proposed corrective packet rather than silently expanding this packet.

`C:\dev\kaizen-mcp` remains non-Git. Do not initialize Git.

## Required proof matrix

### P-01 - Exact immutable bindings

Prove across platform, console, and MCP adapters that wrong or missing values fail before mutation for:

- packet ID;
- operation ID;
- plan SHA-256;
- prior canonical SHA-256;
- candidate SHA-256;
- reviewed diff SHA-256;
- approval actor;
- execute confirmation;
- recovery eligibility;
- Git branch, full HEAD, and clean-state binding.

### P-02 - Execute-once and replay resistance

Prove:

- one approved operation installs canonical bytes once;
- repeated execution converges idempotently or fails closed according to accepted semantics;
- no second successful terminal event is created;
- same-operation contention converges;
- competing operations for one destination fail closed after the first accepted change;
- connector retries cannot create a second installation.

### P-03 - Recovery

Prove:

- recovery is unavailable before an eligible interrupted state;
- interruption before replacement returns to exact prior state;
- interruption after replacement converges to exact candidate state;
- unknown canonical or owned-temp bytes fail closed;
- recovery cannot delete, truncate, reorder, or synthesize historical events;
- recovery cannot create a second successful terminal outcome;
- actor, packet, plan, and eligibility mismatches fail without mutation.

### P-04 - Capability exclusion

Prove by parser/schema inspection and static capability scan that the console and MCP tools expose no:

- arbitrary root or path;
- destination selection in the operator console;
- shell, PowerShell, Python code, SQL, or arbitrary command;
- generic filesystem browser or editor;
- generic Git, commit, push, reset, clean, stash, remote, or remote creation;
- batch, queue, multi-operation approval, or multi-note transaction;
- generalized delete, move, correction, rollback, or cleanup action;
- identity platform, remote multi-user execution, or broad UI.

### P-05 - Inspection side-effect freedom

For ready, approved, invalid, completed, interrupted, dirty-Git, and unavailable-Git fixtures, prove inspection does not change:

- operation evidence;
- candidate or reviewed diff;
- canonical destination bytes;
- governance event log;
- staging contents;
- Git index, worktree, branch, or HEAD.

### P-06 - Connector-boundary recording

Using disposable test operations only, attempt a bounded representative connector matrix:

Read-only:

- Kaizen MCP health;
- amendment status;
- prepared-candidate load when a disposable fixture exists.

Consequence-bearing connector routes must be tested only through deliberately invalid, fail-before-mutation negative calls using well-formed nonexistent operation IDs, impossible expected hashes, or missing prerequisite evidence. The call must be designed so the platform rejects it before any staging, canonical, event-log, approval, or Git mutation. Cover the connector routes for:

- candidate preparation;
- plan;
- approve;
- execute;
- recover.

Successful mutation behavior for those actions must be proven through local adapter integration tests using disposable roots, not through the live fixed-root connector.

For each attempted call, record:

- tool name and consequence class;
- whether ChatGPT allowed the call to reach the server;
- server result when reached;
- upstream block text when blocked;
- verified side-effect result;
- whether local deterministic evidence covers the same contract.

Do not perform a live canonical vault mutation or live staging mutation to manufacture connector evidence. If a safe disposable connector fixture cannot be established through accepted fixed roots, record the limitation and rely on local adapter integration evidence.

### P-07 - No regression

Run and pass:

- focused Packet 010E tests;
- Packet 010D console tests;
- existing amendment hammer tests;
- amendment planning, approval, execution, recovery, status, event-chain, and Git-binding tests;
- first-time promotion tests;
- complete `kaizen-platform` suite;
- complete `kaizen-mcp` suite;
- compilation for both roots;
- Ruff where already installed.

Do not install new dependencies solely to satisfy an unavailable optional quality tool. Record the environment fact exactly.

## Hammer minimums

The execution return must include at least:

- 20 repeated successful amendments;
- 20 repeated interrupted amendments split across pre-replace and post-replace recovery;
- same-operation concurrent execution contention;
- competing-operation same-destination contention;
- wrong-hash and stale-Git failure matrices;
- repeated read-only inspection with byte-for-byte no-side-effect verification;
- schema/parser capability exclusion;
- connector allow/block outcome table.

Existing passing hammer tests may satisfy a requirement when their exact code and execution evidence are reviewed. Do not duplicate tests merely to inflate counts.

## Test-root restrictions

All mutation-path tests must use disposable roots and disposable Git repositories.

Forbidden during Packet 010E:

- canonical vault mutation;
- live staging mutation;
- production operation execution;
- vault commit or push;
- platform remote creation or push;
- Git initialization in `kaizen-mcp`;
- arbitrary shell or broad command execution;
- Packet 010F work.

## Acceptance criteria

Packet 010E is complete only when:

1. the owner accepted the exact audited Packet 010E SHA-256 before execution;
2. predecessor repositories and accepted authority matched;
3. every P-01 through P-07 requirement has evidence or an explicit non-blocking connector limitation;
4. no production-code change was needed;
5. all mutation tests used disposable roots;
6. exact connector allow/block outcomes were recorded without bypass attempts;
7. focused and full regression suites passed;
8. exact changed paths, hashes, commands, results, and deviations were returned;
9. platform and vault remain local-only and clean;
10. Packet 010F and Milestone 6 closure remain separately gated.

## Required execution return

Return:

- predecessor and final repository snapshots;
- exact test-file changes and hashes, if any;
- focused and complete test commands and counts;
- compilation and available lint results;
- P-01 through P-07 evidence matrix;
- connector allow/block outcome table;
- side-effect verification for every connector attempt;
- confirmation that production vault and live staging were untouched;
- confirmation that no production module, dependency, remote, push, MCP migration, Packet 010F, or closure work occurred;
- all limitations and deviations;
- local platform commit only if platform test files changed;
- no commit in non-Git `kaizen-mcp`.

## Non-scope

- product feature implementation;
- production-code repair or schema change;
- broad UI or Tauri work;
- production MCP migration;
- new dependencies;
- canonical vault or live staging mutation;
- live customer/project operation;
- remote creation or push for platform or vault;
- Git initialization in `kaizen-mcp`;
- Packet 010F or Milestone 6 closure.

## Owner gate

```text
I approve Kaizen Task Packet 010E at SHA-256 <EXACT_PACKET_SHA256> for integrated hammer and connector-boundary proof only. I authorize bounded test-only changes within the packet's exact platform and temporary kaizen-mcp test-file scope, disposable-root connector attempts, local platform test commits when required, and documentation return to the existing kaizen-docs origin/main. I do not authorize production-code changes, new dependencies, canonical vault mutation, live staging mutation, production MCP migration, Git initialization in kaizen-mcp, remote creation, platform or vault push, Packet 010F, or Milestone 6 closure.
```
