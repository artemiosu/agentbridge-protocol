# Consequential-action mapping, round 2: an agent deploys a service to production

**Decision served:** choose among **A** (a new AgentBridge protocol core), **B** (a thin interoperability/conformance profile), and **C** (stop or contribute only).

**Test case:** a principal authorizes an agent to deploy one immutable service artifact to one production target, potentially after an asynchronous approval round trip. The deployment can take minutes, create partial side effects, and expose separate status and cancellation operations.

**Research boundary:** only the named round-1 digest was admitted as prior research evidence. All standards claims below were checked again against current official specification/RFC pages during this run. No other project file was used as research evidence. Primary sources only; accessed 2026-09-11. “Not found” means not found in the reviewed normative surface, not proof of universal absence.

## Decision

**Choose B, but describe it honestly as a small, security-critical semantic profile—not as a metadata convention.** The narrowest safe composition keeps **A2A as the sole external task/control protocol**, uses **OpenAPI or Arazzo as the canonical executable contract**, uses **GNAP as the preferred delegated-authorization protocol** (or OAuth Rich Authorization Requests as a less self-contained alternate class), and uses **MCP only as an optional internal tool adapter**. It does not create a new transport, discovery system, RPC framework, credential protocol, or general task service.

The profile must nevertheless standardize three new protocol-neutral objects—an immutable `ActionEnvelope`, an action-bound `ApprovalResolution`, and an `ExecutionReceipt`—plus carrier bindings, lifecycle/error mappings, verifier rules, and conformance tests. That is more than prose, but it remains a profile because every exchange and lifecycle operation is carried by A2A/MCP/GNAP/HTTP, and every domain input is defined by OpenAPI/Arazzo rather than by a new AgentBridge command language.

**A becomes justified only if the project insists that the profile itself own message transport, task creation, streaming/polling, credential issuance, or a general-purpose workflow engine.** **C leaves material safety behavior private:** two individually conforming implementations can disagree on what was approved, whether a duplicate is a replay, whether cancellation prevented side effects, and whether “completed” means deployment success.

## Evidence records

### E1 — MCP supplies tool discovery/invocation and generic safety guidance, not enforceable action approval

- **Claim:** MCP 2026-07-28 defines stateless JSON-RPC requests, per-request capability negotiation, server tools, optional discovery, elicitation, cancellation, progress, and errors. It says hosts must obtain explicit user consent before invoking tools, but also says MCP cannot enforce those security principles at protocol level. Consequently, MCP consent language cannot itself serve as a portable, action-bound production approval.
- **Direct URL:** https://modelcontextprotocol.io/specification/2026-07-28
- **Publisher:** Model Context Protocol, Linux Foundation project
- **Publication / update date:** specification revision 2026-07-28
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** AI standard; scope; discovery; tool invocation; approval boundary; compatibility

### E2 — MCP HTTP authorization binds tokens to an MCP resource and scopes, not to immutable deployment parameters

- **Claim:** MCP authorization is optional and applies to HTTP transports. It standardizes protected-resource and authorization-server discovery, a canonical MCP resource URI, OAuth resource indicators, bearer-token presentation on every request, audience validation, per-operation scope challenges, and step-up flows. It allows required scopes to depend on request arguments, but does not define a delegated-principal chain, an action digest, single-use approval, parameter constraint vocabulary, or execution receipt.
- **Direct URL:** https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization
- **Publisher:** Model Context Protocol, Linux Foundation project
- **Publication / update date:** specification revision 2026-07-28
- **Accessed:** 2026-09-11
- **Confidence:** high for defined behavior; medium-high for the bounded absence claim
- **Class:** AI standard; authorization; resource/audience binding; approval boundary; compatibility

### E3 — MCP Tasks supplies durable asynchronous handles, but cancellation is cooperative and domain failure can be `completed`

- **Claim:** The draft `io.modelcontextprotocol/tasks` extension defines server-created durable task IDs; `working`, `input_required`, `completed`, `failed`, and `cancelled`; polling, updates, notifications, results, JSON-RPC errors, and cancellation. A tool result with `isError: true` is nevertheless a `completed` task, while only JSON-RPC execution errors use `failed`. Cancellation is eventually consistent and cooperative: acknowledgement does not guarantee work stops or that `cancelled` is reached. No idempotency mechanism was found on the reviewed Tasks surface.
- **Direct URL:** https://tasks.extensions.modelcontextprotocol.io/specification/draft/tasks
- **Publisher:** Model Context Protocol Tasks extension project
- **Publication / update date:** revision 2026-07-28; status Draft
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** AI standard; async lifecycle; error semantics; cancellation; compatibility

### E4 — A2A is the correct external task plane and now exposes the exact authorization extension boundary

- **Claim:** Current A2A defines Agent Cards and skills, URI-identified required extensions, structured JSON Parts, server-generated Tasks, polling/streaming/push, Artifacts, cancellation, and a `TASK_STATE_AUTH_REQUIRED` interrupted state. It permits a human-approval use case and authorization delegation chains, but explicitly says the state alone authorizes nothing and that the authorization’s scope, representation, validity, revocation, and operation-identification semantics must come from an implementation, credential issuer, or extension. Send Message is only optionally idempotent using `messageId`; cancel is idempotent but best effort. Messages/history are not reliable audit storage.
- **Direct URL:** https://a2a-protocol.org/latest/specification/
- **Publisher:** A2A Protocol, Linux Foundation project
- **Publication / update date:** current rendered protocol 1.0 surface; page does not state a publication date
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** AI standard; discovery; extensions; async lifecycle; in-task authorization; idempotency; audit boundary; compatibility

### E5 — OpenAPI gives precise HTTP operation and schema identity, but remains descriptive

- **Claim:** OpenAPI 3.2.0 defines globally unique-within-description, case-sensitive `operationId` values, URI-reference `operationRef` values, typed parameters/request bodies, responses, callbacks/webhooks, and per-operation security requirements. `operationRef` is preferred where multi-document `operationId` resolution can clash. The description neither executes an operation nor binds an approval, identity, idempotency record, task, or evidence receipt to it.
- **Direct URL:** https://spec.openapis.org/oas/latest.html
- **Publisher:** OpenAPI Initiative / Linux Foundation
- **Publication / update date:** OpenAPI Specification 3.2.0, 2025-09-19; current page re-opened 2026-09-11
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** API description; exact operation identity; schemas; callbacks; security declaration

### E6 — Arazzo describes the deployment choreography and outcome tests, not authorization enforcement or final-world truth

- **Claim:** Arazzo 1.1.0 defines unique workflow/step IDs, references to OpenAPI/AsyncAPI/Arazzo source descriptions, operation/workflow invocation, parameters and request bodies, dependencies, outputs, success criteria, retry/goto/end behavior, and async correlation/timeout. It explicitly does not enforce security. For an AsyncAPI send step, completion occurs when the message is sent; broker acknowledgement or delivery is not modeled. Arazzo therefore can describe deploy→poll→verify→rollback choreography, but its step completion is not by itself proof that production reached the approved state.
- **Direct URL:** https://spec.openapis.org/arazzo/latest.html
- **Publisher:** OpenAPI Initiative / Linux Foundation
- **Publication / update date:** Arazzo Specification 1.1.0, 2026-05-17
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** workflow description; exact step identity; constraints; async correlation; security/evidence boundary

### E7 — GNAP supplies the strongest native delegated-authority lifecycle, but its access semantics are application-defined

- **Claim:** GNAP RFC 9635 delegates API access to a particular client instance; separates authorization server, client instance, resource server, resource owner, and end user; supports structured or referenced access rights, interactive/asynchronous grant continuation, key-bound tokens, request modification with re-evaluation, grant cancellation, and optional token rotation/revocation. The issued token response must state its granted `access` rights. However, the content and comparison semantics of structured access objects remain resource/application defined. The interaction hash binds client/AS nonces, `interact_ref`, and grant endpoint—not the proposed deployment body. Grant modification can leave older independent tokens valid, and proactive revocation of stateless tokens can be difficult.
- **Direct URL:** https://www.rfc-editor.org/rfc/rfc9635.html
- **Publisher:** IETF / RFC Editor
- **Publication / update date:** October 2024
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** delegated authorization; principal/client roles; interaction; async continuation; token binding; revocation; mutation

### E8 — OAuth RAR is a usable extension point for fine-grained deployment authority, not a ready deployment authorization type

- **Claim:** OAuth Rich Authorization Requests (RFC 9396) defines `authorization_details`, collision-resistant `type` identifiers, and reusable `locations`, `actions`, `datatypes`, `identifier`, and `privileges` fields, while allowing API-specific fields. The authorization server controls each type’s interpretation and must reject unknown or invalid fields. The RFC does not standardize how to compare arbitrary authorization-detail objects and defines no extension to the authorization response. A production-deployment type, action-digest field, approval semantics, and AS-to-resource-server verification behavior therefore remain new profile semantics.
- **Direct URL:** https://www.rfc-editor.org/rfc/rfc9396.html
- **Publisher:** IETF / RFC Editor
- **Publication / update date:** May 2023
- **Accessed:** 2026-09-11
- **Confidence:** high
- **Class:** OAuth authorization; structured access; extension point; comparison/response boundary

## Narrowest standards-composed representation

### Architectural rule: one task plane, one executable identity, one authorization decision

1. **A2A is the public control plane.** The deployment agent publishes an Agent Card and a `deploy-service-production` skill. It declares the AgentBridge profile URI as a required A2A extension. A client sends an A2A Message containing a structured `ActionEnvelope`; the server must return a Task rather than a direct Message for this consequential action. The A2A Task remains the public status/cancel/subscription handle. [E4]
2. **OpenAPI/Arazzo is the executable contract.** A direct deployment uses an absolute OpenAPI operation reference; a multi-step deployment uses an absolute Arazzo document identity plus `workflowId`. The contract is content-pinned. Human wording, A2A skill prose, and MCP tool descriptions are not operation identity. [E5][E6]
3. **GNAP is the preferred authority plane.** When the Task reaches `TASK_STATE_AUTH_REQUIRED`, the client completes the GNAP interaction out of band. The deployment access request uses a registered profile access-object `type`; the resulting token is key-bound to the executing client instance and carries or resolves the exact action digest. GNAP continuation supplies asynchronous authorization and token/grant management. [E7]
4. **OAuth RAR is an alternate authority class.** It can carry the same profile access object in `authorization_details`, but the profile must additionally prescribe how the resource server obtains and validates those details, because RFC 9396 does not define an authorization-response extension or universal detail comparison. [E8]
5. **MCP is optional behind the A2A agent.** If the agent invokes an MCP deployment tool, it passes the same action digest, approval reference, authority reference, and idempotency key in the profile’s MCP request metadata. MCP Tasks may mirror the provider job internally, but the A2A Task remains authoritative to the caller. Exposing two peer task handles without this ownership rule creates an unsolved split-brain lifecycle. [E1][E3][E4]

### Concrete end-to-end sequence

| Step | Concrete representation | Classification | What is still new |
|---|---|---|---|
| 1. Discover agent | Fetch signed/HTTPS A2A Agent Card; select `AgentSkill.id=deploy-service-production`; require profile extension URI. | **Directly normative + profile semantics + operator trust** [E4] | Extension URI; mapping from skill ID to executable contract; which card signer/domain is trusted. |
| 2. Resolve exact operation | Resolve an absolute OpenAPI operation URI such as `https://deploy.example/spec/openapi.json#/paths/~1deployments/post`, or an absolute Arazzo document plus `workflowId=deployProduction`; verify pinned document digest. | **Directly normative + profile semantics** [E5][E6] | Globally canonical reference form, digest algorithm, contract pinning, and skill→operation/workflow mapping. |
| 3. Form exact action | Validate the deployment request against the referenced schema: immutable image digest, service ID, environment=`production`, target region/cluster/namespace, rollout strategy, health gates, deadline, and rollback option. Canonicalize the full action envelope and compute `action_digest`. | **Directly normative schema + profile semantics + domain/operator policy** [E5][E6] | Envelope, canonicalization scope, digest, constraint precedence, immutable-artifact requirement, and policy values. |
| 4. Create public task | Send A2A Message with one structured data Part and profile metadata; use unique `messageId`; require Task response and bind returned Task ID to `action_digest`. | **Directly normative + profile semantics** [E4] | Consequential-action rule forbidding direct Message completion; mandatory idempotency behavior; binding record. |
| 5. Request authority/approval | Task enters `TASK_STATE_AUTH_REQUIRED`; status metadata points to the action digest and an out-of-band GNAP grant interaction. GNAP requests a key-bound token for a structured deployment access object. | **Directly normative extension point + new semantics + operator policy** [E4][E7] | Deployment access type; exact action binding; approver eligibility; policy version; presentation requirements. |
| 6. Resolve approval | Authorization server verifies the principal/approver, displays the exact canonical action or deterministic rendering, obtains the decision, and issues a token plus signed `ApprovalResolution` binding the decision to `action_digest`. | **Profile semantics + operator policy**; GNAP supplies the interaction, not the approval object [E7] | Approval object, signature, display binding, expiry, single-use rule, policy reference, and atomic consumption. |
| 7. Dispatch | Executor revalidates action, approval, authority, operation contract, current policy, and preconditions; atomically consumes the approval/idempotency key; calls the exact OpenAPI operation or Arazzo workflow. | **Directly normative invocation description + profile verifier rules + operator policy** [E5][E6][E7] | All cross-object verifier rules, atomic consume behavior, and fail-closed policy. |
| 8. Execute asynchronously | A2A Task is `WORKING`; optionally map a provider job and internal MCP Task to it; poll, stream, or push updates. Arazzo may describe deploy/poll/health/rollback calls. | **Directly normative + profile lifecycle mapping** [E3][E4][E6] | Cross-task correlation, authoritative-state precedence, and partial/unknown outcome vocabulary. |
| 9. Report outcome | A2A Artifact carries a signed `ExecutionReceipt`; A2A terminal state derives from the receipt’s domain outcome, not merely HTTP 2xx, Arazzo step completion, or MCP `completed`. | **Directly normative carrier + new evidence semantics + domain success policy** [E3][E4][E5][E6] | Receipt schema, signer, evidence level, side-effect list, request/result digests, and common outcome mapping. |
| 10. Cancel/revoke | A2A `CancelTask` requests orchestration cancellation; bridge forwards MCP/provider cancellation when applicable. GNAP grant/token management handles authority revocation. Receipt records whether cancellation became effective and what side effects remain. | **Directly normative signal + new coupling + operator compensation policy** [E3][E4][E7] | Cancellation propagation, revocation check cadence, effective-vs-requested distinction, and compensation authority. |

## Stage classification summary

Legend: **N** directly normative at the relevant standard’s own layer; **P** a profile/extension point exists but semantics must be defined; **X** impossible or contradictory as a portable guarantee; **O** domain/operator policy.

| Stage | Label | Assessment |
|---|---|---|
| Discovery | **N + P + O** | A2A/MCP/API/auth discovery exists; cross-links and trust selection do not. |
| Exact operation identity | **N + P** | OpenAPI/Arazzo identify operations/workflows; a global absolute form, content pin, and A2A/MCP mapping are new. |
| Structured parameters/constraints | **N + P + O** | JSON Schema and workflow inputs validate shape; canonical action scope, constraint precedence, and production policy values are new. |
| Delegated authority | **N + P + O** | GNAP directly delegates to a client instance; OAuth RAR carries details. The deployment access type and resource-server enforcement semantics are new; trusted issuers and policy are local. |
| Approval binding | **P + O** | A2A can pause and GNAP can interact, but neither binds the human decision to immutable deployment bytes. The approval object and who may approve are new. |
| Async execution | **N + P** | A2A and MCP Tasks carry async work; Arazzo describes choreography. Cross-layer ownership and lifecycle mapping are new. |
| Status/result/error | **N + P + O** | Native statuses, results, errors, and Artifacts exist. Domain success, partial side effects, outcome unknown, and evidence are new. |
| Audit evidence | **P + O** | Generic metadata and signed Agent Cards are carriers/trust hints, not execution receipts. Receipt semantics, retention, and accepted signers are new. |
| Cancellation | **N + P + X + O** | Cancel signals exist, but guaranteed stop/rollback is impossible once effects commit. Propagation and compensation are profiled/local. |
| Authority revocation | **N + P + X + O** | GNAP supports grant/token revocation, but immediate invalidation of offline/stateless tokens is not portable. Task coupling and maximum revocation latency are new/local. |

## Exact profile surface

The following is the complete minimum new normative surface for this scenario. Names are illustrative but the semantics are not optional for the claimed conformance class.

### 1. Profile identifiers and discovery binding

Invent these identifiers and fields:

- `profile_uri`: collision-resistant URI naming the profile major version.
- `action_media_type`: media type for the canonical `ActionEnvelope`.
- `approval_media_type`: media type for `ApprovalResolution`.
- `receipt_media_type`: media type for `ExecutionReceipt`.
- `authorization_details_type`: one collision-resistant GNAP/RAR access-object type URI for action-bound deployment authority.
- `skill_bindings[]` in the A2A `AgentExtension.params`, each containing:
  - `skill_id`: exact A2A `AgentSkill.id`.
  - `execution_ref`: absolute OpenAPI operation reference or absolute Arazzo workflow reference.
  - `execution_kind`: `openapi-operation` or `arazzo-workflow`.
  - `contract_digest`: digest of the fully resolved executable description.
  - `mcp_tool_name`: optional internal MCP tool name; never authoritative operation identity.
- `required_conformance_class`: tells a client whether GNAP or OAuth-RAR authority and whether execution receipts are mandatory.

Existing A2A extension declaration/negotiation carries these fields, but their vocabulary and mapping semantics are new. [E4]

### 2. `ActionEnvelope`

Invent exactly these generic fields:

- `profile`: exact `profile_uri`.
- `action_id`: client-generated globally unique identifier.
- `principal_ref`: stable issuer-qualified identity of the principal on whose behalf the action is proposed.
- `delegate_ref`: stable client-instance or key reference for the agent permitted to execute.
- `agent_skill_ref`: absolute Agent Card identity plus `AgentSkill.id`.
- `execution_kind`: `openapi-operation` or `arazzo-workflow`.
- `execution_ref`: absolute operation/workflow identity.
- `contract_digest`: algorithm-tagged digest of the fully resolved OpenAPI/Arazzo contract and referenced schemas.
- `arguments`: the exact structured operation/workflow inputs, including all defaults materialized; no later default insertion is allowed.
- `constraints`: explicit execution-time predicates not already expressible as operation inputs, such as expected current revision, permitted target set, and “no substitution.”
- `idempotency_key`: unique logical-execution key.
- `created_at`: creation timestamp.
- `not_before`: optional earliest dispatch time.
- `expires_at`: last permitted dispatch time.

Derive `action_digest = SHA-256(RFC8785(ActionEnvelope))`; do not place the digest inside the hashed object. RFC 8785 canonicalization is reused as a technique already normatively used by A2A for Agent Card signing, but applying it to action messages is new profile semantics. [E4]

The deployment-specific keys under `arguments`—for example `service_id`, immutable `artifact_digest`, `environment`, `cluster`, `namespace`, `region`, `rollout_strategy`, `max_unavailable`, `health_check_ref`, `deadline`, and `rollback_on_failure`—must be defined by the selected OpenAPI/Arazzo contract, not by this horizontal profile. Their allowed values and risk thresholds are **O**, not new horizontal protocol fields.

### 3. Authorization access object

Reuse existing GNAP/RAR fields `type`, `actions`, `locations`, and where appropriate `identifier`; invent these additional type-specific fields:

- `action_digest`.
- `execution_ref`.
- `contract_digest`.
- `principal_ref`.
- `delegate_ref` or `delegate_key_thumbprint`.
- `approval_id`.
- `not_before`.
- `expires_at`.
- `max_executions`, fixed to `1` for this conformance class.

For GNAP, the AS must return the granted access object in the token’s `access` array, and the executor/resource server must compare it to the proposed action. Prefer a non-bearer, client-key-bound token. [E7] For OAuth RAR, the profile must additionally define the token claim or online authorization-data lookup by which the resource server obtains the granted object; RFC 9396 alone does not do so. [E8]

### 4. `ApprovalResolution`

Invent these fields:

- `approval_id`: globally unique decision identifier.
- `action_digest`: immutable action approved or denied.
- `decision`: `approved` or `denied`.
- `approver_ref`: issuer-qualified approving identity or policy actor.
- `principal_ref`: principal for whom the approval applies.
- `delegate_ref`: exact executing client instance/key.
- `authority_ref`: opaque identifier of the GNAP grant/token authorization decision; never the credential itself.
- `policy_ref`: immutable identifier of the approval policy version applied.
- `presentation_digest`: digest of the deterministic human/policy presentation actually evaluated.
- `issued_at`.
- `expires_at`.
- `max_executions`, fixed to `1`.
- `proof`: detached or enveloping signature over the canonical resolution, including signer key ID and algorithm.

The profile also invents the rule that an approval is consumed atomically at the first authorized dispatch, and that the resulting `execution_id` and `consumed_at` are recorded. GNAP’s one-time `interact_ref` prevents reuse of that interaction continuation value; it does **not** make a deployment approval single-use. [E7]

### 5. Carrier and correlation fields

Invent one namespaced A2A/MCP metadata object with:

- `action_digest`.
- `approval_id`.
- `authority_ref`.
- `audit_trace_id`.
- `idempotency_key`.
- `a2a_task_id`.
- `mcp_task_id` (optional).
- `provider_job_id` (optional).
- `execution_id` once assigned.

Carrier rules:

- A2A Message `extensions` names the profile URI; its namespaced `metadata` contains the full `ActionEnvelope` or a content-addressed reference plus digest.
- A2A Task/TaskStatus metadata carries correlation and profile lifecycle state.
- A2A Artifact extension carries the final `ExecutionReceipt`.
- MCP `tools/call` namespaced metadata carries the same correlation object; the MCP tool arguments must be a deterministic projection of `ActionEnvelope.arguments`.
- OpenAPI HTTP requests carry `idempotency_key` in the exact header/body field described by that API. If the target API offers no idempotency mechanism, the executor must serialize dispatch and retain the consume record; it cannot claim crash-safe exactly-once execution.
- Provider job IDs and MCP task IDs are subordinate aliases of the A2A Task, never alternate public sources of truth.

### 6. Common lifecycle and outcome mapping

Invent this profile lifecycle vocabulary:

`awaiting_approval`, `authorized`, `submitted`, `running`, `blocked_input`, `cancel_requested`, `succeeded`, `failed`, `partially_applied`, `cancelled`, `revoked`, `outcome_unknown`.

Invent these mandatory mappings:

| Profile state/outcome | A2A mapping | MCP mapping | Required interpretation |
|---|---|---|---|
| `awaiting_approval` | `TASK_STATE_AUTH_REQUIRED` | `input_required` only when the outstanding request is profile-typed as approval | A generic input request is never inferred to be approval. |
| `authorized` | Task metadata; outer A2A state may remain interrupted until dispatch | metadata only | Token/approval verified but not consumed. |
| `submitted` | `TASK_STATE_SUBMITTED` | `working` after durable task creation | Provider has acknowledged a durable job or executor has durable ownership. |
| `running` | `TASK_STATE_WORKING` | `working` | Execution is active. |
| `blocked_input` | `TASK_STATE_INPUT_REQUIRED` | `input_required` | Non-authorization input is missing. Any action-changing input creates a new action digest and approval cycle. |
| `cancel_requested` | Task metadata while nonterminal | cancellation acknowledgement plus metadata | Signal acknowledged; no claim that work stopped. |
| `succeeded` | `TASK_STATE_COMPLETED` plus receipt outcome `succeeded` | `completed` plus receipt outcome `succeeded` | Domain postconditions and evidence, not carrier completion alone, decide success. |
| `failed` | `TASK_STATE_FAILED` or `REJECTED` with zero committed effects | MCP `failed`, or `completed` with `isError:true`, after receipt inspection | Protocol failure and domain failure are normalized without erasing their original category. |
| `partially_applied` | terminal `TASK_STATE_FAILED` plus receipt outcome | `completed`/`failed`/`cancelled` plus receipt outcome | Some production effects committed; compensation may be pending or failed. |
| `cancelled` | `TASK_STATE_CANCELED` plus evidence no unreported committed effects remain | `cancelled` plus same evidence | Cancellation signal alone is insufficient. |
| `revoked` | `REJECTED` before dispatch; `FAILED` after start | `failed`/`cancelled` after authorization recheck | No later protected step may run; prior effects are not undone. |
| `outcome_unknown` | terminal `TASK_STATE_FAILED` plus receipt outcome | any terminal state plus receipt outcome | Executor cannot prove whether a commit occurred; never report success or clean cancellation. |

This mapping resolves a real semantic collision: an MCP Task with a tool-level `isError:true` is `completed`, so direct `completed`→A2A `COMPLETED` mapping would be unsafe. [E3]

### 7. `ExecutionReceipt`

Invent these fields:

- `receipt_id`.
- `execution_id`.
- `action_digest`.
- `approval_id`.
- `authority_ref`.
- `idempotency_key`.
- `execution_ref`.
- `contract_digest`.
- `request_digest`: digest of the exact provider request after deterministic translation.
- `a2a_task_id`.
- `mcp_task_id` (optional).
- `provider_job_id` (optional).
- `outcome`: one of `succeeded`, `failed`, `partially_applied`, `cancelled`, `revoked`, `outcome_unknown`.
- `started_at`, `last_observed_at`, and `finished_at` when known.
- `authority_checked_at`: last successful authority/revocation check before the relevant commit.
- `approval_consumed_at`.
- `result`: structured provider/domain result or error.
- `result_digest`.
- `side_effects[]`, each with `effect_id`, `effect_kind`, `resource_ref`, `before_digest`, `after_digest`, `effect_status`, `observed_at`, and `evidence_ref`.
- `cancellation`, when requested, with `requested_at`, `acknowledged_at`, `effective_at`, and `compensation_execution_ref` if any.
- `evidence_level`: `executor_asserted`, `provider_signed`, or `independently_observed`.
- `evidence_refs[]`: content-addressed logs, control-plane responses, health observations, or attestations.
- `executor_ref`.
- `proof`: signature over the canonical receipt.

No reviewed standard provides this receipt. A2A Artifact is the carrier, while provider responses, health/status operations, and Arazzo outputs supply inputs to it. [E4][E5][E6]

### 8. Verifier rules that must be invented

1. **Extension gate:** reject the action if either A2A endpoint did not negotiate the exact required profile URI/version.
2. **Discovery verification:** verify Agent Card origin/signature under configured trust; resolve `skill_id` exactly; reject undeclared or ambiguous skill bindings.
3. **Contract pinning:** resolve all OpenAPI/Arazzo references, verify `contract_digest`, and reject mutable/unpinned descriptions or unresolved/ambiguous operations.
4. **Schema closure:** validate `arguments`; materialize every default before hashing; reject unknown properties where the action schema does not explicitly allow them.
5. **Canonical action:** canonicalize the complete `ActionEnvelope`; recompute `action_digest`; never hash a natural-language summary in place of the structured action.
6. **No post-approval transformation:** verify the provider `request_digest` is the deterministic projection prescribed by the pinned contract/binding; any substitution, newly inserted default, changed target, changed artifact tag/digest, or changed constraint requires a new action digest and approval.
7. **Approval verification:** verify signature, `decision=approved`, digest equality, principal/delegate equality, approver eligibility, policy version, time window, presentation binding, `max_executions=1`, and unconsumed status.
8. **Authority verification:** verify trusted issuer, audience/resource server, key binding to `delegate_ref`, deployment access-object type, action/operation/contract equality, time bounds, and current revocation state.
9. **Double check at commit:** perform rules 6–8 immediately before the first irreversible provider operation and before every separately authorized Arazzo step; a check made only when the A2A task was created is insufficient.
10. **Atomic consume/idempotency:** atomically associate `(approval_id, action_digest, idempotency_key)` with one `execution_id`. Same key and same digest returns the same execution; same key with a different digest is rejected; a consumed approval can never start a second execution.
11. **Task ownership:** ensure all MCP/provider task aliases map to the one A2A Task and action digest. Conflicting mappings fail closed to `outcome_unknown` pending reconciliation.
12. **Outcome derivation:** inspect domain result, committed side effects, postconditions, and evidence. Never derive `succeeded` solely from HTTP success, Arazzo step completion, A2A `COMPLETED`, or MCP `completed`.
13. **Cancellation truthfulness:** represent request, acknowledgement, and effective stop separately; require evidence of remaining effects before emitting clean `cancelled`.
14. **Receipt verification:** verify receipt signature, signer authorization, all identifier/digest joins, evidence freshness, and claimed evidence level. An unsigned API response can support `executor_asserted`, not `provider_signed`.
15. **Audit completeness:** append the proposed action, approval decision, consume record, authority checks, state transitions, cancellation/revocation events, side effects, and receipt under one `audit_trace_id`; detect gaps and duplicate sequence positions.

### 9. Trust and operator policies that remain local

These must be configured, not standardized as universal values:

- trusted Agent Card origins, signing keys, and key-rotation rules;
- trusted OpenAPI/Arazzo publishers and allowed contract-update process;
- trusted authorization servers for each production resource;
- principal identifier namespaces and delegate/client-instance registration;
- which approver identities/roles can authorize which production targets, including separation-of-duty rules;
- policy version governing change windows, artifact provenance, rollout limits, health gates, and rollback;
- trusted executor/control-plane receipt signers and acceptable `evidence_level` for production;
- online revocation-check requirement, maximum cache age, token lifetime, and clock-skew tolerance;
- audit log confidentiality, retention, append-only mechanism, and access policy;
- the irreversible commit boundary for each provider operation;
- compensation/rollback operation, its own authority requirements, and when automatic compensation is allowed;
- failure posture for unavailable authorization, evidence, status, or cancellation endpoints (the consequential-action class should fail closed before commit);
- maximum time a task may remain `authorized` without dispatch and maximum time `outcome_unknown` may remain unresolved.

## Mutation and failure-case tests

| Test | Standards-native behavior | Profile rule and expected verdict | Result |
|---|---|---|---|
| Mutation after approval | GNAP allows an existing grant request to be modified and re-evaluated; old independently issued tokens may remain valid. A2A permits later messages on the same Task but says an authorization decision must not be assumed to authorize them. [E4][E7] | Change image digest, target cluster, rollout limit, inserted default, workflow/contract digest, or any constraint. Recomputed `action_digest` differs; old approval/token is rejected even if its broader grant remains valid. Start a new approval. | **Pass only with new action digest and equality checks.** Native standards alone fail this test. |
| Replay / idempotency | A2A Send Message may use `messageId` for duplicate detection but is not required to be idempotent; MCP Tasks exposes no reviewed idempotency key. GNAP key binding prevents use by the wrong client but not repeat use by the authorized client. [E3][E4][E7] | Atomically consume single-use approval plus idempotency key. Duplicate same digest returns same execution/receipt; same key with different digest rejects; no second provider call. | **Pass only with profile storage/atomicity and provider idempotency or serialized dispatch.** Exactly-once across crash boundaries is **X** if neither exists. |
| Partial execution | Arazzo can branch/retry and A2A/MCP can carry errors, but no common partial-side-effect outcome exists. MCP may call a domain error `completed`. [E3][E4][E6] | Receipt lists each committed effect and marks `partially_applied`; outer A2A state is terminal failed, never completed-success. Rollback is a distinct operation whose authority is either pre-approved in the action or separately approved. | **Pass with receipt and side-effect inventory.** Clean atomic rollback is **X** where provider operations are not transactional/compensable. |
| Cancellation | A2A and MCP both attempt/cooperate; neither guarantees stop. [E3][E4] | Emit `cancel_requested` on acknowledgement. Emit `cancelled` only with evidence of effective stop and remaining effects; otherwise `partially_applied` or `outcome_unknown`. Forward to provider/MCP and record each acknowledgement. | **Pass for truthful semantics; guaranteed no-effect cancellation is X.** |
| Authority revocation | GNAP can cancel grants and revoke managed tokens, but invalidation is “if possible”; stateless tokens can impede immediate revocation. Existing task cancellation is separate. [E7] | Check authority before dispatch and each irreversible step; bound revocation latency via online check or short-lived token; stop later steps and record `revoked`. Do not claim revocation undid prior effects. | **Conditional pass.** Immediate global revocation is **X** without online enforcement or equivalent control. |
| Result/evidence | A2A Artifacts, MCP results, OpenAPI responses, and Arazzo outputs carry data, not a common attestation of production state. Arazzo async send completion is especially weaker than deployment completion. [E3][E4][E5][E6] | Signed receipt joins action, approval, authority, exact request, provider job, side effects, postcondition observations, and timestamps. Evidence level states whether the result is merely executor asserted or independently/provider verified. | **Pass only with the new receipt and trusted evidence source.** Independent proof is **X** when the control plane supplies no trustworthy evidence. |

## Why this is still a thin profile

### Surface reused without reinvention

- A2A: discovery, skills, extension negotiation, task creation, task states, polling, streams, push, Artifacts, cancellation, binding equivalence, and protocol errors. [E4]
- MCP: optional internal tool call, authorization to an MCP resource, async task handle, polling/update/cancel, and native results/errors. [E1][E2][E3]
- OpenAPI/Arazzo: operation/workflow identity, request and constraint schemas, exact parameter projection, expected responses, status callbacks/polls, success criteria, and multi-step choreography. [E5][E6]
- GNAP/OAuth: client-instance delegation, user interaction, asynchronous authorization continuation, structured access-right carrier, token/key binding, and grant/token management. [E7][E8]

### Surface newly owned by the profile

- 3 signed/hashed objects: `ActionEnvelope`, `ApprovalResolution`, `ExecutionReceipt`;
- 1 authorization-details/access-object type;
- 1 A2A extension URI plus MCP metadata binding;
- 1 cross-standard identifier/correlation vocabulary;
- 1 small common lifecycle/outcome mapping;
- 15 verifier rules;
- conformance tests for mutation, replay, partial execution, cancellation, revocation, and evidence.

This is a **semantic conformance kernel**, not a new protocol core. It becomes a new core only if it adds its own network operations or independently negotiates grants/tasks rather than binding the operations already defined by A2A, MCP, GNAP, and HTTP.

## Contradictions and hard limits

1. **Approval-state contradiction if interpreted naively:** A2A provides `TASK_STATE_AUTH_REQUIRED`, yet explicitly says the state carries no standardized authorization meaning. Treating the state transition as approval violates A2A’s own rule. [E4]
2. **MCP consent is not an approval primitive:** MCP normatively urges explicit consent while stating protocol-level enforcement is unavailable. A conforming host UX is not portable approval evidence. [E1]
3. **Terminal-state collision:** MCP `completed` includes tool results with `isError:true`; direct mapping to A2A `COMPLETED` would turn some domain failures into apparent success. [E3]
4. **Async completion collision:** an Arazzo AsyncAPI send step completes when the message is sent, not when the deployment is applied. A workflow step’s success cannot be equated with real-world completion without a later status/postcondition step. [E6]
5. **Cancellation hard limit:** A2A and MCP cancellation are best effort. No horizontal profile can promise rollback after an irreversible production change. [E3][E4]
6. **Revocation hard limit:** GNAP revocation can be non-immediate for stateless/offline validation. Strong bounded revocation requires an online check, short validity, or resource-server mechanism outside the generic token format. [E7]
7. **Grant mutation is not action mutation safety:** GNAP correctly re-evaluates modified grants but can keep prior tokens valid. The profile must independently reject any token/approval whose action digest differs from the dispatch. [E7]
8. **Description is not enforcement:** OpenAPI/Arazzo can precisely describe what should be called, but only executor/resource-server verifier rules can ensure that the approved request is what ran. [E5][E6]

## Searched but not found

Within the eight-source normative set, this run did not find:

- a registered cross-reference from A2A `AgentSkill.id` to an OpenAPI operation, Arazzo workflow, or MCP tool;
- a standard absolute, content-pinned identity spanning those four artifacts;
- a standard horizontal deployment authorization-details/GNAP access-object type;
- an approval object that binds principal, delegate, exact action bytes, presentation, policy version, expiry, and single-use consumption;
- cross-protocol idempotency and atomic approval-consumption semantics;
- a normative A2A↔MCP task/status/error map;
- a standard `partially_applied` or `outcome_unknown` state with side-effect inventory;
- cancellation-to-revocation propagation or a guarantee that cancellation prevents effects;
- a portable, signed execution receipt joining action, approval, authority, provider request, side effects, and observed result;
- a conformance suite covering the composed deployment lifecycle;
- an Arazzo source-description type for MCP or A2A.

## Source and freshness note

Eight distinct primary sources support the digest. The AI-standard claims were re-opened from the live MCP and A2A specification surfaces on 2026-09-11, within the requested three-month freshness window; compatibility assertions are statements about those live surfaces as accessed this run. OpenAPI and Arazzo “latest” pages were also re-opened, while the IETF mechanisms are stable RFCs. No secondary summary was used.
