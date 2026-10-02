# The problem: limitations that grow with scale

Agents are getting more capable, more persistent and more numerous. Each row below is a limitation
that exists in today's agent ecosystem and gets worse as scale increases. IntentWard exists to
measure these limitations and to build primitives that reduce them. It does not assume the
primitives work; every row has a research question that could show they do not.

## Authority and intent

| # | Limitation today | Why it gets worse at scale | IntentWard primitive | RQ |
|---|---|---|---|---|
| L1 | Agents run with ambient authority: broad tokens, the whole filesystem, open network | More tools and longer sessions mean a larger blast radius per compromise | Intent-compiled, argument-scoped capabilities | RQ1, RQ2 |
| L2 | Permission prompts are ad hoc, unversioned and not auditable | Thousands of prompts cause approval fatigue; approvals become rubber stamps | Versioned capability transition protocol, provenance-aware requests | RQ3, RQ5 |
| L3 | A capability for one resource is often treated as permission for the action in general | More resources of the same type, more confusion | Resource and argument scoped grants | RQ1 |
| L4 | The request channel itself is trusted: the "reason" text can be written by an attacker | Users are asked more often, by more agents | Requests carry causal provenance; the reason is untrusted text | RQ5 |

## Context and data flow

| # | Limitation today | Why it gets worse at scale | IntentWard primitive | RQ |
|---|---|---|---|---|
| L5 | Injected content abuses authority the agent already holds (authorized misuse); no escalation needed | More untrusted inputs: web, mail, RAG, memory, other agents | Taint and provenance enforced at tool arguments | RQ6, RQ7 |
| L6 | Context labels are advisory; the model reads every token | Longer contexts mix more trust levels | Labels enforced where data leaves the model | RQ6 |
| L7 | Memory is replayed as if it were trusted instruction | Persistent agents accumulate memory for months | Memory as data with provenance | RQ8 |

## Capability plane

| # | Limitation today | Why it gets worse at scale | IntentWard primitive | RQ |
|---|---|---|---|---|
| L8 | Hundreds of tools are dumped into the model context | Tool counts keep growing (MCP servers, plugins); selection accuracy drops, context cost and attack surface rise | Capability retrieval ("RAG for tools") with multi-signal ranking | RQ10, RQ11 |
| L9 | Agents reason about concrete function names, tied to one provider | More providers per capability (several calendars, several mail systems) | Abstract capabilities plus provider resolution | RQ9, RQ15 |
| L10 | Multi-tool plans are improvised and often invalid | Longer chains compound invalid calls | Capability graph with preconditions and postconditions | RQ12 |
| L11 | Agents pick the broadest tool that works (a full browser instead of a search API) | Broad tools are the easiest to plug in | Minimum-authority planning | RQ13, RQ17 |
| L12 | Agents hallucinate completion when a needed capability is missing | More tasks are attempted end to end without a human | Capability gap analysis and capability-aware failure | RQ14 |
| L13 | Tool descriptions and manifests are trusted, and version changes are accepted silently | Third-party tool ecosystems grow and update constantly | Manifest describes, policy authorizes; version diffs trigger review | RQ16 |

## Runtime lifecycle

| # | Limitation today | Why it gets worse at scale | IntentWard primitive | RQ |
|---|---|---|---|---|
| L14 | Revocation is slow or impossible: long-lived tokens, running processes | Persistent agents hold authority for days | Leases plus revocation that propagates into running sessions | RQ20, RQ21 |
| L15 | Persistent agents act while the user is absent | Background, scheduled and event-triggered work makes synchronous approval impossible | Absent-user authorization policy, re-validation on wake | RQ19 |
| L16 | Wake-up events (mail arrives, webhook fires) are trusted triggers | Event-driven agents let attackers start the agent | Events are data, never authority | RQ22 |
| L17 | Restarts and distributed workers lose or duplicate authorization state | Agents run across many workers and survive crashes | Durable state machine; authorization re-checked on resume | RQ18 |

## Multi-agent, verification and generality

| # | Limitation today | Why it gets worse at scale | IntentWard primitive | RQ |
|---|---|---|---|---|
| L18 | Sub-agents inherit the parent's full authority | Agent trees multiply authority leaks | Attenuated delegation | RQ23 |
| L19 | Agents' claims of success are trusted | Longer chains compound hallucinated "done" | Action and state verification, transactions | RQ25, RQ26 |
| L20 | Drift from the goal is noticed late or never | Long-running autonomy | Drift detector on top of deterministic checks | RQ24 |
| L21 | Security is checked at the tool name, not where execution happens | Shell and code execution bypass gateway checks | Two enforcement planes: gateway and OS sandbox | RQ27 |
| L22 | Every vendor builds its own guard (Sentinel, coding-agent permission systems) and none are comparable | No shared way to measure capability escape | Environment-independent capability model and benchmark | RQ28, RQ29, RQ30 |
| L23 | Risk is scored subjectively | Nobody can compare two deployments | Measured blast radius | RQ31 |
