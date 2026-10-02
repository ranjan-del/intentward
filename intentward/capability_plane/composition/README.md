# Tool composition

`intentward/capability_plane/composition/` · Capability plane · Phase P6

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Builds multi-step capability chains using preconditions and postconditions (flight.search, flight.details, calendar.create).

| | |
|---|---|
| Owns | Chain construction, validity checks, minimum-authority plan selection. |
| Does not own | Execution and authorization. |
| Research questions | RQ12, RQ13 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I10 (see [security model](/docs/security-model.md)) |
| Main risk | Composed chains whose combined authority exceeds what any single step looked like. |
