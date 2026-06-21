# Packet 019B — Service-Boundary Proof and Hardening Task Packet

Status: task packet draft
Date: 2026-06-21
Packet: 019B
Milestone: 13
Scope class: platform proof / bounded hardening plan
Authority lane: owner approval required before platform mutation or test execution beyond read-only inspection

## 1. Purpose

Packet 019B defines the focused proof and bounded hardening work needed for Kaizen's operational service boundary.

The goal is not to redesign the operational database or add new record families. The goal is to prove that the accepted first-slice operational service behaves safely enough to support Milestone 14's real internal project run.

019B focuses on:

```text
execution-attempt lifecycle;
idempotency behavior;
verification-run recording;
artifact provenance and file reference verification;
return manifest freezing;
manifest authority links;
execution evidence graph reads;
project / repository / storage-root scoping;
bounded error outcomes.
```

## 2. Source bindings

```text
Milestone 13 definition:
05-specs/milestone-13-operational-service-and-safe-local-tool-surface-hardening.md
SHA-256: 861b1251fb09906e31ccf783d5f6945add7b2c2f378edb3dd48ee8b3df55b4e8

M13 owner acceptance:
03-research-results/328-milestone-13-definition-owner-acceptance.md
SHA-256: fea2dfe78458882e339d2646273b3086743ab5fcc2ca05bd70858f4310578210

019A inventory:
03-research-results/329-packet-019a-m13-inventory-and-readiness-audit.md
SHA-256: 8cc6e5286f934f21640dae1ae3e6849f57108257d2c6a0830c9c827c6d314b73

Decision 0019:
04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md
SHA-256: c8509d575df11816de4d7dc48cb9b0fb84ac523d4defb5919ccd384eb491731a

Decision 0020:
04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md
SHA-256: 8a5b915ba3db9b558f551caa34d191c1ee212af70f088c896b9169cfa593f845
```

Platform source bindings from 019A:

```text
src/kaizen/ops_service/contracts.py
SHA-256: d741e93a0de51868764310d4535ff50a781b7be99414a266f61b8532b3fa26bf

src/kaizen/ops_service/service.py
SHA-256: bc35554303ac29510fdc78310df5992b187dde96c668039baffbb5806067843e

src/kaizen/ops_service/repository.py
SHA-256: 781f493b4f1ac9f4e7d9da059186f9825d2a8ef5924b19be43cc94e8ffc44394
```

## 3. Starting checkpoint

Approved execution of this packet must bind to exact repository states at approval time.

Current draft checkpoint observed while preparing this packet:

```text
docs HEAD: 10a3d3b38860a55a3f21119e1508ef5b7789672f
platform HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
platform branch: main
platform working tree: clean
platform remotes: none
```

This checkpoint is informational until owner approval. Execution must re-verify or use a later owner-approved starting commit.

## 4. Authorized objective

019B implementation should prove the operational service boundary and apply only minimal hardening that is required by the proof.

Acceptable outcomes:

```text
existing tests/proofs pass with no code change;
focused tests are added to prove currently unproven behavior;
small service/repository hardening is added where proof exposes a real gap;
M14 service-readiness findings are recorded.
```

Unacceptable outcomes:

```text
broad service rewrite;
new operational record families;
new database domain;
new SQL migration unless a concrete proof gap requires it and owner explicitly approves;
new agent access model;
new arbitrary SQL interface;
new provider/customer-data capability;
Qdrant/Hermes/Tauri/Observatory work.
```

## 5. Required proof areas

### 5.1 Execution-attempt lifecycle

Required proof:

```text
reserve execution attempt succeeds for active project/repository;
start succeeds only from reserved;
finish succeeds only from started;
finish accepts succeeded / failed / interrupted / cancelled;
non-success finish requires bounded_outcome_code;
terminal execution attempt cannot be mutated again;
predecessor attempt must be terminal;
missing execution attempt returns not_found or bounded equivalent;
project/repository mismatch rejects.
```

### 5.2 Idempotency

Required proof:

```text
same idempotency key + same request replays completed result;
same idempotency key + changed request rejects with conflict;
pending/unknown idempotency state returns recovery_required;
all mutation requests require idempotency_key;
read requests do not require idempotency_key.
```

### 5.3 Verification run recording

Required proof:

```text
record_verification_run requires valid execution_attempt_id;
allowed caller classes are verification_runner, audit_agent, recovery_operator;
invalid caller class rejects;
finished_at before started_at rejects;
negative summary counts reject;
raw evidence storage root/path must be supplied together;
raw evidence path must be repository-relative;
storage root project mismatch rejects.
```

### 5.4 Artifact provenance and file reference verification

Required proof:

```text
record_artifact_provenance requires execution_attempt_id or verification_run_id;
producer references must exist and share project/attempt when both supplied;
storage root scope is enforced;
relative path traversal rejects;
absolute path / drive-like path rejects;
expected SHA-256 mismatch returns bounded file_hash_mismatch;
verified file creates or reuses logical artifact and byte object safely;
supersedence cannot cross logical artifacts;
supersedence cannot have multiple successors;
verify_artifact_reference updates file state for verified/missing/hash-mismatch/unresolvable cases.
```

### 5.5 Return manifest freezing

Required proof:

```text
freeze_return_manifest requires terminal execution attempt;
manifest inventory is normalized;
present manifest items are hashed and byte length checked;
version 1 rejects predecessor;
version >1 requires predecessor;
predecessor must match same execution attempt and earlier version;
storage root scope checked for present and expected missing items;
manifest freeze writes manifest and entries atomically;
manifest inventory digest is returned in response payload.
```

### 5.6 Manifest authority links

Required proof:

```text
link_manifest_authority_record requires valid return_manifest_id;
link_kind limited to audit_pass, audit_fail, owner_acceptance, completion_record;
invalid link_kind rejects at contract level;
manifest authority link is project-scoped.
```

### 5.7 Read graph and list attempts

Required proof:

```text
get_execution_evidence_graph requires allowed read caller class;
missing attempt returns not_found;
max_per_collection bounded 1..500;
expanded graph respects per-collection limit and truncation flags;
list_project_attempts requires active project;
list_project_attempts page_size bounded;
filters are project-scoped;
created_after/created_before timezone validation works;
created_after > created_before rejects.
```

### 5.8 Failure mapping and bounded responses

Required proof:

```text
validation failure maps to rejected;
conflict maps to conflict;
missing reference maps to not_found;
serialization/deadlock/lock conflicts map to retryable conflict;
unexpected database or service failure maps to recovery_required or service_unavailable;
responses do not expose DSNs, secrets, raw SQL, or private file contents.
```

## 6. Recommended implementation posture

Start with tests/proof before code changes.

Recommended order:

```text
1. Inventory current tests covering ops_service.
2. Run existing relevant tests.
3. Map existing tests to proof areas above.
4. Add focused missing tests only where gaps exist.
5. Apply minimal hardening only if tests expose a real gap.
6. Run focused test profile.
7. Run broader platform test profile if reasonable and bounded.
8. Record implementation return with proof matrix.
```

## 7. Candidate files authorized for modification after approval

Implementation may modify only files needed for service-boundary proof/hardening, likely within:

```text
tests/
src/kaizen/ops_service/contracts.py
src/kaizen/ops_service/service.py
src/kaizen/ops_service/repository.py
src/kaizen/ops_service/manifest.py
src/kaizen/ops_service/storage.py
src/kaizen/ops_service/errors.py
```

Documentation return files may be created under:

```text
03-research-results/
```

Any SQL migration file modification or creation requires explicit owner re-approval before execution because 019B is not intended to add new record families or perform schema redesign.

## 8. Files not authorized for modification

019B does not authorize modification of:

```text
vault repository files
staging evidence
Kaizen MCP files
Go8 files
Northstar files
backup archives
production-like evidence
existing accepted docs records except new implementation/audit/acceptance records
Qdrant/Hermes/Tauri/Observatory files
provider/customer-data files
```

## 9. Verification requirements

Implementation return must include:

```text
starting docs and platform commits;
changed paths;
before/after SHA-256 for each changed file;
proof matrix mapping tests to proof areas;
focused test command and output summary;
full/broader test command and output summary or exact bounded non-run explanation;
Git diff check result;
secret exposure scan result if any platform files changed;
final docs and platform commits;
final clean status for docs and platform.
```

## 10. Acceptance criteria

019B is acceptable only if:

```text
1. All required proof areas are covered by existing or new tests, or explicitly marked deferred with reason.
2. No new operational record family is added.
3. No arbitrary SQL API or direct agent SQL credential path is introduced.
4. No provider/customer-data, Observatory, Qdrant, Hermes, Tauri, Neon Ronin, or SearchClarity work occurs.
5. Service responses remain bounded and do not expose secrets or raw private payloads.
6. Project/repository/storage-root scoping is proven or any gap is fixed.
7. Idempotency conflict/replay/recovery behavior is proven.
8. Artifact path traversal and hash mismatch behavior is proven.
9. Manifest version/predecessor behavior is proven.
10. Git path scope is exactly as authorized.
11. Final working trees are clean.
```

## 11. Claude review hook

Owner requested Claude review when the Milestone 13 portion is ready.

019B should not be treated as final implementation authority until Claude has reviewed the M13 planning package, including:

```text
M13 definition;
019A inventory;
019B task packet;
019C/019D drafts if prepared;
non-authorization boundaries.
```

If implementation urgency requires proceeding before Claude review, the implementation packet must explicitly record owner approval to do so.

## 12. Non-authorization

This task packet draft does not authorize implementation by itself.

It does not authorize:

```text
platform mutation without exact approval;
database mutation without exact approval;
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
arbitrary SQL APIs.
```

## 13. Owner approval wording

Suggested owner approval phrase:

```text
Approve Packet 019B service-boundary proof implementation using docs starting commit <DOCS_COMMIT> and platform starting commit <PLATFORM_COMMIT>, with task packet SHA-256 <TASK_PACKET_SHA>. Scope is limited to focused ops_service proof tests and minimal hardening required by those tests. No new operational record families, SQL migration, arbitrary SQL API, provider/customer-data, Observatory, Qdrant, Hermes, Tauri, Neon Ronin, SearchClarity, production deployment, or M14 execution is authorized.
```
