# Research Result 007 - Kaizen Vision, Architecture, and Research-Gap Audit

Status: active evidence summary
Date: 2026-06-05
Source: Claude audit supplied by the project owner after full-repository review

## Purpose

Preserve the high-value conclusions from Claude's independent audit of the Kaizen vision, architecture, research gaps, and roadmap readiness.

This file is evidence and steward reconciliation. It does not promote Claude's recommendations directly into doctrine.

## Executive finding

Claude concluded that the repository now captures the intended Kaizen vision well in `01-project-standard/kaizen-vision-and-architecture-alignment.md`, but the surrounding accepted decisions, entrypoint language, roadmap, and draft specifications have not fully caught up.

The strongest aligned areas are:

- human authority and agent boundaries
- canonical Markdown and Obsidian portability
- staging and promotion
- search-before-create and diff-before-write
- typed access to Postgres and Qdrant
- hammer tests as a hard gate
- document-contract reconciliation
- research remaining evidence until reviewed

The largest remaining gaps are:

1. the broader Operational Postgres database is still conflated with the Observatory domain;
2. Kaizen's feedback loops are explicit in the vision but weak or absent in operational contracts;
3. MCP and Hermes safety assumptions require hands-on verification;
4. SERP, LLM-citation, external-source, licensing, retrieval, and recovery research must precede affected implementation work;
5. the implementation roadmap is not yet trustworthy.

## Verified live-repository findings

The project steward independently verified these findings after receiving Claude's report:

- The working tree is intentionally dirty and still based on commit `08233d52c94f2f056410ab94beb3ef24c8c15e70`.
- Corrupted control characters and truncated Markdown tokens existed in `05-specs/kaizen-field-registry.md` and `05-specs/kaizen-validation-gate-spec.md`; they were introduced during recent PowerShell edits and require repair before commit.
- `ROADMAP.md` incorrectly marked both Phase 2 and Phase 3 active; Phase 3 must remain ready and dependent on Phase 2.
- `00-entrypoint/LLM_START_HERE.md`, accepted Decision 0004, and `05-specs/postgres-observatory-authority.md` still use the broader term `Postgres Observatory` where the intended vision distinguishes an Operational Postgres database from an Observatory domain.

## Adopt

### Vision diagnosis

Adopt Claude's central diagnosis:

> Kaizen's vision document now describes the intended living intelligence system, but the contract and authority layers remain unevenly aligned.

### Operational Postgres correction

Adopt the need to distinguish:

```text
Operational Postgres database
└── Observatory domain
```

The following domain names remain proposed planning boundaries rather than accepted permanent schemas:

```text
observatory
ingestion
automation
governance
audit
reference
```

### Feedback-loop gap

Adopt the finding that current guidance and contracts must represent:

```text
task packet
-> implementation
-> completion report and test evidence
-> deviations, failures, and discoveries
-> reviewed updates to project intelligence
```

Operational, market, and Kaizen self-improvement loops also require eventual planning ownership.

### Research sequence

Adopt four research batches:

1. agent access and write safety;
2. external intelligence providers and source rights;
3. storage, retrieval, backup, and recovery constraints;
4. knowledge quality, freshness, trust, and context assembly.

### Implementation-roadmap gate

Adopt the recommendation not to begin or accept the implementation roadmap until the vision, required research, v0.2 standard, and implementation-planning inputs are accepted.

## Modify

### Correct accepted doctrine through a new decision

Modify Claude's suggestion to directly amend Decision 0004.

The steward should draft a new decision establishing the broader Operational Postgres boundary and its relationship to the Observatory. After human acceptance, the affected portion of Decision 0004 may be explicitly amended or superseded.

Do not silently edit accepted doctrine.

### Hermes read-only test timing

Modify Claude's recommendation to run the Hermes read-only test immediately.

First:

1. repair corrupted files;
2. align the entrypoint and roadmap;
3. preserve and reconcile the audit;
4. create a clean Git checkpoint.

Then run the read-only test against internally consistent documents.

### Postgres specification split

Modify the recommendation to split the Postgres authority spec immediately.

First accept the database-boundary decision. Then decide whether the correct specification shape is:

```text
operational-postgres-authority.md
observatory-domain-authority.md
```

or another evidence-backed structure.

### Feedback artifact shape

Modify any implication that a new note type is automatically required.

The Decision 0008 dry run should determine whether implementation feedback belongs in:

- the task packet;
- a separate implementation-result artifact;
- current-state updates;
- generated claims or decision proposals;
- or a governed combination.

## Reject

Reject:

- starting the implementation roadmap now;
- treating the current `postgres-observatory-authority.md` ownership list as aligned with the broader vision;
- adopting research-summary lifecycle enums that conflict with Decision 0007;
- assuming MCP or Hermes instructions alone provide security boundaries;
- designing exact database tables, Qdrant collections, dashboards, or model routing before their prerequisite research and decisions.

## Defer

Defer until demonstrated need or prerequisite evidence exists:

- GraphRAG;
- exact Postgres tables and physical schemas;
- exact Qdrant collection and chunk layouts;
- exact MCP server selection;
- exact model router;
- exact source-trust scores;
- exact freshness fields;
- exact monitoring cadence;
- portfolio prioritization automation;
- cross-project wiki implementation details;
- Kaizen self-improvement telemetry schemas.

## Research Batch A - Agent access and write safety

Priority: P0/P1

Questions:

- What security and scoping properties do candidate MCP filesystem and Obsidian integrations actually provide?
- Can access be confined against traversal, symlink, and Windows junction escape?
- Can Hermes retain read/search while write, patch, terminal, and skill-management capabilities are denied or wrapped?
- What atomic or recoverable promotion strategy is correct on Windows?
- How should durable JSONL promotion events be appended and recovered after failure?

Expected result:

- verified constraints;
- native-control versus wrapper requirements;
- runnable boundary tests;
- go/no-go conditions for Hermes staging writes.

## Research Batch B - External intelligence providers and source rights

Priority: P1

Questions:

- What do DataForSEO and credible alternatives provide for SERP, ranking, visibility, cost, and provenance?
- What LLM citation and retrievability measurement capabilities are mature enough to use?
- What storage, citation, copyright, licensing, and retention constraints apply to scientific papers, patents, news, documentation, datasets, and transcripts?

Expected result:

- provider capability and cost matrix;
- source-class rights and storage matrix;
- migrate-versus-reference guidance;
- freshness expectations.

## Research Batch C - Storage, retrieval, backup, and recovery

Priority: P1

Questions:

- What operational workloads and authority boundaries should the broader Postgres database support before table design?
- What Qdrant filtering, rebuildability, project isolation, authority, supersedence, and privacy constraints are required?
- What backup, restore, versioning, and recovery posture is required before automated writes?

Expected result:

- database workload and authority inputs;
- Qdrant constraints rather than final layout;
- backup and recovery requirements.

## Research Batch D - Knowledge quality and context assembly

Priority: P2

Questions:

- How should source trust and evidentiary roles be represented?
- How should freshness classes and recheck triggers differ by knowledge type?
- How should governed context packs filter by project, authority, supersedence, privacy, and freshness?
- How should project resumption avoid dumping the entire vault into an agent context?

Expected result:

- options and recommended directions;
- explicit implementation deferrals;
- inputs for later field and context-pack specifications.

## Roadmap consequences

The roadmap should maintain this sequence:

```text
vision alignment
-> audit reconciliation
-> database-boundary decision
-> required research
-> contract review
-> Decision 0008 dry run
-> v0.2 standard
-> standard audit
-> implementation-planning inputs
-> implementation roadmap
```

Only Phase 2 is active. Phase 3 is ready and depends on Phase 2 completion.

## Files affected by future reconciliation

- `00-entrypoint/LLM_START_HERE.md`
- `README.md`
- `ROADMAP.md`
- `04-design-decisions/0004-system-of-record-boundaries.md`
- a future Operational Postgres and Observatory boundary decision
- `05-specs/postgres-observatory-authority.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-note-type-registry.md`
- `07-hermes/kaizen-hermes-skill-draft.md`
- `03-research-results/004-markdown-qdrant-postgres-architecture-claude-summary.md`

## Immediate next action

Repair and validate the current planning batch, create a clean Git checkpoint, then draft the Operational Postgres and Observatory boundary decision.
