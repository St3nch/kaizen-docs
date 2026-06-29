# M15 Definition / Spec Owner Acceptance

Status: owner accepted
Date: 2026-06-28
Accepted spec: `05-specs/milestone-15-dynamic-obsidian-experience-and-typed-read-model.md`
Accepted spec commit: `a8d9f1230b2ceb7997157188e78fcdeda3236b88`
Audit: `03-research-results/385-m15-definition-spec-audit.md`
Audit result: pass with no blocking defects

## Owner acceptance

The owner explicitly accepted the M15 definition/spec draft with first implementation packet separately bounded:

```text
I accept the M15 definition/spec draft at spec commit a8d9f1230b2ceb7997157188e78fcdeda3236b88, with first implementation packet separately bounded.
```

## Acceptance meaning

This acceptance authorizes Milestone 15 to proceed from definition/spec drafting into separately bounded first implementation-packet planning.

The accepted M15 direction is:

```text
Obsidian = immediate human cockpit over canonical Markdown
Bases / Dataview = derived human read models only
Kaizen parser = client-neutral typed read-model projector
Context packs = governed bundles assembled from canonical notes and typed projections
Tauri = later operator shell that consumes the typed read model
```

M15 prepares the read-model and workflow contract that later Tauri can consume. M15 does not implement Tauri.

## Boundaries preserved

This owner acceptance does not authorize:

```text
implementation without a separately accepted first implementation packet
platform mutation
vault mutation
staging mutation
database mutation
production database mutation
Tauri implementation
Qdrant implementation
Hermes write authority
Obsidian MCP write authority
Obsidian REST write authority
provider purchase
customer-data reuse
logged-in scraping
Observatory / IMI implementation
commercial operations
production deployment
full Neon Ronin implementation
full SearchClarity implementation
backup deletion
restore work
```

## Next authorized planning gate

The next planning step is to draft the first M15 implementation packet:

```text
023D — M15 Note Schema and Parser Direction
```

Recommended scope for that first implementation packet:

```text
Define exact M15 note types, frontmatter fields, allowed enum values, required body headings, validation rules, pilot corpus / fixture target, generated-snapshot posture, and parser language direction.
```

The first implementation packet must still be separately reviewed and accepted before implementation begins.

## Disposition

M15 definition/spec is owner accepted.

Proceed to first implementation-packet planning under the accepted M15 boundaries.
