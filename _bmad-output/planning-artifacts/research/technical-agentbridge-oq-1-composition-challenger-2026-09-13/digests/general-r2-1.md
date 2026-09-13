# General protocols — round 2 digest

Accessed: 2026-09-13. Official primary sources only.

## Final screen

1. **A2A passes all hard gates.** Stable v1.0 released 2026-03-12; compatible patch v1.0.1 dated 2026-05-26. Linux Foundation-hosted, Apache-2.0, TSC governance, official multi-language repositories and current client/server SDKs; official integration-testing kit exists, though no universal certification matrix was verified. Sources: [A2A v1.0 announcement](https://a2a-protocol.org/dev/blog/2026/03/12/a2a-protocol-ships-v10-production-ready-standard-for-agent-to-agent-communication/), [A2A organization](https://github.com/a2aproject/), [A2A Python SDK](https://github.com/a2aproject/a2a-python). Publishers: A2A/Linux Foundation. Confidence: high; medium on certification completeness. Class: release/governance/SDK/conformance.

2. **MCP passes as a component, not full protocol.** It is legally an LF Projects series; core/spec contributions are Apache-2.0 and non-spec docs CC-BY-4.0. Current official SDK/conformance evidence supports 2026-07-28. `server/discover` is endpoint-local; Registry status remains preview/GA unverified and must not be mandatory. Sources: [MCP governance](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md), [MCP Registry](https://github.com/modelcontextprotocol/registry/blob/main/README.md), official release/SDK/conformance sources from round 1. Publisher: MCP/LF Projects. Confidence: high, medium-high on registry. Class: governance/discovery/conformance.

3. **BeeAI Agent Communication Protocol is cut as a separate candidate.** Its official repository says ACP is now part of A2A and directs migration; latest standalone release v1.0.3 is 2025-08-21. Historical semantics remain useful lineage, but it is no longer an independently advancing standard. Source: [BeeAI ACP](https://github.com/i-am-bee/acp). Publisher: BeeAI contributors. Confidence: high. Class: merger/status.

4. **AGNTCY is cut as one protocol but retained as component inventory.** DIR/SLIM are deployable Apache-2.0 components, yet no authoritative BOM/profile pins DIR, SLIM, Identity, OASF and adapters together. Observed DIR/SLIM versions are fragmented and operational dependencies material. Sources: [AGNTCY DIR docs](https://docs.agntcy.org/dir/architecture/), [DIR changelog](https://github.com/agntcy/dir/blob/main/CHANGELOG.md), [SLIM changelog](https://github.com/agntcy/slim/blob/main/CHANGELOG.md). Publisher: AGNTCY/Linux Foundation. Confidence: high on components, medium on absence of suite profile. Class: compatibility/deployability.

5. **No current W3C/IETF candidate displaced the finalists.** W3C WebAgents charter says it produces no specifications/software; IETF `draft-zahed-agent-comm-framework-01` is an individual architectural draft identifying future work, not an adopted interoperable standard. Sources: [W3C WebAgents charter](https://www.w3.org/community/webagents/charter/), [IETF AIPF draft](https://datatracker.ietf.org/doc/draft-zahed-agent-comm-framework/). Publishers: W3C CG/IETF individual draft. Confidence: high. Class: candidate screen.

6. **ANP remains an exploratory broad-suite comparator, not the deployment baseline.** It uniquely tests the possibility that identity/naming/discovery/messaging/federation are already coherent elsewhere, but released/draft mixing, heavy DID/Web/crypto dependencies and weaker governance/conformance evidence prevent Primary status.

## Consolidated disposition

- Include: A2A 1.0.x, MCP 2026-07-28, OpenAPI 3.2.1, Arazzo 1.1.0.
- Exploratory wildcard: ANP 1.1 released subset only.
- Component inventory, not candidate: AGNTCY DIR/SLIM.
- Cut: BeeAI ACP (merged), IETF individual framework drafts, W3C non-spec group.

Novelty exhausted: additional hits were components, superseded projects, individual drafts or non-normative groups. Remaining gaps are certification matrices, MCP Registry GA status and a canonical AGNTCY BOM.
