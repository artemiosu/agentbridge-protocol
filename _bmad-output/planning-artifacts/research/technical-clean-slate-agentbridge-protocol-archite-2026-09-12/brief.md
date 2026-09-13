---
title: "Research Brief: Clean-Slate AgentBridge Protocol Architecture, Security & Performance"
type: technical
shape: select
status: approved
created: 2026-09-12
visibility: private-local
---

# Research Brief

## Decision

At architectural Gate 0, choose among:

1. a native, self-contained-at-agent-layer AgentBridge protocol suite;
2. a permanent profile/composition of existing agent protocols;
3. a native AgentBridge core with optional, fail-closed compatibility bridges;
4. upstream contribution or stopping a separate AgentBridge core.

The current primary hypothesis is option 3, but the research must be capable of overturning it.

## Hard requirements

- Two native AgentBridge implementations must interoperate without MCP, A2A, UCP, AgentBridge Cloud, a mandatory broker, or another agent-protocol runtime.
- AgentBridge must remain a protocol and SDK ecosystem, not an agent, user interface, shopping application, or proprietary aggregator.
- The design must be open, royalty-free at the core, independently implementable, domain-neutral, versionable, and testable through public conformance artifacts.
- Security-critical meaning must survive end to end: capabilities, action identity, principal/delegate authority, consent/approval, lifecycle, idempotency, cancellation/revocation, partial effects, evidence, errors, and negotiation.
- Compatibility bridges must not silently weaken or discard security semantics; an unsafe or lossy mapping must fail closed.
- Do not invent cryptographic algorithms, global identity providers, payment rails, or industry systems of record.

## Post-research confirmed scope clarification

After completion of the research, the user confirmed a broader product requirement. This clarification is project direction, not external evidence, and must be validated in the next PRD rather than treated as a proven research finding.

- The intended protocol core is role-neutral and direction-neutral. It must be capable of supporting agent↔service, agent↔agent, service↔service/B2B, reverse asynchronous flows, multi-party or delegated chains, and participant interaction with replaceable discovery, trust, policy, and audit infrastructure through explicit bindings, regardless of whether a participant belongs to an individual, one organization, or multiple organizations.
- Organizational labels such as consumer, business, client, and server do not define permanent wire roles. Participants may act as principal, initiator/requester, delegate, responder/provider, executor, approver, resource owner, policy authority, or attestor, and may combine or change roles across interactions.
- The interaction model must be evaluated across information exchange, request/response, commands and consequential actions, events/subscriptions, streaming, negotiation, long-running work, evidence, and errors.
- The previously emphasized agent-to-service consequential-action flow remains the first high-risk validation slice, not the architectural boundary of AgentBridge.
- Human-facing agent interfaces and domain systems of record remain outside the protocol core; consent, approval, policy, and evidence needed for interoperability may cross the protocol boundary.

## Weighted criteria

| Criterion | Weight |
| --- | ---: |
| End-to-end security and semantic correctness | 25% |
| Failure isolation, resilience, and operational simplicity | 15% |
| Performance and scalability potential | 15% |
| Independent implementability and conformance | 15% |
| Interoperability and migration path | 10% |
| Extensibility and long-term evolvability | 10% |
| Governance, adoption, and ecosystem credibility | 7% |
| Reversibility and cost of being wrong | 3% |

Any option that violates a hard requirement is rejected regardless of weighted score.

## Research dimensions

1. Defect and constraint audit of MCP, A2A, OpenAPI/Arazzo, UCP, ACP, AP2, AuthZEN, OAuth/GNAP and adjacent initiatives, including current specifications, security guidance, issue trackers, implementation failures, and version churn.
2. Clean-slate protocol invariants and candidate architectures originally researched: trust boundaries, state machines, capability negotiation, delegation/approval, effect evidence, errors, extensions, downgrade resistance, and conformance. The later role/topology-neutral scope clarification is an input to PRD validation, not evidence produced by this completed research run.
3. Performance and implementation reality: round trips, framing, serialization, signing/canonicalization, streaming/backpressure, recovery, wire formats/transports, and the evidence for or against a Rust reference core.
4. Interoperability, adoption, and governance: optional bridges, migration, independent implementations, neutral change control, and lessons from successful and failed standards.
5. Comparative Gate 0 verdict with explicit assumptions, failure trees, strongest counterarguments, falsification criteria, and the next permitted BMAD artifact.

## Evidence rules

- Project context defines requirements but is not evidence.
- Use primary public technical sources wherever available: normative specifications/RFCs, official repositories and issue trackers, security advisories/CVEs, conformance suites, implementation documentation, benchmarks with disclosed methodology, and standards-body governance records.
- Treat absence from reviewed normative material as bounded absence, not universal nonexistence.
- Cross-check performance claims and claims of protocol failure with independent evidence.
- Findings must distinguish documented defects, design tradeoffs, implementation defects, unresolved issues, and researcher inference.

## Deliverable

A Russian decision-grade `research.md` containing the source-backed defect map, invariant set, candidate architectures, threat/failure analysis, benchmark plan, weighted decision matrix, contrary evidence, Gate 0 verdict, open questions, source appendix, and staleness map. No protocol specification, SDK, architecture implementation, or production code is authorized by this run.
