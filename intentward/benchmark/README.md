# Benchmark (Attack World)

`intentward/benchmark/` · Research plane · Phase P3 onward

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The controlled, synthetic, deterministic attack environment. Never attacks real external systems.

## Modules

| Module | Purpose |
|---|---|
| [`attacks/`](attacks/README.md) | Attack implementations: direct and indirect prompt injection, tool poisoning, malicious tool descriptions, memory poisoning, capability and privilege escalation, exfiltration, goal hijacking, drift, unauthorized deletion, secret exposure, excessive agency, malicious tool output, retrieved documents and webpages, sub-agent manipulation, resource exhaustion, loop abuse, capability confusion, revocation bypass, poisoned manifests, capability substitution, version changes. |
| [`environments/`](environments/README.md) | Synthetic world: fake repositories, documents, secrets, APIs, websites, databases, malicious pages, documents, tools and poisoned memories. |
| [`scenarios/`](scenarios/README.md) | scenario.yaml files binding a task, an environment and an attack, plus the new categories: tool overload, ambiguity, missing capabilities, provider selection, minimum authority, persistent and restart authorization, revocation during sleep and execution, tool version changes. |
| [`metrics/`](metrics/README.md) | Security, agent performance, tradeoff and operational metrics, plus capability-plane metrics (invalid calls, unnecessary calls, capability breadth). |
| [`evaluation/`](evaluation/README.md) | Runs configurations (baseline, each defense, full system, ablations), repeats with seeds, computes confidence intervals, compares. |
