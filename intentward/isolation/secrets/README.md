# Secrets broker

`intentward/isolation/secrets/` · Execution plane · Phase P8

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The model never sees secrets. The gateway obtains the minimum short-lived credential for an authorized call (cloud.storage.write on bucket project-output, not the whole cloud key).

| | |
|---|---|
| Owns | Secret-scoped capabilities, short-lived tokens, redaction, rotation. |
| Does not own | Storing long-lived secrets in the agent environment. |
| Research questions | RQ21 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I4 (see [security model](/docs/security-model.md)) |
| Main risk | Credential leakage into context or logs. |
