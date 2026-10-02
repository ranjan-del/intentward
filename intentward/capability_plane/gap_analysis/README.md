# Capability gap analyzer

`intentward/capability_plane/gap_analysis/` · Capability plane · Phase P6

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

For a goal, computes required, available, authorized, missing, blocked and alternative capabilities and execution paths, so the agent can say "I cannot complete this because restaurant.booking is unavailable" instead of hallucinating success.

| | |
|---|---|
| Owns | Gap computation and capability-aware failure messages with legitimate alternatives. |
| Does not own | Requesting new authority on its own; it may only propose a capability request. |
| Research questions | RQ14 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I10 (see [security model](/docs/security-model.md)) |
| Main risk | Treating a gap as a reason to escalate. |
