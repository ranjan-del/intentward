# Transactions

`intentward/verification/transactions/` · Trust plane · Phase P11

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Plan, (simulate,) authorize, execute, verify, commit. Checkpoints, snapshots, compensating actions, rollback, idempotency, action journals.

| | |
|---|---|
| Owns | Journals and rollback for reversible actions; compensations for external ones. |
| Does not own | Pretending external side effects are reversible when they are not. |
| Research questions | RQ26 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I10 (see [security model](/docs/security-model.md)) |
| Main risk | Partial execution leaving inconsistent state. |
