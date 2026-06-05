# Hermes Permission Matrix

Status: draft policy
Date: 2026-06-03
Related research: `03-research-results/001-hermes-agent-boundaries-claude-summary.md`

## Purpose

Define what Hermes may do, may draft, and must never do inside Kaizen.

This document is a draft policy. It does not grant Hermes write access by itself.

## Core principle

Hermes is Kaizen's designated agent clerk and a first-class consumer of the canonical vault. It is a drafting and mechanical-work agent, not an authority-bearing agent.

Kaizen must be designed so Hermes can reliably read, search, trace relationships, and prepare governed drafts. Hermes may produce evidence, drafts, diffs, summaries, validation reports, and proposals. It must not decide what becomes doctrine, issue an audit verdict, or mark work implementation-ready.

MCP root configuration and Hermes profile settings are defense in depth, not the primary write boundary. Any future staging write must pass through the narrow wrapper defined in `05-specs/staging-write-wrapper-and-promotion-recovery.md`, backed by operating-system permissions and hammer-tested final-path confinement.

## Authority levels

| Level | Meaning | Example |
|---|---|---|
| May do | Hermes can perform the action inside its allowed workspace without separate approval | create a raw source draft in staging |
| May draft | Hermes can prepare a proposed artifact or diff, but it requires review before acceptance | task packet draft from an approved spec |
| Must never do | Hermes is not authorized to perform the action; it must escalate | mark a task packet implementation-ready |

## May do

Hermes may do these actions after the relevant preconditions exist:

| Action | Conditions | Required output |
|---|---|---|
| Search/read Kaizen docs or vault files | Read access has been granted to the target root | Search result summary with paths checked against filesystem |
| Create raw source draft | Must search first; only through the approved staging-create wrapper after all write preconditions pass | New staged draft note with required frontmatter and audit evidence |
| Create source-summary draft | Must preserve source; must link source material | Draft summary note |
| Extract claims from source material | Must cite exact source quote or line when possible | Draft claim note or claim section |
| Append log entry | Log must be append-only | New timestamped log entry |
| Regenerate index from scan | Index must be generated from current files, not hand-mutated | Full regenerated index diff |
| Run validation/lint scripts | Scripts must be deterministic | Exit codes and raw validation output |
| Report broken links or missing frontmatter | Read-only check | Report with paths and specific failures |

## May draft

Hermes may draft these, but they are not accepted until reviewed:

| Draft artifact | Preconditions | Review owner |
|---|---|---|
| Task packet draft | Source spec is approved | Frontier model and human |
| Proposed edit to existing note | Existing note was read first; diff is emitted | Human or designated reviewer |
| Proposed move/rename | Move rationale is stated; links are assessed | Human |
| Proposed validation rule | Failure mode is documented | Frontier model and human |
| Proposed index update | Generated from scan | Human or automated validation gate |
| Proposed research summary | Raw research source is preserved | Human/frontier if high-stakes |

## Must never do

Hermes must not perform these actions. These must be blocked by configuration, permissions, scripts, or review gates where possible.

| Forbidden action | Why forbidden |
|---|---|
| Create or modify accepted specs | Specs require design judgment and review |
| Create or modify accepted decisions | Decisions are doctrine-bearing |
| Change Kaizen source-of-truth rules | Governance belongs to human/frontier review |
| Issue or change audit verdicts | `pass`, `pass-with-notes`, `fail`, and `stale` are human judgments |
| Mark task packets implementation-ready | This is an authority-bearing transition |
| Delete canonical notes | Destructive and irreversible without review |
| Move or rename canonical notes | Can break links and history |
| Silently overwrite existing files | Destroys auditability |
| Commit or push to GitHub | Source-control publication is human authority |
| Use terminal or arbitrary code execution | Bypasses the constrained tool and path boundary |
| Use general-purpose filesystem delete, move, rename, or overwrite tools | Broad filesystem mutation is outside the clerk surface |
| Modify source code repos | Out of Kaizen vault scope |
| Publish externally or customer-facing | High blast radius |
| Edit its own skill files or memory as accepted policy | Self-improvement drift risk |
| Treat research as doctrine | Research is evidence until promoted |
| Act outside allowed folders | Boundary escape risk |

## Required workflow rules

1. Hermes must search before creating any note.
2. Hermes must write only to explicitly allowed staging folders unless a human executes promotion.
3. Hermes must emit a diff before changing an existing file.
4. Hermes must preserve raw source material.
5. Hermes must run validation before declaring a draft complete.
6. Hermes must report actual tool output and exit codes, not just narrative success.
7. Hermes must escalate uncertainty instead of guessing.

## Draft routing table

| Operation | Hermes | Frontier model | Human |
|---|---:|---:|---:|
| Read/search | Owns | Optional | Not required |
| Raw source draft | Owns | Optional | Not required |
| Source summary draft | Owns in staging; separates evidence and interpretation | Reviews if high-stakes | Optional |
| Claim extraction | Drafts one core assertion with supporting/conflicting evidence | Reviews if high-stakes | Optional |
| Log append | Owns | Not required | Not required |
| Index regeneration | Owns | Not required | Optional |
| Validation checks | Runs scripts | Not required | Optional |
| Spec creation/modification | Not authorized | Proposes/reviews | Approves |
| Decision creation/modification | Not authorized | Proposes/reviews | Approves |
| Architecture decisions | Not authorized | Proposes | Approves |
| Audit verdict | Not authorized | Recommends | Ratifies |
| Implementation-ready status | Not authorized | Recommends | Approves |
| Delete/move canonical files | Not authorized | Optional review | Approves/executes |
| Commit/push | Not authorized | Not required | Executes |

## Open questions

- What exact Hermes Desktop settings can enforce tool restrictions?
- Can Hermes be configured with read-only roots and staging-only write roots?
- Can Hermes reliably emit diffs before writes, or must Kaizen wrap write operations externally?
- How should Hermes session IDs and model IDs be recorded in note provenance?
- Should Hermes write to this docs repo directly, or only to the future vault staging area?

## Related files

- `03-research-results/001-hermes-agent-boundaries-claude-summary.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/staging-write-wrapper-and-promotion-recovery.md`
