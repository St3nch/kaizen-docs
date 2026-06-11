---
id: kz-tp-01KTW3N2PQRSTUVWXYZABCDE
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-11T21:18:00Z
updated: 2026-06-11T21:18:00Z
review_status: pending-review
authority: proposed
summary: "Packet 012D Go8 health, registry, lifecycle, runbook, 502 classification, and runtime-log provenance reliability."
primary_spec: kz-spec-01KTW2M8N4Q6S8W0Y2Z4ABCDEF
---

# Task Packet 012D - Go8 Reliability and Runbooks

> Go8-only implementation packet. This draft authorizes no implementation until the owner approves its exact audited SHA-256. It does not authorize connector mutation, process termination or restart, Kaizen MCP or platform changes, canonical mutation, remote creation, cleanup, or Packet 012E work.

## Objective

Close the Go8 portions of Milestone 8 defects M8-D03 through M8-D06, M8-D09, M8-D10, and M8-D13 by deriving truthful health and registry evidence, adding deterministic local lifecycle and occupied-port proof, consolidating local and tunnel runbooks, classifying the long-session 502 issue without overclaiming, and separating runtime logs from source provenance.

## Bound authority

```text
Authoritative standard SHA-256:
5b441a9e024446d8a9acb3a58a8a24ad020772f4e7666d697c495352bebb6e90

Roadmap v0.3 SHA-256:
f8f5d20048169178111bd0f31fd41dc363777b90ff72957a5d19ec0b77b57f82

Milestone 8 specification SHA-256:
971f9e6d8b74bf8ea8dfbdcf14c78287eaacfe1cf5c91761aa80016e19ba46a3

Packet 012A contract-freeze SHA-256:
a5a3650adf067fe1a96c8d0a26d087e6f387c5a9e35828cfea9784add0e77ea9

Packet 012C completion audit SHA-256:
3af71c9ec8407f45bb28db6a4bce198ddd0865c5e5cb5a629c5ba8a832df07c2
```

## Repository and implementation boundary

Only this repository is in implementation scope:

```text
C:\dev\chatgpt-mcp
```

Expected checkpoint before implementation:

```text
branch: master
HEAD: 5eccbc71dff5752b083b6b2bd019416d4e5baf73
working tree: clean
remotes: none
service: chatgpt-mcp
version: 0.4.0
FastMCP: 3.4.2
reported tool count: 44
generic execution: false
```

No remote may be created. Any implementation commit remains local.

## Read-first paths

Search before editing, then inspect at minimum:

```text
README.md
TOOLS.md
CHANGELOG.md
IMPLEMENTATION_RETURN.md
pyproject.toml
start.ps1
server.py
src/chatgpt_mcp/server.py
src/chatgpt_mcp/extra_tools.py
src/chatgpt_mcp/models.py
src/chatgpt_mcp/config.py
scripts/
tests/test_server.py
all lifecycle, process-evidence, capability, and profile tests
logs/
.gitignore
```

Runtime logs may be inventoried and hashed, but their contents must not be treated as current connector truth or returned when they may contain stale endpoints or diagnostic noise.

## Verified planning findings

### Health and registry

Current `health()` hard-codes version `0.4.0`, framework `fastmcp-3.4.2`, and tool count `44`. The existing health test separately asserts the literal `44`; that proves consistency between copied constants, not equality with the live FastMCP registry.

### Lifecycle

The root `start.ps1` directly invokes the virtual-environment `fastmcp.exe` on `127.0.0.1:8770/mcp`. It currently has no explicit virtual-environment check, occupied-port preflight, duplicate-start refusal, or operator guidance.

### Runbook and tunnel

README documents the local and custom public endpoints but only instructs `./start.ps1`. It does not document process-scoped execution-policy bypass, occupied-port behavior, listener ownership verification, safe stop/restart sequencing, the exact custom-domain tunnel command, or health-before-use and connector-refresh ordering.

### 502 classification

One long-running session produced repeated 502 responses until server and tunnel restart. Existing evidence does not distinguish local server, tunnel, connector, session, or upstream cause. Packet 012D must not claim a local fix unless local evidence reproduces or correlates the failure.

### Runtime logs

The repository contains a `logs/` directory with runtime ngrok evidence that may reference older random endpoints. These logs are diagnostics, not source authority, connector configuration, or current endpoint truth.

## Work package A - truthful health and registry evidence

Update the existing `health` tool only. Do not add a new public tool.

Required fields:

```text
service
package version
FastMCP version
fixed root names
actual registered tool count
generic execution posture
```

Requirements:

- derive package version from one accepted source;
- derive FastMCP version from installed package metadata or another authoritative installed-runtime source;
- derive tool names and count from the live FastMCP registry through one isolated supported/stable boundary;
- reject duplicate tool names;
- test exact unique registry names and count;
- remove copied literal count assertions as the source of truth;
- expose no token, environment secret, tunnel credential, private identity, or unrestricted path detail.

If FastMCP registry introspection requires unsupported private internals, stop and return an exact packet correction and audit.

## Work package B - lifecycle and occupied-port behavior

Define and prove the fixed local contract: `127.0.0.1:8770/mcp`, with public endpoint `https://go8.ngrok.dev/mcp`.

Required behavior:

- missing `.venv` or `fastmcp.exe` fails clearly;
- occupied port refuses startup before server launch;
- duplicate start never kills, replaces, or attaches to an existing process;
- no automatic process termination;
- process-scoped PowerShell execution-policy bypass is documented;
- stop procedure verifies listener ownership before separately authorized termination;
- restart requires stopped listener, fresh local start, health pass, tunnel validation, then connector refresh;
- no service, scheduler, daemon, or auto-start registration.

Tests must use disposable ports, dependency injection, or static script inspection. They must not touch the live Go8 listener on port 8770.

## Work package C - local and tunnel runbook consolidation

One authoritative runbook must state the exact local start command, process-scoped bypass form, local endpoint, custom public endpoint, exact ngrok command or configuration invocation, separate server and tunnel lifecycles, health verification, connector refresh ordering, and safe ownership checks before any termination.

Do not add tunnel credentials, tokens, secrets, or production authentication architecture.

## Work package D - 502 classification

Treat M8-D09 as `not-reproducible-retain-watch` unless local evidence proves otherwise. Packet 012D may add bounded diagnostics and runbook classification, but must not claim a local fix without reproducing and correlating the failure. Integrated observation belongs to Packet 012E.

## Work package E - runtime logs and provenance

Classify repository files as source/documentation, runtime diagnostics, cache/test artifacts, or secret-bearing exclusions.

Required policy:

- runtime logs are not source provenance;
- stale random ngrok endpoints are not current connector truth;
- endpoint authority comes from accepted runbook/configuration plus live verified state;
- logs may be retained for bounded diagnostics but must not enter source manifests or implementation returns by default;
- no deletion or cleanup under Packet 012D;
- `.gitignore` changes are allowed only to prevent future runtime noise without hiding accepted source evidence;
- log contents containing stale or sensitive endpoint material must not be returned.

## Required verification

Run as separate bounded steps:

1. Ruff using the existing declared configuration;
2. focused health and registry tests;
3. lifecycle and occupied-port tests;
4. fixed-root, exact-path mutation, and capability-boundary tests;
5. full pytest suite;
6. compileall;
7. relevant static capability scans over every changed Python and PowerShell path;
8. hidden-Unicode and binary integrity scan;
9. exact changed-path verification and staged diff review;
10. one local Go8 commit with no remote or push.

No new dependency is authorized.

## Allowed candidate paths

```text
README.md
TOOLS.md
CHANGELOG.md
IMPLEMENTATION_RETURN.md
.gitignore
start.ps1
src/chatgpt_mcp/server.py
src/chatgpt_mcp/models.py
src/chatgpt_mcp/extra_tools.py
relevant lifecycle or configuration modules
relevant tests
```

A new narrow script or runbook file may be added only when existing files cannot hold the consolidated instructions cleanly. No generic shell or process manager may be introduced.

## Required implementation return

Return starting and ending HEADs, exact changed paths and hashes, health derivation sources, unique registry names/count, occupied-port and duplicate-start proof, authoritative runbook path, M8-D09 classification, runtime-log provenance policy, all verification results, local commit hash, and confirmation of no remote, push, connector mutation, process restart, canonical mutation, Kaizen MCP change, platform change, or cleanup.

## Acceptance criteria

Packet 012D is complete only when:

1. health derives authoritative package, FastMCP, and live registry evidence;
2. exact unique tool names and count are tested;
3. missing runtime and occupied port fail clearly;
4. duplicate start cannot kill or replace another process;
5. local, tunnel, health, and connector-refresh instructions are consolidated;
6. M8-D09 remains honestly classified unless reproduced locally;
7. runtime logs are separated from source provenance and endpoint authority;
8. Ruff, focused tests, full tests, compile, capability, and integrity checks pass;
9. changed paths equal the reviewed allowlist;
10. one local commit is created with no remote or push;
11. a separate completion audit passes;
12. owner accepts exact completion evidence before Packet 012E begins.

## Stop gates

Stop when a checkpoint differs, implementation requires connector mutation or live process interference, registry derivation needs unsupported internals, authority would widen, a test could kill or replace a process, runtime logs would expose secrets, M8-D09 is being called fixed without reproduction, or scope drifts into Packet 012E, Kaizen MCP, platform, canonical mutation, or cleanup.

## Explicit non-scope

- Kaizen MCP or platform changes;
- connector refresh, recreation, or mutation;
- live process termination or restart;
- canonical or live staging mutation;
- remote creation or push;
- production MCP migration;
- dependency upgrades;
- cleanup or deletion;
- Packet 012E implementation;
- Milestone 9 execution;
- Postgres, Qdrant, Hermes, UI, or deferred systems.

## Owner implementation gate

Implementation must not begin until the owner approves the exact audited Packet 012D SHA-256.
