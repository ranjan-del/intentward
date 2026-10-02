# Process limits

`intentward/isolation/process/` · Execution plane · Phase P8

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

CPU, memory, disk and process-count limits, time limits.

| | |
|---|---|
| Owns | Resource limits bound to the task. |
| Does not own | Scheduling decisions (core/scheduler). |
| Research questions | RQ17 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I4 (see [security model](/docs/security-model.md)) |
| Main risk | Resource exhaustion and agent loops. |
