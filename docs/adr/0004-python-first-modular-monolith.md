# ADR 0004: Python first, one repository, modular monolith

Status: accepted (2026-10-02)

## Context

The brief lists Python, Go and TypeScript and many services. Premature services and languages slow research.

## Decision

Start in Python as one package with modules per component. Go, TypeScript or separate services are introduced only with a measured reason recorded in a new ADR.

## Consequences

Fast iteration and one test suite. The gateway may later need a rewrite if it becomes a bottleneck.
