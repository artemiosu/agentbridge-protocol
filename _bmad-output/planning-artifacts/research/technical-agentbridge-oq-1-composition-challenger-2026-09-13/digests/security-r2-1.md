# Security stack — round 2 digest

Accessed: 2026-09-13. Official primary sources only. The conclusion about profile sufficiency is a hypothesis for Gate 1, not an OQ-1 project decision.

## Deployability and conformance

1. **OpenID conformance tooling covers surrounding OIDC/FAPI and DPoP environments, but no standalone RAR/RFC 8693 certification was established.** Project-owned profile tests remain required. Source: [OpenID conformance suite](https://github.com/openid-certification/conformance-suite). Publisher: OpenID Foundation. Current 2026-09-13. Confidence: high on stated coverage, medium on absence. Class: conformance.

2. **AuthZEN 1.0 has interop material but no formal certification evidenced.** OpenID spec implementation rights are governed by OIDF terms/IPR; exact harness license not established. Source: official AuthZEN WG materials. Publisher: OpenID Foundation. Confidence: medium-high. Class: maturity/licensing.

3. **A deployable BSD-2-Clause GNAP server exists but documents major omissions.** SUNET implementation includes consent, mTLS/JWS and deployment tests, but omits key-bound access tokens, rotation, multiple token requests and RFC 9767 introspection. It proves implementability, not full-suite maturity. Source: [SUNET auth server](https://github.com/SUNET/sunet-auth-server). Publisher: SUNET. Current 2026-09-13. Confidence: high. Class: implementation/maturity.

4. **SCITT has active 2026 examples/SCRAPI/profile work but no finished conformance program evidenced.** Source: [IETF SCITT organization](https://github.com/ietf-wg-scitt). Publisher: IETF SCITT WG. Updated 2026-06. Confidence: high on activity, medium on absence. Class: implementation maturity.

5. **W3C VC 2.0 has a runnable issuer/verifier interoperability suite, but should be optional.** It is a portable signed-claims envelope, not authorization/consent semantics; reproducibility requires pinning the test suite rather than tracking `main`. Source: [W3C VC 2.0 test suite](https://github.com/w3c/vc-data-model-2.0-test-suite). Publisher: W3C VC WG. Current 2026-09-13. Confidence: high. Class: conformance/scope.

## New material

6. **AuthZEN Access Request and Approval Profile Draft 1 (2026-09-10) materially reduces the async approval handoff gap.** It standardizes requestable denial, durable handles, polling/callback, binding, expiry/replay defenses and mandatory post-approval re-evaluation. It leaves UI/workflow, approver eligibility, authority vocabulary and portable signed consent outside scope. Source: [ARAP Draft 1](https://openid.github.io/authzen/authzen-access-request-approval-profile-1_0.html). Publisher: OpenID Foundation AuthZEN WG. Confidence: high. Class: emerging approval profile.

7. **COAZ-MCP Draft 1 (2026-02-13) gives a fail-closed public MCP→AuthZEN mapping.** It addresses identity smuggling, operation mutation, omitted inputs and PDP failure, but mappings remain server-authored and action/resource semantics still need public definition. Source: [COAZ-MCP Draft 1](https://openid.github.io/authzen/authzen-coaz-mcp-binding-1_0.html). Publisher: OpenID Foundation AuthZEN WG. Confidence: high. Class: public mapping profile.

8. **AuthZEN OAuth Token Exchange Binding Draft 1 (2026-09-03) supplies an AS↔PDP mapping for delegation/token shaping.** It distinguishes attenuation from fresh `sub`/`aud` issuance and uses RAR where distinctions must survive. It remains Draft 1. Source: [AuthZEN Token Exchange Binding Draft 1](https://openid.github.io/authzen/authzen-oauth-token-exchange-1_0.html). Publisher: OpenID Foundation AuthZEN WG. Confidence: high. Class: emerging delegation mapping.

## Pareto result

- **OAuth profile stack dominates for deployment readiness:** RFC 9700, 8707, 9396, 8693, 9449, optional 9068, AuthZEN 1.0, SCITT/COSE Receipts; SSF/CAEP and VC 2.0 only where needed.
- **GNAP 9635+9767 remains non-dominated on coherent stateful grant negotiation**, but loses on OAuth compatibility, conformance and implementation completeness. Keep as a challenger/watch prototype, not mandatory dual runtime.
- Recent AuthZEN profiles strengthen the hypothesis that missing exact-authority glue can be a public profile. They do **not** establish that no Native Core is warranted; only Gate 1 executable evidence may decide that.

## Exact public-profile glue still required

Canonical authority cell and attenuation algebra; deterministic digest/binding; provenance per input; approval lifecycle and separation of duties; fail-closed error table; evidence statement profile; revocation/change semantics. These are not supplied end-to-end by reviewed standards.

## Remaining gaps

No official standalone RAR/Token Exchange or GNAP conformance suite found; no current executable SSF/CAEP certification; SCITT conformance incomplete; several implementation licenses unverified; all three highly relevant AuthZEN additions remain Draft 1.

Coverage/novelty exhausted within source cap.
