# Packet 027I — Kaizen Hermes Read-Only MCP Tool Contract

Status: planned — read-only contract specified
Date: 2026-06-30
Milestone: 17 — Hermes constrained-clerk integration

## Purpose

Specify the first Kaizen-owned MCP tool contract for Hermes Desktop.

027I defines a local, read-only, fixed-root, manifest-governed MCP surface that allows Hermes to perform bounded Kaizen clerk orientation and context work without raw Hermes File Operations, ngrok, HTTP/SSE exposure, canonical writes, staging writes, database mutation, Qdrant mutation, terminal access, browser access, or hidden authority transitions.

This contract is the first safe proof layer for the larger Kaizen MCP direction. It is not the full long-term MCP ambition.

## Inputs

```text
03-research-results/435-packet-026a-hermes-desktop-integration-direction.md
03-research-results/436-packet-026b-hermes-read-only-tool-inventory-and-orientation-test-contract.md
03-research-results/437-packet-026c-hermes-target-capability-profile.md
03-research-results/438-packet-026d-hermes-026b-a-no-file-access-orientation-test-result.md
03-research-results/439-packet-026e-026f-hermes-pasted-context-clerk-workflow-results.md
03-research-results/440-packet-026g-hermes-pasted-context-claim-extraction-draft-sample-result.md
03-research-results/441-packet-027a-hermes-draft-output-intake-contract-planning.md
03-research-results/442-packet-027b-hermes-draft-output-intake-contract-v0-1.md
03-research-results/443-packet-027c-hermes-draft-output-intake-contract-manual-review.md
03-research-results/444-packet-027d-hermes-draft-output-intake-contract-manual-trial-result.md
03-research-results/445-packet-027e-hermes-draft-output-intake-contract-manual-trial-refinements.md
03-research-results/446-packet-027f-hermes-draft-output-intake-contract-refined-manual-trial-result.md
03-research-results/447-packet-027g-hermes-larger-pasted-context-intake-contract-trial-result.md
owner-supplied deep research report: Hermes Extension Architecture and Kaizen-Owned Tool Surface
owner-supplied deep research report: Evidence for 027I Kaizen Hermes Read-Only MCP Tool Contract
```

## Research findings accepted as design inputs

The owner-supplied research for 027I is accepted as evidence for this contract lane.

Decision-grade findings carried forward:

```text
Hermes Agent can act as a local MCP host.
Hermes can launch local stdio MCP servers.
Hermes MCP configuration uses mcp_servers under config.yaml.
On native Windows, the practical config path is %LOCALAPPDATA%\hermes\config.yaml unless HERMES_HOME is overridden.
Hermes supports command, args, env, timeout, connect_timeout, tools.include, tools.exclude, tools.resources, tools.prompts, and sampling.enabled.
Hermes can connect to HTTP / Streamable HTTP and SSE MCP servers, but those transports are not needed for the first proof.
Hermes ordinary MCP tool calls should not be treated as protected by a proven human approval gate.
Hermes session storage can preserve tool calls and tool results.
MCP tool results may enter model context and session history.
MCP roots are advisory and must not be treated as an access-control boundary.
Raw Hermes File Operations are not acceptable for canonical Kaizen repositories.
Kaizen-owned server-side policy enforcement is required.
```

## Core decision

Kaizen will pursue the first Hermes MCP proof using this shape:

```text
Hermes Desktop
-> local stdio MCP
-> Kaizen-owned read-only MCP server
-> fixed-root governed access to C:\dev\kaizen-docs
-> bounded structured outputs
-> audit events
```

The first proof must not use:

```text
ngrok;
public tunnel;
HTTP server exposure;
SSE server exposure;
raw Hermes File Operations;
generic filesystem MCP server;
canonical writes;
staging writes;
write-capable tools;
terminal tools;
browser tools;
raw SQL;
Postgres mutation;
Qdrant mutation;
source-repository mutation;
provider/customer-data access.
```

## Contract name

```text
kaizen.hermes.read_only_mcp.v0.1
```

## Transport contract

Primary transport:

```text
local stdio MCP
```

Postponed transports:

```text
HTTP / Streamable HTTP
SSE
remote MCP
public tunnel
ngrok
```

Rationale:

```text
Hermes Desktop is local.
The Kaizen server is local.
The first proof only needs local process communication.
Public exposure adds no required capability and increases risk.
```

## Hermes configuration expectations

Expected Windows Hermes configuration location:

```text
%LOCALAPPDATA%\hermes\config.yaml
```

Expected server entry:

```yaml
mcp_servers:
  kaizen_docs:
    command: "<absolute-path-to-server-runtime>"
    args:
      - "<server-entrypoint-arg-1>"
      - "<server-entrypoint-arg-2>"
    env:
      KAIZEN_DOCS_ROOT: "C:\\dev\\kaizen-docs"
      KAIZEN_READ_ONLY: "true"
      KAIZEN_AUDIT_DIR: "<absolute-local-audit-dir>"
    timeout: 60
    connect_timeout: 20
    tools:
      include:
        - "kaizen_docs_status"
        - "kaizen_docs_manifest"
        - "kaizen_docs_read_text"
        - "kaizen_docs_search_text"
        - "kaizen_packet_read"
        - "kaizen_context_pack_get"
      resources: false
      prompts: false
    sampling:
      enabled: false
```

Configuration rules:

```text
Use an absolute command path.
Keep args minimal and deterministic.
Prefer a dedicated packaged executable or pinned runtime/module invocation.
Do not rely on a documented cwd field; none is accepted by this contract.
Set KAIZEN_DOCS_ROOT explicitly.
Keep tools.include explicit even if the server exposes only the six tools.
Disable prompts for the first proof.
Disable resources for the first proof unless a later packet records a concrete need.
Disable sampling for the first proof.
Do not configure HTTP/SSE/ngrok for the first proof.
```

## Server-side enforcement model

The Kaizen MCP server is the policy boundary.

Hermes, MCP roots, host UI conventions, prompt instructions, and model obedience are not sufficient authority boundaries.

The server must enforce:

```text
fixed root: C:\dev\kaizen-docs;
read-only behavior;
manifest-approved reads;
manifest-approved search;
text-only file handling;
no path traversal;
no absolute-path user input;
no UNC path input;
no Windows device namespace input;
no extended-length path escape;
no symlink / junction escape;
no hidden control-plane directories unless explicitly manifested;
no .git traversal;
no secrets or environment files;
bounded results;
structured failure modes;
audit event emission.
```

## Model-facing reference strategy

The model-facing interface should prefer Kaizen references over raw paths.

Preferred external references:

```text
doc_ref;
packet_id;
context_pack_id;
manifest scope;
line range;
section selector if later earned.
```

Raw filesystem paths are not a first-class model-facing capability.

Where a relative path is returned, it is evidence metadata only, not an invitation for arbitrary path traversal.

## Global output envelope

Every successful tool result should include a structured payload with these common fields when applicable:

```yaml
contract_name: kaizen.hermes.read_only_mcp.v0.1
contract_version: "0.1"
tool_name: <tool name>
authority_status: read_only_evidence
source_locator: <doc_ref | packet_id | manifest scope | context_pack_id | server status>
doc_ref: <nullable>
packet_id: <nullable>
relative_path: <nullable>
sha256: <nullable>
truncated: <boolean>
known_limitations: []
audit_event_id: <server-generated id if available>
generated_at: <ISO-8601 timestamp>
structured_result: <tool-specific object>
```

Every failure should use a structured failure envelope:

```yaml
contract_name: kaizen.hermes.read_only_mcp.v0.1
contract_version: "0.1"
tool_name: <tool name>
ok: false
error_code: <stable error code>
message: <human-readable bounded message>
authority_status: no_state_change
known_limitations: []
audit_event_id: <server-generated id if available>
generated_at: <ISO-8601 timestamp>
```

Tool results should use MCP structured output where supported.

The canonical payload should be structured JSON / structuredContent. A short text summary may be included for host readability, but the text summary must not carry meaning absent from the structured payload.

## Global limits

Initial suggested limits:

```text
maximum returned text per read call: 24 KiB;
maximum returned lines per read call: 200;
maximum search matches per call: 20;
maximum search snippet length: 300 characters;
maximum manifest entries per call: 200;
maximum context pack response: 50 KiB;
maximum status response: 8 KiB;
maximum packet read response: 32 KiB;
```

A later implementation packet may tune these limits downward or upward, but it must preserve bounded-output behavior.

## Allowed file types

Initial text allowlist:

```text
.md
.txt
.json
.yaml
.yml
.toml
```

Conditionally deferred until earned:

```text
.csv
```

Explicitly excluded for the first proof:

```text
.pdf
.doc
.docx
.xls
.xlsx
.ppt
.pptx
.images
.archives
.sqlite
.db
binary blobs
large generated logs
unknown extensions
```

## Forbidden path families

The server must reject or omit these by default:

```text
outside C:\dev\kaizen-docs;
absolute user-supplied paths;
.. traversal;
UNC paths;
Windows device namespaces;
extended-length path tricks;
symlink or junction escapes;
.git;
node_modules;
.venv;
__pycache__;
build / dist / target caches;
.env and environment files;
credential files;
secret files;
backup plaintext;
local private config;
large generated logs;
unknown binary or generated artifacts;
```

## Audit event minimum

Every tool call should emit a local Kaizen-owned audit event.

Minimum audit event fields:

```yaml
event_id: <stable local id>
timestamp: <ISO-8601 UTC>
contract_name: kaizen.hermes.read_only_mcp.v0.1
contract_version: "0.1"
tool_name: <tool name>
session_id: <nullable if host provides>
request_id: <server-generated id>
input_summary: <bounded redacted summary>
resolved_scope: <root | manifest scope | doc_ref | packet_id | context_pack_id>
result_status: ok | error
error_code: <nullable>
returned_bytes: <integer>
returned_items: <integer nullable>
truncated: <boolean>
state_changed: false
```

Audit logs are evidence. They are not canonical project intelligence by themselves.

## Tool: kaizen_docs_status

Purpose:

```text
Return a compact orientation and policy snapshot for the Kaizen docs MCP server.
```

Input schema:

```yaml
include_counts:
  type: boolean
  required: false
  default: false
```

Output structured_result fields:

```yaml
root_label: docs
root_path: C:\dev\kaizen-docs
read_only: true
transport: stdio
manifest_available: <boolean>
manifest_version: <nullable string>
git_available: <boolean>
git_head: <nullable string>
git_branch: <nullable string>
policy:
  prompts_enabled: false
  resources_enabled: false
  sampling_enabled: false
  raw_file_operations: false
  writes_enabled: false
counts: <nullable bounded object>
```

Constraints:

```text
No file contents.
No broad recursive file listing.
Counts optional and bounded.
```

Failure modes:

```text
ROOT_UNAVAILABLE
MANIFEST_UNAVAILABLE
GIT_STATUS_UNAVAILABLE
RESULT_TOO_LARGE
```

Human confirmation:

```text
not required
```

## Tool: kaizen_docs_manifest

Purpose:

```text
Expose approved document references and navigation structure without raw filesystem crawling.
```

Input schema:

```yaml
scope:
  type: enum
  values: [root, milestone, packet, research_results, decisions, specs, hermes]
  required: false
  default: root
query:
  type: string
  required: false
limit:
  type: integer
  required: false
  default: 100
```

Output structured_result fields:

```yaml
scope: <scope>
query: <nullable string>
entries:
  - doc_ref: <string>
    kind: <string>
    title: <string>
    relative_path: <string>
    sha256: <string>
    authority_status: <string>
    updated: <nullable string>
truncated: <boolean>
returned_entries: <integer>
```

Constraints:

```text
Manifest-approved entries only.
No raw recursive disk crawl as model-facing behavior.
No hidden/system/control-plane paths unless explicitly approved.
```

Failure modes:

```text
INVALID_SCOPE
QUERY_TOO_BROAD
MANIFEST_UNAVAILABLE
RESULT_TOO_LARGE
```

Human confirmation:

```text
not required
```

## Tool: kaizen_docs_read_text

Purpose:

```text
Read one approved text document by doc_ref with bounded pagination and evidence metadata.
```

Input schema:

```yaml
doc_ref:
  type: string
  required: true
start_line:
  type: integer
  required: false
  default: 1
max_lines:
  type: integer
  required: false
  default: 120
```

Output structured_result fields:

```yaml
doc_ref: <string>
title: <nullable string>
relative_path: <string>
sha256: <string>
start_line: <integer>
returned_lines: <integer>
total_lines: <nullable integer>
truncated: <boolean>
content: <line-numbered text>
```

Constraints:

```text
Doc_ref must resolve through the manifest.
Start line must be positive.
Max lines must be bounded by server policy.
Text-only allowlist applies.
No binary or unknown extension reads.
No arbitrary raw path reads.
```

Failure modes:

```text
DOC_REF_NOT_FOUND
DOC_NOT_APPROVED
UNSUPPORTED_FILE_TYPE
BINARY_DISALLOWED
FILE_TOO_LARGE
LINE_RANGE_INVALID
OUT_OF_ROOT
PATH_FORBIDDEN
RESULT_TOO_LARGE
```

Human confirmation:

```text
not required
```

## Tool: kaizen_docs_search_text

Purpose:

```text
Search approved Kaizen docs or an approved generated index with bounded snippets.
```

Input schema:

```yaml
query:
  type: string
  required: true
scope:
  type: enum
  values: [all, milestone, packet, research_results, decisions, specs, hermes]
  required: false
  default: all
limit:
  type: integer
  required: false
  default: 20
```

Output structured_result fields:

```yaml
query: <string>
scope: <scope>
matches:
  - doc_ref: <string>
    title: <nullable string>
    relative_path: <string>
    line: <integer nullable>
    snippet: <string>
    sha256: <nullable string>
returned_matches: <integer>
truncated: <boolean>
searched_scope_summary: <string>
```

Constraints:

```text
Approved manifest or approved index only.
No raw disk search outside approved scope.
No unrestricted regex in v0.1.
Query length bounded.
Snippet length bounded.
```

Failure modes:

```text
QUERY_EMPTY
QUERY_TOO_LONG
QUERY_TOO_BROAD
INVALID_SCOPE
SEARCH_INDEX_UNAVAILABLE
RESULT_TOO_LARGE
```

Human confirmation:

```text
not required
```

## Tool: kaizen_packet_read

Purpose:

```text
Read a packet as a governed project artifact rather than as an arbitrary file.
```

Input schema:

```yaml
packet_id:
  type: string
  required: true
include_related:
  type: boolean
  required: false
  default: false
max_body_lines:
  type: integer
  required: false
  default: 160
```

Output structured_result fields:

```yaml
packet_id: <string>
doc_ref: <string>
title: <string>
status: <nullable string>
milestone: <nullable string>
relative_path: <string>
sha256: <string>
summary: <nullable string>
body_excerpt: <line-numbered text>
related_refs:
  - doc_ref: <string>
    relationship: <string>
truncated: <boolean>
```

Constraints:

```text
Packet must be known to the manifest or packet index.
Related refs are capped.
Body excerpt is capped.
Does not infer approval or closure status beyond recorded fields.
```

Failure modes:

```text
PACKET_NOT_FOUND
PACKET_NOT_APPROVED_FOR_READ
PACKET_INDEX_UNAVAILABLE
RELATED_LIMIT_EXCEEDED
RESULT_TOO_LARGE
```

Human confirmation:

```text
not required
```

## Tool: kaizen_context_pack_get

Purpose:

```text
Return a bounded, preassembled context pack for a defined task or lane.
```

Input schema:

```yaml
context_pack_id:
  type: string
  required: true
mode:
  type: enum
  values: [summary, standard]
  required: false
  default: summary
```

Output structured_result fields:

```yaml
context_pack_id: <string>
mode: <summary | standard>
purpose: <string>
items:
  - doc_ref: <string>
    role: <string>
    relative_path: <string>
    sha256: <string>
    excerpt: <bounded text>
known_omissions:
  - <string>
truncated: <boolean>
```

Constraints:

```text
Context packs must be defined by Kaizen or generated from approved manifests.
No model-generated arbitrary pack expansion in v0.1.
No raw corpus dump.
Every included item must be traceable.
Response size capped.
```

Failure modes:

```text
CONTEXT_PACK_NOT_FOUND
CONTEXT_PACK_STALE
PACK_ITEM_NOT_FOUND
PACK_ITEM_NOT_APPROVED
RESULT_TOO_LARGE
```

Human confirmation:

```text
not required
```

## Forbidden tools and capabilities

The v0.1 read-only MCP server must not expose tools for:

```text
write;
patch;
delete;
rename;
move;
mkdir;
copy;
commit;
push;
branch mutation;
terminal;
shell command;
browser;
network fetch;
provider calls;
raw SQL;
Postgres mutation;
Qdrant mutation;
Qdrant upsert/delete/admin;
vault mutation;
staging mutation;
source-repository mutation;
external messaging;
scheduling;
memory mutation;
owner approval;
promotion;
audit verdict;
implementation-ready verdict;
spending;
publication;
business action.
```

If Hermes requests any forbidden capability, the correct response is a structured refusal from the server or absence of the tool.

Preferred failure code when applicable:

```text
CAPABILITY_FORBIDDEN
```

## Relationship to future Kaizen MCP tool families

027I is the first read-only proof layer. It does not define the whole Kaizen MCP ambition.

Future Kaizen-owned MCP/tool families may include:

```text
claim extraction draft submit;
source-summary draft submit;
review queue submission;
validation result reads;
staged note proposal creation;
diff proposal submission;
amendment candidate preparation;
implementation-return intake;
Observatory signal packs;
market signal packs;
strategy context packs;
opportunity candidate submission;
feature-upgrade candidate submission;
strategy hypothesis submission;
experiment candidate submission;
project-improvement candidate submission.
```

Future tool families must inherit these 027I principles:

```text
Kaizen-owned policy enforcement;
typed schemas;
bounded outputs;
explicit evidence and source locators;
audit events;
no hidden authority transition;
server-side validation;
least dangerous sufficient capability;
human review before acceptance, promotion, spending, publication, implementation-ready marking, or business action.
```

Future mutating or write-adjacent surfaces must not be added to this server casually. They require separate research, contract, proof, implementation packet, validation, and owner approval.

## Acceptance criteria

027I is acceptable as a contract packet if it:

```text
records local stdio as the first transport;
records no ngrok / no public tunnel for the first proof;
records Hermes config expectations for Windows;
defines the six read-only tools;
defines global output and failure envelopes;
defines path, file-type, root, and manifest restrictions;
defines audit event expectations;
explicitly disables prompts/resources/sampling for the first proof unless later justified;
explicitly forbids writes, patching, terminal, browser, database, Qdrant, staging, and canonical mutation;
preserves the larger future Kaizen MCP direction without authorizing it.
```

## Current non-authorizations

027I does not authorize:

```text
implementation code;
server creation;
Hermes configuration changes;
Hermes File Operations;
Hermes writes;
Hermes staging writes;
Hermes canonical writes;
Hermes source-repo access;
Hermes terminal access;
Hermes browser access;
Hermes schedules or always-on behavior;
Hermes memory mutation;
Hermes skill mutation;
Postgres access;
Postgres mutation;
Qdrant access;
Qdrant runtime start;
Qdrant mutation;
Tauri implementation;
Kaizen app implementation;
custom chatbox implementation;
platform mutation;
vault mutation;
staging mutation;
database mutation;
production MCP packaging;
HTTP/SSE MCP hosting;
ngrok/public tunneling;
commercial or production use;
implementation of validators;
creation of database tables;
creation of runtime APIs;
future draft/staging/strategy/opportunity tool families.
```

## Recommended next lane

Next packet:

```text
027J — Local Stdio MCP Proof Plan
```

027J should plan a fixture or controlled-doc-root proof for the 027I contract before implementation.

It should verify:

```text
server launch shape;
Hermes config shape;
tool discovery;
tools.include filtering;
prompts/resources/sampling disabled;
read-only behavior;
root enforcement;
manifest-only reads;
bounded outputs;
structured failures;
audit events;
no state changes.
```

Only after 027J is accepted should Kaizen consider:

```text
027K — Read-Only MCP Server Implementation Packet
027L — Hermes Desktop Integration Trial
027M — Frontier Reasoning Context-Pack Design
```

## Related files

```text
03-research-results/435-packet-026a-hermes-desktop-integration-direction.md
03-research-results/436-packet-026b-hermes-read-only-tool-inventory-and-orientation-test-contract.md
03-research-results/437-packet-026c-hermes-target-capability-profile.md
03-research-results/438-packet-026d-hermes-026b-a-no-file-access-orientation-test-result.md
03-research-results/439-packet-026e-026f-hermes-pasted-context-clerk-workflow-results.md
03-research-results/440-packet-026g-hermes-pasted-context-claim-extraction-draft-sample-result.md
03-research-results/441-packet-027a-hermes-draft-output-intake-contract-planning.md
03-research-results/442-packet-027b-hermes-draft-output-intake-contract-v0-1.md
03-research-results/443-packet-027c-hermes-draft-output-intake-contract-manual-review.md
03-research-results/444-packet-027d-hermes-draft-output-intake-contract-manual-trial-result.md
03-research-results/445-packet-027e-hermes-draft-output-intake-contract-manual-trial-refinements.md
03-research-results/446-packet-027f-hermes-draft-output-intake-contract-refined-manual-trial-result.md
03-research-results/447-packet-027g-hermes-larger-pasted-context-intake-contract-trial-result.md
07-hermes/hermes-permission-matrix.md
07-hermes/hermes-write-access-preconditions.md
05-specs/staging-write-wrapper-and-promotion-recovery.md
ROADMAP_V0.4.md
00-entrypoint/LLM_START_HERE.md
```
