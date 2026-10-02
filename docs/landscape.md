# Landscape and prior art

> Status: initial list from the planning session. **Every entry is to be verified in P0** against
> the primary source, and each gets a note in `research/literature/` answering the eight questions
> below. Do not cite this table as fact until verified.

## The eight questions asked of every system

What problem does it solve? What architecture does it use? What assumptions does it make? What
threat model does it use? What does it NOT solve? What limitations remain? What performance costs
exist? What security guarantees does it actually provide?

## Agent security defenses

| System | What it does (to verify) | Overlap with IntentWard |
|---|---|---|
| Meta Muse **Sentinel** | Separate component that controls whether the assistant can reach the internet and asks permission when needed | Network gating and permission requests. Open questions: formal capabilities, argument scope, versioned transitions, attenuation, revocation, portability, a public benchmark |
| CaMeL (Google DeepMind, 2025) | Privileged planner LLM plus quarantined data LLM; an interpreter tracks capabilities and provenance and enforces policy on tool calls | Very high: capabilities, data flow, context firewall |
| Progent (2025) | Privilege-control policies for agent tools, LLM-generated and dynamically updated | Very high: intent compilation, dynamic capabilities |
| Conseca (Google, 2025) | Context-specific security policies generated just in time from the task | High: intent compilation |
| FIDES (Microsoft, 2025) | Information-flow control with confidentiality and integrity labels for agents | High: provenance, taint |
| Design Patterns for Securing LLM Agents against Prompt Injection (2025) | Catalogue: plan-then-execute, dual LLM, action selector and others | Architectural framing |
| Dual LLM pattern (Willison, 2023) | Privileged and quarantined model split | Ancestor of CaMeL |
| LlamaFirewall (Meta, 2025) | Guardrail system including an alignment check for goal drift | Intent drift layer |
| Spotlighting, instruction hierarchy, StruQ, SecAlign | Model-side provenance defenses | Behavioural layer only |
| Llama Guard, Prompt Guard, NeMo Guardrails, commercial firewalls | Classifiers and rails | Detection, not authorization |
| Small `agent-mandate` style repos and Google's AP2 "intent mandate" | Signed, scoped mandates for agent actions and payments | Terminology and idea overlap; check guarantees and evaluation |

## Benchmarks

| Benchmark | Notes |
|---|---|
| AgentDojo (ETH, 2024) | Utility and injection tasks, pluggable defenses. Our base. |
| InjecAgent, Agent Security Bench (ASB) | Injection and agent attack suites |
| Berkeley Function Calling Leaderboard, ToolBench / StableToolBench, API-Bank | Tool selection and calling accuracy |

## Tool retrieval, selection and composition

| Work | Relevance |
|---|---|
| Toolformer, Gorilla (APIBench), ToolLLM, AnyTool | Tool use and retrieval over large API sets |
| RAG-for-tools and MCP tool-retrieval work (for example RAG-MCP) | Retrieval of tools instead of listing all of them |
| STRIPS and PDDL planning, LLM+P | Preconditions and postconditions for composition |
| OWL-S, WSMO, semantic web service discovery | Abstract capability descriptions and provider matching |
| Android intents | Abstract action resolved to a concrete provider app at runtime |
| OpenAPI, service registries (Consul and others) | Manifests and discovery |

## Capability systems and authorization

| Work | Relevance |
|---|---|
| KeyKOS, EROS, seL4, Capsicum, object-capability work (Mark Miller and others) | Capability theory: no ambient authority, attenuation, confinement |
| Macaroons, Biscuit, UCAN | Attenuable, delegable tokens with caveats and expiry |
| OAuth scopes, incremental authorization, Rich Authorization Requests, GNAP | Scoped and incremental grants |
| Android and iOS runtime permissions | User-facing permission requests and fatigue evidence |
| Cedar (AWS), OPA/Rego, Zanzibar / OpenFGA | Policy engines; Cedar is formally analysable |
| SPIFFE / SPIRE | Workload identity |
| Coding-agent permission systems (allow lists, sandboxed shells) | Practical agent permission UX |

## Sandboxing and runtime

| Work | Relevance |
|---|---|
| gVisor, Firecracker, bubblewrap, nsjail, Landlock, seccomp | Isolation mechanisms |
| Hosted agent sandboxes (e2b, Modal and similar) | Persistent agent environments |
| Temporal, durable functions, workflow engines | Durable state, retries, wake-sleep patterns |
| MCP authorization specification, tool-poisoning research | MCP security |

## Where the gaps appear to be (hypotheses, not findings)

1. The capability request channel as an attack surface, and approval fatigue, measured.
2. Revocation and leases for persistent, distributed agents with delegated credentials.
3. Attenuated delegation for LLM agent trees.
4. Authorization-aware, minimum-authority capability discovery and composition.
5. Manifest and tool-version security measured systematically.
6. A capability-escape benchmark that runs across agent environments.
