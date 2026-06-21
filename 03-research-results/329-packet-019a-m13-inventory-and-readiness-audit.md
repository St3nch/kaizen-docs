# Packet 019A — M13 Inventory and Readiness Audit

Status: implementation-free inventory and readiness audit
Date: 2026-06-21
Packet: 019A
Milestone: 13

## 1. Purpose

Packet 019A inventories the current operational service and local tool surface so Milestone 13 can harden the right things instead of guessing.

This packet is read-only / planning-only. It does not authorize platform mutation, database mutation, vault mutation, staging mutation, or implementation work.

## 2. Starting checkpoint

Docs repository:

```text
HEAD before 019A inventory work: 90ea20daafe95ddbe08396a5b33cdfd960f72a90
branch: main
working tree before M13 definition acceptance: clean
```

Platform repository:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

M13 definition accepted in:

```text
03-research-results/328-milestone-13-definition-owner-acceptance.md
SHA-256: fea2dfe78458882e339d2646273b3086743ab5fcc2ca05bd70858f4310578210
```

## 3. Source files inspected

### Platform repository layout

Relevant platform files and directories observed:

```text
src/kaizen/ops_service/
src/kaizen/ops_db/
src/kaizen/ops_recovery/
src/kaizen/operator_console.py
src/kaizen/operation_status.py
src/kaizen/operation_status_cli.py
src/kaizen/live_amendment_cli.py
src/kaizen/live_promotion_cli.py
src/kaizen/path_confinement.py
src/kaizen/windows_fs.py
pyproject.toml
```

### Operational service files

```text
src/kaizen/ops_service/contracts.py
SHA-256: d741e93a0de51868764310d4535ff50a781b7be99414a266f61b8532b3fa26bf

src/kaizen/ops_service/service.py
SHA-256: bc35554303ac29510fdc78310df5992b187dde96c668039baffbb5806067843e

src/kaizen/ops_service/repository.py
SHA-256: 781f493b4f1ac9f4e7d9da059186f9825d2a8ef5924b19be43cc94e8ffc44394
```

### Operator surface file

```text
src/kaizen/operator_console.py
SHA-256: faa0dba796d7fe99e0257f86ac84b1030c7b9d73861d12760a547d6227a76585
```

### Packaging / entry points

`pyproject.toml` validates and declares these scripts:

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

Dependencies:

```text
PyYAML>=6.0.2,<7
psycopg[binary]>=3.2,<4
```

## 4. Service-boundary inventory

### 4.1 Request envelope

`RequestEnvelope` currently requires:

```text
request_id
correlation_id
project_id
caller_class
service_actor_id
contract_version
requested_at with timezone
optional idempotency_key
```

Mutation envelopes require an idempotency key. Read envelopes do not.

### 4.2 Caller classes

Accepted caller classes in the service contract:

```text
owner_operator
steward_agent
implementation_agent
verification_runner
audit_agent
recovery_operator
```

### 4.3 Core outcomes

Service outcomes:

```text
succeeded
replayed
rejected
conflict
not_found
recovery_required
```

### 4.4 Execution attempt lifecycle

Execution attempt statuses:

```text
reserved
started
succeeded
failed
interrupted
cancelled
```

Lifecycle methods observed:

```text
reserve_execution_attempt
start_execution_attempt
finish_execution_attempt
list_project_attempts
get_execution_evidence_graph
```

Lifecycle hardening already present:

```text
start requires expected reserved status;
finish requires expected started status;
finish terminal status limited to succeeded / failed / interrupted / cancelled;
non-success finish requires bounded_outcome_code;
terminal execution attempts are immutable;
predecessor attempts must be terminal.
```

### 4.5 Verification run recording

Observed method:

```text
record_verification_run
```

Hardening already present:

```text
verification caller classes limited to verification_runner, audit_agent, recovery_operator;
execution attempt reference must exist;
storage root scope checked when raw evidence root is supplied;
verification timestamps require timezone;
finished_at may not precede started_at;
summary counts must be non-negative;
raw evidence root/path must be supplied together;
relative evidence path is validated.
```

### 4.6 Artifact provenance

Observed methods:

```text
record_artifact_provenance
verify_artifact_reference
```

Hardening already present:

```text
producer reference required: execution attempt or verification run;
storage root scope checked;
relative path validation rejects traversal and absolute/local drive forms;
commit identity validation required;
expected SHA-256 validation supported;
file hash verification happens before provenance assertion;
file state can become verified, missing, hash mismatch, or reference unresolvable.
```

### 4.7 Return manifest

Observed methods:

```text
freeze_return_manifest
link_manifest_authority_record
```

Hardening already present:

```text
manifest requires terminal execution attempt;
manifest inventory is normalized;
present files are hashed and byte length checked;
storage root scopes are checked;
version 1 cannot have predecessor;
later versions require predecessor;
predecessor must match same execution attempt and earlier version;
canonical authority link kind is limited to audit_pass, audit_fail, owner_acceptance, completion_record.
```

### 4.8 Idempotency and audit events

Observed repository behavior:

```text
idempotency claimed per project_id + capability + idempotency_key;
request_digest conflict rejects key reuse with changed request;
pending previous result returns recovery_required;
completed result can replay;
service audit event written on successful mutations;
serialization/deadlock/lock conflicts become retryable conflict responses;
other database failures collapse to recovery_required or service_unavailable depending path.
```

## 5. Local tool-surface inventory

### 5.1 Go8 tool surface

Observed Go8 posture from connected tool registry:

```text
63 tools
fixed named roots
no generic execution posture in health evidence
explicit Git tools with exact-path staging and diff checks
bounded file reads
line-range reads
create-only file write
optimistic SHA exact replacement
text-integrity scans
secret-exposure classifier
fixed console-script runners
allowlisted Postgres tools
```

High-value safe tools for M13:

```text
repository_snapshot
git_remote_status
git_status
git_diff
git_diff_check
git_add_exact_paths
git_commit_staged
git_show_commit
verify_exact_path_scope
verify_no_changes
read_file
read_line_range
write_new_text_file
replace_exact_text
sha256_file
sha256_paths
scan_changed_text_integrity
scan_text_integrity
validate_pyproject
inspect_console_parser
installed_entrypoint_status
run_test_profile
run_console_script
pg_health
pg_list_tables
pg_describe_table
pg_read_evidence_rows
```

### 5.2 Platform console scripts

`pyproject.toml` declares a bounded CLI surface, including:

```text
kaizen-operator
kaizen-ops-migrate
kaizen-operation-status
kaizen-promote-live
kaizen-amend-live
```

`kaizen-operator` help inspection succeeded and exposes only:

```text
inspect
approve
execute
recover
```

The operator console requires:

```text
operation_id
packet_id
plan SHA-256 for approve/execute/recover
human actor for approve/execute/recover
literal confirmations for execute / recover
```

Literal confirmations:

```text
PROMOTE-LIVE-EXACTLY-ONCE
RECOVER-LIVE-EXACTLY-ONCE
```

### 5.3 Tool-surface concerns observed

Current concerns for M13 hardening:

```text
Go8 has many safe tools, but M14 needs an approved subset / operator playbook.
Some console parser inspection can be blocked by connector safety checks; M13 should rely on documented help output where inspectable, not assume all parser introspection works.
Go8 warning shows Git may warn about LF-to-CRLF on add under current Windows config; M13 should document expected line-ending posture and confirm no harmful file mutation occurs.
Read/write safety is strong for docs, but platform implementation changes still require exact packet-level approval and tests.
```

## 6. M13 risk register

| Risk | Severity | Current evidence | Recommended handling |
|---|---:|---|---|
| R13-001 — Service exists but M14 approved-use contract is not written | Medium | OpsService methods exist; no M14 reliance contract yet | Packet 019D should define M14 readiness contract. |
| R13-002 — Local tool surface is broad even if bounded | Medium | 63 Go8 tools available | Packet 019C should define approved M14 tool subset. |
| R13-003 — Console parser inspection may be partially blocked | Low | `kaizen-operator` inspect worked; one migration parser inspect was blocked | Do not require parser inspection for every script; document allowlisted scripts and use tests/help where available. |
| R13-004 — Idempotency and lifecycle behavior are implemented but need focused proof | High | Contracts/repository show logic; M13 needs synthetic proof | Packet 019B/019D should run focused tests or add tests if needed. |
| R13-005 — File/object provenance behavior is powerful and must stay project-scoped | High | Storage root scope and relative path checks exist | Include cross-project and traversal rejection proof in M13. |
| R13-006 — M14 may need service role / restore-compatible smoke clarity | Medium | M12 deferred role-ready restore; service smoke was bounded | Packet 019D should state what M14 may rely on and what remains deferred. |
| R13-007 — No direct SQL rule must survive convenience pressure | High | Go8 has allowlisted PG inspection/migration tools; service uses repository layer | M13 must prohibit arbitrary SQL APIs and direct agent credentials. |
| R13-008 — M13 can sprawl into UI/Hermes/Qdrant/Observatory | High | Roadmap forbids these; user wants headway | Keep M13 limited to service/tool hardening and readiness proof. |

## 7. M14 readiness gaps

M14 is not ready until M13 produces:

```text
approved operational service subset for real internal project work;
approved Go8/local tool subset and operator playbook;
focused proof for lifecycle, idempotency, artifact provenance, manifest freezing, and read graph behavior;
clear stop/escalation conditions;
backup/restore compatibility note for M14 data;
evidence capture checklist for the real internal project run;
active reading-path refresh requirement at M13 closeout.
```

## 8. Recommended M13 packet sequence

### 019B — Service-boundary proof and hardening packet

Goal:

```text
Prove or harden OpsService lifecycle, idempotency, artifact provenance, manifest, authority-link, and read-graph behavior.
```

Recommended proof areas:

```text
reserve/start/finish valid lifecycle;
invalid lifecycle rejected;
terminal mutation rejected;
idempotent replay works;
idempotency conflict rejects changed request;
project/repository/storage-root mismatch rejects;
artifact path traversal rejects;
manifest predecessor/version rules reject invalid inputs;
read graph respects project and collection limits;
bounded error codes remain stable.
```

Implementation authorization required before code/test changes.

### 019C — Local tool-surface approval packet

Goal:

```text
Define and, if needed, harden the approved local tool subset for M14.
```

Likely outputs:

```text
approved Go8 tool subset;
operator playbook;
blocked tools / not-for-M14 list;
line-ending and Git hygiene note;
console script allowlist;
exact path-scope verification rule.
```

Implementation authorization required before tool code changes.

### 019D — Synthetic proof and M14 readiness contract

Goal:

```text
Run focused proof and write the M14 readiness contract.
```

Likely outputs:

```text
focused test results;
service/tool proof evidence;
M14 readiness contract;
remaining deferrals;
M13 closure audit inputs.
```

## 9. Claude review target

Owner requested Claude review when the Milestone 13 portion is ready.

Recommended Claude review point:

```text
after 019A inventory is accepted and 019B/019C/019D packet plan is drafted;
before authorizing implementation-heavy M13 work.
```

Claude should review:

```text
M13 definition;
019A inventory;
proposed 019B/019C/019D packet sequence;
risk register;
M14 readiness gaps;
non-authorization boundaries.
```

## 10. 019A verdict

Verdict:

```text
PASS — READY TO PLAN 019B/019C/019D
```

The service and tool surfaces are mature enough to plan focused hardening. The main work remaining is not broad architecture. It is proof, approved-use contracts, and a narrow implementation sequence.

## 11. Non-authorization

Packet 019A does not authorize:

```text
platform mutation
vault mutation
staging mutation
database mutation
migration changes
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
Tauri / broad UI
production MCP packaging
production deployment
retention deletion
physical evidence deletion
direct agent SQL
arbitrary SQL APIs
```
