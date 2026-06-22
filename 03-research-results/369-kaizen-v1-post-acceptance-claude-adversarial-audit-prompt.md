# Kaizen v1 Post-Acceptance Claude Adversarial Audit Prompt

Status: prompt ready for external audit
Date: 2026-06-22
Packet: post-v1 audit

## 1. Purpose

Use this prompt with Claude to perform a thorough post-acceptance adversarial audit of the accepted Kaizen v1 / Milestone 14 record.

This audit is intended to find contradictions, stale docs, hidden blockers, weak evidence, unsafe next steps, and post-v1 hardening priorities.

It is not intended to restart Kaizen or erase owner acceptance.

## 2. Prompt to Claude

```markdown
# Kaizen v1 Post-Acceptance Adversarial Audit

You are performing a fresh-context adversarial audit of the accepted Kaizen v1 / Milestone 14 record.

Do not use prior chat history.
Do not assume the project state beyond the repository and MCP evidence you inspect.
Do not mutate files, repos, databases, vault notes, staging evidence, or remotes.
Do not approve, execute, amend, promote, push, delete, or repair anything.

Your job is to hammer the accepted v1 record and report whether the acceptance story is internally consistent, sufficiently evidenced, and safe to build on.

Important framing:

```text
Owner acceptance has already occurred.
Do not treat owner acceptance as missing.
Do not restart Kaizen.
Do not treat post-v1 hardening as v1 failure unless it invalidates a required v1/M14 exit criterion.
Do identify any contradiction that would make the accepted record unsafe or materially false.
```

## Available MCP posture

You should use MCP tools where available. Use read-only / inspection tools only.

### Authorized Go8 roots

```text
docs
platform
vault
staging
kaizen_mcp
chatgpt_mcp
northstar
pilot_evidence
```

Primary roots for this audit:

```text
docs
platform
vault
staging
```

Secondary roots only if needed:

```text
chatgpt_mcp
kaizen_mcp
pilot_evidence
northstar
```

Do not inspect unrelated private material.
Do not access owner-private oracle material.
Do not inspect secrets or DSNs beyond secret-exposure classification outputs.
Do not request raw secret values.

## Authorized MCP tools

Use the following read-only / inspection tools if available:

```text
health
list_roots
repository_snapshot
git_status
git_remote_status
git_log
git_show_commit
git_commit_details
git_staged_paths
git_diff
git_diff_check
verify_no_changes
verify_exact_path_scope
file_tree_manifest
list_directory
read_file
read_line_range
search_text
sha256_file
sha256_paths
scan_text_integrity
scan_changed_text_integrity
classify_secret_exposure
staging_evidence_inventory
process_lock_status
venv_status
validate_pyproject
installed_entrypoint_status
inspect_console_parser
pg_health
pg_server_isready
pg_list_databases
pg_list_roles
pg_list_tables
pg_list_extensions
pg_describe_database
pg_read_evidence_rows
pg_reconcile_retention_plan_counts
```

Forbidden tool categories:

```text
write_new_text_file
replace_exact_text
ensure_final_newline
git_add_exact_paths
git_commit_staged
git_push_existing_upstream
pg_create_database
pg_drop_database
pg_create_role
pg_grant_database_privilege
pg_run_migration_file
pg_create_extension
run_console_script
run_python_module
run_test_profile
run_coverage_profile
run_editable_install
test_artifact_cleanup
any mutation-capable filesystem, git, database, staging, vault, or repo tool
```

If a tool name differs in your MCP session, use the closest strictly read-only equivalent and report the exact tool names used.

## Repository checkpoint to verify first

Expected Kaizen docs state at acceptance record:

```text
root: docs
branch: main
HEAD: 62232032f0e5da0a0b646a849fc085f39d1ddf8e
working tree: clean
upstream: origin/main
ahead/behind expected around: 38 / 0
```

Expected platform state:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

Expected vault state:

```text
root: vault
branch: main
HEAD: c898f261c0b341eb8419125247c8bd53ef567d6c
working tree: clean
remotes: none
```

If these do not match, report a checkpoint mismatch before continuing.

## Required read set

Read these files from the docs root first:

```text
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
03-research-results/338-packet-019d-synthetic-proof-and-m14-readiness-contract.md
03-research-results/342-milestone-13-owner-closure-acceptance.md
03-research-results/343-milestone-14-definition-readiness-audit.md
03-research-results/345-packet-020a-m14-workload-selection-and-boundary-registration.md
03-research-results/346-packet-020b-phase-0-boundary-and-collision-pre-registration.md
03-research-results/348-packet-020b-claude-review-disposition.md
03-research-results/349-packet-020c-repo-state-pinning-and-idea-only-firewall.md
03-research-results/351-packet-020d-stage-a-idea-only-generation-output.md
03-research-results/352-packet-020d-stage-a-idea-only-output-review.md
03-research-results/354-packet-020e-stage-a-repo-reconciliation-report.md
03-research-results/355-packet-020f-parent-child-and-observatory-boundary-candidate.md
03-research-results/356-packet-020g-implementation-ready-doc-quality-audit.md
03-research-results/358-packet-020h-fresh-context-resumption-proof.md
03-research-results/360-packet-020i-read-only-stage-b-candidate-discovery-report.md
03-research-results/361-packet-020j-stage-b-task-packet-neon-ronin-reference-example-boundary-clarification.md
03-research-results/363-packet-020j-stage-b-implementation-return.md
03-research-results/364-packet-020j-stage-b-closure-audit-and-sequence-reconciliation.md
03-research-results/365-packet-020k-backup-restore-proof-plan-and-evidence-request.md
03-research-results/366-packet-020k-backup-restore-proof-result.md
03-research-results/367-packet-020l-kaizen-v1-completion-audit-and-owner-acceptance-readiness.md
03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md
```

Use search tools to discover any stale docs that contradict v1 acceptance, M14 status, vault role, Neon Ronin/SearchClarity boundaries, or packet sequence.

Suggested searches:

```text
Milestone 14
Kaizen v1
owner acceptance
accepted
draft for owner review
backup / restore
Neon Ronin
SearchClarity
Observatory
IMI
vault
Obsidian
ahead
Git push
020K
020L
```

## Audit questions

Answer these explicitly:

1. Does the accepted M14 chain actually satisfy the M14 exit criteria?
2. Are any required exit criteria only partially met, overstated, or unsupported?
3. Is owner acceptance correctly recorded after the 020L readiness audit?
4. Are the caveats in 020L and 368 sufficient and honest?
5. Did the project preserve the distinction between Kaizen, Neon Ronin, SearchClarity, and Observatory / IMI?
6. Did any record accidentally make kaizen-docs the permanent home of Neon Ronin truth?
7. Is the role of the Kaizen Obsidian Vault clear enough after owner clarification?
8. Did backup/restore proof satisfy the requirement despite the Neon Ronin restore worktree normalization note?
9. Are there stale docs that still imply M14 or Kaizen v1 is not accepted?
10. Are there stale packet-numbering or sequence contradictions that must be fixed before post-v1 work begins?
11. Is the docs repo ahead-by-38 state a hard blocker, a governance caveat, or a post-v1 hardening item?
12. Is the Neon Ronin ahead-by-2 state a hard blocker, a governance caveat, or a post-v1 hardening item?
13. Are there hidden safety issues around secrets, remotes, database state, or restore artifacts?
14. What must be fixed immediately before serious post-v1 usage?
15. What can safely be deferred as post-v1 hardening?

## Required verdict format

Return exactly one of:

```text
PASS
PASS WITH REQUIRED FIXES
FAIL
```

Verdict rules:

```text
PASS
= accepted v1 record is internally consistent and safe to build on; only optional hardening remains.

PASS WITH REQUIRED FIXES
= accepted v1 remains valid, but specific stale docs, sync posture, vault role documentation, or hardening records must be fixed before serious post-v1 usage.

FAIL
= you found a real acceptance-invalidating defect, such as a missing M14 exit criterion, false proof claim, unaccepted owner decision, corrupted restore proof, or serious project-boundary collapse.
```

Do not use FAIL for normal post-v1 hardening items unless they invalidate the accepted v1 proof.

## Required report structure

```markdown
# Kaizen v1 Post-Acceptance Adversarial Audit Report

## 1. Verdict
PASS / PASS WITH REQUIRED FIXES / FAIL

## 2. Repository checkpoint evidence
- docs
- platform
- vault
- any mismatch

## 3. Files read and tools used
- list files read
- list MCP tools used

## 4. Exit criteria audit
Table with criterion, evidence, verdict, notes.

## 5. Contradictions or stale docs
List each issue with file path, line/section if available, severity, and recommended fix.

## 6. Boundary audit
Kaizen vs Neon Ronin vs SearchClarity vs Observatory / IMI.

## 7. Vault role audit
Assess whether the accepted record reflects: kaizen-docs is construction/proof workbench; Kaizen Obsidian Vault is intended canonical project-intelligence layer.

## 8. Backup / restore audit
Assess 020K proof and the Neon Ronin worktree normalization note.

## 9. Repo sync / remote posture audit
Assess docs ahead state and Neon Ronin ahead state.

## 10. Secret / safety audit
Report whether any secret-exposure checks were run and summarize without revealing secrets.

## 11. Required fixes
Only items that must be addressed before serious post-v1 usage.

## 12. Deferred hardening backlog
Items safe to defer.

## 13. Recommended next action
One clear next packet / action.
```

## Final instruction

Be adversarial, but precise. Do not create work just to create work. Separate acceptance-invalidating blockers from normal post-v1 hardening.
```

## 3. Notes for owner

Use this prompt in Claude with the Go8 MCP server available.

Recommended MCP posture:

```text
read-only / inspect tools enabled;
write, replace, stage, commit, push, SQL mutation, and run tools disabled or avoided;
no secret values surfaced;
no owner-private oracle material opened.
```
