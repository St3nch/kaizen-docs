# Claude MCP Build and Audit Quickstart — Kaizen v1 Post-Acceptance Audit

Status: quickstart ready
Date: 2026-06-22
Purpose: reduce Claude token spend before the Kaizen v1 adversarial audit

## 1. One-line mission for Claude

```text
Build or configure a small read-only MCP audit server with the exact roots and tools below, verify the checkpoints, then run the post-acceptance adversarial audit in file 369 using file 370 as the detailed tool requirements reference.
```

## 2. Do not spend time designing architecture

Use a simple MCP server.

Recommended implementation shape:

```text
Language: Python
Framework: FastMCP or equivalent MCP framework
Tool style: small fixed-root functions
Execution posture: read-only / no mutation
Shell access: only bounded git/read/hash commands against fixed roots
```

Do not build:

```text
agent orchestration;
database migration runner;
write tools;
approval tools;
push tools;
cloud tools;
generic shell;
web scraper;
Obsidian writer;
repair system.
```

## 3. Fixed roots

Hardcode these roots:

```text
docs        = C:\dev\kaizen-docs
platform    = C:\dev\kaizen\platform
vault       = C:\dev\kaizen\vault
neon_ronin  = C:\dev\neon-ronin
backup_work = C:\dev\kaizen-backup-work
```

Optional only if easy:

```text
staging       = C:\dev\kaizen\staging
searchclarity = C:\dev\searchclarity
```

Do not allow arbitrary paths.

## 4. Minimum viable MCP tool set

Build these tools first. This is enough to do the audit.

### 4.1 Repository tools

```text
repo_status(root)
repo_log(root, count=10)
repo_show_commit(root, commit='HEAD', include_diff=false)
repo_remote_status(root)
repo_diff_check(root)
repo_changed_paths(root)
repo_verify_clean(root)
```

Implementation notes:

```text
repo_status: git status --short --branch plus git rev-parse HEAD
repo_log: git log --oneline -n <count>
repo_show_commit: git show --stat --name-only, optional bounded diff
repo_remote_status: git branch -vv plus git remote -v
repo_diff_check: git diff --check
repo_changed_paths: git status --short parsed into staged/unstaged/untracked
repo_verify_clean: return clean true/false, do not modify
```

### 4.2 File tools

```text
list_dir(root, path='.')
file_manifest(root, path='.', max_depth=3)
read_file(root, path)
read_lines(root, path, start, end)
search_text(root, query, path='.', regex=false)
sha256_file(root, path)
scan_text_integrity(root, path)
```

Implementation notes:

```text
Refuse paths escaping the fixed root.
Limit file size for read_file.
Search only text files.
scan_text_integrity reports BOM, bidi controls, zero-width characters, NBSP, CRLF/LF summary.
```

### 4.3 Backup proof tools

```text
list_backup_files(path_under_backup_work)
hash_file_under_backup_work(path)
git_bundle_verify(path_under_backup_work)
inspect_restored_repo(path_under_backup_work)
```

Required backup scope:

```text
C:\dev\kaizen-backup-work\m14-20260622-153245
```

Implementation notes:

```text
Only inspect files under backup_work.
Do not recreate bundles.
Do not delete restore directories.
Do not clone again unless explicitly needed; existing restore proof is already present.
```

### 4.4 Secret posture tool

```text
secret_scan_summary(root, paths=null)
```

Output only:

```text
path;
rule category;
severity;
count.
```

Never output:

```text
secret value;
token;
DSN;
surrounding text.
```

## 5. Optional tools only if cheap

```text
pg_health()
pg_list_databases()
pg_list_roles()
pg_list_tables(database, schema='operations')
pg_read_evidence_rows(database, table, limit=50)
venv_status(root)
pyproject_summary(root)
```

Do not let optional tools delay the audit.

## 6. Explicitly forbidden tools

Do not build or expose:

```text
write_file
replace_text
mkdir
delete_file
move_file
git_add
git_commit
git_push
git_pull
git_checkout
git_branch_create
git_branch_delete
git_stash_apply
git_stash_pop
run_migration
create_database
drop_database
create_role
grant_privilege
arbitrary_sql
cloud_upload
cloud_delete
process_kill
service_restart
package_install
```

## 7. Checkpoints to verify immediately

Verify these before reading the audit files.

```text
docs:
  path: C:\dev\kaizen-docs
  expected branch: main
  expected HEAD: b53848f7f13a58bbf68cb88c1dce2d81d7a0711f
  expected status: clean

platform:
  path: C:\dev\kaizen\platform
  expected branch: main
  expected HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
  expected status: clean

vault:
  path: C:\dev\kaizen\vault
  expected branch: main
  expected HEAD: c898f261c0b341eb8419125247c8bd53ef567d6c
  expected status: clean

neon_ronin:
  path: C:\dev\neon-ronin
  expected branch: main
  expected HEAD: 7bbe64f2aec80cd7498e7623f59e68dd2493cb80
  expected status: clean
  expected remote posture: main ahead of origin by 2, unless owner changed it
```

If a checkpoint mismatches, continue the audit but mark confidence reduced and report the mismatch.

## 8. Audit files to read

Read these from docs root:

```text
03-research-results/369-kaizen-v1-post-acceptance-claude-adversarial-audit-prompt.md
03-research-results/370-kaizen-v1-post-acceptance-claude-mcp-tool-requirements.md
03-research-results/368-milestone-14-kaizen-v1-owner-acceptance.md
03-research-results/367-packet-020l-kaizen-v1-completion-audit-and-owner-acceptance-readiness.md
03-research-results/366-packet-020k-backup-restore-proof-result.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
00-entrypoint/LLM_START_HERE.md
ROADMAP_V0.4.md
```

Then use search to inspect supporting records 343 through 365 as needed.

## 9. Searches to run

Run these searches in docs root:

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

Run these searches in neon_ronin root:

```text
SearchClarity captures the signal
M14 Reconciled Boundary
Kaizen governs the project-intelligence
Observatory
IMI
```

## 10. Backup artifact checks

Inspect:

```text
backup root: C:\dev\kaizen-backup-work\m14-20260622-153245
restore root: C:\dev\kaizen-backup-work\m14-20260622-153245\restore-proof
```

Expected bundle hashes:

```text
kaizen-docs bundle:
D8DD597956339ED7384EDB8A99728727E8C2C644A21AFE2A769081A0862A6FC0

neon-ronin bundle:
95F1A1B59D84BF005040928B04DD9CEB94291D95F9E8ED49E6DD55E911ABB64B
```

Expected restored Neon Ronin commits:

```text
7bbe64f Fix SearchClarity boundary markdown fence
b890d8d Clarify SearchClarity reference boundary
```

## 11. Audit report

After tool setup and inspection, produce the report required by file 369.

Final verdict must be exactly one of:

```text
PASS
PASS WITH REQUIRED FIXES
FAIL
```

Use FAIL only for acceptance-invalidating defects.

## 12. Token-saving instruction

Do not explain MCP theory.
Do not debate architecture.
Do not overbuild the server.
Build the minimum read-only tool set, verify the checkpoints, then audit.
