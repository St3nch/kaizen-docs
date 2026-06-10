# Kaizen Roadmap v0.3

Status: draft for owner review
Date: 2026-06-10
Owner: Kaizen project steward

## Purpose

This is the concise forward roadmap after Milestone 6 closure.

It replaces the old roadmap's role as the place to understand current direction. Historical planning remains available in Git and in the temporary `ROADMAP.md.bak` recovery reference until this roadmap is reviewed and accepted.

This roadmap does not authorize implementation. Every milestone still requires:

```text
evidence
-> scoped milestone definition
-> security/steward audit
-> exact owner acceptance
-> task packet
-> exact implementation approval
-> implementation and tests
-> governed return
-> closure audit
-> owner closure acceptance
```

## Current checkpoint

```text
Kaizen Project Standard v0.2: authoritative
Milestones 1-6: closed
Packet 010F: complete
Platform HEAD: 1b8be1d6d42d768587dddb2be8415fa24b670561
Vault HEAD: fc4b03397d770f9b4a8a2f5e7f71e33981bcd181
Docs closure commit: 511c325d4abcbbc9e05b2f4f8ee2379f18607b94
Milestone 7: not defined or authorized
```

Milestone 6 proved the governed Markdown pipeline, fixed-root promotion and amendment controls, deterministic evidence, human approval gates, constrained MCP adapters, minimal local operator console, recovery, hammer testing, and canonical implementation return.

## What happens next

The next work is roadmap selection, not implementation.

### Step 1 - Post-Milestone 6 retrospective

Create one concise evidence report covering:

- what Milestone 6 proved;
- real operational friction;
- temporary components that must not become permanent by accident;
- defects and follow-up items;
- controls that worked and should remain unchanged;
- deferred capabilities that are now justified by actual need;
- capabilities still too early to build.

Required evidence includes:

- intermittent connector/ngrok availability;
- temporary non-Git `kaizen-mcp` packaging;
- stale prepared-candidate loader guard;
- manual runtime startup and refresh friction;
- missing platform Ruff environment;
- local-only repository and backup posture;
- successful exact-plan approval, execute-once, recovery, event-chain, and governed-return behavior.

### Step 2 - Rank the deferred milestone families

Evaluate each candidate against:

```text
current blocker severity
risk reduction
operator time saved
reversibility
security impact
required research
implementation cost
value to later milestones
```

Candidate families:

1. MCP production operationalization and reliability;
2. backup and remote operationalization;
3. progressive human interface beyond the minimal console;
4. Operational Postgres and bounded Observatory work;
5. Qdrant retrieval and rebuild tooling;
6. Hermes read-only, then constrained staging integration;
7. Internet Marketing Intelligence systems;
8. durable identity and multi-user authorization.

No family is authorized merely because it appears here.

### Step 3 - Select and define Milestone 7

The selected milestone must have:

- one primary outcome;
- explicit repository placement;
- exact included and excluded capabilities;
- accepted contracts before implementation;
- test and hammer requirements;
- human-only decisions;
- migration and rollback boundaries;
- implementation-return requirements;
- closure criteria.

### Step 4 - Publish an accepted implementation roadmap

After the milestone-selection audit:

1. draft the next implementation roadmap snapshot;
2. audit its exact SHA-256;
3. correct blockers;
4. obtain explicit owner acceptance;
5. draft the first task packet;
6. do not implement until that exact packet is separately approved.

## Recommended Milestone 7 candidate

### MCP production operationalization and reliability

This is the strongest current candidate because it addresses friction already observed during real governed work.

Possible outcome:

> Convert the temporary Kaizen MCP proving ground into a reliable, versioned, locally operable service without widening its tool authority.

Likely included:

- correct the stale prepared-candidate loader guard;
- define production repository placement and packaging;
- deterministic startup, shutdown, restart, and health checks;
- version and tool-registry verification;
- stable connector/ngrok operating procedure;
- structured runtime diagnostics;
- bounded configuration and fixed-root verification;
- backup and recovery requirements for MCP configuration and evidence;
- migration plan from the temporary proving ground;
- focused, integration, regression, connector-boundary, and hammer tests.

Likely excluded:

- new mutation semantics;
- generic shell, filesystem, Git, SQL, or remote tools;
- broad desktop UI or Tauri;
- multi-user identity platform;
- Hermes integration;
- Postgres, Qdrant, or Internet Marketing Intelligence implementation;
- vault remote creation or push unless separately selected and approved.

This recommendation is not authorization. The retrospective may prove that backup operationalization should precede it or be separated from it.

## Follow-up ledger

These items must be evaluated during roadmap selection:

| Item | Current disposition |
|---|---|
| Prepared-candidate loader stale live-root guard | confirmed defect; candidate for next milestone |
| Intermittent MCP/ngrok HTTP 404 | confirmed operational friction |
| Temporary non-Git `kaizen-mcp` proving ground | migration decision required |
| Platform Ruff unavailable | tooling policy decision required |
| Local-only platform and vault repositories | backup strategy decision required |
| Go8 connector caching and occasional upstream blocks | operating constraint; do not weaken controls |
| Trusted local actor string | accepted first-slice limitation; identity work remains deferred |
| Tauri or broad UI | deferred until operator workflow proves the need |
| Postgres, Qdrant, Hermes, IMI | deferred; require separate evidence-backed milestones |

## Standing boundaries

Until a new roadmap and milestone are accepted:

- no Milestone 7 implementation;
- no production MCP migration;
- no vault push or remote creation;
- no broad UI;
- no new database or retrieval infrastructure;
- no Hermes write access;
- no generalized correction, delete, move, rollback, or multi-note mutation semantics;
- no inferred owner approval.

## Immediate authorized planning sequence

```text
post-Milestone 6 retrospective
-> deferred-family ranking
-> Milestone 7 recommendation
-> roadmap security/steward audit
-> exact owner roadmap acceptance
-> first Milestone 7 task packet
-> separate owner implementation gate
```

## Completion condition for this roadmap-planning cycle

This planning cycle is complete when:

- the retrospective is accepted as accurate evidence;
- one milestone family is selected with explicit rationale;
- the next roadmap snapshot passes audit;
- the owner accepts its exact SHA-256;
- the first task packet is ready for separate review;
- no implementation has started early.
