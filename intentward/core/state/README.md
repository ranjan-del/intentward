# Task state

`intentward/core/state/` · Agent plane · Phase P1 (in-memory), P2 (SQLite, durable), P9 (restart recovery)

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The durable task state machine: CREATED, PLANNING, RUNNING, WAITING, WAITING_FOR_CAPABILITY, CAPABILITY_REQUESTED, WAITING_FOR_USER, PAUSED, RESUMED, VERIFYING, COMPLETED, FAILED, REVOKED, TERMINATED.

| | |
|---|---|
| Owns | Legal transitions, persistence of task, objective, constraints, capability version, plan, actions, observations, pending approvals, next wake condition. |
| Does not own | Capability versions themselves (security/capability). |
| Research questions | RQ18 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I2, I6 (see [security model](/docs/security-model.md)) |
| Main risk | Illegal transitions (for example SLEEPING to RUNNING without re-validation); duplicate actions after restart. |
