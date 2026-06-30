# Packet 025A — Post-M16 Roadmap Reconciliation and Next-Gate Selection

Status: complete — docs-only reconciliation
Date: 2026-06-30
Milestone: post-M16 reconciliation

## Purpose

Reconcile the roadmap after Milestone 16 closure and choose the next active gate without accidental milestone renumbering.

025A is docs-only. It does not authorize implementation.

## Inputs

```text
03-research-results/427-packet-024r-milestone-16-closure-record.md
03-research-results/428-packet-024s-milestone-16-closure-handoff-correction.md
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
```

## Verified roadmap facts

`ROADMAP_V0.4.md` already defines:

```text
Milestone 17 — Hermes constrained-clerk integration
Milestone 18 — Progressive hybrid UX / Tauri operator shell
```

Therefore, the retrieval expansion follow-up must not be assigned to Milestone 17 unless the roadmap is explicitly amended.

## Reconciliation decision

Milestone 16 remains closed.

Existing roadmap Milestone 17 remains:

```text
Hermes constrained-clerk integration
```

The next active gate should be:

```text
Retrieval Expansion Options and Gate Contract
```

The retrieval expansion gate is not assigned a milestone number by 025A.

## Why this is the right next move

Milestone 16 proved the Qdrant retrieval path in a bounded way.

The next retrieval work is an expansion decision, not an automatic implementation step.

The project needs to choose between expansion lanes before writing more platform code.

## Candidate retrieval expansion lanes

The next retrieval gate should compare:

```text
larger docs corpus;
vault pilot corpus;
stronger retrieval/vector baseline;
provider-backed embedding baseline;
agent-facing retrieval contract;
production gateway packaging.
```

## Recommended next packet

Recommended next packet:

```text
025B — Retrieval Expansion Options and Gate Contract
```

025B should be docs-only unless separately authorized.

025B should decide which retrieval expansion lane comes first and what remains unauthorized.

## Roadmap note

025A does not rewrite the milestone sequence.

If the owner wants retrieval expansion to become a numbered milestone, that should happen through a separate roadmap amendment packet.

## Non-authorization

025A does not authorize:

```text
platform mutation;
vault mutation;
staging mutation;
database mutation;
roadmap milestone renumbering;
retrieval implementation;
broader docs indexing;
vault indexing;
agent-facing tools;
production gateway;
Hermes implementation;
Tauri implementation.
```
