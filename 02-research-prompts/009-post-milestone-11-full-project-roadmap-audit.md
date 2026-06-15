# Claude Audit Prompt - Post-Milestone-11 Full-Project Roadmap Audit

## Timing

Run only after Milestone 11 is fully implemented, independently audited, accepted, and canonically returned.

## Role

You are conducting an independent, read-only red-team and forward-roadmap audit of Kaizen.

Do not treat existing plans as correct merely because they are accepted. Distinguish durable evidence, doctrine, owner decisions, proposed direction, implementation reality, and stale narrative.

## Continuity from the last major audit

Begin by reading:

```text
03-research-results/097-post-milestone-6-forward-roadmap-audit-reconciliation.md
```

That record preserves the stewarded conclusions of the June 10 Claude audit. Its corrected dependency sequence was:

```text
Milestone 7 durable recoverability
-> Milestone 8 reliability and defect closure
-> Milestone 9 controlled implementation-return pilot
-> Milestone 10 workload and system-of-record reconciliation
-> Milestone 11 operational data foundation
-> typed operational APIs
-> production MCP architecture
```

Audit whether Milestones 8 through 11 actually satisfied the purpose of that sequence, including where Kaizen deliberately changed course.

## Required project context

Read the current repositories and key records completely, including:

```text
C:\dev\kaizen-docs
C:\dev\kaizen\platform
C:\dev\kaizen\vault
C:\dev\kaizen\staging
C:\dev\kaizen-mcp
C:\dev\chatgpt-mcp
C:\dev\kaizen-pilot-northstar
```

At minimum read:

```text
00-entrypoint/LLM_START_HERE.md
README.md
ROADMAP_V0.3.md
01-project-standard/kaizen-project-standard-v0.2.md
01-project-standard/kaizen-vision-and-architecture-alignment.md
04-design-decisions/0001 through the latest accepted decision
05-specs/operational-postgres-authority.md
05-specs/milestone-11-operational-data-foundation-planning.md
Results 097, 162, 241-250, and all Milestone 11 completion records
canonical Northstar command-center.md, overview.md, and current-state.md
live repository status, commits, tests, migration evidence, backup/restore proof, and hammer results
```

Search the repositories for contradictory, stale, duplicate, or superseded claims rather than relying only on the entrypoint.

## Audit questions

### 1. Recommendation follow-through

For every major recommendation preserved in Result 097, classify:

```text
satisfied
partially satisfied
superseded with evidence
not satisfied
no longer applicable
```

Cite exact files, commits, tests, and gaps.

### 2. Milestones 8-11 audit

Audit each milestone for:

- stated objective versus actual outcome;
- implementation and test evidence;
- owner authority;
- failure preservation;
- recoverability;
- scope drift;
- unnecessary bureaucracy;
- hidden technical debt;
- stale documentation;
- false completion risk;
- operational usefulness.

### 3. Operational data foundation

Determine whether Milestone 11:

- implemented only evidence-backed record families;
- preserved Markdown, file, governance JSONL, staging, and Git authorities;
- avoided dual authority;
- enforced project isolation and legal state transitions;
- proved migrations, backup, restore, recovery, and concurrency;
- used typed services rather than direct agent SQL;
- remained small enough to understand and operate;
- created foundations genuinely useful to later Kaizen work.

### 4. Full-project vision alignment

Evaluate the current implementation against Kaizen's broader planned purpose:

- governed evidence and provenance engine;
- idea -> research -> sources -> claims -> decisions -> specs -> audits -> packets -> implementation return;
- model-agnostic agent operation;
- marketplace and service workflows;
- future Context and Tool Gateway;
- raw evidence -> normalized evidence -> LLM context views;
- future Observatory and Internet Marketing Intelligence capabilities;
- human-in-loop controls;
- typed local services;
- Postgres, Qdrant, DataForSEO, Cloudflare, mcp2py or equivalent, and progressive UI only when earned by evidence.

Identify where current architecture enables or blocks this direction.

### 5. What is proved versus conceptual

Create a strict matrix:

```text
proved in code and tests
proved operationally
proved only by mock project
planned but unproved
research-backed but undecided
speculative or premature
```

### 6. Next milestone recommendation

Recommend the next milestone sequence after 11. Consider at least:

- typed operational APIs;
- production MCP architecture and packaging;
- another real-project pilot;
- controlled research/report pilot;
- connector reliability telemetry;
- governed-operation read models;
- Qdrant retrieval;
- human interface work;
- Hermes integration;
- Observatory/IMI research and implementation;
- multi-user identity.

Do not assume the old tentative sequence remains correct. Base the recommendation on current evidence and dependencies.

Explicitly answer:

1. Should a real-project pilot occur before typed operational APIs or production MCP work?
2. Should a controlled research/report pilot occur before Observatory, ingestion, claims, or Qdrant work?
3. Which current proving-ground components should be retained, replaced, or productionized?
4. What is the smallest next milestone with the highest risk-reduction value?
5. Which attractive projects should remain deferred?

## Required output

Produce:

1. executive verdict;
2. Result-097 follow-through matrix;
3. Milestones 8-11 scorecard;
4. architecture and authority audit;
5. implementation/test/recovery findings;
6. full-project vision alignment analysis;
7. contradictions and stale-record register;
8. risks ranked by severity and reversibility;
9. exact recommended next milestone sequence;
10. smallest immediate next packet;
11. explicit do-not-build-yet list;
12. source and limitation register.

## Restrictions

- read-only audit;
- no repository writes;
- no owner-private oracle access unless explicitly supplied;
- no direct promotion of recommendations into doctrine;
- no generic advice detached from Kaizen's actual repositories;
- no recommendation for broad rewrites without concrete evidence;
- disclose inaccessible repositories, missing tools, and unverified claims.
