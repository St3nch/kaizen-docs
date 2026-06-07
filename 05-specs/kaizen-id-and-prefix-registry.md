# Spec - Kaizen ID and Type-Prefix Registry

Status: implementation baseline accepted by Decision 0012
Date: 2026-06-04
Updated: 2026-06-07
Related decisions:
- `04-design-decisions/0007-foundation-resolution-for-v0.2.md`
- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`

## Purpose

Define the machine identity format for durable Kaizen notes and the initial type-prefix registry used by validators, generators, promotion logs, Postgres references, and future Qdrant payloads.

## Canonical note ID format

```text
kz-<type-prefix>-<ulid>
```

Example:

```text
kz-clm-01JX8J3M8R7K9Q2ABCDEFG
```

Rules:

1. IDs are tool-generated.
2. IDs are globally unique across the canonical vault.
3. IDs are immutable after creation.
4. IDs are never reused, including after archival or deletion.
5. Filenames and titles may change without changing the ID.
6. The type prefix must match the note's registered `type`.
7. Relationship fields store complete Kaizen IDs, not bare ULIDs.

## Initial type-prefix registry

| Note type | Prefix | Example |
|---|---:|---|
| `command-center` | `cc` | `kz-cc-01JX...` |
| `overview` | `ov` | `kz-ov-01JX...` |
| `current-state` | `cs` | `kz-cs-01JX...` |
| `source-summary` | `ss` | `kz-ss-01JX...` |
| `claim` | `clm` | `kz-clm-01JX...` |
| `decision` | `dec` | `kz-dec-01JX...` |
| `spec` | `spec` | `kz-spec-01JX...` |
| `audit` | `aud` | `kz-aud-01JX...` |
| `task-packet` | `tp` | `kz-tp-01JX...` |

Operational event prefixes:

| Event type | Prefix | Example |
|---|---:|---|
| promotion event | `prom` | `kz-prom-01JX...` |
| validation run | `val` | `kz-val-01JX...` |
| escalation event | `esc` | `kz-esc-01JX...` |

Operational event IDs are not note IDs and do not imply a Markdown note exists.

## Generator command direction

Canonical command shape:

```text
kaizen-id <registered-type-or-event>
```

Examples:

```text
kaizen-id claim
kaizen-id spec
kaizen-id promotion-event
```

Expected output:

```text
kz-clm-01JX8J3M8R7K9Q2ABCDEFG
```

The command should:

- fail on unknown types
- use uppercase Crockford Base32 ULID characters
- emit one ID and no decorative prose in normal mode
- support a machine-readable JSON output mode later
- never inspect note titles to generate identity
- never contact an external service

## Machine-readable registry direction

Before implementation, the prefix registry should exist in a small validated data file, for example:

```text
schemas/note-type-prefixes.json
```

Candidate shape:

```json
{
  "command-center": "cc",
  "overview": "ov",
  "current-state": "cs",
  "source-summary": "ss",
  "claim": "clm",
  "decision": "dec",
  "spec": "spec",
  "audit": "aud",
  "task-packet": "tp"
}
```

The Markdown registry explains the rules. The machine-readable registry drives code and validation.

## Validation rules

The validator must reject:

- missing IDs
- malformed ULIDs
- unknown prefixes
- type/prefix mismatch
- duplicate IDs
- changed IDs on existing notes
- reused IDs from archived or deleted notes
- lowercase or ambiguous ULID characters when the generator standard requires uppercase

## Qdrant direction

Future Qdrant point IDs should be deterministic and derived from stable note identity plus chunk identity and embedding version. The exact point-ID formula remains deferred until Qdrant implementation planning.

The note ID remains present in every chunk payload regardless of point-ID strategy.

## Postgres direction

Postgres may use separate internal primary keys. Markdown notes reference stable external Kaizen IDs and governed Observatory result IDs rather than internal database keys.

## Registry change rule

Adding a new prefix requires:

1. accepted note/event type decision
2. uniqueness check against all existing prefixes
3. Markdown registry update
4. machine-readable registry update
5. validator update
6. generator update
7. tests and hammer coverage where boundary-sensitive

Prefixes must not be renamed after IDs using them have been created.

## Implementation location

The first implementation uses Python 3.11.15 in:

```text
C:\dev\kaizen\platform
```

The machine-readable type-prefix registry belongs under:

```text
schemas/note-type-prefixes.json
```

The CLI entry point is exposed as:

```text
kaizen-id <registered-type-or-event>
```

## Open questions

- Whether normal output includes a trailing newline only.
- Whether a batch-generation mode is ever needed.
- Whether archived/deleted ID reservation is tracked by Git scan, registry file, or future Postgres service.

## Related files

- `04-design-decisions/0012-first-slice-contract-and-implementation-boundary.md`
- `05-specs/kaizen-field-registry.md`
- `05-specs/kaizen-note-type-registry.md`
- `05-specs/kaizen-validation-gate-spec.md`
- `05-specs/staging-and-promotion-workflow.md`
