# Threat model

> Status: first draft (P0 deliverable). Will be revised after the P0 literature review.

## System under protection

A persistent agent runtime that executes tasks for a user with tools across files, web, mail,
calendars, code repositories, databases and cloud APIs, possibly with sub-agents, possibly while the
user is absent.

## Assets

| Asset | Examples (all synthetic in the benchmark) |
|---|---|
| User data | Documents, mail, calendar, contacts, repositories |
| Secrets | API keys, OAuth tokens, cloud credentials |
| External state | Sent mail, created events, purchases, deployments, database rows |
| Authority state | Active capability version, leases, approvals, revocations, audit log |
| Availability | Compute, money, rate limits, user attention (approvals) |
| Integrity of results | What the agent reports it did |

## Trust boundaries

```
 TRUSTED                                  | UNTRUSTED
------------------------------------------+----------------------------------------
 User (authenticated, via client)         | The model and everything it outputs
 Policy and capability store              | Web pages, documents, mail, RAG results
 Reference monitor (gateway)              | Tool outputs, API responses
 Audit log writer                         | Tool manifests and descriptions
 OS sandbox                               | Memory contents (data, not authority)
                                          | Events and wake triggers
                                          | Sub-agents and other agents
                                          | Third-party tool providers
```

The host running the reference monitor is trusted. A compromised host is out of scope and listed
in [limitations.md](limitations.md).

## Attackers

| Attacker | Controls | Goal |
|---|---|---|
| A1 Content attacker | Any content the agent reads: web, documents, mail, RAG corpora | Exfiltrate, act, escalate (indirect prompt injection) |
| A2 Direct prompter | Text the user pastes or forwards | Same as A1, via the user's own prompt |
| A3 Malicious tool provider | A tool, its manifest, description, outputs, version updates | Gain authority, shadow other tools, exfiltrate |
| A4 Memory poisoner | Earlier sessions or shared memory | Persist instructions across tasks |
| A5 Peer agent | A sub-agent or other agent's messages | Obtain the parent's authority |
| A6 Event forger | Webhooks, mail arrival, other triggers | Wake the agent and steer it while the user is away |
| A7 Misbehaving model | The model itself (hallucination or adversarial behaviour) | Any of the above, without an external attacker |
| A8 Social engineer of approvals | The text of capability requests | Get the user to approve an escalation |

Not in scope: attackers with code execution on the host, supply-chain compromise of IntentWard
itself, physical access, side channels.

## Attack classes

Direct prompt injection; indirect prompt injection; tool poisoning; malicious tool descriptions;
memory poisoning; capability escalation; privilege escalation; data exfiltration; goal hijacking;
intent drift; unauthorized deletion; secret exposure; excessive agency; malicious tool output;
malicious retrieved documents; malicious webpages; sub-agent manipulation; resource exhaustion;
agent loop abuse; capability confusion; revocation bypass; poisoned manifests; tool shadowing;
capability impersonation; provider substitution; schema manipulation; version confusion; forged
events; approval social engineering; stale capability use after restart or sleep.

## Security properties

The invariants I1 to I10 in [security-model.md](security-model.md), plus these measured (not
guaranteed) properties: attack success rate, exfiltration rate, scope violation rate, revocation
latency, approval rate of malicious requests.

## Assumptions

1. The user's authenticated decisions are genuine (the client is not compromised).
2. The reference monitor and policy store are not writable from the agent environment.
3. The OS sandbox is correctly configured; sandbox escapes are measured, not assumed away.
4. External credentials can be scoped and short-lived; where they cannot, revocation latency is
   bounded by their TTL.
