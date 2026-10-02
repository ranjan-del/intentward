# Positioning: what IntentWard is and is not

## In one sentence

IntentWard is a research-grade, open-source agent runtime that compiles human intent into
constrained capabilities, discovers and composes the tools that implement them, and continuously
enforces, verifies, monitors, revokes and audits that authority for the whole life of a persistent
agent.

## Layering

```
                    PERSONAL / ENTERPRISE AGENT
                              |
             +----------------+----------------+
             |                                 |
        PRODUCT LAYER                    INFRASTRUCTURE LAYER
             |                                 |
   Muse, Dots, Grok-style agents,         INTENTWARD
   coding agents, internal bots                |
   (built by others, or by us                  +-- Intent engine
    only as test agents)                       +-- Capability plane (discovery, composition, gaps)
                                               +-- Capabilities and policy
                                               +-- Context firewall
                                               +-- Tool gateway
                                               +-- Sandbox and persistent environment
                                               +-- Persistent runtime (state, scheduler, events)
                                               +-- Verification
                                               +-- Revocation
                                               +-- Attack World benchmark
```

## Not building / building

| We are NOT building | We ARE building |
|---|---|
| Another personal assistant (Muse, Dots, Grok-style) | The security, control and runtime primitives such assistants need |
| A chatbot, a basic RAG app, a LangChain, AutoGPT or agent-framework clone | A runtime where authority comes from intent and is enforced outside the model |
| A thin wrapper around an LLM API | A reference monitor, capability model and transition protocol |
| "A security agent between the model and the internet" (Meta's Sentinel for Muse already does this, per public descriptions we still have to verify) | A generalization: formal capabilities, transitions, attenuation, revocation, discovery and a cross-environment benchmark |
| An MCP framework | A protocol-independent capability plane; MCP is one transport |
| A new sandbox, egress proxy, policy engine or benchmark from scratch | Integrations of existing ones (bubblewrap or gVisor, a deterministic policy layer, AgentDojo) where they already work |
| A security dashboard without a security runtime | The runtime first, the dashboard last |
| A new foundation model | A model of authority, a runtime and a benchmark |
| Claims of security, or novelty from combining libraries | Measured results, including negative ones, positioned against prior art |

## Engineering vs research contribution

| Part | Kind | Why |
|---|---|---|
| Sandbox, network egress, secrets broker, process limits | Engineering | Well-solved elsewhere; we integrate and measure |
| Tool gateway, policy evaluation | Engineering, with research measurements | Needed as the reference monitor |
| Versioned capability transitions with provenance-aware requests | **Research** | Little prior work on the request channel as an attack surface or on approval fatigue |
| Leases and revocation in persistent, distributed agents | **Research** | Measurable latency and stale-authority questions, little prior work |
| Attenuated delegation for sub-agents | **Research** | Ambient authority leaks in multi-agent trees |
| Capability discovery, minimum-authority planning and gap analysis under authorization | **Research** | Tool retrieval exists; authorization-aware, least-authority composition is largely open |
| Manifest and tool-version security | **Research** | Tool poisoning is known; systematic manifest diffing and policy separation are not measured |
| Intent compilation | Research, positioned against Progent and Conseca | Novel only if measured rigorously (precision and recall) |
| Attack World benchmark extensions | Research artifact | Capability escape, transitions, revocation, discovery and lifecycle scenarios that existing benchmarks lack |

## Relationship to the owner's other projects

| Project | Relationship |
|---|---|
| `agent-lab` | Earlier, finished-scope project: a calendar and transcript workflow agent with a deterministic policy gate. Separate repo. IntentWard reuses its lessons (scripted model, gate traces, "prove the refusal"), not its code. See [ADR 0006](adr/0006-agent-lab-stays-separate.md). |
| `ragfabric` | Retrieval platform. Capability retrieval is "RAG for tools", so its routing and evaluation lessons inform the capability plane. No dependency either way. |
| `ration` | Sibling policy kernel that decides **how much** context, model and reasoning a task needs. IntentWard decides **what authority** a task has. They could share a plan contract later; no coupling now. |
| `loomrun` | Serving layer for one inference machine. Unrelated except as a possible model backend. |
| `enterprise-ai-agent-platform` | Earlier flagship with sandboxed tools and human-in-the-loop pause and resume. Prior work for the approval flow. |
| `contextos` | The research agenda on managing information and computation better. IntentWard is the authority and safety counterpart. |
