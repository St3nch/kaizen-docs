# Handoff Patterns

This folder defines patterns for implementation-ready task packets and coding-agent handoffs.

The goal is to make handoffs specific enough that a coding agent can execute them without guessing.

## Current packets

- `001-bootstrap-kaizen-platform-id-parser-foundations.md` - owner-approved; completed and audited pass-with-notes
- `002-implement-deterministic-note-validation.md` - completed and audited pass-with-notes
- `003-bootstrap-canonical-kaizen-vault.md` - completed and audited pass
- `004-implement-staging-root-and-path-confinement-foundations.md` - completed and audited pass-with-notes
- `005-implement-create-only-staging-write-wrapper.md` - completed and audited pass-with-notes
- `006-implement-human-operated-canonical-promotion.md` - retired combined draft; not eligible for approval; preserved as source material for the required 006A/006B split
- `006a-prove-windows-first-time-atomic-install.md` - complete at platform commit `26271ce`; steward-audited pass-with-documented-limitations in Result 028
- `006b-implement-human-operated-first-promotion.md` - complete at platform commit `703d532`; final steward audit Result 031 pass-with-documented-limitations; no live promotion authority
- `007-bootstrap-live-promotion-governance.md` - complete at vault commit `248b26a`; Result 033 pass; no promotion authority
- `008a-implement-owner-controlled-live-promotion-operator.md` - security-audited pass in Result 034; awaiting explicit owner approval; no live command authority
