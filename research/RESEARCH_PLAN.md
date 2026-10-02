# Research plan

> Status: planning. No research question is assumed to have a positive answer. Each has an
> experiment designed so that it could fail.

## The questions behind everything

**Central:** How much autonomy can we safely give an AI agent when its authority is dynamically
derived from human intent and enforced independently of the model?

**Thesis:** Intent-driven dynamic capability security. Can an agent's authority be derived from the
user's task, continuously constrained during execution, and changed only through an explicit,
auditable authorization transition?

**Umbrella (capability plane):** Can an autonomous agent discover and compose the minimum
capabilities required to accomplish a human goal while continuously proving that every action
remains within the authority granted for that goal?

**What Sentinel-style gating leaves open** (from the session notes): What does it not solve? Can it
be generalized? Can capabilities be formally compiled from intent? Can transitions be formally
represented? Can we guarantee no implicit privilege escalation? Can we benchmark capability escape?
Can we detect intent drift? Can capabilities be dynamically attenuated? Can they be revoked
reliably in a distributed runtime? Can the same model work across agent environments? These are
folded into the questions below.

## Consolidated research questions

Type: **R** research contribution, **D** detection study, **E** engineering measurement.

### A. Authority and intent

| RQ | Question | Type | Measured by | Phase |
|---|---|---|---|---|
| RQ1 | Do intent-derived capability sets reduce attack impact without significantly reducing legitimate task completion? | R | Attack success vs utility, baseline vs defended | P3, P5 |
| RQ2 | Can capability scope be compiled (automatically, and how formally) from user intent? How precise and complete is it? | R | Precision and recall against hand-labelled minimal capability sets | P5 |
| RQ3 | Can capability transitions be formally represented and still support dynamic autonomy safely? | R | Formal model of the protocol, property tests, exact audit replay, task success with transitions | P2 |
| RQ4 | Can capability immutability guarantee no implicit privilege escalation after prompt injection, and under what assumptions? | R | Scripted adversary reaching zero escapes; stated assumptions; proof sketch | P1, P2 |
| RQ5 | Can capability requests be used to social-engineer the user, and does showing causal provenance reduce approval of malicious requests? | R | Approval rate of malicious vs legitimate requests, with and without provenance | P2, P3 |

### B. Context and data flow

| RQ | Question | Type | Measured by | Phase |
|---|---|---|---|---|
| RQ6 | Can provenance-aware context, enforced at tool arguments, prevent untrusted data from becoming implicit instructions? | R | Injection success with prompt-side labels only vs argument-side enforcement | P7 |
| RQ7 | Can taint tracking reduce authorized misuse (attacks that abuse authority already held)? | R | Authorized-misuse success and false block rate | P7 |
| RQ8 | Can memory be kept as data rather than authority, and what does memory poisoning still achieve? | R | Poisoning success across sessions | P7 |

### C. Capability plane

| RQ | Question | Type | Measured by | Phase |
|---|---|---|---|---|
| RQ9 | Can agents reason over abstract capabilities rather than concrete tool names? | R | Task success and invalid calls, abstract vs concrete planning | P4, P6 |
| RQ10 | Does capability retrieval reduce tool-selection complexity compared with exposing all tools? | R | Success, invalid and unnecessary calls, tokens, as tool count grows | P6 |
| RQ11 | Does multi-signal retrieval beat semantic similarity alone? | R | Same, ablating each signal | P6 |
| RQ12 | Do capability graphs with preconditions and postconditions improve multi-tool planning? | R | Plan validity, success, invalid calls | P6 |
| RQ13 | Does minimum-authority planning reduce risk while keeping task success? | R | Capability breadth, data and network exposure vs success | P6 |
| RQ14 | Does capability gap analysis reduce hallucinated task completion? | R | Hallucinated completion rate on infeasible tasks | P6 |
| RQ15 | Do abstract capability interfaces make agents portable across providers? | R | Success when swapping providers without changing the agent | P4, P6 |
| RQ16 | Is capability metadata (descriptions, manifests, versions) an attack surface, and do manifest/policy separation and version diffing contain it? | R | Success of poisoned manifests, shadowing, substitution, version escalation | P4 |
| RQ17 | How do narrowly scoped tools compare with broad tools (shell, full browser) in efficiency and security? | R | Utility, escape rate, cost per tool style | P6, P8 |

### D. Runtime lifecycle

| RQ | Question | Type | Measured by | Phase |
|---|---|---|---|---|
| RQ18 | Can persistent agents keep authorization correct across restarts, scheduled wake-ups and distributed execution? | R | Stale capability use, duplicate actions, consistency after restart | P9 |
| RQ19 | How should authorization work when the user is absent? | R | Unsafe actions, delay and user burden across absent-user policies | P9 |
| RQ20 | What are the security and performance tradeoffs of leases, including across sleep and wake? | R | Overhead vs stale-authority window | P9 |
| RQ21 | How quickly and reliably can authority be revoked from a live, long-running, distributed agent? | R | Revocation latency distribution, post-revocation actions | P9 |
| RQ22 | Can events and wake triggers be kept as data and never authority? | R | Success of forged-event attacks | P9 |

### E. Multi-agent

| RQ | Question | Type | Measured by | Phase |
|---|---|---|---|---|
| RQ23 | Can capabilities be dynamically attenuated and delegated safely to sub-agents? | R | Cross-agent escalation and leakage vs naive inheritance | P10 |

### F. Verification and drift

| RQ | Question | Type | Measured by | Phase |
|---|---|---|---|---|
| RQ24 | Can intent drift be detected with deterministic authorization plus semantic trajectory analysis? | D | Detection and false positive rates | P11 |
| RQ25 | Does action and state verification reduce the impact of hallucination and tool misuse? | D | Undetected false "done" rate | P11 |
| RQ26 | Can agent actions be made transactional (preview, rollback, compensation), and at what cost? | E | Recovery rate, inconsistent end states, overhead | P11 |
| RQ27 | How large is the gap between "policy says ALLOW" and what execution actually does (gateway vs OS plane)? | R | Scope violations despite allowed decisions | P8, P11 |

### G. Generality and measurement

| RQ | Question | Type | Measured by | Phase |
|---|---|---|---|---|
| RQ28 | Can capability escape be benchmarked in a way that transfers across agent systems? | R | Same suite run against more than one runtime | P3, P12 |
| RQ29 | Can the same capability model work across agent environments (IntentWard, MCP hosts, AgentDojo, another runtime)? | R | One policy and benchmark through several adapters | P12 |
| RQ30 | What does network gating of the Sentinel kind not solve, and does a full capability model close those gaps? | R | Attack classes that pass a network-only defense but fail against the full model | P8, P12 |
| RQ31 | Can blast radius be measured rather than scored, and does a capability graph help compared with a flat list? | R | Reachable-resource computation vs observed damage in attacks | P6, P12 |

## Mapping from the original numbering

The brief and the extension both used RQ11 to RQ20 for different questions. This table keeps both
traceable.

| Original | Consolidated |
|---|---|
| Brief RQ1, RQ2, RQ3, RQ4, RQ5 | RQ1, RQ4, RQ3, RQ24, RQ17 |
| Brief RQ6, RQ7, RQ8, RQ9, RQ10 | RQ2, RQ21, RQ20, RQ6, RQ25 |
| Extension RQ11, RQ12, RQ13, RQ14, RQ15 | RQ9, RQ10, RQ11, RQ12, RQ13 |
| Extension RQ16, RQ17, RQ18, RQ19, RQ20 | RQ14, RQ15, RQ16, RQ18, RQ21 |
| Session additions (request channel, absent user, events, authorized misuse, leases across sleep) | RQ5, RQ19, RQ22, RQ7, RQ20 |
| Sentinel questions | RQ2, RQ3, RQ4, RQ21, RQ23, RQ24, RQ28, RQ29, RQ30 |
| Brief sections 21, 28, 29, 37, 39 | RQ26, RQ31, RQ31, RQ8, RQ27 |

## Initial hypotheses (each with how it could fail)

| H | Hypothesis | Disproved if |
|---|---|---|
| H1 | Intent-scoped capabilities cut attack success by at least half on AgentDojo with under 10 points utility loss | Utility loss is larger, or attack reduction is smaller |
| H2 | LLM-compiled capability sets reach at least 0.9 recall of the minimal set at acceptable precision | Recall stays low (tasks fail) or precision is low (over-provisioning) |
| H3 | Showing causal provenance lowers approval of malicious requests without lowering approval of legitimate ones | No difference, or legitimate approvals drop too |
| H4 | Taint on authority-bearing arguments blocks most authorized-misuse attacks at a false block rate under 5 percent | Either number misses |
| H5 | Multi-signal retrieval beats semantic-only retrieval on invalid and unnecessary calls as tool counts grow past 100 | No gain, or gains vanish with stronger models |
| H6 | Minimum-authority planning lowers capability breadth with no significant drop in success | Success drops significantly |
| H7 | Gap analysis cuts hallucinated completion on infeasible tasks to near zero | Hallucinated completions persist |
| H8 | Re-validation on wake eliminates stale-capability use after restart and sleep | Any stale use observed |
| H9 | Revocation latency for in-runtime grants is under one second; for external credentials it equals their TTL | Longer, or authority reappears |
| H10 | Attenuated delegation prevents cross-agent escalation that naive inheritance allows | Escalation still possible |

Thresholds are provisional and will be fixed in each experiment's pre-registration before any run.

## Contribution focus

The headline contributions are expected to come from groups A (transitions and the request
channel), C (authorization-aware discovery and minimum authority), D (revocation and lifecycle) and
E (delegation). Intent compilation and taint are built as infrastructure and compared against
Progent, Conseca, CaMeL and FIDES rather than claimed as new.

## Paper direction

Working title: "Intent-Driven Capability Security for Autonomous AI Agents". Outline in
[papers/README.md](papers/README.md). The paper is written after the evidence exists, not before.
