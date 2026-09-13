# Digest: standards defect and constraint audit

Accessed: 2026-09-12

> Freshness correction: finding 8 below used the stale GitHub Releases surface. The official current release is UCP v2026-08-25; see `ucp-acp-freshness-r2-lead.md`. Finding 8 is retained as first-round history, not current evidence.

Decision signal: **native core with optional, explicitly negotiated, fail-closed bridges** is best supported. Mandatory composition would import cooperative cancellation, optional idempotency, transport-specific error behavior, orchestration without rollback, version coupling, and conformance-tool defects into the core contract.

## Findings

1. **MCP 2026-07-28** is stateless and self-contained per request; versions/capabilities travel on each request and extensions are opt-in. Tasks remains an extension, cancellation is cooperative, and important safety principles are host-enforced rather than protocol-enforced. This supports MCP as an optional tool bridge, not a complete AgentBridge substrate.
   - Source: MCP maintainers, “The 2026-07-28 Specification” — https://blog.modelcontextprotocol.io/posts/2026-07-28/
   - Status: GA release, 2026-07-28
   - Confidence/class: high; normative scope/specification tradeoff

2. MCP has an official conformance program, but its tracker documents test-suite defects and coverage gaps: cancellable fixtures accepting completed status, invalid 2026-07-28 stimuli, safe-integer MUST requirements untested with SDK disagreement, and timing-sensitive SSE concurrency. These are conformance implementation defects, not proof that the normative protocol is wrong.
   - Source: MCP project, conformance issues — https://github.com/modelcontextprotocol/conformance/issues
   - Status: open issues through 2026-09-09
   - Confidence/class: high that reports exist, medium on disposition; verification gaps

3. **A2A wire protocol 1.0** defines task states, polling/streaming/webhooks, error mappings, version and extension negotiation, authorization scoping, and semantic equivalence across JSON-RPC, gRPC, and HTTP+JSON. Identity remains outside A2A semantics; cancellation is only an attempt and may lose races; `SendMessage` idempotency is MAY; unsupported non-required extensions are ignored; and clients are encouraged to fall back among transports.
   - Source: A2A project / Linux Foundation, specification — https://github.com/a2aproject/A2A/blob/main/docs/specification.md
   - Status: current main, accessed 2026-09-12
   - Confidence/class: high; normative semantics/specification tradeoff

4. A2A’s current release is v1.0.1, while the rendered main specification header still says 1.0.0. The patch fixed content-type preference, transcoding errors, and task-status values. Patch releases are declared wire-compatible because negotiation uses Major.Minor.
   - Source: A2A project releases — https://github.com/a2aproject/A2A/releases
   - Status: v1.0.1 released 2026-05-28 for changes dated 2026-05-26
   - Confidence/class: high; version freshness/resolved defects

5. The A2A TCK is active but not a sufficient compatibility oracle by itself: open reports describe incomplete 1.0.1 alignment, stale error mappings, a hard failure on a MAY requirement, result/exit-code disagreement, and incorrect stimuli. Bridges must pin versions and add independent negative tests.
   - Source: A2A project, TCK issues — https://github.com/a2aproject/a2a-tck/issues
   - Status: open issues through 2026-09-07
   - Confidence/class: high that reports exist, medium on disposition; conformance defects

6. **OpenAPI 3.2.0** describes HTTP APIs; it is not a runtime transaction/agent protocol. Extensions are optional, callbacks/webhooks describe expected requests rather than delivery guarantees, and security schemes describe protection rather than enforce it. The specification records a security-requirement resolution ambiguity across referenced documents. Cancellation, idempotency, rollback, and partial effects remain API-specific.
   - Source: OpenAPI Initiative, OAS 3.2.0 — https://spec.openapis.org/oas/v3.2.0.html
   - Status: published 2025-09-19
   - Confidence/class: high; normative scope/documented ambiguity

7. **Arazzo 1.1.0** sequences calls and defines timeout plus `end`, `retry`, and `goto`; it does not define cancellation, rollback, compensation, atomicity, or exactly-once execution. Executing untrusted descriptions can perform unsafe operations on arbitrary network resources, so the consumer retains security responsibility.
   - Source: OpenAPI Initiative, Arazzo 1.1.0 — https://spec.openapis.org/arazzo/latest.html
   - Status: published 2026-05-17
   - Confidence/class: high; failure semantics/security boundary

8. **UCP 2026-04-08** is the strongest composition precedent: REST/MCP/A2A/embedded bindings, capability/version intersection, fail-closed profile validation, and required-capability failure. Security strength nevertheless varies: HTTPS and signed webhooks are mandatory, platform-to-business authentication is SHOULD, and AP2 authorization is optional.
   - Source: UCP project, specification overview — https://ucp.dev/2026-04-08/specification/overview/
   - Status: released snapshot 2026-04-08
   - Confidence/class: high; composition precedent/negotiation

9. UCP’s tracker reports cross-standard semantic failures: raw-body idempotency hashing conflicts with changing MCP JSON-RPC envelopes; specification/reference server/suite disagreement on request-time capabilities; an A2A binding lifecycle gap; and a signed-request example missing a covered component. These are unresolved reports, not adjudicated vulnerabilities, but they directly demonstrate adapter-layer risks.
   - Source: UCP project issues — https://github.com/Universal-Commerce-Protocol/ucp/issues
   - Status: reports from 2026-07-31 through 2026-09-11
   - Confidence/class: high that reports exist, medium on validity/severity; unresolved interoperability/security

10. BeeAI **Agent Communication Protocol** is no longer an independent forward standard: its repository says ACP is now part of A2A and directs migration. It is suitable only as a migration bridge.
    - Source: BeeAI / LF AI & Data, ACP repository — https://github.com/i-am-bee/acp
    - Status: migration notice current; last listed release 2025-08-21
    - Confidence/class: high; governance/upstream status

11. **AP2 v0.2** is a payment-security layer, not a communication or commerce protocol. Catalog, checkout update, and role-to-role APIs are out of scope; it expects an underlying commerce protocol. It places verification in deterministic code and supplies mandates/receipts, but not general cancellation, idempotency, transport errors, or workflow recovery.
    - Source: Google Agentic Commerce / AP2, v0.2 specification — https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md
    - Status: current main, v0.2
    - Confidence/class: high; normative scope/security boundary

12. AP2’s tracker contains reports about repeat redemption of accepted mandates, disclosure-dependent constraints, optional verifier context, missing rail-success evidence, and plaintext logging in samples. These are open reporter claims, not confirmed advisories; they support fail-closed bridges, single-use state, mandatory context binding, external rail attestation, and redaction outside sample SDKs.
    - Source: AP2 project issues — https://github.com/google-agentic-commerce/AP2/issues
    - Status: open issues through 2026-09-10
    - Confidence/class: high that reports exist, medium on validity/severity; unresolved implementation/security

## Strongest contrary evidence

- A2A already supplies extensive task semantics and a TCK; MCP now has stateless requests, opt-in extensions, Tier-1 SDKs, and conformance gates; UCP demonstrates fail-closed cross-binding profiles. A permanent profile remains credible if it can preserve all required invariants.
- A native core avoids imported runtime ambiguity but forfeits established discovery, transport, SDK, and governance ecosystems unless optional bridges provide a migration path.
- The evidence supports upstreaming missing semantics where they naturally belong even if AgentBridge retains an independent core.

## Bridge requirements implied by evidence

- Exact protocol/version pinning and no silent downgrade.
- Explicit capability intersection and fail-closed unsupported required extensions.
- AgentBridge-owned operation IDs, idempotency and durable single-use records.
- Cancellation states that never imply side effects stopped without evidence.
- Explicit partial-effect and compensation reporting.
- Strict error translation preserving original evidence.
- Independent negative conformance tests beyond upstream TCKs.
- Bridge isolation so upstream defects cannot weaken native invariants.

## Searches with no useful evidence

- No normative rollback, compensation, atomicity, or cancellation model in Arazzo 1.1.0.
- No general runtime idempotency/cancellation semantics in OpenAPI 3.2.0.
- No independent future ACP roadmap beyond migration into A2A.
- No official AP2 conformance suite or general transport/retry/cancellation model in v0.2.
- No published security advisory adjudicating the cited UCP/AP2 reports.
