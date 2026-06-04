# Research Summary 001 — Hermes Agent Boundaries

Status: evidence summary
Source: Claude research report pasted by user
Date captured: 2026-06-03

## Purpose

Summarize the Hermes capability and boundary research into Kaizen-relevant claims without promoting those claims directly into doctrine.

This file is evidence. It is not yet accepted Kaizen policy.

## High-value findings

1. Hermes should be treated as a powerful configurable runtime, not as a narrow Obsidian vault assistant by default.
2. Prompt rules are not enforcement. Boundaries must be enforced through deterministic code, filesystem permissions, staging folders, validation scripts, and human approval gates.
3. Hermes should have read access broadly, but write access only to a staging/draft area until Kaizen has hardened guardrails.
4. Hermes should never commit, push, delete, move canonical notes, approve specs, change decisions, or mark work implementation-ready.
5. Kaizen should route by reversibility and authority, not just difficulty.
6. Deterministic validation should be mandatory before accepting agent-created notes.
7. Search-before-create and diff-before-write should become core Kaizen workflow rules.
8. Logs should be append-only; indexes should be regenerated from scans rather than hand-mutated.
9. Hermes skills and memory should not be allowed to mutate production policy without review.
10. The safest operating model is a staged workflow: Hermes drafts, validation checks, frontier review where needed, human approval, then promotion.

## Candidate Kaizen rules extracted from the report

These are proposed candidates, not accepted doctrine yet.

### Candidate rule: two-zone write model

Kaizen should distinguish between:

- agent-writable staging zones
- human-governed canonical zones

Hermes may create or modify drafts in staging. Canonical folders are promoted into through a separate approval/promotion step.

### Candidate rule: deterministic validation gate

Before any agent-created note becomes canonical, it must pass deterministic checks:

- YAML/frontmatter parse
- required field check
- status enum validation
- folder placement validation
- duplicate title or near-duplicate detection
- broken link detection
- source reference check where applicable

### Candidate rule: Hermes task authority

Hermes may own:

- search/read
- raw source draft creation
- source summary drafts
- claim extraction drafts with citations
- append-only logs
- regenerated indexes
- lint/check reports
- task packet drafts from approved specs

Hermes must not own:

- specs
- decisions
- source-of-truth rules
- audit pass/fail judgments
- implementation-ready status changes
- deletes/moves
- commits/pushes
- source repo modification
- external publishing

### Candidate rule: diff-before-write

Any change to an existing file must produce a unified diff and stop for review unless operating inside a disposable staging area.

### Candidate rule: search-before-create

Before creating a new note, Hermes must search for existing related notes and report what it found.

## Claims needing verification

The report includes several specific factual claims that should be checked against primary sources before becoming doctrine:

- Hermes file tool names and exact behaviors
- Hermes approval modes and whether they apply to file tools
- Hermes toolset restriction commands
- Hermes Desktop vs CLI/core behavior
- Hermes support for MCP/tool integration
- Hermes security documentation wording
- cited filesystem MCP CVEs and patched versions
- cited Replit incident details
- cited summarization-bias study details
- cited AGENTS.md study details

## Immediate design implications

The report strongly supports changing the future Kaizen standard to include:

1. A staging/canonical write-zone split.
2. A deterministic validation framework as a first-class project artifact.
3. A Hermes permission matrix.
4. A human promotion gate.
5. A short Hermes skill file that references scripts instead of trying to encode every rule in prose.
6. A rule that research findings do not become doctrine until reviewed and promoted through a decision.

## Recommended next repo artifacts

Create the following next:

1. `04-design-decisions/0001-two-zone-agent-write-model.md`
2. `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
3. `07-hermes/hermes-permission-matrix.md`
4. `07-hermes/hermes-write-access-preconditions.md`
5. `05-specs/kaizen-validation-gate-spec.md`

## Steward note

This report is unusually actionable. The highest-priority takeaway is not Hermes-specific: Kaizen must not rely on model obedience for safety. The system should make the safe path easy and the dangerous path unavailable.
