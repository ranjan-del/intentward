# Network

`intentward/isolation/network/` · Execution plane · Phase P8

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Egress control: deny by default, domain allowlists, DNS filtering, proxy-based control, IP restrictions, request budgets, network identity, TLS interception only in controlled environments.

| | |
|---|---|
| Owns | Egress proxy and its policy binding to the capability set. |
| Does not own | Application-level decisions about what data may be sent (security/context_firewall). |
| Research questions | RQ27, RQ30 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I4 (see [security model](/docs/security-model.md)) |
| Main risk | Exfiltration through allowed domains. |
