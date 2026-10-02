<h1 align="center">IntentWard</h1>

<p align="center">
  <b>An open-source agent runtime where an AI agent's authority comes from the user's intent and is enforced outside the model.</b><br/>
  Intent becomes scoped capabilities. The agent discovers and composes only the tools it needs.
  Every change of authority is an explicit, audited transition. Authority can be leased, attenuated
  and revoked for the whole life of a persistent agent. A built-in Attack World measures whether
  any of it works.
</p>

<p align="center">
  <a href="LICENSE"><img alt="License: Apache 2.0" src="https://img.shields.io/badge/license-Apache%202.0-blue.svg"></a>
  <img alt="Status: planning" src="https://img.shields.io/badge/status-planning-lightgrey">
</p>

> **Status: planning.** Nothing is implemented yet. This repository holds the plan, the research
> questions and the module tree. [ROADMAP.md](ROADMAP.md) tracks what exists and what is next.
> No section below describes a working feature, and no number will appear here unless it came from
> a recorded run.

---

## The question

**How much autonomy can we safely give an AI agent when its authority is dynamically derived from
human intent and enforced independently of the model?**

And, once agents have to find their own tools: **can an agent discover and compose the minimum
capabilities a goal needs while every action provably stays within the authority granted for that
goal?**

## The idea in one picture

```
MODEL DECIDES  ->  SYSTEM AUTHORIZES  ->  SANDBOX ENFORCES  ->  SYSTEM VERIFIES
```

The model is a powerful but untrusted component. It can reason, plan, choose tools and request
more authority. It never holds authority. Nothing it reads or writes (web pages, documents, tool
output, memory, other agents, tool manifests, events) can increase what it may do. Only an
explicit, authorized, audited transition can.

## What it is, and what it is not

| IntentWard is not | IntentWard is |
|---|---|
| Another personal assistant, chatbot or RAG app | The security, control and runtime layer that assistants like these need |
| An agent framework clone or an LLM API wrapper | A reference monitor, capability model, transition protocol and persistent runtime |
| An MCP framework | A protocol-independent capability plane; MCP is one transport |
| A new sandbox, policy engine or benchmark from scratch | Integrations of existing ones, plus the missing pieces, measured |
| A claim that agents are now safe | A system built to be attacked, measured, broken and improved |

Full positioning, including how it differs from network gating of the kind Meta describes for
Muse's Sentinel: [docs/positioning.md](docs/positioning.md).

## What it is meant to solve

Limitations that get worse as agents scale. The full list of 23 is in [docs/problem.md](docs/problem.md).

| Today | IntentWard primitive |
|---|---|
| Agents run with ambient authority | Intent-compiled, argument-scoped capabilities |
| Permission prompts are ad hoc and get rubber-stamped | Versioned transitions; requests show what triggered them |
| Injected content abuses authority the agent already has | Provenance and taint enforced at tool arguments |
| Hundreds of tools dumped into the context | Capability retrieval ("RAG for tools"), least authority first |
| Agents claim success when a tool was missing | Gap analysis, capability-aware failure, verification |
| Tool manifests are trusted and updates accepted silently | Manifest describes, policy authorizes; version diffs trigger review |
| Background agents cannot be stopped cleanly | Leases, live revocation, re-validation on every wake |
| Sub-agents inherit everything | Attenuated delegation |
| No way to compare agent safety | A capability-escape benchmark that runs across environments |

## Architecture

Six planes. The capability plane never bypasses the control plane.

| Plane | Contains |
|---|---|
| Control | Intent engine, policy, authorization and transitions, identity, delegation |
| Agent | Model adapters, planner, memory, context, task state machine, scheduler, event bus, persistent runtime |
| Capability | Registry, manifests, discovery, capability graph, composition, gap analysis, provider resolution |
| Execution | Tool gateway, sandbox, persistent environment, filesystem, network, secrets, process limits |
| Trust | Context firewall, verification, transactions, intent drift, audit, revocation |
| Research | Attack World, scenarios, metrics, evaluation, experiments |

```
HUMAN GOAL -> INTENT ENGINE -> REQUIRED CAPABILITIES -> CAPABILITY DISCOVERY -> AUTHORIZATION FILTER
  -> CAPABILITY COMPILER (version N) -> PLANNER -> TOOL GATEWAY -> POLICY -> SANDBOX -> EXECUTION
  -> VERIFY -> OBSERVE -> AUDIT -> NEXT ACTION
```

Details: [docs/architecture.md](docs/architecture.md).

## Security invariants (to be tested, not yet claimed)

| ID | Invariant |
|---|---|
| I1 | No untrusted component can increase the active capability set without an explicit authorized transition |
| I2 | A capability change produces an observable transition and never silently modifies the active version |
| I3 | A grant for one resource or action never authorizes another just because the action type matches |
| I4 | Revoked authority is not usable through stale state, caches, delegated credentials or running sessions |
| I5 | Untrusted context never gains instruction authority by being retrieved or shown to the model |
| I6 | Every wake or resume re-validates version, lease and revocation; sleeping never extends authority |
| I7 | A tool's manifest or description never grants authority |
| I8 | A tool version that broadens required permissions never takes effect silently |
| I9 | A delegated capability set is always a subset of the delegator's |
| I10 | A goal is never reported complete when a required capability was missing or verification failed |

## Research

31 research questions in seven groups (authority and intent, context and data flow, capability
plane, runtime lifecycle, multi-agent, verification and drift, generality), 10 falsifiable
hypotheses, and an experiment protocol with honesty rules. See
[research/RESEARCH_PLAN.md](research/RESEARCH_PLAN.md) and
[docs/experiment-protocol.md](docs/experiment-protocol.md).

## Roadmap

| Release | Phases | Theme |
|---|---|---|
| v0.1.0 | P0 to P2 | Ground truth, kernel, transitions and audit |
| v0.2.0 | P3 | Attack World and first honest results |
| v0.3.0 | P4 to P6 | Capability registry, intent engine, discovery and composition |
| v0.4.0 | P7, P8 | Context firewall, isolation |
| v0.5.0 | P9, P10 | Persistent runtime, revocation, delegation |
| v0.6.0 | P11 | Verification, transactions, drift |
| v1.0.0 | P12 | Interop, ablations, technical report, SDK |

Each phase has an exit criterion that is a measurement. See [ROADMAP.md](ROADMAP.md).

## Who it is for

Agent framework authors, MCP host and server builders, teams deploying internal agents, security
researchers, benchmark builders and learners. See [docs/use-cases.md](docs/use-cases.md).

## Repository layout

```
intentward/
├── intentward/                 the Python package (one README per module)
│   ├── core/                   agent_runtime, planner, state, scheduler, event_bus
│   ├── intelligence/           llm, memory, context, intent
│   ├── capability_plane/       registry, manifest, discovery, graph, composition, gap_analysis, providers
│   ├── security/               capability, policy, identity, authorization, context_firewall,
│   │                           tool_gateway, delegation, revocation
│   ├── isolation/              sandbox, environment, filesystem, network, secrets, process
│   ├── verification/           action_verifier, state_verifier, transactions, intent_drift
│   ├── observability/          audit, tracing, events, dashboard
│   ├── interop/                mcp, http, agentdojo
│   ├── benchmark/              attacks, environments, scenarios, metrics, evaluation
│   ├── sdk/
│   └── cli/
├── tests/                      invariants, adversarial, property
├── configs/                    baseline, defenses, full system, ablations
├── research/                   RESEARCH_PLAN, questions, hypotheses, literature, ideas,
│                               experiments, results, failed_experiments, papers
└── docs/                       design documents, ADRs, original briefs
```

## Prior art

IntentWard builds on and positions itself against CaMeL, Progent, Conseca, FIDES, the Dual LLM
pattern, LlamaFirewall, AgentDojo, object-capability systems, macaroons, Biscuit and UCAN, and
tool-retrieval research. The list is being verified: [docs/landscape.md](docs/landscape.md).

## Project documentation

Every flagship repository documents the same ten things. Status shows what exists today.

| Section | Document | Status |
|---|---|---|
| README | [README.md](README.md) | Written |
| Architecture | [docs/architecture.md](docs/architecture.md) | Written |
| Design decisions | [docs/adr/README.md](docs/adr/README.md) | Written |
| Benchmarks | [docs/benchmark.md](docs/benchmark.md) | Written |
| Failure cases | [docs/failure-cases.md](docs/failure-cases.md) | Partial |
| Evaluation | [docs/experiment-protocol.md](docs/experiment-protocol.md) | Written |
| Trade-offs | [docs/trade-offs.md](docs/trade-offs.md) | Partial |
| Deployment | [docs/deployment.md](docs/deployment.md) | To be written |
| Cost | [docs/cost.md](docs/cost.md) | To be written |
| Future work | [ROADMAP.md](ROADMAP.md) | Written |

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md). The short version: no number without a run, no guarantee
without a test, and model output never changes authority.

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
