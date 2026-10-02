# Security Policy

## Supported versions

IntentWard is pre-alpha and has not made a release. No version is supported for production use, and
nothing in this repository should be relied on as a security boundary yet.

## Reporting a vulnerability

Please do not open a public issue for a security problem.

Report it through GitHub's private vulnerability reporting on this repository. Include what you
found, how to reproduce it, and the impact you expect. You will get an acknowledgement, and an
assessment once the report has been reviewed.

## Scope

When implementation exists, the areas that matter most are:

- The tool gateway and policy evaluation (any bypass is critical)
- The capability store and transition protocol (any silent change of authority is critical)
- Revocation (any use of revoked authority)
- The secrets broker and audit log

## The benchmark is not an attack tool

The Attack World targets only synthetic, local environments. Contributions that attack real
external systems will not be accepted.
