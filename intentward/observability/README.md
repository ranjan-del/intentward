# Observability

`intentward/observability/` · Trust plane · Phase P2, P3, P12

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Audit, tracing and events that make every security-relevant decision visible and replayable.

## Modules

| Module | Purpose |
|---|---|
| [`audit/`](audit/README.md) | Append-only, hash-chained log of every transition: timestamp, agent, task, old set, requested capability, reason, resources, risk, user identity, decision, new version, expiry. |
| [`tracing/`](tracing/README.md) | Run traces for experiments and debugging. |
| [`events/`](events/README.md) | The observability view of the event bus: persisted security and runtime events for analysis. |
| [`dashboard/`](dashboard/README.md) | Shows agent, task, state, capability version, tools, resources, risk, capability changes (old, request, decision, new), tool calls, blocked actions, policy decisions, context provenance, sandbox status, attacks and drift alerts. |
