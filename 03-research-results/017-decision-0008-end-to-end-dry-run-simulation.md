# Research Result 017 - Decision 0008 End-to-End Dry-Run Simulation

Status: stewarded evidence summary
Date: 2026-06-07
Source: Claude read-only simulation supplied by the project owner

## Purpose

Preserve the dry-run simulation used to test whether proposed Decision 0008 gives a coding agent enough operating clarity to implement the first Kaizen validator and vertical slice without inventing workflow rules.

This file is evidence. Decision 0008 becomes doctrine only through its own accepted status.

## Simulation scope

The simulation used a disposable `kaizen-bootstrap` project and walked through:

```text
command-center
overview
current-state
source-summary
claim
decision
spec
audit
task-packet
completion report
current-state amendment
```

The simulation tested:

- project-level pipeline stages;
- canonical folder placement;
- earned-folder creation;
- portable paths;
- plugin-independent operation;
- source relationships;
- staging and canonical placement;
- completion-report feedback;
- validator implications.

## Executive verdict

Decision 0008 is ready to accept with minor revisions.

The folder model, portable-path convention, plugin policy, `.obsidian` policy, source provenance model, raw/private-data posture, and future Qdrant exclusion all survived the simulation.

The material changes are:

- remove `synthesis` and `operate` from the initial project-stage enum;
- clarify that `pipeline_stage` is the current dominant project phase, not a note lifecycle or monotonic state machine;
- make `current-state.pipeline_stage` authoritative during temporary disagreement;
- document the completion-report and implementation-feedback return loop;
- clarify earned-folder creation without weakening path confinement;
- clarify governed source-summary references;
- define human-authored provenance fields as absent rather than empty.

## Adopt

### Six-stage project enum

```text
capture
research
spec
audit
handoff
build
```

Rationale:

- synthesis occurs within research through source summaries, claims, conflicts, and decisions;
- `operate` is not required for the first v0.2 implementation and may be added through a later amendment when a real operational project earns it.

### Folder placement

The project root and earned-folder model gives each initial note type one unambiguous destination.

### Non-monotonic project movement

A project may return from `build` to `spec`, `research`, or another earlier phase when implementation or evidence exposes new work.

### Completion report as evidence

A task packet completion report is implementation evidence, not governing authority.

Binding findings must become governed follow-up artifacts through the normal validation and promotion workflow.

### Source-summary relationship

Accepted claims normally reference stable governed source-summary IDs through `source_docs`. The source URL remains provenance on the source-summary.

## Modify

### Invalid illustrative IDs

The simulation report used readable fake ULID examples. These must not be copied into doctrine or fixtures.

Use either:

```text
kz-<type-prefix>-<ULID>
```

or generated valid 26-character Crockford Base32 ULIDs.

### Human provenance

For human-authored notes:

- omit `agent`, `model`, and `session`;
- do not use empty strings;
- when any agent-provenance field is present, all required provenance values must be non-empty and valid.

### Human canonical editing

Decision 0008 should not imply that every human typo correction requires staging and promotion.

Humans may edit canonical Markdown directly when authorized. Agent-created or staged candidate artifacts require validation and human promotion. Authority transitions and governed staged promotions remain auditable.

### Earned-folder creation

A promotion operation may create a missing registered destination directory only after proving the resolved destination remains strictly inside the canonical project root and passes traversal, reparse-point, symlink, junction, and placement checks.

## Reject

Reject these interpretations:

- `pipeline_stage` is a per-note stage;
- project stages are monotonic;
- empty provenance strings are acceptable;
- a source-summary ID is only a temporary substitute for a future source registry;
- missing folders may be created without final-path confinement;
- completion-report findings automatically become doctrine.

## Validator implications

### Deterministically enforceable

- stage enum membership;
- type-to-folder placement;
- registered earned-folder destination;
- portable canonical relationships;
- required fields and headings;
- human versus agent provenance shape;
- staging/canonical mode rules;
- promotion-event schema;
- completion-report amendment event structure.

### Human-review only

- whether the selected project stage accurately describes the current dominant phase;
- whether a completion-report finding requires a new claim, decision, spec, audit, or current-state amendment;
- materiality of warning-based raw/private-data findings.

### Deferred

- Qdrant current-state filtering enforcement;
- operational-project `operate` semantics;
- source-registry migration behavior beyond stable source-summary relationships.

## Decision consequence

The simulation satisfies Decision 0008's real-project dry-run acceptance condition.

After applying the reconciled revisions, Decision 0008 can be accepted and used as the first-slice operating baseline.

## Related files

- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `03-research-results/016-kaizen-planning-acceleration-audit.md`
