# Verification

`intentward/verification/` · Trust plane · Phase P11

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Never trust the model's claim that it did something. Check the actual state.

## Modules

| Module | Purpose |
|---|---|
| [`action_verifier/`](action_verifier/README.md) | Checks each action against its declared postconditions (file deleted, event created, transaction committed). |
| [`state_verifier/`](state_verifier/README.md) | Checks that the resulting system state matches the intended result for the task as a whole. |
| [`transactions/`](transactions/README.md) | Plan, (simulate,) authorize, execute, verify, commit. |
| [`intent_drift/`](intent_drift/README.md) | Semantic trajectory monitoring: does this action advance the goal, is it necessary, unusually risky, reversible, a new objective, unrelated resources? A detector only, never the enforcement layer. |
