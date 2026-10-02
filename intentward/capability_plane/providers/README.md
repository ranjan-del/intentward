# Provider resolution

`intentward/capability_plane/providers/` · Capability plane · Phase P4

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Resolves an abstract capability (calendar.create) to a concrete provider (Google Calendar, Outlook, internal) using availability, user authorization, cost, latency, reliability, required data, risk and task constraints.

| | |
|---|---|
| Owns | Selection logic and portability across providers. |
| Does not own | Skipping authorization: the chosen provider still passes the policy system. |
| Research questions | RQ15 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I7 (see [security model](/docs/security-model.md)) |
| Main risk | Provider substitution attacks. |
