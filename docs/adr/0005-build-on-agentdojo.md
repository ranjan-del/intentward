# ADR 0005: Build the benchmark on AgentDojo, extend only where it lacks coverage

Status: accepted (2026-10-02)

## Context

A benchmark built from scratch is expensive and its results are not comparable to published defenses.

## Decision

Run IntentWard as a defense inside AgentDojo through interop/agentdojo. Add our own Attack World scenarios only for capability transitions, revocation, delegation, discovery, manifests and the persistent lifecycle.

## Consequences

Results are comparable with prior work. We depend on AgentDojo's task design and must track its versions.
