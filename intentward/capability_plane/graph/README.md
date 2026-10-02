# Capability graph

`intentward/capability_plane/graph/` · Capability plane · Phase P6

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Relationships between capabilities: requires, produces, consumes, depends_on, enables, conflicts_with, requires_approval, alternative_to.

| | |
|---|---|
| Owns | Graph model and queries used for composition, gap analysis and blast-radius estimation. |
| Does not own | Authority propagation decisions (security/). |
| Research questions | RQ12, RQ31 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I9 (see [security model](/docs/security-model.md)) |
| Main risk | A wrong edge yields a plan that looks valid but is not. |
