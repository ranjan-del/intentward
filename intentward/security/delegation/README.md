# Delegation

`intentward/security/delegation/` · Control plane · Phase P10

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Sub-agents receive attenuated capabilities, never inherited by default. Caveat-style tokens (macaroon, Biscuit or UCAN style) are the candidate mechanism.

| | |
|---|---|
| Owns | Delegation chains, attenuation, cross-agent data flow rules. |
| Does not own | Agent orchestration (core/). |
| Research questions | RQ23 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I9 (see [security model](/docs/security-model.md)) |
| Main risk | A child agent assembling more authority than its parent through several delegations. |
