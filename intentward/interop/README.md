# Interop adapters

`intentward/interop/` · Capability plane · Phase P3 (AgentDojo), P4 (MCP, HTTP), P12 (second runtime)

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Adapters that let the same capability model run behind different protocols and agent environments: MCP, HTTP/REST, gRPC, local functions, AgentDojo, and other agent runtimes.

| | |
|---|---|
| Owns | Protocol adapters and environment adapters. |
| Does not own | The capability model itself (protocol independent). |
| Research questions | RQ15, RQ28, RQ29 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I7 (see [security model](/docs/security-model.md)) |
