# Planner

`intentward/core/planner/` · Agent plane · Phase P1 (simple), P6 (graph-aware composition)

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Turns a goal plus the authorized, retrieved capabilities into an ordered plan of abstract capability calls.

| | |
|---|---|
| Owns | Plan representation (plan is data), re-planning after observations, use of capability pre/postconditions. |
| Does not own | Choosing which capabilities exist (capability_plane/) or are allowed (security/). |
| Research questions | RQ9, RQ12, RQ13 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I10 (see [security model](/docs/security-model.md)) |
| Main risk | The planner is model-driven and therefore untrusted: its plan is a proposal, never an authorization. |
