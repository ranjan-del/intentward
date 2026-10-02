# Who it is for and what it is useful for

> Status: intended uses. None of these is available yet; see [ROADMAP.md](../ROADMAP.md).

## Why this matters for AI work in general

| Today's pain | What IntentWard is meant to give |
|---|---|
| Agents are given broad tokens because scoping is tedious | Capabilities compiled from the task, scoped to resources and arguments |
| "Allow this tool?" prompts that nobody reads | Versioned requests that show what changes, why, and what triggered the request |
| Prompt injection defenses that rely on the model behaving | Enforcement below the model, measured against a benchmark |
| Agents that say "done" when they are not | Gap analysis, capability-aware failure and verification |
| Hundreds of MCP tools in one context | Retrieval of the few relevant capabilities, preferring least authority |
| Background agents nobody can stop cleanly | Leases, revocation and re-validation on every wake |
| No way to compare the safety of two agent setups | A shared capability-escape benchmark and blast-radius report |

## Open-source users

| User | How they would use it |
|---|---|
| Agent framework and harness authors | Embed the gateway, capability model and transition protocol through the SDK instead of writing ad hoc permission code |
| MCP host and server builders | Put the capability plane in front of MCP servers: manifests, version diffs, authorization-aware retrieval |
| Teams deploying internal agents | Run agents with intent-scoped authority, approvals and audit; get a blast-radius report per agent |
| Security researchers | Use the Attack World and scenarios to test new attacks and defenses reproducibly |
| Benchmark and evaluation people | Compare defenses on capability escape, revocation, delegation and discovery, not only injection success |
| Educators and learners | A readable reference implementation of capability security applied to LLM agents |
| Personal-agent builders | The persistent runtime pattern: state machine, scheduler, events, absent-user policy |

## Uses at ISPF (the owner's organization)

These are candidate internal uses, to be validated once the runtime exists. Internal names and
systems are deliberately not listed in this public repository.

| Candidate use | What IntentWard would bound |
|---|---|
| Coding agents working across many product repositories | Read freely; write only on feature branches of the repo in the task; never merge, deploy or touch production without an explicit transition |
| Deploy safety | Deploys become high-risk capabilities that always require a fresh approval, never inherited from an earlier one |
| Issue-tracker and chat automation (triage bots that read reports and open fixes) | Reading is free; posting, filing and opening pull requests are scoped, approved and audited; content from reports is untrusted data |
| Scheduled maintenance agents (sync jobs, nightly checks) | Leases and re-validation on each wake; revocation that stops them cleanly |
| Agents that handle credentials | Secrets stay in the broker; agents get short-lived, call-scoped credentials |
| Data access for analytics or admin tasks | Per-collection, per-operation grants with a blast-radius report before running |

## What it is not for

Building the end-user assistant itself, replacing an identity provider, or guaranteeing safety on a
compromised host.
