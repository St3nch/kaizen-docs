---
id: kz-result-01KTV8P4N6Q8S0W2Y4Z6ABCDEF
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-10T15:42:00-04:00
updated: 2026-06-10T15:42:00-04:00
review_status: steward-reviewed
summary: "Packet 011A read-only protected-root inventory, secrets posture, backup option comparison, and recommended owner decision package."
---

# Research Result 107 - Packet 011A Protected-Root Inventory and Posture Analysis

## Scope and method

Packet 011A performed read-only inspection only.

No archive, backup generation, encryption, upload, restore, remote creation, push, deletion, secret movement, or live-root mutation occurred.

Verified checkpoint:

```text
kaizen-docs:
branch main
approved Packet 011A planning batch based on commit 4a88f25bb3e266f6f39ad59c19cf60f391881d20
upstream origin/main

kaizen-platform:
branch main
HEAD 1b8be1d6d42d768587dddb2be8415fa24b670561
clean
remote none

kaizen-vault:
branch main
HEAD fc4b03397d770f9b4a8a2f5e7f71e33981bcd181
clean
remote none
```

## Protected-root inventory

## 1. Canonical vault

Root:

```text
C:\dev\kaizen\vault
```

Repository posture:

```text
Git repository: yes
branch: main
HEAD: fc4b03397d770f9b4a8a2f5e7f71e33981bcd181
working tree: clean
remote: none
```

Bounded tracked-content manifest:

```text
tracked content files outside .git: 14
tracked content bytes outside .git: 71,271
```

Critical files include:

```text
_governance/promotion-log.jsonl
projects/kaizen-platform/current-state.md
projects/kaizen-platform/handoffs/implement-governed-amendment-support.md
```

Critical exact hashes:

```text
_governance/promotion-log.jsonl:
e17cc9f74f40b4b66ffcad432bceff16d5a583a922228db734b737863e95328f

projects/kaizen-platform/current-state.md:
5dc7608d83b91791fa1930597db6bb1a727eb3021ccc9eb0b9b356d0d13a0e70

projects/kaizen-platform/handoffs/implement-governed-amendment-support.md:
62a79e2104bf9ca5daf0c1e96a32f1b01ebc1cec290e5ab4c76ccf8d357fe0be
```

Required recovery material:

- complete `.git` directory, refs, objects, configuration, and working tree;
- all tracked canonical Markdown;
- complete governance log;
- `.gitignore`;
- any non-ignored Obsidian configuration intentionally added later.

Current ignored vault material:

```text
.obsidian/workspace.json
.obsidian/workspaces.json
.obsidian/cache/
.trash/
.DS_Store
Thumbs.db
```

Disposition:

- workspace/cache/trash and OS metadata are rebuildable and excluded;
- no current ignored item is required to interpret canonical project intelligence;
- future Obsidian settings or plugins must not be assumed recoverable unless separately classified.

## 2. Deterministic platform

Root:

```text
C:\dev\kaizen\platform
```

Repository posture:

```text
Git repository: yes
branch: main
HEAD: 1b8be1d6d42d768587dddb2be8415fa24b670561
working tree: clean
remote: none
```

The bounded manifest contains:

- project metadata and reconstruction inputs;
- `pyproject.toml`;
- `.python-version`;
- accepted JSON schemas;
- platform source package;
- complete test suite and fixtures;
- operator and CLI entry points.

Environment reconstruction contract:

```text
Python: >=3.11,<3.12
runtime dependency: PyYAML >=6.0.2,<7
dev dependency: pytest >=8.3,<9
build backend: setuptools.build_meta
```

Required recovery material:

- complete `.git` directory, refs, objects, configuration, and working tree;
- `.gitattributes`, `.gitignore`, `.python-version`;
- `pyproject.toml`;
- `README.md` and `AGENTS.md`;
- `schemas/`, `src/`, and `tests/`.

Current ignored/rebuildable classes:

```text
.venv/
__pycache__/
*.py[cod]
.pytest_cache/
.coverage
htmlcov/
build/
dist/
*.egg-info/
.vscode/
.idea/
```

Disposition:

- virtual environments, caches, coverage, build output, generated egg metadata, and editor state are excluded;
- platform reconstruction must use committed metadata rather than source-machine `.venv` bytes;
- the tracked `src/kaizen_platform.egg-info/` directory is currently part of repository history and therefore remains included until a separately reviewed cleanup decision changes it.

## 3. Staging and immutable governed-operation evidence

Root:

```text
C:\dev\kaizen\staging
```

Current result:

```text
full bounded inventory: not completed
reason: Go8 intentionally exposes no staging root
```

This is an exact tooling gap, not evidence that staging is empty or safely rebuildable.

What is already proven from Milestone 6 operations:

- staging contains immutable governed amendment preparations and operation evidence;
- Packet 010F used two accepted operation IDs:
  - `kz-prom-01KTV6N2A4B6C8D0E2F4G6H8J0`;
  - `kz-prom-01KTV6R4B6D8F0H2J4M6N8P0R2`;
- evidence classes include plans, approvals, validation results, reviewed diffs, preserved bytes, and results;
- this evidence is required for audit and recovery and cannot be declared rebuildable.

Required Packet 011B prerequisite:

> Add or approve one narrow read-only staging inventory route that returns paths, sizes, hashes, and evidence classes without exposing arbitrary filesystem execution or mutation.

Until that route exists and the inventory passes review:

```text
Packet 011B backup creation must not begin.
```

This prerequisite is inventory tooling only. It must not expand into the Milestone 8 MCP reliability scope or generalized filesystem access.

## 4. Docs recovery

Root:

```text
C:\dev\kaizen-docs
```

Recovery posture:

```text
Git repository: yes
branch: main
upstream: origin/main
remote: existing authorized GitHub origin
```

Required verification anchors:

```text
accepted Roadmap v0.3:
5feec5bef8bf48c30be36fddfe1547f47810145407d1e9cc47301e0c6dddc248

Result 102:
03-research-results/102-revised-roadmap-v0.3-and-controlled-pilot-owner-acceptance.md

Milestone 7 specification:
a5d9cac899fd82888201113bbc2e340030be10c6184cfa036cffcd40ff4294eb
```

Docs recovery should be proven through a fresh clone from the existing upstream. The Milestone 7 encrypted recovery set need not duplicate the complete docs Git object database unless Packet 011B finds a concrete reason.

Credentials required to access a private or authenticated remote remain outside the project archive and require a separate recovery checklist.

## Secrets and privacy classification

### Filename-only inspection

The bounded vault and platform manifests contain no tracked filenames matching obvious credential classes such as:

```text
.env
*.pem
*.key
id_rsa
id_ed25519
credentials
cookies
session
```

### Content-pattern limitation

The current generic text-search tool returns matching lines and could expose secret values. Packet 011A therefore did not run broad content searches for `token`, `password`, `secret`, or similar strings across protected roots.

Required Packet 011B prerequisite:

> Use or add a narrow redaction-safe secret classifier that returns only path, rule ID, line number, and classification—not matched values.

This is a safety improvement, not authorization to build a secrets manager.

### Current treatment

- general archive excludes plaintext secrets by rule;
- committed configuration templates and public metadata remain included;
- authentication material for cloud storage, GitHub, ngrok, password managers, or removable-device encryption remains outside the project archive;
- a recovery checklist may name the required account or credential class but may not contain the secret.

## Backup posture comparison

## Option A - Encrypted cloud object only

Advantages:

- automatically off-machine after successful upload;
- accessible from a replacement machine;
- low physical-handling burden;
- provider versioning may supplement Kaizen generations.

Risks:

- account lockout or provider outage can block recovery;
- deletion or synchronization mistakes can remove generations;
- key and cloud credentials must remain independently recoverable;
- owner may mistakenly treat cloud synchronization as backup.

Verdict:

```text
acceptable but not preferred as the only copy
```

## Option B - Encrypted removable media only

Advantages:

- independent of cloud account access;
- strong separation from online compromise when disconnected;
- simple ownership and visibility boundary;
- fast local restore.

Risks:

- media loss, theft, damage, or forgotten updates;
- easy to leave near the source machine, defeating off-site protection;
- manual generation discipline required;
- one physical copy is a single failure point.

Verdict:

```text
acceptable but not preferred as the only copy
```

## Option C - Encrypted cloud plus removable media

Advantages:

- two independent off-machine recovery routes;
- cloud protects against local physical loss;
- removable media protects against provider/account lockout;
- the same verified encrypted object and manifest can be copied to both;
- no platform or vault Git remote is required.

Risks:

- two destinations must be verified;
- retention and generation state must stay synchronized;
- removable media still requires off-site storage discipline;
- slightly more operator work.

Verdict:

```text
recommended
```

## Option D - Platform/vault Git remotes

Advantages:

- familiar Git recovery;
- efficient source and history transfer;
- easy commit verification.

Limitations:

- does not naturally protect staging evidence;
- does not solve secret recovery;
- changes visibility and push authority;
- risks treating Git hosting as the complete backup system;
- was not authorized by Milestone 7 acceptance.

Verdict:

```text
not selected for Milestone 7
may be reconsidered separately later
```

## Encryption option comparison

## 1. `age` recipient encryption

Properties relevant to Kaizen:

- simple file-encryption format and CLI;
- supports explicit public recipients and separate identity files;
- supports multiple recipients;
- Windows installation and prebuilt binaries are available;
- can encrypt a prebuilt archive before cloud or media transfer;
- encrypted identity files may themselves be passphrase protected.

Primary risk:

- loss of every usable identity means permanent loss of backup access;
- identity custody must be designed before encryption.

Verdict:

```text
recommended encryption format
```

## 2. `age` passphrase-only encryption

Advantages:

- no separate identity file;
- simple replacement-machine recovery when the passphrase is available.

Risks:

- security and recoverability depend entirely on one passphrase;
- entering passphrases through automation can leak through environment, files, history, or process handling if designed badly;
- no recipient rotation without re-encrypting the object.

Verdict:

```text
acceptable fallback; not preferred
```

## 3. 7z AES-256 archive encryption

Advantages:

- familiar Windows tooling;
- compression and encryption in one format;
- 7z supports AES-256 and compressed archive headers.

Risks:

- password handling and command-line leakage require care;
- archive and encryption concerns are coupled;
- future tooling must explicitly enable filename/header protection and verify exact options;
- recovery contract becomes more dependent on 7-Zip-specific behavior.

Verdict:

```text
viable alternative; not preferred over age for the first contract
```

## 4. restic repository

Advantages:

- encryption is built in;
- snapshots, deduplication, retention, and checking are first-class;
- supports local and remote repositories;
- multiple repository keys are supported.

Risks:

- introduces a backup repository format and lifecycle rather than one independently hashable recovery object;
- increases Packet 011B/011C implementation and restore complexity;
- retention and repository maintenance become tool-specific;
- unnecessary for the current small Kaizen corpus unless repeated generations prove manual full archives burdensome.

Verdict:

```text
strong later candidate; too much machinery for the first baseline
```

## Recommended posture

```text
recovery object:
one full portable archive containing vault, platform, staging, and the internal full manifest

encryption:
age recipient encryption

primary copy:
owner-selected private cloud storage

secondary copy:
encrypted removable media stored separately from the laptop

public companion:
minimal non-sensitive verification manifest containing backup ID, encrypted-object SHA-256, size, format version, and storage copy status

full manifest:
inside encrypted object

platform remote:
remain none

vault remote:
remain none
```

## Recommended key custody

```text
primary identity:
age identity file

identity protection:
passphrase-encrypted identity file

primary custody:
owner password manager stores the identity-file passphrase and the public recipient

secondary recovery:
one printed or offline recovery record stored separately from both laptop and removable backup media

prohibited:
identity stored only on the source laptop
identity stored unencrypted beside both backup copies
passphrase embedded in scripts, manifests, shell history, or repository files
```

The exact password manager and physical storage location remain owner choices.

## Recommended retention and cadence

Initial Milestone 7 proof:

```text
generation 1: full baseline; retained
generation 2: later full baseline or accepted successor; retained until independently restored
minimum retained verified generations: 2
```

After closure:

```text
manual backup after a milestone closure or consequential canonical/platform change
maximum target age: 30 days
no automated deletion
owner-only deletion after successor restore proof
```

A later milestone may justify incremental tooling or automation after actual backup workload exists.

## Owner decision package

| Decision | Recommended selection | Owner must confirm |
|---|---|---|
| Primary storage | Private owner-controlled cloud object | Exact provider/account location |
| Secondary storage | Removable media stored separately | Exact device and off-site location |
| Visibility | Encrypted object only; no public access | Yes |
| Archive format | Portable full archive; exact format in Packet 011B | Accept or change |
| Encryption | `age` recipient encryption | Accept or change |
| Identity custody | Passphrase-encrypted identity file | Accept or change |
| Passphrase custody | Existing password manager | Exact password manager |
| Offline recovery | Printed/offline recovery record separate from backups | Exact location |
| Retention | Two verified generations minimum | Accept or change |
| Cadence | Milestone/consequential-change trigger; max 30 days | Accept or change |
| Staging exclusions | None until narrow inventory is complete | Accept |
| Plaintext secrets | Exclude; recovery checklist only | Accept |
| Platform remote | Remain none | Accept |
| Vault remote | Remain none | Accept |
| Deletion authority | Owner only | Accept |
| Restore root | Disposable non-live path under `C:\dev\kaizen-restore-proof\<backup-id>` | Accept or change |

## Required follow-up before Packet 011B

1. owner selects exact cloud destination;
2. owner selects exact removable medium and storage location;
3. owner confirms `age` or chooses another encryption option;
4. owner selects password manager and offline recovery location;
5. add or approve narrow read-only staging inventory capability;
6. add or approve redaction-safe secret classification capability;
7. freeze manifest/restore/retention contracts at exact hashes;
8. audit and approve Packet 011B separately.

## Sources used for tool comparison

- age official repository and usage documentation: `https://github.com/FiloSottile/age`
- 7-Zip official 7z format documentation: `https://7-zip.org/7z.html`
- restic official encryption documentation: `https://restic.readthedocs.io/en/stable/070_encryption.html`

These sources inform the recommendation but do not authorize installation or execution.
