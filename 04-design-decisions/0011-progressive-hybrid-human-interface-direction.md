# Decision 0011 - Progressive Hybrid Human Interface Direction

Status: proposed
Date: 2026-06-06
Owner: Kaizen project steward
Evidence:
- `03-research-results/013-kaizen-human-interface-architecture-claude-summary.md`
- `03-research-results/014-human-interface-technical-verification-and-amendment.md`
Related decisions:
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`

## Context

Kaizen needs both a rich human document workspace and application-like surfaces for portfolio state, review queues, Internet Marketing Intelligence, agents, jobs, costs, and system health.

Obsidian is strong for canonical Markdown, evidence navigation, research, editing, links, Bases, Canvas, and deep human review.

It is not the best sole host for every large structured dataset, live operational workflow, or high-density intelligence interface.

A separate desktop control shell is therefore a strong direction, provided it does not replace Obsidian, become a competing system of record, or bypass Kaizen's typed authority boundaries.

## Proposed decision

Kaizen will pursue a progressive hybrid human-interface direction.

### Obsidian remains the primary document workspace

Obsidian remains the primary human environment for:

- canonical Markdown;
- research and source summaries;
- claims and decisions;
- specifications and audits;
- task packets;
- project narratives;
- accepted opportunity and strategy briefs;
- deep evidence reading and editing.

### A separate desktop control shell is the preferred long-term direction

Kaizen will plan for a desktop control shell for workflows poorly suited to a document editor, including:

- portfolio status;
- review queues;
- staged drafts and validation state;
- Internet Marketing Intelligence;
- ranking and citation history;
- signals, opportunities, experiments, and outcomes;
- agent activity;
- jobs, schedules, costs, failures, and health.

Tauri is the leading implementation candidate, but this decision does not finally select a desktop framework.

### Typed services are the shared authority boundary

All user interfaces, command-line tools, agents, and optional integrations must consume Kaizen-owned typed services for authority-bearing operations and structured systems.

No client receives:

- unrestricted database credentials;
- arbitrary SQL;
- unrestricted Qdrant access;
- broad canonical-vault write authority;
- hidden approval authority.

### Implementation is progressive

```text
prove workflows in Obsidian
-> establish client-neutral typed contracts
-> prototype a minimal desktop shell
-> add intelligence and operational interfaces as their backends mature
```

The first desktop-shell prototype must not require the entire Postgres and Qdrant architecture to be complete.

### Initial integration uses supported navigation mechanisms

Initial integration between the desktop shell and Obsidian should use supported URI and deep-link mechanisms.

A custom Obsidian bridge plugin is not accepted now.

It may be proposed later only when observed workflow friction proves that supported URI, canonical readers, and typed services are insufficient.

## What this decision does not select

This decision does not select:

- final desktop framework;
- frontend framework or component libraries;
- bridge transport;
- bridge-plugin implementation;
- service process topology;
- secret-storage technology;
- updater or rollback mechanism;
- packaging and distribution;
- exact deep-link protocol;
- screen designs;
- mobile or remote interfaces;
- implementation dates.

## Security consequences

Future clients must:

- use narrowly scoped typed commands;
- preserve project, privacy, and authority boundaries;
- avoid direct vault writes outside approved staging and promotion paths;
- distinguish human approval from agent proposals;
- enforce changed-since-review and destination-hash checks;
- keep credentials outside frontend code;
- retain audit and recovery evidence.

A UI capability system is defense in depth, not a substitute for safe service implementations.

## Positive consequences

- Obsidian's ecosystem and document strengths are preserved.
- Kaizen gains a path to serious operational and intelligence interfaces.
- Canonical truth remains portable.
- UI clients can evolve without changing system-of-record ownership.
- The bridge plugin remains small or unnecessary.
- The first useful shell can arrive before every backend is complete.

## Costs and risks

- Two human interfaces must remain consistent without duplicating ownership.
- Typed contracts become a critical dependency.
- Deep links and shared-vault reads require testing.
- Update, secrets, process lifecycle, and optional bridge transport remain future engineering work.
- A desktop shell could distract from proving the core workflow if built too early.

## Acceptance criteria

This decision is ready for human acceptance only when the reviewer confirms:

- Obsidian remains central rather than decorative;
- the desktop shell does not replace canonical Markdown;
- typed services remain the authority boundary;
- Tauri is a leading candidate, not irreversible doctrine;
- the bridge plugin remains optional and evidence-gated;
- the first shell is not blocked on the full database and retrieval stack;
- implementation details remain deferred;
- UI work does not interrupt current planning and Research Batch B.

## Prototype requirements before implementation selection

Later implementation planning should include disposable prototypes for:

- desktop-to-Obsidian note and heading navigation;
- client-neutral canonical reads;
- minimal Home, Projects, and Review Inbox shell;
- changed-since-review and promotion safety;
- intelligence-panel pagination and evidence linking;
- optional bridge only if a concrete feature needs active-note context.

## Open questions

- Is Tauri the final desktop framework after prototypes?
- What is the minimum client-neutral read model?
- What process owns typed services?
- Is any bridge plugin needed?
- How are secrets stored for the chosen process topology?
- What update and compatibility contract is appropriate?
- Which UI surfaces provide enough value to justify custom development first?