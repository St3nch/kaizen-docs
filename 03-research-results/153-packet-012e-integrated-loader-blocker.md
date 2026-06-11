---
id: kz-result-01KTW4A2BCDEFGHIJKLMNOP
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-11T22:05:00Z
updated: 2026-06-11T22:05:00Z
review_status: steward-reviewed
authority: proposed
summary: "Packet 012E integrated blocker: live prepared-candidate loader rejects omitted optional live binding."
---

# Research Result 153 - Packet 012E Integrated Loader Blocker

## Trigger

After verified restart and connector refresh, Kaizen MCP health matched the accepted runtime:

```text
service: kaizen-mcp
version: 0.1.0
FastMCP: 3.4.2
tool count: 14
mutations enabled: true
remote enabled: false
```

A read-only call to `kaizen_load_prepared_amendment_candidate` with an existing operation ID and omitted `packet_id` reached the server and returned:

```text
live_root_forbidden: Packet 006B implementation is restricted to disposable roots
```

## Root cause

`load_prepared_amendment_candidate_adapter` correctly omits live-scope resolution when `packet_id` is omitted.

The platform then calls `enforce_promotion_scope(config, scope=None)`. For fixed live roots, the current implementation calls `ensure_disposable_roots(config.canonical_root, config.staging_root)` before the later live-scope checks. That legacy disposable-root guard rejects the fixed live roots immediately.

Therefore the accepted C1 contract is not satisfied on the actual fixed-root integration path:

```text
omitted optional packet/live binding
must still verify operation binding and evidence integrity
must not require live-scope verification
```

Packet 012B proved the loader on disposable fixtures. Packet 012C proved adapter forwarding with mocked platform boundaries. Neither test exercised the real adapter plus platform loader against the fixed live root configuration.

## Classification

```text
layer: platform live-scope enforcement
upstream block: no
connector defect: no
Kaizen MCP registry defect: no
mutation: no
canonical change: no
staging change: no
contract contradiction: yes
```

## Side-effect verification

The inspected operation retained the exact same seven-file evidence set, sizes, and SHA-256 values after the failed loader call. No plan, approval, event, temporary evidence, Git change, canonical mutation, or staging mutation occurred.

## Required action

Packet 012E must pause. A narrow correction packet is required before integrated proof can continue.

Recommended correction:

```text
Packet 012E.1 - Fixed-root read-only loader scope correction
```

The correction must not weaken live mutation protections. It must distinguish the read-only loader's omitted optional live binding from mutation-capable promotion paths that still require verified live scope.
