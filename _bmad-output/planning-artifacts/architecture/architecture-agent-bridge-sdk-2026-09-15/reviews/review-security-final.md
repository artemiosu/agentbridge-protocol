# AA-1 final security, privacy, authority and trust review

- **Review date:** 2026-09-16
- **Reviewer:** internal AI/fresh-context security review (`I1`; not external recognition)
- **Scope:** the complete AA-1 package in `architecture-agent-bridge-sdk-2026-09-15/`, including the spine, all companions, AD-1--AD-3, prior reviews, `.memlog.md`, the AA-2 contract and AA-1 Gate record
- **Governing retained inputs checked:** frozen `EI-01`--`EI-25`, frozen `SBC-01`--`SBC-10`, and the referenced AD-21 Pre-Prototype Control Manifest
- **Method:** adversarial invariant-preservation review plus exhaustive boundary/path tracing. Editorial structure and prose were considered only where they could hide or weaken a security obligation; they produced no separately counted editorial finding.
- **Reviewed package-set digest:** `885a9d5c7358a61ff4ec6051ea789aa9ba14777166f8d249527a6c41dcd78c40` (SHA-256 over the sorted per-file SHA-256 lines, before this report was added)
- **External control snapshots:** AD-21 template `cc9722775af4de1d8487745c2d97925cf01a83b1ec3cf32e2ee6edd0e7443dac`; EI ledger `8290da34932c986f79067275e57f270e5d539585741633abae91c7eeb69ce575`; SBC policy `937c093fcc2b4a9dd5bc5fc0663c9ddd707ed25e29f9603014feeddbd2bc1fad`

## Verdict

**REDESIGN / BLOCK AA-1 passage.**

The package has a strong fail-closed posture and preserves all EI/SBC identifiers, but identifier coverage is not enough. Eleven enforceability gaps remain. Eight are High because they leave an authority, disclosure, external-effect, shared-limit, evidence or executable-sandbox safety rule without a complete earlier normative seam. Under the package's own Gate policy, any unresolved High blocks passage, and `pass with conditions` is not available.

This verdict does **not** reject Native Architecture-First and does not request implementation or technology selection. It requires documentary corrections to AA-1/AA-2 contracts and the referenced AD-21 guard before AA-1 can authorize documentary AA-2.

### Counts

| Classification | Count |
| --- | ---: |
| Critical | 0 |
| High | 8 |
| Medium | 3 |
| Low | 0 |
| Total | 11 |

## Findings

### SEC-FINAL-01 — Protected-disclosure atomicity is not carried into the AA-2 proof seam

- **Classification:** High; affects `EI-08`, `SBC-01`, `SBC-02`, `SBC-06`.
- **Location:** `AA-2-NORMATIVE-MODEL-CONTRACT.md:59-60,89`; `QUALITY-ATTRIBUTE-SCENARIOS.md:76-84`; `ASSETS-ADVERSARIES-BOUNDARIES.md:93,108-109`.
- **Trigger condition:** the frozen rule applies boundary-local or provably atomic authority checking to both Protected Disclosure and Consequential Effect. The AA-2 contract defines a linearization event and falsifier only for an effect commit, and QAS-SEC-03 tests only a consequential operation. An implementation can therefore validate disclosure authority early, lose authority/purpose/subject before bytes, metadata, timing or diagnostics become observable, and still satisfy the named AA-2 assertion set.
- **Exact fix:** amend AA-2 rule 7 and `AA2-LOM-003`/`AA2-F-LOM-003` to define both (a) a protected-disclosure observation/commit event and (b) an effect commit event. At each event, require the exact Decision Subject, effective permit, purpose/audience, disclosure class, relevant epochs and preconditions to be current, or require a trace-preserving refinement proof. Expand QAS-SEC-03 to disclosure races and include one-field subject, revoke, purpose, audience, classification and policy changes immediately before the first observable content/metadata event.
- **Potential consequence:** revoked or changed data can escape even though the package claims zero post-revoke disclosures.

### SEC-FINAL-02 — Due Obligations have no normative lifecycle or bypass falsifier

- **Classification:** High; affects `EI-07`, `SBC-01` and privacy/retention duties.
- **Location:** `BASELINE-V0X-SCOPE.md:70-78`; `AA-2-NORMATIVE-MODEL-CONTRACT.md:42-48,57,84-99`; `QUALITY-ATTRIBUTE-SCENARIOS.md:56-64`.
- **Trigger condition:** the frozen SBC-01 definition blocks bypass of a due Obligation, and EI-07 requires pre/during/post boundaries, responsible and enforcing parties, evidence, deadline, failure and consequence. AA-2 names authority and approval, but no artifact or stable assertion defines Obligation identity, when it becomes due, who enforces it, or whether unmet/unknown evidence blocks permit. The single reference to a “composed obligation set” only covers Profile composition.
- **Exact fix:** extend `AAM`/`LOM` or add an Obligation Model defining obligation subject/identity, phase (`pre`, `during`, `post`), responsible party, enforcement party, due condition/deadline, evidence predicate, fulfillment/failure/unknown state, permit/effect dependency and consequence. Add a stable assertion/falsifier that omits, reassigns, falsely satisfies or races each due obligation and requires non-permit where the Profile makes it a precondition. Trace it explicitly to EI-07 and SBC-01.
- **Potential consequence:** an operation can be authorized while a mandatory approval, retention, deletion, notification or other due condition is silently bypassed.

### SEC-FINAL-03 — The malicious-executor model conflicts with the zero-unauthorized-effect claim

- **Classification:** High; affects `EI-08`, `EI-10`, `SBC-01`, `SBC-02`, `SBC-04`, `SBC-10`.
- **Location:** `ASSETS-ADVERSARIES-BOUNDARIES.md:57,83,94,108-117`; `QUALITY-ATTRIBUTE-SCENARIOS.md:56-64`; `ARCHITECTURE-SPINE.md` — Constitutional Safety Rules 1--4.
- **Trigger condition:** ADV-05 explicitly permits a malicious/buggy executor to perform a different effect, while BND-06 lets the effect owner itself be the enforcement authority and QAS-SEC-01 measures forbidden effects as zero. If the effect owner/reference monitor is the compromised executor, no protocol check can stop an out-of-band mutation. The package does not distinguish an untrusted worker from the trusted reference monitor, nor does it scope the zero-event measure to protocol-mediated/attributable effects.
- **Exact fix:** split BND-06 into (1) untrusted executor to reference monitor and (2) reference monitor to protected resource/system of record. AA-3 must freeze which component is in the trusted computing base, all bypass paths, and the capability of its full compromise. Either require an independently enforced monitor for the claim scope, or state that full compromise of the final enforcement authority invalidates/suspends the effect-safety claim and cannot yield pass. Rewrite QAS-SEC-01's measure as zero unauthorized effects through the frozen enforcement boundary while separately requiring detection/claim invalidation for out-of-band effects.
- **Potential consequence:** an impossible guarantee can pass on paper, or a compromised effect owner can mutate state while the protocol still advertises zero unauthorized effects.

### SEC-FINAL-04 — Shared-limit safety stops at “reservation” and lacks a complete consumption lifecycle

- **Classification:** High; affects `EI-09`, `EI-11`, `EI-18`, `SBC-03`, `SBC-04`.
- **Location:** `AA-2-NORMATIVE-MODEL-CONTRACT.md:57,86,99`; `QUALITY-ATTRIBUTE-SCENARIOS.md:96-104`; `ASSETS-ADVERSARIES-BOUNDARIES.md:101,114,153`.
- **Trigger condition:** AA-2 requires shared-limit reservation and order-independent authority composition, but never defines reserve/commit/consume/release/expire/recover/reconcile transitions or their coupling to logical operation, effect and epoch identity. Crash after reservation, late commit after release, lease expiry, retry after lost response and coordinator substitution can therefore double-spend or permanently strand capacity while all named assertions still pass.
- **Exact fix:** require a normative shared-limit state machine jointly owned by `AAM` and `LOM`: limit identity/epoch, reservation identity, amount/use/quorum unit, reserve, commit/consume, release, expiry, recovery, reconciliation and terminal states; idempotent linkage to Logical Operation and effect identity; partition behavior; safe-remainder proof; and substitution/migration rules. Add falsifiers for every crash point, delayed message, duplicate retry, epoch change and coordinator/escrow replacement. Unknown reconciliation must not permit new consumption.
- **Potential consequence:** concurrent branches can overspend a frozen limit or reuse a released/forgotten reservation after recovery.

### SEC-FINAL-05 — The retained minimum of two independent observer paths is not preserved

- **Classification:** High; affects `EI-22`, `EI-25`, `SBC-08`, `SBC-10`.
- **Location:** retained `safety-blocking-and-risk.md:108-113`; `QUALITY-ATTRIBUTE-SCENARIOS.md:39-52`; `ALLOCATION-MATRIX.md:222,226`; AD-21 `prototype-control-manifest.md:28`.
- **Trigger condition:** the frozen downstream closure requires at least two independent observer paths for every SBC where Protected Disclosure or Consequential Effect is possible. The AA-1 package requires a qualified observer, independent verdict or generic observer independence, but nowhere preserves the numeric minimum or makes absence of a second path `invalid`/`inconclusive`. AD-21 permits “observer paths” without a minimum or independence test.
- **Exact fix:** add the retained rule verbatim to QAS pass evidence, the SBC-01--SBC-10 allocation/closure routes, AA-5 acceptance and AD-21 Observers: at least two independently derived observer paths for each applicable protected disclosure/effect outcome; document shared components and failure correlation; common oracle code alone is insufficient; missing or disagreeing required paths yield `invalid`/`inconclusive`, never pass.
- **Potential consequence:** a single faulty or captured observer can certify that no forbidden disclosure/effect occurred.

### SEC-FINAL-06 — AD-21 can authorize executable work without bounding real targets or consequential effects

- **Classification:** High; affects `EI-19`, `EI-25`, `SBC-06`, `SBC-07`, `SBC-10`.
- **Location:** AD-21 `prototype-control-manifest.md:15-32,34-38`; `OWNER-ARCHITECTURE-GUIDE.md` — “Что разрешено и что запрещено сейчас”; `ADR-DISCIPLINE.md:99,115-125`.
- **Trigger condition:** AD-21 requires synthetic data, no production credentials and deny-by-default egress, but has no mandatory Targets/Effects field. An allowlisted public or test service may accept real communications, mutate an external resource, incur charges or affect another party without production data or credentials. Once filled, the manifest's `frozen-approved` verdict would authorize execution despite the Owner guide's separate prohibition on real consequential effects and external actions.
- **Exact fix:** add mandatory `Targets and effects` and `External actions` fields: exact endpoints/accounts/namespaces/resources; local/simulated or owner-controlled isolated targets only; zero real money, asset, entitlement, customer, public communication or third-party consequential effect; explicit effect detector; cleanup/rollback and residual-effect verification; spending/contract/registration/publication prohibition; and separately approved exception authority if ever needed. Any target, account, endpoint, effect class or external-action change must reset the manifest to `incomplete/no-use`.
- **Potential consequence:** a “safe” prototype can perform a real external action while satisfying every current AD-21 checkbox.

### SEC-FINAL-07 — Retry safety after deduplication/evidence retention expiry is not normatively resolved

- **Classification:** High; affects `EI-11`, `EI-13`, `SBC-04`, `SBC-08`.
- **Location:** `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-COR-01 and slot QA-N03; `AA-2-NORMATIVE-MODEL-CONTRACT.md:44,59,85`; `ALLOCATION-MATRIX.md:191`.
- **Trigger condition:** the retained invariant explicitly treats retention expiry that makes a replay look new as unsafe. QAS-COR-01 defers the retention/idempotency scope to AA-2/AA-3, but `AA2-LOM-001` only distinguishes IDs and attempts. It does not define the mandatory result when the receiver no longer has enough state to prove whether the logical operation already committed.
- **Exact fix:** add an AA-2 lifecycle state and assertion for `deduplication/effect knowledge unavailable or expired`. Reuse of the same Operation ID/Decision Subject after this point must return a typed `unknown/non-permit` (or another explicitly proved no-duplicate outcome), never silently become a fresh executable operation. Require Profiles to state the outcome-retrieval/deduplication durability claim and AA-3/AA-4 to freeze retention/resource realizations consistent with it. Add expiry-before/after-commit and lost-response falsifiers.
- **Potential consequence:** an old request can create a second irreversible effect after its deduplication record expires.

### SEC-FINAL-08 — Compensation is separate but not explicitly re-authorized at its own effect boundary

- **Classification:** High; affects `EI-06`, `EI-08`, `EI-12`, `SBC-01`, `SBC-02`, `SBC-04`.
- **Location:** `BASELINE-V0X-SCOPE.md:80-90`; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-COR-02; `AA-2-NORMATIVE-MODEL-CONTRACT.md:44,59,88`.
- **Trigger condition:** the baseline says Compensation is a new separately authorized Operation, but the AA-2 output and named falsifier only require a separate linked operation/outcome. Nothing explicitly rejects reuse of the original operation's now-revoked or differently scoped authority for the compensating effect.
- **Exact fix:** add a stable AA-2 assertion/falsifier requiring every compensation to have its own Logical Operation ID, Decision Subject, current effective authority, limits/obligations, commit event and outcome, while preserving the causal link to the original effect. Test revoke, changed resource/amount/audience, partial compensation, repeated compensation, and compensation-of-compensation traces. Update QAS-COR-02 expected response and measure to include unauthorized compensation count `0`.
- **Potential consequence:** a rollback/refund/remediation action can mutate resources under stale authority merely because it is labeled compensation.

### SEC-FINAL-09 — Bridge identity/loss acceptance is asymmetric by direction

- **Classification:** Medium; affects `EI-17`, `SBC-05`, `SBC-09`, `SBC-10`.
- **Location:** `AA-2-NORMATIVE-MODEL-CONTRACT.md:65,96`; `BASELINE-V0X-SCOPE.md:249-266`; `ASSETS-ADVERSARIES-BOUNDARIES.md:99,112`.
- **Trigger condition:** the Bridge contract is directed and intended for either direction, but `AA2-APM-002` accepts only a “foreign-to-native transformation.” A native-to-foreign mapping, Bridge chain or loop can collapse Native Actor/Principal into the Bridge presenter, shed provenance, or inflate assurance without violating the named acceptance case.
- **Exact fix:** make the APM assertion direction-neutral and instantiate it separately for native-to-foreign, foreign-to-native, Bridge chains and round-trip/loop attempts. Preserve source subject, presenter, transformation path, loss and ceiling at every hop; prohibit imported/exported Authority unless a separately scoped trust/authority rule validates it; critical loss in either direction must fail the dependent action.
- **Potential consequence:** outbound or chained translation can impersonate a principal or re-import weakened meaning as stronger Native authority.

### SEC-FINAL-10 — Privacy purpose and necessity are deferred to AA-3 without an AA-2 semantic relation

- **Classification:** Medium; affects `EI-08`, `EI-19`, `SBC-01`, `SBC-06`, `SBC-10`.
- **Location:** `QUALITY-ATTRIBUTE-SCENARIOS.md:108-116`; `AA-2-NORMATIVE-MODEL-CONTRACT.md:40-48,52-67,103-115`; `BASELINE-V0X-SCOPE.md:120-128`.
- **Trigger condition:** QAS-PRI-01 requires every disclosed element to have purpose, recipient/observer and necessity, but its Gate chain starts at AA-3. AA-2 has no normative relation among disclosure subject, purpose, audience, field/class, necessity and allowed linkability. AA-3 could therefore invent incompatible privacy semantics rather than refine one Core seam.
- **Exact fix:** add a mechanism-neutral disclosure authorization relation to DSM/AAM (or a dedicated disclosure model): exact disclosure subject/fields, purpose, audience/observer, necessity basis, classification, linkability scope and epoch. Define deterministic `permit/non-permit/cannot-establish`; absent, conflicting or unknown mandatory purpose/necessity is non-permit. AA-3 may set Profile-specific policy and bounds but may not change that relation. Add clean-room and one-field mutation falsifiers at AA-2.
- **Potential consequence:** two otherwise conformant implementations disclose different data for the same purpose and still claim privacy compliance.

### SEC-FINAL-11 — The allocation matrix misstates the first closure Gate for Core safety semantics

- **Classification:** Medium; affects preservation/allocation of `EI-02`, `EI-05`--`EI-09`, `EI-13`, `EI-18` and `SBC-01`--`SBC-03`, `SBC-08`.
- **Location:** `ALLOCATION-MATRIX.md:17-21,181-205,213-226`; `AA-2-NORMATIVE-MODEL-CONTRACT.md:34-48,78-115`.
- **Trigger condition:** the matrix defines `Closure gate` as the first Gate where applicable evidence must close an obligation. It assigns the named Core semantics to `NM` (AA-2) but lists first closure at AA-3 or later for multiple rows, despite the AA-2 contract explicitly requiring Decision Subject, authority, atomicity, shared-limit, evidence and membership models before AA-2 passes. A reviewer following the matrix can treat missing AA-2 semantics as safely deferred.
- **Exact fix:** split semantic, threat/mechanism and conformance closure explicitly. At minimum add `AA-2 semantic closure` for EI-02, EI-05--EI-09, EI-13, EI-18 and the normative-model portions of SBC-01--SBC-03/SBC-08; retain AA-3 for threat/trust/privacy realization and AA-5/AA-6 for executable/conformance/integrated evidence. Update the header so multi-Gate cells cannot be read as a single first Gate, and mechanically cross-check every `NM` row against AA-2 Gate acceptance.
- **Potential consequence:** the Gate record can report exact allocation while a safety-critical Core relation remains deferred to a later layer that is not allowed to invent it.

## Cross-cutting assessment

### EI-01--EI-25 and SBC-01--SBC-10 preservation

All IDs are present exactly once in the matrix's normative allocation sections, and the package repeatedly states that the frozen source text is not shortened. The failures are semantic preservation gaps, not missing labels: EI-07 obligations, EI-08 disclosure atomicity, EI-09 shared-limit lifecycle, EI-11 retention expiry, EI-12 compensation authorization, EI-17 bidirectional Bridge behavior, EI-19 disclosure-purpose semantics, and EI-22/EI-25 independent observation are not fully operationalized at the first safe seam.

### Fail-closed semantics

The general `unknown`/`partial`/conflict rules are sound. The remaining fail-open paths arise where the model has no state or event to which the rule can attach: disclosure observation, obligation due state, expired deduplication knowledge, compensation authorization, shared-limit reservation lifecycle and reverse-direction Bridge translation.

### Authority/effect atomicity and external trust

Effect atomicity has a strong abstract linearization rule. Protected disclosure needs an equivalent rule. External providers are correctly treated as scoped claim issuers, but the final effect-enforcement trust boundary must distinguish an untrusted executor from the trusted resource monitor and state what a complete compromise does to the claim.

### Privacy and resource safety

The privacy inventory and later data-flow obligations are substantial. The missing earlier disclosure-purpose relation and executable target/effect containment remain blockers. Resource categories are broadly preserved; no separate defect was found in deferring numeric protocol ceilings to AA-4, provided AD-21 continues to impose experimental sandbox ceilings before any run.

### Unsafe deferral test

Concrete wire, crypto, transport, clock windows, numeric protocol budgets, provider choices and Bridge targets are safely deferred. The findings above identify semantics that cannot be deferred safely because AA-3/AA-4 would otherwise have to invent Core meaning or an executable manifest could authorize an effect outside the reviewed boundary.

## Required closure sequence

1. Correct SEC-FINAL-01--04, 07--10 in the AA-2 contract/QAS/boundary artifacts and add the named stable assertions/falsifiers.
2. Correct SEC-FINAL-05 in QAS, allocation, AA-5 acceptance and AD-21 while preserving the frozen OQ-4 observer rule verbatim.
3. Correct SEC-FINAL-06 in the authoritative AD-21 template and align the Owner guide/ADR discipline references.
4. Correct SEC-FINAL-11 and rerun exact allocation plus semantic first-Gate review.
5. Re-run an independent security/privacy/authority review against exact new digests. Preserve this report and link every finding to correction evidence; a prose assertion of closure is insufficient.
6. Only after all High findings are closed and the rerun finds no unresolved SBC/Critical/High blocker may the AA-1 Gate record issue `pass` or a permissible `pass with conditions`.

## Dissent and limits

The strongest contrary view is that AA-2 Gate acceptance item 3 (“every security-critical relation has a forbidden trace”) will eventually force most missing cases to appear. That generic catch-all is useful but is not an adequate substitute for a named semantic owner, state/event and falsifier where the package already knows the exact frozen hazard. The prior divergence review established the same principle for DVG-01--DVG-18.

This is a documentary architecture review. It does not claim empirical exploitability, production readiness, external independence (`I2`) or legal sufficiency. No deliverable other than this review report was edited.

---

## Final security re-review — 2026-09-16

- **Reviewer:** internal AI/fresh-context security re-review (`I1`; not external recognition)
- **Method:** adversarial invariant-preservation review and boundary/path tracing against the exact amended artifacts, followed by mechanical checks of stable assertion/falsifier presence, staged closure routes and the frozen observer rule
- **Exact snapshots:** `AA-2-NORMATIVE-MODEL-CONTRACT.md` `ccf38ddedea6396d42eba50115c8f6e80a952cea95caaf8c9ce2ff144023bac3`; `QUALITY-ATTRIBUTE-SCENARIOS.md` `bcc0d249101386913552539d40a80f079b824620de5943d8af03b6d9e72a1b32`; `ASSETS-ADVERSARIES-BOUNDARIES.md` `bc34dabece403a65f309e74160ad400747be905534673d161aaacab389e5b0be`; `ALLOCATION-MATRIX.md` `d48056f3259620cb75e032b56b810504353d7576eadc11badff3d09e2e20dbcd`; authoritative AD-21 `prototype-control-manifest.md` `1a5b74610aba3b4c10e76213bc6e413cf74b198fbc504e913113efcbd6403e7b`; `architecture-assurance-charter.md` `4cc51418d3f525af575c45833be902a7dc1e6c37181dcae1be510013ace6e705`

### Verdict

**PASS for the security/privacy/authority/trust review lens.**

All eleven `SEC-FINAL` findings are closed in the reviewed snapshots. No unsafe semantic or executable-control deferral remains in the reviewed scope. This verdict removes the prior security-lens `REDESIGN / BLOCK`; it does not by itself issue the overall AA-1 Gate verdict, authorize executable work, or establish external (`I2`) recognition.

### Residual counts

| Classification | Count |
| --- | ---: |
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 0 |
| **Total** | **0** |

### Closure confirmation

| Prior finding | Re-review result |
| --- | --- |
| SEC-FINAL-01 | Closed: Protected Disclosure now has an exact observation/commit boundary, race falsifiers and boundary-local current permit/purpose/audience/necessity/classification checks. |
| SEC-FINAL-02 | Closed: the normative Obligation identity/lifecycle, responsible/enforcing parties, due/evidence states, consequences and bypass falsifier are mandatory AA-2 outputs. |
| SEC-FINAL-03 | Closed: untrusted executor and trusted resource-scoped reference monitor are separate boundaries; full monitor compromise or unknown bypass suspends the positive claim. |
| SEC-FINAL-04 | Closed: shared limits now have reserve, commit/consume, release, expiry, recovery, reconciliation and substitution semantics with stale/unknown reconciliation fail-closed. |
| SEC-FINAL-05 | Closed: every applicable Protected Disclosure/Consequential Effect SBC evaluation requires at least two independently derived observer paths; missing paths or unresolved disagreement are `invalid`/`inconclusive`, never pass, across QAS, AA-2/AFC, boundaries, allocation, Charter AA-3/AA-5 and AD-21. |
| SEC-FINAL-06 | Closed: AD-21 now freezes exact targets/effects and external actions, requires zero real consequential effect or commitment, cleanup/residual-effect verification and reset to `incomplete/no-use` on any relevant change. |
| SEC-FINAL-07 | Closed: expired/unavailable deduplication or effect knowledge is an explicit state; retry cannot silently become a fresh executable operation. |
| SEC-FINAL-08 | Closed: compensation is a separately authorized operation with its own subject, current authority, Obligations, limits, commit and outcome. |
| SEC-FINAL-09 | Closed: Bridge obligations and falsifiers cover both directions, chains, round trips and re-entry while preserving subject, Presenter, path, loss, provenance and assurance ceiling. |
| SEC-FINAL-10 | Closed: AA-2 owns the mechanism-neutral disclosure purpose/audience/necessity/classification/linkability relation and one-field falsifiers. |
| SEC-FINAL-11 | Closed: the matrix defines multi-Gate cells as staged closures and explicitly assigns AA-2 semantic/normative-model closure to EI-02, EI-05--EI-09, EI-13, EI-18 and the normative portions of SBC-01--SBC-03/SBC-08, while retaining later threat/trust/mechanism/conformance/integrated closure. |

### Residual limitations

The review remains documentary. Concrete mechanisms, numeric protocol budgets and empirical results are correctly deferred to their named Gates and cannot weaken the fail-closed AA-2 semantics. Executable models and prototypes remain prohibited unless the exact AD-21 instance is independently reviewed, signed and `frozen-approved`; reference implementation, publication, production and adoption claims remain governed by their later Gates.
