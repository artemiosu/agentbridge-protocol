# Reality-check review — AA-1 Architecture Spine

**Artifact reviewed:** `ARCHITECTURE-SPINE.md` with its AA-1 companions and current approved repository inputs
**Review date:** 2026-09-16
**Lens:** whether adopted decisions are demonstrably grounded in current evidence rather than asserted from memory, convention, or stale technology knowledge
**Verdict:** **redesign/block AA-1 passage pending evidence closure**

The selected direction is not rejected. The package is notably disciplined about deferring implementation technologies. The failure is narrower: AD-1–AD-3 are marked adopted before the package contains the decision records and evidence links needed to show that these particular architectural choices—not merely the Native Architecture-First product direction—were reality-checked.

## Counts

- Findings requiring correction: **3**
- Cleanly deferred technology/stack decision groups: **7**
- Current named-standard/version commitments requiring immediate live verification: **0**
- Named foreign protocols requiring live verification when a concrete Bridge is proposed: **MCP, A2A, UCP, AP2 and OpenAPI, plus any later candidate**

## Findings

### REALITY-01 — Adopted AD-1–AD-3 have no auditable options-and-evidence records

- **Location:** `ARCHITECTURE-SPINE.md` §§ AD-1–AD-3 (lines 42–88); `ADR-DISCIPLINE.md` §§ 3–5; `AA-1-GATE-RECORD.md` §§ 2, 4–7
- **Trigger condition:** AD-1, AD-2 and AD-3 are presented as adopted architectural decisions, but there are no individual ADRs recording realistic alternatives, evidence for and against each choice, source claim dates, consequences, residual risks, or verification/acceptance links. The package-local `.memlog.md` records owner acceptance, not the required technical reality check.
- **Evidence:** The project’s own ADR template requires “Options considered,” “failure modes and evidence,” verification, consequences, residual risks, and provenance (`ADR-DISCIPLINE.md`, lines 33–71). Its adoption process requires scoped independent review and a Gate-owner evidence verdict (lines 126–135). The current Gate record says input digests are pending, all four required review lenses are unrecorded/pending, and no verdict has been issued (`AA-1-GATE-RECORD.md`, lines 21–38, 64–75, 90–104). The spine frontmatter cites the SPEC/charter/PRD/baseline but does not cite either approved research report that the Gate record identifies as governing evidence.
- **Required correction:** Create or attach decision records for AD-1–AD-3 that satisfy the repository’s own ADR template. Pin the clean-slate and standards-gap research revisions/digests; map each material architecture claim to the supporting or contrary evidence; record the strongest alternatives; identify which claims are owner-selected policy versus source-backed technical inference; and obtain the required scoped reviews before changing the AA-1 Gate verdict.
- **Potential consequence:** A later reviewer cannot distinguish a consciously chosen constraint from a plausible-sounding architecture pattern supplied by prior model knowledge. AA-1 could appear evidence-backed while its central decisions remain unauditable.

### REALITY-02 — “Small, complete” universal Core is stated as settled before its key premise is proved

- **Location:** `ARCHITECTURE-SPINE.md` Design Paradigm and AD-1 (lines 34–46), AD-3 (lines 84–88), and `BASELINE-V0X-SCOPE.md` §§ 11–12
- **Trigger condition:** The spine says a “small, complete” waist exists and “owns every universal distinction” across all required topologies. The approved clean-slate research reaches a more limited conclusion: the native semantic Core is a hypothesis, its minimal boundary remains unproved, permanent composition has not been experimentally defeated, and thin-waist/composition tests are required (`research.md`, lines 23–33, 83–89, 283–319).
- **Evidence:** The companion scope later supplies sound Universality and Removal Tests and postpones actual integrated proof until AA-6/AA-7 (`BASELINE-V0X-SCOPE.md`, lines 305–332, 334–362). That safeguard does not cure the spine’s present-tense completeness assertion. The adopted course decision authorizes Native Architecture-First; it does not establish that this exact Core boundary is complete.
- **Required correction:** Keep the Native direction, but label the completeness/minimality of the semantic waist as a gated architecture hypothesis until the Universality, Removal, strongest-composition, cross-topology and independent-implementation evidence exists. Give failure of those tests an explicit AD-1/AD-2 re-open or reallocation outcome. Define “capability-secure federated state machines” as a design target unless and until the normative model and security evidence make it a verified property.
- **Potential consequence:** AA-2 may elaborate a presumed Core rather than test whether a common Core boundary exists, making later contrary evidence expensive to accept and encouraging a fat Core or an empty envelope.

### REALITY-03 — A finite AA-7 matrix is described as proving retained breadth

- **Location:** `ARCHITECTURE-SPINE.md` AD-3 (line 88); `BASELINE-V0X-SCOPE.md` § 13 (lines 350–362)
- **Trigger condition:** The spine says AA-7 “proves the retained breadth” through one additional Binding, two Domain Profiles, two Extensions and at least two Bridge mappings. This matrix is useful falsification and coverage evidence, but it cannot prove universal breadth across future domains, topologies, mechanisms, adversaries or foreign protocol revisions.
- **Evidence:** The companion correctly limits AA-7 to an internal non-production Implementation/Interop Candidate and denies production, adoption and standard-status claims. The wording in the spine is therefore stronger than the package’s own claim boundary.
- **Required correction:** Replace “proves the retained breadth” with a scoped statement such as “tests the retained breadth against the frozen AA-7 matrix and may falsify the claimed boundary.” Require every resulting claim to name the exact tested Profiles, Bindings, Extensions, Bridges, environments and dependency versions.
- **Potential consequence:** A bounded experiment can be cited later as universal validation, bypassing the artifact’s otherwise careful claim-scoping discipline.

## Reality checks that pass

1. **No hidden language/runtime selection.** Rust, Go and any other reference stack remain explicitly deferred to AA-4 evidence and AA-6 scope selection (`ARCHITECTURE-SPINE.md`, lines 163, 183–185).
2. **No hidden transport or encoding selection.** Transport, encoding, canonicalization, cryptographic suites, performance budgets and repository structure remain unselected and have explicit revisit Gates (lines 163, 181–187).
3. **No stale version assertion in the spine.** MCP, A2A and UCP are named only as examples of future optional Bridges; no current version, status or capability claim is imported into the adopted architecture (line 188). `BASELINE-V0X-SCOPE.md` also excludes a preselected full Bridge set.
4. **A current freshness policy exists in the approved research.** Dynamic MCP/A2A/OpenAPI/Arazzo/UCP/ACP/AP2 compatibility claims were checked on 2026-09-12 and carry a 2026-10-12 refresh deadline (`research.md`, lines 353–365). Concrete Bridge work must verify the then-current official, versioned artifacts rather than reuse these snapshots after their expiry.
5. **Technology deferral is deliberate, not missing work.** The package names owners, Gates and revisit conditions and ties technology choices to prototypes, measurements, threat analysis and conformance evidence. This is a valid PASS under this lens.
6. **The original prompt’s prescribed stack is not silently adopted.** JSON-only, Python/FastAPI, microservices, PostgreSQL/Redis, Kafka/RabbitMQ, AWS/GCP and the original three-SDK plan do not leak into the AA-1 architecture as commitments.
7. **The Native-path independence claim is a requirement, not a market fact.** The package treats absence of mandatory cloud, registry, broker, commercial SDK and foreign runtime as a conformance property to be tested, not as an already demonstrated ecosystem claim.

## Closure test

This review can pass when individual evidence-bearing records for AD-1–AD-3 are pinned to exact inputs, the universal-Core language is made explicitly conditional on its scheduled falsification evidence, the AA-7 claim is scoped to the frozen test matrix, and the required AA-1 reviewers have issued traceable verdicts. No language, transport, encoding, cryptographic suite, repository starter, cloud stack or foreign-protocol version needs to be selected to close this review.

## Final re-review — 2026-09-16

### Verdict and residual counts

**Verdict: redesign/block under the reality-check lens pending one package-wide claim-boundary correction.**

- Prior findings fully closed: **2** (`REALITY-01`, `REALITY-02`)
- Prior findings partially closed: **1** (`REALITY-03`)
- New independent findings: **0**
- Residual reality blockers: **1** (stale AA-7 proof language in three package summaries)
- Current named-standard/version commitments requiring immediate live verification: **0**
- Hidden starter, language, runtime, transport, encoding, cloud or provider selections: **0**

This is a content verdict for this lens, not the final AA-1 Gate verdict. Package freezing, exact final digests, reviewer identity/independence/recusal records, reconciliation of findings and the Gate-owner verdict remain administrative Gate closure work recorded in `AA-1-GATE-RECORD.md`; they are not counted again as reality findings here.

### Closure of the original findings

| Finding | Final status | Re-review evidence |
| --- | --- | --- |
| `REALITY-01` — adopted AD-1–AD-3 lacked auditable records | **Closed for content** | `decisions/AD-1.md`, `AD-2.md` and `AD-3.md` now use the required ADR structure, distinguish Owner policy from technical hypothesis, record strongest alternatives and contrary evidence, state consequences/residual risks, and define verification and reopen criteria. They pin the approved clean-slate report at SHA-256 `a4004d4ddcd2e11430233b0f8cc00001711a37f2cdda8b6751bd70fbf111baba` (evidence current 2026-09-12), standards-gap report at `e4ab81f7cb4174fea55385f9105a855861ef3cd37f6b1b6d2a236fce2bb42005` (researched 2026-09-11, updated 2026-09-12), and, where used, OQ-1 at `fd562fd950db82d8da97cba04dee2d4d20d8a7f08596577105031e008f94496b` (evidence current 2026-09-13). `LANDSCAPE-DISPOSITION.md` separately pins the governing landscape inputs, records dispositions and gives explicit refresh triggers. |
| `REALITY-02` — universal Core completeness was asserted as settled | **Closed** | `ARCHITECTURE-SPINE.md` now calls the exact minimal boundary a falsifiable hypothesis and the security/federation properties design targets pending AA-2–AA-7 evidence. AD-1 limits later support to the exact tested scope and requires counterexamples to reopen or reallocate the decision. `AA-2-NORMATIVE-MODEL-CONTRACT.md` supplies named normative artifacts, assertions, falsifiers and acceptance boundaries rather than treating the model as already proved. |
| `REALITY-03` — finite AA-7 matrix was described as universal proof | **Partially closed; one blocker remains** | The normative spine, AD-3/AD-3A and `AA-7-DIVERSITY-ELIGIBILITY.md` now correctly make AA-7 a preregistered, scoped falsification test whose claims name exact artifacts, environments and versions. That correction is not yet consistent across the entire package; the residual finding below must be resolved before this lens can pass. |

### Residual blocker — AA-7 proof language survives outside the corrected normative records

- **Location:** `OWNER-ARCHITECTURE-GUIDE.md` lines 80 and 129; `BASELINE-V0X-SCOPE.md` line 247; `.memlog.md` line 12. The final rubric review also claims at line 168 that the Owner guide already uses scoped, non-proof language, which these passages contradict.
- **Trigger condition:** the Owner guide says the AA-7 matrix is needed to “prove” that Core is not tied to the first transport/domain/foreign protocol and that two implementations “practically prove” interoperability, resilience and performance. The baseline says the second Binding “proves” transport independence, and the decision memory repeats that claim. A finite two-Binding/domain/Extension/Bridge matrix can support or falsify only the exact frozen closure; it cannot prove the general independence or qualities those summaries imply.
- **Required correction:** align all three summaries with AD-3 sections 6–7 and AD-3A: AA-7 **tests and may falsify** the claimed boundary; successful evidence supports only exact, named implementations, Bindings, domains, Extensions, Bridge mappings, environments and dependency versions. Reconcile the rubric review's package-consistency statement after the source wording is fixed.
- **Potential consequence:** the most accessible Owner and baseline documents can be cited later as authority for a broader claim than the actual evidence permits, bypassing the corrected ADR's claim ceiling.

### Confirmed reality checks

No selected implementation technology or current foreign-protocol version is hidden in the updated package. Rust, Go, HTTP/2, HTTP/3, JSON, CBOR, cryptographic suites, repository structure, providers and concrete Bridge targets remain explicit hypotheses or deferred choices with owners, Gates and refresh/reopen conditions. This deliberate deferral **passes** the lens. The AA-2 contract allocates the semantic work without selecting an implementation stack, and the proposed AA-7 eligibility contract makes candidate diversity deterministic without claiming its finite sample is universally complete.

## Targeted final re-check — 2026-09-16

**Verdict: PASS under the reality-check lens. Residual reality blockers: 0.**

The sole blocker from the preceding re-review is closed:

- `OWNER-ARCHITECTURE-GUIDE.md` lines 80 and 129 now describe AA-7 as an attempt to expose hidden coupling and as a bounded check whose result supports or contradicts only the exact frozen matrix; they explicitly deny universality for future cases.
- `BASELINE-V0X-SCOPE.md` line 247 now says the second Binding can expose first-transport coupling and provide scoped supporting evidence, but cannot prove independence across future Bindings.
- `.memlog.md` line 13 is an explicit append-only claim-boundary clarification for the earlier AD-3A shorthand: AA-7 is scoped falsification/supporting evidence, every result names the exact tested closure and assumptions, and success cannot prove independence or fitness outside it. Read as the later controlling constraint in the decision log, it aligns line 12 with AD-3 rather than leaving an unbounded proof claim.

These statements now agree with `decisions/AD-3.md`: finite AA-7 evidence may support, contradict or leave the exact tested claim inconclusive, but never establishes universal breadth. No further live technology/standard verification or stack selection is required for this review. Administrative AA-1 Gate finalization remains outside this lens verdict.
