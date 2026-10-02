# State verifier

`intentward/verification/state_verifier/` · Trust plane · Phase P11

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Checks that the resulting system state matches the intended result for the task as a whole.

| | |
|---|---|
| Owns | Task-level state checks for files, Git, databases, APIs, deployments. |
| Does not own | Per-action checks (action_verifier). |
| Research questions | RQ25 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I10 (see [security model](/docs/security-model.md)) |
| Main risk | Partial verification read as full success. |
