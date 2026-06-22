# Packet 019C — Local Tool-Surface Approval

Status: implementation return / approval record
Date: 2026-06-22
Packet: 019C
Milestone: 13

## 1. Purpose

This record approves the local tool surface Kaizen may rely on during Milestone 14's real internal project run.

019C is documentation-only. It does not change platform code, Go8 code, vault files, database state, staging evidence, or production-like evidence.

## 2. Starting checkpoint

Docs repository:

```text
root: docs
branch: main
starting HEAD: 61d670fb2a8a24b98a5c20cbe1d0cd0239760f0c
working tree: clean
upstream: origin/main
ahead/behind: 9 / 0
```

Platform repository:

```text
root: platform
branch: main
starting HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

Task packet:

```text
03-research-results/331-packet-019c-local-tool-surface-approval-task-packet.md
SHA-256: 56cc184ea20c40bbd55763fe42220143fb31e72f547a443f0aa280526126b9d0
```

## 3. Evidence inspected

Go8 health evidence:

```text
service: chatgpt-mcp
version: 0.5.0
framework: fastmcp-3.4.2
roots: chatgpt_mcp, docs, kaizen_mcp, northstar, pilot_evidence, platform, staging, vault
tool_count: 63
generic_execution: false
```

Platform packaging evidence:

```text
project: kaizen-platform
python requirement: >=3.11,<3.12
pyproject valid: true
```

Declared console scripts:

```text
kaizen-id
kaizen-validate
kaizen-path-check
kaizen-stage-create
kaizen-promote
kaizen-promote-live
kaizen-amend-live
kaizen-operation-status
kaizen-operator
kaizen-ops-migrate
```

`kaizen-operator` parser inspection:

```text
subcommands: inspect, approve, execute, recover
options: --format, --help
returncode: 0
```

## 4. Approved M14 local tool classes

### 4.1 Read / orientation tools

Approved for M14 planning, orientation, and evidence gathering:

```text
health
list_roots
repository_snapshot
git_remote_status
git_status
git_log
git_show_commit
list_directory
file_tree_manifest
read_file
read_line_range
sha256_file
sha256_paths
search_text
scan_text_integrity
scan_changed_text_integrity
validate_pyproject
venv_status
installed_entrypoint_status
inspect_console_parser
```

Conditions:

```text
Use fixed named roots only.
Read only the files needed for the active task.
Summarize evidence instead of copying excessive private file content into chat.
Do not use reads to expose secrets or private payloads.
```

### 4.2 Git safety tools

Approved under accepted packet scope:

```text
git_diff
git_diff_check
git_staged_paths
git_add_exact_paths
git_commit_staged
verify_exact_path_scope
verify_no_changes
```

Conditions:

```text
Verify clean starting state.
Stage exact paths only.
Never use implicit broad staging.
Run diff-check before commit.
Record changed paths and commit IDs.
Verify clean final state.
```

Git push is not approved by default. Push requires explicit owner instruction.

### 4.3 Safe create / replace tools

Approved under accepted packet scope:

```text
write_new_text_file
replace_exact_text
ensure_final_newline
```

Conditions:

```text
Use create-only writes for new docs/evidence files.
Use optimistic SHA exact replacement for existing text files.
Do not use broad overwrite when exact replacement is sufficient.
Do not use these tools for binary, Office, PDF, backup archive, or production-like evidence files.
Record before/after SHA-256 where relevant.
```

### 4.4 Test and package tools

Approved under accepted packet scope:

```text
run_test_profile
run_coverage_profile
run_typecheck_profile
run_package_import_check
run_python_module
run_console_script
run_editable_install
```

Conditions:

```text
Use allowlisted modules, scripts, and profiles.
Do not use arbitrary shell.
Do not use ad hoc Python code execution.
Do not perform network dependency resolution unless explicitly approved.
Record command/profile and output summary.
Treat environment-gated skips as deferrals, not hidden passes.
```

### 4.5 PostgreSQL inspection tools

Approved for read-only inspection under accepted packet scope:

```text
pg_health
pg_server_isready
pg_list_databases
pg_describe_database
pg_list_roles
pg_list_extensions
pg_list_tables
pg_describe_table
pg_list_table_grants
pg_list_column_grants
pg_list_triggers
pg_read_evidence_rows
pg_reconcile_retention_plan_counts
```

Conditions:

```text
These tools may connect to the live kaizen_ops database when configured.
Do not expose DSNs, passwords, secrets, or private payloads.
Do not copy sensitive evidence rows into chat or broad reports.
Do not run broad SQL outside approved fixed inspection tools.
Do not mutate database state.
```

### 4.6 PostgreSQL mutation tools

The following are not approved by default and require exact owner approval per use:

```text
pg_create_database
pg_create_role
pg_create_extension
pg_grant_database_privilege
pg_run_migration_file
pg_drop_database
```

Conditions for any future approval:

```text
Bind exact database name.
Bind exact role/extension/privilege where relevant.
Bind exact migration file SHA-256 before execution.
Never pass passwords through tool-call payloads.
Never drop a database without dry-run evidence and exact destructive-scope approval.
```

### 4.7 Secret and evidence safety tools

Approved when relevant:

```text
classify_secret_exposure
staging_evidence_inventory
process_lock_status
```

Conditions:

```text
Secret classifier must not return matched secret values or surrounding text.
Staging evidence inventory is read-only unless a separate packet authorizes evidence mutation.
Process lock status is read-only; do not kill processes.
```

## 5. Operator console approval

`kaizen-operator` may be used under exact task-packet scope.

Approved posture:

```text
inspect: allowed for one immutable governed operation when needed.
approve: requires exact operation, packet ID, plan SHA-256, human actor, and approval basis.
execute: requires exact operation, packet ID, plan SHA-256, human actor, and exact live confirmation literal.
recover: requires exact operation, packet ID, plan SHA-256, human actor, and exact recovery confirmation literal.
```

Operator approve/execute/recover remain consequential mutation operations. They require explicit owner-approved packet scope and must not be inferred from general planning approval.

## 6. M14 Git hygiene rule

Every M14 docs/platform change must follow:

```text
1. Verify clean starting state.
2. Read before edit.
3. Search before create.
4. Use create-only or exact SHA replacement where possible.
5. Run diff and diff-check.
6. Stage exact paths only.
7. Commit narrow scope.
8. Verify clean final state.
9. Record changed paths, commits, and hashes.
10. Do not push unless owner explicitly says to push.
```

## 7. Line-ending warning handling

Go8 / Git may warn on Windows that LF will be replaced by CRLF when Git next touches a file.

Approved handling:

```text
Record line-ending warnings as evidence.
Do not treat line-ending warnings as automatic failure.
If byte preservation is part of the contract, check SHA-256 before and after.
If line endings affect an accepted contract or expected hash, stop and escalate.
```

## 8. Blocked by default for M14

The following are blocked by default:

```text
any tool outside fixed named roots;
any generic shell or broad command execution;
any arbitrary SQL;
any broad file overwrite;
any unbounded recursive file dump;
any deletion/cleanup tool unless packet-approved;
any database mutation tool without exact owner approval;
any Git push without explicit owner instruction;
any provider/customer-data collection tool;
any tool that changes production-like evidence or backup archives;
any tool that implements Qdrant, Hermes, Tauri, Observatory, Neon Ronin, or SearchClarity as part of M13/M14 without a future accepted packet.
```

## 9. Acceptance criteria check

```text
1. Approved M14 tool subset is explicit: yes.
2. Mutation tools are separated from read-only tools: yes.
3. Database mutation tools require exact owner approval: yes.
4. Push remains separate from commit: yes.
5. Operator console mutation commands remain exact-confirmation gated: yes.
6. No arbitrary shell or arbitrary SQL introduced: yes.
7. No provider/customer-data, Observatory, Qdrant, Hermes, Tauri, Neon Ronin, or SearchClarity work authorized: yes.
8. M14 Git hygiene rule recorded: yes.
9. Line-ending warning handling recorded: yes.
10. Tool-surface approval is suitable for M14 readiness contract reference: yes.
```

## 10. Result

019C verdict:

```text
PASS — LOCAL TOOL-SURFACE APPROVED FOR M14 READINESS CONTRACT USE
```

019C creates an approved local tool-surface playbook for M14 planning and future real internal project execution.

## 11. Platform modification result

No platform files were modified.

Platform final state at time of this record:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

## 12. Recommended next action

Proceed to Packet 019D synthetic proof and M14 readiness contract.

019D should reference:

```text
019B proof return for service-boundary proof status;
019C approval record for local tool-surface posture;
live-DB proof deferrals from 019B;
stop/escalation rules for M14.
```

## 13. Non-authorization

This record does not authorize:

```text
platform mutation
Go8 mutation
vault mutation
staging mutation
database mutation
SQL migration execution
new operational record families
governed-operation read model implementation
connector telemetry implementation
M14 project execution
Neon Ronin implementation
SearchClarity implementation
Observatory / IMI implementation
provider purchase
customer-data reuse
Qdrant
Hermes
Tauri
production deployment
retention deletion
physical evidence deletion
direct agent SQL
broad SQL access
broad command execution
Git push
```
