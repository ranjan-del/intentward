# Persistent secure environment

`intentward/isolation/environment/` · Execution plane · Phase P8, P9

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

A per-agent environment (filesystem, browser, network, processes, credentials, tools) that survives restarts, as a production personal agent would have.

| | |
|---|---|
| Owns | Environment abstraction, lifecycle, snapshot and reset. |
| Does not own | The agent lifecycle (core/agent_runtime). |
| Research questions | RQ18 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I6 (see [security model](/docs/security-model.md)) |
| Main risk | State that leaks between tasks; poisoned leftovers. |
