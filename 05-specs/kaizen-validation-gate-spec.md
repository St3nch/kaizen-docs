# Spec - Kaizen Validation Gate

Status: implemented baseline
Date: 2026-06-04
Updated: 2026-06-09
Related decisions:
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0003-raw-markdown-is-canonical.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`
- `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`

## Purpose

Define the deterministic validation layers that check Kaizen notes intrinsically and check proposed promotion or amendment operations in context.

The gate validates structure, identity, references, policy compliance, and authority transitions. It does not decide whether an argument is wise or a design is correct.

## Design goal

The validator answers:

```text
Is this exact note intrinsically valid, and does the proposed governed mutation satisfy the accepted v0.2 operation-context rules?
```

It returns:

- pass/fail status
- stable validation run ID
- exact file path
- specific error codes and messages
- warnings
- normalized metadata summary
- raw output suitable for humans, agents, and CI

A failed validation returns a nonzero exit code.

## Validation layers

### Intrinsic note validation

Checks UTF-8 Markdown, flat YAML, required fields, enums, identity, body sections, lifecycle and authority combinations, provenance, links encoded in the note, and prohibited content.

Intrinsic validation does not prove destination safety, canonical dependency existence, Git state, approval freshness, or mutation authorization.

### Operation-context validation

Checks fixed roots, destination placement, canonical relationship and local-link resolution, exact source/prior/candidate/diff/validation hashes, packet and operation binding, Git branch and HEAD, clean-state scope, governance-log binding, approval freshness, and filesystem safety.

Only the full governed operation may authorize canonical mutation.

## Validation modes

### Staging validation

Checks a file in the sibling staging root. Allows agent provenance and `validation_status`.

### Canonical validation

Checks existing canonical content. Rejects staging-only fields and non-canonical syntax.

### Governed-operation validation

Runs against exact immutable operation evidence and the proposed canonical destination. Includes intrinsic checks plus promotion- or amendment-specific transition, dependency, event, Git, hash, and filesystem checks.

## Required checks

### 1. File and root check

- file must be UTF-8 Markdown
- staging validation path must resolve inside the configured staging root
- canonical path must resolve inside the configured canonical root
- path checks must use final resolved filesystem targets, not string-prefix comparison alone
- relative traversal, absolute paths, drive-relative paths, UNC paths, device paths, and extended-length paths must fail where not explicitly allowed
- symbolic-link, junction, mount-point, and other reparse-point escape must fail
- destination must not silently overwrite an unrelated note

### 2. YAML/frontmatter parse

- parseable flat YAML frontmatter is required
- nested objects are forbidden
- duplicate keys are errors
- unknown fields warn during early v0.2 rollout and later become errors

### 3. Universal fields

Every durable note requires:

```yaml
id:
type:
status:
project:
summary:
created:
updated:
```

### 4. ID validation

`id` must:

- match `kz-<registered-type-prefix>-<valid-ulid>`
- be globally unique
- match the note's registered type prefix
- remain unchanged after creation
- never be reused after archival or deletion

No separate UUID is required in v0.2.

### 5. Enum validation

```yaml
status:
  - draft
  - active
  - blocked
  - complete
  - archived

review_status:
  - not-required
  - pending
  - approved
  - rejected

authority:
  - none
  - proposed
  - accepted

confidence:
  - low
  - medium
  - high

validation_status:
  - pending
  - passed
  - failed
```

`type` must be one of the nine v0.2 note types in `05-specs/kaizen-note-type-registry.md`.

### 6. Date validation

- `created` and `updated` must be ISO-8601
- `created` is immutable
- `updated` must be greater than or equal to `created`
- timestamps unreasonably in the future are errors

### 7. Type-specific fields

The validator enforces the requirements in `05-specs/kaizen-note-type-registry.md`.

Examples:

- `command-center` and `current-state` require `pipeline_stage`
- `source-summary` requires one independently governed source unit and separated evidence/interpretation sections
- `claim` requires `confidence`, stable source references, and one core assertion
- `spec` requires `related_decisions`
- `audit` requires at least one governed target relationship; any `audit_verdict` must be human-issued
- `task-packet` requires `primary_spec`, runnable validation, and a completion-report section

### 8. Required body sections

Each type must contain its registered H2 sections.

Heading names are exact unless the registry explicitly defines aliases later.

Empty required sections are errors.

### 9. Link and reference validation

Canonical content rules:

- frontmatter relationships contain stable IDs only
- each referenced stable ID resolves to exactly one canonical note or approved external registry object
- body links use relative Markdown links
- internal body links resolve
- Wikilinks are errors in canonical content
- Wikilinks in staging are warnings and must be normalized before promotion
- canonical notes may not link into the staging root

### 10. Source and provenance validation

- source summaries require `source_docs` or `source_urls`
- accepted claims require stable `source_docs`
- source IDs and URLs must be syntactically valid
- agent-created staged notes require `agent`, `model`, and `session`
- durable agent provenance must remain unchanged through promotion
- humans must not fabricate agent provenance on human-authored notes

### 11. Lifecycle and authority combinations

The gate rejects impossible or unauthorized combinations.

Examples:

- `status: draft` with `review_status: approved`
- `review_status: approved` without a human promotion event
- `review_status: rejected` set by an agent
- `authority: accepted` without `review_status: approved`
- `authority: accepted` without a matching human promotion event
- `authority: accepted` on a type that cannot carry authority
- `task-packet` marked accepted when its primary governing spec is not approved and accepted
- `audit_verdict` set by an agent or without a matching human promotion event
- `audit_verdict: pass-with-notes` while blocker/major findings or promotion-required conditions remain

`status: active` means usable at its current authority level and does not imply review approval.

### 12. Staging-transient validation

Staged agent content requires:

```yaml
validation_status: pending | passed | failed
agent:
model:
session:
```

Canonical promoted content must not contain `validation_status`.

### 13. Governed mutation event validation

Every governed promotion or amendment requires append-only event evidence conforming to `05-specs/promotion-event-schema.md` and the governed mutation workflow.

The event must:

- reference the exact note ID
- reference the exact validation run ID
- identify a human approver
- record prior and new review/authority values
- record source and destination paths where applicable
- bind the exact plan and approval SHA-256 values
- bind prior/new content and reviewed diff hashes for amendment
- include an approval basis
- use a unique immutable event ID

Past events must not be modified or deleted.

### 14. Supersedence validation

When `supersedes` or `superseded_by` is present:

- target IDs must resolve
- reciprocal fields must agree
- replacement must contain `## Supersedence rationale`
- human approval must match the prior record's authority posture
- promotion events must cover both the replacement and prior-record update
- prior content remains canonical history

### 15. Duplicate detection

Before promotion, check:

- duplicate stable ID
- exact title match
- exact summary match
- normalized filename collision
- existing frontmatter relationships indicating the same concept

Semantic duplicate detection through Qdrant is deferred until Qdrant exists. It may warn but must never auto-merge.

### 16. Folder placement

The canonical vault uses the accepted destinations per note type.

Initial direction:

| Type | Candidate location |
|---|---|
| `command-center` | project root |
| `overview` | project root |
| `current-state` | project root |
| `source-summary` | earned `source-summaries/` folder |
| `claim` | earned `claims/` folder |
| `decision` | earned `decisions/` folder |
| `spec` | earned `specs/` folder |
| `audit` | earned `audits/` folder |
| `task-packet` | earned `handoffs/` folder |

The docs repo may use separate placement rules.

### 17. Raw/bulk data boundary

Canonical Markdown must not become a raw data warehouse.

The validator should flag:

- large raw JSON arrays
- crawl payloads
- SERP dumps
- excessive tabular observation rows
- logs
- binary data encoded into Markdown
- unusually large files requiring review

Thresholds remain to be calibrated and must allow legitimate long specs/reports.

### 18. Private/customer data and secrets

Promotion and indexing fail when prohibited content is detected, including:

- credentials and API keys
- private/customer identifiers
- data covered by explicit exclusion policy

Detection must be conservative and support reviewed false-positive overrides. Overrides require a human event and may never permit secrets into canonical content.

### 19. Approval freshness and scoped drift

Approval binds to exact reviewed content and relevant reviewed state.

For governed audits and task packets, promotion validation records or verifies:

- exact artifact content hash
- validator version and validation run ID
- governing decision/spec IDs
- reviewed repository and ref when source-repository state is relevant
- reviewed paths, interfaces, or schemas
- relevant dependency versions when material
- required hammer evidence

A prior review becomes stale when changed state affects reviewed paths, in-scope files, interfaces, schemas, governing invariants, validation commands, or relevant dependencies.

Unrelated commits and formatting-only changes do not automatically stale approval. Hashes detect change; a governed review determines materiality.

### 20. Protected content

The gate blocks unauthorized changes to:

- accepted decisions
- accepted specs
- accepted task packets
- project standard doctrine
- source-of-truth boundary docs
- Hermes governance skills
- prior promotion events

Changes to accepted governed content require a reviewed diff and appropriate supersedence or amendment workflow.

## Non-goals

The validation gate does not:

- determine whether research is true
- determine whether a design is optimal
- issue or change a human audit verdict
- approve authority transitions
- replace frontier-model or human review
- mutate Qdrant or Postgres

## Suggested command shape

```text
kaizen-validate <path> --mode staging|canonical|promotion
```

Possible companion commands:

```text
kaizen-id <type>
kaizen-check-links <root>
kaizen-check-references <root>
kaizen-validate-promotion <staged-path> <destination-path> <event-json>
```

## Implemented-baseline evidence

- final `pipeline_stage` enum accepted;
- machine-readable note and event prefix registries implemented;
- staging, canonical, and governed-operation validation implemented;
- canonical folder placement implemented;
- staging and canonical roots fixed and configurable through the approved platform configuration;
- duplicate, relationship, local-link, lifecycle, authority, path, Git, event, and hash checks implemented;
- first-time promotion and bounded amendment task packets implemented and audited;
- full platform suite passed 230 tests at Milestone 4 closure.

Machine-readable schema refinements must remain backward-compatible with existing canonical notes and event lines.

## Implementation location

The first implementation uses Python 3.11.15 in:

```text
C:\dev\kaizen\platform
```

The primary CLI entry point is:

```text
kaizen-validate <path> --mode staging|canonical|promotion
```

Machine-readable validator contracts belong under the platform repository's `schemas/` directory and must be reviewed against the accepted Markdown specifications.

## Open questions

- Exact file-size and raw-data thresholds.
- Whether validation reports become durable Postgres records later.
- When unknown fields move from warning to error.

## Related files

- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-write-access-preconditions.md`
