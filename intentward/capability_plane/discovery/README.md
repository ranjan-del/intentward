# Capability discovery and retrieval

`intentward/capability_plane/discovery/` · Capability plane · Phase P6

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

"RAG for tools": given a goal, retrieve a small relevant set of capabilities instead of exposing hundreds of tools to the model.

| | |
|---|---|
| Owns | Multi-signal ranking: semantic similarity, input and output compatibility, authorization, risk, side effects, cost, latency, availability, provider, user preferences, task constraints. Hybrid semantic, structured, graph and symbolic retrieval. |
| Does not own | Deciding that a retrieved capability is allowed. |
| Research questions | RQ10, RQ11 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I7 (see [security model](/docs/security-model.md)) |
| Main risk | Retrieval poisoning through tool descriptions; authorization-blind ranking. |
