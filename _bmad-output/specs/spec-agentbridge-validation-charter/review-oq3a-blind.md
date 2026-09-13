# Independent blind review — AgentBridge Validation Charter OQ-3A

**Review status:** FAIL / revise before freeze  
**Finding count:** Critical 0 · High 8 · Medium 3 · Low 1

## Scope and information barrier

This review evaluates only the frozen PRD, the frozen EI-01–EI-25 Evaluation Invariant Ledger, and the proposed OQ-3A oracle/outcomes, scenario corpus, observer/holdout, and experiment-matrix documents. No candidate manifest, allocation decision, Candidate N/C design, implementation artifact, private source document, or prior review discussion was consulted.

The proposal is substantially candidate-neutral: it specifies abstract roles and outcomes rather than candidate objects, separates authority/execution/effect, rejects forbidden effects independently of aggregate scores, uses synthetic data, and establishes a sound public/holdout split. It is not yet logically or operationally closed enough to freeze. The defects below can be corrected without choosing implementation technology or candidate architecture.

## Findings

### H-01 — The Outcome Tuple has no total conformance-verdict function and is not a closed algebra

**Severity:** High  
**Location:** `oracle-and-outcomes.md` §§1–3; `scenario-corpus.md` §4; `experiment-matrix.md` OQ-3A acceptance criteria

The documents define tuple fields, allowed and forbidden sets, and several safety implications, but never define a total function from observations to exactly one cell disposition. They also do not resolve all important cross-field combinations. Examples include `compatibility=incompatible|unknown` with `progress=continue`, a current-attempt `execution=not-started` with a pre-existing operation effect, `authority=non-permit` with non-protected execution, and obligations whose enforcement phase is not the phase represented by the tuple. `disclosures/effects` are called exact sets, but equality versus subset matching and the treatment of pre-existing versus newly caused effects are not stated. The 39 holdouts likewise have no frozen aggregation rule stating how pass, fail, invalid, and inconclusive cells affect the candidate and Gate 1.

This leaves room for contradictory tuples, selective treatment of invalid cells, and post-hoc interpretation of an outcome as pass.

**Minimal fix:** Add a normative, candidate-neutral verdict function that:

1. defines the temporal subject of every field (operation state, attempt state, or observed delta in the vector window);
2. defines tuple/set matching, including empty, pre-existing, and newly caused disclosure/effect sets;
3. enumerates permitted cross-field relations for compatibility, authority, progress, delivery, execution, effect, obligations, and assurance, with an explicit rule for any intentionally unconstrained relation;
4. yields exactly one of `pass`, `candidate-fail`, `run-invalid`, or `inconclusive` for every possible observation record;
5. makes an allowed-set match necessary but not sufficient for pass: all required evidence, safety projection, and metamorphic relations must also pass; and
6. freezes cell/tranche/Gate aggregation, including that invalid mandatory cells cannot be dropped and cannot support a positive Gate decision.

### H-02 — Candidate defects can be classified as observer invalidity

**Severity:** High  
**Location:** `observers-and-holdout.md` §§1–4; `oracle-and-outcomes.md` cross-field consistency

Several observer consequences say that missing or ambiguous information produces `invalid`: OBS-01 transcript ambiguity, OBS-02 inability to prove a decision, OBS-08 unavailable trace, and OBS-09 missing provenance. Those conditions can arise because the observer failed, but they can also arise because the candidate failed to emit a required, unambiguous, externally observable result or evidence. The present text does not distinguish the two causes. A candidate could therefore turn a conformance failure into an excluded/invalid run by withholding or confusing mandatory evidence.

**Minimal fix:** Freeze a source-attribution rule before execution. A failure independently induced in or demonstrated by the harness/observer is `run-invalid`; failure of a qualified observer to receive candidate-required output because the candidate omitted, corrupted, equivocated about, or made it uncorrelatable is `candidate-fail`. Require outcome-blind adjudication for disputed attribution, retain both raw paths, and make unresolved attribution `inconclusive` for the claimed scope rather than removable evidence. Apply the rule consistently to every OBS consequence.

### H-03 — The 65-cell matrix is a useful skeleton but lacks a closed obligation/failure coverage universe

**Severity:** High  
**Location:** `scenario-corpus.md` §§2–5; `oracle-and-outcomes.md` §5; PRD SM-5 and §§11.4, 11.7.2

One vector per `VC × P/N/F/A/M` cell cannot establish coverage of all EI falsifiers and PRD safety obligations, and the proposal correctly says additional vectors are required. However, it does not freeze the inventory or equivalence rule that determines what counts as a distinct obligation or failure mechanism. Many cells contain alternatives that one minimum vector cannot cover (for example unknown mandatory **or** collision, subject mutation **or** expiry), while DS-6 omits explicit operators/grammars for several required threat or conditional-hazard classes, including controller/credential/participant/intermediary compromise, collusion limits, enumeration, false-evidence provenance, numeric/Unicode ambiguity, bounded decompression/parsing, and replayable early/fallback behavior when applicable. The EI route table is at whole-EI granularity and therefore cannot prove PRD SM-5's zero silently untested MUST-level obligations.

**Minimal fix:** Before candidate design, freeze a candidate-neutral atomic coverage inventory derived from each EI pass/falsifier clause and each applicable FR/NFR safety obligation and threat category. Give every atom an ID, required positive/negative/boundary/adversarial/state/fault/metamorphic coverage types, and an equivalence rule for justified consolidation. Extend the operator/data grammar for every applicable atom or record a preregistered exclusion that narrows the claim. OQ-3B must show zero uncovered mandatory atoms; 65 remains only the structural floor.

### H-04 — OQ-3B may create exact public cases and expected outcomes after candidate design without a complete information barrier

**Severity:** High  
**Location:** `scenario-corpus.md` §5; `observers-and-holdout.md` §§6, 8; `experiment-matrix.md` OQ-3 split; PRD §§11.5 and 11.7.2

OQ-3A is required before Candidate N/C design, while OQ-3B is required only after OQ-4/OQ-5/OQ-8 and before code. The texts do not prohibit candidate design from occurring between them, nor do they prevent the OQ-3B public-vector author, Oracle Author, or cross-checker from seeing candidate designs. A person who knows the candidate designs can select exact schedules, boundaries, and allowed sets that favor or disfavor a candidate even while using neutral vocabulary. The holdout rule blocks advocates/implementers from exact cases but does not impose the same barrier on oracle personnel or their communications.

**Minimal fix:** Freeze one of two permitted sequences: (a) complete and cross-check all OQ-3B exact public vectors and sealed holdout outcomes before any candidate design; or (b) allow later deterministic generation only by personnel and processes demonstrably blind to candidate designs, outputs, comparative metrics, and advocate communications. In either sequence, all degrees of freedom affecting case selection or outcomes must be fixed before candidate visibility. Define OQ-4/OQ-5/OQ-8 as typed parameter inputs to that rule, require atomic OQ-3B preregistration after those inputs freeze, and prohibit code until the complete OQ-3B manifest is accepted.

### H-05 — The 39-case holdout has no frozen coverage rationale or decision role

**Severity:** High  
**Location:** `observers-and-holdout.md` §6; PRD §§11.7.2–11.7.3 and SM-3/SM-7

Three cases per VC guarantee family count but not the preregistered `risk × feature × topology × mode × domain` coverage required by the PRD. “At least one safety/privacy boundary per family” does not control how EI obligations, high-risk operators, topology permutations, or compound interactions are represented. No precision rationale is given for 39, and no frozen rule says whether a non-safety holdout miss fails conformance, narrows a claim, triggers replication, or is interpreted statistically. This permits post-hoc weighting of a favorable or unfavorable draw.

**Minimal fix:** State the holdout's estimand and decision role, then freeze either a deterministic covering rule or sampling probabilities plus minimum quotas over the required risk dimensions and atomic coverage inventory. Add a near-duplicate rule and a sample-size/precision rationale appropriate to that role. Freeze the disposition of safety failures, ordinary conformance misses, invalid cases, and incomplete strata before opening; statistical thresholds may be supplied by OQ-5/SAP, but their parameter slots and no-discretion application must be fixed in OQ-3A.

### H-06 — Seed selection, privileged access, and contamination handling are not sufficiently manipulation-resistant

**Severity:** High  
**Location:** `observers-and-holdout.md` §§6–7

The Evidence Custodian performs “one fixed draw,” but the rule does not prevent seed shopping, define verifiable entropy contribution, enumerate technical reroll predicates, or require a witness independent of outcome owners. Oracle Author and cross-checker necessarily learn exact hidden outcomes, yet no role-incompatibility, communication restriction, recusal, or access-compartment rule prevents leakage to advocates/implementers. “Equivalent hint” is undefined. Finally, contamination may invalidate a “cell/run” without a frozen scope rule; after seeing results, this could be used to discard a failed subset or replace it selectively.

**Minimal fix:** Require a precommitted, auditable, outcome-independent seed ceremony with at least two independent contributions or an equivalently non-selectable source; enumerate structural reroll predicates and prohibit result-based redraws. Precommit generator identity and validation predicates before the seed. Define privileged-role incompatibilities, least-access boundaries, communication/access logging, and recusal. Define objective contamination classes and outcome-blind adjudication; a contaminated primary case remains in the historical primary result, is never selectively replaced, and triggers a preregistered cell/tranche/full-holdout scope plus a separately versioned replication tranche.

### H-07 — Dual observation does not rule out common-mode false zero, and calibration has no frozen acceptance thresholds

**Severity:** High  
**Location:** `observers-and-holdout.md` §§1–4; PRD §10.1 and NFR-22

The two required paths may still share the same underlying event source, instrumentation boundary, correlation logic, storage, controller, or failure mode. For example, an authoritative simulator ledger and an “independent” commit counter can both miss an effect if both are fed after the same faulty hook. The catalog also gives only one minimum source for most non-effect safety semantics. Calibration lists excellent test classes but does not define coverage per channel/commit point, detection sensitivity, false-positive/false-negative bounds where measurements are statistical (notably timing/linkability/privacy and sampled resource metrics), required repetitions, or independent approval.

**Minimal fix:** Define observational independence by failure domain: no single untested source, transformation, correlation key, storage path, or administrative control may suppress or fabricate both paths. Require canaries injected at or below each declared effect/disclosure/egress boundary and commit point. For every OBS/vector, freeze qualification cases, repetitions or deterministic completeness proof, sensitivity/error tolerances where relevant, continuity and saturation limits, expiry, and approver independence. A failed or stale qualification blocks dependent cells.

### H-08 — Candidate-specific applicability can shrink comparative scope asymmetrically

**Severity:** High  
**Location:** `oracle-and-outcomes.md` Oracle Record `applicability`; `scenario-corpus.md` vector template item 11; PRD FR-99–FR-101

The oracle says the same inclusion/exclusion rule applies to N and C, but the vector template requests an applicability/exclusion rationale “for each candidate.” It does not say what happens when one candidate declares a required vector not applicable. A candidate could avoid a hard case by narrowing its claim while the other candidate is tested, and the remaining results could still be compared outside a common support set.

**Minimal fix:** Freeze a common mandatory claim scope independent of candidate capabilities. Within that scope, a candidate's inability to represent or execute a vector is a candidate result, not `not-applicable`. Permit `not-applicable` only from a candidate-neutral vector predicate fixed before candidate design; apply it symmetrically to the comparison or remove the vector from both candidates' comparative estimand. Record any candidate-elected narrower scope as a claim limitation and prevent it from satisfying Ledger/full-candidate eligibility.

### M-01 — Metamorphic coverage is assigned at EI level but does not exercise several security-critical monotonicities

**Severity:** Medium  
**Location:** `oracle-and-outcomes.md` §§4–5; `scenario-corpus.md` matrix `M01`

The 18 properties cover many important relations, but several EI assignments do not test the cited guarantee. EI-08 has no direct revoke/expiry/policy-change-at-commit transformation; EI-12 lacks a transformation showing compensation is a separately authorized operation and cannot rewrite history; EI-21's MP-02 does not test removal/failure of a claimed-optional or hidden-mandatory dependency; and EI-25 lacks direct observer-removal/non-interference and threat/claim-invalidation transformations. MP-05 does not state whether commit count is lifetime operation count or current-run delta, and MP-14's “below ceiling meaning is preserved” is undefined for parameters whose valid behavior legitimately changes below the maximum.

**Minimal fix:** Add or refine a small set of transformations for commit-boundary revocation/expiry, removal/tightening of approval or obligation, separately authorized compensation, dependency removal/failure, observer blindness/non-interference, and claim invalidation after a new threat/artifact. Define MP-05's count scope and MP-14 as a monotone parameterized boundary relation with exact pre-boundary invariants and at-boundary safe-stop. Map metamorphic coverage to atomic obligations, not only whole EIs.

### M-02 — The OQ-3A document set lacks atomic versioning, precedence, and change-impact control

**Severity:** Medium  
**Location:** all four OQ-3A proposal documents; `oracle-and-outcomes.md` §§1 and 6

Individual Oracle Records are versioned, but the proposal does not define one immutable OQ-3A set version/digest, normative precedence when the four files disagree, dependency identities for the EI/OQ-4/OQ-5/OQ-8 inputs, or the invalidation scope of changes to grammars, observers, coverage rules, or holdout generation. Versioning only allowed/forbidden sets and metamorphic relations is too narrow: a changed observer completeness rule, operator grammar, applicability predicate, sampling quota, or generator can change the exam just as materially.

**Minimal fix:** Add one OQ-3 manifest containing exact file and dependency identities, freeze authority/date, normative precedence/conflict rule, and a change taxonomy. Any semantic change to tuple algebra, coverage inventory, vector/operator grammar, applicability, observer qualification/completeness, sampling/generation, sealing, or decision mapping must create a new preregistration, identify affected prior evidence, and receive blind review before confirmatory use.

### M-03 — `evidence_assurance` is not comparable until its partial order is itself frozen

**Severity:** Medium  
**Location:** `oracle-and-outcomes.md` §2 and cross-field consistency; MP-06, MP-11, MP-13

The field permits an element of an explicitly defined Profile/Binding partial order, but no candidate-neutral minimum properties, comparison rule, incomparable-case rule, or version identity for that order are required in the Oracle Record. If candidates or later profile work define different orders, “assurance cannot increase” is not mechanically decidable and can import candidate-specific semantics after outcomes are known.

**Minimal fix:** Require each applicable vector to reference a frozen, versioned assurance lattice/partial order expressed in candidate-neutral properties and shared by both candidates for that claim. Define comparison of incomparable elements and mapping loss; absent such an order, assurance is `unknown` and no assurance-preservation or superiority claim may pass.

### L-01 — “Public corpus” conflicts with the PRD's private/shared-versus-published distinction

**Severity:** Low  
**Location:** `scenario-corpus.md` title, classifications, and §3; `observers-and-holdout.md` §§6 and 8

The PRD explicitly distinguishes equally available private Gate 1 materials from artifacts approved for public release. Calling the visible confirmatory set `public` and using `confirmatory-public` can be read as authorizing publication or as imposing public availability before the Project Owner's publication decision.

**Minimal fix:** Rename this class `shared-confirmatory` (or define “public-to-implementers” unambiguously) and reserve `public/published` for artifacts separately approved under the PRD publication controls.

## Explicit review conclusions

- **Candidate neutrality:** Substantively achieved at the vocabulary and mechanism level. H-04 and H-08 must be fixed to prevent neutral-looking exact-case selection or applicability rules from introducing candidate bias.
- **Outcome algebra and cross-field consistency:** Not yet complete; H-01 is freeze-blocking. The distinction among authority, execution, effect, and uncertainty is strong, but the temporal scope and total verdict mapping are missing.
- **Metamorphic rules:** Broad and useful, but not complete enough to substantiate the stated EI-wide coverage claim; see M-01.
- **65-cell template:** Sufficient as a minimum structural template, not as a coverage proof. OQ-3B needs the atomic closure rule in H-03.
- **Observers:** The catalog, positive controls, canaries, and non-interference intent are strong. False-zero resistance remains incomplete until H-02 and H-07 are resolved.
- **39-case sealed holdout:** The fixed family count and single/compound/permutation split are clear, but coverage, decision role, seed integrity, privileged access, and contamination disposition are not yet sufficiently frozen; see H-05 and H-06.
- **OQ-3A/OQ-3B sequencing:** The split is defensible only with the explicit information barrier and dependency-resolution sequence in H-04. OQ-3A alone must not authorize candidate design or code while outcome-affecting OQ-3B choices remain discretionary.
- **False pass / false zero / oracle leakage / post-hoc change resistance:** The proposal has good foundations but does not yet close these risks because of H-01, H-02, H-04, H-06, and M-02.

## Verdict

**Do not freeze OQ-3A in its current form.** Resolve all eight High findings and re-run blind review. The three Medium findings should also be corrected before freeze because they affect claimed coverage and reproducibility; the Low finding is editorial-governance hygiene.

## Re-review after fixes — 2026-09-13

### Scope and result

This was a bounded independent re-review of the revised OQ-3A documents, the new `atomic-coverage-inventory.md`, and every original finding above. The original information barrier remained in force: no candidate manifest, allocation decision, Candidate N/C design, implementation artifact, private source document, or prior discussion outside this review was consulted.

**Remaining findings:** Critical 0 · High 0.  
**Disposition of original findings:** Resolved 12 · Partially resolved 0 · Unresolved 0.  
**Regression result:** No new Critical or High requirement-level defect found.

### Per-finding resolution

| Finding | Status | Re-review evidence |
| --- | --- | --- |
| H-01 | **Resolved** | `oracle-and-outcomes.md` now defines temporal scope and causal deltas, exact scalar/set matching, additional cross-field constraints, a total four-way cell-verdict function, and mandatory tranche/Gate aggregation. Unspecified cross-field freedom must be expressed in the exact allowed set rather than treated as a wildcard. |
| H-02 | **Resolved** | The observer catalog is expressly subordinate to a source-attribution rule. Independently proven harness failure is `run-invalid`; qualified observation of candidate omission/corruption/equivocation is `candidate-fail`; disputed attribution is outcome-blindly adjudicated and unresolved disputes become non-positive `inconclusive` evidence. |
| H-03 | **Resolved at OQ-3A contract level; execution verification required in OQ-3B** | `atomic-coverage-inventory.md` atomizes each EI pass condition and falsifier, defines coverage types and a non-combinability rule, extends the threat/operator grammar, and mandates a stable clause-level FR-1–FR-95/NFR index with two-reviewer closure and zero uncovered applicable MUST/safety atoms before OQ-2B or Candidate design. The 65 cells are explicitly only a structural floor. |
| H-04 | **Resolved** | `scenario-corpus.md` and `experiment-matrix.md` now require complete, atomically preregistered OQ-3B after frozen OQ-4/OQ-5/OQ-8 inputs and before OQ-2B, Candidate design, or code. Oracle authors/reviewers cannot see candidate designs, and a barrier violation invalidates confirmatory use. |
| H-05 | **Resolved at OQ-3A contract level; parameter verification required in OQ-3B/OQ-5** | The holdout now has a bounded estimand, structural rationale for 39 cases, stratified covering rule over risk/atom/feature/topology/mode/domain, nonzero mandatory-stratum quota/probability, near-duplicate definition, and frozen dispositions for safety misses, ordinary misses, and invalid/inconclusive strata. Exact probabilities and thresholds must be preregistered before the seed and Candidate design. |
| H-06 | **Resolved** | The holdout contract now requires outcome-neutral multi-party or non-selectable entropy, commitments and an independent witness, precommitted generator/validation predicates, enumerated structural-only rerolls, privileged-role incompatibilities, least-access compartments, immutable access/communication logs, defined derived hints, outcome-blind contamination adjudication, fixed contamination scope, preservation of the primary result, and separate replication rather than selective replacement. |
| H-07 | **Resolved at OQ-3A contract level; qualification evidence required in OQ-3B/OQ-5** | Observational independence is now defined by common failure domain, with canaries at or below each boundary/commit point. Each observer/channel/commit point must preregister qualification cases, completeness proof or repetitions, sensitivity/error tolerances, continuity/saturation limits, expiry, scope, and independent approval; failed or stale qualification blocks dependent cells. |
| H-08 | **Resolved** | A common mandatory claim scope and frozen candidate-neutral applicability predicate are now required. Candidate inability inside that scope is a result, not `not-applicable`; exclusions are symmetric, and a candidate-elected narrower scope cannot satisfy full Ledger eligibility. |
| M-01 | **Resolved** | MP-19–MP-24 add direct commit-boundary revocation, approval/obligation tightening, separately authorized compensation, dependency removal/failure, observer common-mode/non-interference, and claim invalidation tests. MP-05 and MP-14 now define count and boundary semantics, and atomic coverage is delegated to the inventory/closure report. |
| M-02 | **Resolved at OQ-3A contract level; manifest verification required at freeze/OQ-3B** | The oracle now requires one immutable OQ-3 set manifest, exact dependency identities, authority/date, normative precedence, conflict blocking, semantic-change taxonomy, evidence-impact declaration, new preregistration, and blind re-review. |
| M-03 | **Resolved** | Every applicable oracle must reference one candidate-neutral, frozen, versioned assurance partial order shared by both candidates, including comparison, incomparability, mapping loss, and least-known behavior; absence or incomparability yields `unknown` and cannot support preservation/superiority. |
| L-01 | **Resolved** | The corpus and classification are now `shared-confirmatory`, and the text explicitly limits “shared” to equal private Gate 1 access while reserving publication for separate Project Owner approval. |

### Regression check

The fixes remain candidate-neutral and do not introduce implementation, wire-format, transport, language, or candidate-architecture assumptions. The new precedence and change-control rules close rather than reopen post-hoc discretion. The enlarged metamorphic and operator sets remain derived from frozen EI/PRD obligations. The strengthened observer and holdout controls do not reveal hidden cases to future implementers. Requiring OQ-3B before OQ-2B/Candidate design removes the earlier oracle-selection leakage window and is consistent across the revised scenario, observer, and experiment-index documents.

The OQ-3B-dependent items noted above are not residual defects in OQ-3A; they are explicit future evidence gates. They must be checked for actual completion—especially the PRD clause index, exact coverage closure, assurance orders, observer qualification thresholds, holdout strata/probabilities, and immutable set manifest—before OQ-2B, Candidate design, or code. Failure to fill any required slot leaves the project at No Start.

### Re-review verdict

**PASS for presentation to the Project Owner for informed re-acceptance and OQ-3A freeze.** OQ-3A is now candidate-neutral, logically sufficient as an exam-generation contract, and adequately resistant at requirement level to false pass, false zero, oracle leakage, holdout manipulation, and post-hoc rule changes.

This verdict freezes no artifact by itself and does not authorize OQ-2B, Candidate N/C design, or experimental code. Those remain blocked until the complete OQ-3B package and its OQ-4/OQ-5/OQ-8 inputs satisfy the revised pre-design gates.
