# Packet 020J — Stage B Task Packet: Neon Ronin Reference-Example Boundary Clarification

Status: proposed task packet / owner approval required
Date: 2026-06-22
Packet: 020J
Milestone: 14

## 1. Purpose

Define one tiny Stage B implementation-return candidate for a connected coding LLM / agent.

This packet proposes a single docs-only Neon Ronin mutation that clarifies the reconciled M14 boundary without touching code, core doctrine, provider/customer data, or Observatory implementation.

This packet does not authorize execution until the owner explicitly approves the exact task packet.

## 2. Starting checkpoint

Docs repository at 020J task-packet start:

```text
root: docs
branch: main
HEAD: 2411a3e99ad49a29a7090f64e941f6691f701ad8
working tree: clean
upstream: origin/main
ahead/behind: 30 / 0
```

Platform repository at start:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

Downstream Neon Ronin evidence supplied by owner:

```text
path: C:\dev\neon-ronin
branch/status: ## main...origin/main
HEAD: 8f1cc02ab6e084a8408e15287fb7c5abbe7bfbd8
branch: main
clean state: clean; no staged/unstaged/untracked paths reported
```

## 3. Authority basis

```text
03-research-results/354-packet-020e-stage-a-repo-reconciliation-report.md
03-research-results/355-packet-020f-parent-child-and-observatory-boundary-candidate.md
03-research-results/356-packet-020g-implementation-ready-doc-quality-audit.md
03-research-results/358-packet-020h-fresh-context-resumption-proof.md
03-research-results/359-packet-020i-read-only-stage-b-candidate-discovery-preflight.md
03-research-results/360-packet-020i-read-only-stage-b-candidate-discovery-report.md
owner-supplied read-only contents of C:\dev\neon-ronin\docs\reference-examples\searchclarity-service-workspace-examples.md
```

## 4. Task summary

```text
Repo: C:\dev\neon-ronin
Task type: docs-only
Allowed path: docs/reference-examples/searchclarity-service-workspace-examples.md
Objective: Add one M14 reconciled-boundary section that states SearchClarity is a reference-example workspace/business lane, Neon Ronin is the parent workspace operating layer, Kaizen governs docs/returns, and Observatory / IMI is not implemented or decided by this example.
```

## 5. Why this task is safe

```text
The target file is already explicitly labeled Reference example only.
The target file is already the legal home for SearchClarity-aware worked examples outside docs/core.
The task does not mutate core doctrine.
The task does not mutate code.
The task does not touch private/customer/provider data.
The task directly tests the M14 parent/child boundary result.
The task is reversible by deleting one inserted Markdown section.
```

## 6. Allowed scope

Exactly one allowed path:

```text
docs/reference-examples/searchclarity-service-workspace-examples.md
```

Allowed operation:

```text
Insert one Markdown section after the existing Boundary Rule section.
```

## 7. Prohibited scope

Do not edit:

```text
any file under docs/core/;
any file under packages/;
any file under apps/;
any file under services/;
any test file;
pyproject.toml;
AGENTS.md;
README.md;
ADR files;
SearchClarity repo;
Kaizen docs repo;
Kaizen platform repo;
private/git-ignored data;
provider/customer/sample data;
Observatory implementation files.
```

Do not:

```text
implement Observatory / IMI;
make an ownership decision for Observatory / IMI;
rename projects;
claim SearchClarity is onboarded or active;
claim code/test surfaces are built;
run external/provider/customer actions;
push to GitHub.
```

## 8. Exact edit plan

Find this exact block in:

```text
docs/reference-examples/searchclarity-service-workspace-examples.md
```

```markdown
## Boundary Rule

```text
Prepare Neon Ronin for SearchClarity.
Do not turn Neon Ronin into SearchClarity.
```
```

Replace it with:

```markdown
## Boundary Rule

```text
Prepare Neon Ronin for SearchClarity.
Do not turn Neon Ronin into SearchClarity.
```

## M14 Reconciled Boundary

```text
SearchClarity captures the signal.
Neon Ronin scores the signal.
Kaizen governs the project-intelligence and implementation-doc chain.
```

SearchClarity is a reference example of a future workspace/business lane under Neon Ronin, not core doctrine and not a platform authority source.

This example may inform reusable workspace, review, artifact, and signal patterns only after those patterns are extracted into business-neutral Neon Ronin language.

Observatory / IMI remains a shared intelligence capability and ownership-boundary question. This example does not implement Observatory / IMI, decide its ownership, or authorize ingestion, scoring, provider access, customer data, or cross-workspace data movement.
```

## 9. Acceptance criteria

The implementation return passes only if:

```text
only docs/reference-examples/searchclarity-service-workspace-examples.md changed;
existing Reference example only status remains;
existing Boundary Rule remains;
new M14 Reconciled Boundary section is inserted once;
new section includes exact sentence: SearchClarity captures the signal. Neon Ronin scores the signal. Kaizen governs the project-intelligence and implementation-doc chain.;
new section states SearchClarity is a reference example / workspace-business-lane example, not core doctrine;
new section states Observatory / IMI is not implemented or decided;
no core docs changed;
no code changed;
no tests changed;
no provider/customer/private data touched;
git diff --check passes;
working tree after implementation contains only the approved path until commit/return.
```

## 10. Check commands

Run from:

```text
C:\dev\neon-ronin
```

Required checks:

```powershell
git status --short --branch
git diff -- docs/reference-examples/searchclarity-service-workspace-examples.md
git diff --check
```

Optional docs-only sanity check:

```powershell
Select-String -Path "docs/reference-examples/searchclarity-service-workspace-examples.md" -Pattern "M14 Reconciled Boundary","SearchClarity captures the signal","not core doctrine","does not implement Observatory"
```

No unit tests are required because the task is docs-only and limited to one Markdown reference-example file.

## 11. Rollback plan

Rollback is simple:

```text
Remove the inserted M14 Reconciled Boundary section from docs/reference-examples/searchclarity-service-workspace-examples.md.
```

No database, generated artifact, provider state, customer data, or external state is involved.

## 12. Implementation-return evidence required

A connected coding LLM / agent must return:

```text
starting repo path;
starting branch;
starting HEAD;
starting clean/dirty status;
exact changed path list;
git diff for the changed file;
git diff --check result;
post-change git status --short --branch;
commit SHA if committed;
statement that no prohibited paths were changed;
statement that no provider/customer/private data was used;
statement that no Git push occurred.
```

## 13. Owner approval phrase

Execution requires the owner to approve with this phrase:

```text
Approve Packet 020J Stage B docs-only implementation at Neon Ronin HEAD 8f1cc02ab6e084a8408e15287fb7c5abbe7bfbd8.
```

No other approval wording should be inferred.

## 14. Task-packet verdict

```text
PASS — CONCRETE STAGE B DOCS-ONLY TASK PACKET READY FOR OWNER APPROVAL
```

Stage B remains unauthorized until owner approval is given.

## 15. Non-authorization

This packet does not authorize:

```text
execution without owner approval;
any code change;
any core doctrine change;
any SearchClarity repo change;
any Kaizen platform change;
any Kaizen vault/staging/database mutation;
provider/customer-data work;
private/git-ignored data use;
Qdrant/Hermes/Tauri/Observatory implementation;
Git push.
```
