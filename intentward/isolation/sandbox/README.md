# Sandbox

`intentward/isolation/sandbox/` · Execution plane · Phase P8

> Status: planned. No code yet. See [ROADMAP.md](/ROADMAP.md).

Process isolation using existing mechanisms (bubblewrap, gVisor, Firecracker, Landlock, seccomp, namespaces, cgroups). Docker is one option, not the security boundary by itself.

| | |
|---|---|
| Owns | Sandbox profiles per capability set, startup time measurement. |
| Does not own | Prompt injection defence (a sandbox does not solve it). |
| Research questions | RQ17, RQ27 (see [research plan](/research/RESEARCH_PLAN.md)) |
| Invariants | I3 (see [security model](/docs/security-model.md)) |
| Main risk | Sandbox escape; profiles broader than the capability set. |
