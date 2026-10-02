# Policy engine

`intentward/security/policy/` · Control plane · Phase P1

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Deterministic policy evaluation over capability, resource, scope, rate and expiry. Start with plain typed rules; adopt Cedar or OPA only if an ADR shows the need.

| | |
|---|---|
| Owns | Rule evaluation, policy versions, policy diffs. |
| Does not own | Semantic judgement (verification/intent_drift). |
| Research questions | RQ1 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1, I3 (see [security model](/docs/security-model.md)) |
| Main risk | Policy bugs; policy manipulation through untrusted inputs. |
