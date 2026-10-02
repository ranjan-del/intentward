# Verification, transactions and intent drift

> Status: draft.

## Never trust a claim of completion

"I deleted the temporary file" is checked against the filesystem. Git claims against the
repository, database claims against the transaction, API claims against the actual response or
state, deployment claims against deployment state.

```
PLAN -> AUTHORIZE -> EXECUTE -> VERIFY -> COMMIT
PLAN -> SIMULATE -> AUTHORIZE -> EXECUTE -> VERIFY -> COMMIT      (high risk)
```

Postconditions come from the capability manifest (see [capability-plane.md](capability-plane.md)).

## Transactional thinking

Questions: can actions be previewed, rolled back, committed only after verification? What happens
when step 4 fails? How do we recover from partial execution and avoid inconsistent state?

Mechanisms to explore: checkpoints, snapshots (overlay filesystems), transactional tool execution,
compensating actions (for external effects that cannot be undone), rollback, idempotency keys,
action journals. Irreversible actions are labelled as such and never presented as reversible.

## Intent drift

The task is represented as a structured objective ("fix authentication bug") and the trajectory as
a sequence (inspect repo, inspect auth module, run tests, modify auth code, run tests, commit fix).

Questions per action: is it authorized, in scope, advancing the goal, necessary, unusually risky,
reversible, introducing a new objective, moving into unrelated resources?

| Layer | Checks | Role |
|---|---|---|
| Deterministic | Capability, resource, scope, policy, rate, expiry | Enforcement |
| Semantic | Goal and action relationship, trajectory, drift, unusual behaviour | Detection only; can trigger pause |

Semantic monitoring is never the only security layer, and an LLM judge is never the final authority.
