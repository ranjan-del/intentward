# Contributing

IntentWard is a research project and a security system, so contributions are judged by two extra
standards: evidence and boundaries.

## Rules that are not negotiable

These apply to the maintainer as strictly as to anybody else.

1. **No number without a run.** Every number in code, docs, commits or pull requests links to a
   recorded run in `research/experiments/`.
2. **No guarantee without a test.** A security property is claimed only with the tests that show
   it and the assumptions it depends on.
3. **Model output never mutates authorization state.** Any change that lets model output, tool
   output, retrieved content, memory, events or manifests change authority without a transition is
   a critical defect.
4. **Every authorization boundary has adversarial tests**, not only happy-path tests.
5. **Name the prior art.** Before calling anything new, update [docs/landscape.md](docs/landscape.md).
6. **Negative results are results.** Keep them, in `research/failed_experiments/` when they
   contradict a hypothesis.
7. **Keep components switchable** so they can be ablated.

## Getting set up

Setup instructions arrive with phase 1. There is no code to run yet.

## Style

- Conventional commits: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- No em dashes in documentation or commit messages
- Strong typing and schemas for every security-sensitive object
- Tests pass with no API key and no network

## Design changes

Anything that changes the capability model, the transition protocol, an invariant or the evaluation
method needs an ADR in [docs/adr/](docs/adr/README.md) before the code.

## Licensing

Contributions are accepted under the Apache License 2.0. See [LICENSE](LICENSE).
