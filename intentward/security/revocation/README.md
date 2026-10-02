# Revocation

`intentward/security/revocation/` · Control plane · Phase P9

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Live revocation: invalidate the capability, reject future calls, stop or pause running operations, revoke temporary credentials and network access, invalidate tokens, update state, record, notify, and make sure revoked authority cannot reappear.

| | |
|---|---|
| Owns | Propagation into running sessions, proxies and short-lived credentials; revocation latency measurement. |
| Does not own | Credentials that cannot be revoked before expiry; those bound revocation latency by their TTL, and the docs say so. |
| Research questions | RQ21 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I4 (see [security model](/docs/security-model.md)) |
| Main risk | Stale caches, already-running tool sessions, delegated credentials. |
