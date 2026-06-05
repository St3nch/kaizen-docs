# Research Prompt 002 - Kaizen Document Contract Standards

Status: ready to run
Date: 2026-06-04
Recommended model: Claude Opus

## Launch instruction

Clone or read the Kaizen docs repository first. Do not answer from memory.

Repository:

```text
https://github.com/St3nch/kaizen-docs
```

Start with:

```text
00-entrypoint/LLM_START_HERE.md
01-project-standard/kaizen-project-standard.md
01-project-standard/baseline-v0.2-reconciliation-map.md
01-project-standard/standard-revision-plan.md
04-design-decisions/0001-two-zone-agent-write-model.md
04-design-decisions/0002-search-before-create-and-diff-before-write.md
04-design-decisions/0003-raw-markdown-is-canonical.md
04-design-decisions/0004-system-of-record-boundaries.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
04-design-decisions/0007-foundation-resolution-for-v0.2.md
04-design-decisions/0008-v0.2-operating-conventions.md
05-specs/kaizen-field-registry.md
05-specs/kaizen-note-type-registry.md
05-specs/kaizen-validation-gate-spec.md
05-specs/staging-and-promotion-workflow.md
05-specs/kaizen-hammer-test-strategy.md
```

If repository access is unavailable, stop and say so rather than producing an unsourced generic answer.

## Context

Kaizen is a portable Markdown-first project intelligence system.

Its pipeline moves projects through:

```text
idea capture
-> research
-> source summaries
-> claims
-> specs
-> audit
-> task packet
-> coding agent build
```

The accepted foundation already defines:

- raw Markdown plus flat validated YAML as canonical project intelligence
- Obsidian as the current human interface, not an independent source of truth
- one immutable prefixed ULID note ID
- separate status, review, and authority axes
- stable IDs in frontmatter relationships
- relative Markdown body links
- sibling agent staging and human-controlled promotion
- append-only promotion events
- API-only Postgres/Qdrant access
- human-only authority-bearing transitions
- validation and hammer-test gates

Do not re-research Hermes, Obsidian plugins, Qdrant architecture, or Postgres Observatory boundaries in depth. Use the accepted repo decisions as constraints.

## Primary research goal

Research and recommend strong, minimal, agent-readable document contracts for these Kaizen note types:

```text
source-summary
claim
decision
spec
audit
task-packet
```

The main outcome of Kaizen is an audited, implementation-ready task packet that a coding agent can execute without guessing.

The contracts must be:

- readable as raw Markdown
- useful to humans
- deterministic enough for validation
- evidence-linked
- resistant to premature authority
- portable outside Obsidian
- specific enough for coding agents
- not bureaucratically bloated

## Sources to prioritize

Use primary and authoritative sources where possible:

- Architecture Decision Record standards and well-maintained ADR repositories
- RFC and engineering design-document practices from major engineering organizations
- official product and technical specification guidance
- issue/task templates from mature open-source projects
- testing and acceptance-criteria guidance
- agent/coding-agent task specification research or official documentation
- human-in-the-loop and approval-gate guidance
- Veda docs already referenced in Kaizen for governance and hammer patterns

Separate official facts, community practices, recommendations, and assumptions requiring testing.

## Research questions

### 1. Source summaries

Determine the minimum durable contract for a source summary.

Answer:

- What must be captured to preserve provenance and meaning?
- How should direct quotations, paraphrases, and interpretations be separated?
- How should uncertainty and source limitations appear?
- Should one source summary represent one source only?
- What belongs in frontmatter versus body?
- What makes a source summary ready to support a claim?
- What should Hermes be allowed to draft?

Evaluate the current candidate sections:

```text
Summary
Key points
Provenance
Caveats
Project relevance
```

### 2. Claims

Define a claim as a discrete, testable assertion rather than a loose note.

Answer:

- What makes a statement a valid Kaizen claim?
- Must every claim be falsifiable or merely evidence-checkable?
- How should supporting and conflicting evidence be represented?
- How should confidence be assigned?
- How should claims be accepted, rejected, superseded, or revisited?
- Can one claim contain multiple assertions?
- What makes a claim safe to use in a decision or spec?

Evaluate the current candidate sections:

```text
Statement
Evidence
Confidence and caveats
Sources
Conflicts
```

### 3. Decisions

Research ADR, RFC, and engineering decision-record patterns.

Answer:

- What information must every decision record include?
- How should alternatives and tradeoffs be documented?
- How should scope and authority be stated?
- Should accepted decisions be treated as immutable and superseded rather than edited?
- How should amendments differ from supersedence?
- What evidence must support an accepted decision?
- What decisions are too small to deserve a formal record?

Evaluate the current candidate sections:

```text
Context
Decision
Alternatives
Consequences
Evidence
Supersedence rationale
```

### 4. Specs

Research technical design docs, implementation specs, RFCs, and agent-readable build plans.

Answer:

- What sections make a spec implementation-capable without turning it into a giant essay?
- How should goals, scope, non-goals, requirements, constraints, interfaces, failure behavior, and acceptance criteria be separated?
- When are diagrams or examples necessary?
- How should unresolved questions block acceptance?
- How should specs reference accepted decisions and source-repo invariants?
- What makes a spec accepted but not yet task-packet-ready?
- What makes a spec too vague for a coding agent?

Evaluate the current candidate sections:

```text
Goal
Context
Scope
Non-goals
Requirements
Constraints
Acceptance criteria
Open questions
```

### 5. Audits

Define the audit as a real review gate, not checkbox theater.

Answer:

- What exactly should an audit inspect?
- Should audits review claims, decisions, specs, task packets, or all of them?
- How should findings be classified by severity?
- How should contradictions, missing evidence, ambiguity, and unverifiable acceptance criteria be recorded?
- What verdict vocabulary should Kaizen use?
- Who may draft findings and who may issue the verdict?
- Should the audit be immutable after verdict?
- When is re-audit required?

Evaluate the current candidate sections:

```text
Scope
Evidence reviewed
Findings
Contradictions and gaps
Recommendation
Human verdict
```

### 6. Task packets

Research issue templates, agent task plans, coding-agent handoffs, and objective acceptance criteria.

Answer:

- What must a coding agent know before starting?
- How should files/repositories to read first be represented portably?
- How should the task packet prevent scope creep?
- How should output location, allowed changes, forbidden changes, tests, migration needs, rollback, and documentation requirements be specified?
- What makes acceptance criteria objectively verifiable?
- When should one task packet be split into several?
- How should assumptions and open questions be handled?
- What exact conditions make a task packet implementation-ready?

Evaluate the current candidate sections:

```text
Objective
Read first
Context pack
Constraints
Output location
Acceptance criteria
Do not touch
```

### 7. Contract relationships

Recommend the required dependency chain between document types.

Evaluate and improve:

```text
source-summary -> claim -> decision -> spec -> audit -> task-packet
```

Answer:

- Which links are mandatory versus optional?
- Can specs proceed directly from accepted evidence without a formal decision?
- Must every task packet reference exactly one spec?
- Can one audit cover multiple specs or packets?
- What must be accepted before the next artifact can be approved?
- What should block promotion?

### 8. Frontmatter by type

Using the accepted universal fields, recommend only the type-specific additions genuinely required for each document type.

Accepted universal fields:

```yaml
id:
type:
status:
project:
summary:
created:
updated:
```

Accepted lifecycle enums:

```yaml
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
```

Candidate conditional fields include:

```yaml
review_status:
authority:
confidence:
source_docs:
source_urls:
related_claims:
related_decisions:
related_specs:
supersedes:
superseded_by:
conflict_with:
approval_event_id:
agent:
model:
session:
```

Do not recommend fields merely because they could be useful.

### 9. Validation rules

Recommend deterministic validation rules for each contract.

Cover:

- required fields
- required headings
- evidence/reference integrity
- permitted lifecycle combinations
- open-question blockers
- acceptance-criteria quality checks
- supersedence rules
- agent authority restrictions
- stale approval after content changes
- task-packet readiness checks

Clearly separate what can be validated mechanically from what requires human/frontier-model judgment.

### 10. Failure modes

Identify and mitigate:

- summaries that distort sources
- multi-assertion claims
- unsupported accepted claims
- decisions hidden inside specs
- decisions without alternatives or consequences
- giant unfocused specs
- unverifiable acceptance criteria
- audit checkbox theater
- agents issuing their own verdicts
- task packets that permit scope creep
- stale specs or packets after source-repo changes
- ambiguous source-of-truth references
- excessive bureaucracy that slows small projects

## Required output format

Return a practical report with these sections:

1. Executive summary - 10 document-contract rules Kaizen should adopt
2. Recommended source-summary contract
3. Recommended claim contract
4. Recommended decision contract
5. Recommended spec contract
6. Recommended audit contract
7. Recommended task-packet contract
8. Required relationships and promotion dependencies
9. Recommended frontmatter by type
10. Required body sections by type
11. Mechanical validation rules
12. Human/frontier-review requirements
13. Implementation-ready task-packet checklist
14. Failure modes and mitigations
15. Direct changes to current Kaizen registries/specs
16. Open questions requiring hands-on testing or human decision
17. Source list with links

## Required style

Be skeptical, precise, and implementation-focused.

Do not produce generic writing advice.

Do not add fields or sections without showing what failure they prevent.

Prefer minimal contracts with strong semantics over long templates.

Be blunt about:

- what is required
- what is optional
- what should be deferred
- what agents may draft
- what humans must approve
- what validation can and cannot prove

The target is not beautiful documentation. The target is reliable project intelligence and task packets coding agents can execute without guessing.

Do not edit, commit, or push repository files. Return the research report only.
