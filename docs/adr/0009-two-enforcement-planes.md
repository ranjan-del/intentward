# ADR 0009: Two enforcement planes: gateway and OS

Status: accepted (2026-10-02)

## Context

Shell and code execution are Turing complete; argument validation at the gateway cannot cover them.

## Decision

Typed tools are enforced at the gateway. Anything that runs code is enforced by the OS sandbox with a profile no broader than the capability set. Shell is excluded from defended configurations unless confined.

## Consequences

The gap between the two planes becomes a research question (RQ27) instead of a hidden hole.
