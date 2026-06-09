---
id: kz-tp-01KTPHNT95ZK4MWDRWSY2Z6VH2
type: task-packet
status: draft
project: kaizen-platform
summary: "Define the exact Milestone 6 contracts, repository placement, and specification changes before any implementation begins."
created: 2026-06-09T15:59:23Z
updated: 2026-06-09T15:59:23Z
review_status: pending
authority: proposed
primary_spec: IMPLEMENTATION_ROADMAP_V0.2.md
related_specs:
  - 01-project-standard/kaizen-project-standard-v0.2.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/promotion-event-schema.md
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Task Packet 010A - Define Milestone 6 Contracts and Repository Placement

> Packet 010A is a proposed documentation-only planning packet. It does not authorize implementation, runtime changes, MCP changes, canonical vault mutation, staging mutation, plan generation, approval, execution, recovery, remote creation, or push outside `kaizen-docs`.

## Objective

Produce the smallest complete set of reviewed Milestone 6 planning contracts needed before implementation task packets can be drafted.

This packet must resolve:

1. exact amendment-native tool metadata and consequence classification;
2. the authoritative operation-status and evidence-inspection contract;
3. repository placement and packaging boundary for the minimal local operator console;
4. the bounded actor-identity posture for Milestone 6;
5. platform and vault backup/remote checkpoint posture;
6. connector-versus-local-operator expectations;
7. the exact accepted specification amendments or new decisions required before implementation;
8. the decomposition and acceptance gates for Packets 010B through 010F.

## Authorization boundary

The owner accepted Implementation Roadmap v0.2 at SHA-256:

```text
1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc
```

and authorized Milestone 6 planning only.

This packet remains proposed until separately approved. Drafting and auditing it do not authorize implementation.

## Read first

- `IMPLEMENTATION_ROADMAP_V0.2.md`
- `03-research-results/068-implementation-roadmap-v0.2-and-milestone-6-security-steward-audit.md`
- `03-research-results/069-implementation-roadmap-v0.2-owner-acceptance-and-milestone-6-planning-authorization.md`
- `01-project-standard/kaizen-project-standard-v0.2.md`
- `04-design-decisions/0011-progressive-hybrid-human-interface-direction.md`
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`
- `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/promotion-event-schema.md`
- `C:\dev\kaizen-mcp\README.md`
- `C:\dev\kaizen-mcp\docs\ROADMAP.md`
- `C:\dev\kaizen-mcp\docs\UPSTREAM_MUTATION_GATING.md`

Read additional evidence only when needed to verify a current claim.

## Bound state

```text
roadmap path: C:\dev\kaizen-docs\IMPLEMENTATION_ROADMAP_V0.2.md
roadmap sha256: 1dd4eb3ae80b0d855091b26cf598b9a744a5141648b4739580bb90871f2e11bc
roadmap audit: Result 068
roadmap acceptance: Result 069
platform head: 845d65f356bd684c2f858f36aef54d0344791e43
vault head: e5e4eec1adc4ef26f9e735333dbb229b7bb59368
mcp service: kaizen-mcp 0.1.0
mcp fixed roots: platform, staging, vault, docs
```

Any mismatch stops work before writing planning conclusions.

## Required outputs

### 1. Milestone 6 contract decision set

Draft proposed decisions or explicit accepted-spec amendment proposals covering:

- tool consequence classes:
  - read-only inspection;
  - staging-only candidate preparation;
  - immutable plan creation;
  - approval evidence creation;
  - canonical execution;
  - conditional recovery;
- exact input binding for packet ID, operation ID, plan hash, actor, destination, fixed roots, and confirmation literal;
- exact status and evidence fields exposed to human and connector consumers;
- trusted-local-actor limitation and deferred durable identity boundary;
- connector block handling and the one-block rule;
- backup and remote checkpoint timing;
- repository placement for the minimal local console.

### 2. Operation-status contract

Define a typed read-only result that can report, without mutating state:

- operation type and state;
- packet ID and operation ID;
- canonical destination;
- fixed-root bindings;
- plan SHA-256;
- prior, candidate, reviewed-diff, approval, and result hashes;
- approval actor and timestamp when present;
- complete event chain and terminal outcome;
- Git branch, HEAD, cleanliness, and expected binding;
- execute eligibility;
- recovery eligibility;
- explicit mismatch reasons.

The contract must distinguish absent, incomplete, ready, approved, committed, recovered, and invalid states without inventing a generalized workflow engine.

### 3. MCP tool metadata contract

For each proposed amendment-native tool, define:

- exact name;
- consequence class;
- read-only or mutation annotation;
- required inputs;
- returned evidence;
- prohibited capabilities;
- failure and mismatch behavior;
- whether connector execution is opportunistic or required.

Candidate tools:

```text
kaizen_amendment_status
kaizen_prepare_amendment_candidate
kaizen_plan_amendment
kaizen_approve_amendment
kaizen_execute_amendment
kaizen_recover_amendment
```

Tool descriptions must never imply generic shell, arbitrary filesystem, arbitrary Git, remote, or push access.

### 4. Minimal local console placement decision

Evaluate only these bounded choices:

1. platform-local console in `C:\dev\kaizen\platform`;
2. a separately packaged future UI only if concrete evidence proves platform placement inadequate.

The default recommendation is platform-local.

The decision must state:

- why the chosen repository owns the runtime contract;
- what dependencies are permitted;
- what user interface technology is deliberately not selected yet;
- how one immutable operation is loaded;
- which fields are displayed;
- which exact actions are exposed;
- which arbitrary actions are prohibited;
- how the console reuses platform enforcement rather than reimplementing it.

A broad Tauri application, generic desktop shell, arbitrary editor, SQL console, file manager, Git client, or multi-operation dashboard is out of scope.

### 5. Actor-identity boundary

Produce a proposed bounded rule for Milestone 6:

- retain an explicit trusted local actor string unless a minimal stronger binding is already supported by the operating environment and can be verified without creating an identity platform;
- record actor input in approval evidence and events;
- do not claim durable authentication where none exists;
- defer multi-user identity, authorization services, account systems, and remote authentication.

### 6. Backup and remote checkpoint

Produce a planning recommendation, not a remote mutation.

It must state:

- current platform and vault remote posture;
- current accepted local-only risk;
- the exact future owner decision required;
- minimum backup/checkpoint evidence expected before or after Milestone 6 closure;
- that Packet 010A cannot create remotes or push the vault.

### 7. Evidence-integrity check definitions

Define only checks justified by observed first-slice friction:

- active roadmap pointer integrity;
- stale current-state versus accepted completion evidence;
- duplicate live-state records where one is required;
- task-packet status versus closure evidence;
- exact changed-path and staged-scope reporting;
- operation evidence hash and event-chain consistency.

For every check, define:

- input scope;
- deterministic rule;
- pass condition;
- failure output;
- fixture derived from real evidence;
- whether it belongs in platform code, docs-only lint, or remains manual.

Do not define a general automation framework, daemon, scheduler, or universal vault linter.

### 8. Required specification changes

Identify exact proposed changes, if any, to:

- `05-specs/kaizen-validation-gate-spec.md`;
- `05-specs/kaizen-hammer-test-strategy.md`;
- `05-specs/staging-and-promotion-workflow.md`;
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`;
- `05-specs/promotion-event-schema.md`;
- MCP proving-ground documentation.

Do not silently edit accepted contracts. Each change must be represented as a proposed decision, proposed amendment, or separately reviewable specification diff.

### 9. Follow-on packet contracts

Define the minimum objectives and boundaries for:

- Packet 010B: platform inspection and evidence-integrity checks;
- Packet 010C: amendment-native MCP proving-ground adapters;
- Packet 010D: minimal fixed-root local operator console;
- Packet 010E: integrated hammer and connector-boundary proof;
- Packet 010F: governed implementation return and Milestone 6 closure.

No follow-on packet becomes approved through Packet 010A.

## Scope

- repository and evidence verification;
- documentation research using current authoritative files;
- proposed decisions and specification amendment drafts;
- precise interface and metadata contracts;
- task-packet decomposition;
- security/steward audit of Packet 010A outputs;
- docs-only commits and push to the existing `kaizen-docs` remote after exact review.

## Non-scope

- Python or runtime implementation;
- changes in `C:\dev\kaizen\platform`;
- changes in `C:\dev\kaizen-mcp`;
- canonical vault or staging mutation;
- plan generation, approval, execution, or recovery;
- creating or approving Packets 010B through 010F;
- Tauri or broad UI work;
- Operational Postgres, Observatory, Internet Marketing Intelligence, Qdrant, Hermes, providers, website automation, or orchestration;
- generalized edit, delete, move, correction, rollback, supersedence, or multi-note transactions;
- platform or vault remote creation;
- vault push.

## Constraints

- Preserve the accepted roadmap bytes and SHA-256.
- Search before creating new decisions or specs.
- Diff before writing.
- Keep every proposal marked proposed until explicit owner acceptance.
- Do not infer implementation approval from roadmap acceptance.
- Keep platform enforcement authoritative.
- Keep the MCP a temporary proving ground.
- Treat connector mutation as opportunistic, never guaranteed.
- Stop on branch, HEAD, hash, root, cleanliness, remote, or authority mismatch.
- Prefer the smallest contract that protects a demonstrated boundary.

## Required audit

Before Packet 010A can be presented for approval, a separate security/steward audit must verify:

- roadmap hash and owner acceptance binding;
- documentation-only scope;
- no implementation leakage;
- no broad UI leakage;
- no deferred-system leakage;
- correct repository ownership;
- actor-identity proportionality;
- connector/local-operator separation;
- exact follow-on packet gates;
- no canonical vault or staging mutation;
- no platform or MCP modification.

## Acceptance criteria

Packet 010A is ready for owner review only when:

1. every required output is represented by an exact proposed document or an explicit evidence-backed no-change finding;
2. all proposed decisions and spec changes identify their authority status;
3. repository placement for the minimal console is resolved;
4. actor identity remains bounded and honest;
5. backup/remote posture is explicitly deferred to an owner decision without mutation;
6. follow-on packet boundaries are precise and non-overlapping;
7. a security/steward audit passes;
8. only reviewed `kaizen-docs` planning files are committed and pushed;
9. the accepted roadmap hash remains unchanged;
10. the owner separately approves Packet 010A before any work under it begins.

## Current gate

```text
Packet 010A: proposed, not approved
Milestone 6 planning: authorized at roadmap level
Packet 010A execution: prohibited pending separate owner approval
Milestone 6 implementation: prohibited
```
