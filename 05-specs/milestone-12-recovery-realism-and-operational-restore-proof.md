# Milestone 12 — Recovery Realism and Operational Restore Proof

Status: active implementation specification
Date: 2026-06-20
Packet: 017B

## Purpose

Milestone 12 proves recovery realism after the Milestone 11 operational database foundation and Packet 016G attestation-integrity correction.

The milestone is not a new product-surface milestone. It exists to prove recovery behavior under realistic, nonzero-file, service-usable conditions before Kaizen adds new operational record families.

## Governing records

```text
03-research-results/293-packet-016g-completion-audit.md
03-research-results/297-packet-017a-milestone-12-recovery-realism-planning.md
03-research-results/298-packet-017a-owner-acceptance.md
03-research-results/299-packet-017b-recovery-realism-implementation-task-packet.md
03-research-results/300-claude-packet-017b-audit-disposition.md
03-research-results/302-historical-audit-disposition-register.md
```

## What the proof covers

Milestone 12 proof covers:

```text
nonzero-file recovery generation
nonzero-file restore proof
post-restore database-and-file consistency inspection
post-restore typed-service smoke check
restore-forward boundary decision
operational role-ready restore boundary decision
recovery/retention immutability rejection checks in disposable test DBs
ruff/tooling disposition
```

## What remains outside scope

Milestone 12 excludes:

```text
production deployment
retention deletion execution
physical evidence deletion
new operational record families
governed-operation read model implementation
connector telemetry implementation
direct agent SQL
arbitrary SQL APIs
Observatory
Qdrant
Hermes/UI
provider work
customer data
multi-user identity
production MCP packaging
vault mutation
broad schema redesign
```

## Live database boundary

For Packet 017B, live `kaizen_ops` is inventory evidence only.

Allowed live checks:

```text
schema_version read
recovery_generation read
restore_proof read
trigger inventory read
grant inventory read
```

Disallowed against live `kaizen_ops`:

```text
rewrite checks
retention execution
physical deletion
existing retained-generation rewrite
ad hoc SQL mutation
```

## Restore proof posture

Restore proof should use a disposable restore or test database. Any write used for the typed-service smoke check must remain scoped to the disposable database and go through typed service code.

Preferred smoke check:

```text
connect to the restored database with the service role
instantiate OpsService
perform a harmless bounded read
record result without exposing DSN or password
```

## Boundary decisions required

The implementation return must explicitly decide each of these:

```text
restore-forward: proven / unsupported / deferred
operational role-ready restore: proven / unsupported / deferred
ruff/tooling: adopted / not adopted for this milestone / deferred
```

Silence is not acceptable.

## Completion checklist

Packet 017B completion must record:

```text
platform start and end commits
docs start and end commits
changed paths
file hashes
nonzero recovery generation evidence
restore proof evidence
typed-service smoke result
restore-forward decision
operational role-ready decision
negative/rejection-check evidence from disposable DB
full test result or exact bounded non-run explanation
collect-only count
compile result
ruff/tooling disposition
residual limitations
```

## Safety note

This specification authorizes no implementation by itself. Implementation authority comes only from exact owner approval of Packet 017B.
