# Security/authorization/evidence stack — round 1 digest

Accessed: 2026-09-13. Research firewall observed; conclusions use official primary sources.

## Findings

1. **RFC 9396 RAR is a structured authority carrier, not an authority algebra.** It defines `authorization_details` and rejection of unknown/invalid types, but resource/action vocabularies and generic “more/less authority” comparison remain application-defined. Source: [RFC 9396](https://www.rfc-editor.org/rfc/rfc9396.html). Publisher: IETF/RFC Editor. Published: 2023-05. Confidence: high. Class: normative scope/limit.

2. **RFC 8693 Token Exchange transports delegation context but leaves token semantics/trust/policy outside scope.** `act`/`may_act` can represent actor relationships; success does not itself prove attenuation or authorized delegation. Source: [RFC 8693](https://www.rfc-editor.org/info/rfc8693/). Publisher: IETF/RFC Editor. Published: 2020-01. Confidence: high on scope, medium on absence of update. Class: delegation/composition risk.

3. **RFC 9449 DPoP provides sender-constrained OAuth tokens but not approval or full compromise protection.** Fail-closed use needs rejection of Bearer downgrade and verification of binding, request fields, freshness/replay and, for high-risk operations, server nonce. Source: [RFC 9449](https://www.rfc-editor.org/info/rfc9449/). Publisher: IETF/RFC Editor. Published: 2023-09. Confidence: high. Class: sender constraint/downgrade.

4. **GNAP RFC 9635 is the strongest greenfield authorization alternative, with RFC 9767 needed for resource-server relations.** It supports negotiated delegated authorization, interactive approval, key-bound continuations and token management, but rights remain application-defined and GNAP is explicitly not OAuth-compatible. Source: [RFC 9635](https://www.rfc-editor.org/rfc/rfc9635.html), [RFC 9767](https://www.rfc-editor.org/rfc/rfc9767.pdf). Publisher: IETF/RFC Editor. Published: 2024-10/2025-04. Confidence: high. Class: alternative/dependency.

5. **OpenID AuthZEN Authorization API 1.0 Final (2026-01-11) is the strongest standard PDP↔PEP seam, but critical context needs an external profile.** Deny must not proceed; however context semantics are outside the spec and unknown JSON fields are ignored, creating a critical-extension hazard. Source: [AuthZEN Authorization API 1.0 Final](https://openid.net/specs/authorization-api-1_0-final.html). Publisher: OpenID Foundation. Confidence: high. Class: policy interface/scope gap.

6. **OpenID SSF/CAEP/RISC 1.0 Final provides change/risk signals, not authority.** It can shorten stale-authority windows but absence or presence of a signal cannot create permission. Source: [OpenID Shared Signals final announcement](https://openid.net/three-shared-signals-final-specifications-approved/). Publisher: OpenID Foundation. Published: 2025-09-02. Confidence: high. Class: revocation signal.

7. **RFC 9943 SCITT plus RFC 9942 COSE Receipts is the strongest reviewed transparency/provenance layer.** It can prove attributed statement registration and auditable history, but not claim truth, authorization, payload meaning or relying-party acceptance. Source: [RFC 9943](https://www.rfc-editor.org/info/rfc9943/). Publisher: IETF/RFC Editor. Published: 2026-06. Confidence: high. Class: evidence/provenance.

## Decision signal

Strongest OAuth-based profile stack: **OAuth 2.0 + RAR + Token Exchange + DPoP + AuthZEN + SSF/CAEP + SCITT/COSE Receipts**. Strongest alternative authorization plane: **GNAP 9635 + RFC 9767**, not mixed casually with OAuth. Neither supplies a shared action/resource/authority vocabulary, attenuation algebra, exact consent receipt or application effect semantics; a public profile remains mandatory.

## Composition hazards

- RAR rejects unknown typed fields while AuthZEN ignores unknown JSON: critical semantics can disappear without a strict profile.
- DPoP can degrade to Bearer unless every boundary rejects downgrade.
- Token Exchange actor chain records actors but does not prove authorized attenuation.
- SCITT receipt proves logging/provenance, not truth or permission.
- GNAP↔OAuth translation is unsafe without one canonical public authority model.

## Gaps/leads

- Verify official conformance suites/implementation release matrices before ecosystem-readiness scoring.
- Check W3C VC 2.0 only as a claims carrier, not presumed authorization.
- AuthZEN obligations/async approval work was not established as Final.
- No reviewed standard supplies a durable portable human consent receipt bound to exact agent authority.
