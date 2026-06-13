---
id: kz-tp-01KTZ6DOCSAUTHREPAIR013A
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-13T23:00:00Z
updated: 2026-06-13T23:00:00Z
review_status: pending
authority: proposed
summary: "Repair active documentation and planning-authority drift before Milestone 9 readiness verification."
---

# Task Packet 013A - Documentation and Authority Repair

## Purpose

Repair active Kaizen reading surfaces and controlled-pilot authority labels so fresh contexts encounter the accepted Milestone 8 closure state and the correct next planning gate.

This packet is documentation-only.

It does not authorize Milestone 9 execution, platform code changes, canonical mutation, connector or lifecycle work, backup execution, or deferred-system implementation.

## Evidence basis

This packet implements only the documentation repair established in:

```text
03-research-results/162-milestone-8-owner-closure-acceptance.md
03-research-results/166-independent-milestones-6-8-audit-owner-acceptance.md
03-research-results/167-roadmap-lineage-and-corrective-sequence-reconciliation.md
ROADMAP_V0.3.md
```

Relevant accepted planning evidence:

```text
Result 102 controlled-pilot accepted SHA-256:
f018415cedc881c642e5eb8981b50d6c908933b6ba980a43d5e1d542b71df94c

Result 162 Milestone 8 owner closure acceptance:
accepted

current Roadmap v0.3 SHA-256:
3a44c60a71b268f1b2087a71e763f7403ce3cc2131170de9b26a0af4dc858cf9
```

## Repository scope

```text
C:\dev\kaizen-docs only
```

No other repository may change.

## Allowed changed paths

```text
00-entrypoint/LLM_START_HERE.md
README.md
05-specs/controlled-implementation-return-pilot.md
03-research-results/<Packet-013A implementation return>
03-research-results/<Packet-013A completion audit>
03-research-results/<Packet-013A owner completion record, only after owner acceptance>
```

The implementation-return and audit filenames must be search-before-create verified and assigned the next available result numbers at execution time.

No roadmap change is required unless implementation discovers a concrete contradiction with Result 167. Any such contradiction must stop the packet and return to planning.

## Current source bindings

```text
00-entrypoint/LLM_START_HERE.md
SHA-256: 41b4d0ecfac0e1a41a1068cb8eae8e69247c4fbd8cac1c6ed763d88e65c8dda5

README.md
SHA-256: 61159cd387d865ba70a252970fbf6f10efc5109d9d11525577f20a74631019a0

05-specs/controlled-implementation-return-pilot.md
SHA-256: f018415cedc881c642e5eb8981b50d6c908933b6ba980a43d5e1d542b71df94c
```

Any mismatch before editing stops execution.

## Required changes

### A. Entrypoint posture repair

Update `00-entrypoint/LLM_START_HERE.md` so its active posture states:

- Project Standard v0.2 remains authoritative;
- Milestones 1-8 are closed;
- Result 162 records Milestone 8 owner closure acceptance;
- current accepted docs, platform, vault, and Go8 checkpoints are referenced accurately;
- Milestone 9 is planned but not authorized for implementation;
- the immediate next work is the corrective sequence beginning with Packet 013A;
- Observatory research is a parallel evidence track, not implementation authority;
- stale Packet 009B Wave 2 and pre-Milestone-7 instructions are removed from active next-gate guidance.

The entrypoint should become more pointer-oriented where practical. Historical detail may remain only when clearly labeled historical and non-actionable.

### B. README posture repair

Update `README.md` so its start-here and current-status sections agree with:

- Result 162;
- current Roadmap v0.3;
- Result 167;
- Milestone 9 implementation remaining unauthorized;
- the current repository roles and checkpoints.

Remove active language saying Milestone 6 or Milestone 7 is the current gate.

### C. Controlled-pilot authority repair

Amend only the authority/posture metadata and explicit planning-authority language required to align the controlled-pilot specification with Result 102.

The amended spec must communicate:

```text
accepted planning direction
not implementation authority
```

Expected frontmatter direction:

```yaml
status: active
review_status: approved
authority: accepted
```

The body sentence stating that the document is merely proposed planning input must be corrected to state that the specification is accepted planning direction but execution still requires its own milestone and task-packet gates.

The accepted behavioral substance must not change:

- Northstar Stockroom scenario;
- sealed oracle;
- oracle-blind implementer;
- eight failure injections;
- two fresh-context resumption checks;
- workload nomination bar;
- exclusions;
- success and failure criteria.

Any substantive behavioral edit stops the packet and requires a new planning amendment.

### D. Historical-record preservation

Do not edit Result 102 solely to normalize its legacy `review_status: accepted` frontmatter.

Result 167 records the vocabulary mismatch. Historical owner-acceptance bytes remain preserved.

## Verification requirements

### Exact-content and navigation checks

Search all active reading surfaces for stale actionable phrases including:

```text
Milestone 6 is closed
Milestone 7 implementation remains unauthorized
Next authorized work: define and audit Milestone 7
Packet 009B Wave 2
require exact owner approval naming Wave 2
Milestone 8: accepted for planning only
Packet 012A
```

Historical files may retain those phrases. They must not remain actionable in:

```text
00-entrypoint/LLM_START_HERE.md
README.md
ROADMAP_V0.3.md
05-specs/controlled-implementation-return-pilot.md
```

### Authority checks

Verify:

- entrypoint and README point to one current active roadmap;
- Result 162 is linked as Milestone 8 closure authority;
- Result 167 is linked as corrective-sequence planning evidence;
- the pilot spec says accepted planning direction and still denies execution authority;
- no old owner approval phrase is presented as currently executable;
- no implementation permission is added.

### Text and Git checks

Required:

- hidden Unicode scan;
- UTF-8 and line-ending report;
- Git whitespace/conflict-marker check;
- exact changed-path verification;
- staged diff inspection;
- one docs commit only after exact review;
- push only to existing `origin/main` after owner implementation approval and exact staged verification.

## Tests and evidence

No platform test execution is required.

The packet should use deterministic docs checks available through Go8:

- exact path hashes;
- bounded text search;
- changed/staged scope verification;
- text-integrity scan;
- Git diff check;
- commit inspection;
- final clean and synchronized status.

If an existing docs-oriented active-roadmap integrity profile exists, it may be run read-only. Do not add or modify test code in this packet.

## Explicit exclusions

- platform, vault, staging, Kaizen MCP, or Go8 changes;
- canonical mutation;
- Milestone 9 fixture, oracle, golden-output, or repository creation;
- operation-status semantic changes;
- runbook implementation outside these active docs;
- backup Generation 3 execution;
- governance-compression doctrine;
- provider, Observatory, Postgres, Qdrant, LangGraph, Hermes, or UI work;
- dependency changes;
- connector or lifecycle mutation;
- cleanup or deletion;
- remote creation;
- vault or platform push.

## Stop conditions

Stop and return to planning when:

- any source hash differs;
- a substantive controlled-pilot behavior change appears necessary;
- active Roadmap v0.3 contradicts Result 167;
- an unexpected changed path exists;
- a historical accepted record would need rewriting;
- implementation authority would be broadened;
- any repository other than `kaizen-docs` would need mutation.

## Completion evidence

Return:

- before/after hashes for all three edited files;
- exact changed paths;
- stale-reference search results before and after;
- authority-label verification;
- hidden-Unicode and Git diff-check results;
- exact commit hash;
- push result;
- final clean synchronized docs status;
- completion audit;
- exact owner completion-acceptance phrase.

## Authorization boundary

This draft and its audit may be created under the current planning authorization.

Implementation requires separate owner approval bound to the exact audited Packet 013A SHA-256.
