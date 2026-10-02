# Security model

> Status: draft. Invariants are stated as goals to be tested. None is claimed to hold until the
> test suite and benchmark show it, and each claim will state its assumptions.

## Principles

1. The model is untrusted. Its output is a proposal.
2. Authority lives outside the agent process, in a reference monitor the agent cannot write to.
3. Deterministic enforcement first; semantic (LLM-based) monitoring only as an extra detection layer.
4. Default deny. Every grant is scoped by action, resource and arguments.
5. Every change of authority is an explicit, observable, audited transition.
6. Untrusted data may inform; it may not instruct or authorize.
7. The manifest describes; the policy authorizes.
8. Verify outcomes; never trust a claim of completion.
9. Do not claim guarantees stronger than the implementation provides.

## Invariants

| ID | Invariant | Source | Tested against | Phase |
|---|---|---|---|---|
| **I1** | No untrusted component can increase the active capability set without an explicit authorized capability transition. | Brief section 55 | Model output, tool output, malicious documents and webpages, memory, sub-agents, API responses, prompt injection, tool poisoning | P1 |
| **I2** | A capability change produces an observable state transition and cannot silently modify the active capability version. | Brief section 56 | Old set, request, notification, pause or deny, approval, new version, audit record | P2 |
| **I3** | A capability for one resource or action does not authorize another merely because the action type matches. `delete(temp.txt)` never implies `delete(database.db)`. | Brief section 57 | Globs, symlinks, path traversal, TOCTOU, case and encoding tricks | P1 |
| **I4** | Revoked authority is not usable through stale runtime state, cached authorization, delegated credentials or already-running tool sessions. | Brief section 58 | Revocation during execution, during sleep, with running subprocesses, with issued short-lived credentials | P9 |
| **I5** | Untrusted context never gains instruction authority solely by being retrieved or presented to the model. | Brief section 59 | RAG, websites, documents, emails, tool outputs, memory, sub-agent messages | P7 |
| **I6** | Every wake or resume re-validates capability version, lease and revocation state before the first action. Sleeping never extends authority. | Persistent runtime notes | Restarts, scheduled wake-ups, event wake-ups, worker migration | P9 |
| **I7** | A tool's manifest or description never grants authority. Discovery and retrieval never bypass authorization. | Extension sections 19 and 20 | Poisoned manifests, deceptive descriptions, tool shadowing, provider substitution | P4 |
| **I8** | A new tool, manifest or policy version that broadens required permissions never takes effect silently; it triggers a capability and security review. | Extension section 18 | Version 2 of a tool that adds `contact.read` and `email.send` | P4 |
| **I9** | A delegated (child) capability set is always a subset of the delegator's set. No inheritance by default. | Brief section 38 | Delegation chains, re-delegation, cross-agent data flow | P10 |
| **I10** | The runtime never reports a goal as completed when a required capability was missing or verification failed. | Extension sections 14 and 15, brief section 20 | Missing-capability tasks, failed postconditions | P6, P11 |

I1 is close to true by construction once authority lives in a separate reference monitor. It is
still tested exhaustively, but the research weight sits on what I1 does **not** cover: authorized
misuse (L5), the request channel (L4), revocation (I4), lifecycle (I6) and delegation (I9).

## Capability monotonicity (to be formalized)

Informally: authority at time t+1 is a subset of what has been explicitly authorized up to t+1.

```
C(t+1) is a subset of AuthorizedCapabilities(t+1)
AuthorizedCapabilities changes only through transition events signed by an authorized principal
environmental_input, model_output, tool_output, manifest_claim, event  are never such events
```

The exact formalization (state machine, possibly a TLA+ or Alloy model of the transition protocol)
is a research output of P2. It is not assumed.

## Two kinds of evaluation

| Mode | Model | Purpose |
|---|---|---|
| Worst case | Scripted adversarial model that always attempts maximal escalation | Tests enforcement deterministically. Results are exact, not statistical. |
| Average case | Real LLMs, N seeded runs, confidence intervals | Measures utility, attack success, approvals, overheads. |

## Known hard cases (from the start)

| Case | Why it is hard | Plan |
|---|---|---|
| Authorized misuse | No escalation happens | Taint on authority-bearing arguments (P7) |
| Shell and code execution | Turing complete, cannot be validated at the gateway | OS plane, or excluded (P8) |
| TOCTOU and symlinks | Check and use differ | Canonicalize, open-by-handle semantics, property tests (P1) |
| Credentials that cannot be revoked early | External systems | Short TTLs; revocation latency bounded by TTL and stated (P9) |
| Approval fatigue | Humans approve everything | Measure approvals per task, show provenance (P2, P3) |
| Policy says ALLOW but execution violates intended scope | Semantic gap between policy and effect | Measure it (RQ27), verification (P11) |
| A compromised host | The reference monitor runs there too | Out of scope; stated in [limitations.md](limitations.md) |

## Failure modes to study

Model hallucination, malicious model behaviour, direct and indirect prompt injection, tool
poisoning, compromised tools, malicious context, memory poisoning, policy bugs, authorization bugs,
sandbox escape, credential leakage, capability confusion, race conditions, TOCTOU, distributed state
inconsistency, stale authorization, revocation failure, agent loops, excessive tool use, resource
exhaustion, hidden side effects, poisoned manifests, version confusion, provider substitution,
forged events, absent-user actions.

## Questions asked of every mechanism

Does it actually improve security? What attacks bypass it? What legitimate tasks does it break?
What latency does it add? What new attack surface does it introduce? Does it create false positives?
Can the policy be manipulated? Can authorization state become inconsistent? Can a race bypass it?
Can stale capabilities be exploited? Can a sub-agent inherit authority incorrectly? Can the agent
influence its own authorization?
