# Intent engine

`intentward/intelligence/intent/` · Control plane · Phase P5

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Compiles a human goal into a structured task: objective, resources, required, optional and prohibited actions, risk, expected outputs, duration, constraints, and the abstract capabilities required.

| | |
|---|---|
| Owns | LLM-assisted interpretation plus a deterministic validator; the output is a proposed capability plan, not a grant. |
| Does not own | Granting anything. The policy engine validates the plan. |
| Research questions | RQ1, RQ2 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1 (see [security model](/docs/security-model.md)) |
| Main risk | Over-provisioning (no security) or under-provisioning (approval fatigue); injected content inside the user prompt. |
