# Kaizen Roadmap v0.3

Status: accepted and active; refreshed by Packet 018A reading-path cleanup
Date: 2026-06-21
Owner: Kaizen project steward

## Purpose

This is the active roadmap surface after Milestone 12 and the post-Milestone-12 audit program Passes 1 through 3.

This roadmap summarizes current posture, closed milestone traceability, deferred boundaries, and next valid gates. Detailed rationale remains in the linked results, specs, and decisions.

This roadmap does not authorize implementation.

## Current checkpoint

```text
Kaizen Project Standard v0.2: authoritative
Active current-state pointer: 00-entrypoint/LLM_START_HERE.md
Milestones 1-12: closed
Latest milestone closure: Milestone 12, accepted in Result 307
Post-M12 audit program: Passes 1-3 complete and dispositioned in Results 313, 314, and 316
Packet 018A approval starting docs commit: 376908dab2e52c899978c48d618cbafda0d20480
Platform HEAD at Packet 018A approval: b7593c5ee90fd32c1e2a86572cc570d307de2be6
Vault HEAD at Packet 018A approval: c898f261c0b341eb8419125247c8bd53ef567d6c
Live operational database: kaizen_ops migrated through 0005_recovery_retention_integrity
Next gate: finish/accept Packet 018A documentation cleanup, then make an explicit post-M12 roadmap decision
No Milestone 13 implementation is authorized
```

## Closed milestone traceability

| Milestone | Name | Definition / spec | Owner acceptance | Closure result / audit | Owner closure acceptance | Residuals / notes |
|---|---|---|---|---|---|---|
| 1 | Tools foundation | Early first-slice roadmap and packet chain | Packet results 018–025 | Closed in first-slice chain | Captured in historical roadmap/results | Historical first-slice evidence |
| 2 | Canonical vault bootstrap | Early first-slice roadmap and packet chain | Packet results 018–025 | Closed in first-slice chain | Captured in historical roadmap/results | Historical first-slice evidence |
| 3 | Safe staging and promotion | Early first-slice roadmap and packet chain | Packet results 018–025 | Closed in first-slice chain | Captured in historical roadmap/results | Historical first-slice evidence |
| 4 | One governed project loop | `IMPLEMENTATION_ROADMAP.md` | Packet 009A/009B approvals | `03-research-results/062-milestone-4-implementation-return-closure-audit.md` | Result 062 closure audit | Historical first-slice completion |
| 5 | Slice retrospective and v0.2 consolidation | `01-project-standard/kaizen-project-standard-v0.2.md` | Result 067 | Result 067 | Result 067 | Project Standard v0.2 authoritative |
| 6 | Governed operator surface | `05-specs/milestone-6-governed-operator-surface.md` | Result 069 and Packet 010 approvals | Results 094/095 | Results 094/095 | Historical operator-surface foundation |
| 7 | Durable recoverability | `05-specs/milestone-7-durable-recoverability.md` | `03-research-results/104-milestone-7-owner-acceptance.md` | `03-research-results/131-milestone-7-closure-audit.md` | `03-research-results/132-milestone-7-owner-closure-acceptance.md` | Some survivability and cleanup work carried forward |
| 8 | Reliability and known-defect closure | `05-specs/milestone-8-reliability-and-known-defect-closure.md` | `03-research-results/134-milestone-8-owner-acceptance.md` | `03-research-results/161-milestone-8-closure-audit.md` | `03-research-results/162-milestone-8-owner-closure-acceptance.md` | KZA audit-tail register tracked by P3-004 |
| 9 | Controlled implementation-return pilot | `05-specs/controlled-implementation-return-pilot.md` | Results 172/174 and Packet 013/014 approvals | `03-research-results/243-milestone-9-closure-audit.md` | `03-research-results/244-milestone-9-owner-closure-acceptance.md` | Spec filename is non-numbered by design; sealed-oracle waiver disclosed |
| 10 | Workload and system-of-record reconciliation | `05-specs/milestone-10-workload-and-system-of-record-reconciliation.md` | `03-research-results/246-milestone-10-spec-and-packet-015a-owner-acceptance.md` | `03-research-results/249-milestone-10-reconciliation-audit.md` | `03-research-results/250-packet-015a-and-milestone-10-owner-closure-acceptance.md` | Decision 0019 accepted |
| 11 | Operational data foundation | `05-specs/milestone-11-operational-data-foundation-planning.md` plus supporting M11 specs | `03-research-results/254-milestone-11-spec-and-packet-016a-owner-acceptance.md` | `03-research-results/288-packet-016f-owner-completion-and-milestone-11-closure.md` | Result 288 | Post-closure 016G complete under Result 293; M11 raw audit Passes 4-9 tracked by P3-001 |
| 12 | Recovery realism and operational restore proof | `05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md` | Results 296 and 298 | `03-research-results/306-milestone-12-closure-assessment.md` | `03-research-results/307-milestone-12-owner-acceptance.md` | Restore-forward, operational role-ready restore, ruff/tooling, and historical zero-file generations remain deferred |

Result 312 and Result 314 accept that Milestones 7 through 12 are clean and followable end-to-end. Do not broadly re-audit or reopen closed milestones without concrete contradictory evidence.

## Active deferred-boundary table

| Boundary | Origin | Current disposition | Next handling |
|---|---|---|---|
| Restore-forward | Results 293, 306, 307 | Deferred and accepted as residual | Future owner-approved recovery/restore-hardening gate |
| Operational role-ready restore | Results 293, 306, 307 | Deferred and accepted as residual | Future owner-approved recovery/restore-hardening gate |
| Ruff/tooling | Results 293, 306, 307 | Deferred and accepted as residual | Future tooling/lint adoption gate |
| Historical zero-file retained generations | Results 293, 306, 307 | Preserved as historical evidence | Do not delete without successor restore proof and owner approval |
| M11 audit Passes 4-9 / synthesis gap | Result 316, P3-001 | Must-track, non-blocking | Create preservation/gap record or owner-defer |
| M6-8 KZA per-ID register | Result 316, P3-004 | Must-track, non-blocking | Create historical disposition register |
| Decision 0015 reserved/gap record | Result 316, P3-005 | Claim fix required; doctrine not authored | Do not fabricate doctrine; owner decision required |
| Result 302 expansion sweep | Result 316, P3-007 | Must-track, non-blocking | Later historical-disposition sweep |
| Watchlist / Observatory / OBR backlog | Result 316, P3-008 | Deferred-tracked | Future research/planning gate only |
| Milestone-closeout reading-path refresh rule | Result 316, P3-009 | High-value process repair | Add to closeout discipline when accepted |

## Candidate next lanes

The following are candidates only. None is authorized as Milestone 13.

```text
A. recovery/restore-hardening lane for restore-forward and operational role-ready restore;
B. documentation/process-hardening lane for audit-signal registers and closeout refresh discipline;
C. typed operational API / production MCP planning lane after current documentation posture is stable;
D. Observatory / Internet Marketing Intelligence research lane after rights, retention, measurement, and provider questions are proportionately dispositioned.
```

A candidate becomes active only through the standard gate sequence below.

## Parallel Observatory research track

The planned Observatory is Kaizen's future governed search, marketplace, GEO, competitor, and outcome-intelligence capability. Its complete architectural envelope is preserved; implementation order will be decided later from evidence.

Current evidence is recorded in:

```text
03-research-results/163-observatory-existing-evidence-reconciliation.md
03-research-results/164-observatory-current-market-reconnaissance-and-gap-register.md
```

The research track must complete or proportionately disposition OBR-01 through OBR-15 before Observatory implementation planning. This roadmap does not authorize provider purchase, paid sampling, raw capture, crawling, logged-in collection, client-data reuse, physical database schema, Postgres, Qdrant, LangGraph, MCP tools, or hammer execution.

## Standing boundaries

Until each milestone or research track receives its own accepted definition and task packet:

- no Milestone 13 implementation;
- no production MCP migration or packaging;
- no physical Postgres or Observatory schema beyond accepted operational foundation boundaries;
- no Observatory data collection, provider purchase, crawling, logged-in marketplace collection, or client-data reuse;
- no Qdrant, LangGraph, Hermes, broad UI, or Internet Marketing Intelligence implementation;
- no vault push or remote creation;
- no generalized correction, delete, move, rollback, or multi-note mutation semantics;
- no retention deletion or physical evidence deletion;
- no Decision 0015 doctrine creation without owner approval;
- no fabrication of missing M11 audit text;
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
-> active reading-path refresh check
```

## Immediate next planning work

```text
Packet 018A reading-path cleanup: approved and in execution
-> record implementation return
-> owner review / acceptance of cleanup if appropriate
-> choose next post-M12 roadmap lane only through explicit owner decision
```

The active reading path must be checked after every future milestone closure: entrypoint, README status pointer, active roadmap, decisions index if affected, and deferred-boundary table.
