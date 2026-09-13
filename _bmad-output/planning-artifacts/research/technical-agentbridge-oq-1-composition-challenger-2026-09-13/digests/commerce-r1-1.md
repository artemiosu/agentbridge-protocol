# Commerce/payment protocols — round 1 digest

Accessed: 2026-09-13. Research firewall observed; conclusions use official primary sources.

## Findings

1. **UCP v2026-08-25 (`cd78fb3`) is the broadest current commerce substrate found.** It provides well-known profiles, capability intersection, REST/MCP/A2A/embedded bindings, catalog/location, cart/checkout, identity linking, order events and payment handlers; it explicitly includes B2C, B2B and agent-to-agent commerce. Apache-2.0 and a multi-company Payments Technical Council improve deployability/governance. Lodging is foundation/in-development, not a demonstrated complete booking protocol. Source: [UCP releases](https://github.com/Universal-Commerce-Protocol/ucp/releases), [Core Concepts](https://ucp.dev/documentation/core-concepts/). Publisher: Universal Commerce Protocol project. Updated/released: 2026-08-25/current docs. Confidence: high. Class: version/scope/deployability.

2. **AP2 v0.2.0 (`b4587ac`, 2026-04-28) is an authority/payment-evidence layer, not a commerce protocol.** It models Shopping Agent, Credential Provider, Merchant, Merchant Payment Processor and deterministic Trusted Surface; signed mandates bind direct or autonomous user authority to checkout/payment evidence. Catalog, checkout-update APIs and role transport are explicitly out of scope; agent-to-agent mandate delegation, automated evidence retrieval/retention and dispute rules remain out of scope. Official naming is inconsistent between “Agent Payments Protocol” and “Agentic Payment Protocol.” Source: [AP2 specification](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md), [AP2 releases](https://github.com/google-agentic-commerce/AP2/releases). Publisher: Google Agentic Commerce/AP2. Confidence: high. Class: version/authority/limitation.

3. **ACP stable snapshot 2026-04-17 is deployable but narrower/older than current UCP.** It covers buyer/agent-to-seller discovery, feeds, cart/checkout, orders, authentication and delegated payment; seller remains merchant of record. The OpenAI direct payment payload found is card-only and direct delegated payment requires a PSP or PCI-DSS L1 merchant vault. It lacks AP2-style mandates/evidence. Apache-2.0, but OpenAI/Stripe founding maintainers retain appointment/removal and limited veto powers; neutral foundation is aspirational. Source: [ACP repository/spec](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol), [ACP governance](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/docs/governance.md), [OpenAI delegated payment](https://developers.openai.com/commerce/specs/payment/). Publisher: ACP/OpenAI. Confidence: high. Class: version/scope/governance.

4. **x402 v2 (2025-12-09) is credible for paid APIs and machine-to-machine service procurement, not full commerce.** It supports extensible payment schemes, verification/settlement and discovery over HTTP/MCP/A2A, but has no cart, fulfillment, return/refund, consent ceremony or booking lifecycle; dispute handling and budget/session semantics remain external. Source: [x402 v2 specification](https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md). Publisher: x402 Foundation. Confidence: high on scope, medium on exact current commit. Class: alternative/scope.

## Decision signal

Strong commerce layer: **UCP v2026-08-25 + AP2 v0.2 pinned**. ACP is a production-facing adapter/alternative rather than a necessary co-equal layer. x402 is a scoped paid-service extension. None establishes complete booking, agent-to-agent delegation, dispute adjudication, evidence retention/retrieval or universal revocation.

## Contradictions and gaps

- No official ACP↔UCP compatibility mapping found.
- No released complete UCP lodging/booking capability verified.
- AP2 release is older than the one-month compatibility window; pinning and explicit freshness exception/recheck are required.
- Visa/Mastercard agent programs were not treated as protocol finalists because no comparably complete public normative specification was established within budget.
