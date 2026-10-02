# Architecture

> Status: proposed. This architecture is expected to change as implementation and experiments show
> better designs. It is not immutable. Changes are recorded as ADRs in [adr/](adr/).

## The six planes

| Plane | Components | Package |
|---|---|---|
| **Control plane** | Intent engine, policy, authorization and transitions, identity, delegation, capability (authority) objects | `intelligence/intent`, `security/policy`, `security/authorization`, `security/identity`, `security/delegation`, `security/capability` |
| **Agent plane** | LLM adapters, planner, memory, context assembly, task state, scheduler, event bus, agent runtime | `intelligence/*`, `core/*` |
| **Capability plane** | Registry, manifests, discovery and retrieval, capability graph, composition, gap analysis, provider resolution, protocol adapters | `capability_plane/*`, `interop/*` |
| **Execution plane** | Tool gateway, sandbox, persistent environment, filesystem, network, secrets, process limits | `security/tool_gateway`, `isolation/*` |
| **Trust plane** | Context firewall, verification, transactions, intent drift, audit, tracing, revocation | `security/context_firewall`, `security/revocation`, `verification/*`, `observability/*` |
| **Research plane** | Attack World, scenarios, attacks, metrics, evaluation, experiments | `benchmark/*`, `research/`, `configs/` |

The capability plane never bypasses the control plane. Discovery tells the agent what exists;
only the control plane says what is allowed.

## End-to-end flow

```
                         HUMAN GOAL
                             |
                             v
                       INTENT ENGINE ............ proposes objective, constraints,
                             |                    required abstract capabilities
                             v
                   REQUIRED CAPABILITIES
                             |
                             v
           +-------- CAPABILITY PLANE ---------+
           |  registry + manifests             |
           |  discovery / retrieval (multi-signal)
           |  capability graph                 |
           |  gap analysis -> missing? -------------> capability-aware failure
           |  provider resolution              |      or capability REQUEST
           +----------------+------------------+
                            |
                  CANDIDATE CAPABILITIES
                            |
                            v
                  AUTHORIZATION FILTER  <---- policy, current capability version,
                            |                  leases, revocation state
                            v
                  CAPABILITY COMPILER  -----> Capability Version N (immutable)
                            |
                            v
                      AGENT PLANNER  (untrusted: produces a proposal)
                            |
                            v
                      TOOL GATEWAY  (reference monitor)
                            |   identity, capability, arguments, resource,
                            |   rate, policy, taint, network, audit
                            v
                   POLICY ENFORCEMENT
                            |
                            v
               SANDBOX / PERSISTENT ENVIRONMENT  (OS enforcement plane)
                            |
                            v
                        EXECUTION
                            |
                            v
                  VERIFY (action + state)
                            |
                            v
                   OBSERVE -> AUDIT -> NEXT ACTION
```

Any step that needs more authority leaves the loop through the transition protocol
(REQUEST, POLICY EVALUATION, USER NOTIFICATION, APPROVE or DENY, NEW VERSION, ACTIVE) and the task
moves to `WAITING_FOR_CAPABILITY` or `WAITING_FOR_USER` until a decision exists.

## The persistent lifecycle

```
USER DEVICE -> CLIENT APP (interface only)
                    |
                    v
         PERSISTENT AGENT RUNTIME
            +-- model adapters
            +-- memory
            +-- durable task state machine
            +-- tools via the capability plane and gateway
            +-- scheduler (wake, execute, sleep)
            +-- event bus (email, timer, webhook, approval, revocation ...)
            +-- security (capabilities, leases, revocation)
            +-- persistent secure environment
```

Task states: CREATED, PLANNING, RUNNING, WAITING, WAITING_FOR_CAPABILITY, CAPABILITY_REQUESTED,
WAITING_FOR_USER, PAUSED, RESUMED, VERIFYING, COMPLETED, FAILED, REVOKED, TERMINATED. The exact set
will evolve; every transition is observable and audited.

**Lifecycle invariant (I6):** every wake or resume re-validates the capability version, the lease
expiry and the revocation state before the first action. Sleeping never extends authority.

## The eleven runtime components and the risk each adds

| # | Component | Role | New risk it introduces |
|---|---|---|---|
| 1 | Persistent agent runtime | Keeps working after the UI closes | Long-lived authority |
| 2 | Task state machine | Durable, auditable lifecycle | Illegal transitions, duplicate actions after restart |
| 3 | Scheduler | Wake, execute, sleep; retries, backoff, deadlines | Triggers as an attack entry point |
| 4 | Event bus | Internal events with provenance | Forged events, floods |
| 5 | Persistent secure environment | Per-agent filesystem, browser, network, processes, credentials | State leaking across tasks |
| 6 | Capability system | Versioned, scoped, leased authority | Its own bugs |
| 7 | Context firewall | Provenance enforced at data flow | Label laundering |
| 8 | Tool gateway | Reference monitor for every call | Bypass through shell |
| 9 | Verification | Postconditions and state checks | False assurance |
| 10 | Revocation | Remove authority live | Stale caches and credentials |
| 11 | Attack World | Synthetic, deterministic, replayable | Unrealistic scenarios |

Plus the capability plane (registry, manifests, retrieval, graph, composition, gaps, providers),
which adds the discovery attack surface: poisoned manifests, tool shadowing, provider substitution
and version confusion.

## Two enforcement planes

| Plane | Enforces | Cannot enforce |
|---|---|---|
| Gateway (semantic) | Typed tools: action, resource, arguments, taint, rate, policy | Anything inside an arbitrary shell or code process |
| OS sandbox (mechanical) | Files, processes, network, resources for anything that runs code | Intent-level meaning of an action |

Shell is excluded from the defended configuration unless it runs inside the OS plane with a
profile no broader than the capability set. RQ27 measures the gap between the two planes.

## Module map

See [`intentward/README.md`](../intentward/README.md). Every module has a README stating plane,
purpose, ownership, phase, research questions, invariants and main risk.

## Modularity for ablation

Every defense layer can be switched off by configuration (`configs/`), so the benchmark can
compare baseline, each layer alone, the full system, and the full system minus one layer. Adding
layers is not assumed to help; it is measured.

## Implementation style

One repository, modular components, no microservices until a concrete reason exists. Python first
for runtime, experiments and benchmark. Go is considered only if the gateway becomes a measured
bottleneck; TypeScript only for an SDK or dashboard. Security-critical code stays small, explicit,
typed and heavily tested. See [tech-stack.md](tech-stack.md).
