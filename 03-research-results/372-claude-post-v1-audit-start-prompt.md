# Claude Post-v1 Audit Start Prompt

Status: ready for owner use
Date: 2026-06-22
Purpose: one-paste start prompt for Claude post-v1 adversarial audit

## Prompt

```markdown
# Kaizen v1 Post-Acceptance Adversarial Audit — Build MCP and Audit

You are Claude. You are being asked to perform a fresh-context adversarial audit of accepted Kaizen v1 / Milestone 14.

Do not use prior chat history.
Do not restart Kaizen.
Do not mutate anything.
Do not approve, execute, amend, promote, push, delete, migrate, repair, or write files.
Do not print, store, or report passwords, DSNs, API keys, tokens, or surrounding secret text.

## Mission

Build or configure a small read-only MCP audit server, verify the configured roots and checkpoints, then audit the accepted Kaizen v1 record.

Use these files as your main instructions:

```text
C:\dev\kaizen-docs\03-research-results\369-kaizen-v1-post-acceptance-claude-adversarial-audit-prompt.md
C:\dev\kaizen-docs\03-research-results\370-kaizen-v1-post-acceptance-claude-mcp-tool-requirements.md
C:\dev\kaizen-docs\03-research-results\371-claude-mcp-build-and-audit-quickstart.md
```

File 369 defines the adversarial audit mission and report format.
File 370 defines the full MCP tool requirements.
File 371 is the token-saving quickstart: use it first so you do not waste time designing the tool surface.

## Do not overbuild

Build the minimum read-only MCP tool set needed for the audit.

Use simple fixed-root tools:

```text
repo_status
repo_log
repo_show_commit
repo_remote_status
repo_diff_check
repo_changed_paths
repo_verify_clean
list_dir
file_manifest
read_file
read_lines
search_text
sha256_file
scan_text_integrity
secret_scan_summary
list_backup_files
hash_file_under_backup_work
git_bundle_verify
inspect_restored_repo
```

Optional only if cheap and safe:

```text
pg_health
pg_list_databases
pg_list_roles
pg_list_tables
pg_read_evidence_rows
venv_status
pyproject_summary
```

Do not build or expose mutation tools.

## Fixed roots

Configure only these fixed roots:

```text
docs        = C:\dev\kaizen-docs
platform    = C:\dev\kaizen\platform
vault       = C:\dev\kaizen\vault
neon_ronin  = C:\dev\neon-ronin
backup_work = C:\dev\kaizen-backup-work
```

Optional only if needed:

```text
staging       = C:\dev\kaizen\staging
searchclarity = C:\dev\searchclarity
```

No arbitrary path access.
No user-profile sweeps.
No cloud-drive crawling.
No owner-private oracle material.

## Expected checkpoints

Verify these first and report mismatches.

```text
docs:
  path: C:\dev\kaizen-docs
  branch: main
  expected HEAD: 1f5871cddfede27c89c6b1ab82a0d31b4931ee5b
  expected status: clean
  expected remote posture: ahead of origin by about 41, behind 0

platform:
  path: C:\dev\kaizen\platform
  branch: main
  expected HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
  expected status: clean
  expected remotes: none

vault:
  path: C:\dev\kaizen\vault
  branch: main
  expected HEAD: c898f261c0b341eb8419125247c8bd53ef567d6c
  expected status: clean
  expected remotes: none

neon_ronin:
  path: C:\dev\neon-ronin
  branch: main
  expected HEAD: 7bbe64f2aec80cd7498e7623f59e68dd2493cb80
  expected status: clean
  expected remote posture: ahead of origin by 2, behind 0, unless owner changed it
```

## Postgres credential posture

Postgres credentials may be supplied locally by the owner through environment variables or local server config.

Do not request that credentials be pasted into repository files or audit reports.
Do not accept passwords as MCP tool arguments if avoidable.
Do not print or log credential values.
Do not include credential values in your final report.

Known relevant roles from the current Kaizen Postgres posture:

```text
postgres                 superuser login
kaizen_ops_migrator      login
kaizen_ops_test_migrator login
kaizen_ops_service       login, SELECT plus INSERT on operational evidence tables
kaizen_ops_readonly      login
kaizen_ops_backup        login
kaizen_ops_owner         no-login owner role
```

Preferred audit credential order:

```text
1. kaizen_ops_readonly for inspection if available.
2. kaizen_ops_service only if readonly access is insufficient to inspect operational evidence posture.
3. postgres superuser only as a last-resort inspection credential.
```

Important:

```text
kaizen_ops_service is not purely read-only. It can INSERT into some operational evidence tables.
For this audit, treat kaizen_ops_service as inspect-only.
Do not run INSERT, UPDATE, DELETE, DDL, migrations, grants, role changes, arbitrary SQL, or service writes.
```

If database access is unavailable or too expensive to configure, continue the audit and mark DB confidence limited rather than blocking the entire audit.

## Required files to read

Read the three instruction files first:

```text
03-research-results/369-kaizen-v1-post-acceptance-claude-adversarial-audit-prompt.md
03-research-results/370-kaizen-v1-post-acceptance-claude-mcp-tool-requirements.md
03-research-results/371-claude-mcp-build-and-audit-quickstart.md
```

Then read these core evidence files:

```text
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md
03-research-results/367-packet-020l-kaizen-v1-completion-audit-and-owner-acceptance-readiness.md
03-research-results/366-packet-020k-backup-restore-proof-result.md
03-research-results/365-packet-020k-backup-restore-proof-plan-and-evidence-request.md
03-research-results/364-packet-020j-stage-b-closure-audit-and-sequence-reconciliation.md
03-research-results/363-packet-020j-stage-b-implementation-return.md
03-research-results/361-packet-020j-stage-b-task-packet-neon-ronin-reference-example-boundary-clarification.md
03-research-results/358-packet-020h-fresh-context-resumption-proof.md
03-research-results/354-packet-020e-stage-a-repo-reconciliation-report.md
03-research-results/355-packet-020f-parent-child-and-observatory-boundary-candidate.md
03-research-results/349-packet-020c-repo-state-pinning-and-idea-only-firewall.md
03-research-results/345-packet-020a-m14-workload-selection-and-boundary-registration.md
```

Use search to inspect other supporting records as needed.

## Required searches

Run these in docs root:

```text
Kaizen v1
Milestone 14
owner acceptance
draft for owner review
Neon Ronin
SearchClarity
Observatory
IMI
Obsidian
vault
kaizen-docs
backup restore
Git push
ahead
020K
020L
```

Run these in neon_ronin root:

```text
SearchClarity captures the signal
M14 Reconciled Boundary
Kaizen governs the project-intelligence
Observatory
IMI
```

## Backup artifact checks

Inspect this backup root:

```text
C:\dev\kaizen-backup-work\m14-20260622-153245
```

Expected bundle hashes:

```text
kaizen-docs:
D8DD597956339ED7384EDB8A99728727E8C2C644A21AFE2A769081A0862A6FC0

neon-ronin:
95F1A1B59D84BF005040928B04DD9CEB94291D95F9E8ED49E6DD55E911ABB64B
```

Expected restored Neon Ronin commits:

```text
7bbe64f Fix SearchClarity boundary markdown fence
b890d8d Clarify SearchClarity reference boundary
```

## Audit emphasis

Hammer these questions:

```text
Does accepted M14 really satisfy the exit criteria?
Are any claims overstated?
Are stale docs contradicting accepted v1?
Is owner acceptance recorded after readiness audit?
Is kaizen-docs correctly framed as build/proof workbench rather than long-term Neon Ronin truth?
Is the Kaizen Obsidian Vault correctly framed as the intended canonical living project-intelligence layer?
Are Kaizen, Neon Ronin, SearchClarity, and Observatory / IMI boundaries preserved?
Does 020K backup/restore proof pass despite the Neon Ronin line-ending normalization note?
Are docs ahead-by-41 and Neon Ronin ahead-by-2 blockers or post-v1 sync tasks?
Are there secret, DB, restore, or repo-state safety issues?
```

## Required final verdict

Return exactly one:

```text
PASS
PASS WITH REQUIRED FIXES
FAIL
```

Use FAIL only for acceptance-invalidating defects.

## Required final report

Use the report structure from file 369.

Separate findings into:

```text
acceptance-invalidating blockers;
required fixes before serious post-v1 usage;
deferred hardening;
optional cleanup.
```

Do not mutate anything.
Do not hide uncertainty.
Do not reveal secrets.
```

## Owner note

Give this prompt to Claude after making sure his local MCP build environment can see the listed roots.

Supply any Postgres password only through local environment variables or private local configuration, never in the prompt text, repo files, or final report.
