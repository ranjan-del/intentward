> Preserved from the planning session of 2026-10-02. Original text kept as written by the owner;
> the only edit is that em dashes were replaced with colons (project style rule).

> Note: the pasted extension ended mid-way through section 36 (Extended research architecture).
> The full combined architecture is in docs/architecture.md.

# AGENTOS : MASTER PROMPT EXTENSION

## Capability Discovery, Tool Intelligence, Tool Composition & Persistent Agent Runtime

This document is an extension to the existing AgentOS master project specification already provided.

IMPORTANT:

* Do NOT duplicate or replace the previous architecture.
* Do NOT remove existing components.
* Do NOT restate the previous security architecture unless needed to explain a new dependency.
* Add the following concepts, architecture, research questions, and implementation requirements to the existing project.
* Treat the previous master prompt as the foundation.
* This document defines the NEW layer we are adding.

The new focus is:

> How can an AI agent discover the capabilities available to it, understand what each capability can and cannot do, compose multiple capabilities to solve a human goal, recognize when required capabilities are missing, and execute the resulting plan while remaining constrained by the existing AgentOS authorization and security architecture?

---

# 1. NEW CORE IDEA

AgentOS should not think of the world as:

```
"Here are 500 tools. Pick one."
```

Instead, introduce the concept of:

# Capability Discovery

The agent should reason at the level of:

```
"What capability do I need to accomplish this goal?"
```

rather than immediately thinking:

```
"Which function should I call?"
```

Example:

Human:

```
"Find when I am free next week and schedule a meeting with John."
```

The system should derive capabilities such as:

```
contact.search
calendar.read
calendar.availability
calendar.create
```

Then resolve those abstract capabilities to actual tools/providers.

The system should therefore separate:

```
HUMAN GOAL
    ↓
REQUIRED CAPABILITIES
    ↓
AVAILABLE CAPABILITIES
    ↓
AUTHORIZED CAPABILITIES
    ↓
CONCRETE TOOLS
    ↓
EXECUTION
```

This abstraction is a major new architectural layer.

---

# 2. MCP IS NOT THE CAPABILITY SYSTEM

Maintain this distinction throughout the project.

MCP is one mechanism/protocol through which tools and capabilities can be exposed to an AI application.

AgentOS should NOT become:

```
"an MCP framework"
```

Instead:

```
AgentOS understands capabilities.
```

MCP is one possible transport/interface for those capabilities.

Other implementations may include:

```
REST
HTTP
gRPC
local functions
internal services
browser automation
custom tools
cloud APIs
```

Architecture:

```
            CAPABILITY PLANE
                   |
         +---------+---------+
         |         |         |
        MCP       REST      gRPC
         |         |         |
       Tools     Tools     Tools
```

The capability abstraction should remain independent of the underlying provider/protocol.

---

# 3. CAPABILITY PLANE

Add a dedicated:

# Capability Plane

to the AgentOS architecture.

The overall system should conceptually contain:

```
CONTROL PLANE
  - Intent
  - Policy
  - Authorization
  - Identity

AGENT PLANE
  - LLM
  - Planner
  - Memory
  - State
  - Scheduler

CAPABILITY PLANE
  - Tool Registry
  - Capability Registry
  - Capability Discovery
  - Tool Retrieval
  - Capability Graph
  - Tool Composition
  - Capability Gap Analysis

EXECUTION PLANE
  - Tool Gateway
  - Sandbox
  - Network
  - Secrets

TRUST PLANE
  - Context Firewall
  - Verification
  - Intent Drift
  - Audit
  - Revocation

RESEARCH PLANE
  - Attack World
  - Benchmark
  - Experiments
  - Evaluation
```

The Capability Plane must integrate with the existing security architecture rather than bypass it.

---

# 4. CAPABILITY REGISTRY

Build a standardized registry of capabilities/tools.

Each capability should have structured metadata.

Example:

```
id: calendar.create_event

name:
  Create Calendar Event

description:
  Creates a calendar event for an authenticated user.

category:
  calendar

inputs:
  start_time:
    type: datetime

  end_time:
    type: datetime

  title:
    type: string

  attendees:
    type: list[email]

outputs:
  event:
    type: calendar_event

capabilities_required:
  - calendar.write

risk:
  level: medium

side_effects:
  - creates_external_event

reversible:
  true

authentication:
  user_oauth

provider:
  google-calendar

protocol:
  mcp
```

The exact schema should evolve through implementation and research.

Do not hardcode the example as the final schema.

---

# 5. TOOL/CAPABILITY MANIFEST

Every provider/tool should be able to publish a manifest describing:

* capability ID
* human-readable description
* inputs
* outputs
* preconditions
* postconditions
* required authorization
* resource scope
* side effects
* reversibility
* risk level
* network requirements
* authentication requirements
* data access
* data sensitivity
* provider
* protocol
* version
* availability
* cost
* latency characteristics

Example:

```
calendar.create

reads:
  calendar

writes:
  calendar

side_effects:
  external_state_change

authentication:
  user_oauth

risk:
  medium
```

The manifest must be treated as security-sensitive metadata.

---

# 6. TOOL RETRIEVAL

Do NOT expose every available tool directly to the LLM.

Bad:

```
500 tools
   ↓
  LLM
```

This increases:

* context size
* tool-selection difficulty
* ambiguity
* attack surface
* tool-description pollution

Instead:

```
HUMAN GOAL
    ↓
CAPABILITY RETRIEVAL
    ↓
RELEVANT CAPABILITIES
    ↓
LLM / PLANNER
```

The system should retrieve a small relevant subset.

---

# 7. CAPABILITY RETRIEVAL SHOULD NOT BE SIMPLE VECTOR SEARCH

Investigate multi-signal capability retrieval.

Potential signals:

```
semantic similarity
input compatibility
output compatibility
authorization
risk
side effects
cost
latency
availability
provider
user preferences
task constraints
```

Example:

```
flight.search

semantic match: high
authorization: allowed
side effects: none
cost: low
availability: available
```

versus:

```
flight.book

semantic match: high
authorization: approval required
side effects: purchase
risk: high
```

The retrieval/ranking architecture should consider these differences.

---

# 8. CAPABILITY RETRIEVAL AS "RAG FOR TOOLS"

Investigate the relationship between knowledge retrieval and capability retrieval.

Conceptually:

```
RAG:

Query
  ↓
Retrieve documents
  ↓
LLM

Capability Retrieval:

Goal
  ↓
Retrieve capabilities
  ↓
Planner/LLM
```

This can be treated as a form of:

# Capability RAG / Tool RAG

However, do NOT assume vector search alone is sufficient.

Research:

* semantic retrieval
* structured filtering
* graph retrieval
* symbolic constraints
* hybrid retrieval
* capability compatibility
* authorization-aware retrieval

Explore how this relates to the existing RagFabric architecture where appropriate, without unnecessarily coupling the projects.

---

# 9. CAPABILITY GRAPH

Add a graph representation of capabilities.

Example:

```
                "Book a Trip"
                      |
                      v
                flight.search
                      |
                flight.details
                      |
                flight.select
                      |
                 flight.book
                      |
                calendar.create
```

Represent relationships such as:

* requires
* produces
* consumes
* depends_on
* enables
* conflicts_with
* requires_approval
* alternative_to

This allows the system to reason about capability composition.

---

# 10. TOOL COMPOSITION

A human goal may require multiple capabilities.

Example:

```
"Find when I am free and schedule a meeting with John."
```

Possible plan:

```
contact.search
    ↓
calendar.search
    ↓
calendar.availability
    ↓
intersection
    ↓
calendar.create
```

Another example:

```
"Find a flight and add it to my calendar."

flight.search
    ↓
flight.details
    ↓
calendar.create
```

The system should support planning across multiple tools.

---

# 11. CAPABILITY PRECONDITIONS AND POSTCONDITIONS

Capabilities should describe what must be true before execution and what they produce afterward.

Example:

```
flight.details

precondition:
  flight_id exists

output:
  flight_details
```

Then:

```
flight.book

precondition:
  flight_id exists
  authorized_booking_capability exists
  user approval exists
```

This allows the planner to construct valid action sequences.

Investigate whether this can reduce invalid tool calls and improve planning reliability.

---

# 12. ABSTRACT CAPABILITY VS CONCRETE TOOL

Separate:

```
ABSTRACT CAPABILITY
```

from:

```
CONCRETE IMPLEMENTATION
```

Example:

```
calendar.create
```

could resolve to:

```
Google Calendar
Microsoft Outlook
Apple Calendar
Internal Calendar
```

The agent should ideally reason about:

```
calendar.create
```

rather than:

```
google_calendar_create_event_v2()
```

The Capability Plane resolves the abstract capability to a concrete provider/tool.

This provides:

* portability
* provider independence
* cleaner planning
* easier authorization
* easier auditing
* easier testing

Do not assume this abstraction is novel.

Perform prior-art research before making research claims.

---

# 13. PROVIDER RESOLUTION

Implement a provider-resolution layer.

Example:

```
capability:
  calendar.create
```

Possible providers:

```
Google Calendar
Outlook
Internal Calendar
```

Provider selection can consider:

```
availability
user authorization
cost
latency
reliability
required data
risk
task constraints
```

The chosen provider must still pass the existing authorization and policy system.

---

# 14. CAPABILITY GAP ANALYSIS

Add:

# Capability Gap Analyzer

This is an important component.

Given:

```
HUMAN GOAL
```

calculate:

```
required capabilities
available capabilities
authorized capabilities
missing capabilities
blocked capabilities
alternative capabilities
alternative execution paths
```

Example:

```
Goal:
  "Book a restaurant."

Required:

  restaurant.search
  restaurant.availability
  restaurant.booking

Available:

  restaurant.search
  restaurant.availability

Missing:

  restaurant.booking
```

The system must recognize:

```
GOAL = NOT FULLY EXECUTABLE
```

rather than hallucinating completion.

---

# 15. CAPABILITY-AWARE FAILURE

The agent should be able to explicitly say:

```
"I cannot complete this task because the required
 restaurant.booking capability is unavailable."
```

It may then offer legitimate alternatives:

```
1. Search and provide available restaurants.
2. Open the relevant booking interface.
3. Request an additional authorized capability.
4. Ask the user to complete the missing step.
```

Never fabricate successful execution.

---

# 16. MINIMUM CAPABILITY PLANNING

Investigate:

# Minimum Capability Planning

Given:

```
HUMAN GOAL
```

find the smallest/least-authority set of capabilities required to complete it.

Example:

```
Goal:
  "Find flight prices."
```

Possible plans:

```
Plan A:
  browser
  login
  full website access

Plan B:
  flight.search API
```

The planner should prefer the plan that satisfies the goal with less authority when appropriate.

Possible optimization dimensions:

```
capability breadth
risk
side effects
number of tools
cost
latency
data exposure
required credentials
```

This should integrate directly with the existing least-privilege architecture.

---

# 17. TOOL SIDE-EFFECT MODEL

Capabilities should distinguish between:

```
READ
```

and:

```
WRITE
```

and:

```
DELETE
```

and:

```
EXTERNAL ACTION
```

For example:

```
calendar.search
    read-only

calendar.create
    external state change

calendar.delete
    destructive
```

This information should influence:

* planning
* ranking
* authorization
* approval
* verification
* benchmark evaluation

---

# 18. TOOL VERSIONING

Tools/capabilities can change.

Therefore track:

```
provider version
capability version
manifest version
policy version
```

If a tool changes its behavior or permission requirements, detect the difference.

Example:

```
OLD:

calendar.create
  calendar.write

NEW:

calendar.create
  calendar.write
  contact.read
  email.send
```

This should trigger a capability/security review.

Never silently accept a broader permission requirement.

---

# 19. CAPABILITY DISCOVERY SECURITY

Capability discovery itself is an attack surface.

Research:

* malicious tool descriptions
* poisoned manifests
* deceptive capability descriptions
* malicious providers
* tool shadowing
* capability impersonation
* provider substitution
* schema manipulation
* version confusion
* malicious metadata

A tool saying:

```
"This tool can read your files."
```

must not automatically receive permission to read arbitrary files.

The manifest describes capability.

The policy system authorizes capability.

These remain separate.

---

# 20. CAPABILITY DISCOVERY + EXISTING SECURITY

The new flow should become:

```
                    HUMAN GOAL
                        |
                        v
                 INTENT ENGINE
                        |
                        v
              REQUIRED CAPABILITIES
                        |
                        v
             CAPABILITY DISCOVERY
                        |
                        v
              CANDIDATE CAPABILITIES
                        |
                        v
              AUTHORIZATION FILTER
                        |
                        v
               CAPABILITY COMPILER
                        |
                        v
                 AGENT PLANNER
                        |
                        v
                 TOOL GATEWAY
                        |
                        v
                POLICY ENFORCEMENT
                        |
                        v
                    SANDBOX
                        |
                        v
                   EXECUTION
                        |
                        v
                   VERIFY
                        |
                        v
                  OBSERVE
                        |
                        v
                NEXT ACTION
```

The new Capability Plane must never bypass existing authorization.

---

# 21. PERSISTENT AGENT RUNTIME

Extend the existing Agent Runtime to support long-running tasks.

The application UI is NOT the agent.

The agent runtime should be able to continue operating after the user closes the application.

Conceptually:

```
USER DEVICE
    |
    v
CLIENT APPLICATION
    |
    v
PERSISTENT AGENT RUNTIME
    |
    +-- Model
    +-- Memory
    +-- State
    +-- Tools
    +-- Scheduler
    +-- Event System
    +-- Security
    +-- Persistent Environment
```

The client is only an interface.

---

# 22. TASK STATE MACHINE

Implement a durable task state machine.

Possible states:

```
CREATED
PLANNING
RUNNING
WAITING
WAITING_FOR_CAPABILITY
CAPABILITY_REQUESTED
WAITING_FOR_USER
PAUSED
RESUMED
VERIFYING
COMPLETED
FAILED
REVOKED
TERMINATED
```

The exact state model can evolve.

State transitions must be observable and auditable.

---

# 23. SCHEDULER

Persistent agents should not continuously run the LLM.

Support:

```
wake
execute
sleep
```

Example:

```
08:00
  wake

08:01
  check condition

08:02
  no change

08:02
  sleep

next event
  wake again
```

Investigate:

* scheduled execution
* delayed tasks
* retries
* exponential backoff
* task deadlines
* leases
* wake conditions

---

# 24. EVENT-DRIVEN AGENTS

Add an event model.

Possible events:

```
email.received
calendar.changed
price.changed
webhook.received
timer.expired
user.message
tool.completed
capability.changed
approval.received
capability.revoked
```

Architecture:

```
                     EVENTS
                        |
              +---------+---------+
              |         |         |
            Email     Timer     Webhook
              |         |         |
              +---------+---------+
                        |
                        v
                    EVENT BUS
                        |
                        v
                 AGENT WAKE-UP
                        |
                        v
                     AGENT
```

The agent should only wake when necessary.

---

# 25. PERSISTENT AGENT STATE

Long-running tasks require durable state.

Represent:

```
task
objective
constraints
capability version
current plan
previous actions
observations
memory references
pending actions
approvals
failures
current state
next wake condition
```

The system must survive:

```
application closure
process restart
worker restart
network interruption
```

where practical.

---

# 26. PERSISTENT ENVIRONMENT

Investigate the concept of a persistent execution environment.

A production personal agent may have:

```
browser
filesystem
credentials
network
applications
state
```

inside a dedicated secure environment.

AgentOS should support a secure environment abstraction.

Conceptually:

```
Agent
  |
  v
Secure Environment
  |
  +-- Filesystem
  +-- Browser
  +-- Network
  +-- Processes
  +-- Credentials
  +-- Tools
```

The implementation can begin with containers/sandboxing.

Research stronger isolation where appropriate.

---

# 27. THE AGENT IS NOT THE APP

Maintain this conceptual distinction:

```
APPLICATION
    =
user interface
```

while:

```
AGENT
    =
persistent runtime + model + state + tools + environment
```

Closing the application must not necessarily terminate the agent.

The runtime must have its own lifecycle.

---

# 28. NEW RESEARCH DIRECTION

The addition of capability discovery creates a larger research question:

> Can an autonomous agent discover and compose the minimum capabilities required to accomplish a human goal while continuously proving that every action remains within the authority granted for that goal?

Potential research areas:

```
capability retrieval
capability graphs
tool composition
minimum-authority planning
capability-aware failure
dynamic provider selection
capability versioning
capability manifest security
capability gap detection
persistent authorization
long-running agent security
```

---

# 29. NEW RESEARCH QUESTIONS

Investigate:

### RQ11

Can agents reason over abstract capabilities rather than concrete tool names?

### RQ12

Can capability retrieval reduce tool-selection complexity compared with exposing large numbers of tools directly?

### RQ13

Can multi-signal capability retrieval improve tool selection compared with semantic similarity alone?

### RQ14

Can capability graphs improve multi-tool planning?

### RQ15

Can minimum-authority planning reduce agent risk while maintaining task success?

### RQ16

Can capability gap analysis reduce hallucinated task completion?

### RQ17

Can abstract capability interfaces make agents portable across different tool providers?

### RQ18

Can capability metadata itself become a security attack surface?

### RQ19

Can persistent agents maintain authorization correctness across restarts, scheduled wake-ups and distributed execution?

### RQ20

Can capability revocation remain reliable across long-running and distributed agent tasks?

Do not assume these questions have positive answers.

Design experiments.

---

# 30. NEW EXPERIMENTS

Add benchmark categories for:

```
tool overload
tool ambiguity
malicious tool descriptions
poisoned manifests
missing capabilities
incorrect provider selection
excessive capability selection
unnecessary high-risk tools
capability substitution
capability version changes
stale capabilities
persistent-task authorization
restart authorization
scheduled-task authorization
revocation during sleep
revocation during execution
```

---

# 31. TOOL-SELECTION BENCHMARK

Create scenarios where the agent must select capabilities.

Compare:

```
Raw tool selection
```

versus:

```
Capability retrieval
```

versus:

```
Capability graph planning
```

Measure:

```
task success
invalid tool calls
unnecessary tool calls
authorization violations
capability breadth
latency
token usage
cost
```

This should become part of the research benchmark.

---

# 32. MINIMUM-AUTHORITY BENCHMARK

Given a goal:

```
"Find flight prices."
```

Provide several possible tools:

```
browser.full
flight.search
browser.search
airline.api
```

Measure whether the planner chooses the minimum sufficient authority.

Track:

```
capability count
permission breadth
data access
network access
risk
task success
```

---

# 33. MISSING-CAPABILITY BENCHMARK

Give the agent tasks that cannot actually be completed.

Example:

```
User:
"Book a restaurant."
```

Available:

```
restaurant.search
restaurant.availability
```

Missing:

```
restaurant.booking
```

Measure whether the system:

```
correctly detects the gap
avoids hallucinated completion
explains the limitation
proposes alternatives
requests additional capability appropriately
```

---

# 34. LONG-RUNNING AGENT BENCHMARK

Test:

```
create task
  ↓
execute
  ↓
pause
  ↓
restart runtime
  ↓
wake later
  ↓
continue
```

Measure:

```
state consistency
capability consistency
authorization consistency
stale capability usage
revocation correctness
duplicate actions
recovery
```

---

# 35. TOOL VERSION SECURITY BENCHMARK

Test what happens when:

```
tool version 1
    ↓
tool version 2
```

and version 2 requests additional privileges.

The system must detect:

```
capability diff
policy diff
risk diff
```

and prevent silent privilege expansion.

---

# 36. EXTENDED RESEARCH ARCHITECTURE

The complete conceptual architecture is now:

```
                     HUMAN GOAL
                          |
                          v
                   INTENT ENGINE
                          |
                          v
                REQUIRED CAPABILITIES
                          |
                          v
              +----------------------+
              | CAPABILITY PLANE     |
```
