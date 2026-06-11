---
id: kz-result-01KTW2TFN6Q8S0W2Y4Z6ABCDEF
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-11T20:40:00Z
updated: 2026-06-11T20:40:00Z
review_status: steward-reviewed
summary: "Packet 012A read-only defect register, reliability contract freeze, lint policy, and later-packet reconciliation."
---

# Research Result 137 - Packet 012A Defect Register and Contract Freeze

## Authority

```text
Packet 012A SHA-256:
067a3a883f7a12dd437c9d9513ed2499cd7f58914913dbae7938e2fb322ee92d

Owner approval:
03-research-results/136-packet-012a-owner-approval.md
```

## Verified checkpoints

### kaizen-docs

```text
branch: main
HEAD: 805c5d789d4d558cc53bbf882dc825b2c9e3bd88
upstream: origin/main
ahead/behind: 0/0
working tree: dirty only for approved Result 136 and this Packet 012A return
Roadmap v0.3 SHA-256: f8f5d20048169178111bd0f31fd41dc363777b90ff72957a5d19ec0b77b57f82
```

### kaizen-platform

```text
branch: main
HEAD: 1b8be1d6d42d768587dddb2be8415fa24b670561
clean: true
remotes: none
Python contract: >=3.11,<3.12
test dependency: pytest only
Ruff policy: not declared
```

### Go8

```text
branch: master
HEAD: 5eccbc71dff5752b083b6b2bd019416d4e5baf73
clean: true
remotes: none
service version: 0.4.0
FastMCP: 3.4.2
reported tool count: 44
fixed roots: chatgpt_mcp, docs, kaizen_mcp, platform, staging, vault
generic execution: false
```

### Kaizen MCP

```text
root: C:\dev\kaizen-mcp
Git repository: no
service version: 0.1.0
FastMCP dependency: 3.4.2
registered tool declarations: 14
mutations enabled in current runtime: true
remote_enabled in current runtime: false
```

Bounded source inventory hashes are preserved by the Packet 012A inspection evidence. The `.env` file was hashed but not read or returned. `.venv`, caches, and runtime artifacts were not treated as source provenance.

## Defect register

### M8-D01 - Prepared-candidate loader live-scope inconsistency

```text
disposition: already-fixed-but-unproven
recommended packet: 012B and 012C
consequence: correctness and Milestone 9 dependency
```

Current platform loader behavior:

- validates staging configuration and operation ID;
- enforces supplied promotion scope;
- verifies exact preparation file set and symlink safety;
- verifies preparation record hash;
- verifies operation binding;
- verifies optional packet binding;
- verifies optional live binding;
- verifies all evidence hashes;
- returns no mutation or event.

Current Kaizen MCP adapter resolves live scope only when `packet_id` is supplied and otherwise invokes the loader without live scope, matching the platform contract.

No evidence found that the stale guard remains in current code. However, the accepted current test suite does not yet prove the complete C1 matrix across platform and MCP layers. It cannot be called closed until those tests exist and pass.

### M8-D02 - Intermittent amendment concurrency error-code drift

```text
disposition: confirmed-defect
recommended packet: 012B
consequence: deterministic error contract and pilot reliability
```

Observed evidence:

```text
expected: idempotency_conflict
observed once: destination_path_invalid
focused reruns: 20 passed
full rerun: 276 passed
```

Preparation is serialized by `staging_write_mutex`, so mixed evidence was not observed. The mismatch arose while reading the canonical destination, where any native filesystem error is translated to `destination_path_invalid` before preparation evidence is created.

Root cause remains unproven. Packet 012B must determine whether the defect is implementation ordering, native handle behavior, fixture interference, or an over-specific test expectation. The safety property is stronger than the current error-code assertion: exactly one preparation wins and no contender may mix evidence.

### M8-D03 - Stale listener or duplicate-process startup behavior

```text
disposition: operator-runbook-gap plus test-gap
recommended packet: 012C and 012D
consequence: operator reliability
```

Observed sessions required manual `Get-NetTCPConnection`, process identification, and process termination after stale listeners. Neither current startup script performs a preflight port check or explains duplicate-start behavior.

No implementation defect is assigned until lifecycle tests reproduce the behavior. The accepted contract must fail clearly on occupied ports and must never kill an unrelated process automatically.

### M8-D04 - Unsigned PowerShell script invocation friction

```text
disposition: operator-runbook-gap
recommended packet: 012C and 012D
consequence: startup friction, not security-control failure
```

Observed Kaizen MCP startup failed under the current execution policy until invoked with process-scoped bypass. Current README examples call scripts directly and do not document the process-scoped safe form.

Machine-wide execution-policy changes are not required or authorized.

### M8-D05 - Fragmented startup and ngrok instructions

```text
disposition: confirmed operator-runbook-gap and stale-policy conflict
recommended packet: 012C and 012D
consequence: repeated startup mistakes and chat dependence
```

Go8 has a root `start.ps1` but no repository ngrok script. Its README lists the public endpoint but not the exact custom-domain command.

Kaizen MCP has `start-local.ps1` and `start-ngrok.ps1`. The ngrok script refuses remote startup unless `KAIZEN_MCP_REMOTE_AUTH_READY` has a special value and `NGROK_AUTHTOKEN` is present, while the live connector workflow has used a separately started custom-domain tunnel. README text still says remote startup is blocked until authentication is implemented.

Packet 012A does not decide to weaken that safety statement. Packet 012C must reconcile the real connector posture, the meaning of `remote_enabled`, tunnel startup, and any authentication requirement before changing scripts or docs.

### M8-D06 - Health version and tool-count registry drift risk

```text
disposition: confirmed observability-gap and test-gap
recommended packet: 012C and 012D
consequence: stale connector and runtime diagnosis
```

Go8 health hardcodes version `0.4.0`, framework `fastmcp-3.4.2`, and tool count `44`. Its test also asserts the same literal `44`; that proves consistency between two literals, not equality with the live registry.

Kaizen MCP health hardcodes version `0.1.0`, omits FastMCP version, and omits tool count. The package version also appears in `pyproject.toml` and `__init__.py`, creating three drift points.

### M8-D07 - Connector tool discovery stale after restart or recreation

```text
disposition: upstream-only with integrated watch proof
recommended packet: 012E
consequence: operator diagnosis
```

Observed connector schemas sometimes remained absent or stale until the connector was refreshed, deleted/recreated, or added to the active chat. Local servers were healthy during some of these incidents.

No local code fix is justified. Packet 012E must document and field-prove the refresh/recreate sequence and distinguish live registry state from ChatGPT connector state.

### M8-D08 - Upstream safety block before server execution

```text
disposition: upstream-only
recommended packet: 012E documentation and observation
consequence: workflow interruption, no local mutation
```

Multiple tool calls were blocked before reaching Go8 or Kaizen. Operation status repeatedly confirmed no events or mutations. Exact retries sometimes succeeded without local changes.

No unsafe bypass is authorized. Evidence must distinguish:

```text
upstream block
server rejection
platform rejection
successful server execution
```

### M8-D09 - Go8 502 behavior after long-running sessions

```text
disposition: not-reproducible-retain-watch
recommended packet: 012D classification and 012E observation
consequence: session reliability
```

Go8 returned repeated 502 responses during a long session; restarting server and tunnel restored service. Current evidence cannot isolate server, tunnel, connector, session, or platform cause.

Packet 012D may add local health and lifecycle diagnostics. It must not claim a local fix unless the failure is reproduced locally or correlated with local evidence.

### M8-D10 - Ruff availability and repository lint-policy mismatch

```text
disposition: confirmed policy-gap
recommended packet: 012B, 012C, and 012D
consequence: inconsistent completion evidence
```

Kaizen MCP and Go8 both declare Ruff as a development dependency and define lint rules. Kaizen MCP's `scripts/test.ps1` runs Ruff, pytest, and compileall.

The platform declares only pytest in its development dependencies and no Ruff configuration. Earlier completion evidence treated Ruff unavailability as non-blocking.

Accepted policy:

```text
platform: pytest and compileall required; Ruff not required in Milestone 8 unless a separately reviewed Packet 012B change adds a platform lint contract
Kaizen MCP: Ruff, pytest, and compileall required because they are already declared
Go8: Ruff, pytest, and compileall required because they are already declared
```

No repository receives a new lint dependency merely for symmetry.

### M8-D11 - Kaizen MCP non-Git provenance and reproducibility

```text
disposition: confirmed provenance-gap
recommended packet: 012C
consequence: accepted change traceability
```

Kaizen MCP is intentionally non-Git. Current source provenance therefore depends on chat history and file state unless exact manifests are preserved.

Accepted Milestone 8 evidence contract:

- before/after SHA-256 manifest for bounded source files;
- exact changed-path scope;
- unified or exact text diffs;
- test, lint, compile, and capability-scan results;
- no `.env`, secret, `.venv`, cache, PID, or tunnel log contents in returned evidence;
- reproducibility archive or later Git decision remains separate and is not authorized by Milestone 8 definition alone.

### M8-D12 - PID-file lifecycle and stale PID behavior

```text
disposition: confirmed orphan-runtime-artifact and policy-gap
recommended packet: 012C
consequence: misleading operator state
```

`C:\dev\kaizen-mcp\kaizen-mcp.pid` currently contains a numeric PID, but no source, test, script, or documentation references the PID filename. The current `start-local.ps1` does not create, validate, or remove it.

The file therefore has no authoritative semantics and must not be trusted. Packet 012C must either implement and test an explicit PID contract or retire the artifact and document port/health-based lifecycle checks. It may not kill a process solely because a stale file names its PID.

## Additional finding M8-D13 - Tunnel log provenance and stale endpoint evidence

```text
disposition: documentation-and-retention-gap
recommended packet: 012D
consequence: confusing diagnostics
```

Go8 contains runtime ngrok logs that record an older random ngrok URL rather than the accepted custom domain. Runtime logs are useful diagnostics but are not authoritative connector configuration and should not be included in source provenance or treated as current runbook truth.

## Frozen contracts

## C1 - Prepared-candidate read-only contract

A read-only loader must:

1. validate fixed staging configuration and operation ID;
2. verify the exact complete preparation evidence set;
3. reject symlinks and unsafe paths;
4. verify preparation record and every evidence hash;
5. verify operation binding always;
6. verify packet binding when supplied;
7. verify live binding when a verified live scope is supplied;
8. permit omitted optional packet/live binding while still verifying operation and evidence integrity;
9. create no mutex, temporary evidence, approval, event, Git change, or canonical mutation;
10. return deterministic structured errors.

Accepted error classes:

```text
preparation_missing
preparation_recovery_required
preparation_hash_mismatch
preparation_binding_mismatch
live_packet_mismatch
live_vault_drift
preparation_evidence_changed
validation_evidence_invalid
```

Packet 012B and 012C may refine messages but must not collapse these into generic errors.

## C2 - Amendment concurrency contract

For requests sharing one operation ID:

- all preparation state transitions are serialized across processes;
- exactly one conflicting request may create the preparation;
- identical requests return the accepted preparation with `idempotent: true`;
- conflicting losers return `idempotency_conflict` unless a proven lower-level safety failure occurred before an operation binding existed;
- no candidate, prior bytes, diff, validation, metadata, timestamp, or hash may mix between requests;
- incomplete installed evidence returns recovery-required, never silent reuse;
- the accepted Packet 012B stress target is at least 100 focused conflicting rounds plus repeated full-suite and hammer coverage;
- stress evidence records error-code distribution and verifies exact evidence hashes.

If root cause proves the current expected loser code is wrong, Packet 012B must amend this contract through exact audit rather than silently changing the test.

## C3 - Lifecycle contract

### Go8

```text
local start: process-scoped PowerShell invocation of C:\dev\chatgpt-mcp\start.ps1
host: 127.0.0.1
port: 8770
path: /mcp
public endpoint: https://go8.ngrok.dev/mcp
```

### Kaizen MCP

```text
local start: process-scoped PowerShell invocation of scripts\start-local.ps1
host: 127.0.0.1
port: 8765
path: /mcp
public endpoint: https://kaizen.ngrok.dev/mcp
```

For both:

- startup checks required runtime and reports missing environment clearly;
- occupied port causes clear refusal;
- duplicate start never kills or replaces an existing process automatically;
- stop procedure identifies listener and verifies process ownership before owner-authorized termination;
- restart requires stopped listener, fresh start, health pass, then connector refresh;
- health must pass before mutation tools are used;
- process-scoped execution-policy bypass is acceptable; machine-wide policy change is not;
- no service, scheduler, daemon, or auto-start registration.

Kaizen MCP PID policy is unresolved for Packet 012C: implement a verified contract or remove reliance on the orphan artifact.

## C4 - Health and registry contract

Health must derive, not duplicate, authoritative values where feasible.

### Required Go8 fields

```text
service
package version
FastMCP version
fixed root names
actual registered tool count
generic execution posture
```

### Required Kaizen MCP fields

```text
service
package version
FastMCP version
transport
host
port
public endpoint class
fixed roots
mutation status
remote status
actual registered tool count
```

Tests must enumerate the runtime registry and compare exact unique tool names and count. Duplicate names fail. Package version must come from one accepted source. Health must expose no token, `.env` value, tunnel credential, private identity, or secret path content beyond accepted fixed roots.

Connector freshness is proven separately by comparing server registry evidence with connector-visible tools after restart and refresh.

## C5 - Test and lint contract

### Platform

Required:

- compileall or equivalent syntax check;
- focused loader tests;
- focused concurrency and dirty-state tests;
- at least 100 conflicting concurrency rounds in Packet 012B;
- existing hammer tests;
- full pytest suite;
- static capability checks where relevant.

Not required unless separately approved:

- Ruff dependency or formatting rewrite.

### Kaizen MCP

Required:

- Ruff using existing declared configuration;
- pytest full suite;
- compileall;
- adapter contract tests;
- health/registry tests;
- fixed-root and mutation-boundary tests;
- lifecycle-script static and disposable tests;
- before/after file manifest because repository is non-Git.

### Go8

Required:

- Ruff using existing declared configuration;
- pytest full suite;
- compileall;
- health/registry tests;
- fixed-root, exact-path mutation, and capability-boundary tests;
- lifecycle and port-conflict tests;
- Git diff and exact-path scope for every change.

## Failure-injection mapping

```text
012B:
wrong packet binding
stale live binding
tampered preparation evidence
concurrent conflicting requests
dirty canonical repository

012C:
stale PID
PID naming unrelated process
occupied port
duplicate start
missing virtual environment
unsigned-script policy
health/registry mismatch

012D:
occupied port
duplicate start
missing virtual environment
unsigned-script policy
health/registry mismatch
stale runtime log or endpoint evidence

012E:
connector stale after restart
upstream block before server execution
fresh server/connector registry comparison
cross-server stop/restart and health proof
```

No failure injection may terminate an unrelated process or mutate canonical content.

## Reconciled later packet split

### 012B - Platform loader and concurrency reliability

- prove or close M8-D01 at platform layer;
- root-cause and close M8-D02;
- prove dirty-repository and stale-binding behavior required by Milestone 9;
- preserve platform authority and no-remote posture.

### 012C - Kaizen MCP reliability and provenance

- align adapter tests with C1;
- implement truthful C4 health and registry contract;
- reconcile remote/tunnel posture and runbook;
- resolve PID policy;
- add lifecycle checks;
- preserve non-Git manifest evidence and exact bounded authority.

### 012D - Go8 reliability and runbooks

- derive truthful registry/tool count and version evidence;
- add lifecycle and port-conflict proof;
- classify local evidence for 502s without overclaiming;
- consolidate local and ngrok runbooks;
- classify runtime logs and source provenance.

### 012E - Integrated proof and governed return

- fresh start and tunnel proof;
- connector refresh/recreation proof;
- compare live registries with connector-visible tools;
- distinguish upstream blocks from local failures;
- run accepted integrated failure matrix;
- governed current-state return only if required;
- closure audit and owner acceptance.

## Unresolved questions for later packets

1. Is `destination_path_invalid` caused by platform implementation, Windows native handle timing, or a fixture race?
2. Should Kaizen MCP use a real PID contract or no PID file at all?
3. What authentication statement accurately describes the current custom-domain connector exposure without prematurely productionizing it?
4. Can Go8 502s be correlated with local process/tunnel evidence, or must they remain upstream/session watch items?
5. Can FastMCP's live registry be enumerated through a stable supported API, or should tests use the server's accepted internal registry interface?

## Conclusion

Packet 012A classifies two platform reliability items, several local policy/test/runbook gaps, three upstream or watch-list issues, one non-Git provenance gap, and one orphan PID artifact. C1 through C5 are frozen for implementation planning. No code, test, script, tool, dependency, connector, canonical, process, or runtime configuration change occurred.
