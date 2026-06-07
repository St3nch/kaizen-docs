---
id: kz-tp-01KTH99S9FFXVRYY3ADCSVEZ39
type: task-packet
status: active
project: kaizen-platform
summary: Implement deterministic single-note validation using the accepted Kaizen field, note-type, identity, and validation contracts.
created: 2026-06-07T00:00:00Z
updated: 2026-06-07T00:00:00Z
review_status: approved
authority: accepted
primary_spec: 05-specs/kaizen-validation-gate-spec.md
related_specs:
  - 05-specs/kaizen-validation-gate-spec.md
  - 05-specs/kaizen-field-registry.md
  - 05-specs/kaizen-note-type-registry.md
  - 05-specs/kaizen-id-and-prefix-registry.md
---

# Task Packet 002 - Implement Deterministic Note Validation

> Bootstrap status: owner-approved and accepted for execution. The canonical vault, sibling staging root, and promotion system do not exist yet, so this packet authorizes static single-note validation only.

## Objective

Extend `C:\dev\kaizen\platform` with a deterministic `kaizen-validate` CLI and reusable validation library for one Markdown note at a time.

The validator must build on the committed Milestone 1 parser and machine-readable registries. It must validate note structure and intrinsic metadata rules without implementing filesystem confinement, canonical writes, promotion events, cross-vault reference resolution, or authority transitions that require human-event evidence.

## Read First

Read these files in order before changing implementation files:

1. `C:\dev\kaizen-docs\04-design-decisions\0012-first-slice-contract-and-implementation-boundary.md`
2. `C:\dev\kaizen-docs\IMPLEMENTATION_ROADMAP.md`
3. `C:\dev\kaizen-docs\03-research-results\018-milestone-1-tools-foundation-steward-audit.md`
4. `C:\dev\kaizen-docs\05-specs\kaizen-validation-gate-spec.md`
5. `C:\dev\kaizen-docs\05-specs\kaizen-field-registry.md`
6. `C:\dev\kaizen-docs\05-specs\kaizen-note-type-registry.md`
7. `C:\dev\kaizen-docs\05-specs\kaizen-id-and-prefix-registry.md`
8. `C:\dev\kaizen-docs\04-design-decisions\0008-v0.2-operating-conventions.md`
9. `C:\dev\kaizen-docs\04-design-decisions\0006-hammer-tests-are-a-hard-gate.md`
10. `C:\dev\kaizen\platform\AGENTS.md`

The Markdown contracts remain the human-readable doctrine. Machine-readable validator contracts must be reviewed against them and must not silently redefine them.

## Context Pack

Milestone 1 is complete and audited `pass-with-notes`.

Available implementation foundations:

- Python 3.11.15 project at `C:\dev\kaizen\platform`;
- strict UTF-8 Markdown/frontmatter parser;
- duplicate-key and nested-YAML rejection;
- machine-readable note and event prefix registries;
- stable Kaizen exception codes;
- `kaizen-id` CLI;
- twenty-one passing tests;
- clean implementation repository.

This packet is the second tools-foundation packet. It does not authorize Milestone 2 vault creation or Milestone 3 staging/promotion work.

## Scope

Implement only:

1. machine-readable field, enum, note-type, and required-section contracts under `schemas/`;
2. reusable validation result, issue, and normalized-metadata models;
3. deterministic single-note validation for `staging` and `canonical` content modes;
4. a `kaizen-validate <path> --mode staging|canonical` CLI;
5. optional `--json` output for stable CI/agent consumption;
6. human-readable validation output;
7. stable error and warning codes;
8. fixtures and tests covering every rule authorized below;
9. README and AGENTS updates limited to validator usage and scope.

## Non-Scope

Do not implement:

- `promotion` mode;
- canonical or staging root configuration;
- final-path confinement;
- traversal, UNC, device-path, symlink, junction, mount-point, or reparse-point checks;
- canonical writes, moves, deletes, renames, or directory creation;
- promotion event creation or validation;
- approval-event lookup;
- content-hash approval freshness;
- immutable-ID or immutable-`created` comparison against Git history;
- global ID uniqueness scanning across a vault;
- cross-note stable-ID resolution;
- body-link target resolution;
- reciprocal supersedence validation;
- duplicate title, summary, filename, or concept scanning across a corpus;
- raw-data threshold calibration beyond the bounded warnings defined here;
- reviewed false-positive override events;
- Hermes, MCP, Postgres, Qdrant, DataForSEO, marketplace, Tauri, Obsidian plugin, or UI work;
- creation of `C:\dev\kaizen\vault`, `staging`, `data`, or `scratch`.

Do not modify `C:\dev\kaizen-docs` during implementation.

## Allowed Changes

Changes are limited to `C:\dev\kaizen\platform`.

Expected additions or modifications:

```text
pyproject.toml
README.md
AGENTS.md
schemas/
  fields.json
  enums.json
  note-types.json
src/kaizen/
  __init__.py
  cli.py or a dedicated validate_cli.py
  validation.py
  contracts.py or equivalent
  errors.py when new stable error types are needed
tests/
  fixtures/validation/
  test_validation.py
  test_validation_cli.py
  existing tests when required for compatible integration
```

Internal filenames may differ when documented in the completion report. Repository ownership, command names, validation modes, and scope may not change.

## Do Not Touch

Do not modify:

- `C:\dev\kaizen-docs`;
- `C:\dev\kaizen` as a Git root;
- sibling repositories;
- global Python or Git configuration;
- Obsidian configuration;
- paid or network services;
- future vault, staging, data, or scratch roots.

## Output Location

All implementation output belongs in:

```text
C:\dev\kaizen\platform
```

The completion report is returned to the steward. The coding agent does not write it into `kaizen-docs`.

## Implementation Requirements

### Validation command

Provide:

```text
kaizen-validate <path> --mode staging|canonical
```

Optional stable JSON output:

```text
kaizen-validate <path> --mode canonical --json
```

Exit behavior:

- `0` when no errors exist, including warning-only results;
- `1` when validation errors exist;
- `2` for command usage, unreadable input path, invalid mode, registry/configuration failure, or internal execution failure.

Human-readable normal output must identify:

- pass/fail;
- validated path;
- mode;
- note ID and type when parseable;
- each error and warning code with a message;
- error/warning totals.

JSON output must contain stable keys and must not include decorative prose.

### Validation result model

Provide tested library boundaries conceptually equivalent to:

```python
class ValidationSeverity(str, Enum):
    ERROR = "error"
    WARNING = "warning"

@dataclass(frozen=True)
class ValidationIssue:
    code: str
    severity: ValidationSeverity
    message: str
    field: str | None = None
    section: str | None = None
    line: int | None = None

@dataclass(frozen=True)
class ValidationResult:
    path: Path | None
    mode: str
    passed: bool
    issues: tuple[ValidationIssue, ...]
    normalized_metadata: Mapping[str, object]
```

Names may differ, but callers must not parse free-form console text to consume validation results.

### Machine-readable contracts

Create reviewed machine-readable contracts for:

- the seven universal fields;
- allowed known fields;
- enum values;
- note-type required fields;
- note-type allowed authority values;
- note-type required H2 sections;
- conditional relationship rules needed in this packet;
- the accepted six-value `pipeline_stage` enum.

The validator must load these contracts rather than duplicating them across validation branches.

### Universal fields

Every durable note requires non-empty:

```text
id
type
status
project
summary
created
updated
```

Rules:

- `project` is lowercase kebab-case;
- `summary` is non-empty plain scalar text;
- summary length over 200 characters is warning `summary_too_long`, not an error in this packet;
- unknown fields produce warning `unknown_field`;
- nested YAML remains a parser error from Milestone 1.

### ID and type validation

- `type` must be one of the nine registered note types;
- `id` must match `kz-<registered-prefix>-<26-character-ULID>`;
- the ID prefix must match the note type;
- ULID characters must be uppercase valid Crockford Base32;
- malformed ID and type-prefix mismatch use distinct stable error codes;
- global uniqueness and historical reuse checks remain deferred.

### Enum validation

Enforce:

```text
status: draft | active | blocked | complete | archived
review_status: not-required | pending | approved | rejected
authority: none | proposed | accepted
confidence: low | medium | high
validation_status: pending | passed | failed
pipeline_stage: capture | research | spec | audit | handoff | build
```

Unknown enum values are errors.

### Date validation

- `created` and `updated` must parse as ISO-8601 date-times with explicit timezone offsets;
- `updated` must be greater than or equal to `created`;
- timestamps more than five minutes in the future relative to validator time are errors;
- immutability across revisions remains deferred.

### Type-specific fields

Enforce the accepted note-type registry, including:

- `command-center`: `pipeline_stage`, `review_status: not-required`, authority absent or `none`;
- `overview`: `review_status: not-required`, authority absent or `none` unless a later governed rule says otherwise;
- `current-state`: `pipeline_stage`, `review_status: not-required`, authority absent or `none`;
- `source-summary`: `review_status`, `authority: none`, and at least one non-empty `source_docs` or `source_urls` list;
- `claim`: `review_status`, `authority`, `confidence`, and non-empty `source_docs`;
- `decision`: `review_status` and `authority: accepted|accepted`;
- `spec`: `review_status`, `authority: accepted|accepted`, and non-empty `related_decisions`;
- `audit`: `review_status`, authority absent or `none`, and at least one non-empty governed target list among `related_claims`, `related_decisions`, or `related_specs`;
- `task-packet`: `review_status`, `authority: accepted|accepted`, non-empty `related_specs`, and exactly one non-empty `primary_spec` that also appears in `related_specs`.

### Required sections

- enforce exact registered H2 headings;
- headings must be Markdown ATX H2 headings beginning with exactly `## `;
- heading order is not enforced in this packet;
- every required section must contain non-whitespace content before the next H2 or end of file;
- conditional `## Supersedence rationale` is required when `supersedes` or `superseded_by` is present;
- task-packet `## Completion Report

```text
Status: complete
Repository: C:\dev\kaizen\platform
Branch: main
Commit: 4e9a145 Implement deterministic Kaizen note validation
Files created:
- schemas/enums.json
- schemas/fields.json
- schemas/note-types.json
- src/kaizen/contracts.py
- src/kaizen/validate_cli.py
- src/kaizen/validation.py
- tests/fixtures/validation/valid-claim.md
- tests/fixtures/validation/valid-staged-agent-claim.md
- tests/fixtures/validation/invalid-missing-field.md
- tests/test_validation.py
- tests/test_validation_cli.py
Files modified:
- AGENTS.md
- README.md
- pyproject.toml
- src/kaizen/__init__.py
Machine-readable contracts added: field registry, enums, note-type required fields, authority constraints, required H2 sections, relationship fields
Validation issue codes added: stable codes for universal fields, IDs, prefixes, enums, timestamps, note-type fields, required sections, relationships, URLs, provenance, lifecycle/authority combinations, portable-link rules, secret detection, and bounded bulk-content warnings
Commands run:
- .\.venv\Scripts\python.exe -m pip install -e ".[dev]"
- .\.venv\Scripts\python.exe -m pytest
- .\.venv\Scripts\python.exe -m compileall -q src tests
- .\.venv\Scripts\python.exe -m pip check
- .\.venv\Scripts\kaizen-validate.exe tests\fixtures\validation\valid-claim.md --mode canonical
- .\.venv\Scripts\kaizen-validate.exe tests\fixtures\validation\valid-staged-agent-claim.md --mode staging --json
- .\.venv\Scripts\kaizen-validate.exe tests\fixtures\validation\invalid-missing-field.md --mode canonical
Tests run: 67
Test results: 67 passed before and after commit; compileall passed; pip check reported no broken requirements
CLI checks and exit codes: canonical valid note 0; staging valid agent note JSON 0; invalid note 1; missing/unreadable input 2 covered by subprocess tests
Acceptance criteria result: all 17 criteria satisfied
Deviations: no functional scope deviation; promotion mode, path confinement, cross-note resolution, and canonical writes remain absent as required
Unresolved issues:
- no dependency lockfile yet;
- validator does not prove human approval or promotion events;
- validator does not perform vault-wide uniqueness, reference resolution, or immutable-history checks;
- content-safety thresholds remain initial conservative values requiring later corpus calibration
Contract findings for kaizen-docs:
- the accepted registries are implementable as machine-readable contracts;
- deterministic warning-only exit 0 and error exit 1 are workable;
- parser/configuration failures use exit 2;
- the next vault milestone can use canonical mode for bootstrap notes but must not claim full promotion validation
Recommended next task: bootstrap the canonical Kaizen vault as a separate Git repository with one minimal project and validate its bootstrap notes
Final git status: clean after commit
```

The completion report is implementation evidence and does not independently change doctrine.