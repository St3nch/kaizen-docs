> **Historical baseline notice — superseded:** This v0.1-era standard is preserved as source material and audit history. The authoritative standard is `01-project-standard/kaizen-project-standard-v0.2.md`, accepted on 2026-06-09. Accepted decisions, specifications, and verified implementation evidence override conflicting details below.

# Kaizen Project Standard

**Vault:** Kaizen  
**Purpose:** Reusable system for every project added to the Kaizen vault. One pipeline, one structure, one standard — applied to every project automatically. New project joins the vault and the system is already waiting for it.

---

## The Pipeline

Every project in Kaizen moves through the same stages. The vault is the thread that connects every stage. Nothing lives in someone's head or a disconnected chat window.

```
Idea capture
    ↓
Research (web, docs, competitors, domain knowledge)
    ↓
Research filed in vault (raw sources → source summaries → claims)
    ↓
Spec planning docs (what to build, why, scope, decisions)
    ↓
Audit pass (gaps, contradictions, missing pieces caught before build)
    ↓
Implementation docs / task packets (agent-ready handoffs)
    ↓
Coding agent reads from vault, builds in code repo
```

Each stage has a defined output. Each output lives in a defined place. Agents know exactly where to look.

---

## Agent Roles In The Pipeline

Kaizen is designed to work with two agent tiers.

| Stage | Agent | Why |
|---|---|---|
| Research capture and filing | Hermes (cheap, fast, high volume) | Fetch, summarize, file — no deep reasoning needed |
| Source summaries | Hermes | Reads raw notes, writes Kaizen-format summaries |
| Vault search before creating | Hermes | Prevents duplicate notes accumulating |
| Log and index maintenance | Hermes | Housekeeping after every session |
| Task packet drafting | Hermes | Reads spec, drafts packet in correct format |
| Spec audits | Claude / GPT | Real reasoning needed — gaps, contradictions, missing pieces |
| Architecture decisions | Claude / GPT | High stakes, needs frontier reasoning |
| Implementation doc review | Claude / GPT | Final check before agent builds from it |

**The boundary:** Hermes handles volume work and vault hygiene. Claude handles reasoning-heavy stages. Never blur this. The quality of the implementation docs depends on keeping the audit and review stages at the right model tier.

---

## Required Plugins

Install these before building anything else.

| Plugin | Role | Install priority |
|---|---|---|
| **Dataview** | Live queries across vault by frontmatter | Install first |
| **Templater** | Smart templates with JavaScript, auto-routing | Install first |
| **Commander** | Buttons inside notes that trigger Templater scripts | Install first |
| **Kanban** | Board view for task packets in flight | Earn it |
| **Tasks** | Tracks checkboxes with due dates across vault | Earn it |
| **Canvas** | Visual pipeline map per project | Earn it after first full pipeline run |
| **Obsidian MCP server** | Exposes vault to agents as structured tools | Add when Hermes is active |

Do not install AI assistant plugins (Smart Connections, Copilot for Obsidian, etc.). Kaizen already has two agent tiers with defined roles. A third AI layer inside the vault creates source-of-truth confusion and blurs which agent is authoritative for what.

---

## Standard Project Folder Structure

Every project added to `projects/` gets this layout. Nothing more until earned.

```
projects/
  {project-name}/
    command-center.md       ← LLM entry point + live Dataview queries + action buttons
    overview.md             ← What the project is. Kaizen-native narrative.
    current-state.md        ← Current pipeline stage, boss fight, blocked, do not touch.
    roadmap.md              ← Narrative roadmap. Links to source repo for operational detail.
    source-import-map.md    ← Provenance: which source docs mapped to which Kaizen notes.
    research/               ← Created when first raw source or source summary is filed.
    decisions/              ← Created when first real decision is recorded.
    specs/                  ← Created when first spec planning doc is written.
    handoffs/               ← Created when first task packet is ready for an agent.
```

### Folder rules

- No empty folders created speculatively
- `research/` earns its existence when the first source is filed
- `decisions/` earns its existence when the first decision is recorded
- `specs/` earns its existence when the first spec doc is written
- `handoffs/` earns its existence when the first task packet is ready
- Every note has standard frontmatter (see below)

---

## Standard Frontmatter

All durable Kaizen notes use these fields. Required fields always present. Optional fields added when provenance matters.

```yaml
---
type: overview | current-state | roadmap | decision | spec | task-packet | source-summary | claim | raw-source | source-import-map
status: active | draft | complete | archived | blocked | pending-review
project: {project-name}
pipeline_stage: capture | research | spec | audit | handoff | build
phase: {phase name or number if applicable}
summary: "One sentence. What this note contains."
created: YYYY-MM-DD
updated: YYYY-MM-DD
source_repo: {repo-name}             # optional — only when distilled from external repo
source_docs:                          # optional — only when provenance is not obvious
  - path/to/source/doc.md
---
```

The `pipeline_stage` field is what makes Dataview queries useful for tracking where a project is in the pipeline. Keep it accurate.

---

## The Command Center Note

One per project. The most important note in any project folder. Serves two consumers: you (orientation speed) and agents (session entry point).

File: `projects/{project-name}/command-center.md`

### Section 1 — LLM Entry Point (plain text, never a query)

What every agent reads first. Plain text only — Dataview queries do not render for agents reading raw markdown. Must be human-readable and durable.

```markdown
## LLM Entry Point

project: {project name}
type: {service business / platform / tool / research / etc.}
pipeline_stage: {capture | research | spec | audit | handoff | build}
boss_fight: {the one thing that must be solved right now}
blocked_by: {what is blocking progress, or "nothing currently blocked"}
do_not_touch: {files, folders, or systems to leave alone}
specs_live_in: projects/{project-name}/specs/
handoffs_live_in: projects/{project-name}/handoffs/
source_repo: C:\dev\{repo-name}
canonical_ops_docs: {path to operational docs in source repo if applicable}

For coding agents: before touching any code, read {repo}/AGENTS.md first.
For Hermes: before creating any note, search the vault for existing notes on this topic.
```

Keep this under 12 lines. If it grows longer, something is wrong.

### Section 2 — Live Status (Dataview queries)

Auto-updating. No manual maintenance. Rendered inside Obsidian only — agents see raw syntax, not rendered output.

```markdown
## Pipeline stage

\`\`\`dataview
TABLE pipeline_stage, status, summary
FROM "projects/{project-name}"
WHERE type = "current-state"
\`\`\`

## Open specs

\`\`\`dataview
TABLE summary, status, phase
FROM "projects/{project-name}/specs"
WHERE status != "complete"
SORT updated DESC
\`\`\`

## Pending handoffs

\`\`\`dataview
TABLE summary, status, phase
FROM "projects/{project-name}/handoffs"
WHERE type = "task-packet" AND status != "complete"
SORT created DESC
\`\`\`

## Open decisions

\`\`\`dataview
TABLE summary, status
FROM "projects/{project-name}/decisions"
WHERE status = "draft" OR status = "active"
SORT created DESC
\`\`\`

## Recent updates

\`\`\`dataview
TABLE summary, type, pipeline_stage
FROM "projects/{project-name}"
SORT updated DESC
LIMIT 5
\`\`\`
```

### Section 3 — Quick Actions (Templater buttons)

Each button creates the right note in the right place with correct frontmatter. Wired via Commander plugin.

```markdown
## Quick actions

[New Raw Source]      → templates/raw-source.md
[New Source Summary]  → templates/source-summary.md
[New Spec Doc]        → templates/spec.md
[New Decision]        → templates/decision.md
[New Task Packet]     → templates/task-packet.md
[Log Entry]           → appends to log.md
```

### Section 4 — Canonical Source Links

Direct paths to source repo files. Not Kaizen summaries — actual file paths. Agents read these directly.

```markdown
## Canonical sources (for coding agents)

Code and implementation: C:\dev\{repo-name}\
Schema contracts:        C:\dev\{repo-name}\docs\core\schemas\
System invariants:       C:\dev\{repo-name}\docs\core\12-system-invariants.md
Operational runbooks:    C:\dev\{repo-name}\docs\operations\
Active roadmap:          C:\dev\{repo-name}\docs\ROADMAP.md
```

---

## Vault-Level Dashboard

One note at vault root surfaces status across all projects simultaneously.

File: `KAIZEN_COMMAND_CENTER.md`

```markdown
## All active projects and pipeline stages

\`\`\`dataview
TABLE pipeline_stage, phase, summary AS "current state"
FROM "projects"
WHERE type = "current-state" AND status = "active"
SORT project ASC
\`\`\`

## All pending handoffs (all projects)

\`\`\`dataview
TABLE project, summary, phase
FROM "projects"
WHERE type = "task-packet" AND status = "pending"
SORT project ASC, created DESC
\`\`\`

## All open specs (all projects)

\`\`\`dataview
TABLE project, summary, pipeline_stage
FROM "projects"
WHERE type = "spec" AND status != "complete"
SORT project ASC, updated DESC
\`\`\`

## All open decisions (all projects)

\`\`\`dataview
TABLE project, summary
FROM "projects"
WHERE type = "decision" AND (status = "draft" OR status = "active")
SORT project ASC
\`\`\`

## Recently updated (all projects)

\`\`\`dataview
TABLE project, type, pipeline_stage, summary
FROM "projects"
SORT updated DESC
LIMIT 10
\`\`\`
```

---

## Templater Templates

Store in `templates/`. All project-agnostic — prompt for project name at creation time.

### Raw source (`templates/raw-source.md`)

```markdown
---
type: raw-source
status: active
project: <% tp.system.prompt("Project name") %>
pipeline_stage: capture
summary: "<% tp.system.prompt("One sentence: what is this source?") %>"
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
source_url: <% tp.system.prompt("Source URL or path") %>
---

# Raw Source: <% tp.system.prompt("Source title") %>

## Source

Paste raw content, transcript, or notes here. No editing required at this stage.

## Why captured

One line. What question or project need led to capturing this.
```

### Source summary (`templates/source-summary.md`)

```markdown
---
type: source-summary
status: active
project: <% tp.system.prompt("Project name") %>
pipeline_stage: research
summary: "<% tp.system.prompt("One sentence: what does this source tell us?") %>"
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
source_docs:
  - <% tp.system.prompt("Raw source note path") %>
---

# Source Summary: <% tp.system.prompt("Source name") %>

## What this source is

One paragraph. What it contains and why it exists.

## Key claims and findings

The intelligence worth carrying into Kaizen. Distilled meaning only — not a copy of the source.

## What stays in the source

What parts are too raw, too operational, or too large to live here. Link or path.

## Relevance to current work

Why this matters right now for this project.
```

### Spec doc (`templates/spec.md`)

```markdown
---
type: spec
status: draft
project: <% tp.system.prompt("Project name") %>
pipeline_stage: spec
phase: <% tp.system.prompt("Current phase") %>
summary: "<% tp.system.prompt("One sentence: what does this spec define?") %>"
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
---

# Spec: <% tp.system.prompt("Spec title") %>

## What this defines

What system, feature, service, or component this spec covers.

## Context

What research and decisions led to this spec. Link to relevant source summaries and decisions.

## Scope

What is in scope. What is explicitly out of scope.

## Design

The actual spec. Be specific enough that an agent can build from this without asking questions.

## Constraints

What the implementation must not do. Reference system invariants if applicable.

## Open questions

What needs to be resolved before this spec is implementation-ready.

## Audit status

- [ ] Reviewed for gaps
- [ ] Reviewed for contradictions with other specs
- [ ] Reviewed for missing constraints
- [ ] Implementation-ready
```

### Decision (`templates/decision.md`)

```markdown
---
type: decision
status: active
project: <% tp.system.prompt("Project name") %>
pipeline_stage: <% tp.system.prompt("Pipeline stage this decision affects") %>
summary: "<% tp.system.prompt("One sentence: what was decided?") %>"
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
---

# Decision: <% tp.system.prompt("Decision title") %>

## Context

What situation required a decision.

## Decision

State the decision plainly. Quote the key rule verbatim — do not paraphrase it.

## Why

The reasoning. What alternatives were considered and why this was chosen.

## Consequences

What this decision prevents. What it enables. What it defers.

## Source

Where this came from — research note, planning session, ADR number, or source doc path.
```

### Task packet (`templates/task-packet.md`)

```markdown
---
type: task-packet
status: draft
project: <% tp.system.prompt("Project name") %>
pipeline_stage: handoff
phase: <% tp.system.prompt("Current phase") %>
summary: "<% tp.system.prompt("One sentence: what does this packet ask the agent to build?") %>"
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.date.now("YYYY-MM-DD") %>
---

# Task Packet: <% tp.system.prompt("Task name") %>

## Context

What the coding agent needs to know before starting. Link to relevant Kaizen spec docs and source repo files.

## Objective

What must exist when this task is complete. Specific enough that the agent can verify it without asking.

## Source files to read first

- `C:\dev\{repo}\` — relevant paths here

## Spec references

- Link to the spec doc this task is built from

## Constraints

What the agent must not do. Reference system invariants or decisions if applicable.

## Output

Where the code or file should be created. Which repo. Which folder.

## Acceptance criteria

How we know it is done. Specific, verifiable conditions.

## Audit checklist

- [ ] Spec reviewed before this packet was written
- [ ] Constraints verified against source repo invariants
- [ ] Output location confirmed
- [ ] Acceptance criteria are verifiable without human judgment
```

---

## Hermes Configuration (When Active)

When Hermes is connected to the vault, it needs a skill file that defines exactly how it works in Kaizen.

The Hermes skill for Kaizen must cover:

**What the vault is:** Kaizen is a project intelligence pipeline vault. Ideas enter at capture stage. Implementation-ready task packets exit at handoff stage. Everything in between is research, synthesis, and spec work.

**What Hermes is authorized to do:**
- Search vault before creating any note (prevent duplicates)
- Create raw source notes from URLs or pasted content
- Write source summaries from raw sources
- Append to `log.md`
- Update `index.md` when new notes are added
- Draft task packets from approved spec docs
- Run vault lint (check for missing frontmatter, broken links)

**What Hermes must not do without human approval:**
- Create spec docs (specs require human judgment on scope and design)
- Create or modify decision notes
- Commit or push to any Git repo
- Move or delete any existing note
- Mark any task packet as `status: pending` (that approval belongs to the human)

**Model routing inside Hermes:**
- Cheap fast model (Gemini 2.5 Flash Lite): filing, searching, log updates, index updates
- Mid-tier model: source summaries, raw source drafting
- Route to Claude/GPT externally: spec audits, architecture decisions, implementation doc review

---

## Build Order

Do not build everything at once.

**Step 1 — Right now (one session)**
Fresh vault. Install Dataview and Templater. Create `KAIZEN_COMMAND_CENTER.md` bare. Create one project folder with `command-center.md` containing only the LLM entry point section. Confirm Claude can read it via filesystem MCP. Done.

**Step 2 — After first project has 10+ notes**
Add Dataview query sections to the command center. Add vault-level dashboard queries to `KAIZEN_COMMAND_CENTER.md`. Now there is enough frontmatter for queries to be useful.

**Step 3 — When note creation becomes repetitive**
Build Templater templates. Add Commander buttons to the command center. Earn this by feeling the friction first — at least three manual note creations where you wished frontmatter was automatic.

**Step 4 — After first full pipeline run**
One complete idea-to-task-packet run completed by hand. Evaluate what was painful. Add Canvas phase map if visual navigation would help. Add Obsidian MCP server if Hermes is now active and would benefit from structured vault access rather than raw filesystem.

**Step 5 — When Hermes joins**
Write the Hermes skill file before connecting Hermes to anything. Test one complete pipeline run with Hermes handling only the authorized stages. Expand its role only after the boundaries are proven to hold.

---

## Adding a New Project

Every new project follows this sequence:

1. Create `projects/{project-name}/` folder
2. Create `command-center.md` — LLM entry point filled in, sections 2-4 stubbed
3. Create `overview.md` — what the project is, why it exists
4. Create `current-state.md` — current pipeline stage, boss fight, blockers
5. Create `source-import-map.md` if migrating from an existing repo
6. Update `index.md` to link the new project
7. Append `log.md` with a creation entry
8. Commit after review

Do not create `research/`, `decisions/`, `specs/`, or `handoffs/` folders until there is a real note to put in them.

---

## What Never Goes in Kaizen

Applies to every project, permanently.

- Running code, tests, fixtures, build configs — stays in code repos
- Code-governing contracts (schemas, invariants, transaction boundaries) — stays with code
- Active business operations (SOPs, pricing, fulfillment, QC checklists) — stays in ops repos
- Raw or private data (customer data, exports, competitor screenshots) — stays local
- Generated outputs (compiled assets, PDFs, build artifacts) — stays in source repo
- Anything that changes with every customer order, test run, or code commit

**The test:** if it changes faster than planning decisions change, it does not belong in Kaizen.

---

## Source-of-Truth Rules

| Layer | Canonical home |
|---|---|
| Project intelligence, decisions, roadmap narrative | Kaizen |
| Spec planning docs and implementation handoffs | Kaizen |
| Research synthesis, claims, domain knowledge | Kaizen wiki/ |
| Running code and tests | Source code repo |
| Code-governing doctrine and schemas | Source code repo |
| Active business operations and SOPs | Operational repo |
| Raw and private data | Local only |

No content has two canonical homes. If it lives in Kaizen, the source repo version either becomes reference-only or gets a one-line stub pointing to Kaizen.

---

*Kaizen Project Standard — created June 2026.*  
*Applies to all current and future projects in the vault.*  
*Continuous improvement: this document updates as the pipeline matures.*
