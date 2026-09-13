# Trust, authorization, and commerce protocols: standards-gap digest (round 1)

**Research question.** Does a domain-independent AgentBridge authorized-action lifecycle fill a real standards gap, and should the project choose (A) a new protocol core, (B) an interoperability/conformance profile over existing standards, or (C) no separate protocol?

**Access date for every source:** 2026-09-11. **Evidence policy:** primary sources only; current living specifications and the latest stable ACP snapshot were checked. Project files were not used as evidence.

## Bottom line

**Provisional decision: B — build a thin interoperability and conformance profile, not a wholly new protocol core. Confidence: medium-high.**

There is a real standards gap, but it is narrower than “no authorization protocol exists.” AP2 already specifies a domain-neutral *authorization skeleton*: user-to-agent mandate delegation, verifier challenge, transaction binding, deterministic verification, and a signed accept/reject receipt. UCP already specifies discoverable profiles, capability/version negotiation, identity/key discovery, HTTP signatures, and bindings for REST/OpenAPI, MCP/OpenRPC, and A2A. The strongest opportunity is therefore a profile that composes those pieces for arbitrary consequential actions and supplies the lifecycle elements none of them defines end to end: a common action description and constraint contract, portable verifier challenge, issuer/trust-policy discovery, execution-result evidence distinct from authorization acceptance, delegation/revocation, async state, compensation, and cross-transport conformance tests.

Choosing A would duplicate substantial AP2 and UCP machinery. Choosing C would leave real interoperation gaps that today require private mappings, allowlists, issuer agreements, or domain-specific adapters.

## Primary-source ledger (8 distinct sources)

| ID | Source | Publisher | Publication / update status | Essential quote | Confidence | Class |
|---|---|---|---|---|---|---|
| S1 | [UCP draft specification overview](https://ucp.dev/draft/specification/overview/) | Universal Commerce Protocol project (Google/Shopify-led) | Living draft; page gives no last-updated date; current draft retrieved 2026-09-11 | “Authority binding ... guarantees provenance, not trust.” | High | Normative specification |
| S2 | [UCP AP2 Mandates Extension](https://ucp.dev/draft/specification/payment/extensions/ap2-mandates/) | Universal Commerce Protocol project | Living draft; page gives no last-updated date; retrieved 2026-09-11 | “the session is Security Locked” | High | Normative extension specification |
| S3 | [ACP official repository](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) | Agentic Commerce Protocol project; maintained by OpenAI and Stripe | Current repository retrieved 2026-09-11; latest stable snapshot advertised as 2026-04-17; status `beta` | “currently in `beta`” | High | Official specification index / status |
| S4 | [ACP Agentic Checkout OpenAPI, 2026-04-17](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.agentic_checkout.yaml) | Agentic Commerce Protocol project | Stable dated snapshot 2026-04-17; retrieved 2026-09-11 | “MUST create an order” | High | Normative machine-readable specification |
| S5 | [ACP Delegate Payment OpenAPI, 2026-04-17](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.delegate_payment.yaml) | Agentic Commerce Protocol project | Stable dated snapshot 2026-04-17; retrieved 2026-09-11 | “controlled usage ... per the Allowance constraints” | High | Normative machine-readable specification |
| S6 | [AP2 protocol specification v0.2](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md) | Google Agentic Commerce / AP2 project | Current `main`, v0.2; document gives no last-updated date; retrieved 2026-09-11 | “operates as a security feature within a Commerce Protocol” | High | Normative specification |
| S7 | [AP2 Agent Authorization](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md) | Google Agentic Commerce / AP2 project | Current `main`; document gives no last-updated date; retrieved 2026-09-11 | “the model could be applied more generally in the future” | High | Normative model plus explicit future-positioning |
| S8 | [x402 specification v2](https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md) | x402 Foundation | Current `main`, protocol version 2; document gives no last-updated date; retrieved 2026-09-11 | “client-side budget management” is outside scope | High | Normative payment specification |

Date caveat: UCP draft, AP2 `main`, and x402 `main` are living documents without an explicit page-level update date. They were fetched on the access date and are treated as current-as-served, not as immutable releases. ACP is the only reviewed protocol with a clearly advertised stable dated snapshot.

## Lifecycle coverage matrix

Legend: **Y** = normatively covered; **P** = partial, optional, or domain-limited; **N** = absent/out of scope in the reviewed specification.

| Lifecycle element | UCP | ACP | AP2 | x402 v2 |
|---|---:|---:|---:|---:|
| Discovery / endpoint bootstrap | **Y**: `/.well-known/ucp`, platform profile URL, service endpoints | **N**: merchant/API credentials are assumed | **N**: commerce APIs and discovery are out of scope | **P**: server returns requirements; resource metadata exists, but no general agent/authority discovery |
| Capability negotiation | **Y**: service, capability, extension, handler intersection | **P**: checkout/payment-handler capabilities within a session | **P**: extensible mandate/constraint types, but no general negotiation mechanism | **P**: accepted scheme/network/payment requirements |
| User intent and constraints | **P**: policies and checkout state; strong intent proof only through optional AP2 extension | **P**: authoritative checkout plus payment `Allowance`; no general user-intent mandate | **Y**: open mandates constrain later closed mandates | **N**: payment requirements are seller terms, not a principal-to-agent mandate |
| Cart / checkout | **Y**: catalog, cart, checkout, order capabilities | **Y**: create/update/get/complete/cancel checkout session | **P**: signs/binds an externally supplied checkout object; construction is out of scope | **N** |
| Delegated authority / mandates | **P**: OAuth identity linking and optional AP2 extension | **P**: delegated credential token with one-time amount/session/merchant/expiry allowance | **Y**: explicit Mandate Delegation and Action Authorization phases | **N**: payer signature authorizes a transfer, not principal-to-agent delegation |
| Human approval / autonomous mode | **P**: AP2 extension specifies consent routes; core UCP does not | **N/P**: checkout/intervention states exist; no portable proof of user approval in core stable spec | **Y**: human-present and human-not-present flows; Trusted Surface must be deterministic | **N** |
| Payment and credentials | **Y/P**: payment-handler discovery and credential acquisition architecture; rails are handler-specific | **Y/P**: merchant PSP rails plus delegate-payment tokenization; stable endpoint currently supports only card | **P**: authorizes checkout/payment; payment instrument and actual rail are intentionally agnostic | **Y**: payment requirement, signed payload, verification, settlement response |
| Action execution | **P**: `complete_checkout`, order state, and domain operations | **Y** for checkout: completion must create an order | **P**: authorizes commerce roles; commerce APIs and general execution are outside scope | **Y** for paid-resource fulfillment and settlement only |
| Verifiable result / error | **P**: typed protocol/domain errors; signatures; AP2 adds bound evidence | **P**: authoritative state, typed errors, idempotent replay; responses are not general signed effect receipts | **Y/P**: signed mandate receipts bind success/error to the presented mandate, but generic effect details are optional | **P**: verification/settlement response and protected-resource response, not a generic action receipt |
| Audit / non-repudiation | **P**: HTTP signatures and optional AP2 mandate evidence | **P**: request IDs/idempotency; optional signature headers; no portable audit bundle | **Y/P**: mandate/receipt tuple supports dispute evidence; retention and retrieval are out of scope | **P**: signed payments and settlement evidence; no full agent-decision audit trail |
| Refunds / disputes / compensation | **P**: order lifecycle includes returns and policies; generic compensation and dispute rules absent | **N** in stable checkout core; returns/exchanges and PSP semantics are outside scope | **P**: evidence verification is defined; dispute resolution, retention, retrieval, refunds are out of scope | **N** in core v2 |
| Versioning | **Y**: date-based protocol/component versions, older profiles, request-time selection | **Y**: dated snapshots and required `API-Version` | **Y**: AP2 v0.2 plus exact `vct` schema suffix matching | **Y/P**: core version 2 plus scheme/network compatibility |
| Conformance | **P/Y**: normative processing rules and schema resolution; no reviewed cross-protocol authorized-action suite | **P**: OpenAPI/JSON Schema validation; no reviewed generic lifecycle suite | **P**: verification algorithms are normative; no reviewed broad conformance suite | **P**: schemas/SDKs; no reviewed agent-authority conformance profile |

## Findings

### F1 — UCP standardizes commerce interoperability and transport composition, not a domain-independent authorized-action lifecycle

**Claim.** UCP is the broadest reviewed commerce protocol. Its draft normatively defines bilateral profile discovery, service/capability/extension/payment-handler negotiation, date-based version selection, identity/key discovery, HTTP Message Signatures, typed negotiation failures, and thin bindings for REST/OpenAPI, MCP/OpenRPC, A2A Agent Cards, and embedded channels. Its standard capabilities remain commerce-oriented (catalog, cart, checkout, order, identity linking, location), so it does not define an arbitrary action schema, universal delegated constraints, or generic action outcome.

- **Direct source:** S1, https://ucp.dev/draft/specification/overview/
- **Publisher:** Universal Commerce Protocol project
- **Publication/updated:** living draft, no explicit updated date; current as retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative scope

### F2 — UCP can compose with MCP, A2A, and OpenAPI at the payload/transport layer without bespoke wire formats

**Claim.** UCP explicitly maps REST services to OpenAPI 3.x, MCP services to OpenRPC and `tools/call` with UCP payloads in `params.arguments`, and A2A to an A2A Extension carrying structured UCP types. Profiles advertise transport, endpoint, schema, and version. This is an exact normative seam, not an inferred adapter design.

**Limit.** This only makes the UCP commerce vocabulary portable across those transports. It does not make an arbitrary MCP tool or A2A task an authorized action; a party still needs a shared UCP capability/extension schema and common verification policy.

- **Direct source:** S1, https://ucp.dev/draft/specification/overview/
- **Publisher:** Universal Commerce Protocol project
- **Publication/updated:** living draft, no explicit updated date; current as retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative seam plus bounded inference

### F3 — UCP removes the need for pre-shared authentication secrets in one mode, but does not create transitive trust

**Claim.** With RFC 9421 HTTP Message Signatures, UCP profiles publish keys and can authenticate a domain/profile without prior API-key exchange. However, UCP expressly says namespace/schema authority binding establishes provenance, not trust, and the client decides whether to trust or implement an entity. API keys, OAuth, and mTLS still require prior credential exchange, while profiles with HTTP signatures solve key discovery and message integrity only.

**Missing piece.** There is no normative reputation, accreditation, issuer federation, assurance-level vocabulary, or portable authorization-policy decision. “No private agreement” is true for cryptographic onboarding, false for deciding which platform, provider, credential issuer, mandate type, or constraint evaluator is acceptable.

- **Direct source:** S1, https://ucp.dev/draft/specification/overview/
- **Publisher:** Universal Commerce Protocol project
- **Publication/updated:** living draft, no explicit updated date; current as retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed trust boundary

### F4 — UCP’s AP2 extension is a concrete, normative composition of discovery, checkout, and mandate proof

**Claim.** UCP defines the exact AP2 carrier and downgrade-prevention behavior. Both profiles advertise `dev.ucp.common.payment.ap2_mandate`; if it is in the negotiated intersection, the checkout session becomes security-locked. The business must sign checkout terms, the platform must verify them and submit a checkout mandate at completion, and the PSP verifies the payment mandate. The extension specifies placement, JCS canonicalization, JWS/SD-JWT binding, key discovery, and mandate-specific errors.

**Why it matters.** This is strong evidence that a profile-over-existing-standards approach is viable. The seam is domain-specific: it binds a UCP checkout and payment credential, not arbitrary OpenAPI operations, MCP tool calls, or A2A tasks.

- **Direct source:** S2, https://ucp.dev/draft/specification/payment/extensions/ap2-mandates/
- **Publisher:** Universal Commerce Protocol project
- **Publication/updated:** living draft, no explicit updated date; retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative integration

### F5 — ACP is a stable-shaped but beta, REST/OpenAPI commerce contract; it is not an authority protocol

**Claim.** ACP’s official repository describes a beta standard maintained by OpenAI and Stripe, with latest stable dated snapshot 2026-04-17. The stable checkout OpenAPI defines bearer-authenticated merchant endpoints to create, update, retrieve, complete, and cancel sessions; completion must create an order. Responses carry authoritative cart/session state, typed errors, required API version, and idempotency on mutations.

**Scope boundary.** The stable contract is explicitly a merchant-implemented REST API for ChatGPT-driven checkout. It does not advertise an MCP or A2A binding, agent-card/profile discovery, a portable principal mandate, issuer trust discovery, or signed execution receipts. It can be called from MCP/A2A through an implementation adapter, but the mapping and trust relationship are not standardized by ACP.

- **Direct sources:** S3, https://github.com/agentic-commerce-protocol/agentic-commerce-protocol ; S4, https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.agentic_checkout.yaml
- **Publisher:** Agentic Commerce Protocol project (OpenAI and Stripe maintainers)
- **Publication/updated:** stable snapshot 2026-04-17; repository current as retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative scope/status

### F6 — ACP delegated payment is constrained credential delegation, not proof of authority for the purchase or arbitrary action

**Claim.** ACP’s `delegate_payment` endpoint tokenizes a payment credential under a required `Allowance`: one-time use, maximum amount, currency, checkout-session ID, merchant ID, and expiry. The resulting vault-token ID lets the merchant’s PSP charge within that commercial scope. The stable endpoint currently supports card credentials only.

**Missing piece.** Bearer API authentication authorizes access to the tokenization service, while the Allowance constrains the credential. The specification does not cryptographically prove which human approved the purchase, bind a user mandate to agent identity, define a verifier challenge, or return a signed proof that the downstream merchant action executed. Thus it composes operationally with ACP checkout, but not as a general delegated-authority chain.

- **Direct source:** S5, https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.delegate_payment.yaml
- **Publisher:** Agentic Commerce Protocol project
- **Publication/updated:** stable snapshot 2026-04-17; retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative semantics and scope boundary

### F7 — AP2 defines the closest existing domain-independent authorized-action core, but only standardizes payment mandate types today

**Claim.** AP2 Agent Authorization separates (1) Mandate Delegation, where a user approves content on a Trusted Surface, from (2) Action Authorization, where a verifier asks for proof, validates a transaction-bound mandate, and returns a signed receipt. Open mandates carry constraints and a proof-of-possession key; closed mandates bind to a transaction. Unknown constraints must fail. The model permits new mandate and constraint types and explicitly says it could be applied more generally in the future.

**Spec versus roadmap.** The delegation, verification, and receipt mechanics are normative now. Domain-independent deployment is not: “more generally in the future” is positioning, and the only standardized AP2 mandate types in the reviewed protocol are Checkout and Payment. The mechanism for selecting a mandate is also explicitly an implementation detail.

- **Direct sources:** S7, https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md ; S6, https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md
- **Publisher:** Google Agentic Commerce / AP2 project
- **Publication/updated:** current `main`; AP2 protocol v0.2; no explicit document update dates; retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative core plus explicit roadmap boundary

### F8 — AP2 provides strong authorization and dispute evidence, but not discovery, execution orchestration, or dispute operations

**Claim.** AP2 requires deterministic verification even where roles are agentic, cryptographically binds merchant checkout terms to checkout and payment mandates, and returns signed receipts on acceptance or rejection. A checkout/payment mandate-and-receipt tuple can provide non-repudiable dispute evidence.

**Missing pieces explicitly acknowledged by AP2.** Catalog APIs, checkout updates, and role communication APIs are outside scope; agent-to-agent mandate delegation is outside the current spec; dispute resolution, retention, and evidence retrieval are outside scope; and the agent currently selects applicable mandates/disclosures ad hoc. A receipt’s mandatory generic fields are issuer, `success|error`, and a hash reference; rich execution output is use-case-specific and optional. Therefore a receipt proves a verifier’s mandate decision, not a universally modeled external-world effect.

- **Direct sources:** S6, https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md ; S7, https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md
- **Publisher:** Google Agentic Commerce / AP2 project
- **Publication/updated:** current `main`, AP2 v0.2; no explicit update dates; retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative scope gaps

### F9 — AP2’s trust establishment still depends on verifier policy and usually prior trust relationships

**Claim.** AP2 offers two trust models. A verifier may trust a user-credential issuer to vouch that a Trusted Surface obtained consent, or it may trust an Agent Provider directly. The spec says the provider approach requires verifiers to establish trust with every Agent Provider. OpenID4VP and SD-JWT VC standardize presentation, selective disclosure, key binding, and integrity; they do not tell a verifier which issuer/provider to trust.

**Composition consequence.** AP2 artifacts can ride inside UCP and could be transported through MCP, A2A, or an OpenAPI body, but cryptographic format portability is not permissionless semantic acceptance. Without a common trust-list/federation policy and common mandate/constraint semantics, counterparties still need an out-of-band agreement.

- **Direct source:** S7, https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md
- **Publisher:** Google Agentic Commerce / AP2 project
- **Publication/updated:** current `main`; no explicit update date; retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed trust seam and interoperability inference

### F10 — x402 standardizes pay-to-access across transports, not delegated agent authority

**Claim.** x402 v2 defines a transport-independent payment requirement/payload core with version, resource metadata, accepted scheme/network/amount/asset/payee, verification, and settlement. It advertises HTTP, MCP, and A2A integrations. This is valuable for metered tool/API access and returns payment verification/settlement evidence.

**Boundary.** x402 does not distinguish a human principal from an agent, define a human approval ceremony, publish a reusable user mandate, model arbitrary non-payment constraints, or provide a general action receipt/compensation lifecycle. Client-side budget management is explicitly outside core scope. Agent spend controls in vendor SDKs are therefore implementation policy, not x402 protocol semantics.

- **Direct source:** S8, https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md
- **Publisher:** x402 Foundation
- **Publication/updated:** current `main`, protocol v2; no explicit update date; retrieved 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative scope

### F11 — The standards gap is an end-to-end *composition contract*, not missing cryptography or transports

**Claim.** No reviewed source normatively joins all of the following for arbitrary actions: discoverable action capability; canonical proposed action; principal intent and constraints; agent/delegate chain; verifier-requested proof; deterministic authorization decision; idempotent execution; async progress; signed effect result; audit retrieval; revocation/cancellation; and compensation/dispute semantics across MCP, A2A, and OpenAPI.

The reusable pieces are already present:

- UCP: profiles, versions, capabilities, key discovery, HTTP signatures, negotiation, MCP/A2A/OpenAPI bindings.
- AP2 Agent Authorization: open/closed mandates, proof-of-possession, constraints, challenge/presentation, deterministic verification, signed decision receipt.
- ACP: authoritative state, idempotency, checkout/order/error patterns, constrained delegated payment credential.
- x402: paywall requirements, payment verification, settlement result, transport extensions.

The missing normative seams are:

1. A domain-neutral `Action`/`ActionType` contract that canonically binds a mandate to an MCP tool call, A2A task, or OpenAPI operation and its material parameters.
2. A standard verifier challenge describing required mandate type, constraint vocabulary, issuer/assurance policy, freshness, audience, and evidence format.
3. Discovery of trust policy—not just keys—including accepted issuers/providers, assurance levels, delegation depth, revocation endpoints, and constraint evaluators.
4. A distinction between **authorization receipt** (proof accepted/rejected) and **execution receipt** (what side effect actually occurred), with correlation, partial/async states, and verifiable output/error.
5. Portable cancellation/revocation, retry/idempotency, evidence retrieval/retention, and compensation/refund/dispute hooks.
6. Cross-transport conformance vectors proving semantically identical behavior over MCP, A2A, and OpenAPI.

- **Direct sources:** S1–S8 (URLs in source ledger)
- **Publishers:** UCP project; ACP project; Google Agentic Commerce/AP2; x402 Foundation
- **Publication/updated:** current living specs as accessed, plus ACP stable 2026-04-17
- **Accessed:** 2026-09-11
- **Confidence:** Medium-high
- **Class:** Evidence-backed synthesis / absence claim bounded to reviewed primary sources

## Generalizable versus commerce-specific elements

### Generalizable now

- Profile and endpoint discovery; capability and version intersection; namespace authority binding.
- Transport bindings and schema references for REST/OpenAPI, MCP/OpenRPC, and A2A Agent Cards/extensions.
- Domain/key discovery, HTTP message integrity, canonicalization, idempotency, correlation IDs, typed errors.
- Open versus closed authorization artifacts; proof-of-possession; selective disclosure; constraint evaluation with fail-closed unknown constraints.
- Verifier challenge, mandate presentation, deterministic verification, and a signed accept/reject receipt.
- State-machine patterns such as proposed/ready/escalation/in-progress/completed/canceled.

### Commerce- or payment-specific

- Catalog, SKU/line item, cart, tax, discounts, fulfillment, checkout, order, returns, merchant/PSP/credential-provider roles.
- Payment handlers, payment instruments, PCI flags, 3DS, currency/minor units, payee/merchant IDs, card tokenization.
- AP2 Checkout and Payment Mandate schemas and their merchant/PSP verification responsibilities.
- ACP checkout endpoints and delegated-payment Allowance fields.
- x402 payment requirements, schemes, networks, asset transfer, facilitator verification, and settlement.

### Generalizable only after profiling

- “Intent” is not interoperable without an action-type-specific constraint vocabulary and deterministic evaluator.
- A signed receipt is not a general result proof until action-specific effect fields, issuer duties, failure taxonomy, and evidence retrieval are standardized.
- Published keys establish identity/provenance, not trustworthiness, delegation legitimacy, or issuer assurance.
- A state machine is reusable, but cancellation and compensation semantics remain action/domain-specific.

## Exact composition assessment: can these work with MCP/A2A/OpenAPI without private agreements?

| Pairing | Wire-level composition | Semantic/trust composition without private agreement? | Exact seam / failure point |
|---|---|---|---|
| UCP + MCP | **Yes, normative** | **Yes for shared UCP capabilities; no for arbitrary MCP tools** | MCP `tools/call`; UCP payload in `params.arguments`; schemas through OpenRPC. Missing canonical mapping from any tool to a mandate/action type. |
| UCP + A2A | **Yes, normative but thin** | **Yes for a declared UCP A2A extension; otherwise no** | Business exposes A2A agent and UCP Extension/Agent Card. Missing generic mandate/challenge/effect semantics. |
| UCP + OpenAPI | **Yes, normative** | **Yes for UCP REST operations; otherwise no** | Service profile points to OpenAPI 3.x and endpoint. Arbitrary OpenAPI operations lack UCP capability and authority metadata. |
| UCP + AP2 | **Yes, detailed and normative** | **Only when counterparties accept the AP2 trust model/issuer/provider** | Negotiated AP2 extension, security lock, checkout signature, checkout/payment mandates, PSP verification. Key discovery exists; trust acceptance still local. |
| ACP + MCP/A2A | **Adapter possible, not specified** | **No** | ACP publishes REST/OpenAPI and bearer auth only in reviewed stable sources; no MCP/A2A binding or agent-profile trust semantics. |
| ACP + OpenAPI | **Yes, native** | **No permissionless trust** | Machine-readable stable OpenAPI, required API version, bearer credential. Credential provisioning/merchant relationship is out of band. |
| AP2 + MCP/A2A/OpenAPI | **Carrier-neutral in principle; UCP binding exists** | **No for arbitrary actions** | Mandates are portable VDCs, but AP2 does not define generic transport binding, mandate discovery/selection, accepted issuer policy, or non-payment action types. |
| x402 + MCP/A2A/HTTP | **Yes, specified by transport extensions/core model** | **Yes for pay-to-access mechanics; no for delegated principal authority** | Requirement/payload/verification/settlement compose; budgets and human-to-agent mandate remain outside core. |

## Decision implications

### Why B is favored

1. **Avoids duplicate core cryptography.** AP2 already has most of the mandate chain and decision-receipt mechanics; UCP already demonstrates negotiated use of AP2.
2. **Targets the evidenced gap.** A profile can define action binding, trust-policy discovery, result evidence, lifecycle/compensation hooks, and conformance while reusing SD-JWT/OpenID4VP, HTTP signatures, MCP/A2A/OpenAPI, and existing domain protocols.
3. **Offers incremental adoption.** Commerce can map UCP/ACP/AP2/x402 actions first, while other domains register their own action and constraint profiles.
4. **Keeps domain semantics where they belong.** Refund, dispute, fulfillment, payment, and other compensation behavior should remain domain profiles rather than bloating a universal core.

### What would justify A instead

A new core becomes justified only if experiments show that AP2’s artifact model cannot safely support non-payment actions—for example, if its SD-JWT chain, transaction binding, receipt semantics, or trust model cannot express multi-step, asynchronous, revocable, or jointly authorized actions without incompatible changes. No reviewed source proves that impossibility.

### What would justify C instead

No separate work would be reasonable if UCP generalizes its profile/negotiation layer beyond commerce *and* AP2 standardizes generic action types, transport bindings, trust-policy discovery, execution receipts, revocation, and conformance. The reviewed current sources do not do so.

## Leads for round 2

1. Test a concrete non-commerce action (for example, “deploy service,” “send regulated data,” or “sign contract”) by mapping it to AP2 Agent Authorization plus MCP/A2A/OpenAPI. Record every field or verifier rule that requires invention.
2. Inspect the normative MCP and A2A authorization/extension specifications directly for operation identity, task correlation, security schemes, signed artifacts, and result semantics; this round only evaluated their seams as defined by UCP/x402.
3. Inspect OpenID4VP, SD-JWT VC, OAuth Rich Authorization Requests/transaction authorization, and emerging OpenID agent-identity work for accepted-issuer policy, actor/delegation claims, revocation, and transaction-data semantics.
4. Run interoperability vectors across at least two UCP/AP2 implementations, especially capability negotiation, canonicalization, security lock, mandate errors, and receipt storage.
5. Decide whether an AgentBridge profile should normatively extend AP2 Agent Authorization or define an isomorphic envelope with bindings to AP2; prefer extension until a demonstrated incompatibility appears.

## Contradictions and tensions

1. **UCP permissionless onboarding versus trust.** HTTP signatures and profile key discovery avoid pre-shared secrets, yet UCP explicitly says authority binding proves provenance, not trust. Marketing shorthand must not collapse authentication into authorization or reputation.
2. **AP2 algorithm rule.** AP2 v0.2’s main specification requires a non-deterministic signature for checkout JWTs, while the UCP AP2 extension records an AP2 issue tracking a shift toward an entropy-based rule that could allow Ed25519. Implementations should follow the authoritative AP2 resolution; current cross-spec guidance is not fully settled.
3. **AP2 generic present, generic future.** The Agent Authorization document contains normative generic mechanics and allows new types, while simultaneously describing broader applicability as future. It is a reusable foundation, not evidence of a deployed general protocol.
4. **Receipt terminology.** AP2 calls the verifier response a Mandate Receipt and permits use-case-specific fields, but its required generic `success|error` reports authorization acceptance/rejection. Treating it as proof that an arbitrary side effect completed would exceed the normative guarantee.
5. **UCP AP2 schema versus negotiated obligation.** The extension’s `checkout_mandate` field is structurally optional, but becomes mandatory when AP2 is negotiated. Conformance therefore requires session-aware processing, not JSON Schema validation alone.
6. **ACP openness versus current integration shape.** ACP is presented as an open standard for agents and businesses, while its stable OpenAPI description is specifically “ChatGPT-driven checkout” and assumes bearer credentials. Broader permissionless agent interoperability is not established by the stable contract alone.

## Searched but not found

- No normative, domain-independent specification implementing the complete proposed authorized-action lifecycle across discovery, delegation, proof challenge, execution, verifiable effect, audit retrieval, revocation, and compensation.
- No ACP-native MCP or A2A binding in the reviewed official stable specification.
- No AP2-native general action-type registry, generic transport binding, mandate-selection query language, A2A mandate delegation, evidence-retrieval protocol, revocation protocol, or dispute-resolution process.
- No protocol-level x402 human-to-agent delegation or client budget mandate; budget management is explicitly outside the core.
- No trust framework in these sources that turns key discovery into permissionless acceptance of issuers, agent providers, assurance levels, or constraint evaluators.
- No reviewed cross-protocol conformance suite demonstrating equivalent authorized-action behavior over MCP, A2A, and OpenAPI.
- A search for a normative OpenID “agentic AI authorization profile” surfaced an official identity-management paper, but not a finalized normative domain-independent action-authorization lifecycle; it was therefore not used as protocol evidence in this digest.
