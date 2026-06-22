# Packet 020A — M14 Workload Selection and Boundary Registration

Status: accepted selection / boundary registration
Date: 2026-06-22
Packet: 020A
Milestone: 14

## 1. Purpose

Record the selected Milestone 14 proof workload and the boundary design for Kaizen's v1 real-project proof.

M14 tests Kaizen as a project-intelligence system, not as a coding agent.

## 2. Starting checkpoint

Docs repository at 020A start:

```text
root: docs
branch: main
HEAD: d44ac06fca76dd0f8231bd4005362cc13a6ec77b
working tree: clean
upstream: origin/main
ahead/behind: 16 / 0
```

Platform repository at 020A start:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

## 3. Authority basis

```text
ROADMAP_V0.4.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
03-research-results/342-milestone-13-owner-closure-acceptance.md
03-research-results/344-packet-020a-real-internal-project-selection-task-packet.md
```

## 4. External design input

Claude produced an M14 experiment design report recommending a two-stage proof:

```text
Stage A — idea-to-implementation-docs proof;
Stage B — one tiny bounded real implementation-return cycle performed by a connected coding agent, not by Kaizen.
```

The report also recommended adding Phase 0 pre-registration so the proof is scored against boundaries written before generation rather than judged after the fact.

## 5. Owner clarification

Owner clarified the correct Kaizen role:

```text
Kaizen will not build anything.
Kaizen turns ideas into implementation docs.
The docs are handed to a connected LLM / coding agent to follow and code from.
Neon Ronin and SearchClarity were started before Kaizen and already have repo reality.
```

Owner also clarified the Observatory boundary:

```text
The intended Observatory / IMI capability is the same Kaizen-planned Observatory concept.
It should be usable across multiple SERP / SEO / LLM citation / marketplace intelligence projects.
The recurring Observatory mentions across Kaizen, Neon Ronin, and SearchClarity are a boundary/ownership design knot, not proof that three separate incompatible systems should exist.
```

## 6. Selected M14 workload

Selected workload:

```text
Neon Ronin parent project idea-to-implementation-docs proof,
with SearchClarity child-project dependency and repo-reconciliation test,
and Observatory / IMI shared-capability boundary clarification.
```

This selection replaces the narrower earlier wording of only:

```text
Neon Ronin narrow planning/build slice.
```

## 7. Project identity boundaries

M14 must preserve these identities:

```text
Kaizen = governed project-intelligence system.
Neon Ronin = downstream multi-agent business operating system for managing small businesses.
SearchClarity = child/dependent business/service lane intended to run under Neon Ronin.
Observatory / IMI = Kaizen-planned shared intelligence capability, potentially usable by Neon Ronin, SearchClarity, and future SEO/SERP/LLM-citation projects.
```

Hard rule:

```text
The selected workload proves Kaizen. It does not become Kaizen.
```

## 8. Stage A authorization

020A authorizes Stage A planning and documentation only:

```text
idea-only Neon Ronin intake design;
idea-to-docs generation design;
read-only Neon Ronin repo reconciliation design;
idea-only SearchClarity child-project intake design;
read-only SearchClarity repo reconciliation design;
parent/child dependency map design;
Observatory / IMI ownership and consumer-boundary clarification;
implementation-ready-doc quality rubric;
fresh-context resumption plan design.
```

Stage A output should be candidate project intelligence until reconciled.

## 9. Stage B boundary

Stage B is required later to satisfy the accepted v1 implementation-return requirement, but 020A does not authorize executing Stage B.

Stage B must be separately packeted and owner-approved.

Stage B must be:

```text
one tiny bounded real implementation-return cycle;
performed by a connected coding LLM / agent, not by Kaizen itself;
limited to an authorized, reversible area of the selected downstream repo;
returned into Kaizen as governed evidence;
covered by operational records, return manifest, fresh-context resumption, and backup/restore proof.
```

## 10. Repo-reconciliation inputs

M14 may use read-only repo reconciliation against:

```text
C:\dev\neon-ronin
C:\dev\searchclarity
```

Read only enough to determine:

```text
project identity;
current state;
existing docs and doctrine;
source tree shape;
recent commits;
available tests;
implementation-ready surfaces;
reserved-but-not-built surfaces;
parent/child dependency evidence;
Observatory references and ownership ambiguity.
```

Do not read or invent private/git-ignored customer/provider/sample data.

## 11. Comparison method

M14 will compare generated Kaizen docs to repo reality using these classifications:

```text
CONFIRMED;
CONTRADICTED;
STALE;
INVENTED;
RESERVED-NOT-BUILT;
PRIVATE/LOCAL.
```

The proof must score whether Kaizen:

```text
keeps project identities separate;
models parent/child dependency correctly;
identifies Observatory / IMI as a shared capability boundary;
avoids hallucinating repo facts;
avoids treating reserved folders as built systems;
produces implementation-ready docs a coding LLM can follow.
```

## 12. Required Stage A outputs

Stage A should produce, as needed and without bloat:

```text
idea intake summary;
project overview;
current state;
scope boundary register;
parent/child dependency map;
Observatory / IMI boundary decision candidate;
repo reconciliation reports;
implementation-ready-doc quality audit;
initial task packet / coding-agent handoff candidate;
fresh-context resumption pack draft.
```

SearchClarity should remain lean if repo reality is docs-only.

## 13. Stop conditions

Stop and escalate if:

```text
Kaizen starts coding Neon Ronin or SearchClarity;
Kaizen confuses Neon Ronin, SearchClarity, Kaizen, or Observatory identities;
SearchClarity is modeled as Kaizen or as a peer incorrectly;
Observatory / IMI is treated as implemented;
repo facts are invented;
git-ignored/private data is requested or fabricated;
reserved folders are treated as built features;
provider/customer-data work appears;
Qdrant/Hermes/Tauri/Observatory implementation appears;
Stage B execution is attempted without a separate packet and owner approval.
```

## 14. Completion verdict

Packet 020A verdict:

```text
PASS — M14 WORKLOAD SELECTED AND BOUNDARY REGISTERED
```

M14 may proceed to Stage A idea-to-implementation-docs proof design and read-only repo reconnaissance.

M14 implementation-bearing Stage B remains unauthorized until separately packeted and approved.

## 15. Non-authorization

This record does not authorize:

```text
coding Neon Ronin;
coding SearchClarity;
Stage B execution;
platform mutation;
vault mutation;
staging mutation;
database mutation;
production database mutation;
provider purchase;
customer-data reuse;
logged-in scraping;
Qdrant implementation;
Hermes write authority;
Tauri implementation;
Observatory / IMI implementation;
commercial operations;
production deployment;
full Neon Ronin implementation;
full SearchClarity implementation;
Git push.
```
