# Vision

> Status: planning. This document states the problem and the thesis. It claims nothing about results.

## The central question

**How much autonomy can we safely give an AI agent when its authority is dynamically derived from
human intent and enforced independently of the model?**

The umbrella question, after the capability-plane extension:

**Can an autonomous agent discover and compose the minimum capabilities required to accomplish a
human goal while continuously proving that every action remains within the authority granted for
that goal?**

The deepest direction is not "how do we stop bad agents?" It is: how do we construct AI systems
whose ability to affect the external world is formally and dynamically bounded by the authority
derived from human intent?

## The core philosophy

The model is a powerful but **untrusted** decision-making component.

| The model can | The system decides |
|---|---|
| Reason, plan, propose actions | What the agent can see and do |
| Choose tools and interpret information | Which resources, tools and arguments are allowed |
| Generate code | Which network destinations and secrets are reachable |
| Request more capabilities | How long a permission lasts and how many uses it has |
| | Whether a high-risk operation needs approval |
| | Whether a capability is revoked |
| | Whether an action is consistent with the original task |
| | Whether the resulting state matches the intended result |

```
MODEL DECIDES  ->  SYSTEM AUTHORIZES  ->  SANDBOX ENFORCES  ->  SYSTEM VERIFIES
```

The model is never the final security boundary. Prompts guide behaviour; they do not authorize.

## What a harness is here

A harness is the software infrastructure around a model that provides the environment it operates
in: context, memory, tools, planning, state, execution, permissions, policies, sandboxing,
networking, verification, observability, recovery, feedback and human approval. The model is the
reasoning engine. The harness is the controlled operating environment. IntentWard is a harness and
runtime, not an agent application.

## The agent is not the app

The application is a user interface. The agent is a persistent runtime plus model, state, tools and
environment. Closing the app does not have to stop the agent, which is why authority has to be
explicit, scoped, leased, revocable and re-validated every time the agent wakes up.

## The fundamental property

**No implicit authority escalation.** An agent never gains authority because a model, webpage,
document, tool output, memory entry, other agent, sub-agent, external API, prompt injection or
environment change asked for it, or because the model decided it "needed" it.

```
environmental_input != authorization
model_output        != authorization
tool_output         != authorization
manifest_claim      != authorization
event               != authorization
```

Only an explicit, authorized, audited transition changes authority. The precise formal statement
(capability monotonicity, roughly `C(t+1) is a subset of AuthorizedCapabilities(t+1)`) is a research
output, not an assumption. See [security-model.md](security-model.md).

## Long-term picture

```
HUMAN INTENT -> INTENT REPRESENTATION -> CAPABILITY COMPILATION -> POLICY / AUTHORITY
   -> CAPABILITY PLANE (discover, retrieve, compose, find gaps)
   -> AGENT RUNTIME (memory, tools, context, state, scheduler, events)
   -> ENFORCEMENT -> EXECUTION -> VERIFICATION -> AUDIT -> HUMAN OVERSIGHT
```

The agent is powerful because it has useful capabilities. It stays controlled because those
capabilities are explicit, scoped, temporary, auditable, revocable, independently enforced and tied
to intent.

## What "unique" means for this project

IntentWard does not train a new language model. The "model" it contributes is a **model of
authority**: how intent becomes capabilities, how capabilities are discovered and composed, how they
change, and how they are enforced, revoked and measured. The contribution is that model, a runtime
that implements it, and a benchmark that tries to break it. See [positioning.md](positioning.md).
