# Research Result 008 - Agent Access and Write Safety

Status: active evidence summary
Date: 2026-06-05
Source: Claude Research Batch A report supplied by the project owner

## Purpose

Preserve and reconcile Claude's Research Batch A findings on MCP security, Hermes capability boundaries, Windows filesystem confinement, staging-only writes, promotion recovery, logging, and hammer tests.

This file is evidence and steward reconciliation. It does not grant Hermes write access, select an MCP server, or authorize implementation.

## Executive finding

Claude concluded that Kaizen's safety model is conceptually sound:

- prompt obedience is not a security boundary;
- MCP is an integration protocol, not an authorization boundary;
- canonical Markdown should remain read-only to Hermes;
- staging-only writes require structural enforcement;
- human-only promotion remains correct;
- real-execution hammer tests are required before write capability is hardened.

The report found that native Hermes or MCP controls should not be trusted as the sole boundary. Kaizen should require an external staging-write wrapper plus operating-system permissions.

## Adopt

### MCP is not the authorization boundary

Adopt the finding that MCP does not itself provide the fine-grained authorization, root confinement, or capability separation Kaizen needs.

Kaizen must enforce scope through narrow tools, wrappers, service policy, and operating-system permissions.

### Configured roots are not sufficient proof

Adopt the finding that a configured filesystem root is not sufficient evidence of confinement.

Known filesystem MCP vulnerabilities demonstrate recurring failure classes:

- string-prefix containment errors;
- symlink escape;
- failure to resolve final targets;
- over-broad filesystem capabilities.

Every candidate server or wrapper must pass the Kaizen invalid-path and reparse-point test matrix.

### External staging-write wrapper

Adopt the requirement for a narrow wrapper that creates one new staged Markdown file.

The wrapper should:

1. accept a logical relative path rather than arbitrary absolute input;
2. reject traversal, drive-relative, UNC, device, and extended-length path forms;
3. join against a configured staging root;
4. resolve the final filesystem target through Windows handles;
5. compare the resolved target against the resolved root using case-insensitive separator-boundary containment;
6. reject symlink, junction, or other reparse-point escape;
7. use create-new semantics and refuse overwrite;
8. record provenance, content hash, validation result, and audit evidence;
9. expose no delete, move, rename, terminal, or broad filesystem operation.

### Windows final-path resolution

Adopt handle-based final-path resolution as the intended confinement mechanism.

String normalization alone is insufficient because junctions, symbolic links, reparse points, case-insensitive aliases, and extended path forms may resolve somewhere different from the input string.

### Recoverable promotion, not fictional atomicity

Adopt the finding that writing a canonical Markdown file and appending a JSONL event cannot form one atomic filesystem transaction.

Promotion should use a recoverable protocol with:

- intent event;
- temporary file in the canonical destination directory;
- flush and content-hash verification;
- same-volume atomic replacement where supported;
- completion event;
- recovery scan for incomplete intents;
- idempotent retry behavior.

### Expanded hammer-test matrix

Adopt the proposed test categories:

- valid staging create;
- duplicate create;
- overwrite attempt;
- relative traversal;
- absolute canonical path;
- sibling repository path;
- alternate separators;
- UNC and extended-length path forms;
- symbolic-link escape;
- Windows junction escape;
- parent-path replacement or race probe;
- validation failure after temporary write;
- crash between promotion states;
- duplicate promotion retry;
- audit-log verification;
- confirmation that canonical repositories remain clean.

## Modify

### Native Hermes controls

Treat Hermes configuration as defense in depth, not as the primary security boundary.

Hands-on verification must determine whether read and search can remain enabled while write, patch, delete, move, terminal, and skill-management capabilities are disabled.

### MCP server selection

Do not select a final MCP server yet.

First define and test the Kaizen wrapper contract and invalid-path matrix. Candidate servers may then be evaluated against those requirements.

### Promotion specification

Modify current promotion language to use explicit intent, committed, rollback, and recovery states.

Do not describe the file-plus-log operation as atomic.

### Hermes read-only test timing

The report says the read-only test can run now because it performs no writes.

The steward modifies that sequence: this remains a future Hermes integration test and is not required during the current planning phase. It should run under the implementation roadmap only after a Kaizen integration environment, canonical read/search interfaces, a dedicated Hermes profile, and an auditable tool inventory exist.

The read-only test proves orientation and no-write behavior only. It does not prove staging-write safety.

## Reject

Reject:

- relying on prompts, skills, approval mode, or folder names as security controls;
- relying on MCP root configuration without escape testing;
- granting Hermes terminal, delete, move, rename, or skill-management capability for clerk workflows;
- exposing a general-purpose filesystem MCP tool to Hermes;
- claiming that canonical file replacement and JSONL append are one atomic transaction;
- granting any canonical write or promotion authority to Hermes.

## Defer

Defer until prerequisites exist:

- selection of a final MCP server or gateway;
- implementation-language choice for the wrapper;
- cross-volume promotion behavior;
- production performance tuning of recovery scans;
- canonical promotion implementation before the validator and ULID generator exist;
- broader agent routing or gateway architecture.

## Constraint summary

### Protocol and product

- MCP authorization and security behavior depend on transports, clients, servers, and wrappers; MCP alone is not the policy boundary.
- Product capability claims must be verified in the deployed Hermes environment.
- Absence of documentation is not evidence of safety.

### Windows filesystem

The staging wrapper must account for:

- `..` traversal;
- absolute paths;
- mixed path separators;
- drive-relative paths;
- UNC paths;
- device and extended-length paths such as `\\?\`;
- case-insensitive path comparison;
- symbolic links;
- directory junctions and reparse points;
- race conditions between path checking and writing.

### Promotion and recovery

Promotion should be treated as a state machine:

```text
approved
-> intent recorded
-> temporary file written and flushed
-> content hash verified
-> canonical file replaced
-> completion recorded
-> staged copy archived or removed
```

A recovery process must reconcile incomplete intents after crashes or partial failure.

## Minimum staging-write tool contract

A future staging-create tool should accept only:

- relative logical path;
- Markdown content;
- note type;
- agent, model, and session provenance.

It should return:

- created, rejected, or failed result;
- safe relative or redacted resolved path;
- bytes written;
- content hash;
- validation result;
- audit-event ID;
- explicit rejection or failure code.

It must not support overwrite, append, delete, move, rename, ACL changes, terminal execution, or writes outside staging.

## Required hands-on verification

Before a staging-write test, verify the deployed Hermes environment for:

- exact tools exposed to the model;
- read capability;
- search capability;
- write capability;
- patch behavior;
- delete and move capability;
- terminal or execute-code capability;
- skill-management capability;
- tool-discovery behavior;
- root-scoping behavior;
- profile persistence and configuration capture.

Classify each capability as:

```text
verified by documentation
verified by hands-on test
not supported
disabled
unclear
must be externally wrapped
```

## Test sequence

### Stage 1 - future read-only integration test

This stage belongs in the implementation roadmap, not the current planning phase. When its prerequisites exist:

1. Confirm the integration target is clean and backed up.
2. Capture the Hermes profile and exposed tool set.
3. Run `07-hermes/hermes-first-read-only-test.md`.
4. Confirm no files changed.
5. Record the output and test evidence.

### Stage 2 - wrapper and boundary prototypes

1. Specify the narrow staging-create wrapper.
2. Build only in a disposable sandbox.
3. Use a dedicated low-privilege test identity.
4. Run invalid-path and reparse-point tests before any agent write.
5. Keep canonical repositories outside the destructive test target.

### Stage 3 - controlled staging write

Only after the invalid-path matrix passes:

1. run the valid create test;
2. run duplicate and overwrite rejection tests;
3. run validation-failure behavior;
4. verify exactly one expected staged file;
5. verify all canonical repositories remain clean.

### Stage 4 - promotion and recovery

Only after the validator and ULID generator exist:

1. implement the intent/completion protocol;
2. simulate crashes between every state;
3. test duplicate retries;
4. test mismatched-hash rejection;
5. test recovery scanning;
6. retain human-only promotion authority.

## Likely document updates

After human review, the findings likely affect:

- `04-design-decisions/0005-api-only-structured-data-access.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-permission-matrix.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `07-hermes/hermes-first-staging-write-test.md`
- `07-hermes/kaizen-hermes-skill-draft.md`
- `08-obsidian/README.md`

A dedicated staging-write wrapper and promotion-recovery specification may be justified after the research is reconciled. It should not become accepted doctrine automatically.

## Ordered next actions

1. Confirm the repository is clean.
2. Review this reconciliation and the supplied Claude report.
3. Draft the staging-write wrapper and promotion-recovery specification.
4. Update the affected current drafts through a reviewable reconciliation batch.
5. Define the Hermes read-only and staging-write execution prerequisites as Phase 7 implementation inputs.
6. Place live Hermes read-only testing in Phase 8 after read/search integration and profile/tool verification exist.
7. Do not run the staging-write test until the wrapper and invalid-path hammer matrix exist.
8. Do not implement canonical promotion until the validator and ULID generator exist.

## Source limitation

The raw Claude report was supplied as an uploaded text artifact outside the Windows repository environment. This file preserves its load-bearing findings and the project steward's reconciliation. The raw report should also be retained when the repository environment can access it.
