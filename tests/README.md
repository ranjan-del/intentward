# Tests

> Status: planned.

The authorization boundaries are tested here before anything else.

| Directory | Purpose |
|---|---|
| `invariants/` | One test module per invariant, I1 to I10. |
| `adversarial/` | Scripted adversarial model runs that attempt every escalation path. |
| `property/` | Property-based tests (Hypothesis): paths, globs, symlinks, TOCTOU, transition sequences. |
