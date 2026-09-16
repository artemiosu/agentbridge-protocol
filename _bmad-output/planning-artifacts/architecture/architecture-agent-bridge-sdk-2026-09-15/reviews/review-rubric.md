---
title: AA-1 Architecture Spine — Good-Spine Rubric Review
reviewer: rubric walker (fresh context, internal I1 analysis)
date: 2026-09-16
scope: ARCHITECTURE-SPINE.md and the complete AA-1 companion package
verdict: redesign/block
---

# AA-1 Architecture Spine — Rubric Review

## Verdict

**`redesign/block` for AA-1 closure.** The package is architecturally strong, mechanically clean, broad enough for the Charter, operationally bounded, consistent about avoiding premature stack choices, and substantively consistent with AD-3A. It cannot yet receive an AA-1 pass because two previously mandatory closure properties are not demonstrated exactly: FR-99 bidirectional traceability still aggregates requirements whose governing links differ, and the FR-97 AA-1 landscape-learning control has no identified closure artifact/evidence. One additional high finding makes the adopted AD-3A diversity test gameable because its key qualifiers have no acceptance rule or downstream decision slot.

Finding count: **2 Blocker, 1 High, 2 Medium, 1 Low**. Mechanical lint: **0 findings**.

## Review basis and method

Reviewed:

- `ARCHITECTURE-SPINE.md`;
- `.memlog.md` decision evidence;
- `GLOSSARY-AND-CONTEXT.md`;
- `BASELINE-V0X-SCOPE.md`;
- `QUALITY-ATTRIBUTE-SCENARIOS.md`;
- `ASSETS-ADVERSARIES-BOUNDARIES.md`;
- `ALLOCATION-MATRIX.md`;
- `ADR-DISCIPLINE.md`;
- `OWNER-ARCHITECTURE-GUIDE.md`;
- `AA-1-GATE-RECORD.md`;
- the governing Architecture Assurance `SPEC.md` and `architecture-assurance-charter.md`.

The review applied the BMad good-spine checklist: real divergence points, enforceability of every AD's Binds/Prevents/Rule, Deferred safety, capability/spec coverage, inherited/approved-decision consistency, owned-dimension completeness, operational/environmental envelope, named-technology discipline, and package-level consistency. The deterministic `lint_spine.py` pass completed successfully with zero findings.

This is an internal I1 rubric review. It does not satisfy any later I2/I3 independence requirement and does not itself issue the AA-1 Gate verdict.

## Findings

### RBR-001 — Blocker — Claimed exact bidirectional traceability is still semantically aggregated

**Evidence.** `ALLOCATION-MATRIX.md` states that a range may be used only when every included ID has the same allocation and closure treatment, and AA1-F-002 requires splitting ranges whose allocation or verification differs. The normative allocation correctly splits, for example, FR-56–FR-67 into six materially different rows: FR-56–58, FR-59, FR-60, FR-61–63, FR-64–65 and FR-66–67. But the claimed bidirectional companion at line 274 collapses all FR-56–FR-67 into one owner/AD/QAS/boundary row. It similarly collapses FR-82–FR-95, and `7.2` groups NFR ranges whose normative allocations differ. Those broad rows assign the union of links to every contained requirement, so a reverse lookup cannot determine which exact AD/QAS/threat boundary governs an individual ID. This does not meet the document's own statement that every mandatory ID resolves exactly and leaves the earlier blocker AA1-F-002 not fully closed.

**Why it matters.** Separate AA-2–AA-7 units can implement or verify the wrong obligation while every mechanical ID appears present. This is precisely the divergence FR-99 and the AA-1 exit condition are intended to prevent.

**Required correction.** Split every `7.1` and `7.2` range at the point where owner, governing AD, QAS, asset or boundary linkage differs. If a range genuinely shares every field, retain it and document the equality check. Add a mechanical check that expands ranges and verifies one exact traceability record per ID, not merely ID presence.

**Closure test.** For each FR-1–FR-110 and NFR-1–NFR-29, an expanded machine-readable audit returns exactly one accountable owner and the exact applicable AD/QAS/AST/BND set; no ID inherits a union assembled for neighboring requirements. AA1-F-002 is then independently rechecked before being closed.

**Disposition:** fix before AA-1 Gate verdict.

### RBR-002 — Blocker — FR-97 has no identified AA-1 closure artifact or evidence

**Evidence.** `ALLOCATION-MATRIX.md` assigns FR-97 an **AA-1 control** and requires a versioned landscape review, reuse/defect/Bridge log and dependency-cut check. `AA-1-GATE-RECORD.md` lists two research reports as inputs, but neither the AA-1 artifact set nor another package file identifies the required decision log, pins which research revision supplied which mechanism/defect, or records the dependency-cut result. The package therefore allocates FR-97 but does not demonstrate its own first closure point.

**Why it matters.** Without the log, later designers cannot distinguish a consciously rejected mechanism from an unexamined one, and cannot audit whether a foreign runtime or provider dependency entered the Native path through an apparently convenient design choice.

**Required correction.** Add or identify a compact versioned landscape decision record. It should pin the reviewed evidence, record mechanism/reuse/defect/Bridge disposition, state the Native dependency-cut result, and name its recurring owner/revisit trigger. Add it to the Gate artifact/evidence set and trace FR-97 to it.

**Closure test.** A reviewer can start from FR-97 and reach a frozen artifact showing the exact research revisions, considered mechanisms, reuse/reject/defer decisions, Bridge candidates, and a passing dependency-cut check for AD-1–AD-3.

**Disposition:** fix before AA-1 Gate verdict.

### RBR-003 — High — AD-3A's diversity qualifiers are not objectively enforceable

**Evidence.** AD-3A consistently requires a “second distinct” Native Binding, “two unrelated” Domain Profiles, “two independently defined” Extensions, and Bridges to “two distinct” foreign protocol models. The language is repeated consistently in the spine, scope, matrix, Owner guide and Gate record. However, no artifact defines what differences are material, what common ancestry/control is permitted, when two Profiles are unrelated, what “independently defined” means for Extensions, or what makes two foreign models distinct enough to test translation independence. No Deferred/open slot assigns this qualification decision to AA-4, AA-5 or AA-6 before AA-7 execution.

**Why it matters.** Two teams can satisfy the words with cosmetically different artifacts that exercise the same assumptions, defeating the retained F6–F8 breadth test while still claiming pass.

**Required correction.** Add an enforceable preregistration rule or explicit downstream decision slot. It should define disqualifying shared assumptions and minimum diversity axes for Bindings, domain separation criteria for Profiles, authorship/specification independence for Extensions, and semantic-family separation for foreign protocol models. The rule should be frozen before AA-7 artifact selection and reviewed for conflicts of interest.

**Closure test.** Given any proposed AA-7 matrix, two reviewers using only the frozen criteria reach the same eligibility result for every Binding/Profile/Extension/Bridge candidate.

**Disposition:** fix now or defer explicitly to a named pre-AA-7 blocking decision with owner, Gate and revisit condition.

### RBR-004 — Medium — Capability map omits SPEC CAP-7 and CAP-8

**Evidence.** The governing `SPEC.md` defines CAP-1 through CAP-8. The spine's `Capability → Architecture Map` stops at CAP-6. CAP-7 Independent Implementability and CAP-8 Controlled Publication and Evolution are partly represented elsewhere by the conformance, genealogy, rights, Gate and evolution rules, but the explicit capability map does not say where they live or which ADs govern them.

**Why it matters.** A downstream planner reading the declared map can treat independent implementability and controlled publication/evolution as outside the architecture contract even though the package relies on both.

**Required correction.** Add CAP-7 and CAP-8 rows that point to the existing conformance/independence, rights/governance, artifact-lifecycle and Gate boundaries. This should not add new architecture or choose a stack.

**Closure test.** Every SPEC capability CAP-1–CAP-8 has one explicit row and resolves to existing decisions, artifacts and closure Gates.

**Disposition:** autofix before finalization.

### RBR-005 — Medium — The two architecture diagrams use opposite edge directions

**Evidence.** The AD-2 diagram draws `Profiles/Extensions/Bindings → Core` and the text says Core has no upward dependency. The later Structural Seed draws `Core → Profiles/Extensions/Binding contract`, while also saying it is a responsibility topology rather than a deployment prescription. Neither diagram labels what an arrow means.

**Why it matters.** Independent units can read arrows as dependency direction, data flow or normative refinement and derive opposite dependency rules. This undercuts AD-2's central purpose even though the prose itself is clear.

**Required correction.** Use one arrow convention throughout or label every diagram's edge semantics. Prefer dependency arrows toward the artifact depended upon, with refinement/realization edges separately styled and a legend.

**Closure test.** Both diagrams can be read without prose and yield the same Core dependency invariant: Core does not depend on Profiles, Extensions, Bindings, Bridges, SDKs, providers or conformance implementations.

**Disposition:** autofix before finalization.

### RBR-006 — Low — Gate-record status language is internally stale

**Evidence.** `AA-1-GATE-RECORD.md` marks `ALLOCATION-MATRIX.md` as “pending correction and freeze,” while the findings register says AA1-F-001, F-002 and F-006 are corrected and awaiting review confirmation. “Pending correction” and “corrected; confirmation pending” describe different states.

**Why it matters.** Reviewers may disagree about whether they are reviewing an uncorrected draft or validating a completed correction, weakening the closure record.

**Required correction.** Change the artifact-set status to “corrections applied; review confirmation and freeze pending” (or reopen the specific finding if correction is not complete).

**Closure test.** Artifact status, finding status and review evidence describe the same lifecycle state.

**Disposition:** autofix with Gate-record reconciliation.

## Good-spine checklist result

| Check | Result | Notes |
| --- | --- | --- |
| Real divergence points fixed | **Conditional pass** | AD-1–AD-3 correctly fix the major semantic-waist, layer-allocation and bounded-baseline forks. AD-3A eligibility remains underdefined (RBR-003). |
| Every AD has Binds / Prevents / Rule | **Pass** | All three spine ADs have the required fields; deterministic lint reports no mechanical defects. |
| Rules are enforceable and prevent stated divergence | **Conditional pass** | AD-1/AD-2 are backed by allocation, universality/removal and safety checks. AD-3A's diversity terms need objective acceptance criteria. |
| Deferred items are safe | **Pass** | Deferred choices have meaningful Gates/revisit conditions and do not authorize implementation, production or publication. No Deferred item silently permits incompatible Core meaning. |
| Named technology is verified/current | **Pass / not applicable** | No language, transport, encoding, framework, provider or cryptographic suite is selected. Named protocols are examples/research or deferred Bridge targets, not bound stack choices. |
| Brownfield consistency | **Not applicable** | This is a greenfield protocol architecture; no existing implementation is being ratified. |
| SPEC capability coverage | **Conditional pass** | Substantive rules cover the intent, but the explicit map omits CAP-7/CAP-8 (RBR-004). |
| Approved/inherited decisions preserved | **Pass** | AD-1, AD-2, AD-3 and AD-3A match the decision memlog; no local rule weakens them. |
| Every owned dimension decided/deferred/open | **Pass** | Semantics, boundaries, state/failure, authority, evidence, evolution, privacy, resource safety, conformance, implementation independence, governance/rights, deployment and claims are decided or assigned to explicit later Gates. |
| Operational/environmental envelope | **Pass** | Non-production execution is bounded by AD-21; resource/performance ceilings close at AA-4/AA-5; production deployment/domain/jurisdiction/SLO are explicitly outside v0.x and assigned to AA-9; provider strategy is replaceable and Native operation has no mandatory AgentBridge service. |
| Requirement allocation and verification | **Fail for closure** | Normative allocation is comprehensive, but exact reverse traceability and FR-97 closure evidence are incomplete (RBR-001, RBR-002). |
| No premature stack choice | **Pass** | Rust/Go, transport, wire/content format, canonicalization, crypto suite, repository structure and provider are explicitly hypotheses/deferred choices. |

## Positive conclusions retained

- The Semantic Hourglass paradigm is appropriately named and carries a coherent dependency model without turning Core into either a universal domain ontology or an empty envelope.
- Core/Profile/Extension/Binding/Bridge/External/SDK/Conformance responsibilities are substantively separated and protected by non-weakening, fail-closed and specification-primacy rules.
- The package covers every Charter-required quality family and preserves zero-tolerance SBC outcomes without inventing unsupported performance numbers.
- The operational boundary is unusually explicit for an AA-1 design: documentary work, controlled executable experiments, reference implementation, publication, production, adoption and standard-legitimacy claims remain separate permissions.
- AD-3A is textually consistent across the package: one mandatory open Base Binding, one additional experimental Native Binding, two unrelated non-production Domain Profiles, two independently defined Extensions, and optional mappings to at least two foreign models; none becomes a mandatory runtime dependency.
- No selected stack or hidden AgentBridge-operated service is introduced.

## Required closure order

1. Repair exact bidirectional traceability and revalidate AA1-F-002.
2. Supply the FR-97 AA-1 evidence artifact and dependency-cut result.
3. Make AD-3A eligibility objective or create an explicit blocking pre-AA-7 decision slot.
4. Complete the capability map and align diagram edge semantics.
5. Reconcile Gate-record statuses, rerun lint plus the rubric, then freeze revisions/digests and obtain the remaining required I1 reviews.

## Final re-review — 2026-09-16

### Verdict

**PASS under the good-spine rubric.** Residual rubric findings: **0 Blocker, 0 High, 0 Medium, 0 Low**. Deterministic spine lint also passes with **0 findings**.

This verdict closes this review lens only. `AA-1-GATE-RECORD.md` correctly remains `in-review`: exact package revisions/digests still have to be frozen, all required I1 lens verdicts and recusals must be recorded, corrected findings must be formally closed against their evidence, and the Gate owner must issue the final AA-1 verdict. This rubric pass grants no AA-2, prototype, implementation, publication or external-claim permission by itself.

### Closure of prior rubric findings

| Prior finding | Final result | Re-review evidence |
| --- | --- | --- |
| RBR-001 — exact bidirectional traceability | **Closed** | `ALLOCATION-MATRIX.md` §7.1 now has exactly 110 individual FR records and §7.2 exactly 29 individual NFR records; expansion check reports no missing or duplicate IDs. Grouped union rows were removed from the reverse index. |
| RBR-002 — missing FR-97 closure artifact | **Closed** | `LANDSCAPE-DISPOSITION.md` pins the two governing research dates and SHA-256 digests, records reuse/reject/defer/Bridge dispositions, gives a documentary Native dependency-cut PASS, and names the Architecture Research Lead plus refresh triggers. Recomputed source digests match the record. |
| RBR-003 — unenforceable AD-3A diversity terms | **Closed** | Proposed AD-22 in `AA-7-DIVERSITY-ELIGIBILITY.md` defines deterministic `eligible`/`ineligible` tests for materially distinct Bindings, unrelated domains, independently defined Extensions and distinct foreign semantic families; it includes provenance, shared-code, conflict and separated-review controls and blocks selection at AA-6 exit. The matrix, baseline, spine Deferred table and ADR register consistently point to this contract. |
| RBR-004 — CAP-7/CAP-8 omitted | **Closed** | The spine capability map now includes Independent Implementability and Controlled Publication and Evolution with their architecture locations, governing ADs and Gates. |
| RBR-005 — contradictory diagram arrows | **Closed** | Both diagrams now state that `A --> B` means A normatively depends on B and consistently point Profiles, Extensions, Binding/Bridge contracts, runtime realizations and assurance artifacts toward their dependencies. Core has no upward dependency. |
| RBR-006 — stale Gate status wording | **Closed** | The Gate artifact table now says allocation corrections are applied with review confirmation/freeze pending, matching the findings register and current `in-review` state. |

### Full-package rubric confirmation

- **Real divergence points:** AD-1–AD-3 fix the initiative-level paradigm, responsibility boundaries and bounded baseline. The new `AA-2-NORMATIVE-MODEL-CONTRACT.md` assigns the next-level identity/equality, authority, lifecycle/effect, evidence, composition, closure/negotiation, error and typed port seams to AD-4–AD-7 artifacts, owners, stable assertions and falsifiers, preventing Deferred from becoming implementation-defined behavior.
- **Enforceability:** every spine AD retains Binds/Prevents/Rule. The individual AD-1–AD-3 records now preserve alternatives, strongest contrary evidence, policy-versus-hypothesis boundaries, verification, consequences, residual risks, reopen paths and pinned evidence. Core minimality and completeness are explicitly falsifiable rather than asserted as already proved.
- **AD-3A consistency:** the spine, baseline, allocation matrix, Owner guide, Gate record and AD-22 consistently preserve one mandatory Base Binding plus the stricter experimental AA-7 matrix without converting experimental artifacts or Bridges into Core/runtime dependencies. AA-7 claims are scoped to exact tested artifacts and may falsify rather than universally prove breadth.
- **Capability and requirement coverage:** CAP-1–CAP-8 are mapped. FR-1–FR-110 and NFR-1–NFR-29 have exact reverse records; EI-01–EI-25 and SBC-01–SBC-10 remain individually allocated. The QAS set still covers every Charter-required quality family.
- **Operational/environmental envelope:** documentary AA-2, controlled execution under AD-21, AA-4/AA-5 resource and mechanism closure, AA-6 implementation authorization, AD-22 candidate eligibility, AA-8 publication, AA-9 production, and AA-10/AA-11 adoption/legitimacy remain distinct. Deployment/provider/operations dimensions are decided or explicitly Gate-bound.
- **Technology discipline:** no language, runtime, repository layout, transport, encoding, canonical representation, cryptographic suite/provider, cloud or foreign protocol version is selected prematurely. Named external technologies are pinned evidence inputs or optional future Bridge/mechanism candidates with refresh rules.
- **Dependency and evidence integrity:** the Native dependency-cut result is explicit; reference implementations remain non-normative; external systems stay replaceable and bounded; exact artifact/dependency closure, independent evidence, rights and blocker rules remain non-waivable.

### Verification performed

- `lint_spine.py`: `ok: true`, total findings `0`.
- Reverse traceability expansion: FR rows `110`, missing `[]`, duplicates `[]`; NFR rows `29`, missing `[]`, duplicates `[]`.
- Recomputed research SHA-256 values match the pins in the landscape and decision records, including the clean-slate, standards-gap and composition-challenger reports.
- Manual consistency sweep confirmed CAP-7/CAP-8, common arrow semantics, AD-22 references, Gate status language, evidence-bearing AD-1–AD-3 records and the AA-2 DVG-01–DVG-18 acceptance contract.

### Residual administrative closure, not rubric findings

The following are expected Gate-finalization work and do not indicate an architecture-spine defect: freeze exact artifact/input digests, record all required reviewer identities/independence/recusals and verdicts, reconcile every findings-register status with closure evidence, obtain the Project Owner's final nontechnical scope explanation, and issue the precise next-scope Gate verdict. Any failed required lens can still reopen this PASS.
