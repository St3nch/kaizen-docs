# Kaizen Implementation Roadmap v0.2

Status: proposed, non-authoritative planning artifact pending explicit owner acceptance

## Purpose

This roadmap defines the smallest evidence-backed implementation slice after completion of Milestones 1 through 5 and acceptance of Kaizen Project Standard v0.2.

It preserves `IMPLEMENTATION_ROADMAP.md` as the historical v0.1 first-slice roadmap. This file becomes authoritative only after an exact audited version is explicitly accepted by the owner.

## Authority and inherited evidence

This roadmap is subordinate to accepted decisions, accepted specifications, verified implementation evidence, and the authoritative Kaizen Project Standard v0.2.

Inherited checkpoints:

- Milestones 1 through 5 are complete.
- Kaizen Project Standard v0.2 is authoritative.
- Decision 0013 is accepted.
- Result 067 records Milestone 5 closure.
- The platform implementation checkpoint is `845d65f356bd684c2f858f36aef54d0344791e43`.
- The canonical vault checkpoint is `e5e4eec1adc4ef26f9e735333dbb229b7bb59368`.
- The docs checkpoint before this draft is `22c52a32cc5df1ff7b67ffb783faa618ba9c3039`.

No implementation authority is created by this document while its status is proposed.

## Completed milestones

Milestones 1 through 5 remain complete and are not reopened by this roadmap.

- Milestone 1: deterministic platform and validation foundations.
- Milestone 2: canonical vault bootstrap.
- Milestone 3: safe staging, promotion, recovery, and hammer gates.
- Milestone 4: one complete governed project and implementation-return loop.
- Milestone 5: first-slice retrospective, v0.2 reconciliation, acceptance, and closure.

## Roadmap objective

The next slice must reduce operator friction at the already-proven governance boundary without weakening exact-plan approval, fixed-root enforcement, immutable evidence, recovery, Git binding, or human authority.

The shortest useful slice is:

```text
Milestone 6 - Governed operator surface and evidence ergonomics
```

## Milestone 6 objective

Provide a narrow, locally operable surface for inspecting, preparing, approving, executing, and recovering governed amendments, plus deterministic evidence checks that make drift and closure failures easier to detect.

Milestone 6 must prove that consequential governed operations can be reviewed and operated without generic shell, SQL, arbitrary filesystem access, a broad desktop application shell, or reliance on upstream connector mutation permission.

## Included scope

### A. Amendment-native MCP proving-ground tools

In `C:\dev\kaizen-mcp`, add narrowly typed tools that call existing platform enforcement APIs:

```text
kaizen_amendment_status
kaizen_prepare_amendment_candidate
kaizen_plan_amendment
kaizen_approve_amendment
kaizen_execute_amendment
kaizen_recover_amendment
```

Required classifications:

```text
health/status/inspect     read-only
prepare candidate         staging mutation only
plan                      immutable evidence creation
approve                   approval evidence creation
execute                   canonical mutation
recover                   conditional canonical/evidence mutation
```

The MCP remains a temporary proving ground. Milestone 6 does not declare it production doctrine, initialize Git there, or authorize arbitrary command execution.

### B. Minimal local human operator console

Implement the smallest local console needed to operate one immutable governed amendment using platform enforcement code.

The console must display:

- packet ID;
- operation ID;
- destination;
- plan SHA-256;
- prior canonical SHA-256;
- candidate SHA-256;
- reviewed diff SHA-256;
- approval state;
- event state and exact event chain;
- Git branch, HEAD, cleanliness, and root binding;
- canonical/result hash verification.

The only consequential actions are:

```text
Approve exact plan
Execute exactly once
Recover reviewed operation
```

The console must not expose a generic shell, SQL console, arbitrary path picker, arbitrary editor, arbitrary Git action, remote creation, or push.

### C. Typed operation and evidence inspection

Add or consolidate typed read-only inspection for:

- immutable operation state;
- approval and exact-plan verification;
- event-chain completeness and ordering;
- candidate, prior, canonical, and result hash agreement;
- fixed-root binding;
- clean-repository and expected-HEAD checks;
- recovery eligibility.

Inspection may be surfaced through the MCP proving ground and the local console, but authoritative verification logic belongs in the platform repository.

### D. Evidence ergonomics

Implement only narrow deterministic checks justified by observed first-slice friction:

- status-drift checks between current-state records, accepted task packets, and closure evidence;
- index-integrity checks for authoritative entrypoints and active trackers;
- duplicate or stale live-state detection where one active record is required;
- exact changed-path and staged-scope reporting for documentation return.

Generated output must remain reviewable and must not create a general automation framework, daemon, scheduler, or automation zoo.

### E. Security and operations decisions

Milestone 6 planning must produce reviewed decisions or explicit deferrals for:

- durable actor identity and authentication contract direction;
- platform and vault backup/remote checkpoint timing;
- connector-versus-local-operator expectations;
- repository placement for the minimal console.

The default implementation placement is the platform repository because the console operates platform enforcement code and fixed roots. A separate UI repository requires concrete evidence that platform placement creates an actual boundary or packaging problem. A broad UI repository or Tauri shell is not authorized.

### F. Tests and proof

Required tests include:

- unit tests for tool classification and argument binding;
- integration tests against existing amendment APIs;
- fixed-root, packet, operation ID, plan hash, actor, and confirmation-literal enforcement;
- replay and execute-exactly-once behavior;
- recovery eligibility and post-recovery evidence;
- connector allow/block outcome recording without treating connector behavior as guaranteed;
- no regression of first-time promotion or governed amendment behavior;
- hammer tests for every new mutation boundary;
- deterministic drift and index-integrity fixtures.

## Exact exclusions

Milestone 6 does not authorize:

- Operational Postgres;
- Observatory database implementation;
- Internet Marketing Intelligence database implementation;
- Qdrant;
- Hermes live integration;
- paid providers or website automation;
- a production Kaizen MCP migration;
- a broad Tauri or desktop application shell;
- generalized edit, delete, move, correction, rollback, or supersedence execution;
- multi-note transactions;
- remote multi-user execution;
- multi-agent orchestration;
- platform or vault remote creation;
- vault push;
- arbitrary shell, SQL, filesystem, or Git access.

## Repository boundaries

### `C:\dev\kaizen\platform`

Owns:

- authoritative enforcement logic;
- amendment and operation inspection APIs;
- the fixed-root local console;
- mutation-boundary tests and hammer tests;
- implementation documentation tied to runtime behavior.

### `C:\dev\kaizen-mcp`

Owns only:

- temporary typed adapters and connector-facing proving-ground tests;
- metadata and descriptions that accurately classify tool consequences;
- evidence about upstream connector allow/block behavior.

It remains non-Git unless separately authorized.

### `C:\dev\kaizen-docs`

Owns:

- this roadmap;
- accepted decisions and specification changes;
- reviewed task packets;
- audits and closure evidence.

### `C:\dev\kaizen\vault`

Remains canonical governed Markdown and append-only event evidence. No roadmap drafting work mutates it.

### `C:\dev\kaizen\staging`

Remains non-canonical operation evidence and candidate storage.

## Dependencies

Before implementation begins:

1. the owner accepts an exact audited roadmap hash;
2. repository placement for the minimal console is resolved or accepted as platform-local;
3. actor-identity scope is explicitly bounded so Milestone 6 does not accidentally become an authentication-platform project;
4. required spec amendments are reviewed;
5. each implementation task packet is separately reviewed and accepted.

Runtime dependencies must remain limited to what the platform already uses unless a task packet proves a new dependency necessary.

## Required decisions and specification updates

Before or within the first approved planning packet, review whether updates are required to:

- MCP/operator surface descriptions;
- amendment operation metadata and status schema;
- tool consequence classification;
- actor binding and trusted-local-actor limitation;
- backup and remote checkpoint policy;
- hammer-test strategy for connector adapters and the local console;
- implementation-return and evidence-integrity checks.

No accepted specification may be silently rewritten. Changes require reviewed amendments or new accepted decisions.

## Human-only boundaries

The following remain human-only:

- accepting this roadmap;
- accepting task packets;
- approving the exact immutable plan for a consequential operation;
- choosing the actor identity accepted for the operation;
- deciding whether to execute after reviewing hashes and diff evidence;
- authorizing any remote or push posture change;
- accepting milestone closure.

No connector, MCP tool, console, or agent may infer these approvals.

## Upstream connector boundary

Connector mutation permission is testable but never guaranteed.

After one upstream mutation block:

1. verify no approval, event, canonical mutation, or Git mutation occurred;
2. do not retry equivalent routes repeatedly;
3. use the fixed-root local operator or console;
4. resume evidence inspection afterward.

A connector block is recorded as an upstream outcome, not a Kaizen rejection.

## Implementation-return requirements

Each approved implementation packet must return:

- exact source repository commit;
- focused, integration, regression, and hammer-test results;
- changed-path and staged-scope evidence;
- deviations and discoveries;
- security/steward audit;
- governed task-packet completion amendment;
- separate governed current-state amendment;
- scoped local vault commits;
- closure audit.

The canonical vault remains local-only unless a separate owner decision changes that posture.

## Documentation-return requirements

Milestone 6 closure must update or create, as applicable:

- active implementation roadmap status;
- relevant accepted decisions and specifications;
- task-packet completion records;
- current-state record;
- repository and operator documentation;
- test and hammer evidence;
- connector outcome evidence;
- final Milestone 6 closure audit.

Documentation return must not silently repair the known non-blocking editorial sentence in the authoritative v0.2 standard.

## Task-packet decomposition

The expected sequence is:

### Packet 010A - Define Milestone 6 contracts and repository placement

Documentation-only. Resolve exact tool metadata, operation-status contract, console placement, actor-identity boundary, backup checkpoint, and required spec amendments.

### Packet 010B - Implement platform inspection and evidence-integrity checks

Read-only platform APIs and deterministic status/index/drift checks. No new canonical mutation action.

### Packet 010C - Implement amendment-native MCP proving-ground adapters

Typed adapters, metadata, and tests against existing platform APIs. Connector execution remains opportunistic.

### Packet 010D - Implement minimal fixed-root local operator console

One-operation inspection, exact-plan approval, execute-once, and reviewed recovery only.

### Packet 010E - Run integrated hammer and connector-boundary proof

Prove enforcement, replay resistance, recovery, connector allow/block recording, and no regression.

### Packet 010F - Complete governed implementation return and Milestone 6 closure

Return evidence to canonical Kaizen, update current state, and produce a closure audit.

Packets may be split further when an audit finds a real security or review boundary. They must not be combined merely for convenience if that would collapse planning, approval, execution, and closure gates.

## Milestone 6 exit criteria

Milestone 6 is complete only when:

1. the exact roadmap version was accepted before implementation;
2. all implementation occurred through separately accepted task packets;
3. typed amendment tools are narrow, fixed-root, and correctly classified;
4. the local console exposes only inspect, exact approve, execute-once, and reviewed recovery;
5. connector blocks are handled without bypassing Kaizen controls;
6. all new mutation boundaries pass focused, integration, regression, and hammer tests;
7. first-time promotion and governed amendment behavior show no regression;
8. deterministic evidence checks detect the defined stale-status and index-integrity failures;
9. implementation-return amendments and local vault commits are complete;
10. a security/steward closure audit passes;
11. the owner explicitly accepts Milestone 6 closure.

## Stop gates

Stop and report before proceeding when:

- any expected repository branch, HEAD, cleanliness, remote, fixed root, or authoritative hash differs;
- a required decision or specification remains contradictory;
- a task requires arbitrary shell, filesystem, SQL, or Git access;
- console scope expands beyond one governed operation;
- actor identity work expands into a general identity platform;
- connector behavior is treated as guaranteed;
- a deferred system becomes necessary without a separate reviewed roadmap amendment;
- an operation plan, approval, diff, event chain, or result hash does not verify exactly;
- owner approval is absent or ambiguous.

## Deferred milestone families

After Milestone 6, future roadmap selection must be evidence-backed. Candidate families remain deferred, including:

- durable identity and multi-user authorization;
- backup and remote operationalization;
- production MCP packaging;
- Operational Postgres and bounded Observatory work;
- Qdrant retrieval;
- Hermes read-only then constrained staging integration;
- progressive hybrid human interface beyond the minimal operator console;
- Internet Marketing Intelligence systems.

No sequence is accepted here.

## Acceptance gate

This proposed roadmap requires:

1. a security/steward audit bound to its exact SHA-256;
2. correction of any audit blockers;
3. explicit owner acceptance of the exact audited SHA-256;
4. a separate owner gate before any implementation packet is approved.

Until then:

```text
Implementation Roadmap v0.2: proposed
Milestone 6: formally defined for review, not authorized
Implementation: prohibited
```
