# Core

`intentward/core/` · Agent plane · Phase P1, P2, P9

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The agent runtime itself: the loop, planning, durable task state, scheduling and the internal event bus.

## Modules

| Module | Purpose |
|---|---|
| [`agent_runtime/`](agent_runtime/README.md) | The persistent runtime that hosts an agent: model, memory, state, tools, scheduler, events, security and environment. |
| [`planner/`](planner/README.md) | Turns a goal plus the authorized, retrieved capabilities into an ordered plan of abstract capability calls. |
| [`state/`](state/README.md) | The durable task state machine: CREATED, PLANNING, RUNNING, WAITING, WAITING_FOR_CAPABILITY, CAPABILITY_REQUESTED, WAITING_FOR_USER, PAUSED, RESUMED, VERIFYING, COMPLETED, FAILED, REVOKED, TERMINATED. |
| [`scheduler/`](scheduler/README.md) | Wake, execute, sleep. |
| [`event_bus/`](event_bus/README.md) | Internal publish/subscribe for runtime, security and audit events: email.received, calendar.changed, price.changed, webhook.received, timer.expired, user.message, tool.completed, capability.changed, approval.received, capability.revoked. |
