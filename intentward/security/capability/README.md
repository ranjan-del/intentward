# Capability (authority) objects

`intentward/security/capability/` · Control plane · Phase P1, P2, P9, P10

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Authority grants. Not to be confused with capability_plane/, which describes what tools exist. A grant has action, resource, scope, arguments, identity, purpose, time, expiry, rate limit, network limits, sensitivity, tenant, task id, version, parent, approval state.

| | |
|---|---|
| Owns | Immutable versioned capability sets, leases (TTL, max calls), attenuation, hashing. |
| Does not own | Deciding transitions (security/authorization). |
| Research questions | RQ3, RQ4, RQ20, RQ23 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1, I2, I3, I9 (see [security model](/docs/security-model.md)) |
| Main risk | Mutable shared state; confusing resource scopes (symlinks, globs). |
