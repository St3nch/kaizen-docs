---
id: kz-aud-01KTHSA4FYB03ETAXH8QPBBQTZ
type: audit
status: complete
project: kaizen-platform
summary: Security and contract audit of Packet 006A for proving a Windows first-time atomic-install primitive.
created: 2026-06-07T19:35:16Z
updated: 2026-06-07T19:35:16Z
review_status: approved
related_specs:
  - 05-specs/staging-write-wrapper-and-promotion-recovery.md
  - 05-specs/staging-and-promotion-workflow.md
  - 05-specs/kaizen-hammer-test-strategy.md
---

# Research Result 027 - Packet 006A Security Audit

## Scope

Audit:

```text
06-handoff-patterns/006a-prove-windows-first-time-atomic-install.md
```

The audit determines whether Packet 006A is narrow, testable, and safe to present for owner approval. It does not approve implementation on the owner's behalf.

## Governing boundary

Packet 006A exists only to prove a Windows first-time atomic install primitive in disposable roots.

It must not implement:

- the promotion request or approval model;
- canonical-content normalization;
- promotion intent or completion events;
- `_governance` bootstrap;
- promotion recovery scanning;
- a `kaizen-promote` CLI;
- writes to the live vault or staging roots;
- agent or MCP exposure.

## Positive findings

### Scope is materially smaller than the retired combined Packet 006

The packet separates native namespace-install proof from the later promotion workflow. Its output is a platform-internal primitive plus evidence, not a partially implemented promotion system.

### Destination replacement is prohibited by contract

The packet requires the selected native operation, not a preflight check, to enforce destination absence. It explicitly rejects replacement flags, copy-and-delete fallback, and check-then-rename claims.

### Disposable-root-only execution is explicit

The packet prohibits writes to both live roots:

```text
C:\dev\kaizen\vault
C:\dev\kaizen\staging
```

All mutation is confined to pytest or explicitly created disposable roots.

### Primary-source verification is required

The packet requires the implementer to verify exact structures, information classes, access rights, sharing modes, volume identity, and error behavior using primary Microsoft documentation and live tests before choosing the primitive.

The leading candidate is recorded as a hypothesis rather than accepted doctrine.

### Test plan targets the actual unsafe boundaries

The required tests cover:

- destination-exists behavior;
- destination-appearance races;
- independent-process contention;
- no silent replacement;
- same-volume proof and cross-volume refusal;
- reparse/junction parents;
- file-lock and share-violation behavior;
- source disappearance or identity change;
- interruption immediately before and after the native call;
- orphan temporary ownership;
- repeated state classification;
- unrelated-file preservation.

### Required packet sections are present

The draft contains explicit:

```text
Implementation Requirements
Constraints
Validation
Acceptance Criteria
Stop Conditions
Rollback and Recovery
Completion Report
```

Test numbering is unique and sequential.

## Result 025 reconciliation

### Closed or satisfied for Packet 006A

- **F-10:** required packet-contract sections are present.
- **F-11:** test numbering and editorial structure are clean.
- **F-12, Gate A portion:** the retired combined Packet 006 has been split; 006A is a separate narrow packet.

### Explicitly assigned to Packet 006B or later

- **F-09:** `canonical_candidate_sha256` belongs to the later promotion request/plan contract, not the native primitive.
- **F-13:** immutable promotion-plan persistence belongs to 006B.
- **F-14:** `_governance` bootstrap remains a separate owner-approved vault maintenance step before first real promotion.
- **F-15:** `kz-prom-<ULID>` promotion-event IDs belong to 006B and the promotion-event schema.
- **F-18 and F-19:** promotion-event path normalization and schema versioning belong to 006B.

Those findings are not waived. They are excluded from 006A because implementing them here would recreate the oversized combined packet.

## Required implementation interpretation

The implementer must treat the conceptual API as a maximum authority boundary, not a requirement to expose raw handles publicly.

The preferred implementation shape is:

- a narrowly named internal function or module;
- trusted handles obtained through the existing verified Windows layer;
- no absolute path input;
- no replace option;
- no public CLI;
- typed stable outcomes;
- deterministic cleanup only for packet-owned temporary files.

## Risks that remain intentionally open

Packet approval would not prove the primitive works. The following remain implementation questions:

- exact `FILE_RENAME_INFO` or extended-structure layout on the supported runtime;
- whether the chosen information class and root-directory-handle semantics behave as required;
- exact access and delete-sharing requirements;
- collision error mapping;
- behavior under destination appearance and incompatible sharing;
- same-volume identity method;
- interruption-state classification after successful namespace transition.

These are why 006A exists.

## Stop-rule assessment

The packet correctly requires a blocked result when collision-refusing semantics, same-volume proof, race safety, or deterministic failure classification cannot be demonstrated.

It does not authorize weakening the contract to obtain a passing implementation.

## Steward verdict

**pass for owner approval**

Packet 006A is narrow enough to approve as a separate implementation packet. It does not authorize full canonical promotion and does not reopen the retired combined Packet 006.

Implementation must not begin until the owner explicitly approves Packet 006A.

## Next action

Present Packet 006A to the owner for approval. After approval, implement only the bounded native primitive and disposable-root hammer tests described by the packet.
