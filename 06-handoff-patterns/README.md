# Handoff Patterns

This folder defines patterns for implementation-ready task packets and coding-agent handoffs.

The goal is to make handoffs specific enough that a coding agent can execute them without guessing.

## Historical packet library

This folder is preserved as an implementation handoff packet library. It is not the current work queue.

Use active roadmap gates, current task approvals, exact operation IDs, and current repository state before executing any new work. Old packets are preserved for lineage, auditability, and reusable patterns; they do not authorize new mutation by themselves.

## Packet clusters

```text
001-009b  Milestones 1-5 first-slice foundations, vault bootstrap, staging, promotion, and governed amendment proof
010a-010f Milestone 6 operator surface and implementation-return preparation
011a-011e Milestone 7 durable recoverability and backup/restore posture
012a-012e1 Milestone 8 reliability and known-defect closure
013a-013e Milestone 9 Northstar readiness, bootstrap, and backup setup
014a-014f Milestone 9 Northstar controlled implementation-return pilot
015a      Milestone 10 workload and system-of-record reconciliation
016a-016f Milestone 11 operational data foundation
```

## Historical first-slice packet detail (preserved, not active work queue)

- `001-bootstrap-kaizen-platform-id-parser-foundations.md` - owner-approved; completed and audited pass-with-notes
- `002-implement-deterministic-note-validation.md` - completed and audited pass-with-notes
- `003-bootstrap-canonical-kaizen-vault.md` - completed and audited pass
- `004-implement-staging-root-and-path-confinement-foundations.md` - completed and audited pass-with-notes
- `005-implement-create-only-staging-write-wrapper.md` - completed and audited pass-with-notes
- `006-implement-human-operated-canonical-promotion.md` - retired combined draft; not eligible for approval; preserved as source material for the required 006A/006B split
- `006a-prove-windows-first-time-atomic-install.md` - complete at platform commit `26271ce`; steward-audited pass-with-documented-limitations in Result 028
- `006b-implement-human-operated-first-promotion.md` - complete at platform commit `703d532`; final steward audit Result 031 pass-with-documented-limitations; no live promotion authority
- `007-bootstrap-live-promotion-governance.md` - complete at vault commit `248b26a`; Result 033 pass; no promotion authority
- `008a-implement-owner-controlled-live-promotion-operator.md` - complete at platform commit `1a890dd`; Result 035 pass; no live plan or promotion occurred
- `008b-first-real-promotion-two-gate-execution.md` - complete at vault commit `80bc093`; Result 040 pass; first real promotion committed
- `009a-generate-milestone-4-governed-planning-bundle.md` - complete; Result 042 pass; six-note Milestone 4 bundle staged and validated
- `009b-finalize-review-and-run-ordered-bundle-promotion.md` - complete at vault `a306b2c`; all six waves canonical; Result 056 pass
