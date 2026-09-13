# Standards landscape, round 1: MCP, A2A, OpenAPI, Arazzo

**Decision served:** choose among (A) a new AgentBridge protocol core, (B) an interoperability/conformance profile over existing standards, or (C) no separate protocol.

**Research boundary:** primary sources only; generic web search/open; accessed 2026-09-11. This digest describes only semantics stated in the retrieved specifications. An absence below means “not found in the reviewed normative surface,” not proof that no implementation has it.

## Bottom line

The reviewed standards already cover most transport, API-description, capability-advertisement, tool/action invocation, and asynchronous-task mechanics. They do **not** compose into the full target cycle without additional normative agreements. The missing common layer is narrow but real: a cross-protocol representation and binding for structured intent and constraints, delegated-principal authority, action-bound approval, request-to-result evidence, audit correlation, and end-to-end conformance.

Round-1 evidence therefore favors **B: a thin interoperability/conformance profile, with narrowly scoped extension objects where the existing protocols have no field or semantics**. A new full transport/task protocol (A) would duplicate substantial MCP/A2A/OpenAPI/Arazzo surface. C would leave every pair of implementations to define the missing mappings and trust semantics privately. This is a standards-landscape conclusion, not yet proof of adopter demand.

## Evidence records

### F1 — MCP core is an LLM-application-to-context/tool protocol, not an end-to-end delegation-governance protocol

- **Claim:** MCP 2026-07-28 defines Host, Client, and Server roles over JSON-RPC 2.0; stateless self-contained requests; per-request capability negotiation; an optional `server/discover` operation; server Resources, Prompts, and Tools; client Elicitation; and utilities for progress, cancellation, and errors. The core security text requires explicit user consent before tool invocation but also says MCP cannot enforce those principles at protocol level. Tasks are optional extensions, not core.
- **Direct source URL:** https://modelcontextprotocol.io/specification/2026-07-28
- **Publisher:** Model Context Protocol, a Linux Foundation project
- **Publication / last-updated date:** specification revision 2026-07-28
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** scope; interaction model; roles; discovery/capabilities; governance boundary; version/compatibility

### F2 — MCP HTTP authorization standardizes resource access, not an action-bound delegation mandate

- **Claim:** MCP authorization is optional overall and applies to HTTP transports. When used, it profiles OAuth/OIDC-related standards, requires protected-resource and authorization-server metadata discovery, requires a client ID, binds token requests to the canonical MCP server through the OAuth `resource` parameter, uses bearer access tokens on every HTTP request, and supports operation-time scope challenges and step-up authorization. It expresses access to an MCP resource and required scopes; it does not define a principal-to-agent mandate, parameter-level constraint language, approval record, approval-to-action digest binding, or audit receipt.
- **Direct source URL:** https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization
- **Publisher:** Model Context Protocol, a Linux Foundation project
- **Publication / last-updated date:** specification revision 2026-07-28
- **Accessed:** 2026-09-11
- **Confidence:** high for defined mechanics; medium for the bounded absence claim because only the current authorization chapter was inspected
- **Class:** authorization; delegated access; discovery; errors; scope boundary

### F3 — MCP Tasks supplies durable async execution and results/errors, but remains a draft extension

- **Claim:** The `io.modelcontextprotocol/tasks` extension is marked Draft for 2026-07-28. It is explicitly negotiated per request; the server alone decides whether to return a task. It defines `tasks/get`, `tasks/update`, `tasks/cancel`, durable task IDs, `working`, `input_required`, `completed`, `failed`, and `cancelled` states, outstanding input requests, and terminal result/error fields. Protocol errors use JSON-RPC errors; a tool result with `isError: true` is still a `completed` task, whereas a JSON-RPC execution error yields `failed`. Authentication and authorization must be checked on every task request. It has no approval or evidence object.
- **Direct source URL:** https://tasks.extensions.modelcontextprotocol.io/specification/draft/tasks
- **Publisher:** Model Context Protocol Tasks extension project
- **Publication / last-updated date:** specification revision 2026-07-28; page status Draft
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** version/compatibility; task semantics; results/errors; extension maturity

### F4 — A2A is the strongest existing horizontal agent-to-agent task protocol

- **Claim:** A2A defines communication between independent, opaque agent systems. Its core covers Agent Card discovery, skills and capability declaration, modality negotiation, message-driven task creation, polling/streaming/push updates, task retrieval/list/cancel/subscribe, multi-turn input, Artifacts as task outputs, error categories, and multiple equivalent bindings. Agent Cards declare security schemes derived from OpenAPI 3.2. However, the specification explicitly leaves authorization boundaries to each agent’s authorization/pres model, so it does not standardize delegated-principal authority. Extensions are URI-identified, explicitly requested, and require a new URI for breaking versions. Protocol negotiation uses `Major.Minor`, ignoring patch versions.
- **Direct source URL:** https://a2a-protocol.org/latest/specification/
- **Publisher:** A2A Protocol project, a Linux Foundation project
- **Publication / last-updated date:** rendered specification identifies released protocol 1.0.0; page date not stated
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** scope; roles; discovery/capabilities; task/action semantics; errors/results; auth declaration; extension/versioning

### F5 — A2A’s latest repository patch is 1.0.1, while the rendered specification header still says 1.0.0

- **Claim:** The official repository release list marks v1.0.1 as latest, dated 2026-05-26, with specification bug fixes. The rendered specification says “Latest Released Version 1.0.0.” Because A2A explicitly excludes patch numbers from wire compatibility, the wire feature level remains 1.0; nevertheless, the publication surfaces are out of sync and implementers need a pinned artifact/release policy for reproducible conformance.
- **Direct source URL:** https://github.com/a2aproject/A2A/releases
- **Publisher:** A2A Protocol project on GitHub
- **Publication / last-updated date:** v1.0.1 release 2026-05-26
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** version/compatibility; publication governance

### F6 — OpenAPI describes HTTP capabilities and exchanges; it does not create agent/task/approval semantics

- **Claim:** OpenAPI 3.2.0 is a language-agnostic description format for HTTP APIs. It describes servers, operations, parameters and request bodies, expected success/error responses, callbacks/webhooks, and security schemes/requirements including HTTP auth, API keys, mTLS, OAuth2, device flow, and OIDC. Security requirements can name scopes or roles, but non-OAuth roles are not otherwise defined or exchanged in-band. OpenAPI is descriptive: it has no agent roles, task state machine, approval object, delegated-authority chain, execution engine, or audit-evidence model. Extension fields use the `x-` prefix. Version compatibility is by `Major.Minor`; patch versions clarify rather than change the feature set.
- **Direct source URL:** https://spec.openapis.org/oas/latest.html
- **Publisher:** OpenAPI Initiative / Linux Foundation
- **Publication / last-updated date:** OpenAPI Specification 3.2.0, 2025-09-19; verified as the current published version on 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** high for defined mechanics; medium-high for bounded absence claims
- **Class:** API description; discovery/capabilities; action schema; results/errors; auth declaration; extension/versioning

### F7 — Arazzo adds declarative choreography and outcome tests, not trust or approval enforcement

- **Claim:** Arazzo 1.1.0 describes workflows across referenced OpenAPI, AsyncAPI, or Arazzo descriptions. It provides JSON-Schema workflow inputs, ordered/dependent steps, operation or workflow references, parameter/body bindings, outputs, response/message runtime expressions, success criteria, and `end`/`retry`/`goto` failure actions. This is the strongest reviewed standard for declaratively connecting multiple calls to an outcome. The specification explicitly does not enforce a security mechanism. It has no principal/delegate roles, approval primitive, task service, credential issuance, result attestation, or audit ledger. Extension fields use `x-`; compatibility is by `Major.Minor`.
- **Direct source URL:** https://spec.openapis.org/arazzo/latest.html
- **Publisher:** OpenAPI Initiative / Linux Foundation
- **Publication / last-updated date:** Arazzo Specification 1.1.0, 2026-05-17; verified as current on 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** workflow/choreography; structured constraints; success/failure semantics; security boundary; extension/versioning

### F8 — GNAP is directly relevant to the delegated-authority slice, but not to agent task execution

- **Claim:** GNAP (RFC 9635) is an IETF Proposed Standard for delegating authorization to a specific software instance, negotiating a grant over time among an authorization server, client, resource server, resource owner, and possibly an interacting end user, and conveying resulting access artifacts. It supports API access, subject information, user interaction/consent, and discovery. It does not define agent discovery, intent/task semantics, action execution, domain result/error schemas, approval-to-action binding, or audit receipts. GNAP is therefore a credible authorization building block or comparison point, not an AgentBridge substitute.
- **Direct source URL:** https://www.rfc-editor.org/info/rfc9635/
- **Publisher:** Internet Engineering Task Force / RFC Editor
- **Publication / last-updated date:** October 2024
- **Accessed:** 2026-09-11
- **Confidence:** high for scope and roles; medium-high for bounded absence claims
- **Class:** delegated authorization; interaction; discovery; standards maturity

## Exact coverage by protocol

| Protocol | Layer and roles | Interaction model | Discovery / capabilities | Task / action semantics | Results / errors | Auth declaration | Extensions / versioning | Conformance story visible in reviewed source |
|---|---|---|---|---|---|---|---|---|
| **MCP 2026-07-28** | LLM application integration. Host → embedded Client → capability-providing Server. | Stateless JSON-RPC requests; Tools, Resources, Prompts; optional server-to-client Elicitation; multi-round-trip input. | Optional `server/discover`; per-request client capabilities; lists of tools/resources/prompts. | Core tool invocation; durable async execution only through the Draft Tasks extension, created at server discretion. | JSON-RPC/protocol errors and method results; Tasks preserves final result or JSON-RPC error and distinguishes protocol failure from tool-level `isError`. | Optional HTTP OAuth profile, protected-resource/AS discovery, canonical resource indicator, bearer token, scopes and step-up. STDIO uses environment credentials instead. | Date-based protocol revision on each request; opt-in extensions; Tasks independently marked Draft. Core release notes specify a deprecation window, but that policy was not needed for the core gap finding. | BCP 14 requirements and an authoritative TypeScript schema. No end-to-end AgentBridge conformance classes or cross-protocol TCK were found. |
| **A2A 1.0** | Peer/client agent to remote opaque agent. | Send Message or streaming message; Task lifecycle; polling, SSE/streaming, webhook push; multi-turn continuation. | Public and authenticated extended Agent Cards; skills, modalities, optional capabilities, endpoints/bindings, security schemes, extensions. | Message initiates or continues work; Task carries state/history; Artifact carries output; cancellation and subscription are core. | Normative auth/authorization/validation/resource/system error categories with binding mappings; task status and artifacts. | OpenAPI-derived scheme declarations; actual authorization boundary/model is implementation-defined by each agent. | Wire version `Major.Minor`; patches ignored; URI-based opt-in extensions; new URI for breaking extension changes. | Normative functional equivalence across bindings and required version/capability behavior. No section titled conformance was found; the core spec reviewed did not define end-to-end governance conformance classes. |
| **OpenAPI 3.2.0** | Static description of an HTTP API; no first-class agent or principal roles. | Describes individual HTTP exchanges plus callbacks/webhooks; it does not execute them. | An obtained OpenAPI Description exposes servers, paths, operations, inputs, outputs, and schemas. It is description discovery, not a registry or agent rendezvous protocol. | Operation-level HTTP semantics only; no durable task abstraction. | Expected HTTP response codes/content schemas, including known errors; not a universal runtime error envelope. | Describes security schemes and per-operation requirements/scopes/roles; does not issue credentials or define delegation. | Semver-style spec version; `Major.Minor` feature compatibility; `x-` extensions. | BCP 14 requirements for conforming descriptions and tooling behavior. No runtime implementation or composed-cycle conformance model. |
| **Arazzo 1.1.0** | Declarative workflow description; author/processor/executor are implicit, not protocol roles. | Ordered/dependent calls across OpenAPI, AsyncAPI, or Arazzo workflows. | References source descriptions by URL and operation/workflow identifiers; no live agent discovery. | Workflow inputs, step dependencies, parameter/payload injection, outputs, success criteria, success/failure branching and retry. | Evaluates responses/messages with runtime expressions and criteria; controls `end`/`retry`/`goto`; no common service error envelope. | No enforced security mechanism; credentials can only be passed as ordinary referenced-operation parameters or handled by implementations. | Semver-style spec version; `Major.Minor` feature compatibility; `x-` extensions. | Structural conformance for Arazzo documents. No specified executor behavior for authority, approval, evidence, or audit. |
| **GNAP / RFC 9635** | Delegated authorization. Resource owner/end user ↔ Authorization Server ↔ Client instance ↔ Resource Server. | A grant can be negotiated over multiple interactions; returns access/subject artifacts. | Authorization-server/client configuration discovery; access requests describe requested access, not agent skills. | Authorizes later resource/API access; does not describe or execute the domain action. | Grant continuation/denial/access-token results at the authorization layer; not domain results. | This is its core purpose, including proof-bound client instances and interactive consent. | Stable RFC with IANA registries; extension details were not inspected in this round. | IETF Standards Track requirements. It does not define conformance for MCP/A2A/OpenAPI/Arazzo composition. |

## Target-cycle map: normative coverage versus explicit or observed boundary

Legend: **N** = normative mechanism exists at that protocol’s own layer; **D** = descriptive/declarative only; **E** = achievable only through a generic extension/arbitrary data or an implementation-defined policy; **—** = not in the reviewed normative surface.

| Target stage | MCP | A2A | OpenAPI | Arazzo | GNAP | What remains unstandardized across the cycle |
|---|---:|---:|---:|---:|---:|---|
| **Discovery** | **N** optional `server/discover` plus feature lists | **N** Agent Card and extended card | **D** retrievable API description | **D** source-description URLs | **N** authorization-server/client configuration discovery | No normative pointer graph or resolution rule linking an A2A skill ↔ MCP tool ↔ OpenAPI operation ↔ Arazzo workflow ↔ authorization resource. |
| **Capabilities** | **N** client/server capabilities and tool/resource/prompt catalogs | **N** skills, modalities, features, interfaces | **D** operations and JSON Schemas | **D** workflows, inputs, steps, outputs | Narrowly, requested access | No common capability identifier, semantic type system, or compatibility rule across protocols. |
| **Structured intent and constraints** | Tool name plus arguments; shape is tool-specific | Message Parts can carry text/files/arbitrary JSON; meaning is skill-specific | **D** typed operation inputs and schemas | **D** typed workflow input plus criteria/dependencies | Requested access at auth layer | No shared intent envelope or rule that binds natural-language/A2A intent to exact tool/API parameters, invariants, budget/risk constraints, or acceptable substitutions. |
| **Delegated principal authority** | **N**, but only resource-owner-to-client HTTP access/scopes | **E**: scheme is declared; authorization boundary is agent-defined | **D** security schemes/requirements | —; security not enforced | **N** grant delegation to a client instance | No common principal, delegate, delegation chain, parameter-level authority constraint, or portable proof that the action falls within delegated authority. |
| **Approval** | Consent is a required safety principle; Elicitation/input-required can ask for data, but enforcement is outside MCP | Input-required/HITL transport exists; no approval object or release semantics | A provider may describe an approval API, but approval is not an OAS primitive | A workflow can call an approval API, but approval is not an Arazzo primitive | Interactive consent can be part of a grant | No action-bound approval request/resolution, immutable action digest, expiry/consumption rule, or rule for changed actions. |
| **Action execution** | **N** Tool call; optional Tasks for async | **N** Message/Task delegation | Describes the HTTP operation that an implementation executes | Describes how a workflow executor invokes operations | Authorizes but does not execute | The same real-world action can be represented four ways; no normative translation or idempotency/correlation binding spans them. |
| **Result / error** | **N** method result/JSON-RPC error; Tasks final result/error | **N** Task state, Messages, Artifacts, and protocol/binding errors | **D** expected HTTP responses | **D** outputs, success criteria, failure branches | Auth-layer grant result/error only | No common outcome envelope, request-to-result evidence binding, provenance, signer, verification method, or cross-layer error taxonomy. “Structured” is covered; “verifiable” is not. |
| **Audit / versioning** | Version and extensions are normative; no audit record model found | Version/extensions normative; no cross-agent audit record found | Description/spec version and `x-` extensions | Workflow/spec version and `x-` extensions | Stable auth RFC and registries | No shared trace/correlation identifiers, policy/mandate/approval versions, append-only evidence, retention semantics, or conformance claim for the whole chain. |

## Can MCP + A2A + OpenAPI/Arazzo compose without private glue?

**No. They can coexist and are architecturally complementary, but the specifications do not define a closed composition.** A2A’s specification even includes a dedicated relationship-to-MCP appendix, while Arazzo normatively references OpenAPI operations; neither surface supplies the missing cross-standard bindings below.

### Cleanly composable parts

1. **A2A discovery plus agent delegation:** an Agent Card can advertise a remote agent, its skills, modalities, bindings, and declared security schemes; the client can send a message and track a Task to an Artifact or terminal state. [F4]
2. **MCP inside an agent:** the remote agent can independently discover/call MCP tools and use MCP Tasks for long-running tool work if both sides opt into the extension. [F1, F3]
3. **OpenAPI for HTTP action description:** operations, request schemas, expected responses, callbacks, and security requirements can describe non-MCP service APIs. [F6]
4. **Arazzo for declared multi-call procedures:** a workflow can bind and sequence OpenAPI/AsyncAPI calls, inspect responses, and branch/retry toward a declared outcome. [F7]
5. **GNAP or the MCP OAuth profile for authorization:** GNAP can negotiate delegated API access generally; MCP’s OAuth profile can authorize a client to a specific MCP server with scopes. [F2, F8]

### Exact gaps that force an extension, profile, or pair-specific agreement

1. **Identifier and discovery mapping:** no standard says that a specific A2A `AgentSkill` is implemented by a particular MCP Tool, OpenAPI `operationId`, or Arazzo `workflowId`. There is also no normative location in one artifact for the canonical identifiers of the others. A2A/OAS/Arazzo generic extension fields could carry this, but their vocabulary and resolution rules would be new.
2. **Intent normalization:** A2A arbitrary structured `data`, MCP tool arguments, OpenAPI request schemas, and Arazzo workflow inputs are independently typed. No reviewed standard defines lossless translation, constraint precedence, prohibited substitutions, or the authoritative representation.
3. **Principal and delegation binding:** MCP scopes authorize access to an MCP server; A2A advertises security but leaves authorization policy to the agent; OpenAPI declares schemes; Arazzo does not enforce security. None binds one portable principal/delegation proof to the exact A2A task, MCP request, API operation, and workflow step. GNAP covers delegated access but still needs an application profile to name these action objects and constraints.
4. **Approval semantics:** MCP and A2A can pause for input. Arazzo can call an approval API. Those are interaction mechanisms, not normative approval semantics. No core object says who approved which canonical action, under which policy version, until when, whether it is single-use, or whether execution must fail closed after mutation.
5. **Lifecycle translation:** A2A Task is core and client-message-driven; MCP Task is a Draft extension and is materialized solely at server discretion. Their states, persistence, cancellation, and error classification are not normatively mapped. Notably, MCP declares a tool-level `isError: true` result as task `completed`; a bridge must decide how that maps to A2A status and Artifact/error handling.
6. **Result verification:** all four principal standards can carry structured outputs/errors, but none defines a cryptographic or otherwise independently verifiable receipt that binds the output, side effects, request, principal, authorization, approval, executor, and time.
7. **Audit and correlation:** version fields exist independently, but no common trace/evidence object spans A2A → MCP/API → result. Correlation IDs placed in metadata or headers would be a profile choice unless standardized by an extension.
8. **Composition conformance:** individual documents and endpoints can conform to their own standards while the end-to-end behavior remains non-interoperable or unsafe. No reviewed source defines mandatory profiles, test vectors, or a TCK for this combined cycle.

### Overlaps and potential semantic collisions

- **Capability discovery overlap:** A2A Agent Cards, MCP discovery/catalogs, and OpenAPI descriptions all advertise capabilities at different granularity. Without precedence rules, catalogs can disagree or age independently.
- **Task overlap:** A2A and MCP Tasks both model async work, input-required pauses, cancellation, and terminal output. They differ in maturity and creation/control semantics, so a transparent one-to-one bridge is not specified.
- **Workflow overlap:** A2A multi-turn Task coordination and Arazzo multi-step workflow execution can each represent orchestration. A2A intentionally keeps remote internals opaque; Arazzo exposes a prescribed sequence. A profile must state whether an Arazzo workflow is an advertised external contract, an internal plan, or merely documentation.
- **Security overlap:** A2A borrows OpenAPI 3.2 security-scheme shapes; MCP prescribes a narrower HTTP OAuth flow; OpenAPI only describes schemes; GNAP is a different grant protocol. These are not logically contradictory, but declaring a common-looking OAuth scheme does not make token audience, scope semantics, principal identity, or delegated authority equivalent.
- **Error collision:** MCP Tasks separates JSON-RPC failure from tool-level failure, while OpenAPI/Arazzo reason from HTTP responses and criteria and A2A maps across multiple bindings. Cross-layer status is underdetermined, not normatively contradictory.
- **Version collision:** MCP uses dated protocol revisions, A2A negotiates `Major.Minor`, and OpenAPI/Arazzo version their description languages. A composed implementation needs a compatibility matrix; no standard defines one.

## Decision implication

**Recommended after round 1: B, provided “profile” is allowed to define a small set of new, protocol-neutral semantic objects and bindings.** The minimum justified surface is:

1. canonical identifiers and links among A2A skills/tasks, MCP tools/tasks, OpenAPI operations, and Arazzo workflows;
2. a structured intent/constraint envelope with deterministic canonicalization;
3. delegated-principal and authority-reference fields, preferably profileable to established authorization standards rather than a new credential transport;
4. an action-bound approval request/resolution with digest, expiry, mutation, and consumption rules;
5. a result/evidence envelope and common correlation/audit fields;
6. lifecycle/error mappings; and
7. conformance classes plus test vectors for supported compositions.

This is more than documentation but less than a replacement transport/task protocol. If round 2 shows these objects cannot be expressed as interoperable extensions of A2A/MCP plus an authorization profile, then A becomes justified for that narrow semantic core. Nothing in the current standards evidence supports recreating their full transport, discovery, or task machinery.

## Contradictions and unresolved publication issues

1. **A2A version publication drift:** the rendered “latest” specification says 1.0.0 while the official repository marks 1.0.1 latest. A2A’s patch-insensitive wire-version rule contains the compatibility impact, but a conformance profile should pin both protocol feature level and exact source revision. [F4, F5]
2. **MCP Tasks maturity wording:** the MCP core lists Tasks as a notable official extension, while the extension specification itself is marked Draft. Implementations should not treat “official extension” as “stable normative core.” [F1, F3]
3. **Consent versus enforcement:** MCP uses normative safety language for user consent but explicitly states that MCP itself cannot enforce it. This is a deliberate protocol boundary, not an executable approval guarantee. [F1]
4. **A2A security declaration versus policy:** A2A normatively declares authentication schemes and requires authorization checks, but the actual authorization boundary is defined by each agent. Two conforming agents can therefore expose semantically incompatible authorization models. [F4]

## Leads for round 2

1. Inspect the current MCP extension governance/registry and A2A extension governance to determine whether a jointly registered interoperability profile can normatively own cross-links, approval, and evidence without forking either core.
2. Inspect A2A’s Technology Compatibility Kit and MCP conformance tooling for reusable test-harness structure, exact conformance claims, and whether extension tests can be certified independently.
3. Compare GNAP with the current OAuth Rich Authorization Requests, Token Exchange, and protected-resource metadata stack specifically for parameter-constrained delegated authority and actor/delegation-chain representation. Avoid inventing a new authorization transport unless these cannot profile the needed semantics.
4. Search standards-body work for action-bound approval, transaction authorization, non-repudiable execution receipts, and interoperable audit evidence. This round found interaction/consent mechanisms but not a mature horizontal standard that closes those semantics.
5. Test one concrete end-to-end mapping with no hidden state: Agent Card skill → structured A2A Message → authority/approval → MCP Tool or OpenAPI/Arazzo execution → A2A Artifact. Every field that requires prose or private metadata becomes a candidate profile requirement.

## What was sought but not found

- A normative crosswalk from A2A Agent Cards/Skills/Tasks to MCP Servers/Tools/Tasks.
- A normative Arazzo source-description type or operation binding for MCP or A2A; the reviewed Arazzo source types are OpenAPI, AsyncAPI, and Arazzo.
- A shared principal/delegate/delegation-chain model across MCP and A2A.
- An action-bound, replay-safe approval object in MCP, A2A, OpenAPI, or Arazzo.
- A standard result attestation or receipt binding intent, authority, approval, execution, and side effects.
- A shared audit event schema or end-to-end correlation requirement.
- A conformance suite/profile for the combined MCP + A2A + OpenAPI/Arazzo cycle.
- A second additional horizontal initiative with enough current, primary, normative evidence to merit inclusion. Several research/draft proposals surfaced, but round 1 did not establish standards maturity or direct composability strongly enough to include them.

## Sources used (8 distinct primary sources)

1. Model Context Protocol, **Specification 2026-07-28** — https://modelcontextprotocol.io/specification/2026-07-28
2. Model Context Protocol, **Authorization 2026-07-28** — https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization
3. Model Context Protocol Tasks extension, **Tasks Draft 2026-07-28** — https://tasks.extensions.modelcontextprotocol.io/specification/draft/tasks
4. A2A Protocol, **Agent2Agent Protocol Specification (latest rendered)** — https://a2a-protocol.org/latest/specification/
5. A2A Protocol, **official releases** — https://github.com/a2aproject/A2A/releases
6. OpenAPI Initiative, **OpenAPI Specification 3.2.0** — https://spec.openapis.org/oas/latest.html
7. OpenAPI Initiative, **Arazzo Specification 1.1.0** — https://spec.openapis.org/arazzo/latest.html
8. IETF / RFC Editor, **RFC 9635: Grant Negotiation and Authorization Protocol** — https://www.rfc-editor.org/info/rfc9635/
