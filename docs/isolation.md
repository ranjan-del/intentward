# Isolation: sandbox, network, secrets

> Status: draft. This is mostly engineering. We integrate existing mechanisms and measure them.

## Sandbox

To investigate: Linux namespaces, cgroups, seccomp, filesystem isolation, network namespaces,
container isolation, read-only filesystems, ephemeral workspaces, process, CPU, memory and disk
limits, network allowlists, DNS restrictions, secret isolation.

| Option | Notes |
|---|---|
| Docker / OCI containers | Convenient first implementation. Not the complete security boundary on its own. |
| bubblewrap, nsjail | Lightweight namespace sandboxes; good for per-tool profiles |
| gVisor | User-space kernel; stronger syscall isolation |
| Firecracker microVMs | VM-level isolation for the persistent environment |
| Landlock, seccomp-bpf | Fine-grained filesystem and syscall restriction |
| macOS sandbox profiles | For local development on the owner's machine |

Sandbox profiles are derived from the capability set and must not be broader than it.
Sandboxing does not solve prompt injection; it bounds what injected code can touch.

## Network

```
network: DENIED
# or
network:
  allowed_domains: [api.example.com]
```

The agent must not reach the arbitrary internet, scan hosts, contact arbitrary IPs or exfiltrate to
unknown destinations. To investigate: egress policies, DNS filtering, proxy-based control, domain
allowlists, IP restrictions, request budgets, network identity, TLS interception only in controlled
environments. An allowed domain is still an exfiltration channel; the context firewall covers what
data may flow there.

## Secrets

The model never automatically sees secrets. Instead of `AGENT_HAS_AWS_SECRET`:

```
agent requests: cloud.storage.write  bucket=project-output
gateway obtains: the minimum short-lived credential for that call
```

To investigate: secret brokers, temporary credentials, secret-scoped capabilities, environment
isolation, redaction, rotation, short-lived tokens.

## Process limits

CPU, memory, disk, process count and wall-clock limits per task, to bound loops and resource
exhaustion.
