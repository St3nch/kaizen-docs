---
id: kz-result-01KTW3Q6RSTUVWXYZABCDE
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-11T21:18:00Z
updated: 2026-06-11T21:18:00Z
review_status: steward-reviewed
authority: proposed
summary: "Packet 012D Go8 health, lifecycle, runbook, 502 classification, and runtime-log provenance implementation result."
---

# Research Result 149 - Packet 012D Go8 Implementation

## Authority

```text
Packet 012D SHA-256:
9061ba483de5c07beebd18cdc8fcbf5b203596e58bb683cf0ce7f7426f5e1ae0

Owner implementation approval:
exact phrase supplied in active governed session
```

## Repository checkpoints

```text
repository: C:\dev\chatgpt-mcp
branch: master
starting HEAD: 5eccbc71dff5752b083b6b2bd019416d4e5baf73
ending HEAD: 312704a8b8505bdb64f28cc557171c10de8bd5bc
commit message: Improve Go8 health and lifecycle reliability
remotes: none
push: none
```

## Exact changed paths and SHA-256

```text
README.md
SHA-256: c3abf4299370ebbe8dac8c5a6d4bcc370dbecc469a780e6f7b0f1353869c045e

start.ps1
SHA-256: ddc1c27e9cda29b060c7de249533107ea56700ca8a892d319e2d85cf2a76438b

src/chatgpt_mcp/server.py
SHA-256: 589e3547df3d1079be7350c6bfde6f8561749abcd9adeb39d8a8890020a2908e

tests/test_server.py
SHA-256: 386b5f8f2ba30659142b6d51714f81cd88d3f9b00bbcfabddba4fe0bbe4cae49

tests/test_lifecycle.py
SHA-256: d2935aee3fc517e971bfd02fbb7c69bfc6ab0bf4cd71d58468988a3548f0ecf2
```

## Health and registry result

The implementation removed copied health literals as the source of truth.

```text
Go8 package version source:
pyproject.toml project.version

FastMCP version source:
importlib.metadata.version("fastmcp")

registered tool count source:
await mcp.list_tools()
```

A focused test first exposed that installed package metadata reported stale version `0.2.0` while `pyproject.toml` authoritatively declared `0.4.0`. Go8 package version is therefore derived from `pyproject.toml`, and the FastMCP server constructor uses the same helper. FastMCP itself remains derived from installed runtime metadata.

Health now verifies unique live registry names and returns the actual registry count. The accepted checkpoint remains 44 tools with generic execution disabled.

## Lifecycle result

`start.ps1` now:

- verifies `.venv\Scripts\fastmcp.exe` exists;
- performs a temporary loopback bind probe on port 8770;
- releases the probe before launching Go8;
- refuses startup clearly when the port is occupied;
- never kills, replaces, or attaches to another process;
- continues to bind only to `127.0.0.1:8770/mcp`.

The real Go8 process and live port were not stopped, restarted, or disturbed.

## Runbook result

`README.md` is now the consolidated lifecycle and tunnel runbook. It records:

- process-scoped PowerShell execution-policy bypass;
- local endpoint `http://127.0.0.1:8770/mcp`;
- public endpoint `https://go8.ngrok.dev/mcp`;
- separate server and tunnel lifecycles;
- health-before-connector ordering;
- ownership verification before any separately authorized process termination;
- restart order: stopped listener, local start, health pass, tunnel verification, connector refresh;
- no machine-wide execution-policy change;
- no automatic process killing.

## M8-D09 classification

```text
classification: not-reproducible-retain-watch
local fix claimed: no
```

No 502 condition was reproduced locally. The runbook now distinguishes possible local server, tunnel, connector, session, and upstream causes. Integrated field observation remains assigned to Packet 012E.

## Runtime-log provenance

Files under `logs/` are classified as runtime diagnostics, not source provenance or current connector configuration. Older random ngrok endpoints in logs are explicitly non-authoritative. Runtime-log contents remain excluded from implementation returns by default. No logs were deleted or cleaned.

## Verification evidence

```text
full pytest suite:
19 passed, 1 third-party deprecation warning

Ruff:
All checks passed

compileall src + tests:
pass

text-integrity scans:
pass for all five changed files

exact changed-path scope:
pass

staged diff check:
pass

static capability scan:
only pre-existing bounded subprocess use in Git helper and test fixture paths
no new generic execution capability
```

The warning originates from OpenTelemetry's importlib metadata compatibility layer and is unrelated to Packet 012D.

## Boundary confirmation

```text
connector mutation: none
process termination or restart: none
live port interference: none
Kaizen MCP changes: none
platform changes: none
canonical mutation: none
live staging mutation: none
remote creation: none
push: none
dependency changes: none
cleanup or deletion: none
Packet 012E work: none
```

## Completion status

```text
Packet 012D implementation: complete
Packet 012D completion audit: required next
Packet 012E: not authorized
```
