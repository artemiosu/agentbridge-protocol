# Fresh-context verification — governance, versions, extension paths, and conformance

**Verification date / access date:** 2026-09-11  
**Evidence rule:** primary official sources only. The two assigned digests were treated only as claim inventories; neither digest nor any other project context was accepted as evidence.  
**Scope note:** the bounded absence check used official project/Foundation domains and distinguished published cross-vendor interoperability evidence from single-project SDK assessments and third-party self-reports.

## Claim ledger

### MCP

1. **VERIFIED — MCP is an LF Projects series with Apache-2.0 code/spec licensing and CC-BY-4.0 non-spec documentation.**
   - **Direct URL:** https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md
   - **Publisher:** Model Context Protocol / LF Projects, LLC
   - **Published/updated:** live `main`; page-level update date not exposed
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The governance file says MCP is established as a Series of LF Projects, LLC; governance changes also require LF Projects approval; new code/spec contributions and outbound code/specifications use Apache-2.0 except for maintainer-approved open-license exceptions; non-spec documentation uses CC-BY-4.0. This matches the digest's governance/license claim.

2. **VERIFIED — MCP has official, experimental, and unofficial extension paths, with optional negotiation and independent extension versioning.**
   - **Direct URL:** https://modelcontextprotocol.io/seps/2133-extensions
   - **Publisher:** Model Context Protocol project
   - **Published/updated:** created 2025-01-21; status Final; the page warns that the current specification/changelog is authoritative for later changes
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** SEP-2133 defines optional composable extensions, official and experimental repositories, externally governed unofficial extensions, reverse-domain vendor identifiers, a new identifier for breaking changes, capability negotiation, opt-in SDK support, and no extension requirement for core conformance. Official acceptance requires an Extensions Track SEP, an official-SDK reference implementation, and Core Maintainer approval. The digest is accurate that third parties can build outside the official namespace but cannot imply official endorsement. Important qualification: the SEP expressly says profile grouping is not specified.

3. **VERIFIED — MCP's conformance harness targets arbitrary clients/servers and is revision-aware, but `2026-07-28` is the 2026 draft wire revision.**
   - **Direct URL:** https://github.com/modelcontextprotocol/conformance
   - **Publisher:** Model Context Protocol project
   - **Published/updated:** live repository; page-level update date not exposed
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The README documents client command and server URL systems under test, captured traffic, per-check JSON, wire-schema validation against the negotiated revision, `--spec-version`, and frozen `--requirements` sets. It distinguishes dated revisions through `2025-11-25` from the `2026-07-28` draft/stateless revision. The README still shows composite-action examples pinned to `v0.1.11`, but that example is not evidence that `v0.1.11` is the latest harness release. The digest's capability statement is supported; describing `2026-07-28` without the draft qualifier would be incomplete.

4. **VERIFIED — a green MCP CI job can contain expected failures and therefore is not, by itself, a clean conformance claim.**
   - **Direct URL:** https://github.com/modelcontextprotocol/conformance
   - **Publisher:** Model Context Protocol project
   - **Published/updated:** live repository; page-level update date not exposed
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The documented baseline makes a known failure exit successfully while a stale baseline fails. Frozen requirement sets separately mark extensions, post-release tests, and pending tests as unscored. The digest's requirement to publish the revision, requirement/suite selection, raw checks, and baseline is supported. Qualification: a baselined failure remains a recorded failure even when CI exits zero.

### A2A

5. **VERIFIED — A2A reached `1.0.0` on 2026-03-12 and `1.0.1` on 2026-05-26.**
   - **Direct URL:** https://github.com/a2aproject/A2A/releases
   - **Publisher:** A2A Project
   - **Published/updated:** `1.0.0` dated 2026-03-12; `1.0.1` dated 2026-05-26
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The official release page labels `v1.0.1` Latest and records both dates. The `1.0.0` notes include the separation of the application protocol from transport mappings, and `1.0.1` lists binding/spec fixes. This matches the digest.

6. **VERIFIED — A2A explicitly permits independent publication and defines a governed path to A2A-hosted experimental and official extensions/bindings.**
   - **Direct URL:** https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live `main`; page-level update date not exposed
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The policy says anyone may independently develop and publish an extension or custom protocol binding. A2A-hosted official artifacts require Apache-2.0, a reference implementation, adoption/interest evidence, maintainer commitment, and a TSC vote with at least 50% quorum and a majority of attendees. Official URIs are version-capable; breaking changes require a new identifier and TSC review; SDK support is opt-in and excluded from core conformance. This matches the digest's extension-governance claims.

7. **UNVERIFIED — the exact round-1 claim that Google donated A2A to the Linux Foundation and that the TSC has the named eight-company roster.**
   - **Direct URL:** https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live `main`; page-level update date not exposed
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The retained official governance policy establishes TSC control, Apache-2.0 requirements, and a contributor grant to the Linux Foundation, which supports foundation-linked governance. It does not itself state the donation event or reproduce the named corporate roster. Those exact subclaims were not independently confirmed in the bounded verifier source set.

8. **VERIFIED — the A2A TCK targets an external endpoint, covers three bindings, and emits portable reports with requirement-level semantics.**
   - **Direct URL:** https://github.com/a2aproject/a2a-tck/blob/main/README.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live `main`; page-level update date not exposed
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The TCK accepts `--sut-host`, discovers `supportedInterfaces` from the Agent Card, tests gRPC, JSON-RPC, and HTTP+JSON, and emits compatibility JSON, self-contained HTML, pytest HTML, and JUnit XML. MUST failures are blocking, SHOULD tests are non-blocking expected failures, and undeclared MAY capabilities are skipped. This matches the digest.

9. **VERIFIED — A2A TCK reproducibility requires an immutable TCK pin in addition to a protocol pin.**
   - **Direct URL:** https://github.com/a2aproject/a2a-tck/blob/main/README.md
   - **Publisher:** A2A Project / Linux Foundation
   - **Published/updated:** live `main`; page-level update date not exposed
   - **Accessed:** 2026-09-11
   - **Exact support/mismatch:** The README exposes `make spec` to update embedded specification files from the A2A repository but exposes no user-facing specification-version or release selector. Therefore the digest's advice to pin an immutable TCK release/commit is supported. The README alone does not identify which A2A protocol version an arbitrary TCK commit contains; any broader compatibility assertion remains unsupported unless the commit manifest is archived.

### OpenAPI and Arazzo

10. **VERIFIED — OpenAPI has an open proposal path, encourages pre-proposal `x-*` evidence, and records numerical release ballots.**
    - **Direct URL:** https://github.com/OAI/OpenAPI-Specification/blob/main/CONTRIBUTING.md
    - **Publisher:** OpenAPI Initiative / Linux Foundation
    - **Published/updated:** live `main`; page-level update date not exposed
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** Anyone can propose changes. Larger changes start with an enhancement discussion containing use cases, a solution, and alternatives; the policy says it helps to use the idea as an `x-*` extension before a formal proposal. Patch releases require a TSC majority within three days; minor and major releases require 66% within seven and fourteen days. This matches the digest.

11. **VERIFIED — Arazzo sits under the OpenAPI Initiative and has its own public contribution/version process, but the retrieved page has no numerical release ballot or official TCK.**
    - **Direct URL:** https://github.com/OAI/Arazzo-Specification/blob/main/CONTRIBUTING.md
    - **Publisher:** OpenAPI Initiative / Linux Foundation
    - **Published/updated:** live `main`; page-level update date not exposed
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** The page explicitly says Arazzo sits under the OpenAPI Initiative, accepts issues and pull requests, keeps published versions immutable, uses versioned development branches, allows community-proposed additions, and cites adoption of common `x-*` fields as valid minor-release work. It does not state the OpenAPI Specification's numerical TSC ballot thresholds and exposes no Arazzo TCK. The digest correctly refused to transfer OpenAPI's ballot numbers to Arazzo.

12. **OVERTURNED — Arazzo's current version is `1.1.0`.**
    - **Direct URL:** https://github.com/OAI/Arazzo-Specification/blob/main/CONTRIBUTING.md
    - **Publisher:** OpenAPI Initiative / Linux Foundation
    - **Published/updated:** live `main`; page-level update date not exposed
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** The current contribution page lists `1.1.1` as the active patch release line and `1.2.0` as the minor release in development. The round-1 `1.1.0` current-version statement is stale.

### UCP

13. **VERIFIED — UCP has observable Tech Council governance practice, but the cited minutes do not establish a complete constitutional/IP regime.**
    - **Direct URL:** https://github.com/Universal-Commerce-Protocol/meeting-minutes/blob/main/tc/2026/2026-03-13.md
    - **Publisher:** Universal Commerce Protocol project, Tech Council
    - **Published/updated:** 2026-03-13
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** The official minutes record six-month Tech Council elections, contribution/implementation/adoption as election criteria, council decisions on proposals, and release stabilization by snapshots. They do not define Governing Council appointment/removal powers, quorum/ballot thresholds, patent terms, trademark/certification policy, or maintainer removal. This exactly supports the digest's limited classification.

14. **VERIFIED — UCP's conformance suite targets any merchant-server implementation, pins a protocol version, and is fixture-sensitive.**
    - **Direct URL:** https://github.com/Universal-Commerce-Protocol/conformance
    - **Publisher:** Universal Commerce Protocol project
    - **Published/updated:** live `main`; example target `2026-04-08`; page-level update date not exposed
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** The suite describes language-agnostic integration tests against any UCP server. `conformance_input.json` includes `ucp_version` and required capabilities; merchant data and `test_fixtures.json` determine assertions, while unconfigured optional features can be skipped. The digest is correct that comparable publication requires the exact input, fixtures, skipped-test information, and harness revision.

15. **VERIFIED — no official UCP certification registry or named independent pass matrix was found in the bounded official-source check.**
    - **Direct URL:** https://github.com/Universal-Commerce-Protocol/conformance
    - **Publisher:** Universal Commerce Protocol project
    - **Published/updated:** live `main`; page-level update date not exposed
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** The official walkthrough runs the suite against the separately packaged UCP sample server. The repository provides tests but not a registry, plugfest report, or named independently governed implementation results. This is a bounded absence, not proof that no external implementation has ever tested itself.

### AP2

16. **VERIFIED — AP2 governance is transitioning into FIDO's Payments Technical Working Group, not yet represented by a published ratified AP2 standard.**
    - **Direct URL:** https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/
    - **Publisher:** FIDO Alliance
    - **Published/updated:** 2026-04-28
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** FIDO says Google contributed AP2 and Mastercard contributed Verifiable Intent as foundations for agentic-commerce specifications; both will be reviewed and further developed through FIDO's collaborative standards process in the Payments Technical Working Group. Authentication is handled by a separate Agentic Authentication Technical Working Group. This matches the digest's transition language and does not establish final ratification.

17. **VERIFIED — the current official FIDO evidence does not provide an AP2-specific TCK, certification profile, certified-product list, or interoperability result.**
    - **Direct URL:** https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/
    - **Publisher:** FIDO Alliance
    - **Published/updated:** 2026-04-28
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** FIDO describes contributions under review, says work has commenced, and notes FIDO's general capability to publish specifications and certify interoperable products. It publishes no AP2-specific test kit, vector corpus, certification program, product listing, or event result on the retrieved page; the bounded official-domain search found no such AP2 artifact. General FIDO certification capability must not be reported as AP2 certification.

### Agent Communication Protocol consolidation

18. **VERIFIED — BeeAI's Agent Communication Protocol states that it is now part of A2A under the Linux Foundation.**
    - **Direct URL:** https://github.com/i-am-bee/acp
    - **Publisher:** BeeAI contributors / Linux Foundation AI & Data
    - **Published/updated:** live repository; consolidation notice date not exposed
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** The official ACP repository displays the notice “ACP is now part of A2A under the Linux Foundation,” links a migration guide, identifies BeeAI as an LF AI & Data initiative, and exposes an Apache-2.0 license. This matches the digest's consolidation claim. The page does not prove that every pre-existing ACP deployment has migrated.

### Cross-standard profile/TCK inference

19. **VERIFIED — a third-party, version-pinned supplemental profile/TCK is technically and procedurally feasible, with explicit limits on what “official” means.**
    - **Direct URLs:** https://modelcontextprotocol.io/seps/2133-extensions ; https://github.com/modelcontextprotocol/conformance ; https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md ; https://github.com/a2aproject/a2a-tck/blob/main/README.md ; https://github.com/Universal-Commerce-Protocol/conformance
    - **Publisher:** Model Context Protocol project; A2A Project / Linux Foundation; Universal Commerce Protocol project
    - **Published/updated:** live policies/repositories; MCP SEP created 2025-01-21; page-level update dates otherwise not exposed
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** A2A expressly allows independent publication under third-party control; MCP permits externally governed unofficial extensions and native capability negotiation; all three conformance suites can target externally supplied systems; MCP provides revision selection/frozen requirement sets; UCP demonstrates explicit protocol-version and fixture inputs. Therefore an external profile can pin upstream versions, use native negotiation, and add only supplemental assertions. It cannot claim MCP/A2A official-extension status without the relevant upstream process, and MCP's SEP does not itself define profile grouping. The evidence supports feasibility, not adoption demand or a cross-standard official namespace.

### Guardrail red-team

20. **DISPUTED — “No transport” is necessary for a supplemental profile to avoid becoming a new core.**
    - **Direct URLs:** https://modelcontextprotocol.io/seps/2133-extensions ; https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md
    - **Publisher:** Model Context Protocol project; A2A Project / Linux Foundation
    - **Published/updated:** MCP SEP created 2025-01-21; A2A policy live `main`
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** MCP's official extension syntax uses `com.example/websocket-transport` as a valid example, and A2A formally governs Custom Protocol Bindings alongside extensions. These official sources show that a transport/binding artifact can be governed as an extension without automatically becoming a replacement core. “No transport” remains a defensible local scope choice, but the evidence does not establish it as necessary.

21. **OVERTURNED — every non-mechanical semantic change must first be proposed upstream before a provisional profile rule may exist.**
    - **Direct URLs:** https://github.com/OAI/OpenAPI-Specification/blob/main/CONTRIBUTING.md ; https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md ; https://modelcontextprotocol.io/seps/2133-extensions
    - **Publisher:** OpenAPI Initiative / Linux Foundation; A2A Project / Linux Foundation; Model Context Protocol project
    - **Published/updated:** live policies; MCP SEP created 2025-01-21
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** OpenAPI explicitly says it helps to use a proposed change as an `x-*` extension before a formal proposal. A2A allows independent development/publication before seeking hosted experimental or official status. MCP encourages experimental incubation and permits unofficial external extensions. The exact “upstream first” ordering is therefore contrary to the official incubation paths; “incubate externally, then upstream with implementation/adoption evidence” is what these sources support.

22. **UNVERIFIED — unknown upstream extensions must round-trip without loss as a generally available compatibility property.**
    - **Direct URLs:** https://modelcontextprotocol.io/seps/2133-extensions ; https://github.com/a2aproject/A2A/blob/main/docs/topics/extension-and-binding-governance.md
    - **Publisher:** Model Context Protocol project; A2A Project / Linux Foundation
    - **Published/updated:** MCP SEP created 2025-01-21; A2A policy live `main`
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** Both policies make extension implementation optional, opt-in, and outside core conformance. Neither retrieved policy requires generic implementations to preserve and round-trip unknown extension data. AgentBridge could impose that requirement on its own conforming bindings, but it is not an upstream-guaranteed property and must be tested rather than assumed.

23. **VERIFIED — bounded absence of an authoritative public cross-vendor mutual-interoperability pass matrix remains accurate, with an important MCP qualification.**
    - **Direct URLs:** https://github.com/modelcontextprotocol/conformance ; https://github.com/a2aproject/a2a-tck/blob/main/README.md ; https://github.com/Universal-Commerce-Protocol/conformance ; https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/
    - **Publisher:** Model Context Protocol project; A2A Project / Linux Foundation; Universal Commerce Protocol project; FIDO Alliance
    - **Published/updated:** live repositories; FIDO page 2026-04-28
    - **Accessed:** 2026-09-11
    - **Exact support/mismatch:** The official-domain sweep found executable harnesses and individual official MCP SDK tier-assessment issues containing revision-scoped conformance matrices. It also surfaced third-party A2A self-reports inside official project discussions. Neither is an authoritative cross-vendor matrix showing mutually interoperable independent products. No comparable official A2A/UCP multi-vendor pass registry or AP2 certification/result publication was found. The absence statement is therefore supported only in this bounded, official-source sense; it must not be shortened to “no public results exist.”

## Status counts

- **VERIFIED:** 18
- **DISPUTED:** 1
- **UNVERIFIED:** 2
- **OVERTURNED:** 2

