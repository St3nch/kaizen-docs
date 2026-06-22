# Packet 020D — Stage A Idea-Only Generation Protocol

Status: protocol / prompt contract
Date: 2026-06-22
Packet: 020D
Milestone: 14

## 1. Purpose

Define the isolated Stage A idea-only generation protocol for M14.

020D does not generate the Neon Ronin or SearchClarity candidate docs. It defines the exact prompt, output set, and stop rules for a fresh-context generator that must use only the frozen 020C idea bundle.

## 2. Starting checkpoint

Docs repository at 020D start:

```text
root: docs
branch: main
HEAD: 1012dd050a0fde5af21ebfdd2318623fe409fbb7
working tree: clean
upstream: origin/main
ahead/behind: 20 / 0
```

Platform repository at 020D start:

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
03-research-results/349-packet-020c-repo-state-pinning-and-idea-only-firewall.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
```

## 4. Stage A generator role

The Stage A generator acts as a clean Kaizen user-facing doc generator.

It receives only:

```text
the frozen 020C idea-only input bundle;
the required output contract in this 020D protocol;
the instruction that all outputs are unreconciled candidate intelligence.
```

It must not receive:

```text
C:\dev\neon-ronin repo facts;
C:\dev\searchclarity repo facts;
repo manifests;
recent commits;
source tree details;
prior steward-memory repo details;
Claude's repo-reality observations;
private/git-ignored data;
implementation code context.
```

## 5. Frozen idea-only input bundle

The Stage A generator must use this exact bundle.

### Neon Ronin

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

### SearchClarity

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

### Shared Observatory / IMI

```text
The intended Observatory / IMI capability is the same Kaizen-planned shared intelligence capability.
It should be usable across multiple SERP / SEO / LLM citation / marketplace intelligence projects.
The repeated Observatory references across Kaizen, Neon Ronin, and SearchClarity are a boundary/ownership design knot, not proof that three separate incompatible final systems should exist.
Whether Observatory / IMI should remain a Kaizen capability or become its own Kaizen-governed project is an open design question.
No Observatory / IMI implementation is authorized during M14 Stage A.
```

## 6. Required output set

The Stage A generator must produce one consolidated Markdown report with these sections:

```text
Neon Ronin idea intake summary;
Neon Ronin candidate project overview;
Neon Ronin candidate current state;
SearchClarity idea intake summary;
SearchClarity candidate project overview;
SearchClarity candidate current state;
parent/child dependency map;
Observatory / IMI boundary candidate;
scope boundary register;
implementation-ready-doc candidate assessment;
recommended next docs to generate;
known unknowns and reconciliation questions;
non-authorization boundaries preserved.
```

The generator may propose, but must not write, later candidate docs such as:

```text
spec;
audit;
task packet;
coding-agent handoff prompt;
implementation-return template.
```

## 7. Candidate intelligence marking

Every output must be explicitly marked:

```text
UNRECONCILED CANDIDATE INTELLIGENCE — GENERATED FROM IDEA-ONLY INPUT
```

The generator must not claim repo confirmation.

The generator must not mention repo files, commit SHAs, package names, source folders, tests, or current implementation state.

## 8. Quality constraints

The generated report must be:

```text
concise enough to be useful;
clear enough for fresh-context review;
explicit about assumptions;
explicit about unknowns;
strict about project boundaries;
strict about no coding by Kaizen;
strict about Observatory / IMI not being implemented;
lean for SearchClarity unless evidence later justifies more detail.
```

## 9. Pass/fail traps for the generator

The generator fails if it:

```text
confuses Kaizen with Neon Ronin;
confuses SearchClarity with Kaizen;
models SearchClarity as a peer of Neon Ronin instead of a child/dependent lane;
claims SearchClarity is already onboarded into Neon Ronin;
claims Observatory / IMI is implemented;
fragments Observatory / IMI into incompatible systems without preserving the owner clarification;
mentions repo facts;
invents code, tests, package structure, customers, orders, providers, or existing implementations;
starts writing implementation code;
produces a task packet as if it were ready without reconciliation.
```

## 10. Stage A generation prompt

Use this prompt in a fresh context with no repo access:

```markdown
# Kaizen M14 Stage A — Idea-Only Candidate Doc Generation

You are acting as the isolated idea-only generation lane for Kaizen M14.

You must use only the idea bundle below. You do not have access to any repository. Do not invent repository facts. Do not mention source files, package names, commits, tests, folders, or existing implementation state.

Every output is unreconciled candidate intelligence.

Mark the report with:

```text
UNRECONCILED CANDIDATE INTELLIGENCE — GENERATED FROM IDEA-ONLY INPUT
```

## Frozen idea-only input

### Neon Ronin

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

### SearchClarity

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

### Shared Observatory / IMI

```text
The intended Observatory / IMI capability is the same Kaizen-planned shared intelligence capability.
It should be usable across multiple SERP / SEO / LLM citation / marketplace intelligence projects.
The repeated Observatory references across Kaizen, Neon Ronin, and SearchClarity are a boundary/ownership design knot, not proof that three separate incompatible final systems should exist.
Whether Observatory / IMI should remain a Kaizen capability or become its own Kaizen-governed project is an open design question.
No Observatory / IMI implementation is authorized during M14 Stage A.
```

## Required output sections

Produce one Markdown report with:

```text
Neon Ronin idea intake summary;
Neon Ronin candidate project overview;
Neon Ronin candidate current state;
SearchClarity idea intake summary;
SearchClarity candidate project overview;
SearchClarity candidate current state;
parent/child dependency map;
Observatory / IMI boundary candidate;
scope boundary register;
implementation-ready-doc candidate assessment;
recommended next docs to generate;
known unknowns and reconciliation questions;
non-authorization boundaries preserved.
```

## Rules

```text
Do not code.
Do not claim repo confirmation.
Do not invent implementation state.
Do not mention repo paths, commits, files, folders, packages, tests, customers, providers, or existing orders.
Keep SearchClarity lean.
Preserve Kaizen / Neon Ronin / SearchClarity identities.
Preserve SearchClarity as child/dependent lane under Neon Ronin.
Preserve Observatory / IMI as a shared capability boundary question, not an implemented system.
```

## Output title

Use this title:

```markdown
# M14 Stage A Idea-Only Candidate Intelligence — Neon Ronin / SearchClarity
```
```

## 11. Storage rule for generated output

The generated Stage A output should not be filed as canonical doctrine.

Recommended next record after generation:

```text
03-research-results/351-packet-020d-stage-a-idea-only-generation-output.md
```

That record must clearly mark the output as unreconciled candidate intelligence.

## 12. Review gate after generation

After generation, before repo reconciliation:

```text
review the output against the 020D pass/fail traps;
record whether it obeyed the idea-only firewall;
only then proceed to Packet 020E repo reconciliation protocol.
```

## 13. 020D verdict

```text
PASS — STAGE A IDEA-ONLY GENERATION PROTOCOL RECORDED
```

M14 may proceed to isolated Stage A idea-only generation using the prompt in section 10.

## 14. Non-authorization

This record does not authorize:

```text
repo reconciliation;
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
