# ADR 0007: Offline first, with a scripted adversarial model

Status: accepted (2026-10-02)

## Context

LLM runs are non-deterministic and need API keys, which makes enforcement tests flaky and CI dependent on secrets.

## Decision

The test suite and the worst-case benchmark run with no API key, using a scripted adversarial model that always attempts maximal escalation. Real models are an optional path for average-case experiments, run N times with seeds and confidence intervals.

## Consequences

Enforcement results are exact and reproducible. Average-case results stay statistical and are labelled as such.
