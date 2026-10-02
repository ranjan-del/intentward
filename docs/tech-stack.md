# Technology direction

> Each technology must solve a concrete architectural requirement. Nothing is added because it is
> popular. Adoption of anything beyond the "now" column needs an ADR.

| Area | Now | Considered later, only with a measured reason |
|---|---|---|
| Language | Python (runtime, experiments, LLM integration, benchmark, research tooling) | Go for the gateway or distributed services if Python is a measured bottleneck; TypeScript for SDK or dashboard |
| Typing and schemas | Pydantic models, strict typing (mypy or pyright) for every security object | JSON Schema export for manifests |
| Tests | pytest, Hypothesis (property-based), scripted adversarial model | Formal models of the transition protocol (TLA+ or Alloy) |
| Storage | SQLite for durable state and audit | PostgreSQL for multi-worker runs |
| Policy | Plain typed rules in Python | Cedar (analysable) or OPA/Rego if rule volume or analysis needs it |
| Isolation | Docker for development; bubblewrap or nsjail profiles | gVisor, Firecracker, Landlock, seccomp |
| Network | Local egress proxy with allowlist | DNS filtering, network identity |
| Interop | MCP and HTTP adapters, AgentDojo adapter | gRPC, event-driven tools |
| Observability | Structured JSON logs and run traces | OpenTelemetry, event streams |
| Models | Claude first; open-weight models for comparison; scripted model offline | Others as experiments require |

## Linux topics to study deeply

Namespaces, cgroups, seccomp, Linux capabilities, process isolation, filesystem permissions,
networking. The owner's primary machine is macOS on Apple silicon, so OS-plane experiments run in a
Linux VM or container, and every result records its platform.
