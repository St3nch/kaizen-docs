# Spec - Kaizen Field Registry

Status: draft
Date: 2026-06-04

## Purpose

Define the candidate governed fields used across Kaizen notes. This registry prevents ad hoc metadata growth and frontmatter obesity.

Fields are not required on every note. Each note type selects a small required and optional subset.

## Universal candidate fields

| Field | Purpose | Candidate rule |
|---|---|---|
| `id` | stable human-readable note identifier | required; immutable; prefixed by type |
| `uuid` | immutable machine join key | proposed required; validate before adoption |
| `type` | note type registry value | required enum |
| `status` | work/content lifecycle | required enum by type |
| `review_status` | review/promotion lifecycle | required on agent-touchable/governed notes |
| `authority` | authority level | required on claims, decisions, specs, audits/domain doctrine candidates |
| `project` | owning project slug | required |
| `domain` | governed knowledge domain | likely required; taxonomy must remain shallow |
| `summary` | one-sentence description | required; plain text; length limited |
| `created` | creation date/time | required; immutable |
| `updated` | last meaningful update | required; preferably tool-maintained |

## Conditional candidate fields

| Field | Applies to | Rule direction |
|---|---|---|
| `pipeline_stage` | project-state and pipeline artifacts | controlled enum; do not duplicate type blindly |
| `source_docs` | summaries, claims, opportunities | list of stable IDs/paths only |
| `related_claims` | claims, decisions, specs, audits | stable ID list |
| `related_decisions` | claims, specs, audits | stable ID list |
| `related_specs` | specs, audits, task packets | stable ID list |
| `observatory_result_ids` | insights, claims, opportunities | stable result IDs only; no raw rows |
| `supersedes` | claims, decisions, specs | governance-sensitive; requires rationale |
| `superseded_by` | claims, decisions, specs | governance-sensitive; target must exist |
| `conflict_with` | claims and insights | explicit unresolved conflict links |
| `approval_event_id` | promoted governed notes | optional until promotion workflow exists |
| `validation_status` | staging notes | pending/passed/failed |
| `confidence` | claims, insights, opportunities | controlled enum plus body caveats |
| `agent_allowed` | protected/governed notes | default false unless policy grants bounded edit |
| `agent` | staged agent-created notes | provenance only |
| `model` | staged agent-created notes | provenance only |
| `session` | staged agent-created notes | provenance only |

## Deferred or rejected fields

| Field | Direction | Reason |
|---|---|---|
| `phase` | remove/defer | overlaps status/pipeline stage |
| `subdomain` | defer | premature taxonomy until needed |
| `informs_projects` | defer | cross-project relationship can be introduced after real use |
| nested objects | reject | poor Obsidian property support and harder validation |

## Candidate state axes

These axes must remain orthogonal.

### Work status

Draft candidate values:

```text
active | blocked | complete | archived
```

### Review status

Draft candidate values:

```text
draft | staged | in-review | reviewed | rejected
```

### Authority

Draft candidate values:

```text
none | proposed | accepted | doctrine
```

Exact values require a dedicated lifecycle decision before implementation.

## Registry rules

1. Unknown fields produce warnings initially and errors after stabilization.
2. Unknown enum values are errors.
3. No nested YAML in canonical notes.
4. IDs and UUIDs never change after creation.
5. Supersedence fields require explicit rationale and review.
6. Agent provenance fields never grant authority.
7. Private/customer data is forbidden in canonical frontmatter.
8. New fields require registry update, validation update, and review.

## Open questions

- Is dual ID necessary for every note or only governed/cross-system notes?
- Should `domain` be required on project-local operational planning notes?
- Should summary remain in frontmatter, body, or both?
- Which exact date-time format is canonical?
- What is the final lifecycle state machine?

## Related files

- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/postgres-observatory-authority.md`
