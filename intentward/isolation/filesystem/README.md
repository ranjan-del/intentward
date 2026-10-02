# Filesystem

`intentward/isolation/filesystem/` · Execution plane · Phase P1 (canonicalization), P8, P11 (snapshots)

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Path canonicalization, symlink and TOCTOU-safe access, read-only mounts, ephemeral workspaces, snapshots for rollback.

| | |
|---|---|
| Owns | Safe path resolution used by the gateway, overlay snapshots. |
| Does not own | Deciding which paths are allowed (security/). |
| Research questions | RQ26, RQ27 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I3 (see [security model](/docs/security-model.md)) |
| Main risk | Symlink swaps between check and use. |
