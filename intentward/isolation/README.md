# Isolation

`intentward/isolation/` · Execution plane · Phase P8

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

The OS enforcement plane. Required because shell and code execution cannot be validated at the gateway. Reuses existing isolation technology rather than inventing it.

## Modules

| Module | Purpose |
|---|---|
| [`sandbox/`](sandbox/README.md) | Process isolation using existing mechanisms (bubblewrap, gVisor, Firecracker, Landlock, seccomp, namespaces, cgroups). |
| [`environment/`](environment/README.md) | A per-agent environment (filesystem, browser, network, processes, credentials, tools) that survives restarts, as a production personal agent would have. |
| [`filesystem/`](filesystem/README.md) | Path canonicalization, symlink and TOCTOU-safe access, read-only mounts, ephemeral workspaces, snapshots for rollback. |
| [`network/`](network/README.md) | Egress control: deny by default, domain allowlists, DNS filtering, proxy-based control, IP restrictions, request budgets, network identity, TLS interception only in controlled environments. |
| [`secrets/`](secrets/README.md) | The model never sees secrets. |
| [`process/`](process/README.md) | CPU, memory, disk and process-count limits, time limits. |
