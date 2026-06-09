---
id: kz-dec-01KTPJBRCZXGMNHQMYM6P9T86D
type: decision
status: active
project: kaizen-platform
created: 2026-06-09T16:10:17Z
updated: 2026-06-09T16:10:17Z
review_status: pending
authority: proposed
summary: "Proposes the bounded Milestone 6 operator-surface, repository-placement, actor, connector, and backup posture."
---

# Decision 0014 - Milestone 6 Governed Operator Surface Boundary

## Context

Milestones 1 through 5 proved fixed-root promotion and amendment workflows, immutable operation evidence, exact-plan approval, recovery, Git binding, and human authority. The remaining operator friction is not a missing mutation engine; it is the absence of a narrow typed surface for inspection and a minimal local human interface for operating one reviewed amendment without generic shell commands.

Decision 0011 proposes a progressive hybrid interface direction but does not authorize a broad desktop shell. Decision 0013 already accepts the trusted-local-actor limitation, connector/local-operator split, and temporary local-only repository posture.

Milestone 6 therefore needs a smaller decision that applies those accepted principles to the next implementation slice without reopening first-slice contracts.

## Proposed decision

### 1. Platform owns authoritative enforcement and the minimal console

`C:\dev\kaizen\platform` owns:

- operation-status and evidence-verification logic;
- amendment candidate, plan, approval, execution, and recovery enforcement;
- the fixed-root minimal local operator console;
- unit, integration, regression, and hammer tests;
- implementation documentation tied to runtime behavior.

The console reuses platform APIs. It must not reimplement validation, plan verification, approval binding, event writing, Git checks, execution, or recovery logic.

No new UI repository is created for Milestone 6. A future separate UI repository requires concrete evidence that platform-local packaging creates a real boundary or lifecycle problem.

### 2. MCP remains a temporary typed-adapter proving ground

`C:\dev\kaizen-mcp` may later expose typed adapters for the accepted platform APIs, but remains:

- project-local;
- non-canonical;
- non-Git unless separately authorized;
- incapable of arbitrary shell, SQL, filesystem, Git, remote, or push access;
- subordinate to platform enforcement.

Milestone 6 does not declare the temporary MCP production doctrine or authorize migration to a production MCP repository.

### 3. Tool consequence classes are normative

Every operator or MCP tool must declare one consequence class:

```text
inspect              read-only
prepare-candidate    staging mutation only
plan                 immutable operation-evidence creation
approve              approval-evidence creation
execute              canonical mutation
recover              conditional canonical/evidence mutation
```

A tool description must state its class, fixed-root scope, required bindings, returned evidence, and prohibited capabilities.

No tool may blur approval, execution, or recovery into a generic mutation operation.

### 4. One-operation console boundary

The Milestone 6 console loads one immutable governed amendment operation at a time.

It may display:

- packet ID;
- operation ID;
- destination;
- fixed-root bindings;
- operation type and state;
- plan SHA-256;
- prior canonical SHA-256;
- candidate SHA-256;
- reviewed diff SHA-256;
- approval SHA-256;
- result SHA-256;
- approval actor and timestamp;
- event chain and terminal outcome;
- Git branch, HEAD, cleanliness, and expected binding;
- execute eligibility;
- recovery eligibility;
- mismatch reasons.

It may expose only:

```text
Inspect
Approve exact plan
Execute exactly once
Recover reviewed operation
```

It must not expose:

- generic editing;
- arbitrary path selection;
- arbitrary shell or SQL;
- generic filesystem operations;
- generic Git operations;
- remote creation or push;
- multi-operation batch approval;
- multi-note transactions;
- generalized correction, rollback, delete, move, or supersedence.

### 5. Actor identity remains an explicit trusted-local limitation

Milestone 6 retains a trusted local actor string such as:

```text
owner.local
```

Rules:

- the human supplies the actor only at the approval or execution boundary;
- agents and connectors may not select, infer, or fabricate the owner actor;
- approval evidence and events preserve the actor value;
- the console must label this as a trusted local identifier, not authenticated identity;
- multi-user, remote, account, role, session, credential, and identity-provider work remains deferred.

A stronger local binding may be proposed later only if it uses an already-supported operating-system capability without turning Milestone 6 into an identity-platform project.

### 6. Connector execution is opportunistic; the local operator is guaranteed

Typed connector tools are preferred for inspection, validation, candidate preparation, planning, and narrowly classified operations when the upstream platform permits them.

Connector mutation permission is never a Milestone 6 acceptance dependency.

After one upstream mutation block:

1. verify no approval, event, canonical mutation, or Git mutation occurred;
2. do not retry equivalent routes repeatedly;
3. switch to the fixed-root local operator or console;
4. resume evidence inspection after the human-operated action.

A connector block is recorded as an upstream outcome, not a Kaizen rejection.

### 7. Backup and remote posture remains a separate owner decision

The platform and vault remain local-only during Packet 010A planning and until a separate owner decision.

Before approving the first mutation-capable Milestone 6 implementation packet, the owner must be presented with a backup/remote posture checkpoint covering:

- current local commits and clean-state evidence;
- whether an off-machine backup exists;
- proposed public/private visibility if a remote is considered;
- exact repositories affected;
- push authority;
- restore verification expectations.

Packet 010A, Decision 0014, and the operator-surface specification do not create remotes or authorize pushes for platform or vault.

### 8. Evidence-integrity checks remain narrow

Milestone 6 may implement deterministic checks only for demonstrated friction:

- active roadmap pointer integrity;
- stale current-state versus accepted closure evidence;
- duplicate live-state records where one is required;
- task-packet status versus completion evidence;
- exact changed-path and staged-scope reporting;
- operation evidence hash and event-chain consistency.

Each check requires a finite input scope, deterministic rule, pass condition, explicit failure output, and fixture derived from real evidence.

No daemon, scheduler, generalized workflow engine, universal linter, or automation platform is authorized.

## Repository placement consequence

The default Milestone 6 implementation placement is:

```text
platform enforcement and local console:
C:\dev\kaizen\platform

temporary typed connector adapters:
C:\dev\kaizen-mcp

planning, decisions, specs, packets, audits:
C:\dev\kaizen-docs

canonical notes and event evidence:
C:\dev\kaizen\vault

candidates and immutable operation evidence:
C:\dev\kaizen\staging
```

## Required specification work

Before implementation packets are approved, the proposed `05-specs/milestone-6-governed-operator-surface.md` must be reviewed and any necessary amendments to existing implemented-baseline specs must be separately accepted.

## Compatibility

This decision preserves:

- Decisions 0001 through 0013;
- the authoritative v0.2 standard;
- existing promotion and amendment event history;
- current platform and vault commits;
- first-time promotion behavior;
- governed amendment behavior;
- the local-only remote posture;
- the distinction between the temporary MCP and production doctrine.

## Explicit deferrals

- broad Tauri or desktop shell;
- production MCP packaging;
- durable multi-user identity and authentication;
- remote execution trust;
- Operational Postgres;
- Observatory;
- Internet Marketing Intelligence database;
- Qdrant;
- Hermes live integration;
- providers and website automation;
- generalized edit/delete/move/correct/rollback/supersede;
- multi-note transactions;
- multi-agent orchestration.

## Acceptance gate

This decision remains proposed until:

1. its exact SHA-256 is audited;
2. the operator-surface specification is audited for consistency;
3. any required amendments to implemented-baseline specs are identified exactly;
4. the owner explicitly accepts the exact audited decision hash.

No implementation authority is created by this proposal.
