# Capability plane

`intentward/capability_plane/` · Capability plane · Phase P4, P6

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

What capabilities exist, what each can and cannot do, how they compose, and what is missing for a goal. Independent of protocol: MCP is one transport, not the capability system.

## Modules

| Module | Purpose |
|---|---|
| [`registry/`](registry/README.md) | The catalogue of abstract capabilities (calendar.create) and the concrete tools that implement them. |
| [`manifest/`](manifest/README.md) | The security-sensitive manifest each tool or provider publishes: id, description, inputs, outputs, preconditions, postconditions, required authorization, resource scope, side effects, reversibility, risk, network, authentication, data access and sensitivity, provider, protocol, version, availability, cost, latency. |
| [`discovery/`](discovery/README.md) | "RAG for tools": given a goal, retrieve a small relevant set of capabilities instead of exposing hundreds of tools to the model. |
| [`graph/`](graph/README.md) | Relationships between capabilities: requires, produces, consumes, depends_on, enables, conflicts_with, requires_approval, alternative_to. |
| [`composition/`](composition/README.md) | Builds multi-step capability chains using preconditions and postconditions (flight.search, flight.details, calendar.create). |
| [`gap_analysis/`](gap_analysis/README.md) | For a goal, computes required, available, authorized, missing, blocked and alternative capabilities and execution paths, so the agent can say "I cannot complete this because restaurant.booking is unavailable" instead of hallucinating success. |
| [`providers/`](providers/README.md) | Resolves an abstract capability (calendar.create) to a concrete provider (Google Calendar, Outlook, internal) using availability, user authorization, cost, latency, reliability, required data, risk and task constraints. |
