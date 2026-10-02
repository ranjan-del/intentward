# Security

`intentward/security/` · Control and trust planes · Phase P1 to P10

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Authority: what the agent is allowed to do, how that changes, and how it is enforced, independent of the model.

## Modules

| Module | Purpose |
|---|---|
| [`capability/`](capability/README.md) | Authority grants. |
| [`policy/`](policy/README.md) | Deterministic policy evaluation over capability, resource, scope, rate and expiry. |
| [`identity/`](identity/README.md) | Identities for users, agents, sub-agents, tools and providers, and the binding of capabilities to task and session. |
| [`authorization/`](authorization/README.md) | The capability transition protocol: CURRENT, CHANGE REQUEST, POLICY EVALUATION, USER NOTIFICATION, APPROVE or DENY, NEW VERSION, ACTIVE. |
| [`context_firewall/`](context_firewall/README.md) | Provenance and trust for every context object, enforced where data leaves the model: values from untrusted sources cannot become recipients, URLs, paths or other authority-bearing arguments without approval. |
| [`tool_gateway/`](tool_gateway/README.md) | The reference monitor. |
| [`delegation/`](delegation/README.md) | Sub-agents receive attenuated capabilities, never inherited by default. |
| [`revocation/`](revocation/README.md) | Live revocation: invalidate the capability, reject future calls, stop or pause running operations, revoke temporary credentials and network access, invalidate tokens, update state, record, notify, and make sure revoked authority cannot reappear. |
