# MCP adapter

`intentward/interop/mcp/` · Capability plane · Phase P4

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Exposes MCP tools through the capability plane and gateway. MCP is a transport, not the capability system.

| | |
|---|---|
| Owns | MCP client and server adapters. |
| Does not own | Trusting MCP tool descriptions. |
| Research questions | RQ16, RQ29 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I7 (see [security model](/docs/security-model.md)) |
| Main risk | Tool poisoning via MCP descriptions. |
