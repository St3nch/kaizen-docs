# Post-Milestone-11 Roadmap Revision Proposal

Status: proposed
Date: 2026-06-20

## Purpose

Propose the next roadmap posture after Milestone 11 and Packet 016G completion.

This document does not authorize implementation. It identifies the next decision Kaizen needs before any Milestone 12 work begins.

## Current checkpoint

```text
Milestones 1-11: closed
Milestone 11 closure: Result 288
Packet 016G completion: Result 293
Orientation sync: Result 294
Platform HEAD: 00d86943ca54aaff89c4c8428b7bb529994f846c
Docs HEAD before this proposal: 4d9c4580b22a19a6f9c852ead7cab77ce08f01c5
Vault HEAD: c898f261c0b341eb8419125247c8bd53ef567d6c
Operational database: kaizen_ops migrated through 0005_recovery_retention_integrity
```

## Governing constraints

Decision 0019 accepted these operational record families for later design:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
governed-operation read model
connector invocation telemetry
```

Decision 0020 accepted the first Milestone 11 slice as:

```text
execution attempt
verification run
return-bundle manifest
artifact provenance
```

Decision 0020 deferred:

```text
governed-operation read model
connector invocation telemetry
```

Result 293 completed Packet 016G but preserved explicit deferrals:

```text
durable restore smoke-check evidence
nonzero-file retained production proof
restore-forward support
operational role-ready restore proof
```

Therefore the next roadmap revision must not pretend these deferred items are closed.

## Recommended Roadmap v0.4 posture

Roadmap v0.4 should be a short post-Milestone-11 revision that says:

1. Milestones 1-11 are closed.
2. Operational database first-slice infrastructure exists and is migrated through 0005.
3. The next milestone is not automatically Observatory, Qdrant, Hermes, UI, or marketplace intelligence.
4. The next milestone must be chosen from evidence-backed candidates.
5. A short planning/audit packet should select Milestone 12 before implementation begins.

## Candidate Milestone 12 tracks

### Candidate A — Recovery Realism and Operational Restore Proof

Goal:

Prove the recovery system under realistic, nonzero-file, service-usable conditions.

Would address:

```text
post-restore typed-service smoke-check evidence
nonzero-file retained generation proof
restore-forward boundary
operational role-ready restore proof
bounded live negative probes for recovery/retention triggers
```

Why this is attractive:

- It directly follows the explicit 016G residual limitations.
- It strengthens trust in the database before adding new families.
- It reduces future risk before more operational surface is built.

Why it may be too conservative:

- It adds less visible product capability.
- It delays governed-operation status/query work.

### Candidate B — Governed-Operation Read Model

Goal:

Build the deferred non-authoritative read model for governed operation status from immutable evidence, governance JSONL, and Git bindings.

Would address:

```text
operation status projection
evidence replay
operator dashboard/query readiness
less manual status reconstruction
foundation for later UI/operator console improvements
```

Why this is attractive:

- It is directly deferred by Decision 0020.
- It makes Kaizen easier to operate after many packets.
- It remains rebuildable and non-authoritative, preserving governance boundaries.

Why it may be too soon:

- It adds a new operational family before recovery realism is stronger.
- It could drift into UI or production MCP work if not tightly scoped.

### Candidate C — Connector Invocation Telemetry

Goal:

Measure connector/MCP failure modes without storing prompts, secrets, private payloads, or arbitrary tool arguments.

Would address:

```text
blocked before MCP
adapter rejected
platform rejected
platform succeeded
transport failure
timeout without result
retry linkage and outcome
```

Why this is attractive:

- The project repeatedly encountered connector/tooling ambiguity.
- It would help distinguish platform failure from connector failure.
- It supports better future debugging and audits.

Why it may be too soon:

- It has higher privacy risk.
- It needs careful retention and redaction proof.
- It could expand toward production MCP/telemetry before Kaizen needs it.

### Candidate D — Observatory / Internet Marketing Intelligence Research Continuation

Goal:

Continue rights, retention, measurement, provider, and marketplace research without implementation.

Why this is attractive:

- It moves toward Kaizen's later business value.
- It can inform future marketplace products and services.

Why it should not be Milestone 12 implementation:

- Decisions 0019 and 0020 keep Observatory outside the first operational foundation.
- Provider rights, retention, and reuse boundaries are not yet accepted for implementation.
- No Observatory schema, Qdrant index, crawler, provider purchase, or raw capture is authorized.

## Steward recommendation

Choose Candidate A as Milestone 12:

```text
Milestone 12 — Recovery Realism and Operational Restore Proof
```

Rationale:

1. It directly closes the most important residual limitations recorded in Result 293.
2. It improves trust in the new operational database before adding more record families.
3. It is bounded and testable.
4. It keeps Kaizen aligned with its evidence-first doctrine.
5. It gives future governed-operation read model and connector telemetry work a stronger recovery base.

Candidate B should be the default next candidate after Candidate A unless new evidence changes priority.

Candidate C should wait until after either A or B because privacy and payload-boundary risk is higher.

Candidate D should continue only as a research track until rights and measurement doctrine are ready.

## Proposed Milestone 12 scope if owner accepts Candidate A

Milestone 12 should include:

```text
nonzero-file recovery generation proof
post-restore typed-service smoke check
restore-forward feasibility decision
operational role-ready restore decision
bounded trigger negative-probe tooling or test profile
backup/restore runbook correction
completion audit with exact hash evidence
```

Milestone 12 should exclude:

```text
new operational record families
governed-operation read model
connector telemetry
production deployment
retention deletion execution
physical evidence deletion
Observatory implementation
Qdrant
Hermes/UI work
provider purchases
customer data
multi-user identity
production MCP packaging
```

## Proposed immediate next packet

Prepare a planning-only packet:

```text
Packet 017A — Milestone 12 Recovery Realism Planning
```

Packet 017A should not implement. It should define:

1. exact Milestone 12 objective;
2. source evidence and residual limitations from Result 293;
3. implementation candidate slices;
4. changed-path allowlist if implementation is later approved;
5. live DB and file-system safety requirements;
6. no-secret handling requirements;
7. test and hammer requirements;
8. completion-audit criteria;
9. explicit exclusions.

## Owner decision needed

The owner should choose one of:

```text
A. Accept Candidate A as Milestone 12 direction and authorize Packet 017A planning only.
B. Reject Candidate A and choose Candidate B, C, or D as the next roadmap direction.
C. Request Claude or another independent reviewer to audit this proposal before owner decision.
D. Pause roadmap work.
```

## Non-authorization

This proposal does not authorize implementation, database mutation, vault mutation, production deployment, retention deletion, physical deletion, provider work, Qdrant work, Hermes/UI work, or new MCP/network surface.
