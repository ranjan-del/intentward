# ADR 0003: The capability plane is protocol independent and separate from authority

Status: accepted (2026-10-02)

## Context

Tool ecosystems are converging on MCP, and it is tempting to build an MCP framework. The word capability is also used both for what a tool can do and what an agent may do.

## Decision

The capability plane models abstract capabilities and manifests independently of transport; MCP, REST, gRPC, local functions and others are adapters in interop/. Capability descriptions (capability_plane/) and capability grants (security/capability) are separate types. A description never becomes a grant (invariant I7).

## Consequences

Portability across providers and protocols becomes testable (RQ15, RQ29). Some duplication between manifest fields and grant fields is accepted.
