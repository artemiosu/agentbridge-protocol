# Governance and conformance feasibility of a thin cross-standard profile — round 2

**Decision served:** choose among (A) a new AgentBridge core, (B) a thin interoperability/conformance profile over existing standards, or (C) stop the separate protocol effort and contribute upstream.

**Research date / accessed:** 2026-09-11. **Mode:** technical deep recon, round-2 lead-following. **Scope:** official specifications, project repositories, foundation governance pages, and official meeting records only. Eight primary sources were retained. Search was bounded to eleven web calls. The only project evidence admitted was the named round-1 ecosystem digest.

**Terminology:** In this digest, **AP2** means Google's Agent Payments Protocol, now contributed to the FIDO Alliance. **ACP** means the current OpenAI/Stripe Agentic Commerce Protocol when referenced as a commerce standard. The older BeeAI **Agent Communication Protocol** is not treated as a current competitor; it is only the consolidation-into-A2A precedent established in round 1.

## Decision answer

**Option B is governable and independently testable without becoming a hidden runtime, but only under hard negative-scope rules.** The evidence supports a profile as an immutable bundle of version pins, upstream identifiers, mapping assertions, and portable test fixtures. It does **not** support an AgentBridge transport, broker, message envelope, scheduler, registry dependency, or new task lifecycle.

The integration path is asymmetric:

- A2A explicitly permits anyone to publish an extension under their own URI and provides an experimental-to-official path for artifacts hosted by the A2A organization. Official graduation requires a reference implementation, adoption evidence, maintainer commitment, and a TSC vote. [S3]
- MCP now has separately versioned official and experimental extension tracks, but its official status remains under MCP maintainer governance. A third-party cross-standard profile can bind to MCP's negotiated extension surface and test it externally, but cannot imply that its extension is officially endorsed until it completes the upstream process. [S1]
- Both MCP and A2A provide executable core conformance harnesses that can target arbitrary implementations and emit inspectable results. Their retrieved policies keep optional extensions outside baseline core conformance, so the cross-standard profile needs its own **supplemental** TCK while continuing to run the upstream TCKs unchanged. [S1][S2][S3][S4]
- UCP's suite demonstrates that a protocol-specific, language-agnostic test pack can run against any merchant server with an explicit target version and fixture configuration. It does not provide public proof that independent products pass. [S6]
- OpenAPI's change process explicitly encourages trial use as `x-*` extensions before a formal proposal, and its release process uses recorded TSC votes. This creates a credible upstream route for stable profile metadata instead of a permanent AgentBridge namespace. [S8]

**Recommendation:** proceed with B only as a time-boxed conformance experiment. If two independently maintained implementations cannot pass the same cross-standard fixtures without an AgentBridge runtime, choose C. Reopen A only if repeated failures expose semantics that cannot be expressed through upstream extension mechanisms and remain irreducible across at least two independent implementations.

- **Claim:** A thin profile can be tested independently because the upstream MCP, A2A, and UCP suites accept externally supplied systems under test; however, adoption is not demonstrated by suite existence.
  - **Direct URLs:** [S2], [S4], [S6]
  - **Publisher:** Model Context Protocol project; A2A Project / Linux Foundation; Universal Commerce Protocol project
  - **Published/updated:** live repositories; page-level update dates not exposed. MCP README currently covers protocol revisions through 2026-07-28; UCP example pins 2026-04-08.
  - **Accessed:** 2026-09-11
  - **Confidence:** high for independent executability; low for adoption
  - **Class:** conformance architecture / adoption evidence limit

- **Claim:** B avoids becoming a competing core only if its normative content is limited to mappings and tests and all execution semantics remain upstream.
  - **Direct URLs:** [S1], [S3], [S8]
  - **Publisher:** Model Context Protocol project; A2A Project / Linux Foundation; OpenAPI Initiative / Linux Foundation
  - **Published/updated:** S1 created 2025-01-21 and currently Final; S3 and S8 are live main-branch policies with page-level update dates not exposed.
  - **Accessed:** 2026-09-11
  - **Confidence:** medium-high
  - **Class:** architectural inference / protocol-fragmentation control

## 1. MCP and A2A: extension, version, publication, and TCK machinery

### MCP

1. **MCP's extension mechanism is optional, composable, and independently versioned.** SEP-2133 defines official and experimental extensions, separate extension versions from core protocol versions, capability negotiation, backward-compatibility duties, and a new identifier for a breaking extension change. Core maintainers retain ultimate authority over official extensions. This is compatible with an external profile binding, but official publication is not unilateral. [S1]
   - **Direct URL:** https://modelcontextprotocol.io/seps/2133-extensions
   - **Publisher:** Model Context Protocol project
   - **Published/updated:** created 2025-01-21; status Final; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** extension governance / versioning / publication

2. **MCP's official conformance harness can test arbitrary clients and servers and pin a specification revision.** It launches a scenario peer, captures protocol traffic, validates messages against the negotiated version's schema, emits per-check JSON artifacts, supports dated revisions through `2026-07-28`, and publishes a reusable CI action. [S2]
   - **Direct URL:** https://github.com/modelcontextprotocol/conformance
   - **Publisher:** Model Context Protocol project
   - **Published/updated:** live repository; README exposes action `v0.1.11` and revisions through 2026-07-28; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** conformance / version compatibility / CI publication

3. **A green MCP CI job is not automatically a clean conformance result.** The harness permits declared expected-failure baselines to exit successfully. A credible profile result must therefore publish the target protocol version, suite selection, harness release, raw checks, and expected-failure file rather than only a badge. [S2]
   - **Direct URL:** https://github.com/modelcontextprotocol/conformance
   - **Publisher:** Model Context Protocol project
   - **Published/updated:** live repository; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** conformance-claim semantics / auditability

### A2A

4. **A2A provides the cleanest formal home for third-party extension incubation.** Anyone may develop and publish an extension independently. Artifacts seeking A2A-hosted experimental status need maintainer sponsorship; official artifacts require Apache-2.0 licensing, at least one reference implementation, adoption or interest evidence, ongoing maintainer commitment, and a TSC vote with at least 50% quorum and a majority of attendees. [S3]
   - **Direct URL:** https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live main-branch governance document; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** extension governance / license / adoption gate

5. **A2A has explicit anti-fragmentation constraints that a profile should copy.** Official extension identifiers use versioned URIs; breaking changes require a new identifier and TSC review; SDK support is opt-in and disabled by default; and extension support is not required for core protocol conformance. Extensions also cannot redefine core structures or extend core enums directly. [S3]
   - **Direct URL:** https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live main branch; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** compatibility / versioning / protocol-boundary governance

6. **A2A's TCK is independently runnable and produces portable result formats.** It targets a supplied A2A endpoint, discovers declared interfaces from the Agent Card, tests gRPC, JSON-RPC, and HTTP+JSON, and emits machine-readable JSON plus HTML and JUnit reports. Tests are classified by RFC 2119 level: MUST failures block compatibility, SHOULD tests are non-blocking expected failures, and undeclared MAY capabilities are skipped. [S4]
   - **Direct URL:** https://github.com/a2aproject/a2a-tck/blob/main/README.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live main branch; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** TCK / transport coverage / result semantics

7. **A2A TCK pinning needs extra discipline from a cross-standard profile.** The retrieved README exposes `make spec`, which refreshes specification files from the A2A repository, but it does not expose a user-facing `--spec-version` flag comparable to MCP's. The profile must pin both an A2A protocol version and an immutable TCK release or commit; tracking TCK `main` is not reproducible. [S4]
   - **Direct URL:** https://github.com/a2aproject/a2a-tck/blob/main/README.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live main branch; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** medium-high; bounded to the retrieved public README
   - **Class:** version pinning / reproducibility gap

### Cross-standard integration conclusion

8. **A third-party profile can integrate cleanly, but it should publish one profile identifier and separate upstream bindings rather than a shared wire envelope.** A2A can advertise the profile URI through its native extension negotiation. MCP can negotiate the corresponding MCP binding through its native extension surface. Each binding must remain optional, versioned, and removable. The profile's TCK should call the native upstream TCK first and then run only cross-standard semantic assertions. [S1][S2][S3][S4]
   - **Direct URLs:** https://modelcontextprotocol.io/seps/2133-extensions; https://github.com/modelcontextprotocol/conformance; https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md; https://github.com/a2aproject/a2a-tck/blob/main/README.md
   - **Publisher:** MCP project; A2A Project / Linux Foundation
   - **Published/updated:** as above
   - **Accessed:** 2026-09-11
   - **Confidence:** medium-high
   - **Class:** architecture inference / independent conformance

## 2. UCP, AP2, and commerce-protocol governance

### UCP

9. **UCP has observable technical-governance practice, but this pass did not recover a complete governing/IP charter.** Official Tech Council minutes say elections occur every six months, contribution/implementation/adoption are primary candidate criteria, the council makes logged decisions on proposals, and releases are stabilized by snapshots. That is stronger than a partner list, but it does not establish Governing Council appointment rights, formal quorum/ballot thresholds, patent policy, trademark policy, or maintainer removal rules. [S5]
   - **Direct URL:** https://github.com/Universal-Commerce-Protocol/meeting-minutes/blob/main/tc/2026/2026-03-13.md
   - **Publisher:** Universal Commerce Protocol project, Tech Council
   - **Published/updated:** 2026-03-13
   - **Accessed:** 2026-09-11
   - **Confidence:** high for observed council practice; low-medium for full governance neutrality
   - **Class:** governance / council practice / evidence limit

10. **UCP's conformance suite is genuinely external-system-capable and version-aware.** The official suite calls itself language-agnostic, runs against any UCP Merchant Server, accepts merchant-specific fixtures, and includes a `ucp_version` field in the conformance input. This is a usable precedent for profile-specific fixtures. [S6]
   - **Direct URL:** https://github.com/Universal-Commerce-Protocol/conformance/blob/main/README.md
   - **Publisher:** Universal Commerce Protocol project
   - **Published/updated:** live main branch; example target `2026-04-08`; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** conformance / independent implementation targeting / version pinning

11. **UCP results are not automatically comparable without publishing the test input and fixtures.** Merchant-specific expected items, inventory, shipping, discounts, secrets, and capability declarations shape what runs and what is asserted. A profile adopting UCP tests must publish the exact fixture configuration and distinguish skipped optional behavior from passed requirements. [S6]
   - **Direct URL:** https://github.com/Universal-Commerce-Protocol/conformance/blob/main/README.md
   - **Publisher:** Universal Commerce Protocol project
   - **Published/updated:** live main branch; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** conformance comparability / reproducibility

12. **No official UCP certification registry, named independent pass matrix, or public plugfest result was found in the bounded search.** The retrieved official walkthrough tests the separately packaged official sample server; it does not document results from independently governed implementations. [S6]
   - **Direct URL:** https://github.com/Universal-Commerce-Protocol/conformance/blob/main/README.md
   - **Publisher:** Universal Commerce Protocol project
   - **Published/updated:** live main branch; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** medium-high as a bounded-absence statement
   - **Class:** public interoperability evidence / searched-not-found

### AP2 (Google Agent Payments Protocol)

13. **AP2 core-spec governance is in transition from Google's repository to FIDO.** FIDO says Google's AP2 contribution and Mastercard's Verifiable Intent contribution are being reviewed and further developed in FIDO's Payments Technical Working Group, while agent authentication work is handled by a separate Agentic Authentication Technical Working Group. This is standards-body stewardship evidence, not yet a published AP2 ratification or certification program. [S7]
   - **Direct URL:** https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/
   - **Publisher:** FIDO Alliance
   - **Published/updated:** 2026-04-28
   - **Accessed:** 2026-09-11
   - **Confidence:** high for contribution and working-group placement; medium for eventual governance outcome
   - **Class:** governance transition / standards stewardship

14. **The FIDO handoff does not yet supply public AP2-specific conformance evidence.** The retrieved FIDO announcement describes future collaborative review and notes FIDO's general certification capabilities, but it does not publish an AP2 TCK, canonical test-vector set, certified implementation list, or interop-event results. [S7]
   - **Direct URL:** https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/
   - **Publisher:** FIDO Alliance
   - **Published/updated:** 2026-04-28
   - **Accessed:** 2026-09-11
   - **Confidence:** medium-high as a bounded-absence statement
   - **Class:** conformance maturity / searched-not-found

15. **No independently maintained AP2 implementation with official cross-implementation results was found.** Repository discussions and third-party repositories surfaced in search, but they were excluded because they were not official result publications from AP2/FIDO and did not meet the requested primary-source bar. [S7]
   - **Direct URL:** https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/
   - **Publisher:** FIDO Alliance
   - **Published/updated:** 2026-04-28
   - **Accessed:** 2026-09-11
   - **Confidence:** medium; bounded to the official-source search
   - **Class:** independent implementation / public interoperability evidence limit

## 3. OpenAPI and Arazzo: governance, balloting, extensions, and upstreaming

16. **OpenAPI has a documented, contribution-driven upstream path with explicit release ballots.** Anyone may propose a change. Larger changes begin as an enhancement discussion, should show use cases and alternatives, and benefit from real use as an `x-*` extension. A formal proposal follows demonstrated support. For the 3.x release process, patch releases require a TSC majority within up to three days; minor and major releases require 66% within up to seven and fourteen days respectively. [S8]
   - **Direct URL:** https://github.com/OAI/OpenAPI-Specification/blob/main/CONTRIBUTING.md
   - **Publisher:** OpenAPI Initiative / Linux Foundation
   - **Published/updated:** live main-branch contribution policy; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high
   - **Class:** governance / balloting / upstream process

17. **A profile should incubate OpenAPI metadata as narrowly scoped `x-*` fields and seek upstream adoption only after implementation evidence.** The official process expressly treats real extension use as useful evidence and permits commonly used `x-*` fields to be adopted into a later minor release. This makes upstream-first change control practical and reversible. [S8]
   - **Direct URL:** https://github.com/OAI/OpenAPI-Specification/blob/main/CONTRIBUTING.md
   - **Publisher:** OpenAPI Initiative / Linux Foundation
   - **Published/updated:** live main branch; page-level last update not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high for OpenAPI; medium for cross-applying the pattern to Arazzo
   - **Class:** profile extension / upstreamability / reversibility

18. **Arazzo-specific balloting and conformance remain incompletely evidenced.** The live Arazzo contribution page was inspected and showed immutable published versions, active version branches, community-proposed additions, and the possibility of adopting common `x-*` fields; it did not expose a release-ballot threshold or official TCK. Because the eight-source cap retained the more specific OpenAPI ballot policy, this digest does not claim that Arazzo uses the same numerical thresholds. [S8]
   - **Direct URL:** https://github.com/OAI/OpenAPI-Specification/blob/main/CONTRIBUTING.md (retained governing comparison); inspected but not retained: `OAI/Arazzo-Specification/CONTRIBUTING.md`
   - **Publisher:** OpenAPI Initiative / Linux Foundation
   - **Published/updated:** live main branches; page-level update dates not exposed
   - **Accessed:** 2026-09-11
   - **Confidence:** high for the evidence limit; medium for governance analogy
   - **Class:** Arazzo governance / conformance gap / bounded absence

## 4. Observable public conformance and interoperability results

The official sources establish **testability**, not broad demonstrated interoperability.

| Initiative | Observable official artifact | Observable named independent pass result? | Bounded conclusion |
|---|---|---|---|
| MCP | Arbitrary client/server harness, version selection, per-check JSON, CI action, expected-failure baselines [S2] | No authoritative pass registry or public cross-vendor matrix found | Independently testable; ecosystem interoperability rate unproven |
| A2A | Endpoint-driven TCK for three bindings; JSON/HTML/JUnit reports; official SDK code-generation targets [S4] | No retained certification list, plugfest report, or cross-vendor result matrix found | TCK is executable; public independent interoperability unproven |
| UCP | Language-agnostic tests against any merchant server, with target version and custom fixtures [S6] | No named independent pass matrix or interop report found | Suitable precedent for a profile TCK; comparability requires fixture publication |
| AP2 | FIDO working-group review has begun [S7] | No official AP2 TCK, vector corpus, certified implementation list, or event report found | Too early to use as a proven conformance substrate |
| OpenAPI/Arazzo | Formal proposal and release governance [S8] | No official cross-tool conformance report found in this pass | Upstream path exists; interoperability evidence is not supplied by governance alone |

**Important exclusion:** search surfaced self-reported pass claims in applications/issues and third-party AP2 vector repositories. They were not counted as official public interoperability results because the requested evidence bar was an official result publication or reproducible artifact with authoritative provenance.

## 5. Hard guardrails: how B must not become a hidden runtime

These are proposed **normative admission rules** for any AgentBridge profile release.

1. **No transport.** The profile MUST NOT define sockets, HTTP endpoints, RPC methods, streaming, framing, delivery, retries, authentication handshakes, or connection lifecycle. Every message travels through a pinned upstream binding.
2. **No broker or required service.** Conformance MUST run locally or in ordinary CI. No AgentBridge daemon, hosted registry, gateway, control plane, router, or account may be required to implement or test the profile.
3. **No envelope.** Native MCP, A2A, OpenAPI, Arazzo, UCP, ACP, or AP2 objects remain the wire objects. The profile MUST NOT wrap them in an AgentBridge message or replace their error/task/state models.
4. **Upstream identifiers only.** Normative fields, method names, capability names, schema references, and extension URIs MUST be the upstream identifiers. A profile identifier may identify only the mapping/test bundle, never a replacement protocol object.
5. **Immutable version tuple.** Every release MUST pin: profile version; each upstream spec version; each upstream extension/binding identifier and version; schema digest; and each upstream TCK release or commit. `latest`, rolling `main`, or an unqualified protocol name is non-conformant.
6. **Separate core and profile claims.** Results MUST state, for example, “MCP 2025-11-25 core + AgentBridge profile X” rather than “AgentBridge-compatible MCP.” Upstream core conformance MUST be run and reported separately from supplemental profile assertions.
7. **Publish all result-shaping inputs.** Reports MUST include raw checks, requirement IDs, suite selection, skipped tests, expected-failure baselines, capability declarations, UCP-style fixture configuration, and harness versions. A badge alone is not evidence.
8. **Reversible bindings.** Each upstream binding MUST be a separately removable module. Removing it MUST leave a valid native implementation. Unknown upstream extensions MUST round-trip without loss; no AgentBridge-only persistent state may be required.
9. **Open, deterministic TCK.** The profile TCK MUST be permissively licensed, runnable without network access after dependency retrieval, map every assertion to a published requirement, publish deterministic fixtures/expected outputs, and emit JSON plus a standard CI format such as JUnit.
10. **Independent implementation gate.** A mapping cannot be called stable until at least two independently maintained implementations, in different repositories and without a shared AgentBridge runtime library, pass the same public fixtures. At least one test must exercise native MCP and native A2A implementations end to end.
11. **No conformance by common library.** Two adapters importing the same AgentBridge execution package count as one implementation. Shared schemas/test fixtures are allowed; shared orchestration, state machines, brokers, or mapping engines are not evidence of independence.
12. **Upstream-first semantic change.** Any new behavior beyond a mechanical mapping MUST first be proposed to the owning upstream project. A provisional profile rule must link the upstream issue/proposal, carry an expiry, and disappear or be revised when upstream decides.
13. **Automatic supersession.** When an upstream standard defines the profiled semantic, the AgentBridge rule MUST deprecate in the next profile release and map to the upstream identifier. The profile cannot preserve a parallel name for compatibility convenience indefinitely.
14. **No normative execution policy.** Scheduling, planning, model selection, authorization decisions, retries, saga compensation, storage, observability backends, and policy-engine behavior are out of scope. Testable assertions may verify upstream-declared outcomes, not prescribe an AgentBridge runtime.
15. **Narrow governance mandate.** Profile maintainers may approve mappings, version pins, fixtures, errata, and release metadata only. Expanding that mandate requires a public charter change and an explicit A-versus-B decision review.
16. **Stop conditions.** Choose C if two release cycles fail to attract two independent implementations, or if most changes are adapters to upstream churn. Reconsider A only after a documented, repeated semantic failure survives upstream proposals and cannot be expressed by optional extensions, bindings, or registered `x-*` fields.

## Contradictions and tensions

1. **Optional extension versus interoperable profile.** MCP and A2A protect core interoperability by making extension support optional, but a profile needs selected extensions to be mandatory within the profile. Resolution: mandatory only for a named profile conformance claim, never for upstream core conformance. Confidence: high. Sources: S1, S3.
2. **Official conformance versus green CI.** MCP expected-failure baselines, A2A non-blocking SHOULD tests and skipped MAY tests, and UCP merchant-specific fixtures mean “tests passed” can describe materially different coverage. Resolution: publish raw scoped results and all baselines/fixtures. Confidence: high. Sources: S2, S4, S6.
3. **Independent publication versus official namespace.** A2A lets anyone publish under their own URI, but A2A-hosted official status needs sponsorship and TSC approval. MCP official extensions remain under core-maintainer authority. Resolution: begin externally, use permanent profile-owned identifiers, and upstream bindings independently. Confidence: high. Sources: S1, S3.
4. **Neutral stewardship versus mature artifacts.** AP2's FIDO contribution improves the governance trajectory, but no AP2-specific public TCK or ratified result program was found. Governance venue is not conformance maturity. Confidence: medium-high. Source: S7.
5. **Upstream-first versus cross-standard ownership.** OpenAPI can absorb successful `x-*` fields, while A2A and MCP have their own extension processes; no one upstream body owns a cross-standard mapping. Resolution: the profile may own only the mapping and fixtures, while every semantic identifier remains upstream-owned. Confidence: medium-high. Sources: S1, S3, S8.
6. **A2A TCK freshness versus reproducibility.** Refreshing embedded spec material from A2A `main` helps currency but can make historical profile results irreproducible. Resolution: pin an immutable TCK commit and archive the generated requirement manifest with each result. Confidence: medium-high. Source: S4.

## Unfound evidence and bounded absences

- No authoritative public pass matrix, certification registry, or plugfest report naming mutually interoperable MCP implementations was found in the retained official sources.
- No authoritative public A2A cross-vendor result matrix or interoperability-event report was found; the official TCK and SDK integration targets are infrastructure, not published ecosystem results.
- No UCP governing charter with Governing Council appointment/removal powers, formal Tech Council quorum/ballot rules, patent policy, or trademark/certification policy was recovered. Official minutes show practice, not the whole constitution.
- No public UCP certification registry, independently governed implementation pass report, or plugfest result was found.
- No AP2-specific FIDO draft, ballot rules, IPR terms, official TCK, canonical vector corpus, certification profile, independently maintained implementation result, or interop-event report was found.
- No Arazzo-specific release-ballot threshold or official Arazzo TCK was found. Do not assume the numerical OpenAPI 3.x ballot thresholds apply unchanged to Arazzo.
- No official evidence was found that one cross-standard profile identifier is accepted across MCP, A2A, OpenAPI/Arazzo, UCP, ACP, and AP2. Upstreaming must be per standard.
- No evidence was found that implementers want an AgentBridge profile. The evidence establishes technical feasibility and governance constraints, not adoption demand.

Absence means **not found within this official-source, eleven-call round**, not proof that the artifact does not exist.

## Source ledger — eight retained primary sources

| ID | Direct URL | Publisher | Published / updated | Accessed | Primary use |
|---|---|---|---|---|---|
| S1 | https://modelcontextprotocol.io/seps/2133-extensions | Model Context Protocol project | Created 2025-01-21; status Final; last update not exposed | 2026-09-11 | MCP extension governance, versioning, publication authority |
| S2 | https://github.com/modelcontextprotocol/conformance | Model Context Protocol project | Live repo; README covers versions through 2026-07-28 and CI action v0.1.11; page update not exposed | 2026-09-11 | MCP arbitrary-SUT conformance, results, expected failures, pins |
| S3 | https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md | A2A Project / Linux Foundation | Live main branch; page update not exposed | 2026-09-11 | A2A external/official extension paths, identifiers, votes, licensing |
| S4 | https://github.com/a2aproject/a2a-tck/blob/main/README.md | A2A Project / Linux Foundation | Live main branch; page update not exposed | 2026-09-11 | A2A TCK targeting, transports, report formats, requirement levels |
| S5 | https://github.com/Universal-Commerce-Protocol/meeting-minutes/blob/main/tc/2026/2026-03-13.md | Universal Commerce Protocol Tech Council | 2026-03-13 | 2026-09-11 | UCP council practice, elections, decisions, release snapshots |
| S6 | https://github.com/Universal-Commerce-Protocol/conformance/blob/main/README.md | Universal Commerce Protocol project | Live main branch; example pins 2026-04-08; page update not exposed | 2026-09-11 | UCP arbitrary-server tests, fixtures, version input, evidence limits |
| S7 | https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/ | FIDO Alliance | 2026-04-28 | 2026-09-11 | AP2 contribution, working-group review, current governance trajectory |
| S8 | https://github.com/OAI/OpenAPI-Specification/blob/main/CONTRIBUTING.md | OpenAPI Initiative / Linux Foundation | Live main branch; page update not exposed | 2026-09-11 | OpenAPI proposal path, `x-*` incubation, release ballot thresholds |
