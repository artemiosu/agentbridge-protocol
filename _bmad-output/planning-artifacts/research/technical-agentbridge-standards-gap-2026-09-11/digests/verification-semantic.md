# Fresh-context semantic verification

**Scope:** independent verification of the version/compatibility and load-bearing semantic claims in `landscape-r1-1.md` and `mapping-r2-1.md`. Only those two digests were admitted as project evidence. All verification evidence below comes from re-opened primary official sources. Project context was inadmissible. Access date for every source: **2026-09-11**.

**Method:** eight official source surfaces were inspected in ten web calls. Targeted searches looked both for supporting language and for disconfirming primitives or guarantees, especially approval objects, action digests, receipts, idempotency, task mappings, cancellation guarantees, and authorization-chain semantics. A keyword miss is treated only as a bounded negative result, never as proof of ecosystem-wide absence.

**Status totals:** **VERIFIED 13 · DISPUTED 1 · UNVERIFIED 1 · OVERTURNED 0**.

## Claim-by-claim results

### 1. VERIFIED — MCP 2026-07-28 supplies an integration transport/tool layer, not enforceable action approval

**Source:** [MCP Specification, revision route 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28) — Model Context Protocol, Linux Foundation project; revision **2026-07-28**; accessed **2026-09-11**.

**Exact support:** the official page identifies MCP as an open protocol integrating LLM applications with external data sources and tools; defines Host, Client, and Server roles; uses JSON-RPC 2.0; and lists stateless self-contained requests, per-request capability negotiation, Resources, Prompts, Tools, Elicitation, progress, cancellation, and error reporting. It places Tasks under optional opt-in extensions. Its safety section says hosts must obtain explicit user consent before tool invocation, then expressly says MCP itself cannot enforce those principles at protocol level. This supports both the positive transport/tool coverage and the conclusion that MCP consent is not a portable action-bound approval record.

**Mismatch/limit:** the revision date is established by the dated official route and the matching Tasks surface, but the page body does not print a separate “current latest” label. The claim that the retrieved revision is current is therefore verified as the live official revision surface, not through a second independent release index.

### 2. VERIFIED — MCP HTTP authorization binds resource access and scopes, not immutable action parameters

**Source:** [MCP Authorization](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization) — Model Context Protocol, Linux Foundation project; revision **2026-07-28**; accessed **2026-09-11**.

**Exact support:** the chapter says authorization is optional, applies to HTTP transports, and lets clients make restricted-server requests on behalf of resource owners. It requires Protected Resource Metadata discovery, an authorization-server discovery mechanism, a client ID, the OAuth `resource` parameter in authorization and token requests, canonical MCP server identification, bearer presentation on every HTTP request, audience validation, and runtime scope challenges/step-up. Required scopes may be determined dynamically from request arguments.

**Mismatch/limit:** dynamic argument-sensitive scopes are stronger than a purely static resource grant, but the chapter does not standardize an action digest, parameter-level mandate object, approval resolution, single-use consumption, or execution receipt. The digest’s bounded claim is accurate; it must not be broadened to say MCP implementations cannot add such controls through an authorization extension or local policy.

### 3. VERIFIED — MCP Tasks is a Draft async extension with the stated error and cancellation semantics

**Source:** [MCP Tasks Extension](https://tasks.extensions.modelcontextprotocol.io/specification/draft/tasks) — Model Context Protocol Tasks extension project; revision **2026-07-28**, page status **Draft**; accessed **2026-09-11**.

**Exact support:** the page marks itself Draft and says task creation is server-directed: the client advertises support, while the server alone decides per request whether to materialize a task. It defines durable creation, `tasks/get`, `tasks/update`, `tasks/cancel`, polling, notifications, and `working`, `input_required`, `completed`, `failed`, and `cancelled`. A tool result with `isError: true` is explicitly `completed`; `failed` is reserved for JSON-RPC execution errors. Each task-related request requires authentication and authorization checks. Cancellation acknowledgement is eventually consistent and cooperative; the server is not obligated to stop work and a transition to `cancelled` is not guaranteed.

**Mismatch/limit:** no `approval`, `receipt`, or `idempotency` term was found on the reviewed Tasks page. That is a bounded surface finding. The hard lifecycle mismatch is directly normative: `completed` cannot safely map to domain success without inspecting the result.

### 4. VERIFIED — A2A’s rendered/repository version surfaces are out of sync, while wire compatibility is Major.Minor

**Sources:** [A2A Specification](https://a2a-protocol.org/latest/specification/) — A2A Protocol, Linux Foundation project; rendered page says latest released version **1.0.0**, no page publication date stated; accessed **2026-09-11**. [A2A official releases](https://github.com/a2aproject/A2A/releases) — A2A Protocol official GitHub repository; `v1.0.1` marked **Latest**, release heading **2026-05-26** (GitHub UI also displays “released this 28 May”); accessed **2026-09-11**.

**Exact support:** the rendered specification header says “Latest Released Version 1.0.0,” while the official release list marks `v1.0.1` Latest and its release heading is `1.0.1 (2026-05-26)`. The specification says the protocol version is `Major.Minor`, patch versions do not affect compatibility, patch numbers should not be sent, and must not participate in negotiation. Extensions should version their URI and must use a new URI for breaking changes.

**Mismatch/limit:** the release page exposes two dates: the release heading is 2026-05-26 and the GitHub UI publication line is 28 May. The digest’s 2026-05-26 date matches the release heading, but reproducible citations should acknowledge the UI discrepancy. The substantive 1.0.0/1.0.1 publication mismatch and 1.0 wire compatibility are verified.

### 5. VERIFIED — A2A provides the public task plane and a sharply bounded in-task authorization handoff

**Source:** [A2A Specification](https://a2a-protocol.org/latest/specification/) — A2A Protocol, Linux Foundation project; rendered version **1.0.0**, page date not stated; accessed **2026-09-11**.

**Exact support:** the specification defines Agent Cards, skills, required URI-identified extensions, structured Parts, server-generated Tasks, Artifacts, polling, streaming, push, retrieval/list/cancel/subscribe operations, and interrupted `TASK_STATE_INPUT_REQUIRED` and `TASK_STATE_AUTH_REQUIRED` states. Its in-task authorization section gives both API-token and human-approval examples and says A2A can delegate fulfillment of required authorization to the client. Critically, it also says the auth-required transition alone authorizes no operation; A2A does not define the authorization’s scope, representation, validity, or revocation; implementations, credential issuers, or extensions must define operation identity and checks; and a credential obtained in that state must not be assumed to authorize later task messages.

**Mismatch/limit:** this current section is disconfirming evidence against any older characterization that A2A merely declares authentication schemes and lacks an in-task authorization boundary. It now has the handoff state and responsibilities, but deliberately leaves the decision object and enforcement semantics open. The profile gap therefore narrows rather than disappears.

### 6. DISPUTED — A2A “permits authorization delegation chains”

**Source:** [A2A Specification, §7.6](https://a2a-protocol.org/latest/specification/) — A2A Protocol, Linux Foundation project; rendered version **1.0.0**, page date not stated; accessed **2026-09-11**.

**Exact mismatch:** the normative text says A2A can delegate *fulfillment of an authorization* to the client through `TASK_STATE_AUTH_REQUIRED`. It does not, in the reviewed surface, define an authorization-delegation-chain object, hop semantics, chain validation, attenuation, or transitive authority. A targeted search for “authorization delegation chain” found no matching text. Describing the feature as permitting a client authorization handoff is supported; describing it as authorization delegation “chains” overstates the standard unless the term refers only to an implementation- or extension-defined arrangement.

**Impact:** this dispute does not weaken the profile conclusion; it strengthens the need for explicit principal/delegate and cross-hop verifier semantics if chains are required.

### 7. VERIFIED — A2A idempotency, cancellation, and history do not supply execution safety or audit evidence

**Source:** [A2A Specification](https://a2a-protocol.org/latest/specification/) — A2A Protocol, Linux Foundation project; rendered version **1.0.0**, page date not stated; accessed **2026-09-11**.

**Exact support:** Send Message operations only *may* be idempotent, with agents allowed to use `messageId` for duplicate detection. Cancel Task is idempotent as a request, but its behavior is described as attempting to cancel and can return `TaskNotCancelableError`; this does not promise rollback or absence of committed effects. Task retrieval permits the server to return less history than requested, and task-not-found can include expired, completed, and purged tasks. Separately, security guidance says agents should provide audit trails for sensitive operations—supporting the inference that retained task Messages/history are not themselves the normative audit mechanism.

**Mismatch/limit:** A2A does not use the exact phrase “best effort” for cancellation on the reviewed page. “Attempts to cancel” plus the non-cancelable error supports the digest’s no-guaranteed-stop interpretation, but “best effort” should be presented as a characterization, not quoted normative terminology.

### 8. VERIFIED — OpenAPI 3.2.0 identifies API operations within an OpenAPI Description and remains descriptive

**Source:** [OpenAPI Specification 3.2.0](https://spec.openapis.org/oas/latest.html) — OpenAPI Initiative, Linux Foundation; **2025-09-19**; accessed **2026-09-11**.

**Exact support:** the official latest page identifies version 3.2.0 and its revision history dates the release 2025-09-19. The version rules say Major.Minor selects the feature set and patch versions only correct/clarify it. OAS defines a language-agnostic description of HTTP APIs. `operationId` is case-sensitive and must be unique among all operations described in the API. `operationRef` may use a URI reference, including a non-relative URI plus JSON Pointer, to identify an operation. Parameters, bodies, schemas, responses, callbacks/webhooks, and operation security requirements provide the executable API contract surface.

**Mismatch/limit:** uniqueness is within the API/OpenAPI Description, not universal across independently published descriptions. URI-reference resolution does not provide immutable content pinning. No standard approval object or execution receipt was found. This supports the profile’s need for an absolute resolved reference plus digest when immutable cross-document identity is claimed.

### 9. UNVERIFIED — OpenAPI normatively “prefers” `operationRef` when multi-document `operationId` resolution can clash

**Source:** [OpenAPI Specification 3.2.0](https://spec.openapis.org/oas/latest.html) — OpenAPI Initiative, Linux Foundation; **2025-09-19**; accessed **2026-09-11**.

**Exact mismatch:** the retrieved normative text says `operationRef` **MAY** be used instead because `operationId` is optional, and gives relative and non-relative URI examples. It also says `operationId` must be unique among all API operations and recommends considering operations from all parsed documents when resolving an ID. The reviewed text does not establish a general recommendation that `operationRef` is preferred for multi-document clashes. The engineering preference for an absolute URI reference in a cross-standard profile is reasonable, but it is profile design, not a verified OpenAPI preference.

**Impact:** the underlying identity gap remains verified: neither a bare `operationId` nor an unpinned URI reference supplies globally immutable operation identity.

### 10. VERIFIED — Arazzo 1.1.0 supplies workflow/choreography and completion tests, not approval or security enforcement

**Source:** [Arazzo Specification 1.1.0](https://spec.openapis.org/arazzo/latest.html) — OpenAPI Initiative, Linux Foundation; **2026-05-17**; accessed **2026-09-11**.

**Exact support:** the official latest page and revision history identify 1.1.0 dated 2026-05-17; Major.Minor defines the feature set and patch versions clarify/correct. Arazzo defines source descriptions, workflow and step IDs, operation/workflow references, parameter and request-body projection, dependencies, outputs, `successCriteria`, timeout/correlation, and `end`/`goto`/`retry` handling. It says all success criteria must be satisfied for step success and tools must respect dependencies. It expressly says the specification does not enforce a security mechanism and leaves security to implementers. For AsyncAPI send operations, a step completes immediately after message send; broker acknowledgement and delivery confirmation are not modeled.

**Mismatch/limit:** Arazzo can express a later poll, receive, or postcondition test, so it is not intrinsically incapable of describing real-world verification. The accurate boundary is that step/workflow success is only as strong as the authored criteria and observations; Arazzo does not itself attest final-world truth or bind it to an approval.

### 11. VERIFIED — GNAP RFC 9635 supplies delegated-grant mechanics, while access meaning and approval binding remain application-defined

**Source:** [RFC 9635: Grant Negotiation and Authorization Protocol](https://www.rfc-editor.org/rfc/rfc9635.html) — IETF / RFC Editor; Internet Standards Track, **October 2024**; accessed **2026-09-11**.

**Exact support:** RFC 9635 defines delegated authorization to software, including API access and subject information. The grant request identifies a client instance and key, requested access, optional user, and interaction modes. It supports resource-owner/end-user interaction, asynchronous authorization, continuation, key-bound tokens, token/grant management, and request modification with re-evaluation. Issued access-token responses must include an `access` description reflecting granted rights, which may differ from what was requested. Key-bound tokens are the default security posture.

**Mismatch/limit:** the RFC says exact semantics of structured access-request fields depend on the access-object `type`; authorization/consent policy is out of scope for the RFC. The interaction-finish hash covers the client nonce, AS nonce, `interact_ref`, and grant-endpoint URI—not the deployment request body. The one-time interaction reference prevents replay of that interaction continuation value, not repeat execution under an issued access token. No `operationId`, action digest, task protocol, or execution receipt was found. GNAP is therefore a grant plane, not the deployment action/task/evidence model.

### 12. VERIFIED — GNAP revocation and grant mutation have the stated hard limits

**Source:** [RFC 9635](https://www.rfc-editor.org/rfc/rfc9635.html) — IETF / RFC Editor; Internet Standards Track, **October 2024**; accessed **2026-09-11**.

**Exact support:** a grant update returns the request to processing and requires authorization re-evaluation. The `durable` token flag explicitly permits an older token to remain usable after token rotation or modification of the underlying grant. Token rotation/revocation says the AS must invalidate the current token *if possible*, and the security section says self-contained stateless tokens make proactive revocation difficult; short lifetimes or out-of-scope signals to resource servers mitigate rather than eliminate the delay. This directly supports the digest’s requirement to bind and compare the exact action independently and its refusal to promise immediate universal revocation.

**Mismatch/limit:** the claim is correctly modal: older tokens *can* remain valid, not that they always do. Managed/online deployments can provide stronger revocation than the generic minimum.

### 13. VERIFIED — A non-commerce authorized deployment is composable as a profile over the existing standards

**Sources:** [A2A Specification](https://a2a-protocol.org/latest/specification/) — A2A Protocol, rendered **1.0.0**, no page date; [MCP Specification](https://modelcontextprotocol.io/specification/2026-07-28) and [MCP Tasks](https://tasks.extensions.modelcontextprotocol.io/specification/draft/tasks) — Model Context Protocol, revision **2026-07-28**, Tasks Draft; [OpenAPI 3.2.0](https://spec.openapis.org/oas/latest.html) — OpenAPI Initiative, **2025-09-19**; [Arazzo 1.1.0](https://spec.openapis.org/arazzo/latest.html) — OpenAPI Initiative, **2026-05-17**; [RFC 9635](https://www.rfc-editor.org/rfc/rfc9635.html) — IETF, **October 2024**; all accessed **2026-09-11**.

**Exact support:** A2A already supplies an external async task/control plane and negotiated URI extensions; OpenAPI supplies typed HTTP operation contracts; Arazzo supplies multi-step choreography and outcome criteria; GNAP supplies an asynchronous, key-capable delegated-grant lifecycle; MCP supplies an optional internal tool and draft task adapter. Each standard exposes extension or application-defined seams that can carry the missing joins without adding new network operations.

**Nature of verification:** no source declares this particular composition. The status is a constrained architectural inference from complementary normative mechanisms: the deployment exchange can use only A2A/MCP/GNAP/HTTP operations while the profile defines data objects and validation rules. The inference would fail if the profile introduced its own independent message transport, grant endpoint, general workflow engine, or task service.

### 14. VERIFIED — The profile still needs canonical action, approval resolution, execution receipt, and cross-standard mappings/tests

**Sources:** the same six normative specifications listed in Claim 13, with their publisher/date metadata as stated there; all accessed **2026-09-11**.

**Exact support:** the gap is a join failure, not lack of carriers. OpenAPI/Arazzo identify and shape operations but do not bind approval or attest execution. A2A’s auth-required state expressly delegates scope, representation, validity, revocation, operation identity, and authorization checks to implementations/issuers/extensions. GNAP’s access objects are type-defined and its interaction hash does not cover the domain action. MCP consent is unenforceable at protocol level. MCP `completed` includes `isError: true`, Arazzo send completion need not mean delivery, A2A send idempotency is optional, and MCP/A2A cancellation does not prove absence of effects. These mismatches require, for the claimed consequential-action guarantee:

- a deterministic exact-action representation and digest;
- an approval/authorization resolution bound to that action and executing delegate;
- an execution receipt distinguishing carrier completion from domain outcome and observed side effects;
- identifier, lifecycle, error, cancellation, and evidence mappings across layers; and
- conformance tests for mutation, replay/idempotency, partial effects, cancellation, revocation, and evidence joins.

Targeted disconfirmation searches found no `ExecutionReceipt`, approval object, or action digest in A2A; no approval, receipt, or idempotency primitive on MCP Tasks; no approval or execution receipt in OpenAPI; no approval or profile receipt in Arazzo; and no action digest, OpenAPI operation ID, task protocol, or execution receipt in GNAP. These are bounded findings on the eight reviewed official surfaces. The exact field names and the number “15 verifier rules” are design choices, not independently standardized necessities; the semantic functions above are the verified necessities.

### 15. VERIFIED — No new transport or task core is evidenced by the reviewed requirements

**Sources:** [A2A Specification](https://a2a-protocol.org/latest/specification/) — A2A Protocol, rendered **1.0.0**, no page date; [MCP Specification](https://modelcontextprotocol.io/specification/2026-07-28) and [MCP Tasks](https://tasks.extensions.modelcontextprotocol.io/specification/draft/tasks) — Model Context Protocol, revision **2026-07-28**, Tasks Draft; [OpenAPI 3.2.0](https://spec.openapis.org/oas/latest.html) — OpenAPI Initiative, **2025-09-19**; [Arazzo 1.1.0](https://spec.openapis.org/arazzo/latest.html) — OpenAPI Initiative, **2026-05-17**; [RFC 9635](https://www.rfc-editor.org/rfc/rfc9635.html) — IETF, **October 2024**; all accessed **2026-09-11**.

**Exact support:** A2A already standardizes task creation, retrieval/listing, streaming, push updates, cancellation, artifacts, version negotiation, and binding equivalence. MCP already standardizes JSON-RPC tool invocation and offers draft durable async tasks. OpenAPI/Arazzo already supply operation/workflow description, and GNAP already supplies grant negotiation/continuation/token management. The identified gaps are data semantics, trust joins, mappings, and conformance—not missing network or general task operations.

**Boundary:** this is an evidence judgment, not proof that a new core could never be useful. Within the reviewed non-commerce authorized-deployment scenario, no requirement was found that forces a separate transport or task state machine. A new core would need an additional requirement that cannot be carried or profiled over these standards.

## Overall verification judgment

The load-bearing conclusion survives fresh review. Current official material is consistent with a **thin but security-critical interoperability/conformance profile**: existing standards cover transport/tool calls, async task control, HTTP operation description, workflow choreography, and delegated grant mechanics. They do not close the semantic joins needed to say exactly what was approved, which immutable operation ran, what side effects occurred, or how heterogeneous terminal states map. No official source reviewed supplies a contrary end-to-end primitive or demonstrates a need for a replacement transport/task core.

The two corrections are local: replace “authorization delegation chains” with “delegation of authorization fulfillment to the client unless a separate extension defines chains,” and present absolute `operationRef` as a profile choice rather than an OpenAPI preference.
