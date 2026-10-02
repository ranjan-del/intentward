# Limitations

> Status: known from the start. This list grows with every phase; it never shrinks without evidence.

| # | Limitation | Consequence |
|---|---|---|
| 1 | Nothing is implemented yet | No claim in this repository is a result |
| 2 | A compromised host defeats the reference monitor | Out of scope; audit is tamper-evident, not tamper-proof |
| 3 | Authorized misuse cannot be fully prevented by capabilities alone | Taint and approvals reduce it; residual risk is measured |
| 4 | Shell and arbitrary code cannot be validated at the gateway | Relies on the OS plane; excluded from defended configs otherwise |
| 5 | External credentials that cannot be revoked early | Revocation latency is bounded by their TTL |
| 6 | Intent compilation uses an LLM and can be wrong or injected | Validated deterministically; errors measured as over and under provisioning |
| 7 | Semantic drift detection is bypassable | Detection only, never enforcement |
| 8 | Human approvals degrade with volume | Approval frequency is a first-class metric |
| 9 | Synthetic benchmarks may not reflect real deployments | Built on AgentDojo for comparability; realism is a stated threat to validity |
| 10 | LLM results vary between runs and model versions | Seeds, repetitions, confidence intervals, recorded model ids |
| 11 | Verification is only as good as declared postconditions | Weak postconditions give false assurance; stated per tool |
| 12 | Some external side effects are irreversible | Labelled irreversible; compensations, not rollback |
| 13 | Single owner, part of a wider portfolio | Scope is staged; later phases may be cut, and that will be recorded |
