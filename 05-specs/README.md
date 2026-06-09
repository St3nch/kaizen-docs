# Specs

This folder holds Kaizen planning and implementation contracts.

## Maturity vocabulary

```text
draft
accepted planning contract
implemented baseline
superseded
```

- `draft` is not binding.
- `accepted planning contract` is binding for planning or implementation but not yet proven by a completed implementation slice.
- `implemented baseline` is accepted and verified through implementation, tests, and steward evidence.
- `superseded` is retained for history but replaced by a later accepted contract.

## Implemented first-slice baselines

- `kaizen-field-registry.md`
- `kaizen-note-type-registry.md`
- `kaizen-id-and-prefix-registry.md`
- `kaizen-validation-gate-spec.md`
- `kaizen-hammer-test-strategy.md`
- `staging-and-promotion-workflow.md`
- `staging-write-wrapper-and-promotion-recovery.md`
- `promotion-event-schema.md`

These baselines are governed by accepted Decisions 0012 and 0013 and the Milestone 4 implementation evidence.

## Draft or deferred future contracts

- `operational-postgres-authority.md`
- `deferred-dataforseo-llm-ranking-capture-packet.md`

Other future database, retrieval, provider, Hermes, interface, and orchestration contracts remain draft or deferred until explicitly accepted.

## Current rule

Do not build from a specification merely because it exists. Before new implementation, confirm accepted governing decisions, exact scope, unresolved dependencies, validation and hammer requirements, required audit, human review, and a governed task packet.
