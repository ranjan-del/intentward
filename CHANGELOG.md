# Changelog

All notable changes to this project are documented here.
The format follows Keep a Changelog, and this project adheres to Semantic Versioning.

## [Unreleased]

### Added

- Flagship documentation standard: a "Project documentation" table in the README covering README, Architecture, Design decisions, Benchmarks, Failure cases, Evaluation, Trade-offs, Deployment, Cost and Future work, with stub documents for the sections not yet written
- Vision, positioning, and the problem statement: 23 limitations that grow with scale
- Architecture with six planes (control, agent, capability, execution, trust, research) and the persistent agent lifecycle
- Capability (authority) model: grants, versions, transitions, integrity, leases, revocation, delegation, blast radius
- Capability plane: registry, manifests, multi-signal retrieval, capability graph, composition, provider resolution, gap analysis, minimum-authority planning, versioning, discovery security
- Security model with invariants I1 to I10, and a first threat model
- Context security, agent runtime, isolation, verification and benchmark designs
- Experiment protocol and honesty rules
- Research plan with 31 consolidated research questions and 10 falsifiable hypotheses
- Landscape of prior art to verify, use cases, interfaces, tech stack, limitations, glossary
- ADRs 0001 to 0009
- Repository tree with a README for every module
- The original planning briefs, preserved in docs/brief/
- Roadmap: phases P0 to P12 mapped to releases v0.1.0 to v1.0.0

### Notes

- No implementation code yet, and no measured numbers anywhere in the repository
