# Agent runtime

> Status: draft.

## The agent is not the app

```
APPLICATION = user interface
AGENT       = persistent runtime + model + state + tools + environment
```

Closing the app does not necessarily terminate the agent. The runtime has its own lifecycle and
keeps working remotely; the client reconnects to observe and approve.

## Runtime API (planned)

`create_agent()`, `create_task()`, `execute()`, `pause()`, `resume()`, `terminate()`.
See [interfaces.md](interfaces.md).

## Task state machine

CREATED, PLANNING, RUNNING, WAITING, WAITING_FOR_CAPABILITY, CAPABILITY_REQUESTED,
WAITING_FOR_USER, PAUSED, RESUMED, VERIFYING, COMPLETED, FAILED, REVOKED, TERMINATED.

Every transition is observable and audited. Illegal transitions are rejected. The model evolves
through ADRs.

## Durable task state

Each task persists: task, objective, constraints, capability version, current plan, previous
actions, observations, memory references, pending actions, approvals, failures, current state, next
wake condition. It must survive application closure, process restart, worker restart and network
interruption where practical. Resumption must not replay side effects (idempotency keys and the
action journal, see [verification.md](verification.md)).

## Scheduler: wake, execute, sleep

```
08:00 wake -> 08:01 check condition -> 08:02 no change -> sleep -> next event -> wake
```

Persistent agents do not run the model continuously. To investigate: scheduled execution, delayed
tasks, retries, exponential backoff, deadlines, leases on work items, wake conditions.

## Events

```
 Email   Timer   Webhook   ...
   +-------+-------+
           v
       EVENT BUS -> AGENT WAKE-UP -> AGENT
```

Event types include: `email.received`, `calendar.changed`, `price.changed`, `webhook.received`,
`timer.expired`, `user.message`, `tool.completed`, `capability.changed`, `approval.received`,
`capability.revoked`. The agent wakes only when necessary. Every event carries provenance and is
data, never authority (RQ22).

## Persistent secure environment

A production personal agent may have a browser, filesystem, credentials, network, applications and
state inside a dedicated environment:

```
Agent -> Secure Environment
           +-- Filesystem  +-- Browser  +-- Network
           +-- Processes   +-- Credentials  +-- Tools
```

Start with containers and sandboxing; study stronger isolation. See [isolation.md](isolation.md).

## Absent-user authorization

When the agent wakes at 03:00 and needs a capability it does not hold, the user is not there.
Options to compare (RQ19): deny and queue the request; pre-authorized conditional grants defined at
task creation; a delegate approver; degrade to read-only. Each is measured for unsafe actions,
task delay and user burden.

## Multi-agent

```
Planner Agent -> Research Agent -> Coding Agent
```

No capability inheritance by default. Each child gets an attenuated, explicit subset (I9). To
study: delegation, attenuation, agent identity, sub-agent permissions, cross-agent data flow,
capability transfer, delegation chains.

## Minimal first runtime (P1)

```
LLM (scripted or real) -> Planner -> Tool Gateway -> Tool -> Result
```

In-process, in-memory state machine and event bus, three typed tools (filesystem, mock HTTP, mock
mail), no persistence yet. Persistence arrives in P2, the scheduler and wake-sleep in P9.
