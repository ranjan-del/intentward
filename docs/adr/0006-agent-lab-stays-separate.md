# ADR 0006: agent-lab stays a separate project

Status: accepted (2026-10-02)

## Context

agent-lab already has a deterministic policy gate, a scripted model and traced runs for a calendar and transcript workflow agent.

## Decision

Keep agent-lab separate. IntentWard records it as prior work and reuses its lessons (scripted model, gate traces, proving refusals), not its code.

## Consequences

No coupling between the projects; some ideas are reimplemented in IntentWard's more general model.
