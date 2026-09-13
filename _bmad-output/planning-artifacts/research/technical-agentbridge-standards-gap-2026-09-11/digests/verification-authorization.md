# Independent verification: authorization, trust, and commerce claims

**Verification date / access date:** 2026-09-11  
**Evidence rule:** primary official sources only. The two supplied digests were used only to enumerate claims; their citations and conclusions were not treated as evidence.  
**Source budget:** eight official source groups: UCP; ACP; AP2; x402; final IETF RFCs; OpenID4VP; IETF OAuth WG drafts; current official gap-scan documents.  
**Verdict meanings:** `VERIFIED` = the official source directly supports the material claim; `DISPUTED` = part of the claim is contradicted or materially overstated; `UNVERIFIED` = the evidence inspected does not establish the claim; `OVERTURNED` = the claim's conclusion is reversed.

## Verification results

### V1 — UCP is only a living draft

**Verdict: DISPUTED**

- **Direct URL:** https://ucp.dev/2026-04-08/specification/overview/
- **Publisher:** Universal Commerce Protocol project
- **Published/updated:** dated specification `2026-04-08`; page does not state a separate last-updated date
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The cited `/draft/` channel is indeed a mutable draft channel, but it is no longer an adequate status description of UCP as a whole. The official site exposes a dated `2026-04-08` specification and uses `2026-04-08` in profile, service, capability, and schema examples. It also specifies current-version and `supported_versions` resolution. The digest should distinguish the draft channel from the dated release surface.

### V2 — UCP supplies discovery/version negotiation and thin REST/OpenAPI, MCP, and A2A bindings, but its vocabulary is commerce-oriented

**Verdict: VERIFIED**

- **Direct URL:** https://ucp.dev/2026-04-08/specification/overview/
- **Publisher:** Universal Commerce Protocol project
- **Published/updated:** dated specification `2026-04-08`
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The profile example is published at `/.well-known/ucp`; services carry date versions, specification/schema URLs, transport, and endpoint. The transport enum is `rest`, `mcp`, `a2a`, and `embedded`. REST appends OpenAPI paths to the endpoint; MCP uses `tools/call` with the operation in `params.name` and UCP payload in `params.arguments`; A2A carries UCP as an extension. The advertised capability examples are shopping checkout, fulfillment, discount, order, and identity linking. No general cross-domain action vocabulary or signed external-effect receipt is defined on this page.

### V3 — ACP is beta and its latest stable snapshot is 2026-04-17

**Verdict: VERIFIED**

- **Direct URL:** https://github.com/agentic-commerce-protocol/agentic-commerce-protocol
- **Publisher:** Agentic Commerce Protocol project, maintained by OpenAI and Stripe
- **Published/updated:** current repository `main`; README says `beta`; latest stable links target `2026-04-17`
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The official README calls ACP an open standard maintained by OpenAI and Stripe and “currently in `beta`.” Its latest-stable OpenAPI, JSON Schema, and examples links point to `2026-04-17`, and the versioning section says each dated directory is a complete snapshot.

### V4 — ACP's stable surface has no MCP binding

**Verdict: DISPUTED**

- **Direct URL:** https://github.com/agentic-commerce-protocol/agentic-commerce-protocol
- **Publisher:** Agentic Commerce Protocol project, maintained by OpenAI and Stripe
- **Published/updated:** current repository `main`; stable snapshot `2026-04-17`
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The repository's version tree describes `2026-04-17` as adding “Cart, feed, orders, authentication, and MCP.” Therefore the digest's categorical claim that the stable ACP contract “does not advertise an MCP ... binding” is stale. The inspected checkout artifact remains an HTTP OpenAPI contract, so ACP's exact MCP mapping should be assessed from the stable MCP-specific artifacts rather than inferred from that OpenAPI file.

### V5 — ACP's stable surface has no A2A binding

**Verdict: UNVERIFIED**

- **Direct URL:** https://github.com/agentic-commerce-protocol/agentic-commerce-protocol
- **Publisher:** Agentic Commerce Protocol project, maintained by OpenAI and Stripe
- **Published/updated:** current repository `main`; stable snapshot `2026-04-17`
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The official README affirmatively advertises MCP in the stable snapshot but does not make an equivalent A2A statement in the inspected version summary. That omission alone is insufficient to prove that no A2A binding exists anywhere in the current repository or companion documentation.

### V6 — ACP delegated payment is one-time, amount/currency/session/merchant/expiry-constrained credential delegation and is card-only in the stable endpoint

**Verdict: VERIFIED**

- **Direct URL:** https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.delegate_payment.yaml
- **Publisher:** Agentic Commerce Protocol project, maintained by OpenAI and Stripe
- **Published/updated:** stable snapshot `2026-04-17`
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** `Allowance` requires `reason`, `max_amount`, `currency`, `checkout_session_id`, `merchant_id`, and `expires_at`; the only supported reason is `one_time`. `PaymentMethodCard.type` has the sole value `card` and is described as always card. These fields constrain use of a payment credential. The schema does not establish a general human-to-agent authority chain or a signed receipt proving execution of an arbitrary downstream action.

### V7 — ACP checkout completion creates an order and supports idempotent mutation replay, but does not return a general signed effect receipt

**Verdict: VERIFIED**

- **Direct URL:** https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.agentic_checkout.yaml
- **Publisher:** Agentic Commerce Protocol project, maintained by OpenAI and Stripe
- **Published/updated:** stable snapshot `2026-04-17`
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** `completeCheckoutSession` says success “MUST create an order” and returns `CheckoutSessionWithOrder`. POST requests require an `Idempotency-Key`, and replay/conflict behavior is typed. This is authoritative commerce state, not a domain-neutral signed execution-evidence object.

### V8 — AP2 v0.2 standardizes Checkout and Payment mandates, is commerce-scoped, and is explicitly UCP-compatible

**Verdict: VERIFIED**

- **Direct URL:** https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md
- **Publisher:** Google Agentic Commerce / AP2 project
- **Published/updated:** living repository `main`; document identifies itself as Agentic Payment Protocol `v0.2`; no immutable page date
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The specification says “AP2 defines two Mandate types: Checkout Mandate and Payment Mandate.” It scopes itself to securing agent-performed payment transactions, places catalog/checkout-update/role APIs outside scope, and says it is explicitly designed to be compatible with UCP. This supports the digest's payment-specific standardization boundary.

### V9 — AP2 Agent Authorization defines delegation, transaction-bound closed mandates, verifier checking, privacy-preserving presentation, and a signed authorization receipt

**Verdict: VERIFIED**

- **Direct URL:** https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md
- **Publisher:** Google Agentic Commerce / AP2 project
- **Published/updated:** living repository `main`; AP2 v0.2 family; no immutable page date
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The document separates Mandate Delegation from Action Authorization. An open mandate is constraint-bound and tied to an agent; the agent uses its endorsed key to bind a closed mandate to a particular verifier transaction. The verifier checks integrity and whether content authorizes the action; selective disclosures must maximize user privacy. The document says AP2 uses the model for payments and that broader use “could be applied more generally in the future,” confirming that general deployment is positioning, not current standardized coverage.

### V10 — AP2's mandatory receipt is an authorization-decision receipt, not proof of the external effect

**Verdict: VERIFIED**

- **Direct URL:** https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md
- **Publisher:** Google Agentic Commerce / AP2 project
- **Published/updated:** living repository `main`; AP2 v0.2 family; no immutable page date
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** After acceptance or rejection, the verifier must return a signed Mandate Receipt. Its required fields are `iss`, `result` (`success` or `error`) “indicating the result of the action authorization,” and `reference`, a hash of the received mandate; `error` is conditionally required. The required core does not attest that an external-world effect occurred. AP2's commerce specification separately defines Checkout and Payment receipts and dispute evidence, but does not turn the generic receipt into a universal execution proof.

### V11 — x402 v2 is a payment-for-resource/settlement protocol, not a principal-to-agent mandate or general action lifecycle

**Verdict: VERIFIED**

- **Direct URL:** https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md
- **Publisher:** x402 Foundation
- **Published/updated:** protocol version 2; version history dates v2.0 to 2025-12-09; living repository `main`
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The document identifies `Protocol Version: 2` and standardizes payment requirements, payment payloads, verification, and settlement across transport representations including HTTP, MCP, and A2A. Client-side budget management, session handling, transport-specific implementations, and framework-specific integrations are outside core scope. The payment authorization binds payer, recipient, value, validity window, and nonce; it is not a portable human-principal delegation mandate or arbitrary-action receipt.

### V12 — RAR provides typed fine-grained authorization details but delegates exact action semantics to the API profile

**Verdict: VERIFIED**

- **Direct URL:** https://www.rfc-editor.org/rfc/rfc9396.html
- **Publisher:** IETF / RFC Editor
- **Published/updated:** RFC 9396, May 2023; Standards Track Proposed Standard
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** `authorization_details` carries JSON objects with a required `type`; reusable fields include `locations`, `actions`, `datatypes`, `identifier`, and `privileges`, and an API may define type-specific fields. The AS controls interpretation, and allowable fields/values are determined by the API's type. Collision-resistant type names are recommended for openly deployed APIs, not universally mandated. RAR can carry exact action parameters once a profile defines them; it does not itself supply cross-transport action semantics.

### V13 — Token Exchange expresses the current actor and a nested prior-actor history, but prior actors are informational for access control

**Verdict: VERIFIED**

- **Direct URL:** https://www.rfc-editor.org/rfc/rfc8693.html
- **Publisher:** IETF / RFC Editor
- **Published/updated:** RFC 8693, January 2020; Standards Track Proposed Standard
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** `subject_token` represents the subject and optional `actor_token` the acting party. Nested `act` claims form a history in which the outermost actor is current and deeper entries are older. Critically, the token consumer must use only top-level claims and the current actor for access control; prior actors are informational. `may_act` identifies a party authorized to become an actor. The chain records delegation actors, not human approval of exact action parameters.

### V14 — Token Exchange has no inherent input/output revocation linkage or single-use semantics

**Verdict: VERIFIED**

- **Direct URL:** https://www.rfc-editor.org/rfc/rfc8693.html
- **Publisher:** IETF / RFC Editor
- **Published/updated:** RFC 8693, January 2020; Standards Track Proposed Standard
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The RFC expressly says an exchange does not invalidate the subject or actor token absent token-type-specific semantics, creates no tight linkage between input and output tokens, and does not make revocation propagation a general STS property. Expiry, one-time use, and revocation propagation therefore require token-type or deployment/profile rules.

### V15 — DPoP excludes query and body from its base request binding

**Verdict: VERIFIED**

- **Direct URL:** https://www.rfc-editor.org/rfc/rfc9449.html
- **Publisher:** IETF / RFC Editor
- **Published/updated:** RFC 9449, September 2023; Standards Track Proposed Standard
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The mandatory proof binds `jti`, HTTP method (`htm`), target URI without query/fragment (`htu`), issue time, and, for protected-resource access, the access-token hash (`ath`); a server nonce may also be required. The RFC states that only method and URI are covered from the HTTP request. The base proof has no request-body digest and excludes query parameters, although profiles may define additional claims. DPoP is therefore sender/request proof, not by itself approval of material body/query arguments.

### V16 — GNAP supplies a final grant lifecycle with interaction, continuation, cancellation, and optional token management, but not general action semantics or effect receipts

**Verdict: VERIFIED**

- **Direct URL:** https://www.rfc-editor.org/rfc/rfc9635.html
- **Publisher:** IETF / RFC Editor
- **Published/updated:** RFC 9635, October 2024; Standards Track Proposed Standard
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** GNAP supports resource-owner interaction, key-bound continuation, polling/asynchronous authorization, one-time interaction references, grant revocation/finalization, and optional access-token rotate/revoke management. Resource-server sufficiency evaluation is outside the protocol. GNAP explicitly says it is not an OAuth extension and is not intended to be directly compatible with OAuth 2.0. It does not define a portable nested actor history, universal action vocabulary, or signed execution-effect receipt.

### V17 — OpenID4VP 1.0 provides a final verifier challenge, typed transaction data, presentation binding, and consent guidance

**Verdict: VERIFIED**

- **Direct URL:** https://openid.net/specs/openid-4-verifiable-presentations-1_0.html
- **Publisher:** OpenID Foundation
- **Published/updated:** published 2025-07-09; Final Specification 1.0 with errata channel
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The verifier must send a fresh cryptographically random `nonce`; presentations must bind to the verifier `client_id` and that nonce. `transaction_data` is an optional non-empty array of base64url-encoded typed JSON objects; unknown/nonconforming types must be rejected. Wallets should obtain explicit informed consent before releasing credentials or presentations. The result is credential presentation(s), not an access token or action-result receipt. Concrete transaction-data types and the issuer mechanism declaring a credential suitable for them are out of scope.

### V18 — OpenID4VP automatically hashes exact `transaction_data` into every SD-JWT VC key-binding proof

**Verdict: DISPUTED**

- **Direct URL:** https://openid.net/specs/openid-4-verifiable-presentations-1_0.html
- **Publisher:** OpenID Foundation
- **Published/updated:** published 2025-07-09; Final Specification 1.0 with errata channel
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** Appendix B recommends that each transaction-data type define how processed data is returned in the Key Binding JWT and then defines **one profile** using `transaction_data_hashes`. In that profile, each hash covers the exact received encoded string without base64url decoding. The capability is real, but it is not automatic for every SD-JWT VC presentation merely because `transaction_data` is used. An AgentBridge transaction-data type would need to adopt this profile (or define equivalent binding rules).

### V19 — SD-JWT VC is still an active draft and does not by itself establish delegate identity or revocation semantics

**Verdict: VERIFIED**

- **Direct URL:** https://datatracker.ietf.org/doc/html/draft-ietf-oauth-sd-jwt-vc
- **Publisher:** IETF OAuth Working Group / IETF Datatracker
- **Published/updated:** `draft-ietf-oauth-sd-jwt-vc-19`, published 2026-08-31; active Internet-Draft; intended Standards Track; expires 2027-03-04
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The current document is version 19, not a final RFC. It defines an extensible collision-resistant `vct`, selective disclosure, type metadata, optional key binding, and optional credential status. It explicitly says no binding is required between `sub` and `cnf`. If `status` is present it should be checked, but verifier policy decides acceptance and the status mechanism is a separate draft. It is a credential envelope/profile, not a proof that its holder is a user's delegate or a general one-use/revocation lifecycle.

### V20 — OAuth Identity Chaining is an active draft preserving cross-domain identity/authorization context, but leaves claim representation and semantics to agreement

**Verdict: VERIFIED**

- **Direct URL:** https://datatracker.ietf.org/doc/draft-ietf-oauth-identity-chaining/
- **Publisher:** IETF OAuth Working Group / IETF Datatracker
- **Published/updated:** revision 17 dated 2026-07-19; Datatracker last updated 2026-08-21; active Internet-Draft, intended Proposed Standard; in RFC Editor queue, awaiting first editor
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The draft combines intra-domain RFC 8693 exchange with a JWT authorization grant for downstream domains and supports repeated chaining. It directly says the representation of transcribed claims is not defined and that producers/interpreters must agree on semantics and consistent access controls. The digest's “not final” conclusion is correct, though its maturity description omits that the draft has already reached the RFC Editor queue.

### V21 — No current official work defines a signed, domain-neutral agent-action receipt

**Verdict: DISPUTED**

- **Direct URLs:** https://datatracker.ietf.org/doc/draft-noa-scitt-ai-agent-receipt/01/ ; https://datatracker.ietf.org/doc/html/draft-liu-agent-operation-authorization-02
- **Publisher:** individual Internet-Draft authors via IETF Datatracker (not IETF-endorsed standards)
- **Published/updated:** SCITT AI-Agent Action Receipts `-01`, 2026-08-15; Agent Operation Authorization `-02`, 2026-03-16; both active individual Internet-Drafts
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** A current individual draft now defines tamper-evident, signed, offline-verifiable, domain-neutral AI-agent action-receipt records. Another current individual draft defines a fine-grained Agent Operation Authorization request/token. Thus an unqualified claim that no generic signed action-receipt artifact exists is stale. Neither document has IETF consensus or final-standard status.

### V22 — No current reviewed official source closes the combined general action-semantics-to-provable-external-effect gap

**Verdict: VERIFIED**

- **Direct URLs:** https://datatracker.ietf.org/doc/html/draft-liu-agent-operation-authorization-02 ; https://datatracker.ietf.org/doc/draft-noa-scitt-ai-agent-receipt/01/ ; https://openid.net/authorization-api-1-0-final-specification-approved/
- **Publisher:** individual Internet-Draft authors via IETF Datatracker; OpenID Foundation AuthZEN Working Group
- **Published/updated:** Agent Operation Authorization `-02`, 2026-03-16; SCITT AI-Agent Action Receipts `-01`, 2026-08-15; Authorization API 1.0 final approval, 2026-01-12
- **Accessed:** 2026-09-11
- **Exact support/mismatch:** The Agent Operation Authorization draft defines an authorization request/token and ends its flow with “execute or deny” plus an ordinary response; it contains no receipt definition. The SCITT receipt draft defines the record side, but expressly says a valid receipt proves only that a key issued an unaltered record—not that the agent was right or that “the world changed the way the record suggests”; it also points to a separately defined shared action digest. AuthZEN's Authorization API 1.0 is final, but its official approval establishes an authorization-decision API, not a signed execution-effect evidence protocol. Therefore no single current final or consensus-track source inspected here standardizes shared arbitrary-action semantics, delegated approval, execution linkage, and proof of the resulting external effect end to end. This is a bounded result over the official sources reviewed, not proof of absence from every publication ecosystem.

## Count

- **VERIFIED:** 16
- **DISPUTED:** 4
- **UNVERIFIED:** 1
- **OVERTURNED:** 0

