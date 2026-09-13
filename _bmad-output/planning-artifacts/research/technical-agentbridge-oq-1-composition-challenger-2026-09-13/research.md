---
title: 'Technical research: AgentBridge OQ-1 Composition Challenger Selection'
type: technical
topic: 'AgentBridge OQ-1 Composition Challenger Selection'
decision: 'Select Pareto-nondominated Composition Challengers and Primary C'
source: native-web-run
status: complete
preset: standard
validation: high
created: '2026-09-13'
updated: '2026-09-13'
verified_claims: 17
unverified_claims: 2
disputed_claims: 0
overturned_claims: 1
---

# Technical research: AgentBridge OQ-1 Composition Challenger Selection

## Executive recommendation

Select two existing-standard compositions for Gate 1. They are comparison controls, not the AgentBridge architecture and not a decision against a Native Core.

### C1 — Primary Composition Challenger

Use the most deployable public composition found: A2A for inter-agent tasks, MCP for tools/context, the OAuth/AuthZEN family for authority, UCP for commerce and AP2 only for consequential commerce. OpenAPI/Arazzo remain descriptions rather than runtime enforcement; evidence semantics are mandatory but SCITT/COSE is only an optional carrier. Exact constraints appear in the C1 manifest below.

C1 is the strongest deployability baseline found, but it is not a clean, unified protocol. It needs public, versioned mappings for identity, authority, task/effect state, consent, failure behavior and evidence. A current AuthZEN MCP draft cannot serve as that mapping unchanged because it targets removed MCP methods and omits required MCP 2026-07-28 methods.[2][28]

### C2 — GNAP Authority Challenger

Keep the same A2A/MCP/UCP/AP2 application layers, but replace the OAuth grant/token family with GNAP RFC 9635 plus the resource-server protocol in RFC 9767. Use one authorization plane, not both.[16][17]

C2 is non-dominated because GNAP offers a more coherent stateful grant negotiation model. It **conditionally passes** OQ-1's independent-implementability gate because its RFCs are sufficient to build from public material, but it does not pass the stronger “deploy today from a complete existing implementation” test: the public implementation examined omits several important RFC features.[18] No complete conformance program was found in this bounded search; that absence remains unverified rather than being inferred from the implementation repository.

### Reserve and bounded variants

Keep ANP 1.1 as a reserve, and require one bounded AGNTCY infrastructure test under C1 without making AGNTCY a runtime dependency.[19][20][21] ACP, x402 and Visa TAP remain scoped adapters or edge options rather than additional cores.[13][14][15]

### Decision meaning

The evidence does **not** support “existing standards already solve AgentBridge.” It supports a narrower statement: C1 is the strongest practical composition to challenge a Native Core; C2 prevents the experiment from unfairly assuming OAuth is the only viable authority foundation. Gate 1—not this desk research—must decide whether a Native Core delivers enough additional safety, semantic unity, efficiency and operability to justify a new core.

## Method and decision boundary

The research followed the accepted OQ-1 selection brief: public normative specifications, official repositories, release notes and standards-body material only. It screened candidates against these hard gates:

1. exact public versions and normative material;
2. independent implementation without private help;
3. no hidden proprietary runtime or pair-specific semantics;
4. disclosed glue, state, trust/data boundaries and dependencies;
5. legally usable artifacts for the Gate 1 experiment;
6. missing behavior counted as a gap, not removed from common scope.

Surviving candidates were compared using the accepted weights: invariant coverage 35%, authority/safety/effects 25%, topology/lifecycle 15%, deployability/glue 15%, and governance/evolution 10%. Research findings remain hypotheses until executable Gate 1 evidence.

### Terminology used below

- **Authority cell:** the smallest explicit permission unit, binding who may do what, to which resource, under which limits, validity period and provenance.
- **Decision-subject digest:** a canonical fingerprint of the exact facts on which authorization or approval was based, used to prevent applying it to a changed action.
- **False-zero resistance:** protection against treating missing, delayed or ambiguous outcome evidence as proof that no real-world effect occurred.
- **Dependency closure:** the complete versioned set of runtimes, trust services, schemas, adapters and operational dependencies required to reproduce a candidate.

## Candidate-neutral invariant coverage

This is a research-level coverage hypothesis, not an oracle verdict. The frozen EI-01–EI-25 remain authoritative.

| Invariant group | C1 hypothesis | C2 hypothesis | Material gap to test |
| --- | --- | --- | --- |
| Actors, roles, negotiation, lifecycle (EI-01–EI-05) | partial–strong | partial–strong | cross-A2A/MCP/UCP identity, correlation and state ownership |
| Authority, approval, commit atomicity, shared limits (EI-06–EI-09) | partial | partial–strong | portable authority algebra; atomic permit/effect; partition-safe shared budgets |
| Effect truth, retry, cancel/compensation, evidence (EI-10–EI-13) | partial | partial | one outcome model; false-zero resistance; dispute/evidence lifecycle |
| Semantics, extension, composition and mappings (EI-14–EI-18) | partial | partial | versioned loss-aware mappings and multi-party dependency closure |
| Privacy, bounds and dependency independence (EI-19–EI-21) | partial | partial | end-to-end metadata/privacy budgets and bounded failure behavior |
| Independent conformance and evolution (EI-22–EI-23) | partial–strong | partial | one cross-stack TCK, reproducible BOM and coordinated drift rules |
| Cross-provider comparison and threat/claim boundary (EI-24–EI-25) | partial | partial | shared domain semantics and complete composite threat model |

No candidate receives credit merely because a component has a similarly named field. Gate 1 must demonstrate preserved meaning across every boundary.

## Weighted comparison

Scores are bounded ranges on 0–5 evidence, not precise forecasts.

| Configuration | Invariant coverage 35% | Authority/safety/effects 25% | Topology/lifecycle 15% | Deployability/glue 15% | Governance/evolution 10% | Weighted range |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| C1 Primary | 3.7–4.2 | 3.5–4.1 | 3.7–4.2 | 2.8–3.4 | 4.1–4.6 | **3.6–4.1** |
| C2 GNAP | 3.8–4.3 | 3.8–4.4 | 3.9–4.4 | 1.8–2.6 | 3.7–4.2 | **3.5–4.1** |
| ANP reserve | 3.8–4.3 | 3.1–3.8 | 4.2–4.7 | 1.7–2.5 | 2.3–3.1 | **3.2–3.8** |

The ranges overlap. C1 is Primary because its worst-case deployability is materially stronger, not because a fragile midpoint says it is universally superior. C2 remains necessary to test whether C1’s OAuth composition complexity is avoidable.

## Landscape findings

### General interaction and workflow

A2A v1.0 became a stable Linux Foundation-hosted protocol in March 2026 and provides agent discovery metadata, messages, tasks, streaming, cancellation and multiple bindings.[1][32] Its specification relies on external authentication and authorization mechanisms rather than defining a universal identity-issuance or authorization-policy plane.[32]

MCP 2026-07-28 is a strong, current tool/context boundary with updated official SDKs and an official conformance project.[2][33][34] It is governed as an LF Projects series.[3] It is not a network-wide identity, routing, authorization or commerce protocol. Endpoint-local `server/discover` must not be mistaken for global agent/service discovery.[2]

OpenAPI 3.2.1 describes callable HTTP surfaces, while Arazzo 1.1.0 describes multi-operation workflows. Neither is the runtime that executes, authorizes, coordinates or proves effects.[4][5]

The historical BeeAI Agent Communication Protocol has merged into A2A and is not an independently advancing challenger.[22] Current W3C WebAgents work and the reviewed IETF agent communication framework did not provide a deployable replacement: one produces no specifications and the other is an individual architectural draft.[23][24]

### Commerce and payment intent

Within the reviewed source set, UCP v2026-08-25 was assessed as the broadest current public commerce substrate. Its documented capabilities span discovery, catalog/location, cart/checkout, identity linking, orders and payment handlers.[6][38] Its latest specification is newer than the public conformance/sample baseline, so the Gate 1 harness must adapt or obtain upstream tests rather than assuming compatibility.[7]

AP2 v0.2.0 adds signed mandates and a trusted surface for consequential checkout/payment activity.[8][35] The specification places catalog, checkout update transport, general task lifecycle and several delegation/dispute concerns outside its own scope.[35] UCP’s AP2 binding is explicit and asymmetric: UCP supplies commerce semantics; AP2 supplies an optional trust extension.[9]

ACP overlaps UCP and remains beta in the reviewed official repository, so it should not create a second canonical state model.[13] x402 adds useful payment gating for APIs, data and compute.[14] Treating cart, fulfillment, refunds, booking and portable consent as external is a scope inference from what its v2 specification defines, not an explicit normative exclusion list.[14]

No released complete lodging/booking profile was verified. Therefore OQ-1 must not claim that the selected composition already covers universal travel or booking semantics.

### Authority, approval, revocation and evidence

OAuth RAR provides structured authorization details, Token Exchange carries delegation/actor context, and DPoP binds tokens to a presenter. None alone supplies a universal authority vocabulary, attenuation proof or consent model.[25][26][27]

AuthZEN Authorization API 1.0 Final is a useful standard PDP/PEP seam, but application context meaning remains external.[10] New drafts for approval and token exchange reduce local glue, while remaining draft-level.[11][37] Most importantly, COAZ-MCP Draft 1 is incompatible unchanged with MCP 2026-07-28: it maps removed legacy methods, omits `server/discover` and `subscriptions/listen`, and rejects unknown methods. It is design evidence, not deployable glue for C1.[28]

SSF/CAEP 1.0 reached Final status.[29] CAEP communicates security-event changes that can attenuate access and reduce stale-authority windows.[36] The rule that such a signal never creates permission is an explicit safety inference for the challenger, not a claim made by the status announcement. SCITT and COSE Receipts provide signed-statement transparency/provenance, not truth, permission, exact action effects or disputes.[30][31]

GNAP supplies a coherent alternative authorization protocol and resource-server relationship, but is intentionally not OAuth-compatible.[16][17] Its rights vocabulary and AgentBridge-relevant mappings still require a public profile.

## Candidate manifests for comparison

### C1 mandatory configuration

| Concern | Owner | Required rule or unresolved gap |
| --- | --- | --- |
| Agent task lifecycle | A2A 1.0.x | One owner for state/cancel/streaming; no parallel MCP Task truth |
| Tool/context calls | MCP 2026-07-28 | Strict method/version mapping; fail closed on critical unknowns |
| HTTP/workflow descriptions | OpenAPI 3.2.1 + Arazzo 1.1.0 | Descriptive only; an executor/state machine is mandatory glue |
| Grant/token security | OAuth RFC set + DPoP | Audience/resource restriction; no Bearer downgrade |
| Policy decision | AuthZEN 1.0 Final | Public context/action/resource vocabulary required |
| Change signals | SSF/CAEP 1.0 | Never treated as a grant; stale/partition behavior specified |
| Commerce | UCP v2026-08-25 REST | Canonical state; adapters cannot create a second truth |
| Consequential commerce | AP2 v0.2.0 | Scope limited; trust list, revocation and disputes disclosed |
| Evidence | protocol-neutral action-evidence profile | A receipt cannot be interpreted as effect truth by itself; SCITT/COSE is an optional carrier |

Mandatory composition glue includes a canonical actor/principal model, authority cell and attenuation rules, exact decision-subject digest, approval lifecycle, task-to-effect mapping, idempotency and concurrency state, revocation behavior, provenance/loss annotations for every translation, evidence profile, failure/recovery table and complete dependency inventory.

The AGNTCY augmentation is a required bounded **comparison experiment**, not a mandatory C1 runtime component. OQ-3B must pin the tested subset and its dependency closure separately.

### C2 mandatory configuration

C2 inherits the application-layer rows above but replaces OAuth issuance, RAR, Token Exchange and DPoP with RFC 9635 and RFC 9767. It must define a GNAP-to-AuthZEN mapping, an AgentBridge-relevant rights vocabulary, key-bound access, continuation/token management, resource-server introspection and failure behavior. Missing implementation features are built and tested as part of the challenger; they are not silently waived.

### Pinning baseline

| Artifact | OQ-1 pin or status |
| --- | --- |
| A2A | 1.0.x; tag 1.0.0 commit `173695755607e884aa9acf8ce4feed90e32727a1`; v1.0.1 commit `3303592588e388e62e0f69f701af531d2f4e3991`; patch compatibility to be proven |
| MCP | 2026-07-28; commit `5f5440bb26a62e2cf3440b92da5a667efa03b267` |
| OpenAPI | 3.2.1 |
| Arazzo | 1.1.0 |
| UCP | v2026-08-25; commit `cd78fb38e819de77d9b527d110476eccb876f1bd` |
| AP2 | v0.2.0; commit `b4587ac1d055888a73b4b21750973cffba961793` |
| OAuth/GNAP/IETF evidence | RFC-numbered immutable publications |
| AuthZEN | Authorization API 1.0 Final; newer bindings explicitly provisional |
| ANP reserve | 1.1 tag commit `6fc3854ca15f453fa607360844b3a98120f74284`; exact released BOM still missing |
| AGNTCY option | DIR v1.6.2 commit `ad2d2125eaed30b57d04838d9db56bd12386924b`; SLIM repository snapshot `16151eefd31195dcc600b359c2d401926e871d24`, whose `slim-version` component is v2.3.2; a component BOM and suite compatibility remain unverified |

## Strongest contrary evidence

1. C1 may appear more mature only because it combines many mature parts; cross-protocol seams may dominate the actual safety and operational cost.
2. GNAP could outperform OAuth composition after missing features are implemented, particularly for interactive, stateful delegated authority.
3. ANP may already supply more coherent network primitives than the component stack, but its released/draft boundary and governance/conformance evidence are currently insufficient.
4. AGNTCY may materially improve secure routing, identity, discovery and observability under C1; excluding it entirely would weaken the challenger unfairly.
5. AuthZEN drafts show that important gaps can be solved by public profiles instead of a new core. Conversely, their incompatibility/version drift demonstrates the maintenance cost of composition.
6. AP2 and SCITT provide useful evidence mechanisms, but interpreting either as proof of real-world effect would create a dangerous false certainty.

These objections do not overturn the two-candidate selection; they define what Gate 1 must measure.

## Conclusions and required next actions

1. Accept C1 as Primary C and C2 as the authority-plane challenger.
2. Keep ANP as a reserve triggered only after an immutable released BOM and mappings pass the same hard gates.
3. Include a bounded AGNTCY infrastructure augmentation/substitution test without crediting it as a semantic protocol.
4. Freeze exact dependency closures and public glue only in OQ-3B, before any candidate implementation.
5. Do not conclude “build Native,” “use composition,” “upstream,” or “stop” until Gate 1 produces reproducible practical evidence.

## Known unknowns and recheck triggers

- Cross-SDK certification for the exact A2A patch and exact MCP SDK/spec-tag equivalence remain unverified.
- The MCP Registry maturity/status is unverified and it cannot become a hidden mandatory service.
- Latest UCP conformance and samples lag the selected release.
- No standalone AP2, RAR/Token Exchange, GNAP or complete composite TCK was verified.
- Exact x402 stable pin and ACP immutable snapshot hash were not verified.
- The three relevant AuthZEN bindings are drafts; COAZ-MCP must be replaced or updated for MCP 2026-07-28.
- No released complete booking/lodging semantic profile was verified.
- No canonical compatible AGNTCY suite BOM or released-only ANP end-to-end profile was verified.

Recheck version and compatibility claims by 2026-10-13, fast-moving landscape claims by 2026-12-13, and broader ecosystem/adoption claims by 2027-03-13. Recheck immediately on any selected upstream security advisory, breaking release, erratum or governance/IP change.

## Source appendix

| Ref | Claim/finding supported | Publisher/source | Published | Accessed | Confidence |
| --- | --- | --- | --- | --- | --- |
| [1] | A2A 1.0 release, scope and governance | [A2A/Linux Foundation](https://a2a-protocol.org/dev/blog/2026/03/12/a2a-protocol-ships-v10-production-ready-standard-for-agent-to-agent-communication/) | 2026-03 | 2026-09-13 | high |
| [2] | MCP 2026-07-28 methods and scope | [MCP project](https://modelcontextprotocol.io/specification/2026-07-28) | 2026-07 | 2026-09-13 | high |
| [3] | MCP governance and licensing | [MCP/LF Projects](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md) | current | 2026-09-13 | high |
| [4] | OpenAPI 3.2.1 version and descriptive scope | [OpenAPI Initiative](https://spec.openapis.org/oas/v3.2.1.html) | 2026-09 | 2026-09-13 | high |
| [5] | Arazzo 1.1.0 version and workflow scope | [OpenAPI Initiative](https://spec.openapis.org/arazzo/v1.1.0.html) | 2026-05 | 2026-09-13 | high |
| [6] | UCP latest release and commerce scope | [UCP project](https://github.com/Universal-Commerce-Protocol/ucp/releases/tag/v2026-08-25) | 2026-08 | 2026-09-13 | high |
| [7] | UCP conformance baseline lag | [UCP project](https://github.com/Universal-Commerce-Protocol/conformance) | current | 2026-09-13 | high |
| [8] | AP2 v0.2.0 release and scope | [AP2 project](https://github.com/google-agentic-commerce/AP2/releases/tag/v0.2.0) | 2026-04 | 2026-09-13 | high |
| [9] | UCP-to-AP2 dependency and binding | [UCP Authors](https://ucp.dev/documentation/ucp-and-ap2/) | current | 2026-09-13 | high |
| [10] | AuthZEN Authorization API 1.0 Final | [OpenID Foundation](https://openid.net/specs/authorization-api-1_0.html) | 2026-01 | 2026-09-13 | high |
| [11] | AuthZEN approval profile status and scope | [OpenID Foundation](https://openid.github.io/authzen/authzen-access-request-approval-profile-1_0.html) | 2026-09 | 2026-09-13 | high |
| [13] | ACP beta status and overlapping scope | [ACP project](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) | current | 2026-09-13 | medium-high |
| [14] | x402 v2 narrow paid-resource scope | [x402 Foundation](https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md) | current | 2026-09-13 | medium |
| [15] | Visa TAP scheme-controlled trust dependency | [Visa](https://developer.visa.com/capabilities/trusted-agent-protocol/trusted-agent-protocol-specifications) | current | 2026-09-13 | medium-high |
| [16] | GNAP core model | [IETF/RFC Editor](https://www.rfc-editor.org/rfc/rfc9635.html) | 2024-10 | 2026-09-13 | high |
| [17] | GNAP resource-server relationship | [IETF/RFC Editor](https://www.rfc-editor.org/rfc/rfc9767.html) | 2025-04 | 2026-09-13 | high |
| [18] | Public GNAP implementation omissions | [SUNET](https://github.com/SUNET/sunet-auth-server) | current | 2026-09-13 | high |
| [19] | AGNTCY Directory scope and deployment | [AGNTCY/Linux Foundation](https://docs.agntcy.org/dir/architecture/) | current | 2026-09-13 | high |
| [20] | AGNTCY SLIM component versions and repository snapshot; no whole-suite version inferred | [AGNTCY/Linux Foundation](https://github.com/agntcy/slim/releases) | current | 2026-09-13 | medium-high |
| [21] | ANP released/draft component status | [ANP project](https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/docs/anp-getting-started-guide.md) | current | 2026-09-13 | medium-high |
| [22] | BeeAI ACP merger into A2A | [BeeAI contributors](https://github.com/i-am-bee/acp) | current | 2026-09-13 | high |
| [23] | W3C WebAgents group produces no specifications | [W3C Community Group](https://www.w3.org/community/webagents/charter/) | current | 2026-09-13 | high |
| [24] | Agent communication framework is an individual draft | [IETF Datatracker](https://datatracker.ietf.org/doc/draft-zahed-agent-comm-framework/) | current | 2026-09-13 | high |
| [25] | RAR structured details and semantic limit | [IETF/RFC Editor](https://www.rfc-editor.org/rfc/rfc9396.html) | 2023-05 | 2026-09-13 | high |
| [26] | Token Exchange actor/delegation scope and limits | [IETF/RFC Editor](https://www.rfc-editor.org/rfc/rfc8693.html) | 2020-01 | 2026-09-13 | high |
| [27] | DPoP sender constraint and limits | [IETF/RFC Editor](https://www.rfc-editor.org/rfc/rfc9449.html) | 2023-09 | 2026-09-13 | high |
| [28] | COAZ-MCP Draft 1 mappings and incompatibility with selected MCP | [OpenID Foundation](https://openid.github.io/authzen/authzen-coaz-mcp-binding-1_0.html) | 2026-02 | 2026-09-13 | high |
| [29] | SSF/CAEP final status and signal scope | [OpenID Foundation](https://openid.net/three-shared-signals-final-specifications-approved/) | 2025-09 | 2026-09-13 | high |
| [30] | SCITT transparency/evidence scope | [IETF/RFC Editor](https://www.rfc-editor.org/rfc/rfc9943.html) | 2026-06 | 2026-09-13 | high |
| [31] | COSE Receipts standard | [IETF/RFC Editor](https://www.rfc-editor.org/rfc/rfc9942.html) | 2026-06 | 2026-09-13 | high |
| [32] | A2A message/task/cancellation/binding scope and external security mechanisms | [A2A project](https://a2a-protocol.org/v1.0.0/specification/) | 2026-03 | 2026-09-13 | high |
| [33] | MCP 2026-07-28 release and updated official SDKs | [MCP/Linux Foundation](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/blog/content/posts/2026-07-28-spec-ga/index.md) | 2026-07 | 2026-09-13 | high |
| [34] | MCP official conformance project activity | [MCP project](https://github.com/modelcontextprotocol/conformance) | current | 2026-09-13 | high |
| [35] | AP2 mandates, trusted surface, scope and exclusions | [AP2 project](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md) | 2026-04 | 2026-09-13 | high |
| [36] | CAEP 1.0 event semantics and access attenuation | [OpenID Foundation](https://openid.net/specs/openid-caep-1_0-final.html) | 2025-08 | 2026-09-13 | high |
| [37] | AuthZEN OAuth Token Exchange Binding Draft 1 status and scope | [OpenID Foundation AuthZEN WG](https://openid.github.io/authzen/authzen-oauth-token-exchange-1_0.html) | 2026-09 | 2026-09-13 | high |
| [38] | UCP documented commerce capabilities | [UCP Authors](https://ucp.dev/documentation/core-concepts/) | current | 2026-09-13 | high |
