# Packet 020E — Stage A Repo Reconciliation Protocol

Status: protocol / read-only reconciliation contract
Date: 2026-06-22
Packet: 020E
Milestone: 14

## 1. Purpose

Define the read-only repo reconciliation protocol for comparing M14 Stage A idea-only candidate intelligence against pinned Neon Ronin and SearchClarity repo reality.

020E defines how reconciliation will be performed. It does not yet perform the reconciliation.

## 2. Starting checkpoint

Docs repository at 020E start:

```text
root: docs
branch: main
HEAD: 6b460bc5b2910a541d66a0fad58273aed37a9880
working tree: clean
upstream: origin/main
ahead/behind: 22 / 0
```

Platform repository at 020E start:

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
03-research-results/349-packet-020c-repo-state-pinning-and-idea-only-firewall.md
03-research-results/351-packet-020d-stage-a-idea-only-generation-output.md
03-research-results/352-packet-020d-stage-a-idea-only-output-review.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
```

## 4. Pinned reconciliation targets

### Neon Ronin

```text
path: C:\dev\neon-ronin
branch/status: ## main...origin/main
HEAD: 8f1cc02ab6e084a8408e15287fb7c5abbe7bfbd8
branch: main
clean state at pin: clean
```

### SearchClarity

```text
path: C:\dev\searchclarity
branch/status: ## main...origin/main [ahead 4]
HEAD: 1fe0197e570d5a7e63d52130bc3a9668f3479b99
branch: main
clean state at pin: clean
```

Reconciliation must stop and repin if either downstream repo HEAD or working-tree state changes before readout.

## 5. Reconciliation source output

Idea-only candidate output to reconcile:

```text
03-research-results/351-packet-020d-stage-a-idea-only-generation-output.md
SHA-256: fede86ce22f3892d4ad313b8c3437b69cc757e71fa63b74b8c8e4f0a93ecfd8d
```

## 6. Authorized read-only inputs

### Neon Ronin authorized reads

Read only enough to determine project identity, docs reality, implementation reality, tests/checks, and boundaries.

Priority inputs:

```text
README / root overview files;
AGENTS or operator guidance files if present;
docs overview / roadmap / core doctrine files;
architecture / decision docs;
workspace / adapter / business-lane docs;
pyproject / package / config summaries;
source tree manifest at shallow depth;
test/check command discovery if documented;
recent commit metadata already pinned in 020C.
```

Do not read private secrets, credentials, env files, or generated/cache folders.

Do not treat reserved or empty folders as built features.

### SearchClarity authorized reads

Read only enough to determine project identity, docs reality, service-lane reality, parent/dependency references, and private-data boundaries.

Priority inputs:

```text
README / root overview files;
docs roadmap / operations / launch / service definition files;
report or audit template docs if present;
planning docs;
source tree manifest at shallow depth;
.gitignore / private sample boundaries if present;
recent commit metadata already pinned in 020C.
```

Do not read or request git-ignored/private customer data, raw provider data, private source samples, credentials, exports, or generated local artifacts.

## 7. Claim extraction protocol

Extract claims from the idea-only output into rows.

Each row must include:

```text
claim_id;
project: Kaizen / Neon Ronin / SearchClarity / Observatory / Shared;
claim text;
claim source section in 351;
classification;
supporting repo evidence or reason for UNKNOWN;
notes / required correction.
```

## 8. Classifications

Use exactly these classifications:

```text
CONFIRMED — idea-only claim matches pinned repo reality.
CONTRADICTED — pinned repo reality differs.
STALE — idea-only claim or repo state has moved past the other.
INVENTED — idea-only claim has no support but was stated as fact.
RESERVED-NOT-BUILT — generated output treated reserved/deferred structure as built.
PRIVATE/LOCAL — repo references intentionally local/private/git-ignored material.
UNKNOWN — insufficient evidence; do not infer.
```

## 9. Scoring categories

Score 0-3:

```text
project identity accuracy;
scope clarity;
parent/child boundary clarity;
Observatory / IMI boundary clarity;
repo reality alignment;
missing-assumption detection;
hallucination / invented-fact avoidance;
reserved-vs-built honesty;
implementation-ready doc quality;
testability;
fresh-context usefulness;
overbuild control.
```

Thresholds:

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

## 10. Built-evidence standard

A surface counts as built only if:

```text
source files exist;
and tests, checks, or executable proof exercise the surface.
```

Docs, roadmap statements, ADRs, empty folders, reserved folders, or future candidates do not prove a built feature.

## 11. Observatory / IMI two-part scoring

Reconciliation must separate:

```text
repo fidelity: what Neon Ronin and SearchClarity actually say about Observatory-related concepts;
ownership analysis: how those references relate to the owner clarification that Observatory / IMI is intended as a shared Kaizen-planned intelligence capability.
```

Do not pretend the repos already agree if they do not.

Do not pretend repeated Observatory references prove unrelated final systems.

## 12. Required reconciliation outputs

The reconciliation pass should produce one governed report containing:

```text
repo-state repin check;
read-source inventory;
claim classification table;
Neon Ronin reconciliation summary;
SearchClarity reconciliation summary;
parent/child dependency assessment;
Observatory / IMI boundary assessment;
reserved-vs-built assessment;
private/local data assessment;
score table;
required corrections to idea-only candidate output;
recommendation for 020F.
```

Recommended next output path:

```text
03-research-results/354-packet-020e-stage-a-repo-reconciliation-report.md
```

## 13. Stop conditions

Stop if:

```text
downstream repo HEAD changed from 020C pins;
downstream repo working tree is dirty;
a read would require private/git-ignored/customer/provider data;
repo facts are unclear and the generator starts inferring;
reserved folders are being treated as built;
project identities blur;
Observatory / IMI implementation is implied;
Stage B execution is attempted;
repo mutation is needed.
```

## 14. Verdict

```text
PASS — 020E REPO RECONCILIATION PROTOCOL RECORDED
```

M14 may proceed to the read-only 020E reconciliation report if the pinned downstream repo states still match.

## 15. Non-authorization

This protocol does not authorize:

```text
repo mutation;
Stage B execution;
coding Neon Ronin;
coding SearchClarity;
provider/customer-data work;
private/git-ignored data use;
Qdrant/Hermes/Tauri/Observatory implementation;
Git push.
```
