# Kaizen Docs Repo

This repository is the planning and stewardship workspace for the Kaizen project.

Kaizen is intended to become a continuously improving, agent-connected project intelligence system built around a dynamic Obsidian vault. It will gather and evaluate external and operational evidence, compound reusable knowledge across projects, support human strategy and technical decisions, and produce audited implementation-ready handoffs for coding agents working in each project's own source repository.

The intended lifecycle is a loop:

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

This repo is **not** the future Obsidian vault. It is where we align the vision, research unresolved architecture, define governance and document contracts, audit the planned system, and prepare the eventual implementation roadmap.

## Start here

LLMs and humans should begin with:

1. [`00-entrypoint/LLM_START_HERE.md`](00-entrypoint/LLM_START_HERE.md)
2. [`01-project-standard/kaizen-vision-and-architecture-alignment.md`](01-project-standard/kaizen-vision-and-architecture-alignment.md)
3. [`ROADMAP.md`](ROADMAP.md)

The entrypoint defines the complete read order and authority hierarchy.

## Current status

- Active phase: vision and architecture alignment
- Planning roadmap: `ROADMAP.md`
- Source of truth: this repo for Kaizen planning and stewardship only
- Future production Obsidian vault: not created yet
- Operational Postgres database: not implemented
- Observatory domain: not implemented
- Qdrant retrieval layer: not implemented
- MCP and typed agent tools: not implemented
- Hermes canonical read/search: planned
- Hermes write access: not approved
- Implementation roadmap: intentionally deferred until planning gates pass

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

- the Kaizen vision and planning roadmap
- project-standard drafts
- research prompts and evidence summaries
- architecture and governance decisions
- document and system specification drafts
- Hermes and multi-agent boundary planning
- Obsidian, Operational Postgres, Observatory, Qdrant, and MCP planning
- validation, promotion, and hammer-test strategy
- handoff and implementation-readiness contracts

This repo does **not** hold:

- running Kaizen code
- the production Obsidian vault
- private customer or project data
- generated build artifacts
- live Postgres or Qdrant state
- active business operations
