# Red-team digest: the “thin profile” may be a hidden protocol core

**Question tested:** Is the best next step only a thin interoperability/conformance profile over existing agent, API, and authorization standards, rather than a new protocol core?

**Red-team verdict:** That conclusion is too strong. A profile remains genuinely thin only while it selects or tightens semantics already supplied by its base standards. If interoperability requires new mandatory-to-understand action, authorization, lifecycle, failure, or signed-receipt semantics, the official standards below classify or treat that work as protocol-defining. Calling the result a profile does not keep it thin. The evidence does **not** prove that an entirely new transport or wire protocol is required; it does support a narrow new semantic protocol layer, potentially carried over A2A/HTTP/OAuth.

## Decision test

The strongest standards-grounded boundary is:

1. **Thin profile:** narrows existing choices, raises an existing MAY/SHOULD to MUST, or adds optional semantics that an unaware endpoint can safely ignore.
2. **Major extension / semantic core:** changes processing semantics, introduces new message types or state transitions, requires base implementations to change, or makes unaware processing unsafe.
3. **Hidden core:** labels the second category “a profile” while also requiring universal conformance, mandatory negotiation, canonical signed objects, authorization-to-execution binding, and normative failure behavior.

On that test, the proposed action/authorization/execution/receipt layer becomes a semantic protocol core if any of those objects are required for a conforming interaction.

## Counterclaims

### 1. A profile cannot safely change what an unaware implementation does

**Counterclaim.** If the new objects determine whether an action is authorized, executable, complete, failed, or receipted, they are not merely profile metadata. An implementation without profile knowledge would process the same representation with materially different safety or lifecycle semantics. That exceeds the ordinary profile boundary.

**Primary evidence.** RFC 6906 defines profiles as additional semantics that do not alter basic media-type semantics. It says a profile **MUST NOT** change the semantics of a representation when processed without profile knowledge, so clients with and without profile knowledge can safely use it. It contrasts profiles with new media types that define a complete processing model.

**Inference.** A receipt that gates settlement/audit, an authorization object that gates execution, or a state that changes whether a retry is safe cannot be ignored without changing the meaning of the interaction. Therefore those objects either need to be part of an already-understood base protocol or constitute a new mandatory semantic layer. This does not necessarily require a new media type, but it defeats the characterization “thin profile.”

- **Source:** [RFC 6906 — The `profile` Link Relation Type](https://www.rfc-editor.org/rfc/rfc6906.html), especially §§1, 3, and 3.1
- **Publisher:** IETF / RFC Editor (Independent Stream)
- **Date:** March 2013
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Overturns** the thin-profile conclusion if profile-unaware processing would authorize, execute, retry, settle, or audit differently; otherwise only weakens it.

### 2. Standards guidance calls semantic changes and new message types “major extensions”

**Counterclaim.** A bundle of new semantic objects and behavior is not made routine merely by carrying it in extension fields. If existing implementations must understand it, or if it changes protocol semantics or adds message types, IETF architectural guidance treats it as an update to the underlying protocol and a major extension risk.

**Primary evidence.** RFC 6709 says an extension document should be considered to update the underlying protocol when an implementation must be updated to accommodate it. Its examples include changing protocol semantics and defining new message types. The RFC also warns that incompatible extensions and poorly designed profiles create protocol variations, that extensions must interoperate with the unextended protocol and with other extensions, and that testing is required to establish correct extension behavior.

**Inference.** A cross-agent action envelope, authorization decision, execution state, and receipt are message types plus a processing model. If every conforming endpoint must add handlers, validation, transitions, and error behavior, this satisfies RFC 6709’s major-extension test even if the data rides in A2A metadata or an HTTP body. A conformance profile can describe that layer, but the layer is functionally a protocol core.

- **Source:** [RFC 6709 — Design Considerations for Protocol Extensions](https://www.rfc-editor.org/rfc/rfc6709.html), especially §§2.1, 3.4, 3.4.1, 3.5, and 3.6
- **Publisher:** Internet Architecture Board / RFC Editor
- **Date:** September 2012
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Overturns** “not a new protocol core” when the proposed layer changes semantics, adds mandatory objects, or requires implementation changes. It merely **weakens** the claim if the profile only tightens already-defined choices.

### 3. Agent interoperability standards put shared objects, state, operations, and errors in the core—not in optional metadata

**Counterclaim.** The best current primary-source analogy cuts against a thin-profile framing. A2A obtains cross-binding interoperability by defining a canonical data model, abstract operations, task lifecycle, event ordering, version negotiation, and canonical error mappings as normative protocol content.

**Primary evidence.** A2A v1.0 says its canonical data model contains core structures all implementations must understand; its abstract operations define fundamental behavior; and interoperability comes from shared understanding of the canonical model. It defines `Task` as stateful, makes terminal states control whether messages are accepted and when streams close, requires ordered events, and maps A2A-specific errors consistently across JSON-RPC, gRPC, and HTTP. Its Protocol Buffer file is the authoritative normative definition of protocol objects and request/response messages.

**Inference.** If AgentBridge requires different or additional action states, result/receipt objects, acceptance criteria, retry/idempotency rules, or failure classes, those additions occupy the same semantic layer A2A itself calls “core.” Reusing A2A transports or envelope fields would not remove that core; it would create a higher-layer protocol over A2A.

- **Source:** [Agent2Agent Protocol Specification v1.0.0](https://a2a-protocol.org/v1.0.0/specification), especially §§1.3–1.4, 3.1–3.6, 4.1–4.2, and 5.1–5.4; release date corroborated by the [official v1.0.0 release](https://github.com/a2aproject/A2A/releases/tag/v1.0.0)
- **Publisher:** A2A Project
- **Date:** 2026-03-12
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Strongly weakens** the conclusion and **overturns** it if required AgentBridge state/objects cannot be losslessly mapped to A2A’s normative model.

### 4. Optional extensions cannot guarantee a mandatory interoperability property

**Counterclaim.** If authorization binding or receipts are optional extensions, unsupported peers are allowed to proceed without them or must fail when they are marked required. Either outcome contradicts universal, transparent interoperability for the protected behavior.

**Primary evidence.** A2A v1.0 requires clients to opt into extensions. Unsupported optional extension versions should be ignored for that interaction; a required unsupported extension must produce `ExtensionSupportRequiredError`; and implementations must not silently fall back to an older extension version. Required capabilities are declared and validated through the Agent Card and request parameters.

**Inference.** Making the safety/receipt layer optional means some “conforming” interactions lack the promised property. Marking it required creates a mandatory-to-understand subprotocol with discovery, version negotiation, failure semantics, and a separate conformance surface. The latter can be deployed as an extension, but operationally it is a protocol profile/core boundary, not a thin annotation.

- **Source:** [Agent2Agent Protocol Specification v1.0.0](https://a2a-protocol.org/v1.0.0/specification), especially §§3.3.2–3.3.4 and 4.6.1–4.6.3
- **Publisher:** A2A Project
- **Date:** 2026-03-12
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Overturns** the claim that the required behavior can remain an *optional* interoperable extension. It does not overturn the feasibility of a deliberately negotiated mandatory extension.

### 5. Existing authorization standards do not define the authorization-to-execution semantic bridge

**Counterclaim.** OAuth RAR can carry structured authorization details, but it deliberately does not supply generic semantics for comparing them, and A2A deliberately does not define the scope, representation, validity, or revocation semantics of in-task authorization. The missing bridge—identifying the exact operation and proving that the execution matches the authorization—must be standardized somewhere new.

**Primary evidence A.** RFC 9396 requires an authorization server to reject unknown authorization-detail types or unknown fields. It says the fields’ semantics are implementation-specific to an API or set of APIs and provides no standardized mechanism for comparing arbitrary authorization-detail requests. It also does not define an extension to the authorization response.

- **Source:** [RFC 9396 — OAuth 2.0 Rich Authorization Requests](https://www.rfc-editor.org/rfc/rfc9396.html), especially §§4–6.1
- **Publisher:** IETF / RFC Editor
- **Date:** May 2023
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Overturns** any claim that OAuth RAR already supplies interoperable action semantics; **weakens** only the broader claim that a new transport protocol is needed.

**Primary evidence B.** A2A’s `TASK_STATE_AUTH_REQUIRED` explicitly does not define the resulting authorization’s scope, representation, validity, or revocation. It says the state alone authorizes nothing; the operation identity and authorization check must be defined by the implementation, credential issuer, or extension; and authorization cannot be assumed to cover later task messages.

- **Source:** [Agent2Agent Protocol Specification v1.0.0](https://a2a-protocol.org/v1.0.0/specification), §7.6.4
- **Publisher:** A2A Project
- **Date:** 2026-03-12
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Overturns** the assertion that the existing agent-protocol state model already closes authorization/execution coupling. A new normative semantic definition is required, whether named a protocol, extension, or profile.

### 6. Verifiable receipts force normative canonicalization, binding, key, and error rules

**Counterclaim.** A cryptographic receipt is not interoperable merely because implementations use JSON and HTTP signatures. The application must define exactly what is signed, how it is normalized, how request and response are bound, which keys and algorithms are valid, and how verification failures surface. That is a substantial security protocol surface.

**Primary evidence A.** RFC 9421 says an application or profile using HTTP Message Signatures **MUST** define, at minimum, required covered components and parameters, field types, key retrieval, allowed algorithms, key/algorithm suitability, signature context, request-response binding elements, and verification error codes. It states that HTTP Message Signatures are only part of a complete security system. It also warns that application-specific canonicalization harms interoperability outside that application.

- **Source:** [RFC 9421 — HTTP Message Signatures](https://www.rfc-editor.org/rfc/rfc9421.html), especially §§1.4, 2.5, 7.2.8, 7.4.3, and 7.5.4
- **Publisher:** IETF / RFC Editor
- **Date:** February 2024
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Overturns** “thin” if receipts are a required security guarantee. It does **not** prove a new base protocol is mandatory, because RFC 9421 expressly allows an application profile to define these rules.

**Primary evidence B.** RFC 8785 says hashing and signing require invariant data representation and defines JCS by constraining JSON to I-JSON, using strict primitive serialization, and deterministic property sorting.

- **Source:** [RFC 8785 — JSON Canonicalization Scheme](https://www.rfc-editor.org/rfc/rfc8785.html), especially §§1 and 3
- **Publisher:** RFC Editor (Independent Stream)
- **Date:** June 2020
- **Accessed:** 2026-09-11
- **Confidence:** High
- **Impact:** **Strongly weakens** any claim that an informal JSON receipt shape is sufficient; **overturns** it if independent implementations must reproduce the same signed bytes and no canonicalization rule is normative.

## Cross-governance consequence

**Evidence.** RFC 6709 cites the IAB principle that only one standards-development organization should maintain design authority for a protocol, including parameter allocation and the semantics/actions attached to code points. It also requires an extension to interoperate with other extensions and calls for registration and review of shared parameters.

**Inference.** IETF owns OAuth and HTTP signature semantics; the A2A Project owns A2A objects, states, extensions, and errors. A profile spanning both cannot quietly redefine either. It needs its own clearly bounded change authority for the composition layer, registries/URI policy, versioning, conformance tests, and conflict rules. That governance does not prove a new transport, but it is another hallmark of a real protocol layer.

## What this red-team effort could not establish

- No primary source found says that these exact AgentBridge requirements *inevitably* require a wholly new wire protocol, transport, or media type.
- No primary source found says a multi-standard conformance profile cannot be interoperable. In fact, RFC 6709 says profiles can improve interoperability by elevating existing MAY/SHOULD requirements to MUST.
- RFC 9421 expressly permits an application **or profile** to define the missing signature rules. This supports “profile” as a publication form, while undermining only the adjective “thin.”
- A2A provides an explicit negotiated-extension path, including required extensions. This can yield interoperability among mutually supporting peers, though not transparent interoperability with profile-unaware peers.
- No complete primary standard was found that already defines one portable object model tying together: intended action, exact authorization scope, execution identity/state, result, cryptographic receipt, retry/idempotency behavior, and revocation/appeal semantics across independent agents.
- No official source was found establishing that a generic HTTP/API conformance suite can validate semantic equivalence of arbitrary agent actions without a shared action ontology or domain-specific profiles.

## Bottom line

The defensible replacement conclusion is: **reuse existing transports, envelopes, discovery, OAuth, and signature primitives, but expect a new, narrowly scoped semantic protocol core unless every required AgentBridge object and transition can be mapped losslessly to one existing protocol’s normative model.** A conformance profile may be the document packaging, but if it defines mandatory objects, lifecycle, authorization binding, signed receipt semantics, negotiation, errors, and tests, it is no longer thin in the architectural sense.
