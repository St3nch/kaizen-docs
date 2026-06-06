# Research Result 012 - Step 5A Composed Tool Capability Map

Status: active steward synthesis
Date: 2026-06-05
Inputs:
- `03-research-results/009-obsidian-mcp-tool-surface-audit-claude-summary.md`
- `03-research-results/010-postgres-mcp-typed-tool-audit-claude-summary.md`
- `03-research-results/011-qdrant-mcp-typed-tool-audit-claude-summary.md`

## Purpose

Combine the Obsidian, Operational Postgres, and Qdrant comparative audits into one capability and authority model before any product or gateway decision is drafted.

This synthesis is planning evidence, not accepted implementation architecture.

## Common capability lanes

| Lane | Obsidian / Markdown | Operational Postgres | Qdrant |
|---|---|---|---|
| Governed read | portable Markdown plus Obsidian-aware navigation/search | typed project-scoped result and status reads | typed project-scoped semantic retrieval |
| Governed mutation | Kaizen staging-create and governed diff only | typed operational services and legal state transitions | Kaizen-owned headless indexing and reconciliation |
| Human/admin | Obsidian commands, plugins, migration/repair, promotion | migrations, DDL, roles, backup, restore | collections, snapshots, indexes, keys, cluster operations |
| Developer/diagnostic | disposable plugin/MCP tests and vault diagnostics | schema/query diagnostics on dev/staging | collection inspection, recall tests, rebuild comparison |

## Shared rules

In this synthesis, `agent` means an interactive reasoning agent such as Hermes, GPT, or Claude. Trusted headless operational services use separate identities, credentials, contracts, and audit boundaries.

1. Agents do not receive generic filesystem, SQL, vector mutation, or administrative pass-through tools.
2. Read-only operations still require scope, privacy, confinement, result limits, and audit evidence.
3. Unsafe tools should be absent from discovery, not merely denied by prompt or runtime policy.
4. Coarse upstream credentials require a thin allowlisting gateway or proxy.
5. Mature ecosystem capabilities should be reused where they do not carry Kaizen authority.
6. Authority-bearing writes, state transitions, provenance, validation, and promotion remain Kaizen-governed.
7. Headless canonical operations must not require Obsidian or another GUI to be running.
8. Interactive Obsidian-aware capabilities remain valuable for human review and navigation.
9. Postgres and Qdrant candidate enforcement mechanisms require disposable prototype and hammer evidence.
10. No final server, gateway, schema, collection, chunking, embedding, or hosting selection is approved by these audits.

## Leading candidate directions

### Obsidian

```text
portable canonical Markdown access
+
constrained Obsidian-aware read/navigation
+
Kaizen staging-write wrapper
+
separate human/admin plugin profile
```

### Operational Postgres

```text
Kaizen typed read tools
-> scoped gateway candidate such as PostgREST
-> approved views/functions
-> effective-role, RLS, privacy, timeout, and audit controls

Kaizen typed operational services
-> legal state transitions, idempotency, provenance, and audit
```

### Qdrant

```text
Kaizen typed retrieval tool
-> retrieval gateway
-> scoped short-lived Qdrant token and mandatory filters

canonical Markdown
-> Kaizen-owned headless indexer
-> provenance, drift detection, reconciliation, and rebuild
```

## Configure-versus-wrap rule

- Configure when unsafe tools are absent, roles/credentials independently enforce scope, and the configuration is captured and testable.
- Wrap when one token or server exposes more capability than the agent should receive, when trusted scope/filter injection is required, or when audit and redaction must be added.
- Fork only when the reusable implementation is valuable and the reduced surface is easier to audit than a small purpose-built tool.
- Reject when broad pass-through behavior is the core product model or cannot be removed reliably.

## Prototype gates before architecture decisions

### Obsidian

- tool-discovery absence;
- read confinement and private-root denial;
- plugin/REST credential scope;
- headless versus interactive behavior;
- value comparison against portable Markdown access.

### Postgres

- effective role, ownership, views, functions, RLS, and pooler chain;
- cross-project and private-column isolation;
- no SQL or generic endpoint pass-through;
- result limits, timeouts, identity, and audit;
- legal mutation transitions and idempotency.

### Qdrant

- mandatory filters across search, recommendation, scroll, exact-point, and metadata paths;
- project/privacy/authority/supersedence/freshness isolation;
- tool-discovery absence for store/delete/admin;
- stable chunk identity, provenance, drift detection, and logical rebuild reproducibility;
- credential expiry, revocation, and rotation.

## Decision boundary

A later architecture decision may define the composed tool model after prototype evidence exists.

It should define:

- capability lanes and profiles;
- typed operation classes;
- credential and identity separation;
- configure-versus-wrap criteria;
- canonical versus derivative truth;
- headless and interactive contexts;
- audit and hammer requirements;
- prototype evidence required for implementation planning.

It should not hard-code products until evidence justifies selection.

## Immediate consequence

Step 5A research is complete at the comparative-audit level.

The project may proceed to Research Batch B while retaining Postgres and Qdrant prototypes as Phase 7 implementation-planning prerequisites rather than running them during the current planning phase.