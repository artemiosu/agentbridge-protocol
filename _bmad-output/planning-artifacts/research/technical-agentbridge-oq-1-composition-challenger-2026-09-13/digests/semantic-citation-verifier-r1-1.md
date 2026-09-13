# Fresh-context semantic citation verification — `research.md` r1.1

Date/access date: **2026-09-13**
Target: `../research.md`
Method: every inline reference `[1]`–`[31]` was checked against the exact cited target where retrievable and against fresh official primary material for version/compatibility claims. Prior digests were not used as evidence.

## Verdict

**Semantic citation gate: FAIL pending correction.**

The report has one overturned immutable-version claim, one materially ambiguous component-version label, two clear source-to-claim mismatches, eight partially supported citation targets, and one exact target that could not be retrieved in this run. The central selection conclusion is **not overturned**: the decisive COAZ-MCP/MCP 2026-07-28 incompatibility, UCP/AP2 relationship, GNAP alternative, ACP beta status, and UCP conformance lag all survive fresh verification.

Disposition counts by reference target:

- **Verified:** 20 — `[2] [4] [5] [7] [9] [10] [11] [13] [15] [16] [17] [22] [23] [24] [25] [26] [27] [28] [30] [31]`
- **Partially supported:** 8 — `[1] [6] [8] [14] [18] [19] [20] [21]`
- **Citation mismatch:** 2 — `[3] [29]`
- **Exact target unverified:** 1 — `[12]` (the official repository independently confirms the underlying draft and its scope)

## Material mismatches and distortions

### 1. Overturned: A2A v1.0.0 is pinned to the v1.0.1 commit

The pinning table says:

> `A2A | 1.0.x; tag 1.0.0 commit 3303592588e388e62e0f69f701af531d2f4e3991`

This is false. The official A2A releases show:

- `v1.0.0` → commit [`173695755607e884aa9acf8ce4feed90e32727a1`](https://github.com/a2aproject/A2A/commit/173695755607e884aa9acf8ce4feed90e32727a1), published 2026-03-12, publisher A2A/Linux Foundation, accessed 2026-09-13.
- `v1.0.1` → commit [`3303592588e388e62e0f69f701af531d2f4e3991`](https://github.com/a2aproject/A2A/commit/3303592588e388e62e0f69f701af531d2f4e3991), published 2026-05-26/released 2026-05-28, publisher A2A/Linux Foundation, accessed 2026-09-13.

This must be corrected before the table can be treated as an executable baseline. It does not invalidate the choice of the A2A `1.0.x` line.

### 2. Disputed/ambiguous: “SLIM v2.3.2” is a component release, not a demonstrated suite release

The pinning table labels commit `16151eefd31195dcc600b359c2d401926e871d24` as “SLIM v2.3.2.” The official release list identifies the corresponding release as **`slim-version v2.3.2`**, while other SLIM components at that commit have separate versions (for example `slim-tracing v0.4.20`, `slim-proto v0.6.1`, `slim-mls v0.3.10`). The project changelog's last monolithic release entry is `v2.0.0`.

Evidence: [AGNTCY SLIM releases](https://github.com/agntcy/slim/releases), publisher AGNTCY/Linux Foundation, release 2026-09-01, accessed 2026-09-13; [SLIM changelog](https://github.com/agntcy/slim/blob/main/CHANGELOG.md), publisher AGNTCY/Linux Foundation, current, accessed 2026-09-13.

The commit exists and is useful as a repository snapshot, but the report should not imply that `v2.3.2` is a single compatible whole-suite version without a component BOM.

### 3. Citation mismatch: `[3]` does not support “official SDK and conformance activity”

At `research.md:76`, `[2][3]` is attached to the sentence asserting official MCP SDK and conformance activity. `[3]` is only the MCP governance/licensing file; it says nothing about SDKs or conformance. `[2]` establishes protocol scope and version, not a conformance program.

Fresh official material does independently support **SDK activity**: the MCP release announcement says the 2026-07-28 revision shipped with updated Tier 1 SDKs. No cited source establishes the report's broader **conformance activity** claim.

Evidence: [MCP 2026-07-28 release announcement](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/blog/content/posts/2026-07-28-spec-ga/index.md), publisher MCP/Linux Foundation, published 2026-07-28, accessed 2026-09-13; cited [MCP governance](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md), current, accessed 2026-09-13.

### 4. Citation mismatch: `[29]` proves final status, not signal semantics

At `research.md:98`, `[29]` is attached to “SSF/CAEP communicates security-event changes and can reduce stale-authority windows; a signal cannot create permission.” The cited announcement only records that SSF, CAEP, and RISC became Final Specifications. It does not describe CAEP event semantics or the no-grant inference.

The underlying claim is substantively sound when cited to the actual CAEP specification: CAEP defines continuous updates by which receivers can **attenuate** access and includes session-revoked/token-claims-change events. The “cannot create permission” conclusion is an architectural inference, not text supported by the announcement.

Evidence: cited [OpenID final-specifications announcement](https://openid.net/three-shared-signals-final-specifications-approved/), publisher OpenID Foundation, published 2025-09-02, accessed 2026-09-13; corrective primary source [OpenID CAEP 1.0 Final](https://openid.net/specs/openid-caep-1_0-final.html), publisher OpenID Foundation, published 2025-08-29, accessed 2026-09-13.

## Citation-by-citation audit

| Ref | Result | Does the inline source support its attached claim? |
| --- | --- | --- |
| `[1]` | **Partial** | Confirms A2A v1.0, 2026-03-12, stable/production-ready status, Linux Foundation publication, discovery/capability communication, task delegation, multi-bindings, streaming/polling/webhooks. The cited release article does not itself enumerate cancellation or explicitly state the listed identity-issuance/credential-acquisition/authorization-policy exclusions. |
| `[2]` | **Verified** | The 2026-07-28 specification and its linked changelog establish MCP's tool/context boundary and the required `server/discover`, new `subscriptions/listen`, and removal of legacy methods used in the COAZ comparison. |
| `[3]` | **Mismatch** | Establishes LF Projects governance and Apache/CC licensing only. It does not support the inline SDK/conformance assertion. |
| `[4]` | **Verified** | OpenAPI 3.2.1, published 2026-09-10, explicitly defines a language-agnostic interface description for HTTP APIs. Treating execution/enforcement as outside that descriptive role is a fair scope inference. |
| `[5]` | **Verified** | Arazzo 1.1.0, published 2026-05-17, defines sequences of calls and dependencies. It is a description format, not a runtime executor; the report's limitation is a fair scope inference. |
| `[6]` | **Partial** | Confirms UCP `v2026-08-25`, commit `cd78fb3…`, and broad commerce evolution including location, checkout/cart actions, payments, identity/consent, and multi-vertical work. A release note alone does not exhaustively establish every listed existing capability or prove the comparative superlative “broadest.” |
| `[7]` | **Verified** | The public conformance README's sample input targets `2026-04-08`, materially older than selected `2026-08-25`; the report correctly treats compatibility as unproven rather than assumed. |
| `[8]` | **Partial** | Confirms AP2 `v0.2.0`, commit `b4587ac…`, and focus on Human-Not-Present flows. It does not by itself support the full signed-mandate/trust-surface description or the long exclusion list. |
| `[9]` | **Verified** | Explicitly says UCP is compatible with AP2, calls AP2 the trust layer, defines signed checkout/payment mandates, and says AP2 depends on UCP Checkout. This supports the asymmetric composition claim. |
| `[10]` | **Verified** | Confirms AuthZEN Authorization API 1.0 Final (2026-01-11), the PEP/PDP decision seam, and that actual `context` semantics/format remain an implementation concern outside the specification. |
| `[11]` | **Verified** | Confirms AuthZEN Access Request and Approval Profile Draft 1 (2026-09-10) and its requestable-denial, asynchronous task, approval, and re-evaluation scope. |
| `[12]` | **Exact target unverified** | The generated HTML target could not be retrieved in this run. The official AuthZEN repository independently labels the OAuth Token Exchange binding a draft and says it covers delegation, impersonation, identity chaining, ID-JAG, and Transaction Tokens. Underlying claim supported; exact citation-path check inconclusive. |
| `[13]` | **Verified** | Official ACP repository says the specification is currently beta and describes buyer/agent/business purchase flows, cart/feed/orders/authentication/payment-handler snapshots. “Overlaps UCP” is a reasonable comparative inference. |
| `[14]` | **Partial** | Defines x402 Protocol Version 2 for payment-gated external resources/APIs/content/data and explicitly excludes budget management and session handling. Absence of cart/fulfillment/refunds/booking/portable-consent semantics is a reasonable scope inference, but not an explicit normative exclusion list. A mutable `main` URL does not provide an immutable stable pin, so the report's caution is appropriate. |
| `[15]` | **Verified** | Visa's TAP page says agent providers are certified/onboarded and approved by payment schemes, and Visa hosts keys for its implementation. This supports treating TAP as a scheme-controlled optional edge rather than an independently operable mandatory trust root. |
| `[16]` | **Verified** | RFC 9635 specifies the stateful/interactive GNAP grant, rights, continuation, token-management, and key-proof model and explicitly says GNAP is not designed to be directly OAuth 2.0 compatible. |
| `[17]` | **Verified** | RFC 9767 defines the loose-coupling AS–RS APIs, token introspection/access-rights model, downstream token derivation, and RS-facing discovery relationship. |
| `[18]` | **Partial** | The SUNET repository explicitly lists missing key-bound access tokens, multiple token requests, rotation, RFC 9767 introspection, and other features. A single implementation repository cannot prove the ecosystem-wide negative “no complete conformance program”; that remains a search-result statement. |
| `[19]` | **Partial** | The official Directory architecture supports the DIR scope/deployment part. It cannot by itself prove the absence of an authoritative cross-DIR/SLIM/OASF/Identity suite manifest. |
| `[20]` | **Partial** | The official SLIM changelog/releases support SLIM component scope and versions. They do not prove a compatible AGNTCY-wide suite BOM; component versioning also makes the report's singular “SLIM v2.3.2” label ambiguous. |
| `[21]` | **Partial** | Confirms ANP 1.1 coverage of identity, WNS naming, discovery, messaging, E2EE, federation and that the meta-protocol remains draft. It does not directly establish the global negative “no public immutable BOM” or absent cross-layer mappings. |
| `[22]` | **Verified** | Archived BeeAI ACP repository explicitly says ACP is now part of A2A under the Linux Foundation and provides a migration guide. |
| `[23]` | **Verified** | W3C WebAgents charter explicitly says no specifications will be produced and there are no concrete software/test-suite plans under the current charter. |
| `[24]` | **Verified** | IETF Datatracker marks the document an active individual Internet-Draft, not IETF-endorsed and with no formal standards-process standing. |
| `[25]` | **Verified** | RFC 9396 defines structured fine-grained `authorization_details` and leaves type semantics and comparison of arbitrary authorization details deployment/API-specific. |
| `[26]` | **Verified** | RFC 8693 defines OAuth Token Exchange plus `act`/`may_act` delegation and actor semantics. It does not define a universal authority/consent algebra; that limitation is a fair scope inference. |
| `[27]` | **Verified** | RFC 9449 is specifically an OAuth sender-constraint/proof-of-possession mechanism and explicitly is not client authentication. The report does not over-credit it. |
| `[28]` | **Verified** | COAZ-MCP Draft 1 maps `logging/setLevel`, `resources/subscribe`, `resources/unsubscribe`, `tasks/result`, `tasks/list`, `ping`, and server-initiated requests; MCP 2026-07-28 removes/replaces those. COAZ lacks `server/discover` and `subscriptions/listen`, and mandates fail-closed denial for unknown methods. “Incompatible unchanged” is directly supported. |
| `[29]` | **Mismatch** | Confirms only Final Specification approval/status. It does not support the attached CAEP event-semantics/no-grant claim; the CAEP Final specification does. |
| `[30]` | **Verified** | RFC 9943 defines SCITT signed-statement transparency and explicitly warns that issuers can make false statements and registration proves only issuer production, supporting “provenance, not truth.” |
| `[31]` | **Verified** | RFC 9942 defines signed receipts/proofs about VDS states (inclusion, consistency, disclosure, non-inclusion). It does not assert authorization, real-world effects, or dispute resolution; the report's limitation is accurate. |

## Compatibility findings that survive unchanged

1. **COAZ-MCP Draft 1 cannot be used unchanged with MCP 2026-07-28.** MCP's official changelog removes `ping` and `logging/setLevel`, replaces `resources/subscribe`/`resources/unsubscribe` with `subscriptions/listen`, redesigns Tasks, and replaces server-initiated requests with MRTR; it also requires `server/discover`. COAZ maps the old methods and rejects unknown ones. Sources: [MCP changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog), publisher MCP/Linux Foundation, published 2026-07-28, accessed 2026-09-13; [COAZ-MCP Draft 1](https://openid.github.io/authzen/authzen-coaz-mcp-binding-1_0.html), publisher OpenID Foundation/AuthZEN, published 2026-02-13, accessed 2026-09-13.
2. **UCP/AP2 composition is explicit and asymmetric.** UCP provides Checkout and AP2 is an optional trust/mandate extension depending on it. Source: [UCP and AP2](https://ucp.dev/documentation/ucp-and-ap2/), publisher UCP Authors, current, accessed 2026-09-13.
3. **UCP conformance lag is real.** The selected UCP release is `v2026-08-25`, while the public conformance example targets `2026-04-08`. Sources: [UCP release](https://github.com/Universal-Commerce-Protocol/ucp/releases/tag/v2026-08-25), publisher UCP project, published 2026-08-25, accessed 2026-09-13; [UCP conformance](https://github.com/Universal-Commerce-Protocol/conformance), publisher UCP project, current, accessed 2026-09-13.
4. **GNAP implementation incompleteness is real.** SUNET explicitly lists missing key-bound tokens, multiple token requests, token rotation and RFC 9767 introspection. Source: [SUNET GNAP implementation](https://github.com/SUNET/sunet-auth-server), publisher SUNET, current, accessed 2026-09-13.
5. **ACP is still beta despite a dated stable snapshot.** The official repository simultaneously labels the specification beta and its `2026-04-17` directory “Latest Stable”; the report correctly keeps the beta caveat. Source: [ACP repository](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol), publisher ACP/OpenAI/Stripe, current, accessed 2026-09-13.

## Required corrections before treating the report as citation-clean

1. Replace the A2A v1.0.0 SHA with `173695755607e884aa9acf8ce4feed90e32727a1`, or relabel the existing SHA as v1.0.1.
2. Clarify the AGNTCY pin as repository commit plus component BOM; do not call `slim-version v2.3.2` the whole SLIM suite version.
3. Replace `[3]` at the SDK/conformance claim with an official SDK/release source and either add primary evidence for a conformance program or remove/downgrade “conformance activity.”
4. Cite the CAEP Final specification directly for signal semantics; keep `[29]` only for Final-status approval.
5. Add specification-level citations or explicitly label the unsupported portions as scope inferences for A2A exclusions, UCP's full capability list, AP2 exclusions, x402 exclusions, and the ANP/AGNTCY no-BOM findings.
6. Recheck the exact generated HTML URL for `[12]`; if it is not reliably reachable, cite the official repository source instead.

## Final assessment

The report is **substantively resilient but not citation-clean**. None of the verified mismatches overturns the C1/C2 challenger selection. The immutable A2A pin error is execution-blocking for a reproducible baseline, and the MCP conformance claim must remain unverified until properly sourced. Once those are corrected and the partial/negative claims are explicitly marked as inferences or given stronger sources, the report should pass a semantic citation gate.

## Re-review after corrections — 2026-09-13

**Verdict: FAIL — one citation blocker remains.**

The A2A pins, SLIM component-snapshot wording, MCP SDK/conformance split, CAEP source and inference label, AP2/A2A/UCP support, x402 scope-inference label, and bounded-search negative claims now pass semantic review. No remaining version or compatibility distortion was found in those corrected passages.

### Remaining blocker: `[37]` does not support the Token Exchange profile claim

At `research.md:118`, the sentence “New drafts for approval and token exchange reduce local glue, while remaining draft-level.[11][37]” uses `[37]` for the Token Exchange portion. The linked OpenID Foundation announcement names and describes only the **Access Request and Approval Profile (AARP)** and **COAZ for MCP** as approved Working Group Drafts; it does not mention an AuthZEN OAuth Token Exchange profile. `[11]` supports AARP only. Therefore the Token Exchange draft/status claim still lacks a semantically matching inline source.

The bibliography metadata for `[37]` also says `2026-09`, but the linked page states **Published June 15, 2026**.

Evidence: [OpenID Foundation announcement](https://openid.net/openid-foundation-advances-authorization-for-the-agent-era-with-new-authzen-working-group-drafts/), publisher OpenID Foundation, published 2026-06-15, accessed 2026-09-13.

Required correction: cite the official AuthZEN Token Exchange binding/specification or repository target directly for its draft status and scope, and correct `[37]`'s source date to `2026-06`. After that single correction, the semantic citation gate should pass.

## Final point re-check of `[37]` — 2026-09-13

**Verdict: PASS.**

The corrected `[37]` resolves the last blocker. Its exact target is the official OpenID AuthZEN Working Group publication **“AuthZEN Binding for OAuth 2.0 Token Exchange — Draft 1,”** published **2026-09-03**. The document explicitly binds OAuth 2.0 Token Exchange flows to AuthZEN evaluation requests and covers delegation, impersonation, identity chaining, ID-JAG, and Transaction Tokens. It therefore semantically supports `research.md:118` and the bibliography metadata `2026-09`.

Evidence: [AuthZEN Binding for OAuth 2.0 Token Exchange — Draft 1](https://openid.github.io/authzen/authzen-oauth-token-exchange-1_0.html), publisher OpenID Foundation/AuthZEN Working Group, published 2026-09-03, accessed 2026-09-13.

**Final semantic citation gate: PASS. No remaining blockers.**

## Russian-translation regression check — 2026-09-13

**Verdict: PASS.**

The Russian translation preserves the previously verified citation semantics, version/status pins, scope qualifications, inference labels, and compatibility conclusions. In particular, the corrected A2A and SLIM pins, MCP SDK/conformance distinction, COAZ-MCP incompatibility, UCP/AP2 asymmetry, CAEP attenuation semantics, x402 scope inference, and AuthZEN Token Exchange Draft 1 status remain materially unchanged. No translation-induced semantic or version/compatibility blocker was found.
