# Milestone 14 — First Real Internal Project Governed Run

Status: draft for owner review
Date: 2026-06-22
Milestone: 14

## Purpose

Milestone 14 proves Kaizen v1 by running one real, non-self internal project through the governed project loop.

The selected workload proves Kaizen. It does not become Kaizen.

## Authority basis

```text
ROADMAP_V0.4.md
03-research-results/338-packet-019d-synthetic-proof-and-m14-readiness-contract.md
03-research-results/342-milestone-13-owner-closure-acceptance.md
```

## Current entry status

```text
Milestone 13 closed: yes
M13 live DB proof complete: yes
M14 readiness contract recorded: yes
real internal project selected: not yet
project-specific boundaries documented: not yet
backup / restore plan ready: not yet
fresh-context resumption test plan ready: not yet
```

## Objective

M14 must demonstrate this chain on one selected real internal project:

```text
project selection
-> boundary definition
-> project onboarding into Kaizen
-> research / source intake
-> spec
-> audit
-> task packet
-> implementation or implementation-style work
-> implementation return
-> operational records from real work
-> fresh-context resumption proof
-> backup and restore proof
-> Kaizen v1 closure audit
-> owner v1 acceptance
```

## Required first packet

The first M14 packet is:

```text
020A — Real Internal Project Selection and Boundary Definition
```

020A must select exactly one workload and define exact boundaries before any implementation-bearing M14 work begins.

## Candidate workloads

Roadmap candidates:

```text
Neon Ronin planning/build lane;
SearchClarity as a Neon Ronin business/service lane;
another internal project selected by owner.
```

Recommended default:

```text
Neon Ronin narrow planning/build slice.
```

Reason:

```text
Neon Ronin is a real downstream project, clearly separate from Kaizen, and strategically central. A narrow slice can prove Kaizen without provider purchases, customer data, Qdrant, Hermes, Tauri, Observatory, production deployment, or full Neon Ronin implementation.
```

This recommendation is not selection. Owner selection remains required.

## Scope boundaries

M14 does not authorize by default:

```text
full Neon Ronin implementation;
full SearchClarity implementation;
provider purchase;
customer-data reuse;
logged-in scraping;
Qdrant;
Hermes write authority;
Tauri shell;
Observatory / IMI implementation;
commercial operations;
production deployment;
multi-user identity;
arbitrary SQL;
generic shell;
Git push.
```

## Approved operational-record surface

M14 may use the M13-approved operational service surface for real workload evidence:

```text
execution attempt;
verification run;
return manifest;
artifact provenance;
service audit event / idempotency support records.
```

M14 must not rely on:

```text
governed-operation read model;
connector invocation telemetry;
Observatory / IMI records;
provider/customer-data records;
Qdrant-derived truth;
Hermes autonomous action records.
```

## Exit criteria

M14 can close only when:

```text
one real internal project is selected and onboarded;
project-specific boundaries are accepted;
research -> spec -> audit -> task packet -> implementation/return loop is completed;
operational records from real work are generated or an explicit no-write reason is accepted;
fresh-context resumption is demonstrated;
backup and restore proof for selected project data is completed;
Kaizen v1 completion audit is completed;
owner accepts Kaizen v1.
```

## Recommended packet sequence

```text
020A — Real Internal Project Selection and Boundary Definition
020B — Selected Project Bootstrap / Onboarding into Kaizen
020C — Research and Source Intake for Selected Project Slice
020D — Spec / Audit / Task Packet for Selected Project Slice
020E — Implementation / Implementation-Return Proof for Selected Project Slice
020F — Fresh-Context Resumption Proof
020G — Backup / Restore Proof for Selected Project Data
020H — Kaizen v1 Completion Audit and Owner Acceptance
```

## Stop conditions

M14 must stop if:

```text
owner selection is ambiguous;
workload scope expands beyond accepted 020A boundaries;
provider/customer-data work is requested without future gate;
work drifts into Qdrant/Hermes/Tauri/Observatory/full Neon Ronin/full SearchClarity implementation;
operational service returns recovery_required;
repository state is dirty before planned mutation;
secrets or DSNs appear in logs intended for project records;
backup/restore plan is not ready before real project data is generated.
```

## Non-authorization

This M14 definition draft does not authorize:

```text
M14 implementation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
production database mutation;
provider purchase;
customer-data reuse;
logged-in scraping;
Qdrant;
Hermes write authority;
Tauri implementation;
Observatory / IMI implementation;
commercial operations;
production deployment;
full Neon Ronin implementation;
full SearchClarity implementation;
Git push.
```
