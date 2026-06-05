# Claude Research Prompt 003 - Agent Access and Write Safety

Status: active research prompt
Date: 2026-06-05

## Prompt

You are conducting Research Batch A for the Kaizen project.

This is a research, verification, and test-planning task only.

Do not edit files.
Do not create files.
Do not stage, commit, push, reset, clean, or discard repository changes.
Do not grant Hermes write access.
Do not run destructive filesystem tests against real repositories or important directories.
Do not produce an implementation roadmap.

Return a report only.

## Repository

Read the live local repository:

```text
C:\dev\kaizen-docs
```

Start by checking and reporting:

```text
git status
git log -5 --oneline
```

The repository should be treated as the planning and stewardship workspace, not the future production vault.

## Central objective

Determine what Kaizen must prove before Hermes or another agent may receive any write capability, even inside staging.

Research and verify:

1. MCP and agent-tool security boundaries
2. filesystem root confinement on Windows
3. traversal, symlink, and junction escape resistance
4. Hermes tool and permission granularity
5. staging-only write enforcement
6. safe diff-before-write behavior
7. atomic or recoverable promotion of a Markdown file plus append-only event logging
8. logging and evidence requirements for failed boundary attempts
9. the minimum hammer tests required before agent write access can be reconsidered

The goal is not to select products or write production code yet.

The goal is to establish verified constraints, identify what must be wrapped externally, and define runnable evidence gates.

## Required reading order

Read these files first:

```text
00-entrypoint/LLM_START_HERE.md
01-project-standard/kaizen-vision-and-architecture-alignment.md
ROADMAP.md
```

Then read the accepted decisions:

```text
04-design-decisions/0001-two-zone-agent-write-model.md
04-design-decisions/0002-search-before-create-and-diff-before-write.md
04-design-decisions/0005-api-only-structured-data-access.md
04-design-decisions/0006-hammer-tests-are-a-hard-gate.md
04-design-decisions/0007-foundation-resolution-for-v0.2.md
04-design-decisions/0009-operational-postgres-and-observatory-boundary.md
```

Read the current Hermes policy and test plans:

```text
07-hermes/hermes-permission-matrix.md
07-hermes/hermes-write-access-preconditions.md
07-hermes/hermes-first-read-only-test.md
07-hermes/hermes-first-staging-write-test.md
07-hermes/kaizen-hermes-skill-draft.md
```

Read the related staging, validation, promotion, and hammer specifications:

```text
05-specs/staging-and-promotion-workflow.md
05-specs/promotion-event-schema.md
05-specs/kaizen-validation-gate-spec.md
05-specs/kaizen-hammer-test-strategy.md
```

Read the relevant evidence summaries:

```text
03-research-results/001-hermes-agent-boundaries-claude-summary.md
03-research-results/002-hermes-desktop-verification-gpt-summary.md
03-research-results/003-obsidian-agent-readable-infrastructure-gpt-summary.md
03-research-results/005-veda-db-governance-hammer-patterns-claude-summary.md
03-research-results/007-kaizen-vision-architecture-research-gap-audit-claude-summary.md
```

Read other repository files where needed to understand contradictions or dependencies.

## Accepted boundaries

Treat these as fixed unless you identify a direct contradiction requiring human review:

- Canonical Markdown is read-only to Hermes.
- Agent writes begin only in a sibling staging root.
- The intended roots are:

```text
C:\dev\kaizen-vault
C:\dev\kaizen-staging
```

- Hermes must not receive arbitrary SQL, direct Postgres credentials, direct Qdrant mutation, Git commit/push authority, promotion authority, audit-verdict authority, or implementation-readiness authority.
- Search-before-create is mandatory.
- Diff-before-write is mandatory for existing governed content.
- Prompt obedience is not a security boundary.
- Folder naming is not a security boundary.
- MCP is an integration protocol, not an authorization boundary by itself.
- Real-execution hammer tests are required before high-value boundaries are considered hardened.

## Research questions

### A. MCP and tool-surface security

Determine:

- What security properties does the MCP protocol itself provide, and what must be implemented by the server or wrapper?
- How do common filesystem-backed or Obsidian-related MCP servers scope roots?
- Can tools be exposed with separate read, search, create, patch, delete, move, terminal, and skill-management capabilities?
- Can the client or server reliably prevent an agent from invoking tools outside the intended capability set?
- How are authorization, tool discovery, argument validation, and audit logging handled?
- Are there relevant known vulnerabilities, CVEs, issue reports, or documented escape failures?
- What minimum server or wrapper requirements should Kaizen mandate without selecting a specific implementation yet?

Use primary sources where possible:

- official MCP specifications
- official server repositories and documentation
- official security documentation
- CVE and vulnerability databases
- maintainer issue trackers

Distinguish protocol guarantees from implementation-specific guarantees.

### B. Hermes capability verification

Determine what must be verified in the deployed Hermes Desktop or Hermes Agent environment:

- Can read and search remain enabled while write, patch, delete, move, terminal, and skill-management tools are disabled?
- Can write access be confined to one staging root while canonical roots remain readable but not writable?
- Are permissions enforced by Hermes, the MCP/tool server, OS permissions, an external wrapper, or some combination?
- Can Hermes emit a diff without directly changing an existing file?
- Can it report raw tool results and failures honestly and consistently?
- Can a dedicated Kaizen profile capture and preserve the effective tool configuration?
- What exact hands-on tests are required before any assumption is trusted?

Do not claim capabilities from marketing language alone.

Classify each capability as:

```text
verified by documentation
requires hands-on verification
not supported
unclear
must be externally wrapped
```

### C. Windows filesystem confinement

Research Windows-specific path and filesystem behavior relevant to staging-only writes:

- canonical path normalization
- `..` traversal
- absolute paths
- alternate path separators
- drive-relative paths
- UNC paths
- extended-length paths such as `\\?\`
- case-insensitive path comparison
- short 8.3 paths where applicable
- symbolic links
- directory junctions and reparse points
- hard links where relevant
- mount points
- race conditions between validation and write
- inherited permissions and ACLs

Determine what a safe wrapper must do before writing.

At minimum, evaluate whether it must:

1. accept a logical relative path rather than arbitrary absolute input;
2. join it to a configured staging root;
3. normalize and resolve the final parent path;
4. reject traversal and alternate-root forms;
5. inspect reparse points or resolve the final target;
6. verify the resolved target remains inside the configured root;
7. create files with no-follow or equivalent protections where available;
8. recheck after creation where race conditions remain possible;
9. log rejected paths and reasons without exposing secrets.

Use Microsoft and filesystem documentation where possible.

### D. Staging-only write wrapper

Recommend the minimum behavior of a Kaizen staging-write wrapper.

Cover:

- allowed operations
- prohibited operations
- create-new versus overwrite behavior
- exact handling of existing files
- diff generation
- path validation
- extension and filename validation
- maximum file size
- encoding and newline rules
- provenance capture
- validation before success
- partial failure behavior
- audit logging
- idempotency
- concurrency
- cleanup of temporary files

Do not write production code.

Provide pseudocode or a state machine only if it materially clarifies the boundary.

### E. Atomic or recoverable promotion on Windows

Research a safe promotion operation that moves one validated Markdown file from staging to canonical storage while recording an append-only promotion event.

The system must account for the fact that a filesystem write and a JSONL append are not one atomic transaction.

Evaluate patterns such as:

- write temporary canonical file, flush, rename, append event
- append intent event, write/rename file, append completion event
- write event to temporary journal and recover on restart
- content-addressed or idempotent promotion IDs
- compare-and-swap against reviewed content hashes
- rollback or reconciliation after partial failure

Cover:

- filesystem atomic rename guarantees on Windows
- same-volume versus cross-volume behavior
- overwrite semantics
- flushing and durability
- antivirus or file-lock interference
- crash between file write and event append
- duplicate retries
- recovery scanning
- human-visible failure states

Recommend a recoverable protocol, not a fictional two-resource atomic transaction.

### F. Logging and evidence

Define the minimum evidence that must exist for:

- successful read-only tests
- successful staging writes
- rejected traversal attempts
- rejected canonical writes
- rejected symlink/junction escapes
- validation failures
- partial promotion failures
- recovery runs
- tool authorization failures

Consider:

- timestamps
- actor/agent/model/session
- tool and version
- requested logical path
- resolved path or safe redacted representation
- action class
- result
- rejection reason
- content hash
- validation run ID
- promotion/recovery event ID

Avoid logging private payloads or secrets unnecessarily.

### G. Hammer-test design

Review the current first staging-write test and hammer strategy.

Propose a runnable test matrix covering valid and invalid paths.

Include at least:

- valid create inside staging
- duplicate create
- existing-file overwrite attempt
- relative traversal
- absolute canonical path
- future vault path
- sibling repository path
- alternate separators
- UNC or extended path form where applicable
- symlink escape
- junction escape
- race or replacement of a validated parent path where feasible
- validation failure after temporary write
- crash or simulated failure between promotion steps
- duplicate promotion retry
- recovery after partial failure
- audit-log verification
- canonical repository remains clean after staging-only tests

For each test, provide:

```text
precondition
operation
expected result
required evidence
cleanup
```

Clearly separate:

- static validation
- unit tests
- integration tests
- real-execution hammer tests

## Prototype guidance

You may recommend prototypes, but do not execute destructive tests in this task.

For each recommended prototype, specify:

- purpose
- disposable environment
- prerequisites
- exact success/failure evidence
- risk controls
- whether administrator privileges are needed
- cleanup requirements

Prefer disposable directories and dedicated test identities.

Do not recommend testing escape behavior against valuable files or repositories.

## Research standards

For each important finding:

- cite the primary or authoritative source;
- distinguish stable operating-system behavior from rapidly changing product behavior;
- record the date checked for vendor-specific capabilities;
- state uncertainty explicitly;
- avoid treating absence of documentation as proof of safety;
- identify what still requires hands-on verification.

Classify findings as:

```text
stable platform constraint
protocol constraint
product capability
product limitation
security risk
implementation requirement
prototype finding needed
open question
```

Classify priority as:

```text
P0 - required before any agent write test
P1 - required before staging writes can be reconsidered
P2 - required before canonical promotion tooling
P3 - useful hardening or later optimization
```

## Required output

Return one report with these sections.

### 1. Executive verdict

State:

- whether the current Kaizen safety model is conceptually sound;
- which assumptions are already supported;
- which assumptions are unverified;
- whether native Hermes/MCP controls are likely sufficient or an external wrapper is required;
- what must happen before the read-only test, staging-write test, and promotion implementation.

### 2. Repository and Git state observed

Include:

- branch;
- latest commit;
- clean or dirty status;
- confirmation that no files were changed.

### 3. Constraint register

Use columns:

```text
ID
Constraint
Source or evidence
Stability
Priority
Kaizen consequence
```

### 4. MCP security findings

Separate:

- protocol-level guarantees;
- server-level responsibilities;
- client/Hermes responsibilities;
- OS-level controls;
- recommended Kaizen wrapper responsibilities.

### 5. Hermes capability matrix

Use columns:

```text
Capability
Documentation evidence
Verification status
Risk
Required test
Fallback/wrapper need
```

Include read, search, create, patch, delete, move, terminal, skill management, tool discovery, and root scoping.

### 6. Windows path-confinement requirements

Provide:

- threat model;
- path forms to reject;
- normalization/resolution rules;
- reparse-point handling;
- race-condition concerns;
- minimum safe algorithm.

### 7. Staging-write wrapper contract

Define the smallest safe tool contract for creating one new staged Markdown file.

Include request fields, response fields, failure codes, logging, validation, and prohibited behavior.

Do not design a general-purpose filesystem API.

### 8. Promotion and recovery protocol

Recommend a recoverable sequence for:

```text
validated staged file
-> canonical Markdown file
-> append-only promotion evidence
```

Include state transitions and recovery after each partial-failure point.

### 9. Test and hammer matrix

Provide the full valid/invalid-path matrix described above.

### 10. Gaps in current Kaizen documents

Identify exact file paths and the changes likely needed after research reconciliation.

Do not edit them.

### 11. Research disposition

Classify major recommendations as:

```text
adopt
modify
reject
defer
```

### 12. Ordered next actions

Give a dependency-aware sequence for the project steward.

### 13. Final no-write confirmation

Confirm that you:

- changed no files;
- created no files;
- staged nothing;
- committed nothing;
- pushed nothing.

## Important constraints

Do not:

- assume Hermes prompts or skills enforce security;
- assume an MCP server's configured root prevents junction or symlink escape without evidence;
- assume filesystem rename plus JSONL append is atomic;
- recommend giving Hermes direct filesystem write access to canonical roots;
- recommend direct Postgres or Qdrant credentials;
- design a broad general-purpose filesystem MCP server for Kaizen;
- weaken human promotion authority;
- run tests against production or valuable files;
- select a final implementation language without evidence;
- produce the implementation roadmap.

Be skeptical and specific.

The purpose is to produce verified constraints and testable safety gates, not to validate the existing plan by default.

Return the report only.
