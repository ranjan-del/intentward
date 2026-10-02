# Interfaces (planned)

> Status: names and shape only. Signatures are decided in the phase that implements them.

## Agent runtime API
`create_agent()` `create_task()` `execute()` `pause()` `resume()` `terminate()`

## Capability (authority) API
`compile_capabilities()` `request_capability()` `approve_capability()` `deny_capability()`
`revoke_capability()` `inspect_capabilities()` `diff_capabilities()` `delegate_capability()`

## Capability plane API
`register_capability()` `publish_manifest()` `discover_capabilities()` `resolve_provider()`
`compose_plan()` `analyze_gaps()` `diff_manifest()`

## Tool API
`register_tool()` `invoke_tool()` `inspect_tool()`

## Security API
`evaluate_policy()` `inspect_context()` `detect_drift()` `verify_action()` `blast_radius()`

## Benchmark API
`create_scenario()` `run_attack()` `evaluate()` `compare()`

## CLI

The CLI makes the security model visible.

```
intentward task create | inspect
intentward capabilities list | diff | revoke | request | approve | deny
intentward tools list | inspect | diff
intentward agent pause | resume | terminate
intentward security inspect | blast-radius
intentward benchmark run | compare
intentward trace show
```

## Dashboard (last)

Agent: task, state, capability version, tools, resources, risk. Capability changes:
OLD, REQUEST, USER DECISION, NEW. Runtime: actions, tool calls, blocked actions, policy decisions,
context provenance, sandbox status. Security: attack attempts, blocked and successful attacks,
violations, drift alerts. The UI is secondary to the runtime and research.
