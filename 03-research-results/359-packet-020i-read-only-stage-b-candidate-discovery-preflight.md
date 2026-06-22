# Packet 020I — Read-Only Stage B Candidate Discovery Preflight

Status: candidate-discovery preflight / evidence request
Date: 2026-06-22
Packet: 020I
Milestone: 14

## 1. Purpose

Prepare the read-only Stage B candidate discovery step after the successful 020H fresh-context resumption proof.

This packet does not select a Stage B task, does not authorize coding, and does not mutate Neon Ronin or SearchClarity.

## 2. Starting checkpoint

Docs repository at 020I start:

```text
root: docs
branch: main
HEAD: a0db574fa27de7281435384b009362a5e27dbbba
working tree: clean
upstream: origin/main
ahead/behind: 28 / 0
```

Platform repository at 020I start:

```text
root: platform
branch: main
HEAD: b7593c5ee90fd32c1e2a86572cc570d307de2be6
working tree: clean
remotes: none
```

## 3. Authority basis

```text
03-research-results/356-packet-020g-implementation-ready-doc-quality-audit.md
03-research-results/357-packet-020h-fresh-context-resumption-pack.md
03-research-results/358-packet-020h-fresh-context-resumption-proof.md
```

## 4. Current state from 020H

020H disposition records:

```text
Fresh-context resumption proof passed with fixes.
Reading-path fixes were applied.
Stage B execution remains unauthorized.
Next recommended work is read-only Stage B candidate discovery.
```

## 5. Tooling limitation

Go8 fixed roots available in this session do not include Neon Ronin or SearchClarity.

Therefore, this steward cannot honestly claim direct fixed-root reads of:

```text
C:\dev\neon-ronin
C:\dev\searchclarity
```

This packet preserves the limitation instead of fabricating candidate paths or test commands.

## 6. Candidate-discovery target

Preferred target repo:

```text
C:\dev\neon-ronin
```

Reason:

```text
Neon Ronin is the parent workspace/business operating project.
020G recommended preferring Neon Ronin over SearchClarity for Stage B candidate discovery.
SearchClarity is a child workspace/business lane and docs-only/pre-launch in the current evidence.
A Neon Ronin candidate is more likely to prove the parent project boundary without making SearchClarity the proof center.
```

## 7. Candidate class to look for

Safest candidate class:

```text
tiny docs-boundary clarification inside Neon Ronin;
exactly one or two Markdown files;
no code unless an existing test/check surface is clearly proven;
no Observatory / IMI implementation;
no SearchClarity implementation;
no provider/customer/private data.
```

Preferred content of a tiny Stage B docs task:

```text
clarify SearchClarity as a workspace/business lane under Neon Ronin;
clarify that SearchClarity captures the signal and Neon Ronin scores the signal;
clarify that Observatory / IMI ownership is undecided/shared and not implemented;
clarify that Kaizen governs implementation-doc and return discipline, not Neon Ronin runtime behavior.
```

## 8. Candidate classes to avoid

Avoid these as Stage B first task:

```text
new app code;
new agent runtime;
new Tauri shell;
new Hermes integration;
new Qdrant/vector/index work;
new Observatory / IMI code, schema, ingestion, or records;
SearchClarity service implementation;
provider/customer data;
private/git-ignored sample data;
large repo cleanup;
multi-file architecture rewrite;
any task lacking a simple diff and clear rollback.
```

## 9. Required owner-side read-only evidence

Because downstream repos are not Go8 fixed roots, owner should run the following read-only PowerShell from any terminal and paste the output.

```powershell
$repo = "C:\dev\neon-ronin"
Push-Location $repo

Write-Host "===== NEON RONIN STATUS ====="
git status --short --branch
Write-Host "HEAD:"
git rev-parse HEAD
Write-Host "BRANCH:"
git branch --show-current

Write-Host "===== ROOT FILES ====="
Get-ChildItem -Force | Select-Object Mode,Length,Name

Write-Host "===== MARKDOWN FILES DEPTH 3 ====="
Get-ChildItem -Recurse -File -Include *.md |
  Where-Object { $_.FullName -notmatch '\\.git|node_modules|\.venv|__pycache__|dist|build' } |
  ForEach-Object {
    $rel = Resolve-Path -Relative $_.FullName
    Write-Host $rel
  }

Write-Host "===== TEST / CHECK FILES DEPTH 5 ====="
Get-ChildItem -Recurse -File |
  Where-Object { $_.FullName -notmatch '\\.git|node_modules|\.venv|__pycache__|dist|build' } |
  Where-Object { $_.Name -match 'test|spec|check|hammer|pyproject|package|justfile|makefile|tox|ruff|pytest' } |
  ForEach-Object {
    $rel = Resolve-Path -Relative $_.FullName
    Write-Host $rel
  }

Write-Host "===== SEARCH: SearchClarity / Observatory / signal ====="
Select-String -Path (Get-ChildItem -Recurse -File -Include *.md |
  Where-Object { $_.FullName -notmatch '\\.git|node_modules|\.venv|__pycache__|dist|build' }).FullName `
  -Pattern 'SearchClarity','Search Clarity','Observatory','observatory','signal','workspace','business lane' `
  -SimpleMatch |
  ForEach-Object {
    $rel = Resolve-Path -Relative $_.Path
    "{0}:{1}: {2}" -f $rel,$_.LineNumber,$_.Line.Trim()
  }

Pop-Location
```

## 10. Evidence needed to select a Stage B candidate

A concrete Stage B candidate cannot be selected until the evidence shows:

```text
current Neon Ronin HEAD still matches or is explicitly repinned;
working tree is clean;
exact candidate file path(s);
exact existing wording to change or exact insertion point;
whether there is a lightweight check/test command;
whether candidate is docs-only or code-bearing;
whether candidate avoids private/customer/provider data;
whether candidate avoids Observatory implementation.
```

## 11. Selection criteria for next packet

The next packet may select a Stage B candidate only if it can record:

```text
one downstream repo;
one starting commit;
one tiny objective;
one or two allowed paths;
explicit prohibited paths;
acceptance criteria;
read-only discovered check/test command or explicit no-test docs-only rationale;
rollback plan;
implementation-return evidence requirements;
owner approval phrase.
```

## 12. Current candidate recommendation

Current evidence supports only a candidate class, not a concrete task.

Recommended candidate class:

```text
Neon Ronin docs-boundary clarification around SearchClarity workspace/business-lane role and Observatory / IMI non-implementation boundary.
```

Concrete candidate status:

```text
NOT YET SELECTED — waiting for owner-side read-only evidence or fixed-root access.
```

## 13. 020I verdict

```text
PASS — STAGE B CANDIDATE-DISCOVERY PREFLIGHT RECORDED; EXACT DOWNSTREAM EVIDENCE REQUIRED
```

M14 may proceed after owner supplies the read-only Neon Ronin evidence.

## 14. Non-authorization

This packet does not authorize:

```text
Stage B execution;
Stage B task selection;
coding Neon Ronin;
coding SearchClarity;
repo mutation in Neon Ronin;
repo mutation in SearchClarity;
platform mutation;
vault mutation;
staging mutation;
database mutation;
provider/customer-data work;
private/git-ignored data use;
Qdrant/Hermes/Tauri/Observatory implementation;
Git push.
```
