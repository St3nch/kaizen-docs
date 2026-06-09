# Kaizen Docs Repo

This repository is the planning, doctrine, and stewardship workspace for the Kaizen project.

Kaizen is a Markdown-first project-intelligence system designed to move work through a governed loop:

```text
idea or migrated project material
-> research and operational evidence
-> source summaries and reusable knowledge
-> claims, conflicts, decisions, and strategy
-> specifications and audit
-> governed context pack and task packet
-> coding-agent implementation
-> completion reports, tests, deviations, discoveries, and operational results
-> reviewed updates to Kaizen project intelligence
```

This repository is separate from both the running platform code and the canonical Obsidian vault. It preserves the vision, accepted decisions, specifications, audits, task packets, research evidence, and implementation roadmap.

## Start here

LLMs and humans should begin with:

1. [`00-entrypoint/LLM_START_HERE.md`](00-entrypoint/LLM_START_HERE.md)
2. [`IMPLEMENTATION_ROADMAP.md`](IMPLEMENTATION_ROADMAP.md)
3. [`ROADMAP.md`](ROADMAP.md) for historical planning provenance and the current planning gate

The entrypoint defines the authority hierarchy, current project posture, and task-dependent reading guidance.

## Current status

- Current gate: Decision 0013 and supporting spec reconciliation are complete; accept or reject the Kaizen Project Standard v0.2 candidate at SHA-256 `e5749b3a228dc2aaaed634395ed81edd0626511d30ce44ea6f6a4f80684fc95e`; no next implementation milestone is authorized
- Packet 006A: complete and steward-audited
- Packet 006B: complete at platform commit `703d532`; final steward audit Result 031 passed with documented limitations
- Canonical vault: exists at `C:\dev\kaizen\vault`; Milestone 4 governed return amendments are complete at vault `e5e4eec`; the vault is clean and has no remote
- Platform implementation repository: exists at `C:\dev\kaizen\platform`
- Live staging root: exists at `C:\dev\kaizen\staging`
- Active execution roadmap: `IMPLEMENTATION_ROADMAP.md`
- Historical planning roadmap: `ROADMAP.md`
- Operational Postgres database and Observatory domain: not implemented
- Qdrant retrieval layer: not implemented
- Hermes integration: deferred beyond the first slice; no canonical write authority
- Live `_governance`: complete at vault commit `248b26a`; live operator complete at platform commit `1a890dd`; Packet 008B Gate A may only generate a plan after approval, and Gate B remains prohibited

## Stewardship rule

Do not turn research findings directly into doctrine. Research must be reconciled and promoted through review:

```text
research evidence
-> steward reconciliation
-> claims and constraints
-> proposed decision or spec change
-> audit and human review
-> accepted doctrine
```

## Repository purpose

This repo holds:

- Kaizen vision and roadmaps
- project-standard baselines and revision planning
- research prompts and evidence summaries
- architecture and governance decisions
- document and system specifications
- Hermes, Obsidian, database, retrieval, and interface planning
- validation, promotion, recovery, and hammer-test strategy
- task packets and implementation-readiness contracts
- steward audits and reconciliation ledgers

This repo does **not** hold:

- running Kaizen code
- the canonical Obsidian vault
- private customer or project data
- generated build artifacts
- live Postgres or Qdrant state
- active business operations

## Repository boundaries

```text
C:\dev\kaizen-docs      doctrine, planning, specifications, audits, and task packets
C:\dev\kaizen\platform  implementation code and tests
C:\dev\kaizen\vault     canonical project intelligence
C:\dev\kaizen\staging   governed staging evidence and candidates
```
