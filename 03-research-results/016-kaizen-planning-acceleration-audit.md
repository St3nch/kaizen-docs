# Research Result 016 - Kaizen Planning Acceleration Audit

Status: stewarded evidence summary
Date: 2026-06-07
Source: Claude repository audit supplied by the project owner

## Purpose

Preserve the planning audit that evaluated whether Kaizen had enough accepted doctrine to begin a first implementation vertical slice without completing every remaining research lane.

This document is evidence, not doctrine. Its recommendations are adopted, modified, rejected, or deferred below.

## Executive finding

The audit found that Kaizen's governance foundation is substantially coherent and that the planning process had begun treating useful perimeter research as though it blocked the first implementation slice.

The central acceleration finding is adopted:

```text
research dangerous or expensive-to-reverse unknowns
-> freeze the minimum safe contracts
-> build one narrow vertical slice
-> test it
-> feed implementation evidence back into doctrine
-> expand
```

## Adopt

### First implementation does not require the perimeter systems

The first vertical slice does not require:

- Operational Postgres;
- the Internet Marketing Intelligence database;
- Qdrant;
- Hermes integration;
- DataForSEO;
- marketplace-ranking research;
- the desktop control shell;
- an Obsidian bridge plugin;
- final provider-rights policy;
- advanced backup architecture;
- knowledge-quality scoring.

These remain later implementation-phase inputs unless a concrete dependency emerges.

### Decision 0008 is the immediate doctrine gate

The operating conventions must be tested through one realistic project chain, revised where needed, and accepted before validator behavior is frozen.

### First vertical slice direction

The first slice should prove:

```text
canonical Markdown
-> deterministic validation
-> sibling staging
-> human-controlled promotion
-> task packet
-> implementation completion evidence
-> governed project-state update
```

### Claude usage should be concentrated

Use Claude for:

- focused repository audits;
- task-packet drafting and review;
- post-slice contract retrospectives.

Do not spend limited Claude research capacity on broad work that does not protect canonical data, authority, security, customer information, money, or an expensive-to-reverse boundary.

### Batch C and Batch D are not first-slice gates

For the first slice:

- canonical backup is Git plus a remote;
- staging is disposable;
- Postgres and Qdrant recovery are not applicable because those systems are not yet present;
- knowledge quality and context assembly should be evaluated after real promoted notes exist.

## Modify

### A minimum contract freeze is still required

The project should not code directly from an unqualified collection of drafts.

Before the first task packet executes, Kaizen needs a small accepted implementation baseline covering:

- Decision 0008 operating conventions;
- document and metadata contracts used by the slice;
- validation behavior;
- promotion behavior;
- implementation repository and language choice;
- first-slice scope and exclusions.

This is a compact closure batch, not a return to broad planning.

### Promotion remains a high-risk write boundary

A human-operated promotion CLI still writes canonical files and must enforce:

- staging-root confinement;
- canonical-root confinement;
- traversal and reparse-point defenses;
- destination placement;
- stale-content and hash checks;
- collision prevention;
- recoverable event and file sequencing.

Agent absence does not make path and recovery risks disappear.

### The first implementation milestone should be cohesive

The ID generator, Markdown/frontmatter parser, validator, and safe promotion command should be treated as one tools-foundation milestone even if delivered through several task packets.

### The dry run must not duplicate canonical doctrine

A Kaizen meta-project may use disposable fixtures and links to current doctrine. It must not create a second canonical copy of accepted decisions or specifications.

## Reject

Reject these stronger audit suggestions:

- broad path-confinement tests can wait merely because no agent is writing;
- an empty `promotion-log.jsonl` should be committed as a seed file;
- the first slice should proceed directly from drafts without a small accepted contract baseline;
- marketplace Step 6A should be removed from the roadmap;
- document-contract consistency review should be removed entirely rather than compressed into the contract-freeze batch.

## Defer

Defer until the relevant implementation phase:

- marketplace-ranking coverage, before the Intelligence database design session;
- provider-rights completion, before permanent retention or external-use policy;
- storage and recovery research for Postgres and Qdrant;
- knowledge-quality and context-assembly research;
- Decision 0011 acceptance;
- Hermes integration;
- Postgres and Qdrant prototypes;
- desktop-shell implementation.

## Accelerated planning exit direction

The planning roadmap may hand off to an implementation roadmap when:

- Decision 0008 is accepted after the dry run;
- first-slice document, metadata, validation, and promotion contracts are explicitly frozen;
- the tools implementation repository and language are chosen;
- the first vertical slice is scoped with explicit exclusions;
- Implementation Roadmap v0.1 exists for the tools and vault-foundation milestones;
- the first task packet is complete and reviewable.

The exit gate does not require completion of every future research lane.

## Recommended first implementation milestones

```text
Milestone 1 - Kaizen tools foundation
Milestone 2 - Canonical vault bootstrap
Milestone 3 - Safe staging and promotion
Milestone 4 - One complete governed project loop
Milestone 5 - Slice retrospective and v0.2 consolidation
```

## Related files

- `ROADMAP.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `03-research-results/017-decision-0008-end-to-end-dry-run-simulation.md`
