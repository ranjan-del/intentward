# Manifests

`intentward/capability_plane/manifest/` · Capability plane · Phase P4

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The security-sensitive manifest each tool or provider publishes: id, description, inputs, outputs, preconditions, postconditions, required authorization, resource scope, side effects, reversibility, risk, network, authentication, data access and sensitivity, provider, protocol, version, availability, cost, latency.

| | |
|---|---|
| Owns | Manifest schema, validation, signing (if justified), diffing across versions. |
| Does not own | Trusting what a manifest claims. A manifest describes; the policy authorizes. |
| Research questions | RQ16 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I7, I8 (see [security model](/docs/security-model.md)) |
| Main risk | Poisoned manifests, deceptive descriptions, schema manipulation, version confusion. |
