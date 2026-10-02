# Capability (authority) model

> Status: draft. Schemas here are examples to start from, not final. They evolve through ADRs.

Two different things share the word "capability" in this project:

| Term | Meaning | Lives in |
|---|---|---|
| **Capability grant** (authority) | What this agent is allowed to do, on what, with which arguments, until when | `security/capability` |
| **Capability description** (ability) | What a tool or provider can do, its inputs, outputs, side effects | `capability_plane/` |

This document is about grants. The [capability plane](capability-plane.md) is about descriptions.
A description never becomes a grant (I7).

## A grant has many dimensions

action, resource, resource scope, arguments, identity, purpose, time, expiry, rate limit, network
restrictions, sensitivity, tenant, task id, capability version, parent capability, approval state.

```yaml
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
  expires: 2026-10-02T01:00:00Z
  version: 42
```

`delete(/workspace/project/temp.txt)` is allowed. `delete(/workspace/project/.env)` and
`delete(database.db)` are denied, although all three are "delete" (I3). Argument validation exists
throughout the system, not only at the tool name.

## Capability sets are immutable and versioned

```
Capability Version 17
  filesystem.read    /project/docs/*
  filesystem.write   /project/output/*
  filesystem.delete  NONE
```

A request for `filesystem.delete /project/docs/a.pdf` never mutates version 17. It creates
version 18 with status PROPOSED.

```
CURRENT -> REQUESTED --+--> REJECTED
                       v
                    PENDING -> REVIEW --+--> DENIED
                                        v
                                    APPROVED -> NEW VERSION -> ACTIVE
```

## Transition protocol

```
CURRENT -> CHANGE REQUEST -> POLICY EVALUATION -> USER NOTIFICATION
        -> APPROVE / DENY -> NEW CAPABILITY VERSION -> ACTIVE
```

Until a decision exists the agent PAUSES or the call is DENIED, depending on policy. Never a
silent expansion.

**What the user sees for a request:**

```
------------------------------------------------
CAPABILITY CHANGE REQUEST
Agent wants to:
+ DELETE  /project/docs/a.pdf
Reason (written by the agent, untrusted): "Remove temporary file."
Triggered after: reading web page docs.example.com (UNTRUSTED)
Current permission: DELETE = DENIED
Proposed version: 18   Expires: 10 min   Scope: exact path
[ Reject ]                         [ Approve ]
------------------------------------------------
```

The user can always see the old set, the requested change, the reason, affected resources, risk,
agent state, the proposed new set, expiry, scope, and **the causal provenance of the request**
(what the agent had just read). The "Triggered after" line is our addition (RQ5).

**Every transition is logged with:** timestamp, agent, task, old capability set, requested
capability, reason, affected resources, risk, user identity, decision, new version, expiry.

## Capability integrity

Once version N is active, the agent operates under N. Anything that wants more authority must
produce N+1 through the authorization mechanism. Nothing inside the untrusted agent environment can
mutate the active state.

Candidate mechanisms, each to be evaluated for its tradeoffs rather than adopted by default:

| Mechanism | Buys | Costs |
|---|---|---|
| Immutable capability ids and content hashes | Cheap integrity, easy diffing | None significant |
| Append-only, hash-chained transition log | Tamper evidence and replay | Not tamper-proof against a compromised host |
| Signed capability manifests | Integrity across process or machine boundaries | Key management |
| Short-lived capability tokens | Bounded staleness, revocation by expiry | Renewal traffic |
| Lease-based capabilities | Automatic expiry | Renewal UX, interruptions |
| Cryptographic binding to task and session | Prevents replay in another task | Identity plumbing |
| Caveat-style tokens (macaroon, Biscuit, UCAN style) | Offline attenuation and delegation | Token complexity |

## Leases

```
filesystem.read  resource=/project/docs  duration=10 minutes  max_calls=100
```

After expiry the grant is invalid. To investigate: expiry, single-use grants, call limits,
resource limits, risk-based lease duration, automatic revocation, renewal, and leases across sleep
and wake (RQ20). The model can never alter its own lease.

## Revocation

On `revoke(capability)` the runtime must: invalidate it, reject future calls, stop or pause affected
operations where possible, revoke temporary credentials, cut relevant network access, invalidate
tokens, update runtime state, record the event, notify agent and user, and make sure the authority
cannot reappear (I4). Revocation latency is bounded below by the TTL of any credential an external
system will not revoke early; the system states that bound rather than hiding it (RQ21).

## Delegation and attenuation

A child agent never receives the parent's whole authority. It receives an explicitly attenuated
subset (I9). To investigate: delegation, attenuation, agent identity, sub-agent permissions,
cross-agent data flow, capability transfer, delegation chains (RQ23).

## Capability graph and blast radius

Permissions may be better modelled as a graph than a flat list:

```
AGENT
  +-- FILESYSTEM -- PROJECT -- READ, WRITE
  +-- GITHUB ----- REPOSITORY -- READ, ISSUE, PR
```

This would allow analysis of authority propagation, privilege paths, blast radius, inheritance and
dependencies. Whether the graph beats a flat list is itself a question (RQ31). Blast radius must be
measurable, not a subjective score:

```
Agent Capability Report
Filesystem:  READ /project/*   WRITE /project/output/*
Network:     api.example.com
Secrets:     none
Production:  none
Maximum reachable resources: (computed)
```

Dimensions: filesystem, network, secrets, production, databases, external APIs, cloud
permissions, deletion rights, financial actions.
