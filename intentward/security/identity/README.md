# Identity

`intentward/security/identity/` · Control plane · Phase P2, P10

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Identities for users, agents, sub-agents, tools and providers, and the binding of capabilities to task and session.

| | |
|---|---|
| Owns | Identity model, binding, approver identity in audit records. |
| Does not own | Authentication against external providers (isolation/secrets). |
| Research questions | RQ23 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I9 (see [security model](/docs/security-model.md)) |
| Main risk | Identity confusion between parent and child agents. |
