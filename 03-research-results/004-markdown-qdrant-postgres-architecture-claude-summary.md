# Research Summary 004 - Markdown, Qdrant, and Postgres Architecture

Status: evidence summary
Source: Claude architecture research report provided by user
Date captured: 2026-06-04

## Purpose

Capture the independent architecture research for Kaizen's Markdown, Qdrant, Postgres Observatory, and agent-tool boundaries.

This file is evidence. It is not doctrine by itself.

## Core architecture finding

```text
Markdown / Obsidian = canonical project intelligence
Qdrant = rebuildable semantic index over Markdown
Postgres = operational source of truth for observations, jobs, runs, costs, and logs
Hermes / agents = constrained clients through approved typed tools
```

Important correction: Qdrant is rebuildable from Markdown. Postgres is not merely a derivative of Markdown; it owns structured operational records that require backup, retention, and recovery.

## High-value findings

1. Frontmatter should be a thin typed index, not a database.
2. Stable prefixed IDs plus immutable UUIDs are useful for human references and machine joins.
3. Note state should be split into work status, review status, and authority.
4. Qdrant should chunk notes by stable headings and inherit selected note metadata.
5. Qdrant payload filters should be narrow, explicit, indexed, and free of private data.
6. Embedding and chunking versions must be recorded; model migrations should use blue-green re-indexing.
7. Postgres should store observations and machinery, never canonical project narrative.
8. A stable Observatory result object should be the API boundary between structured observations and Markdown insight notes.
9. Hermes should have no arbitrary SQL or direct Qdrant-mutation tools.
10. Agent-created notes should be staged, validated, and human-promoted.

## Candidate registries

Kaizen likely needs governed registries for:

- note types
- fields
- statuses
- review statuses
- authority levels
- pipeline stages
- Qdrant payload fields
- agent tool action classes

## Candidate note state model

```yaml
status: active | blocked | complete | archived
review_status: draft | staged | in-review | reviewed | rejected
authority: none | proposed | accepted | doctrine
```

Exact values remain draft and must be reconciled before implementation.

## Candidate retrieval direction

- Chunk by H2/H3 headings.
- Keep short notes whole.
- Store original text for display and contextualized text for embedding if later justified.
- Filter by project, domain, type, authority, review status, supersession state, and embedding version.
- Exclude navigation/registry notes from semantic indexing.
- Rebuild deterministically from Markdown.

## Candidate Observatory direction

Postgres may eventually own:

- source and ingestion jobs
- crawl metadata
- SERP/ranking/citation observations
- scheduled task runs
- API requests and costs
- agent actions
- stable derived result summaries

Markdown should reference stable result IDs and contain interpretation, claims, decisions, specs, and handoffs - not raw observation rows.

## Open questions

- Whether the dual ID scheme is worth the complexity for every note type.
- Exact status/review/authority enum values.
- Whether Observatory results should be append-only rows or versioned projections.
- Local versus hosted Qdrant and embedding models.
- Where raw crawl payloads live and how long they are retained.
- Whether promotions belong in Markdown, Postgres, or both.

## Related files

- `04-design-decisions/0004-system-of-record-boundaries.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
