# Context firewall

`intentward/security/context_firewall/` · Trust plane · Phase P7

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Provenance and trust for every context object, enforced where data leaves the model: values from untrusted sources cannot become recipients, URLs, paths or other authority-bearing arguments without approval.

| | |
|---|---|
| Owns | Labels, taint tracking on tool arguments, label propagation rules. |
| Does not own | Stopping the model from reading untrusted text (impossible); it controls what flows into actions. |
| Research questions | RQ6, RQ7 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I5 (see [security model](/docs/security-model.md)) |
| Main risk | Label laundering; over-blocking legitimate data use. |
