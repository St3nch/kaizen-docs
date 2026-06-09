# Spec - Kaizen Field Registry

Status: implemented baseline
Date: 2026-06-04
Updated: 2026-06-09
Related decisions:
- `04-design-decisions/0013-v0.2-first-slice-contract-reconciliation.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0008-v0.2-operating-conventions.md`

## Purpose

Define the governed fields used across Kaizen v0.2 notes. This registry prevents ad hoc metadata growth, overlapping state fields, and frontmatter obesity.

Fields are not required on every note. Each note type selects a small required and optional subset.

## Universal required fields

Every durable Kaizen note requires these seven fields.

| Field | Purpose | Rule |
|---|---|---|
| `id` | immutable machine identity | required; format `kz-<type-prefix>-<ulid>`; tool-generated; never changed or reused |
| `type` | note type | required enum from the note-type registry |
| `status` | content/work maturity | required enum: `draft`, `active`, `blocked`, `complete`, `archived` |
| `project` | owning project | required lowercase kebab-case slug |
| `summary` | one-sentence description | required plain text; target maximum 200 characters |
| `created` | creation timestamp | required ISO-8601; immutable |
| `updated` | last meaningful update | required ISO-8601; must be greater than or equal to `created` |

## Identity rule

Kaizen v0.2 uses one canonical note ID.

```yaml
id: kz-clm-01JX8J3M8R7K9Q2
```

The ID is globally unique, immutable, and independent of the filename/title. Kaizen does not require a separate UUID or semantic slug field in v0.2.

Human readability comes from:

- the note title
- the filename
- relative Markdown links in the body

An optional readable alias may be considered later only if opaque frontmatter relationships create demonstrated friction.

## Lifecycle fields

These axes are orthogonal.

### `status`

Describes maturity and work state.

```text
draft | active | blocked | complete | archived
```

Definitions:

- `draft` - incomplete or not yet suitable for normal use
- `active` - structurally complete enough for normal use at its current authority level
- `blocked` - unable to progress because a dependency, decision, or evidence gap exists
- `complete` - the artifact's intended work is finished
- `archived` - retained for history but no longer active

`active` does not mean approved.

### `review_status`

Describes required human review.

```text
not-required | pending | approved | rejected
```

Definitions:

- `not-required` - this note type or instance does not require a review gate
- `pending` - review is required and has not completed successfully
- `approved` - required human review completed successfully
- `rejected` - required human review completed unsuccessfully

Only humans may set `approved` or `rejected`.

### `authority`

Describes governance weight.

```text
none | proposed | accepted
```

Definitions:

- `none` - informative or evidentiary content with no governing force
- `proposed` - submitted as a candidate governing position
- `accepted` - human-approved and binding within its defined scope

Evidence is represented by note type, sources, and confidence rather than an authority value. Audit notes always use `authority: none`; their force comes from a human verdict and promotion evidence.

Only humans may set `accepted`.

## Conditional fields

| Field | Applies to | Rule |
|---|---|---|
| `review_status` | governed/reviewable types | required where the note-type registry says review applies |
| `authority` | claims, decisions, specs, task packets, other governed types | required where the note-type registry says authority applies |
| `pipeline_stage` | command centers and current-state notes | controlled enum defined by the project standard |
| `domain` | cross-project or domain-oriented knowledge | optional; lowercase kebab-case when present |
| `confidence` | claims and evidence interpretations | controlled enum; initial values `low`, `medium`, `high` |
| `source_docs` | source summaries, claims | list of stable governed source-artifact IDs, normally source-summary IDs |
| `source_urls` | source summaries | list of external source URLs retained as human-readable provenance |
| `related_claims` | decisions, specs, audits, other claims | list of stable Kaizen IDs |
| `related_decisions` | specs and audits | list of stable Kaizen IDs |
| `related_specs` | audits and task packets | list of stable Kaizen IDs |
| `primary_spec` | task packets | exactly one stable Kaizen spec ID; must also appear in `related_specs` when that list is present |
| `audit_verdict` | audits after human verdict | enum: `pass`, `pass-with-notes`, `fail`, `stale`; human-controlled |
| `observatory_result_ids` | future Observatory-derived notes | stable Postgres result IDs; deferred until Observatory exists |
| `supersedes` | claims, decisions, specs | stable Kaizen ID; governance-sensitive |
| `superseded_by` | claims, decisions, specs | stable Kaizen ID; governance-sensitive |
| `conflict_with` | claims and audits | stable Kaizen ID list for explicit unresolved conflicts |
| `approval_event_id` | promoted governed notes | latest authority-bearing promotion-event ID; event history remains append-only in the governance log |

## Agent provenance

These fields are set at creation when an agent drafts or materially generates the note and remain as durable history after promotion.

```yaml
agent:
model:
session:
```

Rules:

- Human-authored notes omit `agent`, `model`, and `session`.
- Humans must not fabricate agent provenance.
- Empty-string provenance values are invalid.
- When agent provenance applies, all required provenance fields must be present and non-empty.
- Promotion must not erase true agent provenance.
- Provenance fields never grant review or authority.

## Staging-transient field

```yaml
validation_status: pending | passed | failed
```

`validation_status` exists only while a note is in staging. It is removed during promotion because the promotion event records the successful gate.

## Deferred or rejected fields

| Field | Direction | Reason |
|---|---|---|
| `uuid` | rejected for v0.2 | the prefixed ULID ID already supplies stable global identity |
| `phase` | rejected | overlaps `status` and `pipeline_stage` |
| `subdomain` | deferred | premature taxonomy |
| `informs_projects` | deferred | add only after cross-project reuse creates real need |
| readable `slug`/`aka` | deferred | add only if opaque relationship IDs create demonstrated friction |
| nested objects | rejected | harder validation and weak Obsidian property support |

## Link and reference rules

- Frontmatter relationships store stable IDs, never filenames or Wikilinks.
- Canonical body relationships use relative Markdown links.
- External references use Markdown URLs or explicit repository/path references.
- Wikilinks are allowed only as staging input and must be normalized or rejected before promotion.

## Registry rules

1. Unknown fields warn during early v0.2 rollout and become errors after stabilization.
2. Unknown enum values are errors immediately.
3. Nested YAML is forbidden in canonical notes.
4. Stable IDs never change or get reused.
5. `created` never changes; `updated` is maintained on meaningful edits.
6. Supersedence requires explicit rationale, reciprocal reference consistency, human approval, and a promotion event.
7. Agent provenance never grants review or authority.
8. Private/customer data is forbidden in canonical frontmatter.
9. New fields require registry review, validation updates, and a decision when governance-sensitive.

## Open questions

- Whether `confidence` needs an `unknown` value.
- When unknown fields should switch from warning to error.
- Whether optional readable aliases become necessary after real authoring.

## Implemented-baseline evidence

This contract was exercised by the Milestones 1 through 4 platform and canonical-vault work, including deterministic validation, six ordered promotions, two governed amendments, hammer tests, and Result 062 closure. Remaining open questions are future refinements rather than blockers to the implemented first-slice baseline.

## Related files

- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/operational-postgres-authority.md`
