# Context assembly

`intentward/intelligence/context/` · Agent plane · Phase P7

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Builds the model context from labelled context objects (system policy, user, app state, memory, RAG, web, email, tool output, sub-agents).

| | |
|---|---|
| Owns | Assembly order, budgets, labelling of every object. |
| Does not own | Enforcing labels (security/context_firewall does that at tool arguments). |
| Research questions | RQ6 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I5 (see [security model](/docs/security-model.md)) |
| Main risk | Label laundering through summarization. |
