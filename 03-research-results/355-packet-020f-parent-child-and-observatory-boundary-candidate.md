# Packet 020F — Parent/Child Dependency Map and Observatory / IMI Boundary Candidate

Status: reconciled boundary candidate / decision-prep record
Date: 2026-06-22
Packet: 020F
Milestone: 14

## 1. Purpose

Produce the reconciled parent/child dependency map for Kaizen, Neon Ronin, SearchClarity, and Observatory / IMI, using the Stage A idea-only output and 020E reconciliation findings.

This record prepares an Observatory / IMI boundary decision candidate without deciding or implementing it.

## 2. Starting checkpoint

Docs repository at 020F start:

```text
root: docs
branch: main
HEAD: 2ab38a371e259590e74fe8b8b8d0c84a25d3fa9e
working tree: clean
upstream: origin/main
ahead/behind: 24 / 0
```

Platform repository at 020F start:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

## 3. Authority basis

```text
03-research-results/345-packet-020a-m14-workload-selection-and-boundary-registration.md
03-research-results/349-packet-020c-repo-state-pinning-and-idea-only-firewall.md
03-research-results/351-packet-020d-stage-a-idea-only-generation-output.md
03-research-results/354-packet-020e-stage-a-repo-reconciliation-report.md
05-specs/milestone-14-first-real-internal-project-governed-run.md
```

## 4. Reconciled identity map

```text
Kaizen
  identity: governed project-intelligence and implementation-doc system
  role: turns ideas into governed project records, specs, audits, packets, handoffs, returns, and improvement loops
  does not: become the downstream project or code the downstream application

Neon Ronin
  identity: downstream parent operating-system project
  role: multi-agent business operating system / workspace operating layer for managing small businesses
  relation to Kaizen: consumer of Kaizen-produced implementation docs, not a subsystem of Kaizen
  relation to SearchClarity: parent platform / workspace operating layer

SearchClarity
  identity: child/dependent workspace/business lane
  role: search visibility / marketplace SEO / LLM-citation service business
  relation to Neon Ronin: intended workspace/business lane under Neon Ronin
  relation to Kaizen: downstream project intelligence target, not Kaizen

Observatory / IMI
  identity: planned shared intelligence capability / ownership-boundary candidate
  role: SERP / SEO / LLM-citation / marketplace intelligence foundation usable across multiple projects
  relation to Kaizen: currently planned in Kaizen context as governed intelligence capability unless future decision externalizes it
  relation to Neon Ronin: possible consumer / operationalizer
  relation to SearchClarity: possible signal source / beneficiary / consumer
```

## 5. Reconciled dependency map

```text
Kaizen
  -> produces governed docs for Neon Ronin
  -> produces governed docs for SearchClarity
  -> may govern Observatory / IMI as shared capability or as future separate project

Neon Ronin
  -> parent platform / workspace operating layer
  -> can host/manage SearchClarity as a workspace/business lane
  -> can score, route, operationalize, or coordinate signals/workflows
  -> may consume Observatory / IMI intelligence later

SearchClarity
  -> child/dependent workspace/business lane
  -> captures or defines search/visibility/marketplace/LLM-citation signals
  -> may feed or benefit from Observatory / IMI intelligence later
  -> may be operated through Neon Ronin later

Observatory / IMI
  -> shared intelligence capability candidate
  -> should not be prematurely duplicated across projects
  -> should not be implemented under M14 Stage A
  -> requires explicit future ownership decision before build
```

## 6. Preferred reconciled sentence

Use this sentence in future M14 docs:

```text
SearchClarity captures the signal. Neon Ronin scores the signal. Kaizen governs the project-intelligence and implementation-doc chain.
```

Extended form:

```text
SearchClarity is the customer-facing search-intelligence business lane; Neon Ronin is the workspace operating layer that can later manage, route, and score that lane; Kaizen is the governed project-intelligence system that produces the docs and return discipline for both.
```

## 7. Boundary rules

### Kaizen boundary

```text
Kaizen produces project intelligence and implementation-ready docs.
Kaizen does not code Neon Ronin or SearchClarity.
Kaizen does not become Neon Ronin.
Kaizen does not become SearchClarity.
Kaizen does not silently implement Observatory / IMI.
```

### Neon Ronin boundary

```text
Neon Ronin is the parent workspace operating project.
Neon Ronin may later operate SearchClarity as a workspace/business lane.
Neon Ronin should not absorb SearchClarity's customer-facing business identity.
Neon Ronin should not be treated as already fully built unless repo evidence and tests prove it.
```

### SearchClarity boundary

```text
SearchClarity is a child/dependent workspace/business lane.
SearchClarity captures or defines visibility/search/citation signals.
SearchClarity is not the parent platform.
SearchClarity is not Kaizen.
SearchClarity should remain lean until implementation and repo reality justify deeper build docs.
```

### Observatory / IMI boundary

```text
Observatory / IMI is a shared capability / ownership question.
Repeated Observatory references across project repos must be reported faithfully.
Those references must then be analyzed under the owner clarification that the intended capability is shared.
No Observatory / IMI implementation is authorized by this record.
```

## 8. Observatory / IMI boundary candidate

Problem statement:

```text
Observatory / IMI is referenced across related projects because the intended capability is useful across SERP, SEO, LLM-citation, marketplace intelligence, and business-workspace operations.
The recurring references create ownership ambiguity.
Without a decision, future agents may duplicate, fragment, or incorrectly implement Observatory-shaped systems inside multiple projects.
```

Candidate decision question:

```text
Should Observatory / IMI remain a Kaizen subsystem/capability, or become its own Kaizen-governed project consumed by Neon Ronin, SearchClarity, and future intelligence projects?
```

## 9. Candidate options

### Option A — Keep Observatory / IMI as Kaizen capability

Description:

```text
Observatory / IMI remains inside the Kaizen project as a governed intelligence capability.
Neon Ronin and SearchClarity consume outputs or docs from it but do not own it.
```

Strengths:

```text
keeps governance centralized;
avoids creating another project too early;
fits current Kaizen planning context;
reduces project sprawl during v1 proof.
```

Risks:

```text
may overload Kaizen with product-like capabilities;
may blur Kaizen's role from project-intelligence system into intelligence-platform system;
may make Neon Ronin/SearchClarity dependencies feel indirect.
```

### Option B — Promote Observatory / IMI into its own Kaizen-governed project

Description:

```text
Observatory / IMI becomes a separate governed project with Kaizen-produced docs, explicit ownership, and consumer relationships.
Neon Ronin and SearchClarity become consumers/contributors, not owners.
```

Strengths:

```text
cleanest long-term ownership model;
best fit for multi-project SERP / SEO / LLM-citation / marketplace intelligence;
prevents SearchClarity or Neon Ronin from accidentally owning shared intelligence infrastructure;
keeps Kaizen as the governing system rather than the final product surface.
```

Risks:

```text
adds another project before v1 closure;
requires more docs and governance overhead;
could distract from M14 if acted on too early.
```

### Option C — Leave ownership undecided through M14 Stage A

Description:

```text
Do not decide now. Keep Observatory / IMI as a boundary candidate and block implementation until a future packet.
```

Strengths:

```text
safe for current M14 stage;
prevents premature implementation;
lets repo reconciliation and later packets gather more evidence.
```

Risks:

```text
does not resolve future agent confusion;
requires strong stop conditions to prevent drift;
may leave ambiguous wording in downstream project docs.
```

## 10. Steward recommendation

Recommended near-term posture:

```text
Adopt Option C for M14 Stage A and prepare Option B as the likely long-term direction.
```

Reason:

```text
M14's purpose is to prove Kaizen, not to launch a new Observatory project. However, the intended scope of Observatory / IMI is broad enough that long-term it likely deserves its own Kaizen-governed project rather than being trapped inside Neon Ronin or SearchClarity.
```

Recommended wording for future docs:

```text
For M14 Stage A, Observatory / IMI remains a shared capability boundary candidate. No implementation is authorized. Long-term ownership should be decided by a future Kaizen decision packet, with strong consideration for treating Observatory / IMI as its own governed project consumed by Neon Ronin, SearchClarity, and future intelligence projects.
```

## 11. Required future decision candidate

Future decision candidate title:

```text
Decision — Observatory / IMI Ownership and Consumer Boundary
```

Decision should answer:

```text
Does Observatory / IMI remain a Kaizen capability, or become its own governed project?
What may Neon Ronin consume from it?
What may SearchClarity contribute to or consume from it?
What data is prohibited until future approval?
What implementation surfaces are explicitly blocked?
What project owns schema, ingestion, retention, and provenance?
```

Recommended timing:

```text
After M14 Stage A docs and before any Observatory / IMI implementation-bearing packet.
```

## 12. Reconciled claims for downstream docs

Use these claims as candidate reconciled language:

```text
Kaizen creates governed implementation docs; connected coding agents may later execute approved packets.
Neon Ronin is the parent workspace operating system for business lanes.
SearchClarity is a child workspace/business lane under Neon Ronin.
SearchClarity captures the signal; Neon Ronin scores the signal.
Observatory / IMI is a shared capability boundary candidate, not an implemented system.
Repo-specific Observatory wording must be reported before ownership analysis.
Reserved repo shapes are not built features.
Docs-only business planning is not implementation.
Private/git-ignored evidence remains PRIVATE/LOCAL and must not be invented.
```

## 13. Open questions

```text
What exact Neon Ronin docs should receive the parent/workspace boundary later, if any?
What exact SearchClarity docs should receive the child/workspace boundary later, if any?
Should Observatory / IMI be externalized before Kaizen v1 closure or deferred until v1+?
What is the smallest future Stage B task that proves implementation-return without touching Observatory / IMI?
```

## 14. 020F verdict

```text
PASS — PARENT/CHILD DEPENDENCY MAP AND OBSERVATORY / IMI BOUNDARY CANDIDATE RECORDED
```

M14 may proceed to 020G: Implementation-Ready Doc Quality Audit and Stage B Candidate Recommendation.

## 15. Non-authorization

This record does not authorize:

```text
Observatory / IMI ownership decision;
Observatory / IMI implementation;
repo mutation in Neon Ronin;
repo mutation in SearchClarity;
Stage B execution;
coding Neon Ronin;
coding SearchClarity;
provider/customer-data work;
private/git-ignored data use;
Qdrant/Hermes/Tauri implementation;
Git push.
```
