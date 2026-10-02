# Authorization and transitions

`intentward/security/authorization/` · Control plane · Phase P2

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The capability transition protocol: CURRENT, CHANGE REQUEST, POLICY EVALUATION, USER NOTIFICATION, APPROVE or DENY, NEW VERSION, ACTIVE. Pause or deny until a decision exists.

| | |
|---|---|
| Owns | Requests, approvals, denials, notifications showing old set, requested change, reason, resources, risk, agent state, proposed set, expiry, scope, and the causal provenance of the request. |
| Does not own | Writing the audit log (observability/audit), but it must call it on every transition. |
| Research questions | RQ3, RQ5, RQ19 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1, I2 (see [security model](/docs/security-model.md)) |
| Main risk | Approval fatigue; social engineering through the request reason text. |
