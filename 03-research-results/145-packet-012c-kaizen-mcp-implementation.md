---
id: kz-result-01KTW3J6M8N0PQRSTUVWXYZA
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-11T20:55:00Z
updated: 2026-06-11T20:55:00Z
review_status: steward-reviewed
authority: proposed
summary: "Packet 012C Kaizen MCP adapter, health, lifecycle, runbook, PID, and non-Git provenance implementation result."
---

# Research Result 145 - Packet 012C Kaizen MCP Implementation

## Authority

```text
Packet 012C SHA-256:
ff68fcd554dd79f1789ab189d781653039c8c49a6639f19c7bf81254fafedfaf

Owner implementation approval:
exact phrase supplied in active governed session
```

## Starting checkpoint

```text
root: C:\dev\kaizen-mcp
Git repository: no
service version: 0.1.0
FastMCP dependency: 3.4.2
registered tool declarations: 14
runtime mutations enabled at implementation start: true
runtime remote enabled: false
```

No process restart or connector refresh was authorized or performed. The running connector therefore remained on the pre-change process image during implementation.

## Exact changed paths and before/after SHA-256

```text
README.md
before: f215da35accd4b1ebad522e2f80062bf4021ecde0f16e542250157808ab53035
after:  139d3557bdd3e629b59631144f1c2c2a5af29b337352836ae370dda579b5b79c

docs/TOOL_CONTRACTS.md
before: ae7b4523af1f8d04a258cc7bf852236fcc7808be767271bbc1a8d0d469e968c1
after:  72ed0306d6cf819a7176fb5842102ab3c8b25b4316d995c5704e9585d8a66986

scripts/start-local.ps1
before: 2a230390d6a9f17c18748da7109a2a8093234f2ce53eeee0e0eb824ab21e887f
after:  6adb3bfe2478a0d15adf54fb1e57241b8ae4a1cc2af44c0f8facde397aa2cdda

src/kaizen_mcp/adapter.py
before: a4f1592bbaff042348b630d02e3f8fcbc9141908198bbb9e1128e9783d111c6e
after:  cfec265b402826d163b9c4f2d2428be42b15f4388cba498e9a78d85cdacb6c24

src/kaizen_mcp/server.py
before: b9e4a08c3ee29c3924a6a757c4a2b18909f353725fb9495c773e9316ff4c9370
after:  4d02b75ed7041ebef9e9fe9aadd2931fb856ced70259866de72908dcc1462aff

tests/test_amendment_adapters.py
before: 16550158242ee94e32ffcc864011d64ca8ec47562c1b24f6ccf3b6464d0fab59
after:  4eb27ac3934c2546f91a4b2a735028d3f880f56d2a485c4657100676eb670252

tests/test_boundaries.py
before: e8ca4faf1263e3ee7147c52f78ac435ea6f700f871ff7f267b727b34038d6601
after:  1d66f86de6ce1a974449fe120830e1768d37d5b8b7c4ecbbff37be7e2f71b6eb

tests/test_server_tools.py
before: 232131a3101221e6e0d4d501cfb9d59d84e9fdf01e669ce1418e5077b55959d4
after:  2387be2adf395d25761aa1808beb290e59e5f8707cd976faeb78e128160f4ce5
```

No `.env` contents, token values, PID contents, `.venv`, cache, or tunnel-log contents were read into completion evidence.

## Adapter C1 proof

The adapter implementation already matched the platform contract and required no production correction.

Added tests prove:

- the read-only loader works while mutations are disabled;
- supplied packet binding resolves and forwards live scope;
- omitted packet binding does not resolve live scope;
- operation ID, packet ID, and promotion scope are forwarded exactly;
- platform results are returned verbatim;
- platform structured errors propagate without adapter remapping;
- no mutation gate is entered for the loader.

The platform C1 error matrix remains proven by Packet 012B. Packet 012C proves the adapter does not alter that contract.

## Health and registry implementation

The existing `kaizen_health` tool now derives:

```text
package version: importlib.metadata.version("kaizen-mcp")
FastMCP version: importlib.metadata.version("fastmcp")
registered tool names/count: FastMCP mcp.list_tools()
```

The registry coupling is isolated in one helper in `server.py`. An initial attempt to use a private `_tool_manager._tools` dictionary failed under FastMCP 3.4.2 and was removed. The final implementation uses the same supported `list_tools()` interface already exercised by existing tests.

Health now reports:

```text
service
version
fastmcp_version
transport
host
port
public_url
mutations_enabled
remote_enabled
tool_count
fixed_roots
```

Tests prove 14 unique live tool names and exact count. Duplicate names fail closed in the health adapter.

## Lifecycle implementation

`scripts/start-local.ps1` now:

- verifies the intended virtual-environment interpreter exists;
- performs a short loopback bind probe on port 8765;
- releases the probe before server launch;
- refuses duplicate startup when the port is occupied;
- performs no process lookup, kill, replacement, or attachment.

A static test verifies the script contains the bind probe and contains no `Stop-Process` or `taskkill` behavior.

The real port 8765 listener was not disturbed and the live process was not restarted.

## Runbook and remote posture

README and tool-contract documentation now state:

- local endpoint: `http://127.0.0.1:8765/mcp`;
- accepted public endpoint: `https://kaizen.ngrok.dev/mcp`;
- process-scoped PowerShell execution-policy bypass form;
- health-before-mutation requirement;
- duplicate-start refusal;
- ownership verification before any separately authorized process termination;
- restart order: stopped listener, local start, health pass, connector refresh;
- separate reverse tunnel does not make `remote_enabled` true;
- loopback binding and guarded remote-auth posture remain unchanged;
- connector refresh belongs to integrated upstream work, not local startup.

## PID disposition

```text
kaizen-mcp.pid: non-authoritative orphan runtime artifact
```

Current code and scripts do not create, validate, consume, or remove it. The documentation explicitly forbids killing a process based only on that file. The file was not deleted because cleanup is separately gated.

## Verification results

```text
focused adapter/server/boundary tests:
24 passed, 1 third-party deprecation warning

full pytest suite:
24 passed, 1 third-party deprecation warning

Ruff:
All checks passed

compileall src + tests:
pass

static Python capability scans:
changed source files: no findings
changed test files: no findings

final changed-file manifest:
complete, eight files, UTF-8 text, LF line endings
```

The warning originates from OpenTelemetry's importlib metadata compatibility layer and is unrelated to Packet 012C.

One requested hidden-Unicode scan route was repeatedly blocked upstream before reaching Go8. No scan result is claimed. All changed files were returned by Go8 as UTF-8 text with LF metadata; Ruff, compileall, focused tests, full tests, and exact SHA-256 manifests passed.

## Boundary confirmation

```text
Git initialization: none
remote creation: none
dependency changes: none
platform changes: none
Go8 changes: none
connector mutation: none
process termination or restart: none
live port interference: none
canonical mutation: none
live staging mutation: none
production MCP migration: none
cleanup or deletion: none
Packet 012D or 012E work: none
```

## Completion status

```text
Packet 012C implementation: complete
Packet 012C completion audit: required next
Packet 012D: not authorized
```
