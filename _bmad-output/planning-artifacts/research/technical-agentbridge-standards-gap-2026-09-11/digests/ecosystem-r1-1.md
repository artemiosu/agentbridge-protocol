# Ecosystem, governance, conformance, and five-year-regret digest — round 1

**Decision served:** choose among (A) a new domain-independent AgentBridge protocol core, (B) an interoperability/conformance profile over existing standards, or (C) no separate protocol and contribute upstream.

**Research date / access date:** 2026-09-11. **Method:** primary-source web research only; eight retained sources. Live repository pages were re-opened on the access date. Where GitHub did not expose a file's publication or last-updated date, that limitation is explicit. Repository artifacts are treated as stronger evidence than ecosystem announcements; project-maintained lists of third-party tools establish that artifacts are linked, not that they interoperate.

## Executive finding

The retrieved evidence does **not** establish an unoccupied need for another domain-independent wire-protocol core. MCP has a Linux Foundation home, an Apache-2.0 contribution policy, dated protocol-version support, and an executable client/server conformance framework. A2A is also Linux Foundation-governed, explicitly positions itself as complementary to MCP, and has reached a stable 1.x release line. Arazzo already provides a foundation-governed, implementation-independent description language for deterministic API workflows. ACP has chosen consolidation into A2A instead of continued standalone competition. In commerce, UCP explicitly composes AP2-style authorization/security patterns rather than attempting to replace every layer.

This makes **B, a thin, versioned interoperability and conformance profile, the best-supported hypothesis for further validation**, not a final selection. A new core (A) would need evidence of irreducible semantics that cannot be expressed as MCP/A2A extensions, OpenAPI/Arazzo workflows, or domain profiles. Option C remains rational if cross-implementation tests fail to reveal a persistent cross-standard gap.

**Evidence:** [S1](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md), [S2](https://github.com/modelcontextprotocol/conformance), [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md), [S5](https://github.com/OAI/Arazzo-Specification), [S6](https://github.com/Universal-Commerce-Protocol/ucp), [S7](https://github.com/i-am-bee/acp). Publishers: MCP/LF Projects; A2A Project/Linux Foundation; OpenAPI Initiative/Linux Foundation; UCP; BeeAI/LF AI & Data. Dates: live main-branch pages, updated dates not exposed; A2A stable-release evidence is separately dated in S4. Accessed 2026-09-11. Confidence: **medium-high**. Class: **decision inference / convergence**.

## Initiative-by-initiative evidence

### MCP — Model Context Protocol

- **Governance and change control.** MCP is established as a Series of LF Projects, LLC. Governance changes also require LF Projects approval. The public policy retains copyright with contributors, places new code and specification contributions and outbound code/specifications under Apache-2.0, and places non-spec documentation under CC-BY-4.0. Core Maintainers may exceptionally approve another open license. This is foundation-based change control, not unilateral ownership by the protocol's original vendor.
  - **Evidence:** [S1](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md). Publisher: Model Context Protocol / LF Projects, LLC. Date: live main-branch governance file; updated date not exposed. Accessed 2026-09-11. Confidence: **high**. Class: **governance / license / IP**.

- **Conformance and vitality.** The official conformance repository runs executable scenarios against arbitrary MCP clients or servers, captures protocol traffic, emits checks, supports version selection including `2025-11-25` and `2026-07-28`, and supplies a reusable CI action identified as `v0.1.11`. This is observable conformance infrastructure, stronger than an announcement. It does not itself show that named independent products passed.
  - **Evidence:** [S2](https://github.com/modelcontextprotocol/conformance). Publisher: Model Context Protocol project. Date: live repository; current README supports the 2026-07-28 protocol revision and displays conformance action v0.1.11; page-level updated date not exposed. Accessed 2026-09-11. Confidence: **high** for suite existence and version coverage; **low** for ecosystem pass rates. Class: **conformance / release vitality / implementation independence**.

- **Independence caveat.** A generic client/server harness lowers the cost of independent implementations, but no public certification registry, interoperability-event report, or authoritative pass matrix was found in the retained sources. Therefore, the existence of many mutually interoperable implementations is **not proven here**.
  - **Evidence:** [S2](https://github.com/modelcontextprotocol/conformance). Publisher: Model Context Protocol project. Date: live repository; updated date not exposed. Accessed 2026-09-11. Confidence: **high** as an evidence-limit statement. Class: **searched-but-not-established / interoperability**.

### A2A — Agent2Agent Protocol

- **Governance, backing, license, and relationship to MCP.** A2A's official documentation says Google donated the protocol to the Linux Foundation; a Technical Steering Committee represents AWS, Cisco, Google, IBM Research, Microsoft, Salesforce, SAP, and ServiceNow. The same page states Apache-2.0 licensing and defines A2A as agent-to-agent communication, complementary to MCP's agent-to-tool role. This is foundation-based, multi-company technical governance, although the retained overview does not expose the detailed voting/change-approval procedure.
  - **Evidence:** [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md). Publisher: A2A Project / Linux Foundation. Date: live main-branch documentation; updated date not exposed. Accessed 2026-09-11. Confidence: **high**. Class: **governance / license / backing / official composition**.

- **Release vitality.** The observable release line reached `1.0.0` on 2026-03-12 and `1.0.1` on 2026-05-26. The 1.0 release contains substantial specification changes and separates the application-protocol definition from transport mappings; 1.0.1 contains binding/spec fixes. This is real versioned artifact activity rather than a roadmap promise.
  - **Evidence:** [S4](https://github.com/a2aproject/A2A/releases). Publisher: A2A Project / Linux Foundation. Published 2026-03-12 and 2026-05-26. Accessed 2026-09-11. Confidence: **high**. Class: **release/version vitality**.

- **Independence/conformance caveat.** Multi-company TSC participation and protocol bindings reduce single-vendor dependency, but they do not prove interoperable independent implementations. This pass found official references to SDKs elsewhere in search, but retained no public plugfest results, certification registry, or cross-vendor pass report; those claims remain unverified.
  - **Evidence:** [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md), [S4](https://github.com/a2aproject/A2A/releases). Publisher: A2A Project / Linux Foundation. Dates: live documentation; releases 2026-03-12 and 2026-05-26. Accessed 2026-09-11. Confidence: **medium-high**. Class: **implementation independence / evidence limit**.

### OpenAPI / Arazzo

- **Governance, scope, license, and vitality.** Arazzo is a community-driven specification within the OpenAPI Initiative, a Linux Foundation Collaborative Project, and the repository is Apache-2.0 licensed. Its current version is `1.1.0`. It describes deterministic sequences and dependencies across APIs described by OpenAPI or AsyncAPI; it does not define a general agent messaging wire protocol.
  - **Evidence:** [S5](https://github.com/OAI/Arazzo-Specification). Publisher: OpenAPI Initiative / Linux Foundation. Date: live repository identifying current version 1.1.0; release date not exposed on the retained page. Accessed 2026-09-11. Confidence: **high**. Class: **governance / license / scope / version vitality**.

- **Independent implementation evidence.** The specification repository links separately authored editors, generators, validators, parsers, resolvers, and workflow runners, including offerings from Jentic, Symplr, Specmatic, Redocly, Stoplight/Spectral, and others. These are observable linked artifacts and demonstrate implementation diversity better than partner logos. They are **not** proof of normative cross-tool conformance because the page does not provide a certified results matrix.
  - **Evidence:** [S5](https://github.com/OAI/Arazzo-Specification). Publisher: OpenAPI Initiative / Linux Foundation. Date: live repository; updated date not exposed. Accessed 2026-09-11. Confidence: **medium-high** for independent tooling existence; **low** for interoperability. Class: **implementation independence / ecosystem maturity / conformance gap**.

- **Overlap implication.** A new AgentBridge core that defines deterministic API workflow sequencing would duplicate Arazzo's stated role. A profile can instead bind agent-facing semantics to Arazzo workflows while leaving HTTP/API descriptions in OpenAPI. That is an architectural inference, not an official composition claim.
  - **Evidence:** [S5](https://github.com/OAI/Arazzo-Specification). Publisher: OpenAPI Initiative / Linux Foundation. Date: live repository. Accessed 2026-09-11. Confidence: **medium-high**. Class: **duplication risk / inference**.

### UCP — Universal Commerce Protocol

- **Scope, licensing, artifacts, and release vitality.** UCP defines modular commerce capabilities and extensions, is Apache-2.0 licensed, and exposes specification/docs plus official SDK, samples, schema tooling, and conformance-test links. The repository's latest displayed release is `v2026-04-08`, published 2026-04-09. These are observable project artifacts, but they are mostly under the same project organization and do not by themselves establish independent implementations.
  - **Evidence:** [S6](https://github.com/Universal-Commerce-Protocol/ucp). Publisher: Universal Commerce Protocol project. Published 2026-04-09 for the displayed release; live repository accessed 2026-09-11. Confidence: **high** for artifacts/license/version; **low-medium** for independent adoption. Class: **scope / license / conformance artifact / release vitality**.

- **Governance limit.** The retained root page points to centralized maintainers and welcomes community contributions, but does not identify a foundation/standards body or expose a ratification/change-control charter. Accordingly, classify UCP as project-governed with multi-maintainer signals; do **not** claim foundation neutrality from this source set. A search surfaced Tech/Governing Council material, but it was not retained inside the eight-source cap and should be verified in the next round.
  - **Evidence:** [S6](https://github.com/Universal-Commerce-Protocol/ucp). Publisher: Universal Commerce Protocol project. Date: live repository; updated date not exposed. Accessed 2026-09-11. Confidence: **high** about what the retained page states; **medium** about the provisional classification. Class: **governance / evidence limit**.

- **Official composition.** UCP explicitly describes support for AP2 mandates and verifiable credentials as advanced security patterns. That is direct evidence of a layered design: commerce capabilities in UCP, authorization/security receipts in AP2.
  - **Evidence:** [S6](https://github.com/Universal-Commerce-Protocol/ucp). Publisher: Universal Commerce Protocol project. Date: live repository; updated date not exposed. Accessed 2026-09-11. Confidence: **high**. Class: **official composition / convergence**.

### ACP — Agent Communication Protocol (BeeAI), not the unrelated Agentic Commerce Protocol acronym

- **Governance, license, consolidation, and vitality.** ACP is Apache-2.0 licensed, was developed by BeeAI contributors within LF AI & Data, and its repository now says it is part of A2A under the Linux Foundation with a migration guide. The standalone repository contains Python and TypeScript implementations/examples; the latest displayed standalone release is `v1.0.3` dated 2025-08-21. Consolidation into A2A is stronger evidence against another competing general agent-communication core than a mere partnership announcement.
  - **Evidence:** [S7](https://github.com/i-am-bee/acp). Publisher: BeeAI contributors / Linux Foundation AI & Data. Published 2025-08-21 for the latest displayed standalone release; live consolidation notice accessed 2026-09-11. Confidence: **high**. Class: **governance / license / implementation artifacts / consolidation / vitality**.

- **Independence caveat.** The Python/TypeScript code proves more than a paper specification, but both live in the originating project repository. No retained evidence shows separately governed implementations or public interoperability results. ACP should be treated as a migration/subsumption case, not as a currently competing independent standard.
  - **Evidence:** [S7](https://github.com/i-am-bee/acp). Publisher: BeeAI contributors / Linux Foundation AI & Data. Date: live repository; latest displayed release 2025-08-21. Accessed 2026-09-11. Confidence: **medium-high**. Class: **implementation independence / ecosystem status**.

### AP2 — Agent Payments Protocol

- **Scope and maturity.** The official Google Agentic Commerce repository is Apache-2.0 licensed and contains specification/docs, schemas, a Python SDK, and reference/demo scenarios in Python, Go, and Android. It also states that a PyPI package will be published later and displays no GitHub release. This is a working, vendor-hosted reference implementation ecosystem, but not evidence of independently maintained implementations or release-grade distribution.
  - **Evidence:** [S8](https://github.com/google-agentic-commerce/AP2). Publisher: Google Agentic Commerce. Date: live repository; updated date not exposed and no tagged release displayed. Accessed 2026-09-11. Confidence: **high** for repository artifacts and packaging state; **low** for independent adoption. Class: **license / implementation artifacts / release maturity**.

- **Governance and conformance limit.** The observable control surface is the `google-agentic-commerce/AP2` organization. No foundation charter, standards-body process, TSC ratification document, official TCK, certified implementation list, or interoperability-event results were found in this pass. Therefore, classify it as vendor-hosted/open-source with governance neutrality and conformance maturity **not established**, rather than asserting unilateral vendor control.
  - **Evidence:** [S8](https://github.com/google-agentic-commerce/AP2). Publisher: Google Agentic Commerce. Date: live repository; updated date not exposed. Accessed 2026-09-11. Confidence: **high** for the absence in the retained repository surface; **medium** for ecosystem-wide absence. Class: **governance / conformance / searched-but-not-found**.

- **Composition rather than horizontal competition.** AP2 demos include A2A scenarios, while UCP explicitly supports AP2 mandates. The evidence supports viewing AP2 as a specialized authorization/payment layer, not a candidate general AgentBridge transport.
  - **Evidence:** [S8](https://github.com/google-agentic-commerce/AP2), [S6](https://github.com/Universal-Commerce-Protocol/ucp). Publishers: Google Agentic Commerce; Universal Commerce Protocol project. Dates: live repositories; updated dates not exposed. Accessed 2026-09-11. Confidence: **high**. Class: **official/observable composition / scope boundary**.

## Convergence and likely duplication map

| Existing momentum | Evidence-backed boundary | Duplication/fragmentation risk for a new AgentBridge core |
|---|---|---|
| MCP | Agent-to-tool/context integration; LF-governed; executable client/server conformance | High if AgentBridge redefines tool discovery/calling, resource access, transports, capability negotiation, or its own parallel conformance vocabulary. |
| A2A | Agent-to-agent discovery, messaging, delegation/task lifecycle; LF-governed; explicitly complementary to MCP | High if AgentBridge redefines agent cards, message/task lifecycle, streaming/polling/webhooks, or transport bindings. |
| OpenAPI + Arazzo | Language-independent API descriptions plus deterministic multi-API workflow sequencing; LF/OpenAPI governance | High if AgentBridge creates another workflow description language for API calls. |
| ACP | Standalone agent-communication effort consolidated into A2A | Very high as a warning precedent: the ecosystem has already reduced overlapping agent-communication cores through merger. |
| UCP + AP2 | Commerce capability profile plus authorization/payment mandate layer; observable composition with A2A examples | High if a horizontal core embeds commerce/payment semantics; lower if AgentBridge merely profiles these standards for a domain. |

**Evidence:** [S2](https://github.com/modelcontextprotocol/conformance), [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md), [S5](https://github.com/OAI/Arazzo-Specification), [S6](https://github.com/Universal-Commerce-Protocol/ucp), [S7](https://github.com/i-am-bee/acp), [S8](https://github.com/google-agentic-commerce/AP2). Publishers and dates as listed in the source ledger below; accessed 2026-09-11. Confidence: **medium-high**. Class: **convergence / duplication-risk inference**.

## Decision alternatives and five-year regret

### A — New protocol core

- **Adoption cost:** highest. Every client, server, gateway, SDK, and test harness must add another negotiation and lifecycle surface alongside MCP/A2A/OpenAPI/Arazzo.
- **Governance burden:** highest and durable. The core would need neutral ownership, contribution/IP terms, versioning/deprecation rules, extension registries, security response, multiple independent implementations, and public conformance evidence to approach the maturity now observable around MCP/A2A.
- **Lock-in:** initially high to AgentBridge's own semantics and maintainers even if the license is permissive. A permissive license does not create independent change control.
- **Reversibility:** low after external adopters persist protocol identifiers or wire contracts; migration aliases and gateways become permanent tax.
- **Five-year regret:** high if the missing semantics could have been an extension/profile. Likely regret modes are fragmented adoption, perpetual adapters, and a small maintainer pool competing with LF-backed cores. The counter-regret is only credible if AgentBridge can demonstrate a domain-independent invariant that existing extension mechanisms cannot represent or test.
  - **Evidence:** [S1](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md), [S2](https://github.com/modelcontextprotocol/conformance), [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md), [S5](https://github.com/OAI/Arazzo-Specification), [S7](https://github.com/i-am-bee/acp). Publishers: MCP/LF Projects; A2A/Linux Foundation; OpenAPI Initiative/Linux Foundation; BeeAI/LF AI & Data. Dates: live repositories, updated dates not exposed. Accessed 2026-09-11. Confidence: **medium-high**. Class: **five-year-regret inference**.

### B — Interoperability/conformance profile over existing standards

- **Adoption cost:** medium-low if it is declarative, version-pinned, and lets implementers reuse native MCP/A2A/OpenAPI/Arazzo libraries; medium-high if it becomes a hidden new runtime.
- **Governance burden:** bounded to mappings, profiles, test vectors, and compatibility matrices. It can use upstream licenses and governance while maintaining its own much smaller conformance process.
- **Lock-in:** low if profiles use normative upstream identifiers, preserve extensions, and publish portable fixtures/results; higher if AgentBridge invents proprietary envelopes or a mandatory broker.
- **Reversibility:** high. Individual bindings can be retired or upstreamed without invalidating the underlying implementations.
- **Five-year regret:** lowest supported risk at present. Main regret modes are becoming a lowest-common-denominator mapping or continuously chasing upstream versions. Mitigate with narrow profiles, explicit non-goals, per-standard version pins, and executable bidirectional tests. A successful profile should be upstreamable; inability to upstream is a warning signal.
  - **Evidence:** [S2](https://github.com/modelcontextprotocol/conformance), [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md), [S5](https://github.com/OAI/Arazzo-Specification), [S6](https://github.com/Universal-Commerce-Protocol/ucp). Publishers: MCP project; A2A/Linux Foundation; OpenAPI Initiative/Linux Foundation; UCP. Dates: live repositories; A2A stable releases 2026-03-12 and 2026-05-26 in S4; UCP displayed release 2026-04-09. Accessed 2026-09-11. Confidence: **medium-high**. Class: **five-year-regret inference / reversible strategy**.

### C — Stop the separate protocol effort; contribute elsewhere

- **Adoption cost and governance burden:** lowest. Engineering can target upstream extensions, SDK bugs, and conformance gaps.
- **Lock-in:** lowest to an AgentBridge namespace, but practical dependency shifts to upstream roadmaps and governance.
- **Reversibility:** high before external AgentBridge contracts exist; lower if a later restart must recover lost mindshare.
- **Five-year regret:** low if horizontal standards keep converging, as ACP-to-A2A and UCP/AP2 composition suggest. The principal counter-regret is discovering later that cross-layer semantics—such as identity/delegation, error mapping, audit evidence, or end-to-end conformance—remain orphaned between standards. This pass did not establish whether that orphan gap is real.
  - **Evidence:** [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md), [S6](https://github.com/Universal-Commerce-Protocol/ucp), [S7](https://github.com/i-am-bee/acp), [S8](https://github.com/google-agentic-commerce/AP2). Publishers: A2A/Linux Foundation; UCP; BeeAI/LF AI & Data; Google Agentic Commerce. Dates: live repositories; ACP displayed standalone release 2025-08-21. Accessed 2026-09-11. Confidence: **medium**. Class: **five-year-regret inference / evidence gap**.

## Decision gate suggested by the evidence

Do not score A/B/C yet. First require one concrete cross-standard use case to fail under this order:

1. native MCP or A2A feature;
2. official extension/binding mechanism;
3. OpenAPI/Arazzo workflow description;
4. domain profile such as UCP/AP2;
5. thin AgentBridge mapping plus executable cross-implementation tests.

Only semantics that still cannot be represented or verified after step 5 justify reopening option A. If step 5 produces no stable, repeated failure across at least two independent implementations, C is safer than preserving a protocol-shaped artifact.

**Evidence:** the layered scopes and conformance artifacts in [S2](https://github.com/modelcontextprotocol/conformance), [S3](https://github.com/a2aproject/A2A/blob/main/docs/index.md), [S5](https://github.com/OAI/Arazzo-Specification), and [S6](https://github.com/Universal-Commerce-Protocol/ucp). Publishers and dates in source ledger; accessed 2026-09-11. Confidence: **medium**. Class: **decision method / inference**.

## Contradictions and cautions

1. **Open-source license versus neutral change control.** Apache-2.0 appears across the initiatives, but only MCP, A2A, and OpenAPI/Arazzo have foundation governance clearly evidenced in the retained set. UCP and AP2 need separate governance verification. Confidence: high. Class: license/governance distinction. Sources: S1, S3, S5, S6, S8.
2. **Conformance tooling versus demonstrated interoperability.** MCP and UCP publish or link test harnesses, yet no retained source provides a certified pass list or public interop report. A test repository is necessary evidence, not proof of ecosystem-wide interoperability. Confidence: high. Class: conformance caution. Sources: S2, S6.
3. **Stable label versus ecosystem maturity.** A2A has a 1.x release line, while AP2 has no displayed tagged release/package and ACP's standalone line is now subsumed. Version labels across projects are not comparable maturity scores. Confidence: high. Class: vitality caution. Sources: S4, S7, S8.
4. **Official composition versus full-stack completeness.** A2A/MCP complementarity and UCP/AP2 layering reduce scope overlap, but do not prove cross-layer identity, delegation, audit, error, and version semantics are aligned. Confidence: medium-high. Class: standards-gap caution. Sources: S3, S6, S8.

## Leads for round 2

- Retrieve and analyze UCP's governing charter, Governing Council/Tech Council appointment rules, meeting minutes, IPR terms, and ratification process; compare effective seat distribution with the formal rules.
- Inspect A2A and MCP conformance repositories for machine-readable requirement coverage, known exclusions, and CI integrations in independently maintained SDKs; collect actual result artifacts, not README claims.
- Inspect AP2's changelog, contribution/CLA policy, maintainer ownership, open conformance-vector proposals, and any independent implementation that does not depend on Google's SDK.
- Locate public interoperability events, plugfests, certified implementations, or compatibility reports for MCP, A2A, UCP, and AP2.
- Build a normative overlap table for discovery, capability advertisement, identity/authentication, delegation, lifecycle, streaming, error models, receipts/audit evidence, extension negotiation, and version negotiation.
- Verify the OpenAPI Initiative and Arazzo voting/ratification procedure and any formal conformance program beyond independent tooling.

## Searched but not found in this pass

- Public plugfest/interoperability-event result reports for the retained initiatives.
- Public certification registries or authoritative pass matrices naming conformant MCP, A2A, UCP, or AP2 implementations.
- A foundation or standards-body charter for AP2.
- An AP2 official TCK or released package distribution.
- Independent AP2 implementations with published cross-implementation results.
- A retained primary source proving that a domain-independent cross-layer identity/delegation/audit profile is already standardized end to end.

Absence here means **not found within the bounded search**, not proof that the artifact does not exist.

## Source ledger — eight retained primary sources

| ID | Direct URL | Publisher | Published / updated | Accessed | Primary use |
|---|---|---|---|---|---|
| S1 | https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md | Model Context Protocol / LF Projects, LLC | Live main branch; updated date not exposed | 2026-09-11 | MCP governance, copyright, licenses/IP |
| S2 | https://github.com/modelcontextprotocol/conformance | Model Context Protocol project | Live repository; supports protocol revisions through 2026-07-28; page update date not exposed | 2026-09-11 | MCP conformance artifact and version coverage |
| S3 | https://github.com/a2aproject/A2A/blob/main/docs/index.md | A2A Project / Linux Foundation | Live main branch; updated date not exposed | 2026-09-11 | A2A governance, license, backing, MCP complementarity |
| S4 | https://github.com/a2aproject/A2A/releases | A2A Project / Linux Foundation | 1.0.0: 2026-03-12; 1.0.1: 2026-05-26 | 2026-09-11 | A2A release vitality and specification change artifacts |
| S5 | https://github.com/OAI/Arazzo-Specification | OpenAPI Initiative / Linux Foundation | Live repo; current version 1.1.0; release date not exposed | 2026-09-11 | Arazzo governance, license, scope, independent tooling |
| S6 | https://github.com/Universal-Commerce-Protocol/ucp | Universal Commerce Protocol project | Displayed release v2026-04-08 published 2026-04-09; live repo | 2026-09-11 | UCP scope, license, SDK/samples/conformance links, AP2 composition |
| S7 | https://github.com/i-am-bee/acp | BeeAI contributors / Linux Foundation AI & Data | Latest displayed standalone release v1.0.3: 2025-08-21; live merger notice | 2026-09-11 | ACP license, implementations, governance, A2A consolidation |
| S8 | https://github.com/google-agentic-commerce/AP2 | Google Agentic Commerce | Live repo; no tagged release displayed; update date not exposed | 2026-09-11 | AP2 license, implementation artifacts, packaging/governance limits |

