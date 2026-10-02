# ADR 0008: Enforce provenance where data leaves the model

Status: accepted (2026-10-02)

## Context

Labelling context as untrusted does not stop a model from following it. Authorized misuse needs no escalation.

## Decision

The context firewall enforces provenance and taint on authority-bearing tool arguments (recipients, URLs, paths, commands, amounts), on capability requests and on memory writes, not on what the model is shown.

## Consequences

Adds lightweight information-flow tracking to the gateway. Some legitimate data use will be blocked; the false block rate is measured.
