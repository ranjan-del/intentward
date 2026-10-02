# Event bus

`intentward/core/event_bus/` · Agent plane · Phase P1 (in-process), P9 (durable, external sources)

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Internal publish/subscribe for runtime, security and audit events: email.received, calendar.changed, price.changed, webhook.received, timer.expired, user.message, tool.completed, capability.changed, approval.received, capability.revoked.

| | |
|---|---|
| Owns | Event schemas, delivery, provenance on every event. |
| Does not own | Treating any event as authority. An event is data. |
| Research questions | RQ22 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1, I5 (see [security model](/docs/security-model.md)) |
| Main risk | Forged or spoofed events; event floods (resource exhaustion). |
