# Capability plane: discovery, tool intelligence and composition

> Status: draft. Added by the capability-plane extension (see
> [brief/03-capability-plane-extension.md](brief/03-capability-plane-extension.md)). It adds a
> layer; it does not replace any part of the security architecture.

## The question

How can an agent discover the capabilities available to it, understand what each can and cannot
do, compose several to solve a human goal, recognize when required capabilities are missing, and
execute the resulting plan while staying inside the authority granted for that goal?

## From "pick one of 500 tools" to "which capability do I need?"

```
HUMAN GOAL
  -> REQUIRED CAPABILITIES      (intent engine)
  -> AVAILABLE CAPABILITIES     (registry, discovery)
  -> AUTHORIZED CAPABILITIES    (control plane)
  -> CONCRETE TOOLS             (provider resolution)
  -> EXECUTION                  (gateway, sandbox)
```

"Find when I am free next week and schedule a meeting with John" derives `contact.search`,
`calendar.read`, `calendar.availability` and `calendar.create`, then resolves each to a provider.

## MCP is not the capability system

MCP is one way to expose tools. IntentWard understands capabilities; MCP, REST, HTTP, gRPC, local
functions, internal services, browser automation, custom tools and cloud APIs are transports.

```
            CAPABILITY PLANE
                   |
         +---------+---------+
        MCP       REST      gRPC   ...
         |         |         |
       tools     tools     tools
```

## Registry and manifests

Each capability has structured metadata. Example (not the final schema):

```yaml
id: calendar.create_event
name: Create Calendar Event
description: Creates a calendar event for an authenticated user.
category: calendar
inputs:
  start_time: {type: datetime}
  end_time:   {type: datetime}
  title:      {type: string}
  attendees:  {type: list[email]}
outputs:
  event: {type: calendar_event}
capabilities_required: [calendar.write]
risk: {level: medium}
side_effects: [creates_external_event]
reversible: true
authentication: user_oauth
provider: google-calendar
protocol: mcp
```

A full manifest can describe: capability id, description, inputs, outputs, preconditions,
postconditions, required authorization, resource scope, side effects, reversibility, risk, network
requirements, authentication, data access, data sensitivity, provider, protocol, version,
availability, cost and latency characteristics.

**The manifest is security-sensitive metadata.** It describes; the policy authorizes (I7).

## Side-effect model

| Class | Example | Influences |
|---|---|---|
| READ | `calendar.search` | Ranking (preferred), low approval need |
| WRITE / external state change | `calendar.create` | Authorization, approval, verification |
| DELETE / destructive | `calendar.delete` | Approval, transactions, benchmark scoring |
| EXTERNAL ACTION | purchase, send | Approval, irreversibility handling |

Side effects feed planning, ranking, authorization, approval, verification and benchmark scoring.

## Tool retrieval ("RAG for tools")

Do not expose every tool to the model. Large tool lists raise context size, selection difficulty,
ambiguity, attack surface and tool-description pollution.

```
RAG:                    Query -> retrieve documents     -> LLM
Capability retrieval:   Goal  -> retrieve capabilities  -> planner / LLM
```

Retrieval is **not** only vector search. Candidate signals: semantic similarity, input
compatibility, output compatibility, authorization, risk, side effects, cost, latency,
availability, provider, user preferences, task constraints.

| Candidate | Semantic match | Authorization | Side effects | Risk |
|---|---|---|---|---|
| `flight.search` | high | allowed | none | low |
| `flight.book` | high | approval required | purchase | high |

To study: semantic retrieval, structured filtering, graph retrieval, symbolic constraints, hybrid
retrieval, compatibility matching and authorization-aware retrieval. RagFabric's multi-strategy
retrieval and routing lessons apply, without coupling the projects.

## Capability graph and composition

Edges: requires, produces, consumes, depends_on, enables, conflicts_with, requires_approval,
alternative_to.

```
"Book a trip": flight.search -> flight.details -> flight.select -> flight.book -> calendar.create
"Schedule with John": contact.search -> calendar.search -> calendar.availability -> intersect -> calendar.create
```

### Preconditions and postconditions

```
flight.details:  precondition flight_id exists            output flight_details
flight.book:     precondition flight_id exists,
                              authorized booking capability exists,
                              user approval exists
```

This is close to classical planning (STRIPS and PDDL operators) and to semantic web service
descriptions. Whether it reduces invalid tool calls with LLM planners is RQ12.

## Abstract capability vs concrete tool

The agent reasons about `calendar.create`, not `google_calendar_create_event_v2()`. The plane
resolves it to Google Calendar, Outlook, Apple Calendar or an internal calendar. This gives
portability, provider independence, cleaner planning, easier authorization, auditing and testing.
This is **not assumed to be novel** (Android intents, OWL-S and WSMO service discovery, OpenAPI
catalogues are prior art); see [landscape.md](landscape.md).

## Provider resolution

Selection considers availability, user authorization, cost, latency, reliability, required data,
risk and task constraints. The chosen provider still passes the authorization and policy system.

## Capability gap analysis

Given a goal, compute: required, available, authorized, missing, blocked, alternative capabilities
and alternative execution paths.

```
Goal: "Book a restaurant."
Required:  restaurant.search, restaurant.availability, restaurant.booking
Available: restaurant.search, restaurant.availability
Missing:   restaurant.booking      ->  GOAL = NOT FULLY EXECUTABLE
```

## Capability-aware failure

The agent says: "I cannot complete this task because the required restaurant.booking capability is
unavailable." It may offer legitimate alternatives: search and list restaurants, open the booking
interface, request an additional authorized capability, or ask the user to do the missing step.
It never fabricates success (I10).

## Minimum-capability planning

Find the least-authority plan that satisfies the goal.

| Goal: "Find flight prices" | Authority |
|---|---|
| Plan A: browser + login + full website access | broad |
| Plan B: `flight.search` API | narrow (preferred) |

Optimization dimensions: capability breadth, risk, side effects, number of tools, cost, latency,
data exposure, required credentials. This is least privilege applied at planning time (RQ13).

## Versioning

Track provider version, capability version, manifest version and policy version.

```
OLD  calendar.create  requires calendar.write
NEW  calendar.create  requires calendar.write, contact.read, email.send
```

A broader requirement triggers a capability and security review and never takes effect silently
(I8).

## Discovery is an attack surface

To research: malicious tool descriptions, poisoned manifests, deceptive capability descriptions,
malicious providers, tool shadowing, capability impersonation, provider substitution, schema
manipulation, version confusion, malicious metadata. A tool saying "this tool can read your files"
does not receive permission to read files (RQ16).

## How it plugs into the security architecture

```
HUMAN GOAL -> INTENT ENGINE -> REQUIRED CAPABILITIES -> CAPABILITY DISCOVERY
  -> CANDIDATE CAPABILITIES -> AUTHORIZATION FILTER -> CAPABILITY COMPILER -> AGENT PLANNER
  -> TOOL GATEWAY -> POLICY ENFORCEMENT -> SANDBOX -> EXECUTION -> VERIFY -> OBSERVE -> NEXT ACTION
```
