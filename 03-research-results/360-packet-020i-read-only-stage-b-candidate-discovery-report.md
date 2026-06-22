# Packet 020I — Read-Only Stage B Candidate Discovery Report

Status: discovery report / candidate narrowed
Date: 2026-06-22
Packet: 020I
Milestone: 14

## 1. Purpose

Record owner-supplied read-only Neon Ronin discovery evidence and narrow the Stage B candidate class without selecting or executing a Stage B task.

This report does not authorize coding or downstream repo mutation.

## 2. Starting checkpoint

Docs repository at report start:

```text
root: docs
branch: main
HEAD: acb2ff2fa1e50cc8b438c518717f32da335e42e4
working tree: clean
upstream: origin/main
ahead/behind: 29 / 0
```

Platform repository at report start:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

## 3. Authority basis

```text
03-research-results/356-packet-020g-implementation-ready-doc-quality-audit.md
03-research-results/358-packet-020h-fresh-context-resumption-proof.md
03-research-results/359-packet-020i-read-only-stage-b-candidate-discovery-preflight.md
owner-supplied PowerShell read-only discovery output for C:\dev\neon-ronin
```

## 4. Owner-supplied Neon Ronin repo state

Owner-supplied evidence reports:

```text
path: C:\dev\neon-ronin
branch/status: ## main...origin/main
HEAD: 8f1cc02ab6e084a8408e15287fb7c5abbe7bfbd8
branch: main
clean state: clean; no staged/unstaged/untracked status paths reported
```

This matches the previously pinned 020C Neon Ronin commit.

## 5. High-signal discovered surfaces

Owner-supplied read-only discovery identified these high-signal candidate surfaces.

### SearchClarity / worked-example surfaces

```text
docs/reference-examples/searchclarity-service-workspace-examples.md
docs/reference-examples/README.md
docs/claudeai-audits/phase5c-audit-question.md
docs/claudeai-audits/phase5c-audit.md
docs/decisions/adr-003-first-business-containment.md
```

### Observatory / shared intelligence surfaces

```text
docs/core/04-observatory.md
docs/core/17-observatory-scoring-contract.md
docs/decisions/adr-004-observatory-shared-intelligence-boundary.md
docs/future-cadidate/cross-workspace-content-market-intelligence.md
docs/future-cadidate/cross-workspace-visibility-observation.md
research-docs/r10-observatory-and-strategy-layer.md
```

### Workspace / parent-child surfaces

```text
docs/core/02-workspace-model.md
docs/core/03-business-onboarding.md
docs/core/09-workspace-lifecycle.md
docs/workspace-adapters/service-business-workspace.md
docs/workspaces/README.md
docs/reference-examples/searchclarity-service-workspace-examples.md
```

### Lightweight check/test surfaces

```text
pyproject.toml
tools/dev/check_first_proof.py
packages/neon-core/tests/test_workspace_config_create.py
packages/neon-core/tests/test_signal_candidate_create.py
packages/neon-core/tests/test_phase_6_business_neutrality_proof.py
packages/neon-core/tests/test_phase_6_cross_boundary_proof.py
tools/hammers/README.md
```

## 6. Discovery findings

### Finding 1 — Neon Ronin remains at pinned clean state

Verdict:

```text
PASS
```

The owner-supplied output reports Neon Ronin at the same HEAD pinned in 020C:

```text
8f1cc02ab6e084a8408e15287fb7c5abbe7bfbd8
```

### Finding 2 — SearchClarity has an existing legal home in reference examples

Verdict:

```text
PASS
```

The file list confirms:

```text
docs/reference-examples/searchclarity-service-workspace-examples.md
```

This aligns with earlier Neon Ronin audit logic that SearchClarity-aware content should live as a reference example, not as core doctrine.

### Finding 3 — Observatory boundary already has a decision surface

Verdict:

```text
PASS
```

The file list confirms:

```text
docs/decisions/adr-004-observatory-shared-intelligence-boundary.md
```

This is a better boundary surface than implementing Observatory or rewriting core docs.

### Finding 4 — Stage B should remain docs-only unless exact test/read contents justify more

Verdict:

```text
PASS
```

The discovery found tests and code, but current M14 evidence does not justify a code-bearing task. A docs-only task remains the safest first implementation-return proof.

## 7. Candidate shortlist

### Candidate 1 — Preferred: Reference-example boundary clarification

Candidate path:

```text
docs/reference-examples/searchclarity-service-workspace-examples.md
```

Candidate objective:

```text
Clarify that SearchClarity is a reference example / workspace-business-lane example under Neon Ronin, not core doctrine, and include the reconciled signal sentence if missing.
```

Why preferred:

```text
It is a legal home for SearchClarity-specific content.
It avoids contaminating core doctrine.
It is docs-only and reversible.
It directly tests the M14 parent/child boundary result.
```

Missing evidence before task packet:

```text
Exact file contents and exact insertion/replacement point.
```

### Candidate 2 — Secondary: Observatory ADR clarification

Candidate path:

```text
docs/decisions/adr-004-observatory-shared-intelligence-boundary.md
```

Candidate objective:

```text
Clarify that Observatory / IMI ownership remains a shared-boundary question and is not implemented, while preserving repo-specific Observatory language.
```

Why secondary:

```text
Good boundary fit, but could accidentally look like an Observatory ownership decision if written too strongly.
```

Missing evidence before task packet:

```text
Exact ADR contents and status/authority wording.
```

### Candidate 3 — Not preferred for first Stage B: Core docs cleanup

Candidate paths:

```text
docs/core/04-observatory.md
docs/core/17-observatory-scoring-contract.md
docs/core/02-workspace-model.md
```

Reason not preferred:

```text
Core-doc mutation has higher doctrine risk and broader blast radius.
It may be valid later, but not for the first tiny Stage B return proof.
```

## 8. Recommended Stage B candidate

Current recommendation:

```text
Candidate 1 — docs/reference-examples/searchclarity-service-workspace-examples.md
```

Recommended task class:

```text
one-file docs-only clarification;
no core doctrine mutation;
no code;
no tests beyond git diff/check and optional markdown inspection;
implementation-return evidence only.
```

## 9. Required focused evidence before Stage B task packet

Before a concrete Stage B task packet can be drafted, collect exact contents for the preferred candidate file and, optionally, the Observatory ADR.

Owner-side read-only command:

```powershell
$repo = "C:\dev\neon-ronin"
Push-Location $repo

Write-Host "===== STATUS ====="
git status --short --branch
Write-Host "HEAD:"
git rev-parse HEAD

Write-Host "===== PREFERRED CANDIDATE FILE ====="
Write-Host "--- docs/reference-examples/searchclarity-service-workspace-examples.md ---"
Get-Content "docs/reference-examples/searchclarity-service-workspace-examples.md" -Raw

Write-Host "===== OPTIONAL OBSERVATORY ADR ====="
Write-Host "--- docs/decisions/adr-004-observatory-shared-intelligence-boundary.md ---"
Get-Content "docs/decisions/adr-004-observatory-shared-intelligence-boundary.md" -Raw

Pop-Location
```

## 10. Stage B packet readiness

Current readiness:

```text
NOT READY FOR EXECUTION.
READY FOR FOCUSED TASK-PACKET DRAFT AFTER FILE CONTENTS ARE PROVIDED.
```

A later Stage B packet must include:

```text
starting Neon Ronin HEAD;
exact file path;
exact find/replace or insertion plan;
acceptance criteria;
check command;
rollback instructions;
return evidence requirements;
owner approval phrase;
explicit non-authorization of wider mutation.
```

## 11. Verdict

```text
PASS — STAGE B CANDIDATE NARROWED; EXACT FILE CONTENTS REQUIRED BEFORE TASK PACKET
```

## 12. Non-authorization

This report does not authorize:

```text
Stage B execution;
Stage B task packet approval;
coding Neon Ronin;
coding SearchClarity;
repo mutation in Neon Ronin;
repo mutation in SearchClarity;
platform mutation;
vault mutation;
staging mutation;
database mutation;
provider/customer-data work;
private/git-ignored data use;
Qdrant/Hermes/Tauri/Observatory implementation;
Git push.
```
