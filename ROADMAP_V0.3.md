# Kaizen Roadmap v0.3

Status: accepted and active
Date: 2026-06-11
Owner: Kaizen project steward

## Purpose

This is the concise forward roadmap after Milestone 6.

Historical planning remains available in Git and in the temporary `ROADMAP.md.bak` recovery reference. Detailed rationale belongs in linked evidence and specifications, not in this roadmap.

This roadmap does not authorize implementation.

## Current checkpoint

```text
Kaizen Project Standard v0.2: authoritative
Milestones 1-7: closed
Packet 011E: complete
Platform HEAD: 1b8be1d6d42d768587dddb2be8415fa24b670561
Vault HEAD: 37e756a8e85157bd83c22e832af285e1992f8d8e
Milestone 8: accepted for planning only
```

Milestone 7 proved encrypted off-machine recovery, two retained generations, independent Google Drive and USB restoration, fresh platform reconstruction, bounded failure proof, repeatability, and governed canonical return.

The next evidence gap is reliability and known-defect closure before the controlled implementation-return pilot.

Full reconciliation:

```text
03-research-results/097-post-milestone-6-forward-roadmap-audit-reconciliation.md
```

## Forward sequence

### Milestone 7 - Durable recoverability

Create privacy-aware off-machine backups of the vault, platform, and immutable staging evidence, then prove restoration with exact hash and Git verification.

Detailed milestone definition:

```text
05-specs/milestone-7-durable-recoverability.md
```

### Milestone 8 - Reliability and known-defect closure

Correct or retire the evidence-bounded reliability defects in prepared-candidate loading, amendment concurrency, MCP lifecycle, health/version/tool-registry reporting, connector behavior, runbooks, tests, and lint policy without declaring the temporary proving grounds production or widening authority.

Detailed milestone definition:

```text
05-specs/milestone-8-reliability-and-known-defect-closure.md
```

### Milestone 9 - Controlled implementation-return pilot

Run Kaizen's complete governed loop on a purpose-built mock project with a sealed owner oracle, oracle-blind implementation lane, independent audit, eight injected failures, two fresh-context resumption checks, a rule-modifying change request, and a workload ledger as the primary new evidence.

Milestone 9 runs only after Milestone 8 proves the read-only loader and dirty-repository behavior; the pilot then field-validates those controls rather than debugging them.

Detailed pilot specification:

```text
05-specs/controlled-implementation-return-pilot.md
```

### Milestone 10 - Workload and system-of-record reconciliation

Use the pilot evidence to identify real record lifecycles, transaction boundaries, read/write patterns, retention, privacy, volume, Markdown friction, and justified operational-data candidates.

No physical database schema is authorized in this milestone.

Detailed rationale:

```text
03-research-results/097-post-milestone-6-forward-roadmap-audit-reconciliation.md
```

### Milestone 11 - Operational data foundation

Only after workload reconciliation, design and prove the smallest justified Postgres schemas, migrations, constraints, roles, backup, restore, and typed operational APIs.

Scope is limited to record families actually evidenced by Milestone 9 and accepted in Milestone 10. Ingestion or Observatory schema work requires additional front-half evidence from a later controlled research/report pilot or real project if Northstar does not produce it.

Detailed milestone definition: deferred until Milestone 10 evidence exists.

### Later milestone families

```text
typed operational APIs
-> production MCP architecture and packaging
-> Qdrant retrieval when justified
-> progressive human interface
-> Hermes integration
-> Internet Marketing Intelligence systems
-> durable multi-user identity when required
```

No sequence below Milestone 11 is accepted yet.

## Controlled pilot rule

The mock project is an evidence instrument, not a toy demo.

It must prove that Kaizen can:

- resolve ambiguity before implementation;
- preserve conflicting evidence;
- reject an intentionally faulty implementation return;
- require exact completion evidence;
- preserve baseline history through a controlled amendment;
- resume from canonical records without chat history;
- reveal real operational workloads without designing tables early.

Expected behavior, injected failures, golden outputs, and scoring are defined at:

```text
05-specs/controlled-implementation-return-pilot.md
```

## Standing boundaries

Until each milestone receives its own accepted definition and task packet:

- no Milestone 8 implementation beyond separately approved packets;
- no production MCP migration or packaging;
- no physical Postgres schema;
- no mock-project execution before Milestone 8 closes;
- no Qdrant, Hermes, broad UI, or Internet Marketing Intelligence implementation;
- no vault push or remote creation;
- no generalized correction, delete, move, rollback, or multi-note mutation semantics;
- no inferred owner approval.

## Required gate for every milestone

```text
evidence
-> milestone definition
-> security/steward audit
-> exact owner acceptance
-> task packet
-> separate implementation approval
-> implementation and tests
-> governed return
-> closure audit
-> owner closure acceptance
```

## Immediate next planning work

```text
draft and audit Packet 012A defect register and contract freeze
-> obtain separate implementation approval for read-only inspection only
-> classify every initial defect candidate
-> freeze the accepted reliability contracts and later packet split
-> stop before Packet 012B implementation
```

The controlled mock pilot remains planned but unexecuted. It stays behind Milestone 8 closure so its controls are field-validated rather than debugged during the pilot.
