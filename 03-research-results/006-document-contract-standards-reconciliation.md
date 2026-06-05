# Research Reconciliation 006 - Kaizen Document Contracts

Status: active draft
Date: 2026-06-05
Source: `Kaizen Document Contract Standards - Research Report` supplied as an external project source

## Purpose

Reconcile the completed document-contract research with accepted Kaizen decisions and active draft specifications.

The source report remains evidence. This file records the Kaizen-specific disposition and does not silently promote every recommendation into doctrine.

## Scope

This reconciliation covers:

- `source-summary`
- `claim`
- `decision`
- `spec`
- `audit`
- `task-packet`
- approval freshness and scoped repository drift
- validation, promotion, and hammer-test implications
- Hermes participation in the document pipeline

## Stewardship constraints

1. Raw Markdown with flat validated YAML remains canonical for project intelligence.
2. Humans retain approval, verdict, promotion, and accepted-authority powers.
3. Hermes is a required Kaizen vault consumer and designated agent clerk, but receives only the permissions proven safe by tooling and hammer tests.
4. Mechanical validation, frontier-model review, and human judgment remain distinct.
5. New fields and sections must earn their existence through concrete machine, review, or governance value.
6. Research evidence does not become doctrine merely because it is comprehensive.

## Adopt

### Bounded source units

A source summary covers one independently governed source unit. A source unit may be one document, one versioned report, one repository snapshot, one bounded dataset release, or another artifact reviewed as a coherent whole. It is not necessarily one URL.

### Evidence and interpretation separation

Source summaries separate extracted evidence from interpretation. Evidence items include precise locators when the source supports them.

### One core assertion per claim

Each claim expresses one evidence-checkable assertion. Supporting and conflicting evidence remain explicit and visible.

### Decision immutability in meaning

Accepted decisions are not silently rewritten. Material changes use supersedence. Clerical corrections use a governed amendment or correction path.

### No hidden decisions in specs

A spec must not smuggle in unresolved authority-bearing choices. Governing decisions must be explicit and linked.

### Accepted does not equal implementation-ready

An accepted spec may still lack implementation detail, validation, dependency resolution, or a task-packet-ready boundary.

### Human audit verdicts

Agents may draft audit evidence, findings, severities, contradictions, and recommendations. Only a human issues the verdict.

### Exact-content approval binding

Approval binds to the reviewed artifact content, validator evidence, governing artifacts, and relevant repository state.

### Scoped drift

Repository changes stale an audit or task packet only when they affect reviewed assumptions or boundaries. Unrelated commits do not automatically invalidate every approval.

### Primary governing spec

A task packet identifies one primary governing spec. Additional specs may be referenced as supporting constraints.

### Explicit completion reporting

Task packets require a completion report describing changes made, validation run, residual issues, and deviations.

## Modify

### "One source only"

Modify to "one independently governed source unit." Literal one-URL enforcement would fragment coherent artifacts and confuse mirrors, versions, appendices, and repository snapshots.

### Mandatory quotations

Precise locators are required where available; direct quotation is not universally required. Valid evidence forms include quotations, paraphrases, data points, code references, figure/table references, and timestamped observations.

### Confidence calculation

Do not calculate confidence from crude source counts. Confidence remains `low`, `medium`, or `high` and must be justified by evidence quality, independence, directness, conflict, and caveats.

### Alternatives in decisions

Record genuine alternatives considered. Do not fabricate a minimum number to satisfy a template.

### Given/When/Then

Use Given/When/Then when it improves behavioral precision. Do not require it for every acceptance criterion.

### Content hashes and materiality

Hashes detect exact change. A human-reviewed or governed workflow determines whether a change is clerical, material, or irrelevant to an existing approval.

### Audit verdict vocabulary

Use:

```text
pass
pass-with-notes
fail
stale
```

`pass-with-notes` permits only non-blocking observations. Any unmet condition required before promotion produces `fail`, not conditional approval.

### Task-packet shape

Use a lean required core plus conditional sections. Do not force every packet into a universal large template.

### Approval-binding metadata

Store detailed approval context primarily in audit and promotion events rather than inflating ordinary note frontmatter.

## Reject

- `approve-with-conditions`
- source-count confidence formulas
- mandatory fabricated alternatives
- universal fifteen-section task packets
- audit authority as accepted doctrine

Audit notes retain `authority: none`. Their operational force comes from the human verdict, reviewed evidence, and promotion event.

## Defer

- A dedicated source-registry note type until ingestion work earns it.
- A structured materiality-classification event vocabulary until real amendment workflows are exercised.
- Automatic repository-drift classification until scoped paths and dependency rules can be tested on a real project.
- Additional confidence vocabulary beyond `low`, `medium`, and `high`.
- A separate completion-report note type.

## Reconciled contracts

### Source summary

Required body sections:

```text
Summary
Extracted Evidence
Interpretation
Provenance
Caveats
```

Conditional:

```text
Project Relevance
```

Rules:

- one independently governed source unit
- evidence items identify evidence type and precise locator when available
- interpretation must not masquerade as extracted evidence
- provenance identifies the reviewed source version or retrieval context

### Claim

Required body sections:

```text
Statement
Supporting Evidence
Conflicting Evidence
Confidence and Caveats
```

Rules:

- one core assertion
- stable evidence relationships before accepted authority
- empty conflicting evidence is stated explicitly rather than silently omitted
- confidence includes a short rationale

### Decision

Required body sections:

```text
Context
Decision
Alternatives
Consequences
Evidence
```

Conditional:

```text
Supersedence Rationale
```

Rules:

- formal decisions are reserved for consequential or difficult-to-reverse choices
- alternatives are genuine, not quota-driven
- accepted meaning is immutable
- material change uses supersedence

### Spec

Required body sections:

```text
Goal
Scope
Non-Goals
Requirements
Constraints
Acceptance Criteria
Open Questions
```

Conditional when applicable:

```text
Context
Interfaces and Data
Failure Behavior
Migrations
Test Strategy
Dependencies
Examples
```

Rules:

- unresolved correctness questions block task-packet readiness
- requirements may use MUST, MUST NOT, SHOULD, and MAY when useful
- acceptance criteria are objective but need not always use Given/When/Then

### Audit

Required body sections:

```text
Scope
Evidence Reviewed
Findings
Contradictions and Gaps
Recommendation
Human Verdict
```

Finding severities:

```text
blocker
major
minor
info
```

Human verdicts:

```text
pass
pass-with-notes
fail
stale
```

Rules:

- Hermes or another agent may draft findings but not the verdict
- `pass-with-notes` contains no blocking or promotion-required condition
- material scoped drift changes the verdict to `stale`
- audit authority remains `none`

### Task packet

Required body sections:

```text
Objective
Read First
Scope
Non-Scope
Implementation Requirements
Constraints
Validation
Acceptance Criteria
Completion Report
```

Conditional when applicable:

```text
Context Pack
Allowed Changes
Do Not Touch
Output Location
Interfaces and Data
Migrations
Documentation Updates
Rollback and Recovery
Dependencies and Blockers
```

Rules:

- exactly one primary governing spec
- exact source-repository context and reviewed ref
- runnable validation commands or explicit validation procedures
- objective acceptance criteria
- completion report is mandatory
- conditional sections become required when the implementation touches their concern

## Approval freshness and scoped drift

Approval evidence should bind to:

- exact artifact content hash
- validator version and validation run
- governing decision and spec IDs
- reviewed repository and ref
- reviewed paths or interfaces
- relevant dependency versions when material
- hammer-test evidence when required

A review becomes stale when changes affect:

- `Read First` paths
- in-scope files or directories
- referenced interfaces, schemas, or migrations
- governing decisions or spec invariants
- validation commands
- relevant dependency versions

Formatting-only and unrelated changes do not automatically stale approval. Hashes identify change; governed review determines materiality.

## Hermes reconciliation

Hermes will be used with the Kaizen vault and must be treated as a first-class system consumer.

Required posture:

- Hermes receives reliable read and search access to approved canonical Markdown.
- Hermes may draft source summaries, claims, audits, and task packets within its registered boundaries.
- Hermes may write only to the sibling staging root after root-scoping and escape tests pass.
- Hermes must preserve precise source locators and distinguish evidence from interpretation.
- Hermes must emit diffs before proposed changes to existing canonical content.
- Hermes may not issue audit verdicts, approve artifacts, accept authority, promote content, mark implementation-ready, mutate Postgres or Qdrant directly, or commit/push.

Kaizen therefore designs for Hermes from the beginning while withholding each authority-bearing or mutating capability until its controls are proven.

## Files affected by this reconciliation

- `00-entrypoint/LLM_START_HERE.md`
- `01-project-standard/standard-revision-plan.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`

## Remaining decisions

- Human acceptance of this reconciliation and updated draft contracts.
- Final Decision 0008 review after a real-project dry run.
- Exact machine schema for audit verdicts and approval-binding context.
- Exact reviewed-repository-state representation in promotion events.
- Tooling implementation location and language.
