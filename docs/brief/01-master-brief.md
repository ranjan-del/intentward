> Preserved from the planning session of 2026-10-02. Original text kept as written by the owner;
> the only edit is that em dashes were replaced with colons (project style rule).

# MASTER PROJECT BRIEF

# Secure Agent Runtime / AgentOS

## Intent-Driven Capability Security for Autonomous AI Agents

You are working with me on a serious, research-oriented AI systems project.

This is NOT a small demo, chatbot, LangChain clone, generic RAG application, AutoGPT clone, or simple "AI firewall".

We are building a **full agent runtime / AgentOS-style security and control infrastructure** around AI agents.

The core problem is:

> As AI models become increasingly capable and increasingly autonomous, how can we allow agents to perform useful real-world tasks while ensuring that their authority, capabilities, resources, context, and actions remain strictly bounded by the user's actual intent?

The project should combine:

* AI agent runtime
* capability-based security
* intent compilation
* dynamic authorization
* policy enforcement
* context security
* sandboxing
* tool security
* agent security
* prompt-injection resistance
* intent-drift detection
* capability escalation prevention
* capability revocation
* runtime verification
* auditability
* observability
* controlled attack environments
* security benchmarks
* reproducible experiments
* research methodology

The goal is to create one coherent system rather than several disconnected projects.

---

# 1. CORE PHILOSOPHY

The fundamental architectural principle is:

> The LLM should be treated as a powerful but untrusted decision-making component.

The model can:

* reason
* plan
* propose actions
* choose tools
* interpret information
* generate code
* request additional capabilities

But the model must NOT directly own authority.

The system around the model determines:

* what the agent can see
* what the agent can do
* what resources it can access
* what tools it can invoke
* what arguments it can provide
* what network destinations it can reach
* what secrets it can access
* how long permissions remain valid
* how many times a capability can be used
* whether a high-risk operation requires approval
* whether a capability can be revoked
* whether an action is actually consistent with the original task
* whether the resulting state matches the intended result

The core separation is:

```
MODEL DECIDES
      ↓
SYSTEM AUTHORIZES
      ↓
SANDBOX ENFORCES
      ↓
SYSTEM VERIFIES
```

Never rely on the LLM itself as the final security boundary.

---

# 2. WHAT IS A HARNESS IN THIS PROJECT?

The project should be understood as an AI harness / agent runtime.

A harness is the software infrastructure around an AI model that provides the environment in which the model operates.

It can provide:

* context
* memory
* tools
* planning
* state
* execution
* permissions
* policies
* sandboxing
* networking
* verification
* observability
* recovery
* feedback
* human approval

The model is the reasoning engine.

The harness is the controlled operating environment.

We are therefore building a serious harness/runtime architecture rather than simply an agent application.

---

# 3. CORE RESEARCH THESIS

The main research direction is:

## Intent-Driven Dynamic Capability Security

Investigate whether an AI agent's authority can be automatically derived from the user's task, continuously constrained during execution, and changed only through an explicit, auditable authorization transition.

The central question is:

> Can we compile user intent into a narrowly scoped capability set and guarantee that no model output, tool output, retrieved document, memory entry, external webpage, sub-agent, or environmental event can silently increase the agent's authority?

This should become the conceptual center of the system.

---

# 4. FUNDAMENTAL SECURITY PROPERTY

The system should aim to enforce:

## No Implicit Authority Escalation

An agent must NEVER gain new authority merely because:

* the model requested it
* a webpage requested it
* a document instructed it
* a tool returned an instruction
* memory contained an instruction
* another agent requested it
* a sub-agent requested it
* an external API requested it
* a prompt injection attempted it
* an environment changed
* the model decided that it "needed" it

A capability can only change through an explicitly defined authorization transition.

In simplified form:

```
environmental_input != authorization
```

and:

```
model_output != authorization
```

and:

```
tool_output != authorization
```

Only an authorized policy transition can modify capabilities.

This invariant should be tested extensively.

---

# 5. HIGH-LEVEL ARCHITECTURE

The target architecture is approximately:

```
                    USER
                      |
                      v
             +----------------+
             | Intent Compiler |
             +--------+-------+
                      |
                      v
             +----------------+
             | Capability Plan|
             +--------+-------+
                      |
                      v
             +----------------+
             | Policy Engine  |
             +--------+-------+
                      |
                Capability Set
                      |
                      v
          +-----------------------+
          |     AGENT RUNTIME     |
          |                       |
          | LLM                   |
          | Planner               |
          | Memory                |
          | Context Manager       |
          | State                 |
          +-----------+-----------+
                      |
                      v
             +----------------+
             |Context Firewall|
             +--------+-------+
                      |
                      v
             +----------------+
             |  Tool Gateway  |
             +--------+-------+
                      |
            +---------+---------+
            |         |         |
            v         v         v
         Browser   Files      APIs
            |         |         |
            +---------+---------+
                      |
                      v
             +----------------+
             |Policy Enforcement|
             +--------+-------+
                      |
                      v
             +----------------+
             |    Sandbox     |
             +--------+-------+
                      |
                      v
                  EXECUTION
                      |
                      v
             +----------------+
             |  Verification  |
             +--------+-------+
                      |
                      v
              Audit / Trace
                      |
                      v
                User/System
```

This architecture should evolve as implementation and research reveal better designs.

Do not treat this diagram as immutable.

---

# 6. MAJOR SUBSYSTEMS

The system should eventually contain the following major components.

## 6.1 Intent Compiler

Input:

```
"Read these documents and create a summary."
```

The compiler should produce a structured task representation.

For example:

```
task:
  objective: summarize documents

resources:
  - /documents/project-x/*

allowed:
  - filesystem.read
  - report.write

denied:
  - shell.execute
  - network.access
  - email.send
  - database.write
  - filesystem.delete
```

The compiler should distinguish:

* objective
* resources
* required actions
* optional actions
* prohibited actions
* risk level
* expected outputs
* duration
* constraints

Do not assume the LLM alone should determine authorization.

The intent compiler may use an LLM for semantic interpretation, but the resulting authorization must be validated by deterministic policy.

---

# 7. CAPABILITY MODEL

Build a capability-based authorization system.

A capability should not merely say:

```
"agent can delete files"
```

It should describe multiple dimensions.

A capability may contain:

* action
* resource
* resource scope
* arguments
* identity
* purpose
* time
* expiration
* rate limit
* network restrictions
* sensitivity
* tenant
* task ID
* capability version
* parent capability
* approval state

Example:

```
capability:
  action: filesystem.delete

  resource:
    path: /workspace/project/temp.txt

  scope:
    exact: true

  purpose:
    task_id: 82f91
    reason: cleanup

  max_calls: 1

  expires:
    2026-10-02T01:00:00Z

  version: 42
```

The system must distinguish:

```
delete(temp.txt) -> ALLOW
```

from:

```
delete(database.db) -> DENY
```

even though both are technically "delete".

---

# 8. CAPABILITY VERSIONING

Capabilities should be versioned.

Example:

```
Capability Version 17

filesystem.read
  /project/docs/*

filesystem.write
  /project/output/*

filesystem.delete
  NONE
```

If the agent later requests:

```
filesystem.delete
  /project/docs/a.pdf
```

do NOT mutate Version 17.

Instead create:

```
Version 18
  STATUS = PROPOSED
```

The system should explicitly represent capability transitions.

Possible states:

```
CURRENT
   |
   v
REQUESTED
   |
   +------> REJECTED
   |
   v
PENDING
   |
   v
REVIEW
   |
   +------> DENIED
   |
   v
APPROVED
   |
   v
NEW VERSION
   |
   v
ACTIVE
```

This state transition must be auditable.

---

# 9. USER NOTIFICATION FOR CAPABILITY CHANGES

This is a critical requirement.

If the agent originally has:

```
READ
```

and later requests:

```
DELETE
```

the system must NOT silently grant DELETE.

The user must be informed.

Example UI:

```
------------------------------------------------
CAPABILITY CHANGE REQUEST

Agent wants to:

+ DELETE
  /project/docs/a.pdf

Reason:
"Remove temporary file."

Current permission:
DELETE = DENIED

[ Reject ]              [ Approve ]
------------------------------------------------
```

Until an explicit authorization transition occurs, the agent should:

```
PAUSE
```

or:

```
DENY
```

depending on policy.

The user should always be able to see:

* old capability set
* requested change
* reason
* affected resources
* risk
* agent state
* proposed new capability
* expiration
* scope

Never allow silent capability expansion.

---

# 10. CAPABILITY INTEGRITY

Research and implement the concept of capability integrity.

Once a capability set is established:

```
Capability Version N
```

the agent operates under that version.

Anything that wants to change authority must create:

```
Capability Version N+1
```

through the authorization mechanism.

No component inside the untrusted agent environment should be able to mutate the active capability state directly.

Explore cryptographic or tamper-resistant representations where useful.

Potential ideas:

* signed capability manifests
* immutable capability IDs
* capability hashes
* append-only transition logs
* short-lived capability tokens
* lease-based capabilities
* cryptographic binding to task/session identity

Do not blindly implement every idea.

Evaluate their tradeoffs.

---

# 11. CAPABILITY LEASES

Investigate temporary capabilities.

Instead of:

```
filesystem.read = forever
```

use:

```
filesystem.read
resource = /project/docs
duration = 10 minutes
max_calls = 100
```

After expiry:

```
capability becomes invalid
```

Investigate:

* expiration
* single-use capabilities
* call limits
* resource limits
* risk-based lease duration
* automatic revocation
* capability renewal

Do not allow the model to manipulate its own lease.

---

# 12. CAPABILITY REVOCATION

Support live revocation.

If the user revokes permission while the agent is operating:

```
revoke(capability)
```

the runtime should:

1. invalidate the capability
2. reject future tool calls
3. terminate or pause affected operations where possible
4. revoke temporary credentials
5. terminate relevant network access
6. invalidate capability tokens
7. update runtime state
8. record the event
9. notify the agent/user
10. ensure revoked authority cannot silently reappear

Research the difficulty of revocation in distributed agent systems.

---

# 13. CONTEXT FIREWALL

Build a Context Firewall.

Not all context should have equal authority.

Potential sources:

* system policy
* user instruction
* trusted application state
* memory
* retrieved documents
* RAG results
* websites
* emails
* tool outputs
* external APIs
* sub-agents

Every context object should carry metadata such as:

```
source
provenance
trust
authority
sensitivity
timestamp
tenant
instruction_allowed
data_allowed
```

Example:

```
source: website
trust: untrusted
authority: data
instruction_allowed: false
```

A website can provide information.

It cannot grant the agent authority.

A document can contain instructions as data.

It must not automatically become a system instruction.

This is a core defense against indirect prompt injection.

---

# 14. TOOL GATEWAY

The LLM should NEVER directly access arbitrary tools.

All tools should pass through a gateway.

Architecture:

```
LLM
  |
  v
Tool Gateway
  |
  +-- Identity
  +-- Capability check
  +-- Argument validation
  +-- Resource validation
  +-- Rate limiting
  +-- Policy evaluation
  +-- Network policy
  +-- Audit
  |
  v
Tool
```

The gateway should support:

* MCP tools
* HTTP APIs
* filesystem
* shell
* databases
* browser
* Git providers
* cloud APIs
* internal services

The tool gateway is a major security boundary.

---

# 15. TOOL ARGUMENT SECURITY

Do not only validate the tool name.

Validate the arguments.

Example:

```
capability:
  action = filesystem.delete
  scope = /workspace/project/temp.txt
```

Request:

```
filesystem.delete(
    "/workspace/project/temp.txt"
)
```

ALLOW.

Request:

```
filesystem.delete(
    "/workspace/project/.env"
)
```

DENY.

The action may be allowed while the specific resource is not.

This distinction must exist throughout the system.

---

# 16. SANDBOX

The runtime should support isolation.

Investigate:

* Linux namespaces
* cgroups
* seccomp
* filesystem isolation
* network namespaces
* container isolation
* read-only filesystems
* ephemeral workspaces
* process limits
* CPU limits
* memory limits
* disk limits
* network allowlists
* DNS restrictions
* secret isolation

Docker can be one implementation.

Do not assume Docker itself is the complete security boundary.

Study stronger isolation mechanisms.

---

# 17. NETWORK SECURITY

Network access must be explicitly controlled.

Examples:

```
network = DENIED
```

or:

```
network:
  allowed_domains:
    - api.example.com
```

The agent should not be able to:

```
access arbitrary internet
scan arbitrary hosts
contact arbitrary IPs
exfiltrate data to unknown destinations
```

Investigate:

* egress policies
* DNS filtering
* proxy-based control
* domain allowlists
* IP restrictions
* request budgets
* network identity
* TLS interception where appropriate for controlled environments

---

# 18. SECRETS

The model should never automatically see all secrets.

Research:

* secret brokers
* temporary credentials
* secret-scoped capabilities
* environment isolation
* secret redaction
* credential rotation
* short-lived tokens

Example:

Instead of:

```
AGENT_HAS_AWS_SECRET
```

use:

```
agent requests:
    cloud.storage.write
    bucket = project-output
```

The gateway obtains the minimum required credential without exposing unnecessary secrets to the model.

---

# 19. INTENT DRIFT DETECTION

Represent the original task as a structured objective.

Example:

```
GOAL:
Fix authentication bug.
```

Then the agent trajectory becomes:

```
inspect repository
    ↓
inspect auth module
    ↓
run tests
    ↓
modify auth code
    ↓
run tests
    ↓
commit fix
```

The system should detect suspicious divergence.

Questions:

* Is this action authorized?
* Is it within resource scope?
* Does it advance the goal?
* Is it necessary?
* Is it unusually risky?
* Is it reversible?
* Does it introduce a new objective?
* Is the agent moving into unrelated resources?

Use two layers:

### Deterministic layer

* capability
* resource
* scope
* policy
* rate
* expiration

### Semantic layer

* goal/action relationship
* trajectory analysis
* intent drift
* unusual behavior

Do not make semantic monitoring the only security layer.

---

# 20. VERIFICATION

Never trust the model's claim that it completed an action.

Example:

```
Agent:
"I deleted the temporary file."
```

Verify actual state.

For files:

```
check filesystem
```

For Git:

```
check repository state
```

For databases:

```
verify transaction
```

For APIs:

```
verify actual response/state
```

For deployments:

```
verify deployment state
```

Use:

```
PLAN
  ↓
AUTHORIZE
  ↓
EXECUTE
  ↓
VERIFY
  ↓
COMMIT
```

For high-risk operations consider:

```
PLAN
  ↓
SIMULATE
  ↓
AUTHORIZE
  ↓
EXECUTE
  ↓
VERIFY
  ↓
COMMIT
```

---

# 21. TRANSACTIONAL THINKING

Investigate whether agent actions can be treated as transactions.

Questions:

* Can actions be previewed?
* Can they be rolled back?
* Can they be committed only after verification?
* What happens if step 4 fails?
* How do we recover from partial execution?
* How do we prevent an agent from leaving the system in an inconsistent state?

Explore:

* checkpoints
* snapshots
* transactional tool execution
* compensating actions
* rollback
* idempotency
* action journals

This should become part of the runtime.

---

# 22. AGENT SECURITY BENCHMARK

The benchmark must be part of the same repository.

Do NOT build it as a separate toy project.

Create a controlled attack environment.

All targets must be synthetic and isolated.

Possible attack classes:

* direct prompt injection
* indirect prompt injection
* tool poisoning
* malicious tool descriptions
* memory poisoning
* capability escalation
* privilege escalation
* data exfiltration
* goal hijacking
* intent drift
* unauthorized deletion
* secret exposure
* excessive agency
* malicious tool output
* malicious retrieved documents
* malicious webpages
* sub-agent manipulation
* resource exhaustion
* agent loop abuse
* capability confusion
* revocation bypass

---

# 23. BASELINE VS DEFENDED AGENT

Create a deliberately weak baseline.

Example:

```
Baseline Agent
    |
    +-- Shell
    +-- Filesystem
    +-- Internet
    +-- APIs
```

Then run the benchmark.

Then progressively introduce:

```
Baseline
   ↓
Least privilege
   ↓
Capability enforcement
   ↓
Context firewall
   ↓
Intent monitoring
   ↓
Verification
   ↓
Revocation
   ↓
Full system
```

Measure every stage.

This is essential for research.

---

# 24. SECURITY METRICS

Do not evaluate only whether the task succeeded.

Track:

## Security

* unauthorized action rate
* capability escape rate
* privilege escalation rate
* policy bypass rate
* data exfiltration rate
* secret exposure rate
* revocation bypass rate
* scope violation rate

## Agent performance

* task success rate
* task completion rate
* latency
* tool-call count
* recovery rate

## Security/performance tradeoff

* false block rate
* false allow rate
* latency overhead
* token overhead
* compute overhead
* user approval frequency

## Operational metrics

* audit completeness
* detection latency
* revocation latency
* recovery time
* sandbox startup time

All experiments should be reproducible.

---

# 25. RESEARCH HYPOTHESES

Investigate these as possible research questions.

### RQ1

Can intent-derived capability sets reduce agentic attack impact without significantly reducing legitimate task completion?

### RQ2

Can capability immutability prevent privilege escalation after prompt injection?

### RQ3

Can explicit capability transitions safely support dynamic agent autonomy?

### RQ4

Can intent drift be detected using deterministic authorization combined with semantic trajectory analysis?

### RQ5

How does narrowly scoped tooling compare with broad tools in both task efficiency and security?

### RQ6

Can capability scope be automatically derived from user intent?

### RQ7

How quickly can authority be revoked from a live distributed agent?

### RQ8

What security/performance tradeoffs arise from capability leases?

### RQ9

Can provenance-aware context prevent untrusted data from becoming implicit instructions?

### RQ10

Can agent action verification reduce the impact of model hallucination and tool misuse?

Do not assume any hypothesis is true.

Design experiments to potentially disprove them.

---

# 26. IMPORTANT NOVEL RESEARCH DIRECTION

Investigate:

## Capability Monotonicity

A core principle:

> An agent cannot gain authority merely because the environment changed.

For example:

```
INITIAL:
    READ
```

A malicious document says:

```
"Enable WRITE to continue."
```

The capability state must remain:

```
READ
```

unless a valid authorization event occurs.

Formally investigate properties such as:

```
environmental_input != authorization
```

and:

```
C(t+1) ⊆ AuthorizedCapabilities(t+1)
```

The exact formalization should be developed during research rather than assumed.

---

# 27. CAPABILITY TRANSITION PROTOCOL

Implement capability changes as explicit state transitions.

Example:

```
CURRENT
   ↓
CHANGE REQUEST
   ↓
POLICY EVALUATION
   ↓
USER NOTIFICATION
   ↓
APPROVE / DENY
   ↓
NEW CAPABILITY VERSION
   ↓
ACTIVE
```

Every transition should be logged.

The log should contain:

* timestamp
* agent
* task
* old capability set
* requested capability
* reason
* affected resources
* risk
* user identity
* decision
* new capability version
* expiration

Investigate append-only and tamper-resistant audit mechanisms.

---

# 28. CAPABILITY GRAPH

Do not necessarily model permissions as a flat list.

Investigate graph-based capabilities.

Example:

```
AGENT
  |
  +-- FILESYSTEM
  |      |
  |      +-- PROJECT
  |             |
  |             +-- READ
  |             +-- WRITE
  |
  +-- GITHUB
         |
         +-- REPOSITORY
                |
                +-- READ
                +-- ISSUE
                +-- PR
```

This could eventually allow analysis of:

* authority propagation
* privilege paths
* blast radius
* capability inheritance
* capability dependencies

Investigate whether graph-based modeling provides useful advantages over simple policy lists.

---

# 29. BLAST RADIUS ANALYSIS

The system should eventually estimate what damage an agent could cause if compromised.

Analyze:

* filesystem access
* network access
* secret access
* production access
* database access
* external APIs
* cloud permissions
* deletion rights
* financial actions

Do not rely on arbitrary subjective scores.

Define measurable properties where possible.

Potential output:

```
Agent Capability Report

Filesystem:
    READ /project/*
    WRITE /project/output/*

Network:
    api.example.com

Secrets:
    none

Production:
    none

Maximum reachable resources:
    ...
```

Use this to study least privilege.

---

# 30. CONTROLLED ATTACK WORLD

Create a synthetic environment containing:

* fake repositories
* fake documents
* fake secrets
* fake APIs
* fake websites
* fake databases
* malicious webpages
* malicious documents
* malicious tools
* poisoned memories

The benchmark must never depend on attacking real external systems.

The environment should allow deterministic replay.

Example:

```
scenario.yaml

task:
  "Fix authentication bug."

environment:
  repository: vulnerable-auth-app
  website: malicious-docs-site
  secrets: synthetic
  api: fake-payment-api

attack:
  type: indirect_prompt_injection
```

This makes research reproducible.

---

# 31. EXPERIMENTAL METHODOLOGY

Every research experiment should include:

1. Research question
2. Hypothesis
3. Threat model
4. Baseline
5. Proposed mechanism
6. Experimental environment
7. Attack scenarios
8. Metrics
9. Results
10. Ablation study
11. Limitations
12. Conclusion

Do not manufacture results.

Never claim an improvement without running the experiment.

Maintain reproducible experiment configurations.

Store:

```
experiments/
results/
configurations/
traces/
benchmark_cases/
```

---

# 32. ABLATION STUDIES

The system should support removing components.

For example:

```
Full system

- Context Firewall
- Capability enforcement
- Intent monitor
- Verification
- Revocation
```

Then compare results.

This helps identify which architecture components actually matter.

Do not assume that adding more security layers always improves the system.

Measure.

---

# 33. SYSTEM TECHNOLOGY DIRECTION

Use technologies deliberately.

Initial implementation may use:

### Python

For:

* agent runtime
* experiments
* LLM integration
* benchmark
* research tooling

### Go

Potentially for:

* security-critical gateway
* runtime services
* high-performance policy enforcement
* distributed components

### TypeScript

Potentially for:

* SDK
* dashboard
* UI

### Linux

Deeply study:

* namespaces
* cgroups
* seccomp
* capabilities
* process isolation
* filesystem permissions
* networking

### Containers

Use Docker initially, but research stronger isolation where appropriate.

### Policy

Investigate:

* RBAC
* ABAC
* capability-based security
* policy-as-code
* OPA/Rego

### Agent interoperability

Investigate:

* MCP
* HTTP
* gRPC
* event-driven tools

### Observability

Investigate:

* OpenTelemetry
* structured logs
* distributed tracing
* event streams

Do not add technologies merely because they are popular.

Each technology must solve a concrete architectural requirement.

---

# 34. EXPECTED INTERFACES

The system should eventually expose:

## Agent Runtime API

```
create_agent()
create_task()
execute()
pause()
resume()
terminate()
```

## Capability API

```
compile_capabilities()
request_capability()
approve_capability()
deny_capability()
revoke_capability()
inspect_capabilities()
```

## Tool API

```
register_tool()
invoke_tool()
inspect_tool()
```

## Security API

```
evaluate_policy()
inspect_context()
detect_drift()
verify_action()
```

## Benchmark API

```
create_scenario()
run_attack()
evaluate()
compare()
```

Design these cleanly.

---

# 35. CLI

Eventually create something like:

```
agentos task create

agentos task inspect

agentos capabilities list

agentos capabilities diff

agentos capabilities revoke

agentos agent pause

agentos agent resume

agentos security inspect

agentos benchmark run

agentos benchmark compare

agentos trace show
```

The CLI should make the security model visible.

---

# 36. DASHBOARD

Eventually provide a UI showing:

### Agent

* current task
* current state
* current capability version
* current tools
* current resources
* current risk state

### Capability changes

Show:

```
OLD
  ↓
REQUEST
  ↓
USER DECISION
  ↓
NEW
```

### Runtime

* current actions
* tool calls
* blocked actions
* policy decisions
* context provenance
* sandbox status

### Security

* attack attempts
* blocked attacks
* successful attacks
* capability violations
* drift alerts

The UI is secondary to the runtime and research.

Do not spend excessive time polishing UI before the security architecture works.

---

# 37. MEMORY SECURITY

Memory must not automatically become authority.

Research:

```
memory = data
```

not:

```
memory = trusted instruction
```

Every memory item should potentially carry:

* source
* trust
* timestamp
* task
* tenant
* provenance
* sensitivity
* authority level

Investigate memory poisoning.

---

# 38. MULTI-AGENT SECURITY

Eventually support multiple agents.

Example:

```
Planner Agent
     |
     v
Research Agent
     |
     v
Coding Agent
```

Do not allow capability inheritance by default.

Investigate:

* capability delegation
* capability attenuation
* agent identity
* sub-agent permissions
* cross-agent data flow
* capability transfer
* delegation chains

A child agent should not automatically receive the parent's entire authority.

This is an important research area.

---

# 39. FAILURE MODES TO STUDY

The system should explicitly investigate:

* model hallucination
* malicious model behavior
* prompt injection
* indirect prompt injection
* tool poisoning
* compromised tools
* malicious context
* memory poisoning
* policy bugs
* authorization bugs
* sandbox escape
* credential leakage
* capability confusion
* race conditions
* TOCTOU issues
* distributed state inconsistency
* stale authorization
* revocation failure
* agent loops
* excessive tool use
* resource exhaustion
* hidden side effects

Especially investigate cases where:

```
policy says ALLOW
```

but:

```
actual execution violates intended scope
```

This is an important systems problem.

---

# 40. SECURITY IS NOT ONLY A PROMPT PROBLEM

Never design the system around:

```
"Tell the model not to do bad things."
```

Prompts are useful for behavior.

They are not sufficient for authorization.

Security must exist below the model.

Use:

```
behavioral guidance
      +
deterministic policy
      +
capability enforcement
      +
sandboxing
      +
network controls
      +
verification
      +
observability
      +
revocation
```

---

# 41. DON'T OVERBUILD EVERYTHING AT ONCE

The final architecture is large.

Development should proceed incrementally.

## Stage 0 : Threat model

Before implementation:

* define attacker
* define protected resources
* define trust boundaries
* define assets
* define security properties
* define attack classes

Produce:

```
THREAT_MODEL.md
```

---

## Stage 1 : Minimal agent runtime

Implement:

```
LLM
  ↓
Planner
  ↓
Tool
  ↓
Result
```

No advanced security yet.

---

## Stage 2 : Tool gateway

All tool calls pass through:

```
Agent
  ↓
Gateway
  ↓
Tool
```

Implement deterministic validation.

---

## Stage 3 : Capability system

Implement:

* capability objects
* scopes
* expiration
* versioning
* authorization
* denial

---

## Stage 4 : Capability transitions

Implement:

* request
* approval
* denial
* pause
* versioning
* audit

This is where the user notification requirement becomes real.

---

## Stage 5 : Sandbox

Add:

* filesystem isolation
* process isolation
* network control
* resource limits

---

## Stage 6 : Context Firewall

Implement provenance and trust levels.

---

## Stage 7 : Verification

Implement action/result verification.

---

## Stage 8 : Intent Drift

Add semantic trajectory monitoring.

---

## Stage 9 : Revocation

Implement live capability revocation.

---

## Stage 10 : Attack Benchmark

Create controlled attack scenarios.

---

## Stage 11 : Research experiments

Run:

```
baseline
vs
each defense
vs
full architecture
```

---

# 42. WHAT NOT TO DO

Do NOT:

* build another generic chatbot
* build another basic RAG app
* clone LangChain
* clone AutoGPT
* clone an existing agent framework
* simply wrap an LLM API
* claim novelty because the project combines existing libraries
* build a security dashboard without a security runtime
* rely exclusively on another LLM as a judge
* rely exclusively on prompts for security
* assume Docker solves agent security
* assume sandboxing solves prompt injection
* assume prompt injection detection alone solves the problem
* invent benchmark results
* claim that the system is secure without testing it

We are looking for architectural insight and measurable research results.

---

# 43. HOW TO HANDLE EXISTING SYSTEMS

Before designing major components:

Research existing work.

Study:

* agent runtimes
* AI harnesses
* agent security frameworks
* sandboxing systems
* MCP security
* capability systems
* OS capability security
* policy engines
* IAM systems
* prompt injection defenses
* agent benchmarks
* AI security benchmarks
* identity for AI agents
* current industry agent-security platforms

For each relevant system, record:

```
What problem does it solve?
What architecture does it use?
What assumptions does it make?
What threat model does it use?
What does it NOT solve?
What limitations remain?
What performance costs exist?
What security guarantees does it actually provide?
```

Never reinvent something without first understanding the existing solution.

But also do not simply copy existing implementations.

Our research should emerge from identified limitations.

---

# 44. RESEARCH GAP PROCESS

For every major component:

```
Existing approach
      ↓
Limitation
      ↓
Failure case
      ↓
Research question
      ↓
Hypothesis
      ↓
Proposed architecture
      ↓
Experiment
      ↓
Results
      ↓
Limitation
      ↓
Next hypothesis
```

This process is more important than the amount of code.

---

# 45. RESEARCH DOCUMENTATION

Maintain:

```
docs/

architecture/
threat-model/
security-model/
capability-model/
context-model/
benchmark/
experiments/
research/
decisions/
```

Important documents:

```
ARCHITECTURE.md
THREAT_MODEL.md
SECURITY_MODEL.md
CAPABILITY_MODEL.md
CONTEXT_SECURITY.md
AGENT_RUNTIME.md
BENCHMARK.md
RESEARCH_PLAN.md
EXPERIMENT_PROTOCOL.md
LIMITATIONS.md
```

Maintain an ADR directory:

```
docs/adr/
```

Every significant architectural decision should have an ADR.

---

# 46. RESEARCH NOTEBOOK

Maintain:

```
research/
```

with:

```
questions/
hypotheses/
experiments/
results/
failed_experiments/
literature/
ideas/
```

Failures are valuable.

Document:

* what was expected
* what happened
* why it may have happened
* what changed
* what should be tested next

---

# 47. PAPER-ORIENTED DEVELOPMENT

The implementation should eventually support a research paper or technical report.

Potential paper direction:

## "Intent-Driven Capability Security for Autonomous AI Agents"

Possible sections:

1. Introduction
2. Threat Model
3. Agent Authority Problem
4. Existing Approaches
5. Intent-to-Capability Compilation
6. Capability Integrity
7. Context Firewall
8. Dynamic Capability Transitions
9. Revocation
10. Intent Drift Detection
11. Experimental Environment
12. Benchmark
13. Results
14. Ablation Studies
15. Security/Performance Tradeoffs
16. Limitations
17. Future Work

Do not write the paper first.

Build evidence first.

---

# 48. IMPORTANT RESEARCH PRINCIPLE

Do not assume our architecture is correct.

The system should be designed to discover whether the ideas work.

For every proposed mechanism ask:

```
Does it actually improve security?

What attacks bypass it?

What legitimate tasks does it break?

What latency does it add?

What new attack surface does it introduce?

Does it create false positives?

Can the policy itself be manipulated?

Can the authorization state become inconsistent?

Can a race condition bypass enforcement?

Can an attacker exploit stale capabilities?

Can a sub-agent inherit authority incorrectly?

Can the agent influence its own authorization?
```

These questions are as important as implementation.

---

# 49. THE DEEPEST RESEARCH DIRECTION

The long-term research direction is not merely:

```
"How do we stop bad agents?"
```

It is:

> How do we construct AI systems whose ability to affect the external world is formally and dynamically bounded by the authority derived from human intent?

This moves the project toward:

* AI systems
* security
* operating systems
* distributed authorization
* agent architecture
* formal reasoning
* AI safety
* infrastructure

That intersection is where we should explore.

---

# 50. LONG-TERM VISION

Eventually imagine:

```
                 HUMAN INTENT
                       |
                       v
              INTENT REPRESENTATION
                       |
                       v
            CAPABILITY COMPILATION
                       |
                       v
             POLICY / AUTHORITY
                       |
                       v
                AGENT RUNTIME
                       |
         +-------------+-------------+
         |             |             |
       Memory       Tools         Context
         |             |             |
         +-------------+-------------+
                       |
                       v
                ENFORCEMENT
                       |
                       v
                 EXECUTION
                       |
                       v
                 VERIFICATION
                       |
                       v
                   AUDIT
                       |
                       v
                HUMAN OVERSIGHT
```

The agent becomes powerful because it has useful capabilities.

It remains controlled because capabilities are:

* explicit
* scoped
* temporary
* auditable
* revocable
* independently enforced
* tied to intent

---

# 51. DEVELOPMENT RULES FOR CLAUDE CODE

When implementing this project:

1. Understand the architecture before writing large amounts of code.
2. Keep security boundaries explicit.
3. Prefer deterministic enforcement over LLM-based enforcement.
4. Use strong typing and schemas for security-sensitive objects.
5. Write tests for every authorization boundary.
6. Write adversarial tests, not only happy-path tests.
7. Treat every external input as potentially malicious.
8. Never allow model output to directly mutate authorization state.
9. Never silently expand capabilities.
10. Log every security-sensitive transition.
11. Make experiments reproducible.
12. Benchmark performance.
13. Document assumptions.
14. Document limitations.
15. Do not claim guarantees stronger than the implementation provides.
16. Research existing solutions before introducing a new abstraction.
17. Preserve modularity so individual security mechanisms can be disabled for ablation studies.
18. Keep baseline and defended configurations reproducible.
19. Prefer simple security primitives before complex ML-based mechanisms.
20. When uncertain about a security property, design an experiment rather than making an assumption.

---

# 52. HOW I WANT YOU TO WORK WITH ME

Treat me as a research/engineering collaborator.

Do not merely generate code because I ask for code.

When appropriate:

* challenge assumptions
* identify security weaknesses
* identify existing prior art
* identify possible novelty
* propose experiments
* explain tradeoffs
* identify missing threat models
* suggest better architectures
* point out when an idea already exists
* distinguish engineering from research contribution

When proposing a new feature, answer internally:

```
What problem does this solve?
What threat does it address?
What existing systems already do this?
What is different?
What new attack surface does it introduce?
How can we measure it?
How can we break it?
How can we reproduce the result?
```

---

# 53. IMPLEMENTATION STYLE

Start with a clean modular architecture.

Do not create a huge monolithic application.

But also do not prematurely create dozens of microservices.

Start with modular components in one repository.

Extract services only when there is a concrete architectural reason.

Security-critical code should be small, explicit and heavily tested.

---

# 54. FIRST TASK

Before writing major implementation code, perform the following:

1. Inspect the repository.
2. Determine what already exists.
3. Create an initial architecture proposal.
4. Create THREAT_MODEL.md.
5. Create SECURITY_MODEL.md.
6. Create CAPABILITY_MODEL.md.
7. Create RESEARCH_PLAN.md.
8. Identify relevant existing systems and prior art.
9. Identify likely research gaps.
10. Define the minimal end-to-end prototype.
11. Define the first benchmark scenario.
12. Define the first measurable security invariant.
13. Propose the initial repository structure.
14. Then begin implementation incrementally.

Do not build the entire system in one pass.

---

# 55. FIRST SECURITY INVARIANT

The first invariant to implement and test should be:

> **No untrusted component can increase the active capability set without an explicit authorized capability transition.**

Test this against:

* model output
* tool output
* malicious document
* malicious webpage
* memory
* sub-agent
* API response
* prompt injection
* tool poisoning

The system should demonstrate that all of these can request or suggest additional authority but cannot directly grant it.

---

# 56. SECOND SECURITY INVARIANT

Implement and test:

> **A capability change must produce an observable state transition and cannot silently modify the active capability version.**

Test:

```
old capability
requested capability
user notification
pause/deny
approval
new capability version
audit record
```

---

# 57. THIRD SECURITY INVARIANT

Implement:

> **A valid capability for one resource/action cannot automatically authorize another resource/action merely because the action type is the same.**

Example:

```
delete(temp.txt) = ALLOW
```

must not imply:

```
delete(database.db) = ALLOW
```

---

# 58. FOURTH SECURITY INVARIANT

Investigate:

> **Revoked authority must not remain usable through stale runtime state, cached authorization, delegated credentials, or already-running tool sessions.**

This is a particularly important distributed-systems/security research problem.

---

# 59. FIFTH SECURITY INVARIANT

Investigate:

> **Untrusted context must never gain instruction authority solely by being retrieved or presented to the model.**

This should apply to:

* RAG
* websites
* documents
* emails
* tool outputs
* memory
* sub-agent messages

---

# 60. FINAL PROJECT DEFINITION

The project is:

# A research-grade secure AI agent runtime that compiles human intent into constrained capabilities and continuously enforces, verifies, monitors, and audits agent authority throughout execution.

It combines:

```
Agent Runtime
+
Intent Compilation
+
Capability-Based Security
+
Policy Enforcement
+
Context Firewall
+
Tool Gateway
+
Sandboxing
+
Network Isolation
+
Secret Isolation
+
Capability Versioning
+
Dynamic Capability Requests
+
Explicit User Approval
+
Capability Revocation
+
Intent Drift Detection
+
Action Verification
+
Auditability
+
Controlled Attack Environment
+
Agent Security Benchmark
+
Reproducible Research
```

The project should ultimately answer:

> **How much autonomy can we safely give an AI agent when its authority is dynamically derived from human intent and enforced independently of the model?**

That is the central engineering and research question.

Build toward evidence, not assumptions.

Build toward measurable security properties, not marketing claims.

Build toward a system that can be attacked, measured, broken, improved, and eventually defended with reproducible experimental evidence.
