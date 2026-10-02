# LLM adapters

`intentward/intelligence/llm/` · Agent plane · Phase P1

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Model adapters behind one interface, plus the scripted adversarial model used to test enforcement deterministically.

| | |
|---|---|
| Owns | Provider adapters (Claude first, open-weight models for comparison), the scripted model, recorded-response replay for reproducible runs. |
| Does not own | Any security decision. |
| Research questions | RQ28 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1 (see [security model](/docs/security-model.md)) |
| Main risk | Non-determinism breaks reproducibility unless runs are seeded, repeated and recorded. |
