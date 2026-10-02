# Intent drift detector

`intentward/verification/intent_drift/` · Trust plane · Phase P11

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Semantic trajectory monitoring: does this action advance the goal, is it necessary, unusually risky, reversible, a new objective, unrelated resources? A detector only, never the enforcement layer.

| | |
|---|---|
| Owns | Drift signals and alerts that can trigger pause. |
| Does not own | Being the security boundary. |
| Research questions | RQ24 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1 (see [security model](/docs/security-model.md)) |
| Main risk | LLM-judge bypass; false positives. |
