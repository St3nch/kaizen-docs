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
Roadmap v0.3: accepted and active
Milestones 1-8: closed
Packets 013A through 013D: complete
Canonical current-state alignment: complete
Platform HEAD: ba3b5feca90e4fb5cb02e34981dc7ed86942962f
Vault HEAD: 2487de669bc44ed50e54fd5dbbfdd128ce659dbb
Canonical current-state SHA-256: e50fc7d1d88257ecf6a818a47673775a9f8960e999a6a843872306227dd2b8c7
Go8 HEAD: 312704a8b8505bdb64f28cc557171c10de8bd5bc
Next gate: Packet 013E Generation 3
Milestone 9: planned but not authorized for implementation
```

Milestone 8 closed the accepted reliability and known-defect register, proved the integrated platform, Kaizen MCP, Go8, lifecycle, tunnel, connector, and governed-return boundaries, and recorded the owner closure acceptance in Research Result 162.

Before Milestone 9 implementation, the next planning work is corrective-roadmap reconciliation, documentation and authority repair, readiness verification, operation-status semantics correction, operator fallback updates, and post-Milestone-8 backup Generation 3 planning.

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

## Parallel Observatory research track

The planned Observatory is Kaizen's future governed search, marketplace, GEO, competitor, and outcome-intelligence capability. Its complete architectural envelope is preserved now; implementation order will be decided later from evidence.

Current evidence is recorded in:

```text
03-research-results/163-observatory-existing-evidence-reconciliation.md
03-research-results/164-observatory-current-market-reconnaissance-and-gap-register.md
```

The research track must complete or proportionately disposition these workstreams before Observatory implementation planning:

```text
OBR-01 DataForSEO written rights clarification
OBR-02 Fiverr operational observability
OBR-03 Upwork operational observability
OBR-04 Etsy authorized evidence routes
OBR-05 Amazon and Shopify rights verification
OBR-06 first-party web retention and export proof
OBR-07 AI and GEO observability methods
OBR-08 local, directory, review, and knowledge-source landscape
OBR-09 alternative provider rights and continuity
OBR-10 client data reuse and deletion doctrine research
OBR-11 measurement doctrine
OBR-12 perishable-data decision memo
OBR-13 rights-aware data and learning taxonomy
OBR-14 Observatory hammer research map
OBR-15 workload and business decision map
```

This research may proceed in parallel with corrective planning and Milestone 9 readiness work because it is evidence gathering, not Observatory implementation.

The track does not authorize provider purchase, paid sampling, raw capture, crawling, logged-in collection, client-data reuse, physical database schema, Postgres, Qdrant, LangGraph, MCP tools, or hammer execution.

SearchClarity and Neon Ronin are workload evidence for future Kaizen-governed projects. They do not define or limit the Observatory's complete architecture.

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

Until each milestone or research track receives its own accepted definition and task packet:

- no Milestone 9 implementation;
- no corrective implementation from the independent Milestones 6-8 audit until exact packets are approved;
- no production MCP migration or packaging;
- no physical Postgres or Observatory schema;
- no Observatory data collection, provider purchase, crawling, logged-in marketplace collection, or client-data reuse;
- no Qdrant, LangGraph, Hermes, broad UI, or Internet Marketing Intelligence implementation;
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
complete Packet 013E Generation 3 backup and restore proof before the first canonical Northstar mutation
-> draft and audit the exact Milestone 9 implementation packet
-> obtain separate owner approval for artifact inventory, information lanes, roots, hashes, and injection controls
-> only then create Northstar artifacts and the disposable repository
-> continue bounded Observatory evidence research under OBR-01 through OBR-15
-> stop before Milestone 9 execution or Observatory implementation without separate authority
```

The controlled mock pilot remains planned but unexecuted. Milestone 8, Packets 013A through 013D, and canonical current-state alignment are complete. Packet 013E Generation 3 is the remaining precondition before an executable Milestone 9 packet may be approved.

Observatory research is a parallel evidence track. It preserves the full architecture and resolves rights, access, retention, measurement, and reuse questions before any physical database or collection program is proposed.
