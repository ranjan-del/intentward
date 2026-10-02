# Intelligence

`intentward/intelligence/` · Agent plane · Phase P1, P5, P7

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Everything model-shaped: model adapters, memory, context assembly and the intent engine. All of it is untrusted from the security point of view.

## Modules

| Module | Purpose |
|---|---|
| [`llm/`](llm/README.md) | Model adapters behind one interface, plus the scripted adversarial model used to test enforcement deterministically. |
| [`memory/`](memory/README.md) | Agent memory where every item carries source, trust, timestamp, task, tenant, provenance, sensitivity and authority level. |
| [`context/`](context/README.md) | Builds the model context from labelled context objects (system policy, user, app state, memory, RAG, web, email, tool output, sub-agents). |
| [`intent/`](intent/README.md) | Compiles a human goal into a structured task: objective, resources, required, optional and prohibited actions, risk, expected outputs, duration, constraints, and the abstract capabilities required. |
