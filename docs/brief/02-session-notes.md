> Preserved from the planning session of 2026-10-02. Original text kept as written by the owner;
> the only edit is that em dashes were replaced with colons (project style rule).

Notes the owner added between the master brief and the capability-plane extension.

## Muse Sentinel and the questions it leaves open

Muse already has something conceptually similar to what we're proposing:

Sentinel

Meta describes Sentinel as a separate security component that controls whether Muse can reach the internet and requests permission when necessary.

So we shouldn't claim:

"We invented a security agent that sits between the model and the internet."

That's already being implemented.

Instead ask:

What does Sentinel not solve?
Can we generalize it?
Can capabilities be formally compiled from intent?
Can capability transitions be formally represented?
Can we guarantee no implicit privilege escalation?
Can we benchmark capability escape?
Can we detect intent drift?
Can capabilities be dynamically attenuated?
Can capabilities be revoked reliably in a distributed runtime?
Can the same model work across different agent environments?

Those are much more interesting research questions.

## Proposed repository tree

agentos/
│
├── core/
│   ├── agent_runtime/
│   ├── planner/
│   ├── state/
│   └── scheduler/
│
├── intelligence/
│   ├── llm/
│   ├── memory/
│   ├── context/
│   └── intent/
│
├── security/
│   ├── capability/
│   ├── policy/
│   ├── identity/
│   ├── authorization/
│   ├── context_firewall/
│   ├── tool_gateway/
│   └── revocation/
│
├── isolation/
│   ├── sandbox/
│   ├── filesystem/
│   ├── network/
│   ├── secrets/
│   └── process/
│
├── verification/
│   ├── action_verifier/
│   ├── state_verifier/
│   └── intent_drift/
│
├── observability/
│   ├── audit/
│   ├── tracing/
│   ├── events/
│   └── dashboard/
│
├── benchmark/
│   ├── attacks/
│   ├── environments/
│   ├── scenarios/
│   ├── metrics/
│   └── evaluation/
│
├── sdk/
│
├── cli/
│
└── research/
    ├── experiments/
    ├── results/
    └── papers/

## Positioning: we are not building Muse

We're not building Muse.

We're not building:

"another personal AI assistant."

We're building the security/control/runtime primitives that a Muse-like autonomous system needs.

You could think of the relationship like:

## Layering: product layer vs infrastructure

PERSONAL AGENT
                          │
            ┌─────────────┴─────────────┐
            │                           │
       PRODUCT LAYER              INFRASTRUCTURE
            │                           │
       Muse / Dots /             OUR AGENTOS
       Grok-style agent                │
                                       ├── Intent
                                       ├── Capabilities
                                       ├── Policy
                                       ├── Context
                                       ├── Tools
                                       ├── Sandbox
                                       ├── Runtime
                                       ├── Verification
                                       ├── Revocation
                                       └── Benchmark

## Persistent runtime additions

One more thing I'd add to our master plan

The next revision of the AgentOS architecture should explicitly include:

1. Persistent Agent Runtime
2. Task State Machine
3. Scheduler / Wake-Sleep mechanism
4. Event Bus
5. Persistent Agent Environment / Secure VM abstraction
6. Capability system
7. Context Firewall
8. Tool Gateway
9. Verification
10. Revocation
11. Attack World / Benchmark

That makes the project capable of representing the full lifecycle of a real autonomous agent, not merely securing a chatbot with tools.

And it answers your "how does it keep working after I close the app?" question: the UI is only the client. The persistent agent runtime, state, scheduler, environment and security layer live remotely and continue executing. Meta explicitly confirms this model for Muse.

This is also why I think your original intuition was correct: the future AI-engineering problem isn't just making smarter models. It's building the infrastructure that lets those models safely operate as persistent, tool-using software entities in the real world.
