# Packet 019C — Local Tool-Surface Approval Task Packet

Status: task packet draft
Date: 2026-06-21
Packet: 019C
Milestone: 13
Scope class: documentation / tool-surface approval, with optional bounded tool hardening if separately approved
Authority lane: owner approval required before implementation or tool changes

## 1. Purpose

Packet 019C defines the approved local tool surface Kaizen may rely on during Milestone 14's real internal project run.

019A found that Go8 exposes a broad but bounded local tool registry. That breadth is useful, but M14 should not treat every available tool as approved-by-default. 019C creates a narrow operator playbook and approved tool subset.

## 2. Source bindings

```text
Milestone 13 definition:
05-specs/milestone-13-operational-service-and-safe-local-tool-surface-hardening.md
SHA-256: 861b1251fb09906e31ccf783d5f6945add7b2c2f378edb3dd48ee8b3df55b4e8

019A inventory:
03-research-results/329-packet-019a-m13-inventory-and-readiness-audit.md
SHA-256: 8cc6e5286f934f21640dae1ae3e6849f57108257d2c6a0830c9c827c6d314b73

M13 owner acceptance:
03-research-results/328-milestone-13-definition-owner-acceptance.md
SHA-256: fea2dfe78458882e339d2646273b3086743ab5fcc2ca05bd70858f4310578210
```

Relevant platform/operator source:

```text
src/kaizen/operator_console.py
SHA-256: faa0dba796d7fe99e0257f86ac84b1030c7b9d73861d12760a547d6227a76585

pyproject.toml
validated scripts: kaizen-id, kaizen-validate, kaizen-path-check, kaizen-stage-create, kaizen-promote, kaizen-promote-live, kaizen-amend-live, kaizen-operation-status, kaizen-operator, kaizen-ops-migrate
```

## 3. Starting checkpoint

Current draft checkpoint observed while preparing this packet:

```text
docs HEAD: 10a3d3b38860a55a3f21119e1508ef5b7789672f
platform HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
platform working tree: clean
```

Execution must re-verify or use a later owner-approved starting commit.

## 4. Authorized objective

019C should produce a local tool-surface approval record for M14 and identify any hardening gaps.

The approval record should answer:

```text
Which tools may a steward use during M14?
Which tools are read-only / planning-safe?
Which tools require explicit owner approval each time?
Which tools are blocked for M14?
Which tool outputs must be recorded as evidence?
Which Git hygiene steps are mandatory?
Which file/database/secret boundaries apply?
```

## 5. Proposed M14-approved tool classes

### 5.1 Safe read / orientation tools

Recommended approved for M14 planning and evidence gathering:

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
Do not use reads to exfiltrate secrets or private payloads.
Summarize evidence rather than copying excessive file content into chat.
```

### 5.2 Git safety tools

Recommended approved for M14 docs/platform work when scoped by an accepted packet:

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
Stage exact paths only.
Never use implicit broad staging.
Run diff-check before commit.
Verify exact path scope before and/or after commit.
Record commit IDs and changed paths in implementation return.
```

### 5.3 Safe create / replace tools

Recommended approved only under accepted packet scope:

```text
write_new_text_file
replace_exact_text
ensure_final_newline
```

Conditions:

```text
Use create-only for new docs/evidence files.
Use optimistic SHA exact replacement for existing files.
Do not overwrite existing files blindly.
Do not write binary or office documents.
Record before/after SHA-256.
```

### 5.4 Test and package inspection tools

Recommended approved under accepted packet scope:

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
Use predefined/allowlisted profiles or modules only.
No arbitrary shell.
No ad hoc Python -c execution.
No network dependency resolution unless separately approved.
Record command/profile and output summary.
```

### 5.5 PostgreSQL inspection tools

Recommended approved for read-only or explicit migration tasks only:

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
Read-only inspection is allowed under planning/inventory scope.
Do not expose DSNs or passwords.
Do not run arbitrary SQL.
Do not mutate live kaizen_ops unless exact owner-approved packet scope requires it.
```

### 5.6 PostgreSQL mutation tools

Tools such as the following require exact separate owner approval:

```text
pg_create_database
pg_create_role
pg_create_extension
pg_grant_database_privilege
pg_run_migration_file
pg_drop_database
```

Conditions:

```text
Use only with explicit packet authorization.
Bind migration file SHA-256.
Never pass passwords through tool calls.
Never drop databases unless dry-run plus exact confirmation protocol and owner-approved destructive scope exist.
```

### 5.7 Secret and evidence safety tools

Recommended approved when relevant:

```text
classify_secret_exposure
staging_evidence_inventory
process_lock_status
```

Conditions:

```text
Secret classifier must not return matched secret values or surrounding text.
Use staging inventory without mutating evidence.
Do not kill processes from process inventory.
```

## 6. Tools not approved by default for M14

The following categories are not approved by default:

```text
any tool outside fixed named roots;
any generic shell or arbitrary command execution;
any arbitrary SQL;
any broad file overwrite;
any unbounded recursive file dump;
any deletion/cleanup tool unless packet-approved;
any database mutation tool without exact owner approval;
any push tool unless owner explicitly approves push;
any provider/customer-data collection tool;
any tool that changes production-like evidence or backup archives.
```

## 7. Operator console posture

`kaizen-operator` currently exposes:

```text
inspect
approve
execute
recover
```

Required use rules:

```text
inspect is read-oriented and may be used to evaluate one immutable operation.
approve requires exact operation, packet ID, plan SHA-256, human actor, and approval basis.
execute requires exact operation, packet ID, plan SHA-256, human actor, and PROMOTE-LIVE-EXACTLY-ONCE confirmation.
recover requires exact operation, packet ID, plan SHA-256, human actor, and RECOVER-LIVE-EXACTLY-ONCE confirmation.
```

M14 should not use operator approve/execute/recover casually. These remain consequential mutation operations and require exact task-packet scope.

## 8. Git hygiene rule for M14

Every M14 docs/platform change must follow:

```text
1. Verify clean starting state.
2. Read before edit.
3. Search before create.
4. Use create-only or exact SHA replacement.
5. Run diff and diff-check.
6. Stage exact paths only.
7. Commit narrow scope.
8. Verify clean final state.
9. Record changed paths, commits, and hashes.
```

Push remains separate. Do not push merely because a local commit exists.

## 9. Line-ending warning handling

Go8 / Git may warn on Windows that LF will be replaced by CRLF when Git next touches a file.

M14 rule:

```text
Treat line-ending warnings as evidence to record, not as automatic failure.
If a packet requires byte preservation, line-ending behavior must be checked with SHA-256 before and after.
If line endings materially affect a contract, stop and escalate.
```

## 10. Acceptance criteria

019C is acceptable only if:

```text
1. Approved M14 tool subset is explicit.
2. Mutation tools are separated from read-only tools.
3. Database mutation tools require exact owner approval.
4. Push remains separate from commit.
5. Operator console mutation commands remain exact-confirmation gated.
6. No arbitrary shell or arbitrary SQL is introduced.
7. No provider/customer-data, Observatory, Qdrant, Hermes, Tauri, Neon Ronin, or SearchClarity work is authorized.
8. M14 Git hygiene rule is recorded.
9. Line-ending warning handling is recorded.
10. Tool-surface approval is suitable for Claude review.
```

## 11. Deliverable

019C implementation should create one approval/playbook record, likely:

```text
03-research-results/333-packet-019c-local-tool-surface-approval.md
```

If no tool code changes are needed, platform repository should remain unchanged.

## 12. Claude review hook

Claude should review 019C together with 019A and 019B.

Specific Claude questions:

```text
Is the approved tool subset too broad, too narrow, or missing a critical category?
Are mutation boundaries strong enough?
Is push/separate-approval handling sufficient?
Are PostgreSQL read/mutation boundaries clear?
Would this tool surface be safe enough for M14 real internal work?
```

## 13. Non-authorization

This task packet draft does not authorize implementation by itself.

It does not authorize:

```text
platform mutation without exact approval;
Go8 mutation;
database mutation;
SQL migration execution;
new operational record families;
governed-operation read model implementation;
connector telemetry implementation;
M14 project execution;
Neon Ronin implementation;
SearchClarity implementation;
Observatory / IMI implementation;
provider purchase;
crawling;
logged-in marketplace collection;
customer-data reuse;
Qdrant;
Hermes;
Tauri or broad UI;
production MCP packaging;
production deployment;
retention deletion;
physical evidence deletion;
direct agent SQL;
arbitrary SQL APIs;
Git push without explicit owner approval.
```

## 14. Owner approval wording

Suggested owner approval phrase:

```text
Approve Packet 019C local tool-surface approval implementation using docs starting commit <DOCS_COMMIT> and task packet SHA-256 <TASK_PACKET_SHA>. Scope is limited to creating the M14 local tool-surface approval/playbook record. No platform, Go8, vault, database, staging, provider/customer-data, Observatory, Qdrant, Hermes, Tauri, Neon Ronin, SearchClarity, production deployment, M14 execution, arbitrary SQL, arbitrary shell, or Git push is authorized.
```
