# Packet 020B — Claude Review Prompt

Status: review prompt
Date: 2026-06-22
Packet: 020B
Milestone: 14

## Purpose

Use this prompt to have Claude review Packet 020B before M14 Stage A begins.

Stage A generation must wait until this review is returned and dispositioned.

## Prompt

```markdown
# Kaizen M14 / Packet 020B Review — Boundary and Collision Pre-Registration

You are reviewing the Phase 0 pre-registration for Kaizen Milestone 14.

Kaizen is a governed project-intelligence system. It does not code the downstream project itself. Kaizen produces governed implementation docs that a connected coding LLM / agent may later follow under a separately approved packet.

## Current status

Milestone 13 is closed.

Packet 020A selected the M14 workload:

```text
Neon Ronin parent project idea-to-implementation-docs proof,
with SearchClarity child-project dependency and repo-reconciliation test,
and Observatory / IMI shared-capability boundary clarification.
```

Packet 020B pre-registers the scoring key before Stage A generation begins.

## Files to review

Read:

```text
03-research-results/345-packet-020a-m14-workload-selection-and-boundary-registration.md
03-research-results/346-packet-020b-phase-0-boundary-and-collision-pre-registration.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
```

Optional context if needed:

```text
ROADMAP_V0.4.md
03-research-results/342-milestone-13-owner-closure-acceptance.md
```

## Owner clarification to preserve

The owner clarified:

```text
Kaizen will not build anything.
Kaizen turns ideas into implementation docs.
The docs are handed to a connected LLM / coding agent to follow and code from.
Neon Ronin and SearchClarity were started before Kaizen and already have repo reality.
```

The owner also clarified the Observatory boundary:

```text
The intended Observatory / IMI capability is the same Kaizen-planned Observatory concept.
It should be usable across multiple SERP / SEO / LLM citation / marketplace intelligence projects.
The recurring Observatory mentions across Kaizen, Neon Ronin, and SearchClarity are a boundary/ownership design knot, not proof that three separate incompatible systems should exist.
```

## Review task

Review Packet 020B for readiness before Stage A begins.

Answer these questions:

### 1. Is 020B strict enough?

Does it prevent:

```text
project identity confusion;
parent/child inversion;
Observatory / IMI fragmentation or premature implementation;
reserved-vs-built repo hallucination;
private/git-ignored data fabrication;
Kaizen accidentally acting as the coding agent;
Stage B execution before separate approval.
```

### 2. Is the scoring rubric sufficient?

Review the scoring categories and pass/fail gates.

Recommend changes if any category is missing or too vague.

### 3. Is the Observatory / IMI boundary represented correctly?

Important: do not treat the repeated Observatory references as three unrelated final systems by default.

Evaluate whether 020B correctly represents:

```text
Kaizen Observatory / IMI as the planned governed shared intelligence capability;
Neon Ronin as a possible consumer/operationalizer;
SearchClarity as a child business lane that may capture or benefit from SEO/SERP/LLM-citation signals;
the question of whether Observatory / IMI should eventually become its own governed project.
```

### 4. Are Stage A and Stage B separated correctly?

Stage A is idea-to-implementation-docs and repo reconciliation.

Stage B is one tiny bounded real implementation-return cycle done by a connected coding agent, not by Kaizen.

Does 020B preserve this correctly?

### 5. What must be fixed before Stage A?

List required fixes, should-fixes, and optional improvements.

### 6. What should the next packet be?

Recommend the exact next M14 packet after review disposition.

Candidate:

```text
020C — Stage A Idea-Only Intake and Generated Docs Protocol
```

or suggest a better packet.

## Output format

Use this structure:

```markdown
# Packet 020B Review Report

## Verdict
PASS / PASS WITH REQUIRED FIXES / FAIL

## Executive summary

## Required fixes

## Should-fixes

## Optional improvements

## Observatory / IMI boundary assessment

## Stage A / Stage B separation assessment

## Scoring rubric assessment

## Recommended next packet

## Final recommendation
```

Be strict. Do not rubber-stamp. The purpose is to catch drift before Stage A begins.
```

## Non-authorization

This prompt does not authorize:

```text
Stage A generation;
Stage B execution;
repo mutation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
Git push.
```
