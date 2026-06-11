---
id: kz-spec-01KTW2M8N4Q6S8W0Y2Z4ABCDEF
type: spec
status: draft
project: kaizen-platform
created: 2026-06-11T19:40:00Z
updated: 2026-06-11T19:40:00Z
review_status: pending-review
authority: proposed
summary: "Milestone 8 reliability and known-defect closure for Kaizen platform, Kaizen MCP, and Go8 proving-ground tooling."
---

# Milestone 8 - Reliability and Known-Defect Closure

## Status

Proposed milestone definition. This document authorizes no code change, MCP tool change, connector migration, production packaging, authority expansion, remote creation, vault push, mock-project execution, or database work until the owner accepts the exact audited SHA-256 and separately approves implementation task packets.

## Primary outcome

Kaizen's accepted governed workflow and temporary MCP proving grounds behave deterministically under the known failure conditions already observed, report their real state truthfully, and can be started, stopped, reconnected, inspected, and tested through documented operator procedures without widening authority or pretending the proving grounds are production systems.

## Why this milestone is next

Milestone 7 proved durable recovery. Milestone 9 will exercise the full governed implementation-return loop on a controlled mock project.

Before that pilot, Kaizen must close the known defects and operator friction that would otherwise turn the pilot into a debugging exercise:

- the prepared-candidate read-only loader previously used a stale or inconsistent live-scope guard;
- one amendment-candidate concurrency hammer test intermittently returned `destination_path_invalid` instead of the expected `idempotency_conflict`;
- MCP startup, shutdown, PID, port, and reconnect behavior has required repeated manual recovery;
- connector tool discovery and upstream blocking have sometimes disagreed with the live server state;
- version and tool-count reporting need exact registry-backed proof;
- the startup and connector runbooks are fragmented across chat and repository files;
- Ruff is declared in the Kaizen MCP development dependencies, while earlier platform return evidence recorded Ruff as unavailable in the platform environment.

Milestone 8 closes those known reliability gaps before Milestone 9.

## Authority and boundaries

Accepted authority:

- Kaizen Project Standard v0.2;
- accepted Roadmap v0.3 at SHA-256 `5feec5bef8bf48c30be36fddfe1547f47810145407d1e9cc47301e0c6dddc248`;
- Milestone 7 closure acceptance in Result 132;
- current platform HEAD `1b8be1d6d42d768587dddb2be8415fa24b670561`;
- current vault HEAD `37e756a8e85157bd83c22e832af285e1992f8d8e`;
- current Go8 HEAD `5eccbc71dff5752b083b6b2bd019416d4e5baf73`;
- Kaizen MCP remains a temporary non-Git proving ground at `C:\dev\kaizen-mcp`.

Milestone 8 must not:

- declare Kaizen MCP or Go8 production-ready;
- migrate production MCP architecture or packaging;
- add arbitrary shell, filesystem, SQL, Git-remote, or process-execution capability;
- broaden fixed roots or mutation authority without a separate accepted decision;
- create platform or vault remotes or pushes;
- begin the Milestone 9 mock project;
- design or implement Postgres, Qdrant, Hermes, UI, Observatory, or Internet Marketing Intelligence systems;
- change canonical Markdown or governance semantics except through separately approved governed amendment packets when required for the final return.

## Reliability track R1 - Prepared-candidate read-only scope correctness

### Problem

The roadmap and earlier return evidence identify a stale prepared-candidate loader guard or live-scope inconsistency. Current Kaizen MCP adapter code conditionally resolves live scope when a `packet_id` is supplied and loads without live scope when it is omitted.

Milestone 8 must determine the exact current state rather than assume the defect still exists.

### Required proof

- identify the original defect and the exact code path that produced it;
- compare platform loader contract, Kaizen MCP adapter behavior, and current tests;
- prove read-only loading succeeds for a valid prepared candidate with a correct packet binding;
- prove loading without an optional packet binding still verifies operation and evidence hashes;
- prove wrong packet, wrong live binding, stale vault HEAD, stale platform HEAD, changed governance log, malformed preparation evidence, and missing evidence fail with deterministic structured errors;
- prove the read-only loader creates no mutex, temporary evidence, canonical mutation, Git change, or operation event;
- remove stale comments, guards, or contradictory documentation only when evidence shows they are obsolete;
- preserve fixed roots and existing authority.

### Exit condition

One accepted contract and test matrix demonstrate that read-only prepared-candidate loading is correct, deterministic, and suitable for Milestone 9 field validation.

## Reliability track R2 - Amendment concurrency determinism

### Problem

During Generation 2 restored-platform testing, `test_concurrent_conflicting_requests_cannot_mix_evidence` produced one intermittent mismatch:

```text
expected: idempotency_conflict
observed: destination_path_invalid
```

The focused test then passed 20 of 20 reruns, and a full rerun passed 276 of 276.

### Required proof

- reproduce the race through repeated focused and stress runs;
- inspect the ordering between preparation-directory creation, destination reads, evidence installation, and idempotency checks;
- determine whether `destination_path_invalid` can arise from a partial concurrent preparation state, an unrelated native-filesystem race, or a test fixture defect;
- define the correct deterministic contract for conflicting requests using one operation ID;
- fix the implementation or test only after root cause is proven;
- prove exactly one preparation wins;
- prove all losing conflicting requests return the accepted deterministic error code;
- prove candidate, diff, metadata, and hashes never mix between contenders;
- retain hammer coverage for identical concurrent requests, conflicting concurrent requests, partial evidence, stale evidence, and interrupted preparation.

### Exit condition

The accepted stress profile completes without mixed evidence or error-code drift across the required repetition count.

## Reliability track R3 - MCP lifecycle and local operator behavior

### Problem

Operator sessions have encountered stale listeners, duplicate processes, unsigned-script execution-policy failures, missing startup entry points, connector 502s, and repeated manual restart sequences.

### Required proof

For both Go8 and Kaizen MCP:

- one documented local startup command;
- one documented ngrok startup command using the reserved custom domain;
- deterministic port and host reporting;
- clear behavior when the port is already occupied;
- PID-file behavior defined and tested where a PID file exists;
- stale PID detection and safe refusal or replacement semantics;
- no duplicate listener after repeated start attempts;
- graceful shutdown procedure documented;
- restart procedure documented;
- execution-policy-safe PowerShell invocation documented without changing machine-wide policy;
- connector refresh/recreate procedure documented;
- health verification performed before mutation tools are used;
- no background service, scheduler, daemon installation, or auto-start registration.

### Exit condition

A fresh operator can start, verify, stop, and restart both servers and tunnels from repository runbooks without relying on chat history.

## Reliability track R4 - Health, version, and tool-registry truthfulness

### Problem

A connector may expose stale tool schemas or fail to reflect the server's current registry. Tool counts and version strings must come from one authoritative source and be testable.

### Required proof

- define the authoritative version source for Go8 and Kaizen MCP;
- define the authoritative registered-tool inventory for each server;
- make health return exact service name, version, framework version where relevant, transport, host, port, fixed roots, mutation status, remote status, and tool count when supported;
- prove the reported tool count equals the actual registered tool inventory;
- prove duplicate tool names are impossible;
- prove deprecated aliases are explicit and intentional;
- prove connector discovery after restart matches the live registry;
- document that upstream connector blocking is distinct from server rejection;
- preserve redaction and avoid exposing secrets, tokens, or private configuration values.

### Exit condition

Health and registry tests fail whenever reported version, tool count, tool name, or runtime posture drifts from the actual server.

## Reliability track R5 - Runbooks, tests, and lint policy

### Required proof

- consolidate Go8 startup, ngrok, connector, health, restart, and troubleshooting instructions in the Go8 repository;
- consolidate equivalent Kaizen MCP instructions in the Kaizen MCP repository;
- document exact reserved endpoints:

```text
Go8 local: http://127.0.0.1:8770/mcp
Go8 public: https://go8.ngrok.dev/mcp
Kaizen local: http://127.0.0.1:8765/mcp
Kaizen public: https://kaizen.ngrok.dev/mcp
```

- document the non-Git status of Kaizen MCP and how accepted changes are evidenced;
- define whether Ruff is mandatory for platform, Kaizen MCP, Go8, or only selected repositories;
- ensure declared development dependencies match the accepted lint policy;
- run syntax/compile, focused tests, full tests, static capability scans, and lint where required;
- document known upstream connector limitations without treating them as local implementation defects;
- remove stale instructions that reference nonexistent `start.ps1` or other missing entry points.

### Exit condition

Repository runbooks and automated checks agree with the actual supported operator workflow.

## Required evidence inventory before implementation

The first packet must produce a bounded defect register containing at least:

- defect identifier;
- first observed evidence;
- current reproducibility;
- affected repository and code path;
- expected contract;
- observed behavior;
- consequence class;
- security impact;
- proposed proof method;
- disposition: fix, test gap, documentation gap, upstream limitation, obsolete, or deferred;
- closure evidence required.

Initial required candidates:

```text
M8-D01 prepared-candidate loader live-scope inconsistency
M8-D02 intermittent concurrency error-code drift
M8-D03 stale listener or duplicate-process startup behavior
M8-D04 unsigned PowerShell script invocation friction
M8-D05 fragmented Go8 and Kaizen startup/ngrok instructions
M8-D06 health version and tool-count registry drift risk
M8-D07 connector tool discovery stale after restart or recreation
M8-D08 upstream connector safety block before server execution
M8-D09 Go8 502 behavior after long-running sessions
M8-D10 Ruff availability and repository lint-policy mismatch
M8-D11 Kaizen MCP non-Git change provenance and reproducibility
M8-D12 PID-file lifecycle and stale PID behavior
```

New defects may be added only with evidence. Milestone 8 is not permission for broad cleanup.

## Required test classes

### Platform

- prepared-candidate loader contract tests;
- amendment concurrency stress and hammer tests;
- dirty-repository behavior;
- stale live-binding behavior;
- evidence-integrity and fail-closed tests;
- full platform suite.

### Kaizen MCP

- adapter contract tests;
- read-only versus mutation-gated boundary tests;
- health and registry tests;
- fixed-root and no-generic-capability tests;
- lifecycle-script tests where feasible;
- full Kaizen MCP suite;
- Ruff and compile checks if accepted by policy.

### Go8

- health and registered-tool inventory tests;
- fixed-root tests;
- exact-path mutation tests;
- no-shell/no-generic-execution static scans;
- startup and port-conflict tests where feasible;
- full Go8 suite;
- lint and compile checks if accepted by policy.

### Integrated operator proof

- fresh start of both local servers;
- fresh start of both ngrok tunnels;
- connector refresh or recreation;
- health checks;
- one read-only tool from each server;
- one mutation-gated Kaizen planning operation in disposable or accepted bounded evidence scope;
- stop and restart;
- repeated health and registry verification;
- no canonical mutation unless a separately approved implementation packet requires it.

## Failure injections

Implementation packets must include at least:

1. stale PID file with no live process;
2. PID file pointing to an unrelated live process;
3. port already occupied;
4. duplicate start attempt;
5. missing virtual environment;
6. unsigned-script execution-policy restriction;
7. connector schema stale after server restart;
8. upstream block before server execution;
9. wrong packet binding on read-only prepared-candidate load;
10. stale vault, platform, or governance-log binding;
11. concurrent conflicting preparation requests;
12. health tool-count mismatch injected in a disposable test fixture.

Expected behavior is deterministic, bounded, and fail-closed. No test may kill an unrelated process, mutate live canonical files, expose secrets, or weaken authority.

## Packet sequence

### 012A - Defect register and contract freeze

Read-only inspection and documentation only:

- inventory current code, tests, scripts, versions, health payloads, and tool registries;
- reproduce or retire each initial defect candidate;
- define exact accepted contracts and test profiles;
- select lint policy;
- return the implementation packet split.

### 012B - Platform reliability defects

- close prepared-candidate loader defect or prove it already closed;
- root-cause and fix concurrency error-code drift;
- add platform hammer and dirty-repository proof;
- no MCP lifecycle changes.

### 012C - Kaizen MCP reliability

- stabilize lifecycle scripts, health, registry, version reporting, and adapter tests;
- reconcile read-only loader adapter behavior with the accepted platform contract;
- keep Kaizen MCP temporary and non-Git;
- no production packaging or authority expansion.

### 012D - Go8 reliability and runbooks

- stabilize lifecycle and registry behavior;
- close 502 or stale-session defects that are reproducible locally;
- consolidate operator runbooks;
- preserve the exact bounded tool surface.

### 012E - Integrated proof and governed return

- run cross-server lifecycle and connector proof;
- run all accepted failure injections;
- record versions, tool registries, tests, limitations, and upstream-only issues;
- update canonical current state through governed amendment only if required;
- complete Milestone 8 closure audit and owner acceptance.

The packet split may change after 012A only when evidence justifies it. Implementation authority remains packet-specific.

## Milestone exit criteria

Milestone 8 is complete only when:

1. the defect register is accepted and every in-scope defect has a disposition;
2. prepared-candidate read-only loading has one proven deterministic contract;
3. concurrency stress produces no mixed evidence or error-code drift;
4. dirty-repository and stale-binding behavior fail closed as required by Milestone 9;
5. Go8 and Kaizen MCP startup, port, PID, shutdown, restart, ngrok, connector, and health procedures are documented and proven;
6. health version and tool-registry reporting matches actual runtime registration;
7. fixed roots and no-generic-capability boundaries remain intact;
8. lint policy is explicit and enforced consistently;
9. focused, full, hammer, static capability, compile, and accepted lint checks pass;
10. integrated connector proof distinguishes upstream blocks from local server failures;
11. known upstream-only limitations are documented rather than patched around unsafely;
12. no production MCP migration or authority widening occurs;
13. repositories and canonical roots remain clean except for separately approved governed returns;
14. implementation returns and security/steward audits pass;
15. the owner explicitly accepts exact Milestone 8 closure evidence.

## Explicit exclusions

- production MCP architecture, packaging, installation, hosting, authentication, or multi-user service;
- new remote APIs or cloud deployment;
- generalized process control or arbitrary shell tools;
- broad filesystem access;
- platform or vault remote creation or push;
- mock-project execution;
- Postgres, Qdrant, Hermes, UI, Observatory, or Internet Marketing Intelligence work;
- automatic backup scheduling or cleanup;
- generalized canonical correction, deletion, move, rollback, or multi-note transaction semantics;
- unrelated refactoring, dependency upgrades, or style cleanup without defect evidence.

## Required planning return

Before implementation, return:

- exact repository and runtime checkpoints;
- initial defect register with disposition evidence;
- accepted contracts for R1 through R5;
- test and failure-injection matrix;
- lint policy recommendation;
- packet sequence and exact scope per packet;
- security/steward audit;
- exact owner acceptance phrase bound to this specification SHA-256.
