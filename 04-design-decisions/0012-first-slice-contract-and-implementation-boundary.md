# Decision 0012 - First-Slice Contract and Implementation Boundary

Status: accepted
Date: 2026-06-07
Accepted: 2026-06-07
Owner: Kaizen project steward
Related decisions:
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
Related specifications:
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-id-and-prefix-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/kaizen-hammer-test-strategy.md`

## Context

Kaizen has completed enough governance, write-safety, document-contract, and operating-convention work to begin a narrow implementation vertical slice.

The project must now stop treating every future research lane as a prerequisite while still giving coding agents a clear accepted baseline.

This decision freezes only the contracts and repository boundaries needed for the first slice. It does not freeze the final architecture of Postgres, Qdrant, Hermes, the Internet Marketing Intelligence system, provider adapters, or the desktop interface.

## Decision

### 1. Repository boundaries

Use these repository and folder roles:

```text
C:\dev\kaizen-docs
= planning, doctrine, research, accepted decisions, and specifications

C:\dev\kaizen
= non-repository umbrella folder

C:\dev\kaizen\platform
= Kaizen implementation and code repository
```

The umbrella folder `C:\dev\kaizen` must not be initialized as a Git repository.

Future roots are created only when an implementation milestone authorizes them:

```text
C:\dev\kaizen\vault
C:\dev\kaizen\staging
C:\dev\kaizen\data
C:\dev\kaizen\scratch
```

They must not be nested inside the platform Git repository.

### 2. First-slice implementation language

Use Python 3.11.15 for the first Kaizen tools and validation slice.

The initial implementation should use:

- `pyproject.toml`;
- type hints;
- `pytest`;
- a small CLI entry-point layer;
- flat, testable modules;
- no large application framework;
- no network dependency for ID generation or Markdown validation.

The first-slice tooling includes:

- Kaizen ID generation;
- Markdown and flat-YAML parsing;
- deterministic note validation;
- safe human-operated promotion;
- promotion journal and recovery support;
- fixtures and hammer tests required by the first milestone.

Python is a first-slice implementation choice, not a permanent requirement that every future Kaizen component use Python. A future Tauri shell may use Rust and TypeScript without superseding this decision.

### 3. First-slice contract baseline

The following contracts are accepted as the implementation baseline for the first vertical slice:

- the seven universal fields and conditional-field rules in `kaizen-field-registry.md`;
- the nine v0.2 note types, required fields, and required H2 sections in `kaizen-note-type-registry.md`;
- the prefixed-ULID identity and type-prefix rules in `kaizen-id-and-prefix-registry.md`;
- the staging, canonical, and promotion validation modes in `kaizen-validation-gate-spec.md`;
- the sibling staging and human-controlled promotion workflow;
- the append-only promotion-event model;
- the final-path confinement and recoverable promotion requirements;
- the six project stages accepted by Decision 0008;
- the completion-report and governed follow-up loop;
- hammer tests as a hard implementation gate.

These contracts may still be revised from implementation evidence. A change that affects existing canonical content, generated IDs, accepted enums, required fields, placement, or promotion behavior requires compatibility and migration review.

### 4. First vertical-slice objective

The first slice must prove one governed path:

```text
canonical Markdown
-> ID generation
-> deterministic validation
-> sibling staging
-> safe human promotion
-> task packet
-> implementation completion evidence
-> governed current-state update
-> retrospective audit
```

The implementation roadmap may divide this into several task packets, but they form one milestone family rather than unrelated tools.

### 5. Explicit exclusions

The first slice does not include:

- Hermes integration;
- Operational Postgres;
- the Internet Marketing Intelligence database;
- Qdrant;
- DataForSEO or another paid provider;
- marketplace-ranking collection;
- Tauri or another desktop shell;
- an Obsidian bridge plugin;
- production deployment;
- final backup and disaster-recovery architecture;
- advanced knowledge-quality scoring;
- multi-project semantic retrieval.

These systems remain governed future implementation phases.

### 6. Machine-readable registries

The implementation repository must contain machine-readable representations of the accepted registries needed by code.

Initial expected location:

```text
C:\dev\kaizen\platform\schemas\
```

At minimum:

- note-type to prefix mapping;
- field and enum definitions needed by the validator;
- note-type required-field and required-section definitions;
- promotion-event schema or equivalent validated model.

The Markdown specifications remain the human-readable doctrine. Machine-readable files must be generated from or reviewed against the accepted contracts and must not silently become a competing source of truth.

### 7. First implementation roadmap

The next planning artifact is `Implementation Roadmap v0.1` with these milestones:

1. Kaizen tools foundation;
2. canonical vault bootstrap;
3. safe staging and promotion;
4. one complete governed project loop;
5. slice retrospective and v0.2 consolidation.

The first implementation task packet must target the tools foundation and define exact scope, tests, files, non-goals, and completion-report requirements.

## Consequences

### Benefits

- gives coding agents an explicit accepted implementation baseline;
- keeps production code out of the doctrine repository;
- avoids premature repository sprawl;
- lets Kaizen learn from a real vertical slice before finalizing every future system;
- preserves strict write-safety and authority boundaries while accelerating execution.

### Costs

- some draft specifications remain broader than the first slice and must be interpreted through this decision's exclusions;
- later implementation evidence may require migrations or contract amendments;
- Python and the initial repository layout must be maintained consistently through the first milestone.

## Rejected alternatives

### Put implementation code in `kaizen-docs`

Rejected because planning doctrine and executable implementation have different lifecycles, permissions, tests, and release concerns.

### Initialize `C:\dev\kaizen` as the code repository

Rejected because future vault, staging, data, and scratch roots would create nested-repository and accidental-cleanup risks.

### Create a separate `kaizen-tools` repository now

Rejected for the first slice because the platform repository can own the initial tools coherently. A split may be earned later.

### Require Postgres, Qdrant, Hermes, or the desktop shell before the first slice

Rejected because none is required to prove canonical Markdown, validation, staging, promotion, task packets, and implementation feedback.

### Wait for every remaining research batch

Rejected because the unresolved lanes do not protect a first-slice dependency or expensive-to-reverse choice.

## Acceptance criteria

This decision is satisfied when:

- `C:\dev\kaizen\platform` exists as the designated implementation location;
- the roadmap records Implementation Roadmap v0.1 as the next artifact;
- the ID and validation specs no longer list language or repository location as open;
- the first implementation task packet is drafted against this baseline;
- no implementation code is added to `kaizen-docs`.

## Related files

- `ROADMAP.md`
- `00-entrypoint/LLM_START_HERE.md`
- `03-research-results/016-kaizen-planning-acceleration-audit.md`
- `03-research-results/017-decision-0008-end-to-end-dry-run-simulation.md`
