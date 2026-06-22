# Packet 020A — Real Internal Project Selection

Status: task packet draft for owner review
Date: 2026-06-22
Packet: 020A
Milestone: 14

## Purpose

Select exactly one real internal project workload for Milestone 14 and define the boundary that Kaizen will prove against.

## Authority basis

```text
ROADMAP_V0.4.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
03-research-results/342-milestone-13-owner-closure-acceptance.md
```

## Candidate workloads

```text
A. Neon Ronin narrow planning/build slice
B. SearchClarity as a Neon Ronin business/service lane
C. Another internal project selected by owner
```

## Steward recommendation

Recommended selection:

```text
A. Neon Ronin narrow planning/build slice
```

Recommended slice:

```text
Neon Ronin first governed workspace / agent-OS planning slice
```

Reason:

```text
It is real downstream work, strategically central, clearly separate from Kaizen, and can prove the Kaizen loop without provider/customer-data/Qdrant/Hermes/Tauri/Observatory or production scope.
```

## 020A deliverables

```text
selected workload;
explicit project boundary;
out-of-scope list;
initial source/research needs;
project data classification;
backup/restore posture;
fresh-context resumption plan;
first follow-on packet recommendation.
```

## Acceptance criteria

```text
owner selection is recorded;
selected workload is clearly not Kaizen;
selected scope is small enough for M14;
blocked technologies and data categories are explicit;
next packet can onboard the project without guessing.
```

## Non-authorization

This packet draft does not authorize:

```text
M14 implementation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
provider/customer-data work;
Qdrant;
Hermes write authority;
Tauri;
Observatory / IMI implementation;
full Neon Ronin implementation;
full SearchClarity implementation;
Git push.
```

## Owner approval wording

```text
Approve Packet 020A selection of Neon Ronin narrow planning/build slice.
```

or:

```text
Approve Packet 020A selection of SearchClarity as a Neon Ronin business/service lane.
```

or:

```text
Approve Packet 020A selection of another internal project: <name and short boundary>.
```
