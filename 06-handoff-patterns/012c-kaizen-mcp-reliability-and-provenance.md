---
id: kz-tp-01KTW3G2J4K6M8N0PQRSTUVWX
type: task-packet
status: draft
project: kaizen-platform
created: 2026-06-11T20:42:00Z
updated: 2026-06-11T20:42:00Z
review_status: pending-review
authority: proposed
summary: "Packet 012C Kaizen MCP adapter, lifecycle, health, registry, provenance, and operator-runbook reliability."
primary_spec: kz-spec-01KTW2M8N4Q6S8W0Y2Z4ABCDEF
---

# Task Packet 012C - Kaizen MCP Reliability and Provenance

> Kaizen MCP proving-ground only. This packet draft authorizes no implementation until the owner approves its exact audited SHA-256. It does not authorize connector mutation, process termination or restart, canonical mutation, platform or Go8 changes, production MCP migration, Git initialization, or remote creation.

## Objective

Close the Kaizen MCP portions of Milestone 8 defects M8-D01, M8-D03 through M8-D06, M8-D11, and M8-D12 by proving the adapter-level prepared-candidate loader contract, making health and registry evidence derive from authoritative runtime/package state, defining deterministic local lifecycle behavior and operator instructions, resolving the orphan PID artifact policy, and preserving exact non-Git provenance for every approved change.

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

Packet 012B owner completion acceptance SHA-256:
e96bc91c7d40ce046c504a0e4509dce3b062b2932539a34da2b2bbf545d60292
```

## Repository and implementation boundary

Only this proving-ground root is in implementation scope:

```text
C:\dev\kaizen-mcp
```

It is intentionally not a Git repository. Packet 012C must not initialize Git, create a remote, or present it as production infrastructure.

Expected runtime checkpoint:

```text
service: kaizen-mcp
version: 0.1.0
FastMCP: 3.4.2
registered tool declarations: 14
mutations enabled in current runtime: report, do not assume
remote enabled: false
```

## Read-first paths

```text
README.md
AGENTS.md
pyproject.toml
.env.example
ngrok.yml
docs/BOUNDARIES.md
docs/TOOL_CONTRACTS.md
docs/UPSTREAM_MUTATION_GATING.md
src/kaizen_mcp/__init__.py
src/kaizen_mcp/config.py
src/kaizen_mcp/adapter.py
src/kaizen_mcp/server.py
scripts/bootstrap.ps1
scripts/start-local.ps1
scripts/start-ngrok.ps1
scripts/start-stdio.ps1
scripts/test.ps1
tests/test_amendment_adapters.py
tests/test_boundaries.py
tests/test_server_tools.py
```

Do not read or return `.env` contents. Hash-only provenance is allowed. Runtime PID and tunnel artifacts are not source authority.

## Verified planning findings

### Adapter loader

`load_prepared_amendment_candidate_adapter` correctly resolves live scope only when `packet_id` is supplied. With omitted packet binding it delegates without live scope while the platform still verifies operation and all evidence hashes. Packet 012B proved the platform layer; Packet 012C must prove exact adapter forwarding, no mutation gate, no write mutex, and preserved structured platform errors.

### Health and registry

Current health hard-codes version `0.1.0` and omits FastMCP version and actual registered tool count. Package version also exists in `pyproject.toml` and `src/kaizen_mcp/__init__.py`, creating drift risk. The live server currently exposes 14 tool declarations, but no test derives and compares the runtime registry.

### Lifecycle and runbook

`start-local.ps1` checks only for the virtual-environment interpreter and launches the server. It does not preflight port 8765 or explain duplicate-start behavior.

`start-ngrok.ps1` refuses startup unless remote authentication is declared reviewed and an ngrok token is present. The README still says remote startup is blocked, while real connector use has relied on a separately started tunnel. Packet 012C must reconcile documentation without weakening the current local-only bind and remote-auth safety posture.

### PID artifact

`kaizen-mcp.pid` exists, but no current source, script, test, or documentation assigns it authoritative semantics. It must not be trusted to identify or kill a process.

### Provenance

Because the root is non-Git, every implementation return must preserve before/after SHA-256 manifests, exact changed paths, exact diffs, tests, Ruff, compile, and capability scans. `.env`, credentials, `.venv`, caches, PID contents, and tunnel logs must not appear in returned evidence.

## Work package A - adapter C1 proof

Add focused tests proving the MCP adapter:

- is usable when mutations are disabled;
- forwards `operation_id` exactly;
- forwards supplied `packet_id` exactly;
- omits live-scope resolution when packet binding is omitted;
- resolves and forwards verified live scope when packet binding is supplied;
- returns platform result fields verbatim;
- preserves exact platform error codes for missing, recovery-required, hash mismatch, binding mismatch, wrong packet, stale live binding, changed evidence, and invalid validation evidence;
- creates no plan, approval, event, temporary evidence, canonical mutation, or Git change;
- does not enter the canonical promotion mutex or preparation write mutex.

Prefer adapter tests using monkeypatched platform boundaries. Do not touch live staging or canonical repositories.

## Work package B - health and registry contract

Health must report:

```text
service
package version
FastMCP version
transport
host
port
public endpoint class or accepted public URL
fixed roots
mutation status
remote status
actual registered tool count
```

Requirements:

- derive package version from one accepted source;
- derive FastMCP version from installed package metadata;
- derive actual registered tool names and count from the live FastMCP registry or the narrowest supported introspection boundary;
- reject duplicate tool names;
- test the exact 14-tool registry at the approved checkpoint without merely comparing two copied literals;
- expose no tokens, `.env` values, private identities, or secret-bearing paths beyond accepted fixed roots.

Do not add a new health API or public tool. Update the existing `kaizen_health` result only.

## Work package C - lifecycle behavior and operator runbook

Define and test the smallest local lifecycle contract:

```text
local host: 127.0.0.1
local port: 8765
MCP path: /mcp
public endpoint: https://kaizen.ngrok.dev/mcp
```

Required behavior:

- missing virtual environment fails clearly;
- occupied port is detected before server launch and fails clearly;
- duplicate start does not kill, replace, or attach to an existing process;
- process-scoped PowerShell execution-policy bypass is documented; no machine-wide change;
- stop procedure identifies the listener and verifies ownership before any separately owner-authorized termination;
- restart sequence requires stopped listener, fresh local start, health pass, then connector refresh;
- health-before-mutation is explicit;
- no service, scheduler, daemon, auto-start, or automatic process kill;
- no connector mutation during implementation tests.

Lifecycle tests must use disposable ports or static script tests. They may not start, stop, or kill the real Kaizen MCP runtime under this packet unless separately authorized.

## Work package D - ngrok and remote posture reconciliation

Do not silently weaken authentication policy.

The packet may:

- document the currently supported separate-tunnel workflow;
- document the exact custom-domain command or accepted `ngrok.yml` invocation;
- distinguish local server startup from tunnel startup;
- explain that `remote_enabled` concerns non-loopback server bind, not whether a separate reverse tunnel exists;
- preserve the current requirement that the server itself binds locally;
- preserve the guarded `start-ngrok.ps1` unless owner-approved evidence justifies a narrower change.

Connector refresh and integrated upstream proof belong to Packet 012E.

## Work package E - PID policy

Preferred disposition: retire reliance on the orphan `kaizen-mcp.pid` artifact and document port/health/process ownership as authoritative.

The implementation must not delete the file merely because it is stale. Deletion or cleanup is separately gated. Packet 012C may:

- add tests proving scripts and code ignore it;
- document that it is non-authoritative;
- add it to provenance exclusions where appropriate;
- propose later owner-authorized cleanup.

Implementing a new PID contract is allowed only if exact evidence proves port/health ownership checks are insufficient and the packet is re-audited before that expansion.

## Work package F - non-Git provenance

Before editing, capture a bounded source manifest covering approved source, tests, scripts, README, pyproject, and operator docs. After editing, capture the same manifest and exact changed paths.

Required return:

- before/after SHA-256 per changed source file;
- exact text diff for every changed file;
- explicit unchanged sensitive/runtime exclusions;
- no `.env` content;
- no secret values;
- no `.venv`, cache, PID content, or tunnel log content;
- reproducible test commands and results.

## Allowed candidate paths

Subject to exact need after failing tests or static evidence:

```text
README.md
docs/BOUNDARIES.md
docs/TOOL_CONTRACTS.md
docs/UPSTREAM_MUTATION_GATING.md
src/kaizen_mcp/__init__.py
src/kaizen_mcp/adapter.py
src/kaizen_mcp/server.py
scripts/start-local.ps1
scripts/start-ngrok.ps1
scripts/test.ps1
tests/test_amendment_adapters.py
tests/test_boundaries.py
tests/test_server_tools.py
```

`pyproject.toml`, `.env.example`, `ngrok.yml`, `config.py`, bootstrap, or stdio scripts may be edited only when exact inspection proves the smallest required correction. No new dependency without a separate packet correction and owner approval.

## Required verification

Run as separate bounded steps:

1. Ruff using existing declared configuration;
2. focused adapter-loader tests;
3. focused health/registry tests;
4. lifecycle and occupied-port tests using disposable/static boundaries;
5. fixed-root and mutation-boundary tests;
6. full pytest suite;
7. compileall;
8. static capability scans over every changed Python and PowerShell path where supported;
9. before/after non-Git manifest comparison;
10. hidden-Unicode and binary scan;
11. exact changed-path verification.

## Required implementation return

Return:

- exact starting source manifest;
- exact ending source manifest;
- exact changed paths and diffs;
- adapter C1 case-to-test mapping;
- health fields and actual registry names/count;
- package and FastMCP version derivation sources;
- lifecycle behavior and occupied-port proof;
- final PID disposition;
- ngrok/runbook reconciliation;
- Ruff, focused, full-suite, compile, capability, and integrity results;
- confirmation that no Git repository or remote was created;
- confirmation that no process, connector, canonical, platform, Go8, vault, or live staging change occurred.

## Acceptance criteria

Packet 012C is complete only when:

1. adapter-level C1 behavior is directly proven;
2. health derives authoritative package, FastMCP, and runtime registry evidence;
3. exact unique tool names and count are tested;
4. duplicate start and occupied port fail clearly without process killing;
5. runbooks distinguish local server, separate tunnel, connector refresh, and mutation status;
6. orphan PID artifact is explicitly non-authoritative or an exact audited replacement contract exists;
7. before/after non-Git provenance is complete and excludes secrets/runtime noise;
8. Ruff, focused tests, full tests, compile, capability, and integrity checks pass;
9. no public authority or arbitrary capability is widened;
10. a separate completion audit passes;
11. owner accepts exact completion evidence before Packet 012D begins.

## Stop gates

Stop when:

- source/runtime checkpoints differ unexpectedly;
- implementation would require Git initialization or remote creation;
- a test would touch live staging, vault, connector, or real runtime process state;
- authentication policy would be weakened;
- a script would kill or replace a process automatically;
- registry introspection requires unsupported private internals without an audited fallback;
- a dependency or public API expansion appears necessary;
- `.env` or secret content would need to be returned;
- scope drifts into Go8, connector integration, Packet 012D/012E, production MCP migration, or cleanup.

## Explicit non-scope

- Go8 changes;
- platform changes;
- connector mutation or integrated connector proof;
- process termination or restart;
- canonical mutation;
- live staging mutation;
- Git initialization, remote creation, or push;
- production MCP packaging or migration;
- generalized filesystem, shell, SQL, Git, remote, or process tools;
- dependency upgrades;
- cleanup or deletion;
- Packet 012D or 012E implementation;
- Milestone 9 execution;
- Postgres, Qdrant, Hermes, UI, or deferred systems.

## Owner implementation gate

Implementation must not begin until the owner approves the exact audited Packet 012C SHA-256.
