---
id: kz-result-01KV68F7N3P2Q9W6T4M8Y1C5RA
type: research-result
status: complete
project: kaizen-platform
created: 2026-06-15T14:35:00-04:00
updated: 2026-06-15T14:35:00-04:00
review_status: owner-accepted
authority: accepted
summary: "Authorize the minimal Go8 correction needed to complete Northstar R1 regeneration and tests."
---

# Research Result 227 - R1 Northstar Bounded Execution-Surface Correction

## Context

Fresh-context R1 correctly reconciled canonical truth and independently recovered the omitted committed-event ID, but could not regenerate outputs or run tests because Go8 registered the Northstar root without a fixed Python interpreter or Northstar execution profiles.

The owner directed the steward to continue efficiently and avoid unnecessary stop gates. This record authorizes only the smallest bounded Go8 correction needed to complete R1.

## Authorized Go8 changes

```text
repository:
C:\dev\chatgpt-mcp

allowed source paths:
src/chatgpt_mcp/extra_tools.py
pyproject.toml
tests/test_extra_tools.py
tests/test_server.py
uv.lock only if the version update deterministically changes it
```

Permitted behavior:

1. map the `northstar` root to the already-fixed Kaizen platform Python 3.11 virtual-environment interpreter;
2. set `PYTHONPATH` to `C:\dev\kaizen-pilot-northstar\src` only when executing under the `northstar` root;
3. allow module `northstar_stockroom` only when `root=northstar`;
4. add one fixed `northstar_full` pytest profile;
5. preserve argument validation, fixed roots, no shell, no generic execution, no network, no remote creation, and no arbitrary filesystem access;
6. bump Go8 patch version and update exact version assertions;
7. add tests proving the Northstar module is rejected on every other root and the profile remains fixed.

## Explicitly forbidden

- generic Python or shell execution;
- user-selected executables;
- arbitrary environment variables or `PYTHONPATH` values;
- package installation into Northstar;
- creation of a Northstar virtual environment;
- edits to Northstar source, fixtures, golden files, or accepted tests;
- access to owner-private oracle material;
- remote creation or push from Northstar;
- Phase 5 work.

## Completion gate

After Go8 tests, compile, Ruff, diff, integrity, exact-scope verification, local commit, server restart, and live health verification, run only:

```text
northstar_stockroom baseline regeneration into r1-output
northstar_full pytest profile
```

Freeze output hashes and test evidence, then stop before oracle comparison or Phase 5.
