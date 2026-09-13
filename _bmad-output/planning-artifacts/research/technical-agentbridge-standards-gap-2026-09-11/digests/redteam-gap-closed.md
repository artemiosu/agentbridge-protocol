# Red-team digest: has the AgentBridge standards gap already closed?

**Conclusion tested:** “Current standards leave a real cross-protocol semantic/conformance gap for authorized agent actions, so stopping all separate AgentBridge work is premature.”

**Verdict:** **Not overturned.** The contrary case is substantially stronger than a simple “nothing exists” account: stable standards already cover fine-grained action authorization, policy decisions, agent task/result envelopes, and a cross-transport commerce profile; AP2 adds cryptographically bound mandates and signed receipts. UCP + AP2 arguably close the pattern for a bounded commerce/payment slice. But no current official source found in this run defines a stable, general, cross-protocol profile that binds the same exact action through authorization, delegated approval, execution, effect receipt, task/result history, and a shared conformance regime. Separate work is therefore not inherently duplicative, provided it targets that narrow binding/profile layer rather than re-specifying the mature pieces.

## Method and evidence boundary

- Fresh-context adversarial search: the tested conclusion only, with no supporting evidence or project material used.
- Sources restricted to current official specifications, standards-body pages, and official project repositories/releases.
- Eight web calls; six supporting source families retained below. All were accessed **2026-09-11**.
- Normative maturity is explicit below. RFC/final/stable text is not conflated with experimental sections, pre-1.0 specifications, working-group drafts, or roadmaps.
- No claim below relies on model memory or training data.

## Strongest contrary claims

### 1. UCP already provides a stable cross-protocol commerce profile

**Contrary claim.** A separate interoperability layer may be duplicative because Universal Commerce Protocol already defines dated, discoverable capability profiles and transport bindings for the same commerce operations over REST, MCP, A2A, and embedded transports, with an AP2 mandates extension. That is real cross-protocol semantic profiling rather than a mere protocol comparison.

**Primary source:** [UCP 2026-08-25 specification overview](https://ucp.dev/2026-08-25/specification/overview/) and its [A2A checkout binding](https://ucp.dev/latest/specification/checkout-a2a/)

- **Publisher:** Universal Commerce Protocol project
- **Publication/update date:** **2026-08-25** stable specification release
- **Accessed:** 2026-09-11
- **Normative maturity:** **Stable, versioned specification.** The official repository identifies 2026-08-25 as the latest stable production specification.
- **What it covers:** common capability/profile discovery; transport-independent commerce data; explicit MCP and A2A bindings; AP2 extension integration; versioned extension identifiers; idempotent checkout operations.
- **Limit that preserves the tested conclusion:** the semantics are commerce-specific. The evidence does not establish a general authorization-to-effect envelope for arbitrary MCP tools, A2A tasks, or OpenAPI operations, nor a shared conformance suite spanning those ecosystems.
- **Confidence:** **High**
- **Effect:** **Strongly weakens** the general conclusion and **overturns a narrower claim** that no current cross-protocol profile exists for authorized commerce actions. It does **not overturn** the general cross-domain conclusion.

### 2. AP2 already binds user/agent authority to exact transaction state and signed receipts

**Contrary claim.** Agent Payments Protocol v0.2 implements the core authorization/evidence chain that a new protocol might otherwise propose. A merchant-signed checkout JWT is hash-bound into a checkout mandate; payment mandates are bound to that checkout; direct mode uses user-signed closed mandates; autonomous mode uses user-signed open mandates plus agent-signed closed mandates constrained to the user's intent; merchants and processors have normative verification duties; acceptance or rejection produces receipts; and dispute verification checks receipt references against mandate hashes.

**Primary source:** [Agentic Payment Protocol v0.2 specification](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md)

- **Publisher:** Google Agentic Commerce / AP2 project
- **Publication/update date:** **v0.2 on `main`; official repository activity last updated 2026-06-17.** The specification page itself does not provide a dated release tag.
- **Accessed:** 2026-09-11
- **Normative maturity:** **Pre-1.0 project specification.** It uses normative MUST/SHOULD language but is not presented as a final standards-body release.
- **What it covers:** exact checkout/payment semantics; human-present approval; constrained autonomous delegation to an agent key; deterministic verifier duties; signed checkout/payment receipts; cryptographic mandate-to-checkout and receipt-to-mandate binding; non-repudiable dispute evidence.
- **Limit that preserves the tested conclusion:** AP2 explicitly says agent-to-agent delegation is outside the current specification. Commerce API mechanics are outside AP2 and delegated to a commerce protocol. Receipt retention/retrieval is also outside scope, and the specification notes that mandate/disclosure selection is currently ad hoc. Its coverage is payments/checkout, not arbitrary agent actions.
- **Confidence:** **High**
- **Effect:** **Strongest weakening evidence.** It **overturns a commerce/payment-only version** of the conclusion, but **does not overturn** the general cross-protocol claim.

### 3. A2A v1.0 supplies a stable task, result, history, authorization, and extension envelope

**Contrary claim.** A2A v1.0 is no longer merely an early draft. It defines `Task` as the core unit of action, with stable IDs, states, artifacts, messages, context IDs, and history. Its in-task authorization flow normatively requires an authorization-dependent operation to be tracked by a Task and moved to `AUTH_REQUIRED`; a client may negotiate, reject, contact a human/agent/service, or satisfy the request through a negotiated extension. Required, versioned extensions and multiple wire bindings provide a standard carrier for domain semantics.

**Primary source:** [Agent2Agent Protocol Specification v1.0.0](https://a2a-protocol.org/v1.0.0/specification/)

- **Publisher:** A2A Protocol Project (Linux Foundation)
- **Publication/update date:** **2026-03-12** v1.0.0 release
- **Accessed:** 2026-09-11
- **Normative maturity:** **Stable v1.0 release.** The official release announcement calls it production-ready.
- **What it covers:** cross-vendor agent task lifecycle; artifacts/results; multi-turn history; authenticated access controls; in-task authorization state; required/versioned extensions; JSON-RPC, gRPC, and HTTP+JSON bindings; optional signed Agent Cards.
- **Limit that preserves the tested conclusion:** A2A says authorization boundaries are defined by each agent's model, not prescribed by the protocol. Authorization details may be out of band or extension-defined. Agent Card signing is optional. The core does not prescribe an exact-action authorization object, an execution/effect receipt, or a cryptographic authorization-to-artifact binding.
- **Confidence:** **High**
- **Effect:** **Strongly weakens**, but **does not overturn**.

### 4. MCP has an authorization-bound task/result chain, albeit experimentally

**Contrary claim.** MCP's 2025-11-25 Tasks utility substantially reduces the alleged task/result-binding gap. A task wraps an underlying request; terminal `tasks/result` must return exactly the underlying request's result; all related requests, notifications, responses, and dependent elicitations carry the related task ID; and receivers must bind tasks to the requester's authorization context when one exists.

**Primary source:** [MCP 2025-11-25 Tasks specification](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks)

- **Publisher:** Model Context Protocol project
- **Publication/update date:** **2025-11-25** specification release
- **Accessed:** 2026-09-11
- **Normative maturity:** **Explicitly experimental.** The page says the design and behavior may evolve.
- **What it covers:** durable request execution; task status; final result retrieval; task-related message/elicitation correlation; cancellation; authorization-context isolation.
- **Limit that preserves the tested conclusion:** the task ID correlates protocol messages but is not a cryptographic action digest or effect receipt. Authorization remains transport/resource oriented, and the task utility is experimental rather than a stable cross-protocol profile.
- **Confidence:** **High**
- **Effect:** **Moderately weakens**, but **does not overturn**.

### 5. AuthZEN Final standardizes exact-action authorization decisions across policy engines

**Contrary claim.** OpenID AuthZEN Authorization API 1.0 already provides a vendor-neutral PDP/PEP boundary in which an authorization request carries a subject, resource, action, and context; the Action has a required name and can carry action parameters; and the PDP returns an allow/deny Decision for the PEP to enforce. This directly closes much of the “exact action semantics” and cross-vendor authorization-decision gap.

**Primary source:** [OpenID AuthZEN Authorization API 1.0 Final](https://openid.net/specs/authorization-api-1_0-final.html); current status and follow-on drafts are listed on the [AuthZEN specifications page](https://openid.net/wg/authzen/specifications/)

- **Publisher:** OpenID Foundation, AuthZEN Working Group
- **Publication/update date:** **2026-01-11** final specification
- **Accessed:** 2026-09-11
- **Normative maturity:** **Final** for Authorization API 1.0. Follow-on obligations and asynchronous access-request/approval lifecycle work is **working-group draft**, not final.
- **What it covers:** interoperable subject/resource/action/context requests; parameterized actions; boolean decisions; PDP discovery and capabilities. The official WG page also describes draft obligations and a task-handled approval/re-evaluation flow, evidence that adjacent lifecycle coverage is actively converging.
- **Limit that preserves the tested conclusion:** the final standard decides whether an operation may proceed; it does not carry user consent evidence, invoke the operation, produce an effect receipt, or bind the decision to an A2A/MCP/OpenAPI task/result. The most relevant obligation/approval lifecycle additions are drafts with no published finalization date found.
- **Confidence:** **High** for the final API; **Medium** for “imminent closure” because the follow-on work is draft and undated.
- **Effect:** **Strongly weakens** the authorization-semantics part; **does not overturn** the end-to-end conclusion.

### 6. OAuth RAR already expresses fine-grained transaction intent in a stable token-grant flow

**Contrary claim.** OAuth 2.0 Rich Authorization Requests replaces coarse scopes with structured `authorization_details`. The Standards Track RFC expressly supports fine-grained requests such as transferring a stated amount to a named merchant or applying different actions to specific resources, with common fields such as `type`, `locations`, and `actions`. The authorization server and resource server jointly enforce the user's consent.

**Primary source:** [RFC 9396 — OAuth 2.0 Rich Authorization Requests](https://www.ietf.org/rfc/rfc9396.pdf)

- **Publisher:** Internet Engineering Task Force (IETF)
- **Publication/update date:** **May 2023**
- **Accessed:** 2026-09-11
- **Normative maturity:** **Standards Track RFC.** Stable normative text.
- **What it covers:** machine-readable, fine-grained authorization requirements; typed action/resource/transaction details; consent carried into an OAuth authorization grant.
- **Limit that preserves the tested conclusion:** authorization-detail types and their semantics remain API/domain defined. RAR does not define an agent delegation chain, task ID, execution record, effect receipt, result binding, or mapping to MCP/A2A/Arazzo.
- **Confidence:** **High**
- **Effect:** **Strongly weakens** any claim that exact-action authorization itself is unsolved; **does not overturn** the cross-protocol lifecycle claim.

## Coverage test

| Required property | Best current official coverage found | Stable? | Remaining gap |
|---|---|---:|---|
| Exact-action semantics | OAuth RAR structured details; AuthZEN action + properties; UCP checkout schemas | Yes | No common canonical action identity/digest spanning arbitrary MCP, A2A, and API operations |
| Delegated approval | AP2 user-signed open/closed mandates; A2A `AUTH_REQUIRED`; AuthZEN lifecycle draft | Mixed | AP2 excludes agent-to-agent mandate delegation; general approval evidence is not standardized across protocols |
| Execution/effect receipt | AP2 signed checkout/payment receipts | Pre-1.0 and domain-specific | No general receipt vocabulary or verifier rules for arbitrary tool/API/agent effects |
| Task/result/audit binding | A2A Task/artifacts/history; MCP related-task + exact underlying result; AP2 hash-linked dispute evidence | Mixed | A2A/MCP records are not cryptographically bound to the authorizing action; AP2 binding is commerce-only |
| Cross-protocol profile | UCP profiles commerce capabilities over REST/MCP/A2A and integrates AP2 | Yes, domain-specific | No general profile joining OAuth/AuthZEN authorization, MCP/A2A execution, and receipt semantics |
| Conformance | Each mature standard has its own normative requirements and implementation ecosystem | Fragmented | No shared conformance target found for authorization → approval → execution → effect across the protocol boundary |

## Why the contrary case falls short of overturning the conclusion

The evidence shows a **composition opportunity**, not a completed general standard. A plausible stack can be assembled from RAR for fine-grained grant requests, AuthZEN for runtime decisions, A2A or MCP for execution state, and AP2-like signed receipts. UCP proves such composition can be standardized successfully in one vertical. What was not found is a normative rule that makes those artifacts share the same action identifier, canonical semantics, cryptographic binding, verifier behavior, and cross-protocol test vectors outside commerce.

This materially narrows defensible AgentBridge scope. Work that reinvents task envelopes, OAuth detail objects, PDP/PEP APIs, generic workflow descriptions, or payment mandates would be duplicative. Work limited to a thin, domain-neutral binding/profile and conformance layer remains differentiated by the evidence reviewed.

## Searched but not found

- **OpenAPI/Arazzo:** The current Arazzo 1.1.0 specification defines multi-API workflows, exact operation references, inputs, step dependencies, success criteria, outputs, and correlation IDs. It does not define delegated user approval, authorization evidence, tamper-evident execution receipts, or audit binding. It even distinguishes message send completion from broker acknowledgment/delivery confirmation. No official Arazzo cross-profile with MCP/A2A/AuthZEN/AP2 was found.
- **MCP authorization and elicitation:** MCP has a strong OAuth-based HTTP authorization profile, scope challenges, and user-interaction mechanisms. No normative exact-action consent object or signed effect receipt was found in the stable core; Tasks remains experimental.
- **A2A conformance/TCK:** A2A v1.0 has stable protocol requirements and official validation/TCK activity, but no official cross-standard TCK was found that validates an OAuth/AuthZEN authorization against the exact A2A task, resulting artifact, and external effect.
- **ACP:** Official protocol/repository materials were located for agentic commerce and payment handlers, but no primary normative artifact surfaced in the bounded search that generalized cryptographic authorization-to-execution receipt binding beyond commerce or supplied cross-MCP/A2A conformance.
- **FIDO / W3C Secure Payment Confirmation:** Current official work can produce cryptographic evidence that a user confirmed displayed payment details. No general agent delegation, arbitrary-action receipt, or MCP/A2A task binding was found.
- **IETF OAuth/OpenID agent initiatives:** The official OAuth working-group index contains several 2026 Internet-Drafts on AI-agent authorization, delegation chains, transaction tokens, scope aggregation, and actor profiles. This is strong evidence that the gap is recognized and active work is converging, but not that it is already or imminently closed: the relevant items found were drafts, some individual rather than adopted working-group documents, and no unified final profile or committed completion date was found.

## Bottom line

**Best good-faith argument that the conclusion is wrong:** UCP + AP2 already demonstrate a cross-transport, authorization-to-receipt standard for agentic commerce, and the remaining generic pieces exist separately in stable A2A, AuthZEN, and OAuth standards; a new protocol could be unnecessary if a profile of those standards is enough.

**Red-team judgment:** that argument is persuasive against a broad new protocol and against any commerce-first reinvention, but it is not enough to justify stopping all separate AgentBridge work. The official record still shows a general cross-protocol semantic/conformance seam rather than one end-to-end, stable solution.
