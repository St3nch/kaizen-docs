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
Platform HEAD: 7cbc132a508071c94e68fcd5bd206b19ac8bd61a
Vault HEAD: 2487de669bc44ed50e54fd5dbbfdd128ce659dbb
Canonical current-state SHA-256: e50fc7d1d88257ecf6a818a47673775a9f8960e999a6a843872306227dd2b8c7
Go8 HEAD: 5830962ba34e62bbfb65508307ee0c706ed31e14
Northstar HEAD: bf9abb513d6f22fc5ed800301a8b3ba0b75c5189
Next gate: accept specification SHA-256 bfbfed7301361edc712b37dbd59ebc3575c1904f34490d3ca28e5734d548bfae and approve Packet 014E SHA-256 a5b89ddef8c33ed94a5af7c95efacea8ee8283dab56ddd2a6e70bc35e1fc114b
Milestone 9: active; Phases 1-3 complete, Phase 4 partially complete
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

- no further Milestone 9 project-bootstrap implementation before accepted doctrine, an audited replacement packet, and separate owner approval;
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
Packet 013E Generation 3 backup and restore proof: complete
-> Packet 014A Phase 1: complete
-> Packet 014A Phase 2: complete
-> Packet 014A Phase 3 and Packet 014B implementation: complete
-> Packet 014A Phase 4: active and partially complete
-> Injection 7 dirty-repository proof: complete
-> Packet 014C review-status normalization correction: complete
-> Packet 014D: retired unimplemented after independent audit
-> Decision 0017 atomic three-note project-bootstrap doctrine: accepted through Result 213
-> Packet 014A Phase 4 reconciliation, Northstar command-center candidate, atomic bootstrap specification, and Packet 014E audit: complete
-> obtain combined specification acceptance and Packet 014E implementation approval
-> implement and hammer-test the governed bootstrap-project operation
-> execute one approved three-note bootstrap, commit the vault locally, and run fresh-context R1
-> stop before Phase 5 amendment work without separate authority
-> continue bounded Observatory evidence research under OBR-01 through OBR-15
```

The controlled mock pilot has entered execution under Packet 014A. Phases 1 through 3 are complete and Phase 4 is partially complete. Independent audit retired Packet 014D and exposed the missing governed project-bootstrap abstraction. Phase 4 now resumes at doctrine definition and owner acceptance before any replacement implementation packet. Phase 5 amendment work and later closure work remain unauthorized.

Observatory research is a parallel evidence track. It preserves the full architecture and resolves rights, access, retention, measurement, and reuse questions before any physical database or collection program is proposed.
