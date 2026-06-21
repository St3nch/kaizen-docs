# Milestone 13 — Operational Service and Safe Local Tool-Surface Hardening

Status: draft milestone definition
Date: 2026-06-21
Milestone: 13
Recommended roadmap lane: Kaizen v1 internal operating completion

## 1. Purpose

Milestone 13 makes Kaizen's operational service and local tool surface safe enough to support a real internal project in Milestone 14.

The milestone exists because Milestones 11 and 12 proved the operational database foundation and recovery realism, but a real v1 project run needs a hardened caller-facing layer before Kaizen relies on operational records during real work.

Milestone 13 is not a product-surface milestone. It is not the Tauri milestone, not the Hermes milestone, not the Qdrant milestone, not the Observatory milestone, and not a Neon Ronin or SearchClarity implementation milestone.

## 2. Current entry conditions

At draft time, the required roadmap conditions are satisfied or available as follows:

```text
Milestones 1-12: closed
Packet 018A: closed
ROADMAP_V0.4: accepted and active
SP-1: closed by owner acceptance in Result 326
```

Current docs checkpoint when this definition was drafted:

```text
docs HEAD: d74bd6f9667d3fa7b9608bea5913f9f101bab84f
branch: main
working tree: clean
upstream: origin/main
ahead/behind: 3 / 0
```

## 3. Governing sources

```text
ROADMAP_V0.4.md
SHA-256 after SP-1 correction: dbdc37f3e77981b2d701dc546ad0ed0c8d009c7d51960375b6ba754ab5550837

03-research-results/322-roadmap-v0.4-owner-acceptance.md
SHA-256: 70d553726a365226f49bc43afc9e72809d60ca6f0f8c03b997ef7eb213a6267d

03-research-results/326-sp-1-owner-acceptance.md
SHA-256: 884eca7f56f228824b34cc6d0bced27378dcac42186c6f43322857b4d66f8fc0

04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md
SHA-256: c8509d575df11816de4d7dc48cb9b0fb84ac523d4defb5919ccd384eb491731a

04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md
SHA-256: 8a5b915ba3db9b558f551caa34d191c1ee212af70f088c896b9169cfa593f845

05-specs/milestone-12-recovery-realism-and-operational-restore-proof.md
SHA-256: 887a5ccfa7d88f9790e21d0cf0a7550e35665289f2632aa4ff47168a274a4b82
```

## 4. Accepted upstream boundaries carried forward

Milestone 13 must preserve these accepted boundaries:

```text
Markdown / Obsidian remains canonical project intelligence.
Operational Postgres owns accepted structured operational records only through typed boundaries.
Artifact and evidence bytes remain files, not database convenience payloads.
Canonical governance JSONL and immutable staging evidence keep their authority.
Agents, MCP clients, and generic scripts do not receive direct SQL or broad credentials.
Qdrant owns no truth and remains deferred.
Hermes and other agents remain clerks, not authorities.
```

Decision 0020's first-slice operational families remain the first operational foundation slice:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

The following remain deferred unless a later accepted decision changes them:

```text
governed-operation read model
connector invocation telemetry
Observatory / IMI
customer-data operations
multi-user identity
production deployment
```

## 5. Milestone objective

Milestone 13 should produce a service and local tool posture that is safe for Milestone 14's first real internal project run.

The objective is to prove:

```text
typed service boundaries are clear and enforced;
local tool surface is bounded and non-generic;
operator commands are small, explicit, and recoverable;
operational-record writes are lifecycle-valid and idempotent;
reads are project-scoped and do not leak arbitrary data;
backup, restore, and retention behavior remain compatible with M12 proof;
M14 can use the operational foundation without arbitrary SQL or hidden authority.
```

## 6. In-scope work families

Milestone 13 may include the following planning and implementation families after accepted task packets.

### 6.1 Service boundary inventory and hardening

Inventory current operational service interfaces and harden boundaries around:

```text
project-scoped reads
first-slice write commands
lifecycle transitions
idempotency keys
retry and duplicate behavior
error taxonomy
secret and DSN handling
file/database consistency checks
```

### 6.2 Safe local tool-surface inventory and hardening

Inventory Go8 / local tool behavior relevant to Kaizen and define a safe local surface for real project work:

```text
fixed roots only
no arbitrary filesystem mutation
no arbitrary shell
no direct database credentials
no broad SQL
bounded file reads
explicit write tools with create-only or optimistic-hash controls
exact changed-path reporting
clean Git state checks
```

Milestone 13 may improve local tool contracts only through approved implementation packets.

### 6.3 Minimal operator command surface

Define and prove a minimal operator surface for human-reviewed local work:

```text
health
status
bounded inventory
bounded evidence reads
typed service smoke checks
prepare / inspect / approve / execute / recover only where already designed
```

The operator surface should be boring, inspectable, and hard to misuse.

### 6.4 Synthetic operational proof

Use synthetic or disposable data to prove service and tool behavior without touching live production-like evidence incorrectly.

Acceptable proof areas:

```text
bounded reads
lifecycle-valid write attempts
duplicate/idempotency behavior
lock behavior
rejection of invalid lifecycle transitions
rejection of stale references
database/file consistency checks
restore-compatible smoke behavior
```

### 6.5 M14 readiness contract

Define what Milestone 14's real internal project run may rely on:

```text
which operational records may be created;
which service methods are approved;
which local tools are approved;
which failure modes require stop/escalation;
which evidence must be captured for implementation return;
which backup / restore checks are required for project data.
```

## 7. Out-of-scope work

Milestone 13 does not authorize:

```text
Milestone 14 project execution
Neon Ronin implementation
SearchClarity implementation
Observatory / IMI implementation
OBR implementation
provider purchase
crawling or logged-in marketplace collection
customer-data reuse
Qdrant
Hermes integration or write authority
Tauri or broad UI implementation
production MCP packaging
production deployment
multi-user identity
new operational record families beyond accepted first-slice hardening
retention deletion or physical evidence deletion
direct agent SQL
arbitrary SQL APIs
broad schema redesign
vault mutation except through separately approved governed Kaizen workflows
Decision 0015 doctrine creation
fabrication of missing M11 audit text
```

## 8. Required deliverables

Milestone 13 should produce the following deliverables through one or more task packets.

```text
service-boundary inventory
local tool-surface inventory
M13 implementation task packet(s)
service hardening implementation return
local tool-surface hardening implementation return
focused tests / hammer proof where appropriate
M14 readiness contract
closure audit
owner closure acceptance
active reading-path refresh check
```

## 9. Required proof

Milestone 13 closure must prove:

```text
typed service boundaries verified;
safe local tool surface verified;
minimal operator command surface proven on synthetic or disposable data;
no arbitrary SQL introduced;
no uncontrolled mutation introduced;
bounded command envelopes documented;
idempotency and lock behavior tested where relevant;
retention and recovery compatibility checked;
operational-record authority / rebuildability decision recorded or explicitly deferred;
M14 readiness contract exists;
working trees are clean after implementation;
no unauthorized paths changed.
```

## 10. Suggested packet breakdown

To keep the milestone from turning into a swamp monster, split it into small packets.

### Packet 019A — M13 inventory and readiness audit

Purpose:

```text
Inventory current service/tool surface and produce a concrete M13 hardening plan.
```

Likely outputs:

```text
service-boundary inventory
local-tool inventory
risk register
recommended packet sequence
M14 readiness gaps
```

No implementation.

### Packet 019B — service-boundary hardening

Purpose:

```text
Harden operational service behavior needed for real internal project evidence.
```

Possible scope:

```text
lifecycle validation
idempotency behavior
project-scoped reads
error taxonomy
safe DSN/secret handling
```

### Packet 019C — local tool-surface hardening

Purpose:

```text
Harden or document the safe local tool surface used by ChatGPT / steward workflows.
```

Possible scope:

```text
fixed-root operations
bounded reads
safe create-only writes
hash-controlled replacements
Git status/diff/commit discipline
blocked generic execution posture
```

### Packet 019D — synthetic proof and M14 readiness contract

Purpose:

```text
Prove the hardened surface on synthetic/disposable data and define what M14 may rely on.
```

Possible scope:

```text
synthetic proof
readiness contract
closure audit inputs
active reading-path refresh check
```

The packet split may be revised after Packet 019A inventory.

## 11. Milestone acceptance criteria

Milestone 13 definition is ready for owner acceptance when the owner confirms:

```text
M13 is a hardening milestone, not an implementation of Qdrant/Hermes/Tauri/Observatory/Neon Ronin/SearchClarity;
SP-1 is complete;
M13 may begin with inventory before implementation;
consequential implementation still requires task-packet approval;
M14 remains the first real internal project run, not part of M13;
```

Milestone 13 is complete only when:

```text
accepted M13 task packets complete;
required proof is recorded;
M14 readiness contract exists;
closure audit passes;
owner accepts Milestone 13 closure;
active reading path is verified or refreshed.
```

## 12. Non-authorization

This draft definition does not authorize implementation.

Implementation requires owner-approved task packet(s) with exact scope, starting commit, tests, verification requirements, and non-authorization boundaries.
