# Tool gateway

`intentward/security/tool_gateway/` · Execution plane · Phase P1

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The reference monitor. Every tool call passes through identity, capability check, argument validation, resource validation, rate limiting, policy evaluation, network policy and audit.

| | |
|---|---|
| Owns | Mediation of all tool calls for MCP, HTTP, filesystem, shell, databases, browser, Git providers, cloud APIs and internal services. |
| Does not own | OS-level enforcement for shell and code execution (isolation/). |
| Research questions | RQ1, RQ27 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I1, I3, I4 (see [security model](/docs/security-model.md)) |
| Main risk | Bypass through shell or code execution; TOCTOU between check and use. |
