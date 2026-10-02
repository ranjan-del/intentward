# Glossary

| Term | Meaning in IntentWard |
|---|---|
| Harness | The infrastructure around a model: context, memory, tools, state, permissions, sandbox, verification, observability, approval |
| Capability grant | Authority to perform an action on a resource with given arguments, for a purpose, until an expiry |
| Capability description | What a tool or provider can do, as stated in its manifest. Never authority |
| Capability set / version | The immutable set of grants an agent operates under; changes create a new version |
| Transition | The explicit, audited change from version N to N+1 |
| Lease | A grant with a time or use limit |
| Revocation | Removing a grant live, including from running sessions |
| Attenuation | Deriving a narrower grant from a broader one, for example for a sub-agent |
| Reference monitor | The component every tool call must pass, which the agent cannot bypass or modify |
| Tool gateway | IntentWard's reference monitor for tool calls |
| Capability plane | Registry, manifests, discovery, graph, composition, gap analysis, provider resolution |
| Abstract capability | A provider-independent action such as `calendar.create` |
| Provider resolution | Choosing the concrete tool that implements an abstract capability |
| Gap analysis | Finding which required capabilities are missing or blocked for a goal |
| Capability-aware failure | Saying exactly which capability is missing instead of pretending success |
| Minimum-authority planning | Choosing the plan that meets the goal with the least authority |
| Context firewall | Provenance labels on context, enforced where data flows into actions |
| Taint | A mark on data derived from untrusted sources |
| Authorized misuse | An attack that abuses authority the agent already legitimately holds |
| Intent drift | The trajectory moving away from the original objective |
| Blast radius | What a compromised agent could reach, measured from its grants |
| Absent-user authorization | Policy for decisions needed while the user is not available |
| Attack World | The synthetic, deterministic benchmark environment |
| Scripted adversarial model | A fake model that always attempts maximal escalation, for exact enforcement tests |
