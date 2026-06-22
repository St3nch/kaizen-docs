# Packet 020C — Repo-State Pinning and Idea-Only Firewall

Status: accepted preflight / Stage A gate record
Date: 2026-06-22
Packet: 020C
Milestone: 14

## 1. Purpose

Satisfy Claude review required fixes RF-1 and RF-2 from Packet 020B before any M14 Stage A idea-only generation begins.

020C pins downstream repo reality, freezes the idea-only input bundle, defines the idea-only firewall, and completes the scoring standards needed for reproducible Stage A evaluation.

## 2. Starting checkpoint

Docs repository at 020C start:

```text
root: docs
branch: main
HEAD: f695dbd049bd8827657d2757bf0ab9c7369c42f1
working tree: clean
upstream: origin/main
ahead/behind: 19 / 0
```

Platform repository at 020C start:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

## 3. Authority basis

```text
03-research-results/345-packet-020a-m14-workload-selection-and-boundary-registration.md
03-research-results/346-packet-020b-phase-0-boundary-and-collision-pre-registration.md
03-research-results/348-packet-020b-claude-review-disposition.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
```

## 4. RF-1 downstream repo-state pins

Repo pins are owner-supplied from PowerShell read-only Git commands.

### Neon Ronin

```text
path: C:\dev\neon-ronin
branch/status: ## main...origin/main
HEAD: 8f1cc02ab6e084a8408e15287fb7c5abbe7bfbd8
branch: main
remote: origin https://github.com/St3nch/neon-ronin.git
clean state: clean, no staged/unstaged/untracked paths reported by git status --short --branch
```

Recent commits:

```text
8f1cc02 Add visibility observation future candidate
23a0d59 Add future candidate docs area
e5fbc52 Fix post-audit persistence doc nits
09c8403 Add Phase 7 readiness gate
f8ae2cc Mark Phase 6 sufficient for current scope
```

### SearchClarity

```text
path: C:\dev\searchclarity
branch/status: ## main...origin/main [ahead 4]
HEAD: 1fe0197e570d5a7e63d52130bc3a9668f3479b99
branch: main
remote: origin https://github.com/St3nch/search-clarity.git
clean state: clean, no staged/unstaged/untracked paths reported by git status --short --branch
```

Recent commits:

```text
1fe0197 docs: add source sample action plan to roadmap
3f8c6cc docs: add data acquisition and source sample planning
a4ca8cd chore: ignore generated sample exports
3d11a3d docs: add modular report system and Etsy audit mock
554e316 docs: update SearchClarity operations launch docs
```

### Repo-reality scoring declaration

M14 Stage A reconciliation must be scored only against the pinned repo states above unless a later owner-approved record explicitly repins them.

If either repo changes before reconciliation, Stage A must stop and repin before scoring.

## 5. RF-2 idea-only input bundle

The idea-only generation phase must use only the frozen input below.

### Neon Ronin idea-only input

```text
Project name: Neon Ronin.
Project type: downstream project.
Core idea: a multi-agent business operating system for managing small businesses.
Purpose: help an owner operate multiple small-business workspaces with human-in-the-loop controls.
Known relationship to Kaizen: Kaizen governs project intelligence and produces implementation docs; Neon Ronin is not Kaizen.
Known child/dependent project: SearchClarity is a business/service lane intended to run under Neon Ronin.
Known constraints: no autonomous publish, spend, submit, or customer-facing action without human approval; no provider/customer-data work during this proof; no Qdrant, Hermes, Tauri, production deployment, or full Neon Ronin implementation during M14 Stage A.
Desired Kaizen output: implementation-ready docs and handoff packets that a connected coding LLM could later follow.
```

### SearchClarity idea-only input

```text
Project name: SearchClarity.
Project type: child/dependent business/service lane under Neon Ronin.
Core idea: a search visibility / marketplace SEO / LLM-citation service business.
Purpose: produce professional evidence-based visibility audits and related search-intelligence deliverables.
Known relationship to Neon Ronin: SearchClarity should eventually run under Neon Ronin as a business/workspace lane.
Known relationship to Kaizen: SearchClarity is not Kaizen; Kaizen governs project intelligence and docs.
Known Observatory relationship: SearchClarity may capture or benefit from SEO/SERP/LLM-citation signals that relate to the Kaizen-planned Observatory / IMI capability.
Known constraints: no customer-data use, no provider purchase, no logged-in scraping, no commercial launch, no full SearchClarity implementation during M14 Stage A.
Desired Kaizen output: lean implementation-ready docs, parent/child dependency model, and reconciliation plan.
```

### Shared Observatory / IMI idea-only input

```text
The intended Observatory / IMI capability is the same Kaizen-planned shared intelligence capability.
It should be usable across multiple SERP / SEO / LLM citation / marketplace intelligence projects.
The repeated Observatory references across Kaizen, Neon Ronin, and SearchClarity are a boundary/ownership design knot, not proof that three separate incompatible final systems should exist.
Whether Observatory / IMI should remain a Kaizen capability or become its own Kaizen-governed project is an open design question.
No Observatory / IMI implementation is authorized during M14 Stage A.
```

## 6. Idea-only firewall

Stage A idea-only generation must follow this firewall:

```text
Generate Phase 1/2 docs only from the frozen idea-only input bundle in this 020C record.
Do not use Neon Ronin repo facts during idea-only generation.
Do not use SearchClarity repo facts during idea-only generation.
Do not use prior steward-memory facts about repo internals during idea-only generation.
Mark generated docs as unreconciled candidate intelligence.
Admit repo reads only in the later reconciliation packet.
Classify later matches or mismatches using CONFIRMED / CONTRADICTED / STALE / INVENTED / RESERVED-NOT-BUILT / PRIVATE-LOCAL / UNKNOWN.
```

Recommended enforcement:

```text
Use a fresh-context or isolated generation prompt that includes only this 020C idea bundle and the relevant Kaizen doc-output rules.
Do not include repo paths or repo summaries in the generation prompt.
```

## 7. Verdict thresholds

Stage A scoring uses the 020B categories with these thresholds:

```text
Any pass/fail gate breach = FAIL regardless of score.
Any invented repo fact = FAIL for reconciliation unless corrected before closure.
Any project identity merge = FAIL.
Any parent/child inversion = FAIL.
Any Observatory / IMI implementation claim = FAIL.
Any coding by Kaizen = FAIL.
Any single rubric category scored 0 = PASS WITH REQUIRED FIXES at best.
Aggregate average >= 2.5 with no gates and no 0 category = PASS.
Aggregate average >= 2.0 and < 2.5 with no gates = PASS WITH FIXES.
Aggregate average < 2.0 = FAIL.
```

## 8. Observatory / IMI two-part scoring rule

Stage A must score Observatory / IMI boundary clarity in two parts:

```text
Repo-fidelity part: accurately report what each repo says about Observatory-related language after reconciliation.
Ownership-analysis part: analyze those references as a shared-capability boundary knot under the owner clarification.
```

Do not collapse repo reality into the owner's target model as if the repos already agreed.

Do not fragment the owner's intended shared capability into unrelated final systems without boundary analysis.

## 9. Built-evidence standard

A surface counts as built only if:

```text
source files exist;
and tests, checks, or executable proof exercise the surface.
```

Reserved folders, empty folders, roadmap statements, ADRs, or docs-only planning do not prove a built feature.

## 10. No-ready-packet consequence path

If Stage A produces no implementation-ready packet that meets the quality bar:

```text
Do not force Stage B.
Record no-ready-packet as a finding.
Either create a deliberately hand-seeded minimal task packet for owner review, or trigger a v1-scope conversation.
Stage B remains unauthorized until a separate packet and owner approval exist.
```

## 11. Generating lane vs review lane

```text
Kaizen Stage A generation lane: produces candidate docs from frozen idea-only input.
Review/audit lane: evaluates candidate docs and reconciles them against pinned repo reality.
Connected coding lane: may later execute one approved Stage B task; Kaizen itself does not code.
```

## 12. RF closure status

```text
RF-1 downstream repo commit state: satisfied by owner-supplied pins in section 4.
RF-2 idea-only input and firewall: satisfied by sections 5 and 6.
RF-3 packet-numbering divergence: satisfied in Result 348 and M14 spec correction.
```

## 13. 020C verdict

```text
PASS — REPO STATES PINNED AND IDEA-ONLY FIREWALL RECORDED
```

M14 may proceed to Packet 020D: Stage A Idea-Only Intake and Generated-Docs Protocol.

Stage A generation remains blocked until 020D is explicitly created/accepted.

## 14. Non-authorization

This record does not authorize:

```text
Stage A generation;
Stage B execution;
coding Neon Ronin;
coding SearchClarity;
repo mutation in Neon Ronin or SearchClarity;
platform mutation;
vault mutation;
staging mutation;
database mutation;
provider/customer-data work;
Qdrant/Hermes/Tauri/Observatory implementation;
Git push.
```
