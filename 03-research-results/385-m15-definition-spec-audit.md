# M15 Definition / Spec Audit

Status: audit complete — recommended for owner acceptance after explicit owner approval
Date: 2026-06-28
Audited spec: `05-specs/milestone-15-dynamic-obsidian-experience-and-typed-read-model.md`
Spec commit under audit: `a8d9f1230b2ceb7997157188e78fcdeda3236b88`

## Audit question

Does the M15 definition/spec draft provide a safe, clear, and bounded basis for owner acceptance and first implementation-packet planning?

## Summary finding

Pass with no blocking defects.

The M15 draft is acceptable as a milestone definition candidate because it:

```text
preserves raw Markdown as canonical;
keeps Obsidian dynamic views derived and non-authoritative;
keeps Tauri in the roadmap without implementing it early;
defines a parser-backed typed read model as the shared contract;
keeps Bases / Dataview as human read models only;
keeps agent authority bounded;
sets concrete deliverables, exit criteria, proof requirements, stop conditions, and non-authorization boundaries.
```

The draft is ready for owner acceptance if the owner agrees with the milestone scope and the first implementation packet is separately bounded.

## Source basis checked

```text
ROADMAP_V0.4.md
03-research-results/384-m15-research-synthesis-and-tauri-aligned-direction.md
05-specs/milestone-15-dynamic-obsidian-experience-and-typed-read-model.md
Uploaded M15 deep research report
Kaizen raw-Markdown and progressive-interface decisions referenced by the spec
```

## Detailed audit

### 1. Canonical truth boundary

Result: pass.

The draft clearly states that raw Markdown remains canonical and that M15 must not move project truth into:

```text
rendered plugin output
.base files
Dataview results
Canvas layout
generated JSON snapshots
Tauri state
agent memory
```

This matches accepted Kaizen doctrine and the M15 research synthesis.

### 2. Obsidian role

Result: pass.

The draft treats Obsidian as the immediate human cockpit and not as an authority layer.

It allows dashboards, queues, links, bookmarks, searches, and Bases views, but requires those views to remain derived from canonical Markdown.

### 3. Tauri alignment

Result: pass.

The draft preserves Tauri as the likely later operator shell and explicitly says M15 prepares the read-model/workflow contract Tauri later consumes.

It also explicitly blocks Tauri implementation inside M15.

This avoids both drift risks:

```text
Obsidian replacing Tauri;
Tauri arriving later and inventing a second truth layer.
```

### 4. Typed read-model clarity

Result: pass, with one implementation note.

The draft defines the typed read model as a deterministic projection from canonical Markdown and frontmatter into typed records and validation errors.

This is the correct contract for Obsidian, future Tauri, agents, and context-pack assembly.

Implementation note:

```text
The first implementation packet should choose Python-first or TypeScript-first parser direction, or explicitly prove a narrow parser prototype before deciding final language.
```

This is not blocking for spec acceptance.

### 5. Frontmatter and note schema scope

Result: pass.

The draft keeps frontmatter flat and limited to routing/lifecycle facts. It keeps prose, rationale, evidence reasoning, objections, implementation details, and long context in Markdown body sections.

This avoids YAML bloat, invalid nested-property reliance, and plugin-specific serialization.

### 6. Review queue governance

Result: pass.

The draft defines review items as first-class Markdown notes and gives a minimal state machine:

```text
proposed -> ready -> in-review -> approved | rejected | superseded
```

It preserves the authority boundary that agents may prepare review artifacts but humans own approval, rejection, and promotion.

### 7. Resume and current-state safety

Result: pass.

The draft requires current-state and resume surfaces to remain useful in raw Markdown, which is essential for fresh-context continuation and agent readability.

It blocks unreviewed auto-generated current-state truth.

### 8. Context-pack boundary

Result: pass.

The draft defines context packs before Qdrant as governed bundles from canonical notes and typed projections.

It includes explicit excludes:

```text
raw evidence dumps
unrelated projects
superseded material unless requested
speculative drafts without authority
private notes outside boundary
plugin-render-only artifacts
```

This is adequate for M15.

### 9. Premature capability prevention

Result: pass.

The draft explicitly does not authorize:

```text
Tauri implementation
Qdrant implementation
Hermes write authority
Obsidian MCP write authority
Obsidian REST write authority
platform mutation
vault mutation
database mutation
Observatory / IMI work
downstream project implementation
backup deletion
restore work
```

The stop conditions are specific enough to catch drift.

## Non-blocking improvements for first implementation packet

The following should be handled in the first M15 implementation packet, not by expanding the milestone definition now:

1. Choose or prototype parser language direction.
2. Define the exact first pilot corpus or fixture project for M15 proof.
3. Decide whether generated typed snapshots are ephemeral command output, JSON files, or both.
4. Define exact accepted enum values for `type`, `status`, `review_state`, and `authority_level`.
5. Decide whether concrete `.base` files are created in M15 implementation or deferred until after schema/parser proof.

## Recommended first implementation packet direction

The first implementation-bearing packet should be:

```text
023D — M15 Note Schema and Parser Direction
```

Recommended scope:

```text
Define exact M15 note types, frontmatter fields, allowed enum values, required body headings, validation rules, and parser direction.
```

It should not yet create broad Bases views, command-center templates, or context-pack assembly until the schema and parser contract are stable.

## Owner acceptance recommendation

Recommended owner action:

```text
Accept the M15 definition/spec draft at spec commit a8d9f1230b2ceb7997157188e78fcdeda3236b88, with first implementation packet separately bounded.
```

Acceptance should not be interpreted as authorization for implementation, platform mutation, vault mutation, database mutation, Tauri, Qdrant, Hermes, Obsidian MCP writes, downstream project mutation, backup deletion, or restore work.

## Final disposition

M15 definition/spec audit passes.

The milestone can move to owner acceptance and first implementation-packet planning.
