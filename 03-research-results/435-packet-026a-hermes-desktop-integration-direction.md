# Packet 026A — Hermes Desktop Integration Direction

Status: complete — direction recorded
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Record the agreed Milestone 17 direction for Hermes integration after 025F closed the post-M16 retrieval expansion lane.

This record documents the direction only. It does not authorize Hermes execution, Hermes writes, platform mutation, vault mutation, staging mutation, Tauri implementation, API integration, production MCP work, external messaging, or autonomous agent operation.

## Decision

Kaizen will use Hermes Desktop / Hermes Agent first as a controlled proof harness, not as the permanent Kaizen user interface and not as the authority boundary.

If Hermes proves useful and controllable, Kaizen may later integrate Hermes more deeply as a worker behind Kaizen-owned typed tools and services.

The long-term Kaizen direction remains:

```text
Obsidian = canonical document and evidence workspace
Kaizen app / control shell = human operational and review interface
Typed Kaizen services = authority and data-access boundary
Hermes / agents = constrained clerks through approved typed tools
Humans = approval, promotion, and authority
```

In plain terms:

```text
Use Hermes Desktop to test the clerk.
Do not make Hermes Desktop the Kaizen app.
Build Kaizen as the boss.
Let Kaizen use Hermes only after Hermes earns the role.
```

## Why this direction

Hermes Desktop is useful for early M17 because it can expose real deployed behavior without requiring Kaizen to build a full Tauri chat surface first.

The first question is not whether Kaizen can make a nice chatbox. The first question is whether Hermes can operate safely as a constrained Kaizen clerk.

A custom Kaizen chatbox may be the better long-term human experience, but building it before verifying Hermes would risk building around assumptions that may not survive contact with the actual tool surface.

## What Hermes Desktop should prove first

The first Hermes lane should answer:

```text
Can Hermes run in a dedicated Kaizen profile?
Can its exposed tool inventory be captured exactly?
Can read/search remain available while unsafe tools are absent?
Are unsafe tools absent from discovery, not merely denied after being shown?
Can skill-management, persistent memory, schedules, terminal, browser actions, patch, write, delete, move, rename, and external messaging be disabled or isolated?
Can Hermes read the approved Kaizen entrypoint and summarize current project state without writing?
Can the target repository remain clean after the test?
Can Hermes report uncertainty and tool results honestly?
```

The first proof is read-only orientation. It is not a write test.

## What deeper integration may look like later

If the read-only proof succeeds and later gates approve additional capabilities, Hermes may become a worker behind Kaizen-owned operations.

Candidate future pattern:

```text
Kaizen app / operator command
-> Kaizen typed service or gateway
-> approved context pack / tool contract
-> Hermes clerk run
-> draft, report, or proposal returned
-> deterministic validation and human review
-> human-controlled promotion when applicable
```

Hermes may eventually help with:

```text
source-summary drafting;
claim extraction drafts;
research filing proposals;
duplicate-search reports;
context-pack preparation;
validation-report summaries;
task-packet draft scaffolding from approved specs;
review-queue preparation;
agent-run summaries.
```

Hermes must not own approval, promotion, audit verdicts, accepted doctrine, implementation-ready status, source-code changes, direct database mutation, Qdrant mutation, publishing, spending, customer-facing action, or Git commit/push.

## Relationship to the future Kaizen app

A future Kaizen app or Tauri-style control shell remains the preferred long-term human operational interface.

The future app may contain a governed chat or command panel, but that panel must be a Kaizen-controlled interface over typed operations. It must not be a generic chatbot with broad repo, database, Qdrant, or filesystem power.

The future chat/control panel should route work to Hermes, GPT, Claude, or other agents only through approved typed contracts and recorded context packs.

Hermes Desktop may remain useful as:

```text
a test harness;
a clerk runtime;
a debugging / profile-verification surface;
a manual agent console for bounded experiments.
```

Hermes Desktop should not become:

```text
the primary Kaizen product UI;
the canonical document workspace;
the approval surface of record;
the authority boundary;
the permanent system of record;
a bypass around typed services.
```

## What Kaizen needs before live Hermes integration

Before any live Hermes read-only test, Kaizen needs:

```text
a clean target repository or disposable target;
a dedicated Hermes Kaizen profile;
captured Hermes version and tool inventory;
confirmation of enabled and disabled tools;
confirmation that unsafe tools are absent from discovery where possible;
a reviewed Kaizen skill or instruction bundle marked as guidance only;
persistent memory and schedule behavior disabled or explicitly documented;
external messaging channels disabled;
clear local-only versus remote/backend behavior notes;
a pre-test Git or filesystem snapshot;
a post-test clean-state check;
a place to record test evidence without creating a junk drawer.
```

Before any Hermes staging-write test, Kaizen additionally needs:

```text
read-only test accepted;
Kaizen staging-create wrapper matched to the current root model;
invalid-path and reparse-point hammer tests current and passing;
canonical roots protected from the Hermes identity;
disposable staging target only;
no overwrite capability;
append-only attempt evidence;
deterministic validation;
human go/no-go after evidence review.
```

## Current non-authorizations

026A does not authorize:

```text
Hermes execution;
Hermes write access;
Hermes access to canonical mutation tools;
Hermes access to staging mutation tools;
Hermes access to source repositories;
Hermes commit or push;
Hermes direct Postgres access;
Hermes direct Qdrant access;
Hermes external messaging integration;
Hermes scheduling or always-on operation;
Hermes skill or memory self-management for governance workflows;
Tauri implementation;
Kaizen app implementation;
custom chatbox implementation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
Qdrant runtime start;
production MCP migration;
commercial or production use.
```

## Next recommended gate

Proceed to a Milestone 17 read-only test contract.

Recommended next packet:

```text
026B — Hermes Read-Only Tool Inventory and Orientation Test Contract
```

026B should define the exact test target, required Hermes profile evidence, allowed and forbidden tool surface, test prompt, clean-state checks, success criteria, and stop conditions.

026B should not authorize writes.
