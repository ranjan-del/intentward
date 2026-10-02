# Roadmap

> Status: P0 not started. Week estimates assume one person working roughly full time and will be
> revised. A phase is done only when its exit criterion is met and recorded, not when its code
> exists. Each phase has a GitHub issue; releases map to milestones.

## Releases

| Release | Phases | Theme |
|---|---|---|
| v0.1.0 | P0, P1, P2 | Kernel: gateway, capabilities, transitions, audit |
| v0.2.0 | P3 | Attack World and first honest results |
| v0.3.0 | P4, P5, P6 | Capability plane and intent engine |
| v0.4.0 | P7, P8 | Context firewall and isolation |
| v0.5.0 | P9, P10 | Persistent runtime, revocation, delegation |
| v0.6.0 | P11 | Verification, transactions, drift |
| v1.0.0 | P12 | Interop, ablations, technical report, SDK |

## P0: Ground truth (weeks 1 to 2)

- [ ] Verify every entry in [docs/landscape.md](docs/landscape.md); one note per system in `research/literature/`
- [ ] Read and position against Sentinel, CaMeL, Progent, Conseca, FIDES, AgentDojo, LlamaFirewall, the Design Patterns paper, and tool-retrieval work
- [ ] Finalize [docs/threat-model.md](docs/threat-model.md)
- [ ] Choose the task suites and the first benchmark scenario before any code
- [ ] Write one file per RQ in `research/questions/` and pre-register H1 to H3
- [ ] Reserve the `intentward` name on PyPI

**Exit:** a one-page statement of what is new compared with Sentinel, Progent and CaMeL, with
citations.

## P1: Kernel (weeks 3 to 5)

- [ ] Minimal agent loop: LLM (scripted or real), planner, gateway, tool, result
- [ ] In-memory task state machine and event bus
- [ ] Capability grant objects with action, resource and argument scope
- [ ] Tool gateway as reference monitor; three typed tools (filesystem, mock HTTP, mock mail)
- [ ] Path canonicalization, symlink and TOCTOU-safe resolution
- [ ] Scripted adversarial model
- [ ] Tests for I1 and I3, including property-based tests

**Exit:** every enumerated escalation attempt by the scripted adversary is blocked; I1 and I3 hold
under property tests.

## P2: Transitions and audit (weeks 6 to 8)

- [ ] Immutable, versioned capability sets
- [ ] Transition protocol: request, policy evaluation, notification, approve or deny, new version, active
- [ ] Pause or deny until a decision; causal provenance attached to each request
- [ ] Hash-chained append-only audit log
- [ ] Durable state in SQLite
- [ ] CLI: task create and inspect, capabilities list, diff, request, approve, deny
- [ ] Tests for I2; first formal sketch of the transition protocol (RQ3)

**Exit:** I2 holds; replaying the audit log reconstructs every capability version exactly.

## P3: Attack World and first results (weeks 9 to 11)

- [ ] AgentDojo adapter (`interop/agentdojo`)
- [ ] Synthetic environments and the first own scenarios
- [ ] Metrics and evaluation runner with seeds and repetitions
- [ ] Baseline vs defended on real models

**Exit:** the first results table (utility, attack success, approvals per task), published with
its run records, including any negative result (RQ1, RQ5, RQ28).

## P4: Capability registry and manifests (weeks 12 to 14)

- [ ] Abstract capabilities and concrete tools in the registry
- [ ] Manifest schema: inputs, outputs, pre and postconditions, side effects, reversibility, risk, auth, data, cost, latency, version
- [ ] Side-effect model (read, write, delete, external action)
- [ ] Provider resolution under authorization
- [ ] Manifest and tool version diffing; review on broadened permissions
- [ ] MCP and HTTP adapters
- [ ] Tests for I7 and I8; tool-version and poisoned-manifest scenarios

**Exit:** RQ16 data; no broadened tool version takes effect silently.

## P5: Intent engine (weeks 15 to 17)

- [ ] Goal to structured task: objective, resources, required, optional and prohibited actions, risk, outputs, duration, constraints
- [ ] Goal to required abstract capabilities
- [ ] Deterministic validator and capability compiler
- [ ] Hand-labelled minimal capability sets for the task suite

**Exit:** RQ1 and RQ2 data: over- and under-provisioning rates.

## P6: Discovery, composition and gaps (weeks 18 to 21)

- [ ] Multi-signal capability retrieval
- [ ] Capability graph with typed edges
- [ ] Composition with preconditions and postconditions
- [ ] Gap analysis and capability-aware failure
- [ ] Minimum-authority planning
- [ ] Tool-selection, minimum-authority and missing-capability benchmarks
- [ ] Test for I10 (no fabricated completion)

**Exit:** RQ9 to RQ15 and RQ17 data.

## P7: Context firewall (weeks 22 to 24)

- [ ] Provenance labels on every context object
- [ ] Taint on authority-bearing tool arguments, requests and memory writes
- [ ] Memory as provenance-labelled data
- [ ] Test for I5; authorized-misuse and memory-poisoning scenarios

**Exit:** RQ6 to RQ8 data, including the false block rate.

## P8: Isolation (weeks 25 to 27)

- [ ] Sandbox profiles derived from the capability set
- [ ] Persistent secure environment abstraction
- [ ] Egress proxy with allowlists
- [ ] Secrets broker with short-lived, call-scoped credentials
- [ ] Process limits
- [ ] Shell confined to the OS plane

**Exit:** RQ17, RQ27 and RQ30 data; shell-based escape attempts measured.

## P9: Persistent runtime and revocation (weeks 28 to 31)

- [ ] Durable state machine with restart recovery and idempotent resumption
- [ ] Scheduler: wake, execute, sleep; retries, backoff, deadlines
- [ ] External events with provenance
- [ ] Leases (TTL, call counts) and renewal
- [ ] Live revocation into running sessions, proxy and credentials
- [ ] Absent-user authorization policies
- [ ] Tests for I4 and I6; long-running benchmark

**Exit:** RQ18 to RQ22 data, including the revocation latency distribution.

## P10: Delegation (weeks 32 to 34)

- [ ] Agent and sub-agent identity
- [ ] Attenuated, caveat-style delegation; no default inheritance
- [ ] Test for I9; delegation scenarios

**Exit:** RQ23 data compared with naive inheritance.

## P11: Verification, transactions, drift (weeks 35 to 37)

- [ ] Action and state verifiers from manifest postconditions
- [ ] Action journal, snapshots, rollback, compensations
- [ ] Drift detector (detection only)

**Exit:** RQ24 to RQ27 data.

## P12: Interop, ablations, report (weeks 38 to 42)

- [ ] A second agent environment through `interop/`
- [ ] Full ablation study
- [ ] Blast-radius report
- [ ] Python SDK, CLI completion, dashboard
- [ ] Technical report with every claim linked to a run

**Exit:** RQ28 to RQ31 data; the report is published.

## Deferred until there is a measured reason

Go gateway rewrite, TypeScript SDK, Cedar or OPA, OpenTelemetry, gRPC, signed manifests, TLS
interception. See [research/ideas](research/ideas/README.md).
