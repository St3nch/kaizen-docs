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
2. [`ROADMAP_V0.3.md`](ROADMAP_V0.3.md) - active planning roadmap through Milestone 8 closure, corrective pre-Milestone-9 work, and the parallel Observatory research track
3. [`03-research-results/162-milestone-8-owner-closure-acceptance.md`](03-research-results/162-milestone-8-owner-closure-acceptance.md) - exact Milestone 8 closure authority
4. [`03-research-results/167-roadmap-lineage-and-corrective-sequence-reconciliation.md`](03-research-results/167-roadmap-lineage-and-corrective-sequence-reconciliation.md) - current corrective planning sequence
5. [`IMPLEMENTATION_ROADMAP_V0.2.md`](IMPLEMENTATION_ROADMAP_V0.2.md) - accepted completed Milestone 6 implementation roadmap snapshot; preserve exact bytes
6. [`ROADMAP.md`](ROADMAP.md) - historical planning provenance

The entrypoint defines the authority hierarchy, current project posture, and task-dependent reading guidance.

## Current status

- Current gate: Milestones 1 through 8 are closed; Result 162 records exact owner acceptance of Milestone 8 closure
- Corrective status: Decision 0017 atomic project-bootstrap doctrine is accepted through Result 213; Packet 014D remains retired unimplemented
- Controlled implementation-return pilot: Phases 1 through 3 are complete and Phase 4 is partially complete; Result 211 records the project-bootstrap reconciliation; approximately 45 percent of Milestone 9 work remains
- Canonical vault: local-only at `2487de669bc44ed50e54fd5dbbfdd128ce659dbb`; current-state SHA-256 is `e50fc7d1d88257ecf6a818a47673775a9f8960e999a6a843872306227dd2b8c7`; no remote exists and no push is authorized
- Platform implementation repository: local-only at `286b8273498e04a77730d6bdbeaadb2b1c7d2d6b`; no remote exists
- Go8 repository checkpoint: `5830962ba34e62bbfb65508307ee0c706ed31e14`
- Kaizen MCP: temporary non-Git proving ground; not production infrastructure
- Live staging root: exists at `C:\dev\kaizen\staging`; no live staging mutation is authorized by the current packet
- Active planning roadmap: `ROADMAP_V0.3.md`; historical roadmap and implementation snapshots remain preserved
- Observatory research: parallel evidence track only; no provider purchase, raw capture, crawler deployment, client-data reuse, physical schema, Postgres, Qdrant, LangGraph, MCP-tool implementation, or hammer execution is authorized
- Hermes integration: deferred; no canonical write authority
- Next valid gate: restart Kaizen MCP, verify the live 19-tool registry, and stop before creating any Northstar bootstrap plan

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
