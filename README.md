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

This repository is separate from both the running platform code and the canonical Obsidian vault. It preserves the vision, accepted decisions, specifications, audits, task packets, research evidence, and implementation roadmaps.

## Current state pointer

Do not use this README as the live project-status ledger.

For current posture, latest closed milestone, repository checkpoints, active gate, deferred boundaries, and read-first guidance, start here:

1. [`00-entrypoint/LLM_START_HERE.md`](00-entrypoint/LLM_START_HERE.md)
2. [`ROADMAP_V0.3.md`](ROADMAP_V0.3.md) - active accepted roadmap
3. [`ROADMAP_V0.4.md`](ROADMAP_V0.4.md) - proposed end-to-end roadmap for Kaizen v1 and full-project completion; not accepted until audited and owner-accepted

Historical roadmap files remain preserved for provenance:

- [`ROADMAP.md`](ROADMAP.md) — historical planning provenance only; not a current-gate source
- [`IMPLEMENTATION_ROADMAP.md`](IMPLEMENTATION_ROADMAP.md) — historical completed first-slice roadmap for Milestones 1 through 5
- [`IMPLEMENTATION_ROADMAP_V0.2.md`](IMPLEMENTATION_ROADMAP_V0.2.md) — historical accepted Milestone 6 roadmap snapshot; preserve bytes unless separately approved

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

## Standing caution

This repository records evidence and authority. It does not itself authorize implementation, deployment, database mutation, vault mutation, provider work, or customer-data work. Those require an accepted gate and owner-approved task packet.
