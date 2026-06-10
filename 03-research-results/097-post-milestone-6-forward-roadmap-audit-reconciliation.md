---
id: kz-result-01KTV7A2N4Q6S8W0Y2Z4ABCDEF
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-10T14:05:00-04:00
updated: 2026-06-10T14:05:00-04:00
review_status: steward-reconciled
summary: "Steward reconciliation of Claude's post-Milestone 6 forward-roadmap audit."
---

# Research Result 097 - Post-Milestone 6 Forward-Roadmap Audit Reconciliation

## Source evidence

Claude inspected `kaizen-docs`, `kaizen-platform`, `kaizen-vault`, `kaizen-staging`, `kaizen-mcp`, and `chatgpt-mcp` read-only and produced the report:

```text
Kaizen Post-Milestone 6 Forward Roadmap Audit
Date: 2026-06-10
Status: research/audit evidence; not doctrine or authorization
```

The report is adopted only through the reconciliation below.

## Deepest finding

The main gap is not MCP packaging or Postgres infrastructure.

Kaizen's differentiator — the governed implementation-return loop — has only been exercised on Kaizen itself. Designing physical Postgres schemas or the long-term production MCP from that self-referential corpus risks encoding the wrong workload.

Before physical data-model design, Kaizen needs evidence from a second project whose inputs, expected decisions, implementation outputs, failures, deviations, and return evidence are known in advance.

## Adopt

### Durable recoverability first

The vault, platform, and immutable staging evidence are currently local-only. A verified off-machine backup and restore proof should be the first post-Milestone 6 implementation milestone.

### Reliability without premature productionization

The stale prepared-candidate loader guard, MCP lifecycle friction, version/tool-registry reporting, connector runbook, and Ruff policy are legitimate reliability work. They should be handled without declaring the temporary MCP proving ground production or widening its authority.

### Controlled second-project pilot before Postgres

Run the full governed implementation-return loop on a purpose-built mock project with predefined inputs, expected outcomes, injected failures, and measurable acceptance criteria.

The pilot must produce workload evidence, not merely another successful demo.

### Workload discovery before physical schema

Record lifecycles, transaction boundaries, write authorities, read patterns, retention, privacy, volume assumptions, and cross-project needs must be derived from the pilot before physical Postgres schemas or typed operational APIs are designed.

### MCP architecture after typed APIs

Current governance tools are largely storage-independent and remain useful. Future job, run, queue, cost, observation, and analytics tools must wait for accepted operational schemas and typed APIs.

## Modify

### Backup recommendation

Backup is elevated from a follow-up item to the leading Milestone 7 candidate. The exact privacy and off-machine mechanism remains an owner decision.

### MCP recommendation

The prior Roadmap v0.3 recommendation is split:

```text
reliability and known-defect closure
!=
production MCP migration
```

Reliability may occur early. Production repository placement, packaging, and migration remain later decisions.

### Second-project proof

Rather than immediately onboarding a live external project with uncontrolled variables, first run a controlled mock project with an answer key. A later real-project pilot should follow if the controlled pilot proves the loop and reveals no blocking contract gap.

## Reject

- physical Postgres schema design from the Kaizen-only corpus;
- productionizing the current `kaizen-mcp` proving ground as the immediate next milestone;
- treating named planning domains as physical schemas;
- designing operational MCP tools before typed service contracts;
- moving canonical Markdown or proven governance JSONL into Postgres merely because Postgres exists;
- combining backup, reliability, pilot, schema, MCP, Qdrant, Hermes, and UI into one milestone.

## Defer

- physical Postgres schemas and migrations;
- typed operational APIs;
- production MCP architecture and packaging;
- Qdrant payload design;
- Hermes integration;
- progressive UI work;
- Internet Marketing Intelligence implementation;
- durable multi-user identity.

## Corrected dependency sequence

```text
Milestone 7: durable recoverability
-> Milestone 8: reliability and known-defect closure
-> Milestone 9: controlled implementation-return pilot
-> Milestone 10: workload discovery and system-of-record reconciliation
-> Milestone 11: operational data foundation
-> typed operational APIs
-> production MCP architecture
```

The detailed controlled pilot is defined at:

```text
05-specs/controlled-implementation-return-pilot.md
```

## Owner decisions still required

- backup privacy and storage posture;
- exact Milestone 7 scope;
- whether the controlled mock pilot is sufficient before a real-project pilot;
- whether governance JSONL remains canonical permanently or receives a future query-only mirror;
- which later project becomes the first real external pilot.

## Steward conclusion

The Roadmap v0.3 draft should be revised to reflect the corrected sequence above. No milestone implementation is authorized by this reconciliation.
