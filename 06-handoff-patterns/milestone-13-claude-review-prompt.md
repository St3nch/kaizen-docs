# Milestone 13 Claude Review Prompt

Use this prompt when asking Claude to review the Milestone 13 planning portion.

```markdown
You are reviewing Kaizen Milestone 13 planning as an independent security/stewardship auditor.

Do not treat this as implementation approval. Do not propose Qdrant, Hermes, Tauri, Observatory, Neon Ronin, SearchClarity, provider/customer-data work, production deployment, or broad architecture expansion unless identifying scope creep.

Read these files in order:

1. `00-entrypoint/LLM_START_HERE.md`
2. `ROADMAP_V0.4.md`
3. `03-research-results/326-sp-1-owner-acceptance.md`
4. `05-specs/milestone-13-operational-service-and-safe-local-tool-surface-hardening.md`
5. `03-research-results/327-milestone-13-definition-readiness-audit.md`
6. `03-research-results/328-milestone-13-definition-owner-acceptance.md`
7. `03-research-results/329-packet-019a-m13-inventory-and-readiness-audit.md`
8. `04-design-decisions/0019-milestone-10-system-of-record-reconciliation.md`
9. `04-design-decisions/0020-milestone-11-operational-foundation-boundaries.md`

Then inspect platform files as needed:

- `src/kaizen/ops_service/contracts.py`
- `src/kaizen/ops_service/service.py`
- `src/kaizen/ops_service/repository.py`
- `src/kaizen/operator_console.py`
- `pyproject.toml`

Review questions:

1. Does the M13 definition correctly preserve the accepted Kaizen boundaries?
2. Does Packet 019A accurately inventory the operational service and safe local tool surface?
3. Are the risks complete enough for the next M13 planning step?
4. Is the recommended 019B / 019C / 019D packet sequence sound?
5. What must be fixed before any M13 implementation packet is approved?
6. Is there any hidden scope creep into M14, Qdrant, Hermes, Tauri, Observatory, Neon Ronin, SearchClarity, provider/customer-data, or production work?
7. Are the non-authorization boundaries strong enough?

Produce a concise audit with:

- Verdict: PASS / PASS WITH REQUIRED FIXES / FAIL
- Required fixes
- Should-fix items
- Non-blocking observations
- Suggested next packet boundary for 019B

Do not rewrite the project. Audit the current M13 planning package.
```
