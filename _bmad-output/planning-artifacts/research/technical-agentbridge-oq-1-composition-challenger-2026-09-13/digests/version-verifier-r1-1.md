# Independent version/compatibility verifier digest

Accessed: 2026-09-13. Audit: 14 calls, 12 official source records.

## Material correction

**Overturned:** COAZ-MCP Draft 1 is not compatible unchanged with MCP 2026-07-28. It maps removed legacy methods (`logging/setLevel`, resource subscribe/unsubscribe, `ping`, server-initiated requests), lacks `server/discover` and `subscriptions/listen`, and fails unknown methods closed. The selected composition therefore needs an updated independently reviewed mapping; current COAZ-MCP is evidence that public mapping is possible, not ready glue.

## Claim audit

- **A2A 1.0.x:** versions verified (1.0.0 on 2026-03-12; 1.0.1 changes dated 2026-05-26, GitHub release May 28). Cross-SDK certification/compatibility unverified. Source: [A2A releases](https://github.com/a2aproject/A2A/releases). Publisher: A2A/Linux Foundation. Status: verified/unverified as scoped.
- **MCP 2026-07-28:** stable release and `5f5440b` verified. Go SDK v1.7.0 support through spec commit `f817239…` verified, but equivalence to final tag unverified. Source: [MCP releases](https://github.com/modelcontextprotocol/modelcontextprotocol/releases), [Go SDK releases](https://github.com/modelcontextprotocol/go-sdk/releases). Publisher: MCP project. Status: verified with pin caveat.
- **OpenAPI 3.2.1/Arazzo 1.1.0:** exact versions/dates verified; bundle/runtime compatibility unverified. Sources: [OAS 3.2.1](https://spec.openapis.org/oas/v3.2.1.html), [Arazzo 1.1.0](https://spec.openapis.org/arazzo/v1.1.0.html). Publisher: OpenAPI Initiative. Status: verified/unverified as scoped.
- **UCP v2026-08-25:** `cd78fb3` and release date verified; breaking changes mean no assumed April compatibility. Source: [UCP release](https://github.com/Universal-Commerce-Protocol/ucp/releases/tag/v2026-08-25). Publisher: UCP. Status: verified.
- **UCP↔AP2:** asymmetric compatibility verified; AP2 extension depends on UCP Checkout. Source: [UCP/AP2](https://ucp.dev/documentation/ucp-and-ap2/). Publisher: UCP Authors. Status: verified.
- **AP2 v0.2.0:** `b4587ac`, 2026-04-28 verified. Source: [AP2 releases](https://github.com/google-agentic-commerce/AP2/releases). Publisher: AP2. Status: verified.
- **ACP 2026-04-17:** snapshot verified but protocol remains beta; `9abf303` unverified from cited source. Source: [ACP repository](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol). Publisher: ACP. Status: disputed wording/unverified hash.
- **x402:** protocol version 2/scope partially verified; 2025-12-09 date, stable status, immutable pin and A2A/MCP compatibility unverified. Source: [x402 v2](https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md). Publisher: x402 Foundation. Status: partial/unverified.
- **AuthZEN:** Authorization API 1.0 Final dated 2026-01-11 verified; Approval Draft 1 (2026-09-10), COAZ-MCP Draft 1 (2026-02-13), Token Exchange Binding Draft 1 (2026-09-03) verified. Sources: [AuthZEN 1.0](https://openid.net/specs/authorization-api-1_0.html), [Approval](https://openid.github.io/authzen/authzen-access-request-approval-profile-1_0.html), [COAZ-MCP](https://openid.github.io/authzen/authzen-coaz-mcp-binding-1_0.html), [Token Exchange binding](https://openid.github.io/authzen/authzen-oauth-token-exchange-1_0.html). Publisher: OpenID Foundation. Status: verified, compatibility correction above.
- **RFC 9942/9943:** IETF Proposed Standards published 2026-06 verified; evidence scope correctly limited. “Strongest” remains comparative inference. Source: [RFC 9943](https://www.rfc-editor.org/info/rfc9943/). Publisher: IETF/RFC Editor. Status: verified/unverified comparative label.

## Citation corrections required in final report

Use immutable OAS/Arazzo URLs and canonical AuthZEN Final URL. Do not claim ACP GA/stable protocol or the unverified hash. Do not assign x402 date/stable pin. Qualify multi-language/TCK completeness and the MCP SDK final-tag gap. Treat COAZ-MCP as obsolete for the selected MCP revision until updated.
