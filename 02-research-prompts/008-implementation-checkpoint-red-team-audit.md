# Claude Audit Prompt 008 - Kaizen Implementation Checkpoint Red-Team Audit

Status: active audit prompt
Date: 2026-06-07

## Prompt

You are conducting a comprehensive, adversarial checkpoint audit of the Kaizen project.

This is a **read-only repository, implementation, security, test, contract, and roadmap audit**.

Do not behave as a supportive reviewer whose job is to confirm that the project is going well. Act as a skeptical senior software architect, Windows filesystem/security engineer, test engineer, documentation-contract auditor, and project-delivery reviewer.

Your job is to find:

- errors;
- unsafe assumptions;
- gaps;
- contradictions;
- stale claims;
- unproven security properties;
- incomplete or misleading tests;
- implementation defects;
- roadmap drift;
- unnecessary complexity;
- missing simplifications;
- maintenance hazards;
- poor developer experience;
- documentation that no longer matches reality;
- work that should be stopped, changed, combined, split, or deferred;
- the shortest safe path from the current checkpoint to one complete Kaizen vertical slice.

Do not grade the project gently because substantial work has already been invested. Prior work is evidence, not proof.

Return a report only.

## Critical no-write boundary

Treat every repository and directory as read-only.

Do not:

- edit, create, rename, move, or delete files;
- stage, commit, push, reset, clean, checkout, restore, stash, merge, or rebase;
- install packages, plugins, services, or system components;
- modify Windows settings, ACLs, registry values, environment variables, Developer Mode, Git configuration, or Python configuration;
- execute canonical promotion;
- create new staged artifacts;
- alter existing staged artifacts or logs;
- run destructive, mutation-heavy, escape, crash, or recovery tests against the live roots;
- run paid provider calls;
- access secrets;
- produce database schemas;
- begin the next implementation task.

You may run read-only commands, source inspection, existing automated tests, static analysis, dry-run commands, and non-mutating validation commands when safe.

If an existing test suite normally writes only to its own disposable temporary directories, you may run it. Report exactly what it did and whether it touched live roots.

## Local paths

Use MCP filesystem and shell tools to inspect the live local state.

```text
Planning and governance repository:
C:\dev\kaizen-docs

Implementation repository:
C:\dev\kaizen\platform

Canonical vault repository:
C:\dev\kaizen\vault

Non-canonical staging root:
C:\dev\kaizen\staging
```

Do not substitute GitHub copies for these local repositories unless a local file is genuinely unavailable. The local files, source code, tests, Git state, vault, and staging evidence are the audit subjects.

## Git and filesystem state reporting

At the beginning, report read-only state for both Git repositories:

```text
git -C C:\dev\kaizen-docs status --short --branch
git -C C:\dev\kaizen-docs log -10 --oneline

git -C C:\dev\kaizen\platform status --short --branch
git -C C:\dev\kaizen\platform log -10 --oneline

git -C C:\dev\kaizen\vault status --short --branch
git -C C:\dev\kaizen\vault log -10 --oneline
```

Also inventory, read-only:

```text
C:\dev\kaizen\staging
```

Identify untracked, modified, generated, temporary, backup, malformed, or suspicious files.

At the end, repeat Git status and confirm no writes occurred.

## Current checkpoint

The expected current checkpoint is approximately:

```text
kaizen-docs HEAD: 738e2e1 Close create-only staging write milestone
kaizen-platform HEAD: 36fa419 Implement create-only staging write wrapper
kaizen-vault HEAD: 3de6042 Bootstrap canonical Kaizen vault
```

Expected implementation evidence:

- ID generation, Markdown parsing, deterministic validation, trusted-root configuration, and path-confinement tooling exist;
- the canonical vault has been bootstrapped;
- the staging root exists and is non-canonical;
- a create-only staging-write wrapper exists;
- the platform test suite last reported 126 passed and 0 skipped;
- a real two-process staging smoke test reportedly produced one physical create and one idempotent replay;
- Task Packet 005 is complete and steward-audited;
- Task Packet 006 is an uncommitted or otherwise pending draft for human-operated canonical promotion;
- canonical promotion has not been implemented or authorized;
- Hermes, MCP write exposure, Postgres, Qdrant, DataForSEO, Tauri, and the Obsidian bridge remain outside the first slice.

Verify every one of these claims. Do not assume this checkpoint description is accurate.

## Audit strategy

Use a traceability-driven audit rather than reading files in isolation.

For each important claim, trace:

```text
accepted decision
-> specification
-> task packet
-> implementation
-> tests
-> runtime evidence
-> roadmap/status claim
```

Flag every broken, ambiguous, or stale link in that chain.

Separate findings into:

```text
verified
partially verified
unverified
contradicted
misleading
obsolete
```

Use severity:

```text
Critical - could corrupt canonical truth, bypass human authority, escape roots, or create unrecoverable state
High - blocks safe canonical promotion or invalidates a claimed milestone
Medium - important correctness, maintainability, test, or workflow defect
Low - polish, naming, minor documentation, or non-blocking technical debt
```

Use disposition:

```text
fix before Task Packet 006 approval
fix before Task Packet 006 implementation
fix before the first real promotion
fix before Milestone 4
fix before Hermes
future hardening
remove or defer
```

## Required reading order

### 1. Entry points and roadmaps

```text
C:\dev\kaizen-docs\00-entrypoint\LLM_START_HERE.md
C:\dev\kaizen-docs\README.md
C:\dev\kaizen-docs\ROADMAP.md
C:\dev\kaizen-docs\IMPLEMENTATION_ROADMAP.md
```

### 2. Vision and project standard

```text
C:\dev\kaizen-docs\01-project-standard\kaizen-vision-and-architecture-alignment.md
C:\dev\kaizen-docs\01-project-standard\kaizen-project-standard.md
C:\dev\kaizen-docs\01-project-standard\internet-marketing-intelligence-vision.md
C:\dev\kaizen-docs\01-project-standard\baseline-v0.2-reconciliation-map.md
```

Identify historical guidance that has been superseded but is still written as current instruction.

### 3. Accepted decisions

Read all files in:

```text
C:\dev\kaizen-docs\04-design-decisions
```

Pay special attention to:

```text
0001-two-zone-agent-write-model.md
0002-search-before-create-and-diff-before-write.md
0003-raw-markdown-is-canonical.md
0005-api-only-structured-data-access.md
0006-hammer-tests-are-a-hard-gate.md
0008-v0.2-operating-conventions.md
0009-operational-postgres-and-observatory-boundary.md
0010-dedicated-internet-marketing-intelligence-database.md
0011-kaizen-desktop-shell-direction.md
0012-first-slice-contract-and-implementation-boundary.md
```

Verify statuses and determine which decisions actually govern the implemented slice.

### 4. First-slice specifications

Read all relevant files in:

```text
C:\dev\kaizen-docs\05-specs
```

At minimum:

```text
kaizen-field-registry.md
kaizen-note-type-registry.md
kaizen-id-and-prefix-registry.md
kaizen-validation-gate-spec.md
staging-and-promotion-workflow.md
staging-write-wrapper-and-promotion-recovery.md
promotion-event-schema.md
kaizen-hammer-test-strategy.md
```

Audit specifications for internal contradictions, underspecified behavior, duplicated rules, and implementation-incompatible requirements.

### 5. Research and reconciliations that directly shaped implementation

At minimum:

```text
C:\dev\kaizen-docs\03-research-results\008-agent-access-and-write-safety-claude-summary.md
C:\dev\kaizen-docs\03-research-results\012-step-5a-composed-tool-capability-map.md
C:\dev\kaizen-docs\03-research-results\016-kaizen-planning-audit-accelerated-path-to-implementation.md
C:\dev\kaizen-docs\03-research-results\017-decision-0008-dry-run-simulation.md
C:\dev\kaizen-docs\03-research-results\019-first-slice-tools-foundation-steward-audit.md
C:\dev\kaizen-docs\03-research-results\020-canonical-vault-bootstrap-steward-audit.md
C:\dev\kaizen-docs\03-research-results\021-staging-path-confinement-steward-audit.md
C:\dev\kaizen-docs\03-research-results\022-create-only-staging-write-wrapper-packet-audit.md
C:\dev\kaizen-docs\03-research-results\023-create-only-staging-write-wrapper-steward-audit.md
```

If filenames differ, locate the corresponding numbered reports.

### 6. Task packets and their completion evidence

Read every current task packet in:

```text
C:\dev\kaizen-docs\06-handoff-patterns
```

Focus especially on:

```text
001-bootstrap-kaizen-platform-id-parser-foundations.md
002-implement-deterministic-note-validation.md
003-bootstrap-canonical-kaizen-vault.md
004-implement-staging-root-and-path-confinement-foundations.md
005-implement-create-only-staging-write-wrapper.md
006-implement-human-operated-canonical-promotion.md
```

Task Packet 006 is a draft. Audit it as a proposed contract, not accepted authority.

### 7. Platform repository instructions and source

Read:

```text
C:\dev\kaizen\platform\AGENTS.md
C:\dev\kaizen\platform\README.md
C:\dev\kaizen\platform\pyproject.toml
```

Then inspect all relevant code and tests under:

```text
C:\dev\kaizen\platform\src
C:\dev\kaizen\platform\tests
C:\dev\kaizen\platform\schemas
```

Do not review only public APIs. Inspect internal error paths, native Windows calls, serialization, concurrency, locking, fsync behavior, parsing, normalization, test fixtures, and CLI behavior.

### 8. Canonical vault and staging evidence

Read and inventory:

```text
C:\dev\kaizen\vault
C:\dev\kaizen\staging
```

Verify:

- vault repository boundaries;
- `.gitignore` posture;
- governance log state;
- bootstrap note validity;
- note IDs and required sections;
- actual staged smoke artifact;
- actual attempt-log events;
- hashes where reported;
- absence of unauthorized canonical writes;
- absence of unrelated staging content;
- whether evidence supports the steward reports.

## Audit workstreams

## A. Doctrine and implementation consistency

Identify every place where:

- code violates or weakens an accepted decision;
- a draft spec is treated as accepted doctrine;
- a task packet silently changed a specification;
- implementation behavior is not documented;
- documentation claims a feature that code does not provide;
- implementation provides authority broader than documented;
- historical project-standard guidance conflicts with current decisions;
- terminology is inconsistent across docs, code, tests, and runtime evidence.

Produce a traceability matrix:

```text
Requirement or claim
Authoritative source
Implementation location
Test evidence
Runtime evidence
Verdict
Gap or correction
```

## B. Platform code correctness

Audit the platform implementation for:

- incorrect assumptions about Markdown/YAML parsing;
- field-registry and note-type-registry drift;
- incorrect ID parsing or generation;
- Unicode and newline bugs;
- unsafe path handling;
- Windows path edge cases;
- malformed error classification;
- swallowed exceptions;
- non-deterministic behavior;
- concurrency problems;
- resource/handle leaks;
- encoding inconsistencies;
- improper fsync or durability claims;
- idempotency failures;
- corrupted or partially written JSONL handling;
- large-file or denial-of-service exposure;
- CLI/library behavior differences;
- hidden global state;
- poor separation between policy, validation, filesystem mutation, and presentation;
- APIs that will be difficult to expose safely through future typed services.

For each code finding, provide exact file and symbol references.

## C. Native Windows security review

Hammer the implementation's Windows-native assumptions.

Inspect:

- `NtCreateFile` structures and ctypes declarations;
- access masks;
- share modes;
- create dispositions;
- create options;
- root-directory-relative object naming;
- reparse-point behavior;
- directory handle lifetime;
- mutex naming and scope;
- abandoned mutex behavior;
- error-code and NTSTATUS handling;
- file descriptor ownership transfer;
- handle close paths;
- partial writes;
- append behavior;
- `os.fsync` meaning on Windows;
- audit-log append interleaving;
- antivirus and file-lock behavior;
- path races not covered by current locks;
- behavior across multiple Kaizen installations or user sessions;
- path identity and case normalization;
- same-volume assumptions;
- whether the implementation's claims are stronger than what the primitives prove.

Do not rely only on comments or task-packet language.

Selective external verification is permitted only when materially needed for these platform claims. Use official Microsoft documentation, CPython documentation/source, or other primary sources. Do not use Deep Research for the whole audit.

## D. Test-suite red-team audit

Run the existing suite if safe and report:

```text
command
result
passed
failed
skipped
warnings
duration
whether live roots were touched
```

Then inspect whether tests actually prove their names and claimed properties.

Look for:

- tests that only restate implementation;
- mocks that bypass the dangerous layer;
- concurrency tested only in threads when cross-process behavior matters;
- crash tests that catch ordinary exceptions rather than simulate process death;
- recovery paths not tested after genuine partial persistence;
- tests dependent on execution order;
- missing property-based or fuzz-style coverage;
- missing malformed UTF-8/YAML/JSONL cases;
- insufficient native-handle failure injection;
- tests that cannot fail because the fixture preconditions are wrong;
- skipped tests hidden by environment;
- assertions that verify only exit codes, not filesystem state and evidence;
- missing checks that unrelated files remain byte-for-byte unchanged;
- missing Git-state checks;
- missing repeated-recovery/idempotency tests;
- missing multi-process audit-log contention tests;
- false confidence from total test count.

Recommend the smallest additional test set that materially raises confidence. Do not propose tests merely to inflate coverage.

## E. Runtime-evidence verification

Independently verify the evidence claimed for Task Packets 001 through 005 where practical.

For Task Packet 005 specifically, verify:

- platform commit exists;
- test count is reproducible;
- Developer Mode caused the symlink tests to run rather than skip;
- the staged smoke artifact exists;
- its byte count and SHA-256 match the report;
- attempt-log events parse correctly;
- event sequence is exactly what the report claims;
- event/request/hash relationships are coherent;
- two-process idempotency is genuinely supported by code and evidence;
- the canonical vault remained untouched;
- the completion report and audit did not overstate what was proved.

Do not mutate the runtime evidence while checking it.

## F. Task Packet 006 promotion design review

Audit the pending Task Packet 006 more aggressively than any prior packet because it crosses into canonical writes.

At minimum inspect:

### Approval and normalization

- Is there a deterministic, non-mutating `plan` phase?
- Are staged-source bytes and normalized canonical-candidate bytes treated as distinct when appropriate?
- Does the request contract explicitly contain both hashes?
- Is human approval bound to source, candidate, destination, validation evidence, transition, approver, time, and basis?
- Can execution recreate the exact candidate deterministically?
- Can any mutable external state invalidate approval without detection?
- Are human identity and approval evidence defined strongly enough for this local first slice?

### Canonical installation

- Is first-time installation truly atomic and collision-refusing on Windows?
- Which native primitive should be used?
- Is check-then-rename avoided?
- Are destination-directory replacement, reparse points, file locks, antivirus, and same-volume constraints handled?
- Can the temporary file be confused with unrelated files?
- Are temporary-file naming and cleanup ownership unambiguous?

### Event protocol

- Does intent become durable before canonical mutation?
- Are intent and committed events unambiguously linked?
- Are IDs, operation IDs, hashes, paths, validator evidence, and approval evidence sufficient?
- What happens if the canonical file is installed but the terminal event cannot be appended?
- Can recovery safely infer committed state without lying?
- Is the existing promotion-event schema actually sufficient for both source and candidate hashes?

### Recovery

- Is every partial state classifiable?
- Can recovery run repeatedly without new damage?
- What state requires human review?
- Can recovery accidentally overwrite unrelated content?
- Does retained staging create confusion or repeated promotion risk?
- Are log corruption, duplicate events, and orphan temporary files handled?

### Scope

- Does Packet 006 remain first-time promotion only?
- Has amendment, replacement, delete, Git automation, agent access, or general canonical filesystem behavior slipped in?
- Is the packet too large and should it be split into planning/preview, native-install prototype, and full promotion packets?

Deliver an explicit Packet 006 verdict:

```text
ready for owner approval
ready after listed corrections
prototype-first before approval
split before approval
reject and redesign
```

## G. Canonical vault audit

Determine whether the vault is a valid basis for the first real project loop.

Check:

- required bootstrap notes;
- frontmatter validity;
- note IDs;
- required headings;
- project naming;
- folder placement;
- governance files;
- promotion log posture;
- Git state;
- remote posture if visible without credentials;
- whether existing notes match current contracts or an older version;
- whether any bootstrap shortcut will break Milestone 4;
- whether promoting the current smoke claim would be appropriate or whether a cleaner fixture/project artifact is needed.

## H. Roadmap and status truthfulness

Audit:

```text
ROADMAP.md
IMPLEMENTATION_ROADMAP.md
LLM_START_HERE.md
README files
research indexes
handoff indexes
```

Identify:

- stale “current phase” statements;
- milestones marked complete without meeting exit criteria;
- work marked unstarted despite committed implementation;
- contradictory next actions;
- research lanes accidentally reintroduced as blockers;
- implementation work missing from the roadmap;
- first-slice scope creep;
- completed work that should be consolidated rather than repeatedly documented.

Produce a minimal exact edit list. Do not edit files.

## I. Architecture and maintainability review

Evaluate whether the first-slice implementation is becoming too complex for its value.

Ask:

- Is native Windows code justified at this stage?
- Is the custom ctypes layer maintainable and testable?
- Would a small Rust helper, existing audited library, or different design reduce risk?
- Are policy and native mechanics too tightly coupled?
- Is JSONL the right first-slice evidence store?
- Is the named mutex sufficient and appropriately scoped?
- Is the current CLI contract usable by a human without dangerous mistakes?
- Is the code structured to support later typed services without a rewrite?
- Are we solving future Hermes requirements prematurely?
- What should be simplified now?
- What must not be generalized yet?

Do not recommend a rewrite merely because another language or framework is fashionable. Compare migration risk against current verified evidence.

## J. Missing first-slice work

Identify the exact minimum work remaining before Kaizen has one completed vertical slice.

Classify each item as:

```text
before Packet 006 approval
before promotion implementation
before first real promotion
before Milestone 4
before Milestone 5 retrospective
not part of the first slice
```

Challenge the current sequence where warranted.

## K. Security and trust-boundary audit

Look for ways a future caller, compromised frontend, plugin, agent, malformed request, or local process could misuse existing tooling.

Include:

- root override opportunities;
- direct library bypasses;
- environment-variable influence;
- malformed request coercion;
- provenance spoofing;
- fake human approval;
- stale validation IDs;
- replay attacks;
- log tampering;
- mutex bypass;
- direct invocation of lower-level native helpers;
- TOCTOU gaps;
- unsafe public exports;
- overbroad CLI options;
- behavior if staging or audit files are manually altered;
- behavior under multiple Windows users or sessions.

Separate first-slice local-human assumptions from requirements needed before Hermes or a desktop UI.

## L. Delivery and process audit

Determine whether the project process is still appropriately thorough or has begun generating unnecessary ceremony.

Identify:

- duplicate reports that no longer add value;
- status updates that should be automated;
- places where one task packet could replace several planning passes;
- places where an external audit is genuinely necessary;
- whether owner approvals are occurring at the right risk boundaries;
- whether implementation evidence is returning to contracts efficiently;
- whether Claude usage should be concentrated differently;
- which remaining work should be performed directly by the steward without another Claude audit.

## Required output

Return one report with the following sections.

### 1. Executive verdict

State plainly:

- whether the current first-slice architecture is sound;
- whether the completed milestones are supported by evidence;
- whether any Critical or High findings invalidate a completed milestone;
- whether Task Packet 006 is ready for owner approval;
- whether implementation should pause pending fixes;
- the shortest safe path to a complete vertical slice.

### 2. Repository, Git, and runtime state observed

Include exact local paths, branches, commits, clean/dirty status, untracked files, staging inventory, and confirmation that no writes were made.

### 3. Verified checkpoint map

Use columns:

```text
Claimed capability
Documentation source
Code location
Test evidence
Runtime evidence
Verdict
```

### 4. Findings register

Use columns:

```text
ID
Severity
Area
Finding
Evidence
Consequence
Required disposition
```

Order Critical and High findings first.

### 5. Doctrine-to-code traceability audit

Show where accepted decisions and specs are correctly or incorrectly reflected in implementation and tests.

### 6. Platform code audit

Provide exact file and symbol references, focusing on correctness, security, maintainability, and interface design.

### 7. Windows native-filesystem audit

State which security and durability properties are actually proved, partly proved, or unproved.

### 8. Test-suite audit

Include commands run, results, weaknesses, false-confidence risks, and the smallest missing hammer-test set.

### 9. Runtime-evidence audit

Verify or dispute the completion evidence for Task Packets 001 through 005, especially Packet 005.

### 10. Task Packet 006 verdict

Give one of:

```text
ready for owner approval
ready after listed corrections
prototype-first before approval
split before approval
reject and redesign
```

List exact required edits and prototypes.

### 11. Vault and staging audit

Assess readiness for the first real promotion and governed project loop.

### 12. Roadmap and documentation audit

List exact stale or contradictory paths and the smallest corrections.

### 13. Architecture and maintainability recommendations

Classify recommendations as:

```text
adopt now
modify before promotion
defer until after the slice
reject
```

### 14. Remaining first-slice critical path

Give an ordered dependency-aware path from the current checkpoint to Milestone 5 completion.

For every step include:

```text
output
owner
risk level
Claude needed: yes/no
external research needed: yes/no
exit evidence
```

### 15. Work to stop or defer

Explicitly identify work that should not interrupt the first slice.

### 16. Suggested document and code changes

Provide exact paths and concise change descriptions. Do not make edits.

### 17. Recommended next three Claude windows

Given tight Claude usage limits, identify the three highest-value future prompts or reviews. State which work should not consume Claude.

### 18. Ordered next actions

Give the steward a short, actionable sequence.

### 19. Final no-write confirmation

Confirm:

- no files changed or created;
- no Git state changed;
- no staging or canonical artifacts changed;
- no packages or settings changed;
- no paid or provider calls occurred.

## Audit standards

- Prefer direct code and runtime evidence over report claims.
- Prefer accepted decisions over drafts.
- Prefer primary platform documentation for unstable native claims.
- Distinguish a passing test from proof of the claimed property.
- Do not infer safety from clean Git status.
- Do not infer authority from filenames, folders, prompts, or comments.
- Do not praise volume of work as evidence of correctness.
- Do not propose broad future architecture when a narrow first-slice fix is sufficient.
- Do not allow Postgres, Qdrant, DataForSEO, Hermes, MCP, Tauri, Obsidian plugins, or Internet Marketing Intelligence schema work to distract from the first-slice audit.
- Be specific enough that the steward can directly reconcile each finding.

## Final instruction

Hammer the project.

Try to disprove its safety claims, completion claims, and roadmap claims. If the implementation is sound, show why with traceable evidence. If it is not, identify the smallest safe corrections.

Return the report only.
