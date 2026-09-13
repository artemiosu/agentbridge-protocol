# Authorization primitives and exact-action binding: standards-gap digest (round 2)

**Decision.** Choose among (A) a new AgentBridge core, (B) a thin interoperability/conformance profile over existing standards, or (C) stop and contribute elsewhere.

**Bottom line.** **B remains the evidence-backed choice, with high confidence.** Existing standards already provide the credential, grant, presentation, actor-chain, audience-binding, proof-of-possession, interaction, selective-disclosure, expiry, revocation, and authorization-receipt primitives needed to authorize an exact action. A new grant or credential protocol is not justified. What does not exist is the application profile that gives the same action and constraints the same meaning across MCP, A2A, and OpenAPI, plus a signed *execution* receipt and lifecycle/compensation semantics. C is premature because those seams remain unstandardized.

**Access date for all sources:** 2026-09-11. **Freshness:** the current SD-JWT VC and identity-chaining drafts are within one month; AP2 was rechecked as current-as-served and is within the three-month AI-standards window. Stable RFC semantics are versioned, immutable standards rather than time-sensitive compatibility claims.

**Epistemics:**

1. Never conclude from training data alone. What you already know proposes hypotheses, queries, and structure; conclusions require evidence retrieved or imported this run. A claim you cannot evidence is stated as an unverified belief or not at all.
2. The research firewall. Project context—briefs, PRDs, code, memory—is inadmissible as evidence. Only this brief and named digest are allowed.

## Evidence ledger

Eight official sources were used for the new standards pass. The one permitted round-1 digest supplied AP2 continuity; AP2 claims below were freshness-checked against the official current document.

| ID | Official source | Publisher | Publication/update and status | Confidence | Class |
|---|---|---|---|---|---|
| S1 | [RFC 9635: GNAP](https://www.rfc-editor.org/info/rfc9635/) | IETF / RFC Editor | October 2024; Standards Track, Proposed Standard | High | Final standards-track protocol |
| S2 | [RFC 9396: OAuth 2.0 Rich Authorization Requests](https://www.rfc-editor.org/rfc/rfc9396.html) | IETF / RFC Editor | May 2023; Standards Track, Proposed Standard | High | Final standards-track extension |
| S3 | [RFC 8693: OAuth 2.0 Token Exchange](https://www.rfc-editor.org/info/rfc8693/) | IETF / RFC Editor | January 2020; Standards Track, Proposed Standard | High | Final standards-track extension |
| S4 | [RFC 9449: DPoP](https://www.rfc-editor.org/info/rfc9449/) | IETF / RFC Editor | September 2023; Standards Track, Proposed Standard | High | Final standards-track extension |
| S5 | [RFC 9728: OAuth 2.0 Protected Resource Metadata](https://www.rfc-editor.org/rfc/rfc9728.html) | IETF / RFC Editor | April 2025; Standards Track, Proposed Standard | High | Final standards-track discovery metadata |
| S6 | [OpenID for Verifiable Presentations 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) | OpenID Foundation | Final Specification 1.0, 2025; current page incorporates errata | High | Final OpenID specification |
| S7 | [SD-JWT-based Verifiable Digital Credentials, draft-ietf-oauth-sd-jwt-vc-19](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-sd-jwt-vc) | IETF OAuth WG | 2026-08-31; active Internet-Draft, intended Standards Track; not final | High on current text; medium on durability | Work in progress |
| S8 | [OAuth Identity and Authorization Chaining Across Domains, draft-ietf-oauth-identity-chaining-17](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-identity-chaining-17) | IETF OAuth WG | 2026-07-19; active Internet-Draft, intended Standards Track; not final | High on current text; medium on durability | Work in progress; only emerging proposal admitted |
| D1 | [AP2 Agent Authorization](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md), as documented in the permitted [round-1 digest](./trust-commerce-r1-1.md) | Google Agentic Commerce / AP2 | Current `main`, AP2 v0.2 family; no immutable release date on the page; current-as-served 2026-09-11 | High on text; medium on ecosystem durability | Living project specification, not an SDO final standard |

S8 is the only emerging identity proposal included. It is directly relevant and normative in style, but generic OAuth cross-domain identity chaining rather than an agent-specific identity standard. No additional OpenID/IETF agent-identity proposal was admitted.

## Capability matrix

Legend: **Y** = the specification normatively supplies the mechanism; **P** = partial, optional, format-dependent, or needs a profile; **N** = not supplied by that specification. “Receipt” means a signed authorization or execution receipt, not merely a signed token.

| Mechanism | GNAP | RAR | Token Exchange | DPoP | Resource metadata | OpenID4VP | SD-JWT VC | AP2 Agent Auth | Identity Chaining draft |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Requested action details | **Y/P**: typed access objects; semantics external | **Y/P**: typed JSON and API fields; semantics external | **P**: `scope`, token claims | **N** | **P**: supported scopes only | **Y/P**: typed `transaction_data`; types external | **P**: extensible claims/`vct`; no request protocol | **Y/P**: mandate types/constraints; only payment types supplied today | **P**: scopes and transcribed claims |
| Principal / delegate / actor chain | **P**: RO, user, client roles; no portable nested actor chain | **N** | **Y**: `subject_token`, `actor_token`, nested `act`, `may_act` | **N** | **N** | **P**: issuer/holder/verifier, not delegation | **P**: issuer/subject/holder/key, not delegation | **Y/P**: user-approved open mandate to agent-signed closed mandate; agent-to-agent delegation out of scope | **Y/P**: identity/authorization propagated across domains; claim representation and semantics external |
| Audience / resource binding | **Y/P**: locations and token rights | **Y/P**: `locations`; composes with OAuth resource indicators | **Y**: `resource`, `audience`, `aud` | **Y/P**: HTTP method + URI and access-token hash; not body/query | **Y**: resource identifier and AS discovery; recommends audience-restricted tokens | **Y**: verifier `client_id` + transaction `nonce` | **Y/P**: KB `aud` + `nonce` when key binding used | **Y**: closed mandate binds to transaction and verifier | **Y**: grant audience is downstream AS; repeats across domains |
| Proof of possession | **Y/P**: key-bound requests/tokens; bearer remains possible | **N** | **N** by itself | **Y** | **N** | **Y/P**: holder binding is format-dependent but normally verified | **Y/P**: `cnf` and KB proof optional unless required by profile | **Y**: open mandate requires `cnf`; agent key binds closed mandate | **P**: can delegate sender constraint using DPoP/mTLS |
| User interaction / consent | **Y**: redirect, user code, app interaction; RO can consent | **P**: supplies details intended to inform OAuth consent | **N** | **N** | **N** | **Y/P**: Wallet should obtain explicit informed consent | **N** | **Y**: deterministic Trusted Surface obtains user authorization/consent | **N** |
| Verifier challenge | **P**: AS interaction/continuation, not an action-verifier proof request | **N** | **N** | **P**: server nonce challenge | **N** | **Y**: authorization request, DCQL, `transaction_data`, fresh `nonce` | **P**: KB proof consumes verifier `aud`/`nonce` | **Y/P**: verifier requests a suitable mandate; carrier/discovery remain unspecified | **N** |
| Selective disclosure | **N** | **N** | **N** | **N** | **N** | **Y/P**: requests only needed claims; credential-format dependent | **Y** | **Y** | **P**: claims may be removed/transcribed, not holder-selective disclosure |
| Revocation / cancellation | **Y**: cancel grant; token management can revoke | **N** itself | **P**: propagation is explicitly not a general property | **N** | **N** | **P**: verifier policy may require revocation checks | **P**: optional `status`, external status mechanism and verifier policy | **N/P**: receipt reduces remaining scope; no portable revocation protocol | **P**: cautions against refresh tokens; no cross-domain revocation protocol |
| Single-use / expiry | **Y/P**: one-use interaction reference/callback, `expires_in`; action token use is profile-dependent | **N** itself | **P**: token expiry possible; exchange/input tokens are not single-use by default | **Y/P**: unique proof per request; strict `jti` replay cache is strong but not always feasible | **N** | **Y/P**: fresh nonce/session invalidation; credential reuse is format/policy dependent | **P**: optional `exp`; no generic single-use rule | **P**: `exp`, signed receipt, scope reduction; not a general single-use credential rule | **P**: recommends short-lived or single-use JWT grants as mitigations |
| Signed receipt | **N** | **N** | **N** | **N** | **N** | **N**: returns a presentation, not an action result | **N** | **Y/P**: signed accept/reject Mandate Receipt; execution-effect fields are optional | **N** |

## Findings

### F1 — Fine-grained exact action data already has two standards-grade carriers

**Claim.** RAR's `authorization_details` is a general JSON carrier for fine-grained authorization. It defines collision-resistant `type` identifiers and reusable `locations`, `actions`, `datatypes`, `identifier`, and `privileges` fields, while allowing type-specific fields such as amount, currency, creditor, path, or other material parameters. OpenID4VP independently defines `transaction_data` as typed, base64url-encoded JSON details that the verifier asks the user to authorize; the Wallet must reject an unknown type, and SD-JWT VC presentations can cryptographically return hashes of the exact encoded transaction-data strings. Therefore an exact tool call can be approved without defining a new grant or credential protocol.

- **URL:** [RFC 9396](https://www.rfc-editor.org/rfc/rfc9396.html); [OpenID4VP 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html)
- **Publisher:** IETF / RFC Editor; OpenID Foundation
- **Published/updated:** 2023-05; Final 1.0 (2025), current errata page accessed 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed protocol capability

### F2 — The exact semantics are intentionally delegated to an application profile

**Claim.** Neither RAR nor OpenID4VP defines the vocabulary of an arbitrary action. RAR says allowable fields and values are determined by the API's `type`; OpenID4VP says concrete `transaction_data.type` values are out of scope and the document defining a type decides which credentials authorize it. GNAP imports essentially the same extensible access-object pattern and says API designers define reference-string relationships and enforcement. This is not a cryptographic gap; it is the precise profile gap AgentBridge could fill.

- **URL:** [RFC 9396](https://www.rfc-editor.org/rfc/rfc9396.html); [OpenID4VP 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html); [RFC 9635](https://www.rfc-editor.org/info/rfc9635/)
- **Publisher:** IETF / RFC Editor; OpenID Foundation
- **Published/updated:** 2023-05; Final 1.0 (2025); 2024-10
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed normative scope boundary

### F3 — OAuth Token Exchange already expresses principal, current actor, prior actors, and an eligible actor

**Claim.** RFC 8693 distinguishes the party represented by `subject_token` from the acting party represented by `actor_token`. In JWT/introspection forms, `act` may nest the current and prior actors, while `may_act` identifies a party authorized to become the actor. Requests can also target `resource` and `audience`. Thus a principal/delegate/actor chain does not require a new token-exchange protocol.

**Limit.** The actor claims identify parties; they do not say which exact MCP tool, A2A task, or OpenAPI operation was approved. `scope` and lifetime are suggested constraints, and the STS's authorization policy remains deployment-specific.

- **URL:** [RFC 8693](https://www.rfc-editor.org/info/rfc8693/)
- **Publisher:** IETF / RFC Editor
- **Published/updated:** 2020-01
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed actor/delegation primitive and scope boundary

### F4 — Token Exchange does not create revocation linkage or single-use semantics

**Claim.** RFC 8693 explicitly says an exchange does not invalidate the subject or actor token, creates no tight linkage between input and output tokens, and does not make revocation propagation a general property. Single-use or other semantics come from the chosen token type or deployment. AgentBridge can reuse Token Exchange for actor propagation but must profile expiry, delegation depth, downscoping, revocation behavior, and replay policy.

- **URL:** [RFC 8693](https://www.rfc-editor.org/info/rfc8693/)
- **Publisher:** IETF / RFC Editor
- **Published/updated:** 2020-01
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed negative capability / profile requirement

### F5 — DPoP proves the caller has the bound key, not that the approved body parameters are unchanged

**Claim.** DPoP signs a unique JWT containing HTTP method (`htm`), target URI (`htu`), issuance time, identifier (`jti`), optional server nonce, and—at protected-resource access—the access-token hash (`ath`). `htu` excludes query and fragment, and DPoP contains no request-body digest. Therefore DPoP cannot by itself bind approval to the material JSON arguments of an MCP call, A2A task, or OpenAPI request. It is the correct sender-constraining layer *after* an action profile binds the approved parameters into RAR details, OpenID4VP transaction data, an AP2 mandate, or a token claim/reference.

**Replay nuance.** Each HTTP request requires a unique proof and servers may reject repeated `jti` values; the RFC notes strict single-use tracking is strong but may not always be feasible. A server nonce narrows pre-generation but is not an action-approval challenge.

- **URL:** [RFC 9449](https://www.rfc-editor.org/info/rfc9449/)
- **Publisher:** IETF / RFC Editor
- **Published/updated:** 2023-09
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed proof-of-possession boundary

### F6 — GNAP already supplies a modern delegated-grant lifecycle, including interaction and revocation

**Claim.** GNAP is a final Standards Track protocol expressly for delegating authorization to software. It supports typed access requests, resource-owner interaction and consent, key-bound client requests, asynchronous continuation, token expiry, grant cancellation, and optional token-management rotation/revocation. Interaction references and callbacks are one-time use. A new AgentBridge core duplicating those grant-state mechanics would be hard to justify.

**Limit.** GNAP is not an OAuth extension and is not directly compatible with OAuth. It does not standardize nested actor-chain claims, selective-disclosure credentials, exact cross-transport action identifiers, action-specific constraint evaluators, or signed action/execution receipts. Its extensible access-object semantics are still assigned to API designers.

- **URL:** [RFC 9635](https://www.rfc-editor.org/info/rfc9635/)
- **Publisher:** IETF / RFC Editor
- **Published/updated:** 2024-10
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed mature core and scope boundary

### F7 — Protected-resource metadata solves bootstrap, not semantic trust

**Claim.** RFC 9728 provides a standard resource identifier, well-known metadata retrieval, associated authorization-server discovery, supported scopes, and optionally signed metadata. It recommends audience-restricted tokens. This can bootstrap an AgentBridge authorization flow and prevent endpoint confusion, but it does not advertise canonical action types, accepted credential issuers, constraint evaluators, execution-receipt formats, or compensation policies. Signed metadata lets an issuer vouch for resource metadata; relying-party trust in that issuer is still policy.

- **URL:** [RFC 9728](https://www.rfc-editor.org/rfc/rfc9728.html)
- **Publisher:** IETF / RFC Editor
- **Published/updated:** 2025-04
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed discovery capability and trust boundary

### F8 — OpenID4VP is already an exact verifier-challenge and consent rail

**Claim.** OpenID4VP 1.0 is final. A verifier sends a fresh `nonce`, verifier `client_id`, a DCQL credential query, and optionally typed `transaction_data`; presentations must be bound to the verifier and nonce. The Wallet should obtain explicit informed consent before releasing credentials, and verifier policy performs applicable trust-framework and revocation checks. With the SD-JWT VC format, hashes of the exact `transaction_data` strings are included in the key-binding proof.

**Limit.** OpenID4VP returns credential presentations, not delegated access tokens or action receipts. Concrete transaction-data types and the mechanism by which issuers declare a credential suitable to authorize them are explicitly out of scope. Its issuer/trust-framework extension points do not define a universal trust policy.

- **URL:** [OpenID4VP 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html)
- **Publisher:** OpenID Foundation
- **Published/updated:** Final Specification 1.0 (2025); current errata page accessed 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Class:** Confirmed final presentation protocol and profile seam

### F9 — SD-JWT VC supplies the privacy-preserving credential envelope, not authority semantics

**Claim.** The current SD-JWT VC draft defines an extensible `vct`, issuer/subject claims, selective disclosures, optional expiry, optional `cnf` key binding, issuer-key discovery, type metadata, and optional credential `status`. The type's claim semantics and additional issuer/validation policy are ecosystem-defined; `status` depends on a separate status mechanism and verifier policy. The draft also says no binding is required between `sub` and `cnf`. It therefore can carry an action mandate but does not itself prove that the holder is the user's delegate or define revocation/single-use of that mandate.

- **URL:** [draft-ietf-oauth-sd-jwt-vc-19](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-sd-jwt-vc)
- **Publisher:** IETF OAuth WG
- **Published/updated:** 2026-08-31; active Internet-Draft, intended Standards Track; not final
- **Accessed:** 2026-09-11
- **Confidence:** High on current text; medium on final shape
- **Class:** Current normative draft / credential-format primitive

### F10 — AP2 already composes consent, delegation, transaction binding, fail-closed constraints, selective disclosure, and a signed authorization receipt

**Claim.** AP2 Agent Authorization separates Mandate Delegation from Action Authorization. A user approves Mandate Content on a Trusted Surface; an open mandate binds the agent's key and constraints; the agent creates a closed mandate bound to a transaction and verifier; unknown constraints fail; and the verifier returns a signed JWT receipt whose mandatory fields are issuer, `success|error`, and a hash of the received mandate. The document permits new mandate and constraint types beyond payments. This is the closest ready-made model for an AgentBridge action-approval credential.

**Limits.** AP2's standardized types remain payment/checkout types; general use is described as future applicability. Agent-to-agent delegation is outside the current AP2 scope according to D1. The challenge carrier and mandate-selection mechanism are not interoperably specified. Its required receipt proves accept/reject of *action authorization*; use-case-specific effect fields are optional, so it is not a generic proof that the external action executed. The document also normatively references a 2026 individual “Delegate SD-JWT” draft, adding maturity risk.

- **URL:** [AP2 Agent Authorization](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md)
- **Publisher:** Google Agentic Commerce / AP2
- **Published/updated:** living `main`, AP2 v0.2 family; no explicit immutable date; current-as-served 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** High on current text; medium on durability/interoperability
- **Class:** Living project specification / normative model with roadmap boundary

### F11 — The one admitted identity-chaining proposal strengthens B, but does not complete it

**Claim.** The IETF OAuth identity-chaining draft uses RFC 8693 plus JWT authorization grants to preserve identity and authorization information as requests cross trust domains. It targets the downstream authorization server with `resource`/`audience`, permits claims transcription/downscoping, can carry a delegated sender constraint for later DPoP or mTLS use, and recommends short-lived or single-use grants against replay.

**Limit.** The draft says the representation of transcribed claims is not defined and parties must agree on semantics; downstream trust in the upstream authorization server's authority remains configuration/policy. It does not define agent-specific roles, an exact action object, user approval, a constraint evaluator, or receipts. It is also an Internet-Draft, not a final RFC.

- **URL:** [draft-ietf-oauth-identity-chaining-17](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-identity-chaining-17)
- **Publisher:** IETF OAuth WG
- **Published/updated:** 2026-07-19; active Internet-Draft, intended Standards Track; not final
- **Accessed:** 2026-09-11
- **Confidence:** High on current text; medium on final shape
- **Class:** Emerging normative proposal / directly relevant generic identity chain

## Exact MCP / A2A / OpenAPI binding assessment

**Answer: yes, cryptographically exact approval is achievable without a new credential or grant protocol, but only after defining an AgentBridge action profile.**

A conforming profile can use either or both of these paths:

1. **OAuth authorization path.** Put a canonical action object in one RAR `authorization_details` element. The object contains the transport binding, operation identity, material arguments or a canonical digest, resource/audience, and constraints. The authorization server approves that object and issues a restricted token; Token Exchange propagates subject/current/prior actors to downstream services; DPoP sender-constrains each HTTP use. GNAP can express the same action object as a typed access item for deployments that choose GNAP instead of OAuth.
2. **Credential/mandate path.** Put the same canonical action object in OpenID4VP `transaction_data` and bind its exact encoding into an SD-JWT VC key-binding proof, or define it as an AP2-compatible mandate type. The agent presents the user-approved/open mandate and transaction-bound/closed mandate; the verifier evaluates the constraints and issues the AP2-style signed authorization receipt.

The following elements are profile work, not new protocol work:

| Required semantic | Why existing machinery is insufficient | Thin-profile obligation |
|---|---|---|
| Canonical action identifier | RAR/GNAP/OpenID4VP/AP2 deliberately leave type values and fields to API/ecosystem designers | Define one collision-resistant action-type namespace and deterministic mappings for MCP tool name + server, A2A operation/skill + agent, and OpenAPI server + operation; do not treat a task/run ID as the reusable operation identity |
| Material parameter binding | DPoP excludes query and body; generic tokens normally authorize a class of calls | Define canonical serialization and whether approval carries full parameters, selected material fields, or a digest; bind the exact object into RAR and/or `transaction_data`/mandate |
| Constraint vocabulary and evaluator | RAR fields, GNAP types, OpenID4VP transaction types, and AP2 constraints are type-specific | Specify schemas, comparison rules, unknown-field/unknown-constraint failure, numeric/unit/time semantics, and deterministic test vectors |
| Issuer and verifier trust policy | Keys, signatures, metadata, and credential presentations prove provenance; acceptance authority remains local policy | Advertise accepted issuers/providers, assurance/trust frameworks, actor/delegation depth, credential status requirements, and policy version |
| Authorization versus execution evidence | AP2's required receipt proves accept/reject of the mandate; other standards return tokens/presentations | Define a signed execution receipt linked to authorization receipt/mandate/action digest, with executor, status, timestamps, effect/output digest, error and evidence references |
| Revocation and single use | GNAP can revoke its own grants/tokens; OAuth exchange has no inherent revocation linkage; SD-JWT status is optional/external | Define action-authorization status, revocation lookup/event propagation, replay keys, expiry, use count, retry/idempotency rules, and fail-closed behavior |
| Compensation | None of the reviewed authorization standards defines undo/refund/remediation semantics | Keep compensation domain-specific; define only generic linkage (`compensates`, policy URI, deadline, result receipt) and let action profiles own the effect |

### Concrete binding caveats

- **MCP:** the authorization object can name a server and tool and hash/carry `params.arguments`; no reviewed authorization standard assigns the canonical MCP identifier or decides which arguments are material.
- **A2A:** the authorization object can name the target agent and action/skill and correlate the resulting task; no reviewed authorization standard supplies the canonical A2A operation identity or equates a task instance with approved semantics.
- **OpenAPI:** the authorization object can name the server and operation and bind the path/query/body material fields; no reviewed authorization standard selects a globally stable operation key or canonical request representation.

These are bounded interoperability findings about the reviewed authorization sources. The round did not inspect the MCP, A2A, or OpenAPI base specifications themselves.

## Status and maturity split

### Standards-track or final now

- **GNAP RFC 9635:** IETF Standards Track Proposed Standard. Broad delegated-grant lifecycle, but a parallel protocol to OAuth rather than an OAuth profile.
- **RAR RFC 9396:** IETF Standards Track Proposed Standard. Mature JSON authorization-details carrier.
- **Token Exchange RFC 8693:** IETF Standards Track Proposed Standard. Mature subject/actor/audience exchange and actor claims.
- **DPoP RFC 9449:** IETF Standards Track Proposed Standard. Mature application-layer sender constraint and request proof.
- **Protected Resource Metadata RFC 9728:** IETF Standards Track Proposed Standard. Mature resource/AS discovery metadata.
- **OpenID4VP 1.0:** OpenID Foundation Final Specification. Mature verifier request/presentation/transaction binding, with format/profile dependencies.

“Proposed Standard” is a final RFC maturity level on the IETF Standards Track, not an Internet-Draft and not the terminal “Internet Standard” level.

### Draft, living specification, or roadmap

- **SD-JWT VC -19:** active IETF Internet-Draft intended for Standards Track; not final. Its underlying SD-JWT format is already RFC 9901, but the VC claims/metadata profile remains work in progress.
- **OAuth Identity Chaining -17:** active IETF Internet-Draft intended for Standards Track; not final and not agent-specific.
- **AP2 Agent Authorization:** living project specification in the AP2 v0.2 family, not an IETF/OpenID final standard. Generic non-payment use and agent-to-agent delegation remain future/out-of-scope; a referenced Delegate SD-JWT building block is an individual 2026 draft.

## Decision judgment

### Choose B: thin interoperability/conformance profile

**Judgment.** Standardize the seams, not another authorization core. The profile should normatively reuse:

- RAR or GNAP typed access objects for the requested action;
- RFC 8693 `act`/`may_act` and Token Exchange for OAuth delegate chains;
- RFC 9728 for protected-resource/authorization-server bootstrap;
- DPoP for sender-constrained HTTP execution;
- OpenID4VP `transaction_data` plus SD-JWT VC for user-approved, selectively disclosed, verifier-challenged evidence;
- AP2's open/closed mandate and signed authorization-receipt pattern where its draft dependencies are acceptable.

AgentBridge's own normative surface should be the canonical action schema, mappings, deterministic constraint evaluation, trust-policy metadata, execution receipt, status/revocation behavior, and cross-transport conformance vectors. Domain profiles should define compensation.

**Why not A.** A new core would duplicate mature RFC grant/token/PoP and final OpenID presentation machinery. The evidence does not show an inability to express exact action parameters; it shows deliberately open extension points awaiting shared semantics.

**Why not C.** The standards do not agree on how an MCP/A2A/OpenAPI action is named and canonicalized, how constraints are evaluated identically, how issuer policy is discovered, or how authorization is linked to a signed effect result. Contributing the profile upstream is sensible, but merely stopping leaves those gaps to bilateral conventions.

**Reversibility hedge.** Define the action object independently of carrier, with normative mappings to RAR, GNAP, and OpenID4VP/AP2. If AP2 generic authorization stabilizes upstream, AgentBridge can become only a conformance suite and registry/profile with no incompatible credential migration.

## Contradictions and tensions

1. **“Exact action” is both solved and unsolved.** RAR and OpenID4VP can carry and cryptographically bind arbitrary exact JSON. They intentionally do not define what any action type means. The wire/crypto problem is solved; semantic interoperability is not.
2. **DPoP request binding is narrower than action binding.** It signs method and URI but omits query and body, so treating DPoP as approval of tool arguments would be incorrect.
3. **Token Exchange has an actor chain but not an approval chain.** Nested `act` records who acted; it does not itself prove the human saw or approved the eventual parameterized action.
4. **OpenID4VP has transaction authorization but not delegated execution.** It binds a verifier challenge and user credential presentation to transaction data; it neither issues an action token nor reports execution.
5. **SD-JWT VC advertises `status`, but revocation remains external.** Presence of the claim does not supply the status mechanism, propagation policy, or rejection rule.
6. **AP2 is generic in model and payment-specific in standardization.** Its document permits new types and describes a general action flow, but current standardized mandate types, deployment evidence, and protocol integration are payment-oriented; agent-to-agent delegation is not current scope.
7. **AP2 receipt wording can overstate effects.** `result=success` is normatively the result of action authorization. Rich external-world effect evidence is optional and must not be inferred.
8. **Identity chaining preserves context but acknowledges semantic agreement.** The draft can propagate identity and authorization across trust domains, yet explicitly leaves transcribed-claim representation undefined and requires parties to agree on semantics.
9. **GNAP is the most complete grant lifecycle but the least incremental OAuth choice.** It eliminates a reason to invent a new core while making an OAuth-first AgentBridge deployment less direct because GNAP is intentionally not OAuth-compatible.

## Leads and unfound

### Leads

1. Build one carrier-neutral test vector for a consequential non-commerce action, for example `deploy service`, including material arguments. Encode the identical action under RAR, GNAP, OpenID4VP `transaction_data`, and an AP2-compatible mandate; require byte/digest equivalence and negative tests.
2. Inspect the MCP, A2A, and OpenAPI normative specifications next for their most stable operation identity and request canonicalization surfaces; this round only proved the authorization carriers can hold whatever mapping is chosen.
3. Prototype whether RFC 8693 nested `act` plus AP2's user→agent mandate chain can be reconciled without duplicating identities or leaking unnecessary prior actors.
4. Track SD-JWT VC -19 and OAuth Identity Chaining -17 until RFC publication, and isolate draft-dependent fields behind versioned carrier mappings.
5. Decide whether to contribute a general `transaction_data`/mandate action type upstream to OpenID/AP2 while keeping AgentBridge as the conformance and execution-receipt profile.

### Searched but not found in the reviewed official sources

- A finalized, agent-specific OpenID/IETF identity standard that combines human principal, multi-agent delegation, exact action approval, and execution evidence.
- A standardized canonical identifier for the same operation across MCP, A2A, and OpenAPI.
- A DPoP mechanism that binds query parameters or request bodies.
- A general revocation linkage across exchanged tokens and delegated credentials.
- A discovery vocabulary for accepted mandate issuers, assurance levels, constraint evaluators, or delegation depth.
- A final standards-track generic mandate/action credential based on AP2; AP2's generic model is living and its Delegate SD-JWT dependency is an individual draft.
- A signed, domain-neutral execution receipt proving the side effect rather than only grant issuance, credential presentation, or authorization acceptance.
- Generic compensation semantics; this remains domain and application policy.

