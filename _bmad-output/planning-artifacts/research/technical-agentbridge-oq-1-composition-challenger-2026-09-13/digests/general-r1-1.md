# General agent/API/workflow protocols — round 1 digest

Accessed: 2026-09-13. Research firewall observed; conclusions use official primary sources.

## Findings

1. **MCP stable revision 2026-07-28 (`5f5440b`) is the strongest tool/context boundary, not a complete agent internet.** It uses JSON-RPC 2.0, stateless self-contained requests and per-request capability metadata; resources/prompts/tools and optional Tasks/Skills/Apps are strong inside a host↔server boundary. The protocol itself cannot enforce user consent/privacy/tool policy and no network-wide peer identity/routing/discovery layer was established. Current Go SDK v1.7.0 supports the revision, with opt-in needed for stateless HTTP mode. Source: [MCP specification](https://modelcontextprotocol.io/specification/2026-07-28), [MCP releases](https://github.com/modelcontextprotocol/modelcontextprotocol/releases), [Go SDK releases](https://github.com/modelcontextprotocol/go-sdk/releases). Publisher: MCP project. Released: 2026-07-28/current SDK 2026-08. Confidence: high. Class: version/scope/implementation.

2. **A2A 1.0.0 is the strongest general agent-to-agent task protocol.** It covers opaque independent agents, Agent Cards, capability/modality negotiation, task/context state, multi-turn messages, polling/streaming/webhooks, cancellation and multiple bindings (JSON-RPC, gRPC, HTTP+JSON). Identity issuance, credential acquisition and authorization policy remain outside A2A; no universal global directory/trust anchor was established. Source: [A2A v1.0.0 specification](https://a2a-protocol.org/v1.0.0/specification/). Publisher: A2A project. Current as accessed; publication date not shown. Confidence: high on scope, medium on global-discovery absence. Class: lifecycle/discovery/scope.

3. **OpenAPI 3.2.1 (2026-09-10) is the strongest callable HTTP-surface description.** It describes operations, schemas, servers, security declarations and streaming in a language-neutral form under Apache-2.0, but not autonomous task lifecycle, multi-agent coordination, durable state, agent discovery or execution policy. Source: [OAS 3.2.1](https://spec.openapis.org/oas/latest.html). Publisher: OpenAPI Initiative. Confidence: high. Class: version/API description.

4. **Arazzo 1.1.0 (2026-05-17) is a portable multi-operation workflow description, not an executor.** It expresses inputs, ordered API/workflow steps, dependencies, runtime expressions, success criteria and success/failure actions, including async examples. Scheduling, durable task state, agent identity/routing/discovery and actual execution remain external. Source: [Arazzo 1.1.0](https://spec.openapis.org/arazzo/latest.html). Publisher: OpenAPI Initiative. Confidence: high. Class: workflow/scope.

5. **ANP 1.1 is the closest released protocol-suite comparator to a broad agent internet.** Released material covers `did:wba` identity, WNS naming, active/passive discovery, direct/group messaging, E2EE, attachments, federation and payments, reusing HTTP/DNS/PKI/DID/JSON-RPC and complex cryptographic state. AgentConnect is linked implementation and code is Apache-2.0. However method-independent DID authentication, semantic meta-protocol negotiation, Messaging 1.2 and Core vNext remain drafts; governance maturity is unverified. Source: [ANP repository](https://github.com/agent-network-protocol/AgentNetworkProtocol). Publisher: ANP project. Current 2026-09-13. Confidence: high on components/draft status, medium on governance. Class: suite/maturity/dependency.

6. **AGNTCY is a strong modular Linux Foundation ecosystem, not one coherent wire protocol.** DIR, SLIM, Identity, OASF and SDKs supply discovery/messaging/identity/description components with active Apache-2.0 repos, but exact mutually compatible versions and one end-to-end conformance level were not established. Source: [AGNTCY GitHub organization](https://github.com/agntcy). Publisher: AGNTCY/Linux Foundation. Updated through 2026-09-13. Confidence: high on modularity, medium on compatibility closure. Class: ecosystem/infrastructure.

## Decision signal

Leading general stack: **A2A 1.0 + OpenAPI 3.2.1 + Arazzo 1.1.0 + MCP 2026-07-28**. ANP 1.1 deserves a separate wildcard challenger because it directly targets broader naming/discovery/identity/messaging/federation, though draft boundaries and deployment complexity are material. AGNTCY components may strengthen a deployable configuration but require an exact compatibility manifest.

## Gaps/leads

- Verify A2A publication date, license/foundation governance and SDK compatibility.
- Verify MCP governance/license and network discovery conclusion.
- Establish exact compatible AGNTCY component releases if included.
- Do not treat ANP draft specifications as released capability.
