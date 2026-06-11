---
id: kz-tp-01KTW2QCN0Q2S4W6Y8Z0ABCDEF
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-11T20:10:00Z
updated: 2026-06-11T20:10:00Z
review_status: pending-review
authority: proposed
summary: "Packet 012A read-only defect register, reproducibility inventory, and reliability contract freeze."
primary_spec: kz-spec-01KTW2M8N4Q6S8W0Y2Z4ABCDEF
---

# Task Packet 012A - Defect Register and Contract Freeze

> Planning and read-only inspection only. This packet authorizes no source-code edits, test edits, tool-schema edits, lifecycle-script edits, dependency changes, connector migration, canonical mutation, cleanup, or Milestone 8 implementation until separately approved.

## Objective

Produce an evidence-backed Milestone 8 defect register, determine the current disposition of every initial candidate, freeze the exact accepted reliability contracts and test profiles for later implementation packets, and recommend the final 012B-012E split.

## Bound authority

```text
Milestone 8 specification SHA-256:
971f9e6d8b74bf8ea8dfbdcf14c78287eaacfe1cf5c91761aa80016e19ba46a3

Milestone 8 security/steward audit SHA-256:
51a5a1315273a07b407fa62747141bf716a8af5dba383c77d05b9837ee3ae404

Owner acceptance:
03-research-results/134-milestone-8-owner-acceptance.md

Reconciled Roadmap v0.3 SHA-256:
f8f5d20048169178111bd0f31fd41dc363777b90ff72957a5d19ec0b77b57f82
```

## Read-only scope

Inspect only:

```text
C:\dev\kaizen\platform
C:\dev\kaizen-mcp
C:\dev\chatgpt-mcp
C:\dev\kaizen-docs
C:\dev\kaizen\staging
C:\dev\kaizen\vault
```

Allowed activities:

- read files and bounded line ranges;
- search code, tests, docs, scripts, and evidence;
- inspect Git status, HEAD, remotes, diffs, and history for Git repositories;
- inspect fixed-root staging operation evidence without returning sensitive file contents;
- run existing read-only health and status tools;
- run existing focused and full tests only after separate implementation approval if test execution is treated as implementation activity by the active tool boundary;
- draft documentation in `kaizen-docs` and push reviewed docs to the existing upstream.

No live canonical mutation is required or authorized.

## Required initial checkpoints

Record:

### kaizen-docs

- branch, HEAD, clean state, upstream sync;
- accepted Roadmap v0.3 SHA-256;
- Milestone 8 specification and audit hashes.

### kaizen-platform

- branch, HEAD, clean state, remotes;
- Python and test metadata;
- current prepared-candidate loader and amendment concurrency code paths;
- relevant focused and hammer tests;
- current lint configuration and installed-tool status.

### kaizen-mcp

- fixed-root path and non-Git status;
- complete file inventory excluding `.venv`, caches, PID files, and secrets from content return;
- exact hashes for source, tests, scripts, README, pyproject, and operator docs;
- health payload and registered tool inventory;
- startup, ngrok, PID, port, and connector instructions;
- current FastMCP and package versions;
- current read-only and mutation-gated adapter boundaries.

### Go8

- branch, HEAD, clean state, remotes;
- health payload and registered tool inventory;
- source, tests, startup, ngrok, PID, port, and connector instructions;
- current FastMCP and package versions;
- exact fixed roots and capability surface;
- existing static capability scans and lifecycle tests.

## Defect register schema

Each candidate must include:

```text
id
short_name
first_observed_evidence
current_checkpoint
repository_or_layer
code_or_doc_paths
expected_contract
observed_behavior
current_reproducibility
consequence_class
security_impact
upstream_dependency
proposed_proof_method
disposition
closure_evidence
recommended_packet
```

Allowed dispositions:

```text
confirmed-defect
already-fixed-but-unproven
upstream-only
operator-runbook-gap
test-gap
policy-gap
obsolete
not-reproducible-retain-watch
deferred-with-reason
```

No candidate may be called fixed merely because the current code looks correct.

## Required candidates

Classify at least:

```text
M8-D01 prepared-candidate loader live-scope inconsistency
M8-D02 intermittent concurrency error-code drift
M8-D03 stale listener or duplicate-process startup behavior
M8-D04 unsigned PowerShell script invocation friction
M8-D05 fragmented startup and ngrok instructions
M8-D06 health version and tool-count registry drift risk
M8-D07 connector tool discovery stale after restart or recreation
M8-D08 upstream safety block before server execution
M8-D09 Go8 502 behavior after long-running sessions
M8-D10 Ruff availability and repository lint-policy mismatch
M8-D11 Kaizen MCP non-Git provenance and reproducibility
M8-D12 PID-file lifecycle and stale PID behavior
```

Add new candidates only when exact evidence is found.

## Contract-freeze outputs

### C1 - Prepared-candidate read-only contract

Define:

- behavior with correct packet binding;
- behavior with omitted optional packet binding;
- wrong packet behavior;
- stale vault/platform/governance binding behavior;
- malformed or missing evidence behavior;
- no-mutation guarantees;
- exact accepted error codes.

### C2 - Concurrency contract

Define:

- exactly one winner for conflicting requests;
- accepted loser error code;
- identical-request idempotency behavior;
- no mixed evidence property;
- stress repetition target;
- acceptable timing and filesystem assumptions.

### C3 - Lifecycle contract

Define for Go8 and Kaizen MCP:

- canonical start command;
- canonical ngrok command;
- host and port;
- port-occupied behavior;
- PID-file ownership and stale-PID behavior;
- duplicate-start behavior;
- stop and restart procedure;
- execution-policy-safe invocation;
- health-before-mutation rule.

### C4 - Health and registry contract

Define:

- authoritative version source;
- authoritative tool-registry source;
- required health fields;
- tool-count verification;
- alias and duplicate-name policy;
- connector freshness check;
- redaction requirements.

### C5 - Test and lint contract

Define per repository:

- required compile or syntax checks;
- focused tests;
- full tests;
- hammer or stress tests;
- static capability scans;
- Ruff or alternative lint requirement;
- dependency policy;
- evidence format.

## Reproduction boundary

Packet 012A may design reproduction procedures but must not mutate source code or tests.

A reproduction may be executed only if it:

- uses existing tests or read-only tools;
- writes solely to disposable test or temporary paths already allowed by the repository contract;
- creates no canonical mutation or governed event;
- kills no process;
- changes no connector configuration;
- exposes no secret;
- is explicitly included in a separately approved 012A implementation gate.

## Required analysis

For each candidate, answer:

1. Does current code still contain the suspected defect?
2. Does an existing test prove the required behavior?
3. Can the behavior be reproduced locally, only upstream, or not at all?
4. Is the impact correctness, security, operator friction, observability, or documentation?
5. What is the smallest later packet that can close it?
6. What evidence would prove closure?
7. Could fixing it widen authority or accidentally productionize a proving ground?

## Recommended packet assignment rules

```text
012B: platform contract, loader, concurrency, dirty-state, and stale-binding work
012C: Kaizen MCP adapter, lifecycle, health, registry, provenance, and runbook work
012D: Go8 lifecycle, health, registry, 502 classification, and runbook work
012E: integrated proof, accepted failure injections, governed return, and closure
```

Do not force a candidate into implementation when the correct disposition is upstream-only, documentation-only, obsolete, or watch-listed.

## Required return

Return:

- exact live checkpoints;
- complete defect register;
- evidence citations and file paths;
- reproducibility classification;
- accepted C1-C5 contracts;
- failure-injection matrix mapped to later packets;
- recommended lint policy;
- revised 012B-012E packet split;
- explicit exclusions and unresolved questions;
- security/steward audit;
- exact owner implementation gate for Packet 012A read-only execution, if any execution remains necessary;
- exact next gate for drafting Packet 012B only after 012A completion acceptance.

## Acceptance criteria

Packet 012A is complete only when:

1. every initial defect candidate has a supported disposition;
2. no unsupported fix claim is made;
3. current repository and runtime checkpoints are exact;
4. C1 through C5 are explicit and testable;
5. later packet assignments are proportional;
6. upstream-only issues are separated from local defects;
7. no source, test, tool, script, dependency, connector, canonical, or runtime configuration change occurs;
8. documentation changes are limited to the accepted planning return;
9. audit passes;
10. owner explicitly accepts the exact 012A completion evidence before Packet 012B drafting.

## Stop gates

Stop when:

- a repository is dirty unexpectedly;
- a root, version, tool registry, or runtime differs from the accepted checkpoint;
- a reproduction would require source or test mutation;
- a command may kill or alter an unrelated process;
- a secret-bearing file would need to be returned;
- connector configuration would need mutation;
- evidence is insufficient to classify a candidate;
- scope drifts into implementation or production migration.

## Explicit non-scope

- fixing any defect;
- editing code or tests;
- editing startup or ngrok scripts;
- adding dependencies;
- changing tool schemas or registrations;
- changing connector configuration;
- killing or restarting processes;
- production MCP architecture;
- mock-project execution;
- canonical amendment;
- vault push or remote creation;
- Postgres, Qdrant, Hermes, UI, Observatory, or Internet Marketing Intelligence work;
- cleanup or deletion.
