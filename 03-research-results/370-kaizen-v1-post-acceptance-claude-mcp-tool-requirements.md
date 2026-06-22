# Kaizen v1 Post-Acceptance Claude MCP Tool Requirements

Status: tool-requirements prompt ready
Date: 2026-06-22
Packet: post-v1 audit support

## 1. Purpose

Define the MCP server/tool surface Claude should build or expose before performing the Kaizen v1 post-acceptance adversarial audit.

This is not a request for Claude to use ChatGPT's Go8 server directly.

Claude may build or configure its own MCP server, but the server must provide bounded, read-heavy, evidence-producing tools and must not expose mutation capabilities for this audit.

## 2. Top-level instruction for Claude

```markdown
You may build or configure your own MCP server for this audit.
The server must be designed for adversarial inspection of the accepted Kaizen v1 record.
It must be read-only by default.
Do not include tools that mutate repositories, databases, files, vault notes, staging evidence, remotes, cloud state, or operating-system state.

The goal is to hammer Kaizen v1 with strong evidence, not to repair it during the audit.
```

## 3. Required roots

Configure fixed, explicit roots only.

Required roots:

```text
docs = C:\dev\kaizen-docs
platform = C:\dev\kaizen\platform
vault = C:\dev\kaizen\vault
neon_ronin = C:\dev\neon-ronin
```

Optional roots:

```text
staging = C:\dev\kaizen\staging
kaizen_mcp = C:\dev\kaizen-mcp
chatgpt_mcp = C:\dev\chatgpt-mcp
searchclarity = C:\dev\searchclarity
backup_work = C:\dev\kaizen-backup-work
```

Rules:

```text
No arbitrary path access.
No user profile sweeps.
No secret directories.
No cloud-drive crawling unless separately approved.
No owner-private oracle material.
```

## 4. Required tool categories

Claude's MCP server should provide these tool categories.

### 4.1 Root and repository inventory

Required tools:

```text
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
```

Purpose:

```text
Verify current state, HEADs, clean/dirty status, ahead/behind state, changed paths, commit metadata, and diff hygiene.
```

Must support these roots at minimum:

```text
docs
platform
vault
neon_ronin
```

### 4.2 File inspection and search

Required tools:

```text
file_tree_manifest
list_directory
read_file
read_line_range
search_text
sha256_file
sha256_paths
scan_text_integrity
scan_changed_text_integrity
```

Purpose:

```text
Read required audit files, discover stale docs, inspect line ranges, search for contradictions, compute hashes, and detect hidden Unicode / BOM / bidi / suspicious whitespace issues.
```

### 4.3 Safety and secret posture

Required tools:

```text
classify_secret_exposure
secret_scan_summary
```

The tool must not return secret values or surrounding secret text.

Allowed output:

```text
file path;
rule category;
severity;
count;
recommendation.
```

Forbidden output:

```text
secret value;
credential text;
DSN value;
API key;
token;
surrounding lines that reveal the secret.
```

### 4.4 Backup and restore evidence inspection

Required tools:

```text
list_backup_artifacts
hash_backup_artifacts
verify_git_bundle
inspect_restored_repo
compare_restored_head
```

Required scope:

```text
C:\dev\kaizen-backup-work\m14-20260622-153245
```

Purpose:

```text
Verify the 020K backup bundles exist, match recorded hashes, verify as Git bundles, and restore evidence matches accepted records.
```

The server may use Git read-only commands for bundle verification.
It must not delete, overwrite, or recreate backup artifacts during this audit.

### 4.5 Database inspection, if available

Required read-only Postgres tools, if database access is configured:

```text
pg_health
pg_server_isready
pg_list_databases
pg_list_roles
pg_list_tables
pg_list_extensions
pg_describe_database
pg_describe_table
pg_list_table_grants
pg_list_column_grants
pg_read_evidence_rows
pg_reconcile_retention_plan_counts
```

Purpose:

```text
Confirm platform operational DB posture where relevant to v1 proof and detect contradiction with recorded operational-data claims.
```

Database tools must be read-only.
They must not accept arbitrary SQL.
They must not accept passwords or secrets as tool arguments.

### 4.6 Process and environment inspection

Optional tools:

```text
process_lock_status
venv_status
validate_pyproject
installed_entrypoint_status
inspect_console_parser
```

Purpose:

```text
Confirm expected project tooling and check whether stale or broken entrypoints contradict v1 acceptance.
```

These tools must not run project mutation commands.

## 5. Forbidden tool categories

Do not expose or use these during the audit:

```text
write file;
replace text;
create directory;
delete file;
move file;
stage files;
commit;
push;
pull with merge;
checkout with mutation;
branch create/delete;
stash apply/pop;
run migration;
create/drop database;
create/drop role;
grant privileges;
arbitrary SQL;
cloud upload;
cloud delete;
process kill;
service restart;
package install;
network publish;
approval / execute / amend / promote operations.
```

If a mutation tool exists in the MCP server, disable it for the audit or do not call it.

## 6. Minimum repository checkpoints Claude must verify

### docs

```text
root: C:\dev\kaizen-docs
expected branch: main
expected HEAD: 6852f57706285a617463e5cdfb21c28d64b9aa70
expected status: clean
expected ahead/behind: ahead 39 / behind 0, or explain mismatch
```

### platform

```text
root: C:\dev\kaizen\platform
expected branch: main
expected HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
expected status: clean
expected remotes: none
```

### vault

```text
root: C:\dev\kaizen\vault
expected branch: main
expected HEAD: c898f261c0b341eb8419125247c8bd53ef567d6c
expected status: clean
expected remotes: none
```

### neon_ronin

```text
root: C:\dev\neon-ronin
expected branch: main
expected HEAD: 7bbe64f2aec80cd7498e7623f59e68dd2493cb80
expected status: clean
expected ahead/behind: ahead 2 / behind 0, or explain mismatch
```

## 7. Required project-boundary checks

Claude must use search/read tools to verify the accepted record preserves these distinctions:

```text
Kaizen = governed project-intelligence / implementation-return system.
Kaizen docs = temporary construction/proof/governance workbench for building Kaizen, not long-term downstream project truth.
Kaizen Obsidian Vault = intended canonical living project-intelligence layer for governed projects.
Neon Ronin = parent downstream business/workspace OS.
SearchClarity = child business/service lane under Neon Ronin.
Observatory / IMI = shared intelligence capability boundary, not implemented or finally owned yet.
```

Claude must flag any record that contradicts these distinctions.

## 8. Required v1 acceptance checks

Claude must verify:

```text
020L readiness audit exists and is after 020K backup/restore proof.
368 owner acceptance exists and is after 020L.
Owner acceptance references docs commit 2a76363d3257cd3230746dc4c5901965291591d8.
Post-acceptance audit prompt 369 exists after owner acceptance.
This MCP tool-requirements file 370 exists after 369.
```

Claude must not confuse post-acceptance audit prompt creation with missing acceptance.

## 9. Required output from Claude's MCP server setup

Before the audit report, Claude should report:

```text
MCP server name/version;
fixed roots configured;
read-only enforcement posture;
tool list actually exposed;
mutation tools disabled or not used;
checkpoint verification result;
any tool gaps that limit audit confidence.
```

## 10. Audit report expectations

After tool setup, Claude should run the adversarial audit using the report structure in:

```text
03-research-results/369-kaizen-v1-post-acceptance-claude-adversarial-audit-prompt.md
```

The final report must clearly separate:

```text
acceptance-invalidating blockers;
required fixes before serious post-v1 usage;
deferred hardening;
optional cleanup.
```

## 11. Recommended one-line instruction to Claude

```text
Build or configure a read-only MCP audit server with the capabilities in this tool-requirements file, verify the stated roots and checkpoints, then perform the Kaizen v1 post-acceptance adversarial audit from file 369. Do not mutate anything.
```
