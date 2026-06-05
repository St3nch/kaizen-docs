# Spec - Staging-Write Wrapper and Promotion Recovery

Status: active draft
Date: 2026-06-05
Related decisions:
- `04-design-decisions/0001-two-zone-agent-write-model.md`
- `04-design-decisions/0002-search-before-create-and-diff-before-write.md`
- `04-design-decisions/0005-api-only-structured-data-access.md`
- `04-design-decisions/0006-hammer-tests-are-a-hard-gate.md`
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
Related evidence:
- `03-research-results/008-agent-access-and-write-safety-claude-summary.md`
Related specifications:
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`

## Goal

Define the smallest safe write surface for agent-created Markdown drafts and a recoverable human-operated promotion protocol.

This specification does not authorize Hermes writes, select an MCP server, select an implementation language, or approve canonical promotion tooling.

## Scope

This specification covers:

- one narrow create-new operation inside the sibling staging root;
- Windows path confinement requirements;
- required request, response, validation, provenance, and audit behavior;
- prohibited filesystem capabilities;
- promotion state and recovery behavior for one validated staged Markdown file;
- implementation and hammer-test prerequisites.

## Non-Goals

This specification does not define:

- a general-purpose filesystem API;
- direct canonical writes by Hermes or another agent;
- file deletion, move, rename, or overwrite tools;
- terminal or arbitrary code execution;
- exact implementation language or framework;
- final MCP server selection;
- cross-volume promotion support;
- validator or ULID-generator implementation details;
- database tables or Qdrant behavior.

## Authority boundary

- Hermes and other drafting agents may eventually request a staging create through the approved wrapper.
- The wrapper owns path validation, create-new enforcement, validation invocation, provenance capture, and audit evidence.
- Agents do not receive raw filesystem write access.
- Canonical promotion remains a human-controlled operation.
- Passing the future Hermes read-only test does not authorize staging writes.
- Passing a staging-write test does not authorize canonical promotion.

## Root model

Intended roots:

```text
C:\dev\kaizen-vault
C:\dev\kaizen-staging
```

The staging root and canonical root are siblings.

The wrapper is configured with one resolved staging root. It does not accept a caller-selected root.

## Staging-create operation

### Allowed behavior

The wrapper may create exactly one new Markdown file under an explicitly allowed staging subtree.

Initial logical operation:

```text
create_staged_markdown
```

The operation must use create-new semantics. If the target exists, the request fails without modifying it.

### Request contract

Required request fields:

| Field | Type | Rule |
|---|---|---|
| `logical_path` | string | relative path only; no caller-supplied root |
| `content` | UTF-8 string | bounded Markdown content |
| `note_type` | registry value | used for validation and placement checks |
| `project` | stable project ID or approved slug | required project scope |
| `agent_id` | string | durable actor provenance |
| `model_id` | string | exact model/version where available |
| `session_id` | string | durable session provenance |
| `request_id` | string | unique idempotency and audit key |
| `search_run_id` | string | evidence that search-before-create and duplicate review occurred |

Conditional request fields may include:

- source or research references;
- expected content hash supplied by an approved upstream drafting step;
- validation mode.

The request must not contain:

- an absolute destination path;
- an overwrite flag;
- delete, move, rename, chmod, ACL, terminal, or database instructions;
- a caller-selected canonical root;
- a general filesystem command.

### Response contract

Required response fields:

| Field | Type | Meaning |
|---|---|---|
| `request_id` | string | copied request identifier |
| `result` | enum | `created`, `rejected`, or `failed` |
| `logical_path` | string | accepted safe relative path |
| `resolved_path` | string | safe relative or redacted resolved representation |
| `bytes_written` | integer | zero on rejection |
| `content_sha256` | string or null | final content hash when written |
| `validation_status` | enum | `not-run`, `passed`, or `failed` |
| `validation_run_id` | string or null | validator evidence |
| `audit_event_id` | string | append-only attempt record |
| `failure_code` | string or null | stable rejection/failure code |
| `message` | string | concise human-readable explanation |

## Path acceptance rules

The wrapper accepts only a logical relative path.

Reject before writing when the path:

- is absolute;
- contains a `..` segment;
- uses a drive-relative form such as `C:folder`;
- is a UNC path;
- uses a device or extended-length prefix such as `\\?\` or `\\.\`;
- contains a null byte;
- uses a reserved Windows device name;
- resolves to the staging root itself rather than a child file;
- exceeds configured path-length limits;
- has a non-Markdown extension;
- violates filename or placement rules.

Mixed or alternate separators must be normalized only for rejection and comparison. Normalization must not turn a prohibited caller path into an accepted path silently.

## Windows final-path confinement

String-prefix comparison is not sufficient.

The implementation must prove confinement before writing content:

1. resolve the configured staging root through a directory handle and record its final filesystem path;
2. parse and validate the logical relative path before touching the filesystem;
3. open each existing parent directory segment without following an unapproved reparse target;
4. resolve every opened parent to its final path and require it to remain a strict descendant of the resolved staging root;
5. reject symbolic links, junctions, mount points, or other reparse points unless a later accepted policy explicitly permits and proves them safe;
6. open the immediate parent directory through the validated handle chain;
7. create the new target with create-new semantics relative to that validated parent where the implementation platform permits;
8. resolve the newly created target handle and verify strict descendant containment before writing any content;
9. write and flush content through the validated handle;
10. re-resolve and verify the final target after writing;
11. remove any zero-byte or temporary artifact if a post-create check fails and cleanup is provably inside staging;
12. log every rejected or ambiguous attempt.

If the chosen platform cannot create relative to a validated parent handle or provide an equivalent race-resistant guarantee, the wrapper must remain unapproved. A check-then-create sequence using only path strings is insufficient.

The wrapper must fail closed when final-path resolution, parent-handle verification, reparse-point inspection, or safe cleanup is unavailable or ambiguous.

## Existing-file behavior

The staging-create operation never overwrites an existing file.

Possible outcomes:

- `created` when no target exists and all checks pass;
- `rejected: target-exists` when a file or directory already occupies the target;
- `rejected: duplicate-candidate` when deterministic duplicate checks identify an existing artifact requiring review;
- `rejected: edit-requires-diff` when the request is actually an edit to existing content.

Editing an existing governed artifact requires a separate diff-producing workflow and human review. That workflow is outside this create-only contract.

## Content rules

Initial constraints:

- UTF-8 text;
- `.md` extension;
- lowercase-kebab filename unless a registry explicitly permits another form;
- bounded content size;
- normalized line endings;
- no unsupported binary or encoded payload;
- required staging frontmatter and provenance;
- no `accepted`, `approved`, canonical, or implementation-ready authority state set by the agent.

Exact size limits remain a configuration decision informed by real vault use.

## Validation behavior

After a safe create:

1. run the staging-mode deterministic validator;
2. record raw exit status and validation run ID;
3. set validation state from validator output, never agent narrative;
4. retain a failed draft in staging with explicit failed validation state unless cleanup policy says otherwise;
5. never promote automatically after validation passes.

If validation tooling does not exist, the wrapper is not implementation-ready for Hermes staging access.

## Provenance and audit evidence

Every accepted, rejected, or failed request produces an append-only attempt record containing at least:

- audit-event ID;
- request ID;
- timestamp;
- agent, model, and session identifiers;
- tool and version;
- action class;
- project scope;
- requested logical path;
- safe redacted final-path representation where applicable;
- result;
- stable failure or rejection code;
- content hash when written;
- validation run ID and result;
- duration;
- correlation or trace ID where later adopted.

Do not log credentials, private payloads, full customer data, or unnecessary content bodies.
A durable attempt or intent record must be appended and flushed before any filesystem mutation. If that pre-write audit append fails, the wrapper rejects the request and writes nothing.

After mutation and validation, the wrapper appends a terminal event. If the terminal append fails:

- the tool must not report `created` success;
- the staged file enters an explicit `audit-recovery-required` state;
- the file must remain non-promotable;
- a recovery process reconciles the pre-write event, filesystem state, content hash, and validation result;
- recovery appends a terminal record rather than editing history.

## Failure codes

Initial failure-code vocabulary:

```text
invalid-relative-path
absolute-path-rejected
drive-relative-path-rejected
unc-path-rejected
device-path-rejected
extended-path-rejected
reserved-name-rejected
path-outside-root
reparse-point-escape
target-exists
duplicate-candidate
edit-requires-diff
invalid-extension
invalid-filename
content-too-large
write-failed
hash-mismatch
validation-failed
audit-precondition-failed
audit-terminal-failed
audit-recovery-required
ambiguous-filesystem-state
```

The implementation may refine names before acceptance, but failures must remain machine-readable and stable.

## Idempotency and concurrency

- `request_id` is unique.
- A repeated request with the same ID and same accepted content returns the recorded prior outcome without another write.
- A repeated request ID with different content is rejected.
- Create-new filesystem semantics prevent two requests from silently racing to overwrite the same target.
- Duplicate ID, title, filename, and summary checks are performed by deterministic validation.

## Promotion principle

Promotion moves one reviewed, validated staged Markdown artifact into canonical storage under human control.

The canonical file write and append-only promotion evidence are separate operations. The system must be recoverable across partial failure and must not claim they are one atomic transaction.

## Promotion prerequisites

Promotion cannot begin until all are true:

- staged artifact passes deterministic validation;
- exact content hash is recorded;
- human review and approval bind to that hash;
- target canonical path is approved;
- target does not conflict with unrelated content;
- validator version and run ID are recorded;
- a unique intent event ID is allocated and will also serve as the promotion `operation_id`;
- backup and recovery prerequisites exist;
- staging and canonical destinations required for atomic replacement are on the same volume, or the operation is explicitly rejected as unsupported;
- the human-operated promotion command is the only actor with canonical write authority.

## Recoverable promotion state machine

### State P0 - approved

Human approval exists for the exact staged content and target.

### State P1 - intent recorded

Append an intent event containing:

- unique intent `event_id`;
- `operation_id` equal to the intent `event_id`;
- note ID;
- action;
- staged path;
- destination path;
- approved content hash;
- validation run ID;
- approver;
- timestamp;
- `event_phase: intent`.

The intent append is flushed before continuing.

### State P2 - canonical temporary file written

Write the approved content to a uniquely named temporary file in the destination directory.

Requirements:

- same volume as final destination;
- create-new semantics;
- flush content;
- compute and verify hash against approved content;
- preserve recoverable temporary-file metadata.

### State P3 - canonical file installed

Use a same-volume atomic replacement or rename mechanism supported by Windows.

Do not use a cross-volume copy-and-delete path as though it were atomic.

If the destination unexpectedly exists or has changed since approval, fail closed unless the approved operation explicitly governs replacement or amendment.

### State P4 - completion recorded

Append and flush a completion event with:

- a unique completion `event_id`;
- the same `operation_id` as the intent;
- `intent_event_id` referencing the intent record;
- the verified final hash;
- `event_phase: committed`.

### State P5 - staging finalized

Archive or remove the staged copy according to the approved retention policy.

Failure to finalize staging after completion does not invalidate the canonical artifact; recovery may safely repeat this step.

## Recovery rules

A recovery scan evaluates every intent without a matching terminal event.

### Intent exists, canonical file absent

- remove orphaned temporary files when safe;
- append a rollback or abandoned event;
- retain the staged source;
- allow a new human-reviewed retry.

### Intent exists, canonical file exists with approved hash

- append the missing completion event;
- continue staging finalization;
- do not rewrite the canonical file.

### Intent exists, canonical file exists with a different hash

- append a failure or rollback event;
- stop;
- require human review;
- do not overwrite either artifact automatically.

### Completion exists, staged copy remains

- archive or remove the staged copy according to retention policy;
- record recovery completion where required.

### Duplicate retry

- same operation or request ID and same hash: idempotent no-op or return prior committed outcome;
- same note and different hash without governed amendment or rollback: reject.

## Human-visible states

At minimum:

```text
promotion-ready
promotion-intent
promotion-incomplete
promotion-committed
promotion-rollback
promotion-conflict
recovery-required
```

These are operational workflow states, not universal Markdown frontmatter fields unless a later decision adopts them there.

## Security and permission requirements

- Hermes has no canonical write permission.
- The staging wrapper identity has write access only to the allowed staging root.
- The promotion identity is separate from the Hermes staging identity.
- Canonical promotion requires explicit human invocation.
- MCP root configuration is not accepted as sufficient proof of confinement.
- Terminal, delete, move, rename, ACL modification, and skill-management capabilities are absent from the Hermes clerk surface.
- Operating-system ACLs provide defense independent of agent behavior.

## Required test layers

### Static validation

- request and response schema;
- filename and extension rules;
- rejection of prohibited path forms;
- event schema validation.

### Unit tests

- path parser and rejection codes;
- descendant-boundary comparison;
- idempotency decisions;
- promotion state transitions;
- recovery classification.

### Integration tests

- valid create inside disposable staging;
- duplicate create;
- validation pass and failure;
- audit append;
- same-volume temporary write and replacement.

### Real-execution hammer tests

Required before staging writes are reconsidered:

- relative traversal;
- absolute canonical and sibling-repository paths;
- drive-relative path;
- UNC path;
- extended-length and device paths;
- mixed separator path;
- symbolic-link escape;
- Windows junction escape;
- parent-path replacement/race probe where feasible;
- overwrite attempt;
- unexpected target state;
- audit-log failure;
- canonical repositories remain clean.

Required before promotion is hardened:

- crash after intent;
- crash after temporary write;
- crash after canonical install but before completion event;
- duplicate retry;
- mismatched-hash retry;
- temporary-file cleanup;
- recovery scan idempotency;
- antivirus/file-lock interference;
- unsupported cross-volume attempt.

## Hermes integration ordering

Live Hermes tests belong to the implementation roadmap.

Order:

```text
canonical read/search integration
-> dedicated Hermes profile and tool inventory
-> read-only integration test
-> wrapper implementation
-> invalid-path and reparse-point hammer tests
-> human go/no-go
-> controlled Hermes staging-write test
```

The read-only test is not a prerequisite for current planning documents. It is a prerequisite for later live Hermes integration.

## Acceptance criteria

This draft is ready for acceptance only when the human reviewer confirms that:

- it defines a narrow create-only staging surface;
- it rejects broad filesystem capability;
- it requires final-path, not string-only, confinement;
- it includes symbolic-link and Windows junction escape handling;
- it treats promotion as recoverable rather than transactionally atomic across file and log;
- it preserves human-only canonical promotion;
- it defines failure and recovery states precisely enough to implement and hammer-test;
- it does not prematurely select an MCP server or implementation language.

## Open questions

- Exact maximum staged Markdown size.
- Exact Windows APIs and language/library bindings used for handle-based resolution and atomic replacement.
- Exact append durability guarantees for the promotion journal.
- Whether intent and completion events remain in JSONL for the first implementation or move to Operational Postgres later.
- Temporary-file naming and cleanup interval.
- Whether failed staged drafts remain in place or move to a quarantine subtree.
- Exact audit-log canonical home during the vault-only implementation phase.
- Exact human promotion interface.
- Whether a separate diff-only wrapper is needed for proposed edits to existing staged content.

## Related files

- `ROADMAP.md`
- `03-research-results/008-agent-access-and-write-safety-claude-summary.md`
- `05-specs/staging-and-promotion-workflow.md`
- `05-specs/promotion-event-schema.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/kaizen-hammer-test-strategy.md`
- `07-hermes/hermes-write-access-preconditions.md`
- `07-hermes/hermes-first-read-only-test.md`
- `07-hermes/hermes-first-staging-write-test.md`
