---
id: kz-dec-01KXPROJECTBOOTSTRAP0017
type: decision
status: active
project: kaizen-platform
created: 2026-06-15T10:45:00-04:00
updated: 2026-06-15T11:10:00-04:00
accepted: 2026-06-15T11:10:00-04:00
accepted_by: owner.local
accepted_source_sha256: 0aa6a2799bee035eccb758a4fc3d6676c7dcc73547331d692295c05389898abf
review_status: approved
authority: accepted
summary: "Define a canonical project as an atomically published three-note governed bootstrap rather than a side effect of single-note promotion."
---

# Decision 0017 - Atomic Governed Project Bootstrap

## Context

Decision 0008 and the authoritative Kaizen Project Standard v0.2 define the minimal canonical project root as:

```text
projects/<project-slug>/
  command-center.md
  overview.md
  current-state.md
```

They also define governed creation of missing earned folders for note types such as claims, specs, audits, and handoffs. They do not define how a new canonical project root itself is created.

Packet 014A Phase 4 exposed that gap when an approved first promotion to `projects/northstar-stockroom/overview.md` failed safely because the project directory did not exist. Packet 014D proposed creating the missing project directory as a side effect of promoting one root note. Independent audit found that this could create a partial canonical project with no command-center and no current-state.

A project root is not an earned folder. It is the canonical orientation boundary for the whole project and must not become authoritative through whichever root note happens to be promoted first.

Decision 0015 remains reserved for governance-compression reconciliation. Decision 0016 remains the accepted Northstar baseline business-rule decision.

## Decision

### 1. Canonical-project completeness invariant

A new project becomes canonical only when all three required root notes are published together:

```text
command-center.md
overview.md
current-state.md
```

The project root and the three root notes form one governed bootstrap unit.

A directory containing only one or two of the required notes is not an accepted canonical project state.

### 2. Separate governed operation

Project bootstrap is a distinct governed mutation operation, not a side effect of ordinary single-note promotion.

The operation class is conceptually:

```text
bootstrap-project
```

Its authority unit is the complete project root, not one note.

Ordinary single-note promotion remains the mechanism for:

- notes promoted into an already canonical project;
- later root-note amendments through governed amendment controls;
- earned-folder creation where Decision 0008 already permits it.

### 3. Atomic publication boundary

Bootstrap execution must provide one atomic canonical visibility boundary for the complete project root.

Before the publication boundary, the project must remain absent from accepted canonical state.

After the publication boundary, the project root must contain exactly the three approved root notes with their approved bytes.

An implementation may use an atomically published prepared project directory or another mechanism only when it proves equivalent no-partial-visibility behavior under the supported Windows filesystem contract.

A sequence that publishes the three notes individually into the final project root is not acceptable for v0.2.

### 4. Immutable plan binding

A bootstrap plan must bind at least:

```text
operation ID
packet ID
project slug
expected absent canonical project root
canonical root identity
platform repository identity
vault repository identity
required governance-log SHA-256
command-center source path and source SHA-256
command-center canonical candidate SHA-256
overview source path and source SHA-256
overview canonical candidate SHA-256
current-state source path and source SHA-256
current-state canonical candidate SHA-256
exact three destination paths
cross-note validation result
bundle SHA-256
review context
```

The plan must fail closed if the project root, any destination, any candidate, any repository binding, the governance log, or required ancestor posture changes before execution.

### 5. Cross-note validation

Before plan creation, the three candidates must:

- pass their individual note-type contracts;
- use the same exact project slug;
- resolve to the exact three root-note filenames;
- contain unique valid Kaizen IDs of the correct note types;
- contain no conflicting source-of-truth boundary statements;
- contain no contradictory do-not-touch boundaries;
- use compatible current-state and command-center orientation;
- use the same initial `pipeline_stage` on command-center and current-state;
- identify the same current repository and governance checkpoint where such checkpoints are stated.

`current-state.pipeline_stage` remains authoritative after bootstrap under Decision 0008. Equality is required only for the initial bootstrap bundle so the new project does not begin in contradiction.

### 6. Project-root creation authority

Creation of `projects/<project-slug>/` is a canonical mutation authorized only by the approved bootstrap plan.

The operation may create exactly one direct child beneath canonical `projects/` and only for the bound normalized project slug.

It may not expose generic directory creation, recursive earned-folder creation, arbitrary path arguments, direct Go8 canonical writes, or manual canonical bootstrap as the normal workflow.

### 7. Event and evidence model

Bootstrap must produce append-only governed evidence for the bundle.

At minimum, the operation requires:

```text
intent event
committed event
```

The evidence must bind:

- action `bootstrap-project`;
- project slug;
- exact root-note IDs, destinations, and hashes;
- bundle SHA-256;
- plan SHA-256;
- approval SHA-256;
- whether the project root was absent as required;
- installed project-root identity;
- final verification of all three note hashes.

A committed event is valid only when the complete project root verifies.

### 8. Crash and recovery posture

Any crash or interruption before the committed event leaves the operation nonterminal and requires recovery classification.

Recovery must distinguish at least:

- no prepared directory and no canonical project root;
- prepared directory only;
- prepared directory with incomplete root-note set;
- prepared complete bundle not yet published;
- published complete project root without committed event;
- published partial or mismatched project root;
- committed event with missing or mismatched canonical state;
- forbidden reparse or identity substitution;
- unexpected pre-existing project-root content.

Recovery may finish only when exact plan, approval, bundle, filesystem identity, and content hashes remain provable.

Recovery must not broadly delete evidence, infer approval, overwrite unexpected content, or convert a partial project into accepted state by guesswork.

### 9. Concurrency and idempotency

The canonical-root mutex must cover bootstrap live-state verification, preparation, publication, event append, and terminal verification.

Required behavior:

- two operations for the same project cannot publish divergent roots;
- two operations for different project slugs remain safe and deterministic;
- retries after a verified committed event are idempotent;
- a nonterminal operation cannot be bypassed by a fresh operation for the same project;
- external creation of the project root or untracked content after planning makes the approval stale or blocks execution;
- case-folding, path alias, reparse-point, and project-slug collisions fail closed.

### 10. Git and filesystem authority

Git cleanliness and repository HEAD remain necessary but are not sufficient to describe empty-directory or untracked filesystem state.

Bootstrap planning and execution must bind and reverify the relevant canonical filesystem posture in addition to Git and governance-log state.

The supported implementation must use handle-based final-path and reparse-point verification on Windows. Path-string checks alone are insufficient.

### 11. Fresh-context readiness

A fresh-context resumption trial may treat a project as canonically bootstrapped only after:

1. the bootstrap committed event exists;
2. all three canonical root notes verify;
3. the exact canonical vault paths and governance-log change are committed locally;
4. the vault working tree is clean;
5. the fresh context begins from the canonical command-center, overview, and current-state records rather than prior chat history.

Packet 014A Phase 4 R1 must not begin before these conditions hold.

### 12. Compatibility

The existing `projects/kaizen-platform/` root is grandfathered as pre-decision canonical state. This decision does not require migration or reconstruction of that historical bootstrap.

Existing promotion and amendment evidence remains valid.

Packet 014D is retired unimplemented and preserved as rejected-design evidence.

The retired Northstar overview operation and all retired current-state plans must never be reused.

## Consequences

### Benefits

- closes the missing project-root creation doctrine;
- prevents partial canonical projects;
- gives command-center, overview, and current-state one coherent authority boundary;
- creates a meaningful event and recovery model for project creation;
- makes R1 test real canonical orientation instead of a two-note approximation;
- preserves ordinary promotion as a single-note operation for existing projects;
- creates a permanent end-to-end bootstrap regression target.

### Costs

- requires a new governed operation and tool surface;
- requires bundle-level planning, approval, execution, status, and recovery semantics;
- requires new Windows atomic-publication proof and hammer coverage;
- requires a Northstar command-center candidate and Packet 014A Phase 4 reconciliation;
- delays R1 and Phase 5 until the stronger boundary is proven.

## Rejected alternatives

### Create the project directory as a side effect of the first root-note promotion

Rejected because it can publish a partial canonical project and makes project creation depend on note ordering.

### Promote the three root notes sequentially

Rejected for v0.2 because the canonical project is incomplete between promotions and crash recovery becomes an exposed lifecycle state.

### Allow an explicit incomplete-project canonical state

Rejected for v0.2 because it adds lifecycle, validation, recovery, and agent-orientation complexity without evidence that partial canonical projects are useful.

### Create the project directory manually through Go8

Rejected as the normal workflow because it bypasses the Kaizen canonical control plane and produces no governed project-creation evidence.

### Create the project root during planning

Rejected because planning must not mutate canonical state.

## Required follow-up

After owner acceptance:

1. reconcile Packet 014A Phase 4 against this decision;
2. create and validate the Northstar command-center candidate;
3. update the existing overview and current-state candidates as needed;
4. draft a project-bootstrap specification;
5. draft and independently audit the replacement implementation packet;
6. implement the operation and its MCP surface;
7. run deterministic, recovery, reparse, stale-binding, concurrency, and live end-to-end hammer tests;
8. restart Kaizen MCP;
9. create one immutable Northstar bootstrap plan;
10. obtain exact plan-hash approval;
11. execute exactly once;
12. commit the vault locally;
13. run R1.

No replacement implementation work is authorized by this accepted decision alone.

## Related evidence

- `03-research-results/211-packet-014d-independent-audit-and-project-bootstrap-reconciliation.md`
- `06-handoff-patterns/014d-governed-first-project-directory-creation.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `01-project-standard/kaizen-project-standard-v0.2.md`
- `06-handoff-patterns/014a-controlled-implementation-return-pilot-execution.md`
- `03-research-results/210-packet-014d-first-project-directory-audit.md`
