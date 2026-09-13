# Independent selection/fairness verification digest

Accessed: 2026-09-13. Local digests framed questions only; official primary sources support findings.

## Verdict

Select two Pareto configurations:

1. **Primary C:** A2A/MCP + OAuth/AuthZEN + SSF/CAEP + UCP/AP2.
2. **GNAP authority challenger:** same task/tool/commerce substrate, with GNAP 9635/9767 replacing the OAuth grant/token family.

Only Primary C is credibly deployable today from existing implementations. GNAP remains non-dominated under “independent implementation from public normative material,” conditional on implementing missing RFC features. ANP fails the exact released-version/glue hard gate and remains reserve until an immutable BOM exists.

## Corrected Primary C boundaries

- A2A 1.0 semantics; MCP 2026-07-28 with MCP Tasks disabled because A2A owns inter-agent lifecycle.
- OAuth RFC 9700/8707/9396/8693 plus one proof-of-possession mechanism, normally DPoP 9449.
- AuthZEN Authorization API 1.0 Final; SSF/CAEP 1.0 Final for authority/context invalidation.
- UCP 2026-08-25 over canonical REST; A2A/MCP exposure is adapter-only; AP2 v0.2 only for consequential checkout/payment scope.
- OpenAPI 3.2.1 and Arazzo 1.1.0 are descriptive artifacts, not runtime-semantic credit.
- AuthZEN MCP/approval/obligations/Token Exchange bindings are provisional Draft glue, not mature Final coverage. Sources: [AuthZEN specifications](https://openid.net/wg/authzen/specifications/), [A2A spec](https://a2a-protocol.org/dev/specification/), [MCP release](https://blog.modelcontextprotocol.io/posts/2026-07-28/), [MCP conformance](https://github.com/modelcontextprotocol/conformance), [UCP overview](https://ucp.dev/specification/overview/), [AP2 spec](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md). Confidence: high on scope; medium on draft integration.

## Corrected GNAP challenger

Keep task/tool/commerce stack, replace OAuth issuance/RAR/token exchange/DPoP with RFC 9635 + RFC 9767 and explicit GNAP→AuthZEN mapping/authority vocabulary. Do not run both authorization planes mandatorily. GNAP improves coherent grant negotiation but available SUNET implementation omits key-bound tokens, multiple requests, rotation and 9767 introspection. Sources: [RFC 9635](https://datatracker.ietf.org/doc/html/rfc9635), [SUNET server](https://github.com/SUNET/sunet-auth-server). Confidence: high. Class: alternative/deployability limitation.

## Score ranges (0–5)

| Configuration | Coverage 35% | Safety 25% | Topology 15% | Deployability 15% | Governance 10% | Weighted range |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Primary C | 3.7–4.2 | 3.5–4.1 | 3.7–4.2 | 2.8–3.4 | 4.1–4.6 | **3.6–4.1** |
| GNAP challenger | 3.8–4.3 | 3.8–4.4 | 3.9–4.4 | 1.8–2.6 | 3.7–4.2 | **3.5–4.1** |
| ANP reserve | 3.8–4.3 | 3.1–3.8 | 4.2–4.7 | 1.7–2.5 | 2.3–3.1 | **3.2–3.8** |

Intervals overlap; Primary is preferred on materially stronger worst-case deployability, not a false-precision midpoint.

## Hard-gate corrections

- Add SSF/CAEP when claiming continuous revocation.
- Do not count OpenAPI/Arazzo as execution/lifecycle.
- Disable overlapping MCP Tasks by default.
- ACP is adapter-only; one canonical UCP semantic model.
- Do not double-wrap AP2 evidence in SCITT without a public equivalence profile.
- CloudEvents may be optional event-envelope glue, not lifecycle.
- Both finalists must disclose task/idempotency state, authority/PDP/token/trust stores, canonical actor/attenuation semantics, mappings, consent lifecycle, stale-signal behavior, effect digest/evidence, payment/trust integrations and complete failure/recovery table.

## ANP disposition

ANP is a meaningful reserve but not selected until an immutable BOM separates released 1.1 profiles from Messaging 1.2 drafts and defines task/authority mappings. Source: [ANP messaging overview](https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/09-ANP-end-to-end-instant-messaging-protocol-specification.md). Confidence: medium-high. Class: hard-gate failure.
