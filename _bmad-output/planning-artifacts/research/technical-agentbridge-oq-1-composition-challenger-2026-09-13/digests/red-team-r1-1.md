# Fresh-context red-team digest

Accessed: 2026-09-13. No local project context was provided to the reviewer.

## Verdict

The broad primary stack survives, but “cleanly deployable” is overstated. AGNTCY must be included as a serious infrastructure configuration, ANP promoted to substantive integrated runner-up, and AP2/SCITT treated as requiring explicit profiles rather than complete authority/evidence solutions.

## Surviving counterarguments

1. **AGNTCY is a credible omitted Pareto contender/augmentation.** Official components combine OASF descriptions, verifiable identity, distributed Directory, SLIM secure messaging, observability and reference app. DIR v1.6.2 provides DHT discovery, OIDC, signatures, A2A/MCP import/export and scanners; SLIM v1.3 provides multi-language packages and MLS E2EE. Lack of a single suite BOM remains real, but it may be stronger as infrastructure under A2A/MCP than ANP as wholesale replacement. Sources: [AGNTCY organization](https://github.com/agntcy), [DIR changelog](https://github.com/agntcy/dir/blob/main/CHANGELOG.md), [SLIM changelog](https://github.com/agntcy/slim/blob/main/CHANGELOG.md). Publisher: AGNTCY/Linux Foundation. Confidence: high on omission, medium on displacement. Class: contrary candidate evidence.

2. **AP2/UCP composition contains unrepaired seams.** AP2 leaves commerce APIs, mandate/disclosure selection, agent-to-agent delegation, dispute retrieval/retention/resolution outside scope. Its text contains a signature-algorithm tension that UCP resolves by selecting ECDSA, making this an adapter-specific choice. Sources: [AP2 v0.2](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md), [UCP AP2 mandates](https://ucp.dev/specification/payment/extensions/ap2-mandates/). Publishers: AP2/UCP. Confidence: high. Class: composition gap.

3. **AP2 maturity/governance is weaker than a stable layer label suggests.** FIDO characterizes AP2 and Mastercard Verifiable Intent as initial contributions to new standards work. Source: [FIDO agentic standards announcement](https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/). Publisher: FIDO Alliance. Published: 2026-04-28. Confidence: high. Class: maturity/governance.

4. **AuthZEN closes some local gaps, not cross-stack semantics.** Authorization API 1.0 is Final and new drafts address MCP mapping, async approval and obligations, so saying authority/effect is wholly unsolved is false. Equivalent bindings across A2A, Arazzo, UCP/AP2 and evidence systems were not found. Sources: [AuthZEN 1.0 approval](https://openid.net/authorization-api-1-0-final-specification-approved/), [agent-era drafts](https://openid.net/openid-foundation-advances-authorization-for-the-agent-era-with-new-authzen-working-group-drafts/). Publisher: OpenID Foundation. Confidence: high. Class: contrary gap evidence.

5. **SCITT is only an evidence substrate.** RFC 9943 concerns signed statements/transparency receipts and does not define agent mandates, policy decisions, task effects or disputes. An agent-action evidence profile is still required. Source: [RFC 9943](https://www.rfc-editor.org/info/rfc9943/). Publisher: IETF/RFC Editor. Published: 2026-06. Confidence: high. Class: semantic mismatch.

6. **ANP is more than a speculative wildcard.** Released identity, naming, discovery, direct/group messaging, E2EE, federation and AP2 support plus Python/Go/Rust SDKs merit a substantive runner-up, though semantic negotiation remains draft and adoption/governance evidence is weaker. Source: [ANP status guide](https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/docs/anp-getting-started-guide.md). Publisher: ANP project. Confidence: high on scope, low-medium on primary candidacy. Class: contrary ranking.

## Disproven/weak attacks

- AGNTCY does not replace commerce/authority/evidence; it is complementary infrastructure. Source: [SLIM-A2A transport](https://github.com/agntcy/slim-a2a-python). Publisher: AGNTCY. Confidence: high.
- OAP is broader on paper but only a Public Working Draft with weak governance/adoption evidence, so not a deployable finalist. Source: [OAP repo](https://github.com/openagentprotocol-OAP/oap-spec). Publisher: OAP contributors. Confidence: high.
- No fatal licensing blocker or Visa TAP replacement for general delegation/evidence was established.

## Required synthesis correction

Describe Primary C as the strongest broad composition **requiring substantial public glue**, add AGNTCY as an infrastructure variant, promote ANP to integrated runner-up, and do not claim AP2/SCITT or AuthZEN completes cross-protocol authority/effect/evidence semantics.
