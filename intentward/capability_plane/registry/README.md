# Capability registry

`intentward/capability_plane/registry/` · Capability plane · Phase P4

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The catalogue of abstract capabilities (calendar.create) and the concrete tools that implement them.

| | |
|---|---|
| Owns | Registration, lookup, availability, versions of capability and provider. |
| Does not own | Authorization. Being in the registry grants nothing. |
| Research questions | RQ9, RQ15 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I7 (see [security model](/docs/security-model.md)) |
| Main risk | Tool shadowing and capability impersonation via registration. |
