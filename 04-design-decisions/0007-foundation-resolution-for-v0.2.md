# Decision 0007 - Foundation Resolution for Kaizen v0.2

Status: accepted
Date: 2026-06-04
Related research:
- `03-research-results/003-obsidian-agent-readable-infrastructure-gpt-summary.md`
- `03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md`
- `03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md`

## Context

Kaizen's foundation work left eight unresolved decisions blocking a coherent v0.2 standard: lifecycle enums, identity, domain metadata, internal links, staging location, promotion records, the initial note-type set, and initial required frontmatter.

GPT and Claude independently reviewed the repo and then reconciled their disagreements. This decision records the accepted foundation package.

## Decision

### Lifecycle axes

Kaizen uses three orthogonal fields.

```yaml
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
```

Definitions:

- `status` describes content/work maturity.
- `review_status` describes whether required human review has completed.
- `authority` describes governance weight.

A note becomes `active` when required frontmatter and body sections exist and it is usable at its current authority level. Active does not imply approved.

Supersedence is represented by `supersedes` and `superseded_by`, not by a status value.

Hermes may not set `review_status: approved`, `review_status: rejected`, or `authority: accepted`.

### Identity

Every durable note uses one immutable, tool-generated, globally unique ID:

```text
kz-<type-prefix>-<ulid>
```

Example:

```text
kz-clm-01JX8J3M8R7K9Q2
```

Kaizen does not require a second UUID field in v0.2. Human readability comes from the title, filename, and body links.

### Domain metadata

- `project` is required.
- `domain` is optional and must be lowercase kebab-case when present.
- `subdomain` remains deferred.

### Link convention

- Frontmatter relationships use stable Kaizen IDs.
- Canonical body links use relative Markdown links.
- External references use normal Markdown URLs or explicit repository/path references.
- Wikilinks may appear in staging but must be normalized or rejected before canonical promotion.

### Staging location

Agent staging lives in a sibling folder outside the canonical vault.

Recommended layout:

```text
C:\dev\kaizen-vault
C:\dev\kaizen-staging
```

The staging folder is not required to be its own Git repository. Agent write access remains blocked until root scoping, traversal resistance, and canonical read-only behavior are verified.

### Promotion records

Promotion events use an append-only JSONL log in the canonical vault repository.

Recommended path:

```text
_governance/promotion-log.jsonl
```

Each event records the note ID, prior and new review/authority values, human approver, timestamp, action, and basis.

Git history is supplementary. Postgres may become canonical for promotion events later when the operational API exists.

### Initial note types

Kaizen v0.2 begins with nine types:

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
```

Deferred until earned:

```text
roadmap
raw-source
source-import-map
observatory-insight
opportunity
domain-note
promotion-record
conflict
```

`current-state` is a human-readable project snapshot, not live operational state. Jobs, runs, measurements, queues, and automation state belong in Postgres when implemented.

### Universal frontmatter

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

Review, authority, pipeline, domain, relationships, sources, confidence, supersedence, and provenance fields are conditional by note type.

Agent-created notes retain durable provenance after promotion:

```yaml
agent:
model:
session:
```

`validation_status` is staging-transient and is removed on promotion.

## Deferred decisions

The following remain deferred:

- `authority: doctrine`
- `review_status: in-review`
- a second UUID/slug identity field
- required domain taxonomy
- in-vault staging
- Postgres promotion records
- Qdrant chunk-size and embedding-model choices
- deferred note types listed above

## Consequences

- Kaizen can now define a deterministic v0.2 field and note-type registry.
- ULID generation tooling is required before real note authoring begins.
- The promotion workflow must validate JSONL events.
- The validation gate must distinguish durable provenance from transient workflow state.
- The sibling staging boundary must be hammer-tested before Hermes write access.

## Related files

- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `01-project-standard/standard-revision-plan.md`
