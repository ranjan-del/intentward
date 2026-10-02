# Action verifier

`intentward/verification/action_verifier/` · Trust plane · Phase P11

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Checks each action against its declared postconditions (file deleted, event created, transaction committed).

| | |
|---|---|
| Owns | Postcondition checks for typed tools. |
| Does not own | General program verification (out of scope). |
| Research questions | RQ25 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I10 (see [security model](/docs/security-model.md)) |
| Main risk | False assurance from weak postconditions. |
