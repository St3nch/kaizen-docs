---
id: kz-spec-01KV6LGQ8M2N7R5C3Y9P4T6BR0
type: specification
status: proposed
project: kaizen-platform
created: 2026-06-16T01:40:00Z
updated: 2026-06-16T01:40:00Z
review_status: pending
authority: proposed
summary: "Define deterministic concurrency, failure, migration, recovery, isolation, backup, and retention tests for the Milestone 11 first slice."
---

# Milestone 11 Hammer and Failure-Test Plan

## Purpose

Define the proof required before the first operational-data foundation may be accepted. This document designs tests; it does not run them.

## General rules

- every hammer profile uses a clean disposable database and storage root unless testing upgrade or recovery;
- random seeds, worker counts, start barriers, and expected distributions are recorded;
- connector timeout is not a test failure unless the test process reports failure;
- all unexpected rows, files, states, and audit events are failures;
- all tests verify project isolation and authority boundaries;
- failures preserve database, logs, and file evidence for audit;
- no profile may be weakened after implementation failure without owner-approved design revision.

## H1 - Exact idempotency contention

Run 100 rounds with at least 20 concurrent callers submitting one identical `reserve_execution_attempt` request and idempotency key.

Expected per round:

```text
1 created attempt
19 replayed responses referencing the same attempt
0 duplicates
0 conflicting results
1 idempotency record
1 mutation audit event
```

## H2 - Conflicting idempotency reuse

Run 100 rounds with two request bodies sharing one project/capability/idempotency key.

Expected:

```text
1 winning request
all different-body requests rejected as idempotency_conflict
no partial records from losers
```

Distribution must be deterministic with respect to one winner, not winner identity.

## H3 - Distinct concurrent attempts

Run 100 rounds with 20 distinct valid attempt requests in one project.

Expected:

```text
20 created attempts
unique external IDs
correct request bindings
no dead rows or cross-link contamination
```

## H4 - Cross-project isolation

Run 100 rounds attempting:

- child reference from project A to attempt in project B;
- retrieval of B records through A scope;
- storage-root reference across projects;
- predecessor link across projects;
- manifest entry to another project's verification run.

Expected: every attempt rejected before committed cross-project state.

## H5 - Execution lifecycle matrix

Test every state-transition pair.

Allowed:

```text
reserved -> started
reserved -> cancelled
started -> succeeded
started -> failed
started -> interrupted
started -> cancelled
```

Every other pair rejects with `invalid_state_transition` and leaves timestamps unchanged.

Concurrent terminal transitions run for 100 rounds. Exactly one terminal transition commits.

## H6 - Verification classification

For passed, failed, timed_out, blocked, transport_failed, and interrupted:

- validate required and prohibited fields;
- prove blocked/timeout do not become failed tests;
- reject negative counts;
- reject inconsistent passed summaries;
- verify raw evidence all-null/all-present rule;
- prove exact retries replay and conflicting retries reject.

Run 100 duplicate-submission rounds for each classification family.

## H7 - Manifest freeze race

Run 100 rounds with at least 10 callers freezing the same logical manifest version.

Expected:

```text
1 frozen manifest
identical retries replay
conflicting inventories reject
no partial entries
no duplicate file rows
inventory digest matches deterministic recomputation
```

## H8 - Manifest immutability and versioning

- reject update to frozen inventory;
- reject changed digest;
- permit append-only authority link;
- permit version 2 with predecessor version 1;
- reject predecessor from another attempt or project;
- preserve rejected and incomplete predecessor evidence.

## H9 - Provenance contention

Run 100 rounds for:

1. identical provenance assertion retry;
2. same bytes produced by multiple valid runs;
3. conflicting expected hash;
4. supersedence race;
5. missing producer reference;
6. cross-project logical artifact reference.

Expected exact duplicate replay, separate legitimate provenance assertions, hash mismatch rejection, and no overwrite of prior assertions.

## H10 - Storage path confinement

Test:

```text
..
../
..\
absolute drive paths
UNC paths
alternate separators
mixed normalization
trailing dots/spaces
reserved Windows names
symlink or junction escape where applicable
case-only collisions
Unicode normalization variants
```

Every resolved path must remain beneath its configured storage root. Durable records store normalized relative paths only.

## H11 - File/database crash boundaries

Inject crash at each boundary:

1. before file write;
2. after file write before hash;
3. after hash before database begin;
4. after database begin before row insert;
5. after rows before commit;
6. after commit before response;
7. after response transport failure.

For each, prove retry behavior, idempotency, explicit incomplete state, and absence of false verified claims.

## H12 - Missing and changed files

After verified provenance:

- delete file;
- change file bytes;
- move storage root;
- make root unavailable;
- restore original bytes;
- place different file at same relative path.

Expected explicit transitions among file_verified, file_missing, file_hash_mismatch, and reference_unresolvable. Provenance history remains intact.

## H13 - Migration empty-state and upgrade

For every migration sequence:

- create from empty database;
- apply one migration at a time;
- verify schema version/hash;
- interrupt before and after each transactional boundary;
- restart safely;
- reject modified applied migration;
- restore old retained generation and upgrade;
- test unsupported downgrade or fail-forward behavior.

No migration may silently partially apply.

## H14 - Backup under active writes

Run writers while creating a recovery generation.

Expected:

- database snapshot cutoff recorded;
- files referenced before cutoff captured;
- later records excluded consistently;
- restored generation has no database reference to uncaptured files;
- exact record and file counts match manifest.

Run at least 20 rounds due higher cost, with documented rationale if not 100.

## H15 - Restore consistency

Test:

1. matching database/file generation;
2. newer database with older files;
3. older database with newer files;
4. missing migration set;
5. corrupt database backup;
6. corrupt file archive;
7. hash mismatch in one artifact;
8. missing storage-root mapping.

Only the matching generation may pass full restore verification. Others fail closed with preserved diagnostics.

## H16 - Retention and holds

Test dry-run and later execution design for:

- Class B expired and eligible;
- active owner hold;
- accepted audit reference converting provenance to Class A;
- project archived but not deletable;
- project deletion with tombstone;
- canonical reference discovered between plan and execute;
- exact plan digest mismatch;
- concurrent retention attempts.

No canonical Markdown, Git, governance JSONL, staging evidence, or file bytes may change through metadata retention unless separately authorized.

## H17 - Query bounds and denial-of-service

- reject missing project ID;
- reject excessive page size;
- reject unbounded date scans where contract requires range;
- reject arbitrary sort or filters;
- cap evidence graph size;
- test pathological project with high record count;
- verify timeout and cancellation do not leave mutation state.

## H18 - Role and privilege boundary

Prove:

- runtime service cannot alter migration metadata or schema;
- readonly role cannot mutate;
- backup role cannot perform application writes;
- migrator cannot access secrets outside its purpose;
- agent environment has no direct DB credential;
- cross-role escalation attempts fail;
- schema search path cannot be hijacked.

## H19 - Audit observation integrity

For every mutation capability:

- successful mutation emits one bounded audit event;
- exact replay does not create duplicate mutation event unless a separate replay observation is intentionally specified;
- failed authority or validation request records only the allowed bounded diagnostic evidence;
- no payload, secret, absolute path, or raw output appears in audit rows or logs.

## H20 - Full first-slice loop

Prove end-to-end:

```text
reserve attempt
start attempt
finish attempt
record several verification runs
record artifact provenance
freeze return manifest
link canonical audit record
query complete evidence graph
backup and restore
deterministically compare restored graph
```

Run baseline successful, failed/corrected, and amendment-style histories modeled after Northstar.

## Test evidence contract

Every profile returns:

```text
test profile ID
source commit
migration-set hash
service version
Postgres version
worker count
round count
random seed
start and end timestamps
expected distribution
observed distribution
unexpected rows/files
elapsed time
result
frozen log and result hashes
```

## Acceptance bar

- all deterministic tests pass;
- no unexplained flaky outcome;
- exact expected distributions match;
- backup/restore proof succeeds;
- cross-project and privilege violations remain zero;
- connector failures are reported separately from test failures;
- complete evidence is independently audited.
