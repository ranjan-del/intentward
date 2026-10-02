# Agent runtime

`intentward/core/agent_runtime/` · Agent plane · Phase P1 (in-process loop), P9 (persistent, survives restarts)

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The persistent runtime that hosts an agent: model, memory, state, tools, scheduler, events, security and environment. The client app is only an interface; closing it does not stop the agent.

| | |
|---|---|
| Owns | Agent lifecycle (create, execute, pause, resume, terminate); the plan, act, observe loop; crash recovery; resumption from durable state. |
| Does not own | Authorization decisions (security/), tool execution (security/tool_gateway, isolation/). |
| Research questions | RQ18, RQ19, RQ22 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1, I6 (see [security model](/docs/security-model.md)) |
| Main risk | A long-lived process accumulates authority and state; resumption after a crash may replay side effects. |
