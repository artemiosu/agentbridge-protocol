# Digest: security architecture and reusable primitives

Accessed: 2026-09-12

Decision signal: **native semantic core with optional fail-closed bridges**. Reuse mature cryptography, sender-constrained authority, authorization decisions, canonical encodings, and transparency receipts; AgentBridge must own the end-to-end action processing model.

## Findings

1. RAR is a useful fine-grained authorization envelope but deliberately does not standardize arbitrary authorization-object comparison or field meaning; it cannot decide whether a changed action remains within approval.
   - Source: IETF/RFC Editor, RFC 9396 — https://www.rfc-editor.org/rfc/rfc9396.html
   - Status: Proposed Standard, 2023-05
   - Confidence/class: high; foundational primitive/design limit

2. OAuth Token Exchange supplies subject/actor vocabulary, actor chains, audience/resource targeting, and attenuation through scope/lifetime. Trust, token security, revocation propagation, and downstream semantics are out of scope; an unauthenticated exchange endpoint can transform a stolen token.
   - Source: IETF/RFC Editor, RFC 8693 — https://www.rfc-editor.org/rfc/rfc8693.html
   - Status: Proposed Standard, 2020-01
   - Confidence/class: high; foundational primitive/design limit

3. DPoP sender-constrains OAuth tokens but does not authorize an exact action: it signs method/URI rather than payload/general headers. Same-endpoint replay requires short windows, `jti` tracking or nonces; accepting nonce-free proof after requiring a nonce is downgrade. Code in the client context can still generate valid proofs.
   - Source: IETF/RFC Editor, RFC 9449 — https://www.rfc-editor.org/rfc/rfc9449.html
   - Status: Proposed Standard, 2023-09
   - Confidence/class: high; foundational primitive/intentional limits

4. HTTP Message Signatures can bind action requests more strongly, but an AgentBridge profile must mandate covered components, payload digest, operation identity, nonce, freshness, algorithms/keys, and failure behavior. Insufficient coverage, replay, proxy-altered context, or ignored verification can attach a valid signature to changed meaning.
   - Source: IETF/RFC Editor, RFC 9421 — https://www.rfc-editor.org/rfc/rfc9421.html
   - Status: Proposed Standard, 2024-02
   - Confidence/class: high; foundational primitive/profile obligation

5. JCS gives deterministic JSON signing, not semantics. It requires I-JSON, rejects duplicate properties/invalid Unicode, restricts numbers to IEEE-754 precision unless strings are used, and does not normalize Unicode. Schema, numeric, Unicode, null/absence and semantic-equivalence rules remain AgentBridge responsibilities.
   - Source: RFC Editor, RFC 8785 — https://www.rfc-editor.org/rfc/rfc8785.html
   - Status: Informational, 2020-06
   - Confidence/class: high; encoding primitive/design constraint

6. AuthZEN 1.0 is a reusable PDP/PEP decision API with a strong denial rule, not an action protocol. PDP state, API authentication and obligation semantics remain outside or implementation-defined; it assumes faithful PEP enforcement. Critical obligations must be typed, negotiated, and mandatory to understand in AgentBridge.
   - Source: OpenID Foundation AuthZEN, Authorization API 1.0 — https://openid.net/specs/authorization-api-1_0.html
   - Status: Final Specification, 2026-01-12
   - Confidence/class: high; authorization decision primitive/trust limit

7. GNAP is the strongest contrary candidate: clean-slate grants, explicit pending/approved/finalized states, interactive consent, key-bound continuation, signing, rotation/revocation, and idempotent management guidance. Resource-server authorization, internal state and downstream effects are outside core; stateless-token revocation may wait for expiry, network loss can desynchronize client/server, and revocation does not undo completed effects.
   - Source: IETF/RFC Editor, RFC 9635 — https://www.rfc-editor.org/rfc/rfc9635.html
   - Status: Proposed Standard, 2024-10
   - Confidence/class: high; strong upstream alternative/boundary

8. HTTP idempotency concerns intended server effects of repeating a method; clients must not retry non-idempotent requests without knowing they are safe or unapplied. GNAP separately documents lost-response inconsistency and stable-result retries. Multi-service agent actions need durable operation IDs and outcome replay, not merely HTTP methods/correlation IDs.
   - Sources: IETF/RFC Editor, RFC 9110 — https://www.rfc-editor.org/rfc/rfc9110.html; RFC 9635 — https://www.rfc-editor.org/rfc/rfc9635.html
   - Status: Internet Standard 2022-06; Proposed Standard 2024-10
   - Confidence/class: high; processing-model requirement

9. Macaroons demonstrate decentralized monotonic delegation through contextual caveats, but remain bearer credentials with application-defined caveat meaning. They do not supply action semantics, revocation latency, execution state, or proof-of-effect.
   - Source: Google Research / NDSS, “Macaroons” — https://research.google/pubs/macaroons-cookies-with-contextual-caveats-for-decentralized-authorization-in-the-cloud/
   - Status: original paper, 2014
   - Confidence/class: high; capability-security primitive/semantic limit

10. SCITT supplies signed statements, COSE receipts, append-only transparency, inclusion/consistency proofs, and verifier-selected trust. A receipt proves registration of an assertion under a policy, not truth of an external action. AgentBridge must define effect claims, evidence types, verifier inputs, trust roots, freshness and proof chains.
    - Source: IETF, RFC 9943 — https://datatracker.ietf.org/doc/rfc9943/
    - Status: Proposed Standard, 2026-06
    - Confidence/class: high for SCITT; medium-high for action-domain inference

11. A formal OAuth/OIDC analysis found four exploitable attacks, including an IdP mix-up from a logical protocol flaw; fixes and explicit assumptions were required before proving security properties. Prose review and primitive composition alone are insufficient for AgentBridge.
    - Source: Fett, Küsters, Schmitz / ACM CCS, “A Comprehensive Formal Security Analysis of OAuth 2.0” — https://publ.sec.uni-stuttgart.de/fettkuestersschmitz-ccs-2016.pdf
    - Status: original paper, 2016-10
    - Confidence/class: high; protocol-design vulnerability/formal verification

12. OpenID FAPI 2.0 combines formally analyzed profiles with executable client and authorization-server conformance tests. Adversarial/negative tests should be a protocol deliverable, not an afterthought.
    - Source: OpenID Foundation, FAPI 2.0 conformance announcement — https://openid.net/fapi2-0-final-conformance-tests-available/
    - Status: 2025-07-09
    - Confidence/class: high; verification/conformance practice

## Safe reuse boundary

Reusable: RAR-style typed authorization; Token Exchange subject/actor distinctions; DPoP for OAuth token binding; HTTP Message Signatures; JCS or COSE; AuthZEN as external PDP integration; GNAP as bridge/reference; macaroon-style attenuation; SCITT/COSE receipt containers; formal OAuth attacker models and FAPI-style harnesses.

AgentBridge must own: action schemas/equality/subsumption; approval-to-action binding; principal/actor/delegate/executor/resource/issuer relations; monotonic delegation; legal lifecycle states; idempotency/deduplication/outcome retrieval; cancellation/compensation/revocation/partial effects; receipt/evidence vocabulary and verifier contract; version/extension/algorithm downgrade resistance; bridge equivalence and assurance-loss handling.

## Candidate non-negotiable invariants

- No effect before applicable authorization/approval reaches explicit approved state.
- Approval binds exact action type, canonical parameters, actor chain, resource/audience, constraints, schema/version, expiry and operation ID; material mutation requires reapproval.
- Delegation only preserves or reduces authority.
- Subject, actor, client, executor, issuer and service remain distinguishable.
- Unknown critical fields, obligations, action types, versions, signature components, algorithms or mappings fail closed.
- No silent fallback to bearer, unsigned, nonce-free or weaker-version behavior.
- Effecting operations have collision-resistant IDs; identical repeats return durable outcomes, changed-body reuse is rejected.
- Ambiguous transport failure yields unknown/pending, never inferred success or safe retry.
- Cancellation, partial effect, compensation and compensation failure are distinct; revocation affects future authorization and cannot erase prior effects.
- Terminal records are immutable; compensation/disputes are linked events.
- Signed receipts prove assertions, not external truth; verifier policy names accepted evidence producers and proof types.
- Bridges never upgrade assurance and reject unrepresentable semantics.
- Authorization endpoints and executor endpoints are issuer/audience bound.
- The normative state machine is model-checked under replay, reorder, duplication, crash/restart, partition, stale policy, concurrent cancellation and compromised intermediaries.
- Negative conformance covers duplicate JSON members, numeric/Unicode transformations, missing signed fields, stale/nonce misuse, issuer/audience swaps, actor-chain truncation, unknown critical extensions, downgrade, operation-ID body mutation and late cancellation.

## Strongest contrary evidence

- GNAP plus AuthZEN, RAR and SCITT could reduce new surface area substantially. Each component nevertheless hands action meaning or downstream effects to the application, so a profile would still need the AgentBridge state machine and verifier contract.
- Formal OAuth results show that carefully fixed/profiled reuse can achieve strong properties. This argues for narrowly audited reuse, not wholesale rejection of existing primitives.

## Searches with no useful evidence

- No complete end-to-end standard was found covering authorization, execution, cancellation, partial effects and verifiable outcomes.
- No evidence supports token/grant revocation as rollback.
- No evidence supports unconditional exactly-once execution across unreliable multi-service boundaries.
- No isolated CVE was used; the strongest evidence was a formally demonstrated protocol-logic flaw plus explicit standards limitations.
