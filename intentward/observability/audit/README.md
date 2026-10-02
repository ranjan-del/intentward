# Audit log

`intentward/observability/audit/` · Trust plane · Phase P2

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Append-only, hash-chained log of every transition: timestamp, agent, task, old set, requested capability, reason, resources, risk, user identity, decision, new version, expiry.

| | |
|---|---|
| Owns | Tamper evidence, replay to reconstruct every capability version. |
| Does not own | Tamper proofing against a compromised host (stated as a limitation). |
| Research questions | RQ3 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I2 (see [security model](/docs/security-model.md)) |
| Main risk | Gaps in audit completeness. |
