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

### 6. Error, gap, and improvement hunt

Actively search for errors, missing proof, weak assumptions, security gaps, privacy and retention problems, recovery weaknesses, test blind spots, stale doctrine, dual-authority risks, unnecessary bureaucracy, operational friction, and opportunities to simplify without weakening evidence or human control.

For every finding, provide a finding ID, severity, confidence, exact evidence, why it matters, classification, smallest safe correction, and whether it blocks the next milestone.

Audit at minimum:

- malformed IDs, stale hashes, broken links, invalid metadata, contradictory authority claims, and repository/document drift;
- race conditions, partial-failure windows, idempotency and recovery gaps, migration hazards, backup/restore mismatches, and illegal state transitions;
- tests that prove less than their names imply, overmocking, missing negative tests, weak concurrency proof, and untested upgrades;
- privilege creep, unsafe defaults, path traversal, project leakage, secret exposure, telemetry overcollection, denial-of-service risks, and weak trust boundaries;
- privacy, deletion, provenance, licensing, data-rights, and customer-data gaps;
- accidental dual authority among Markdown, Git, Postgres, governance JSONL, staging evidence, and derived read models;
- connector blocking, manual evidence joins, sequential-plan churn, brittle runbooks, and other friction being normalized instead of fixed;
- duplicate documents, over-governance, premature abstraction, and maintenance costs that exceed risk reduction;
- Windows-local packaging, dependency, upgrade, portability, and disaster-recovery risks;
- architecture choices that could obstruct the Context and Tool Gateway, Observatory, IMI, marketplace services, model-agnostic operation, or multi-agent work.

Also identify at least five areas that are sound and should not be rewritten merely for novelty.

### 7. Independent architecture challenge

Challenge the current design from first principles using the same evidence. Explicitly assess whether Postgres remains the right first structured foundation, whether the six record families are correctly split and owned, whether governance and audit should remain separate, whether the derived governance read model and connector telemetry are worth building, whether the packet/milestone process earns its operating cost, and where Kaizen risks becoming elaborate project management instead of a useful evidence engine.

Do not recommend fashionable technology without a demonstrated Kaizen problem.

### 8. Next milestone recommendation

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
12. source and limitation register;
13. error, gap, and improvement register with blocking classification;
14. test-coverage and missing-proof matrix;
15. security, privacy, retention, and recovery findings;
16. simplification and bureaucracy-reduction recommendations;
17. independent architecture challenge and alternative design analysis;
18. strengths-to-preserve register.

## Restrictions

- read-only audit;
- no repository writes;
- no owner-private oracle access unless explicitly supplied;
- no direct promotion of recommendations into doctrine;
- no generic advice detached from Kaizen's actual repositories;
- no recommendation for broad rewrites without concrete evidence;
- disclose inaccessible repositories, missing tools, and unverified claims.
