# Scheduler

`intentward/core/scheduler/` · Agent plane · Phase P9

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Wake, execute, sleep. Persistent agents must not run the model continuously; they wake on time or on events.

| | |
|---|---|
| Owns | Timers, delayed tasks, retries with exponential backoff, deadlines, wake conditions, leases on work items. |
| Does not own | Deciding whether the woken agent still has authority (security/, enforced by I6). |
| Research questions | RQ18, RQ19, RQ20, RQ22 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I6 (see [security model](/docs/security-model.md)) |
| Main risk | Wake triggers are an attack entry point; a schedule must never extend a lease. |
