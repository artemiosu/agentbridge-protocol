---
title: AgentBridge AA-2 — Normative Model Contract
status: final-aa1-input
created: 2026-09-16
updated: 2026-09-16
scope: обязательные выходы и критерии приёмки AA-2 Abstract Normative Model
authority:
  - AD-1
  - AD-2
  - AD-3
allocation_targets:
  - AD-4
  - AD-5
  - AD-6
  - AD-7
sources:
  - ARCHITECTURE-SPINE.md
  - GLOSSARY-AND-CONTEXT.md
  - BASELINE-V0X-SCOPE.md
  - QUALITY-ATTRIBUTE-SCENARIOS.md
  - ASSETS-ADVERSARIES-BOUNDARIES.md
  - ALLOCATION-MATRIX.md
  - reviews/review-divergence.md
---

# AA-2 Normative Model Contract

## 1. Назначение и обязательность

Этот контракт определяет, что AA-2 обязан зафиксировать до положительного Gate verdict. Он устраняет возможность, при которой две независимо разработанные части буквально соблюдают AD-1–AD-3, но расходятся в общей семантике.

AA-2 остаётся документальным этапом. Этот контракт **не выбирает** wire encoding, transport, canonical bytes, cryptographic algorithm/suite/provider, clock tolerance, runtime, язык, repository layout, численные resource ceilings или конкретный foreign-protocol Bridge. Любой executable model, model-checker run либо prototype по-прежнему требует отдельного `frozen-approved` AD-21 manifest.

AD-4 владеет participant/role/context/interaction/composition/evolution model; AD-5 — Decision Subject, disclosure purpose/audience/necessity, authority, consent/approval/authorization/Obligation, protected-disclosure/effect-boundary и evidence semantics; AD-6 — Operation, attempt, lifecycle, retry/replay/cancel/recovery/compensation/shared-limit consumption model; AD-7 — method, assertion/falsifier coverage и доказательство непротиворечивости.

## 2. Обязательный комплект нормативных артефактов

| Код | Нормативный выход AA-2 | Владелец | Governing AD |
| --- | --- | --- | --- |
| `AIM` | Abstract Information Model: semantic domains, presence, cardinality, equality, collections, numeric/time domains, identifiers и reference integrity | Normative Information Model Owner | AD-4 |
| `RCM` | Role, Context, Interaction and Membership Model | Role/Context Model Owner | AD-4 |
| `DSM` | Decision Subject and Disclosure Model: identity, equality, criticality, refinement, exact disclosed subject/fields, purpose, audience/observer, necessity, classification и linkability scope | Authority/Decision/Disclosure Model Owner | AD-5 |
| `AAM` | Authority, Approval and Obligation Algebra: grants, delegation, aggregation, conflicts, limits, effective permit и pre/during/post Obligation lifecycle | Authority/Obligation Model Owner | AD-5 |
| `LOM` | Logical Operation and Lifecycle Model: attempts, disclosure/effect commit, knowledge, retry, replay, expired deduplication knowledge, shared-limit consumption, recovery, cancel и separately authorized compensation | Lifecycle/Effect Model Owner | AD-6 |
| `ECM` | Evidence and Claim Model: graph, provenance, conflict, redaction, assurance и knowledge refinement | Evidence/Provenance Owner | AD-5, AD-6 |
| `CEM` | Composition and Evolution Model: Profiles, Extensions, versions, negotiation и artifact closure | Protocol Evolution Owner | AD-4 |
| `APM` | Abstract Port Model for Binding, Bridge and External Dependency boundaries | Protocol Architect with Security Architect | AD-4, AD-5, AD-6 |
| `AFC` | Assertion, Falsifier and Coverage Catalog tying every relation and transition to stable IDs and mandatory topologies | Formal Methods Lead / Conformance Lead | AD-7 |

Все девять выходов нормативны, независимо версионируются, входят в точную AA-2 artifact closure и не могут существовать только как prose example, SDK behavior или private explanation.

## 3. Общие правила модели

1. `AIM` задаёт abstract semantics до любого Binding mapping: primitive domains; distinction между absent, explicit empty, `unknown` и invalid; cardinality; ordered/unordered collections; numeric and time domains; identifier comparison; graph-reference integrity; deterministic equality и invalid-input result.
2. `RCM` задаёт relation `Participant × Context × Role × Epoch`, controller-equivalence, independence, допустимые multi-role combinations, создание/изменение/окончание роли, Interaction/Branch membership и stale/conflicting-role result.
3. `DSM` задаёт total semantic projection Decision Subject, точное equality, collision rule, Profile-authorized criticality и refinement partial order. Неклассифицированное изменение создаёт новый Decision Subject. Для Protected Disclosure он также задаёт mechanism-neutral relation `disclosure subject/fields × purpose × audience/observer × necessity basis × classification × allowed linkability × epoch → permit | non-permit | cannot-establish`; отсутствующее, конфликтующее или неизвестное mandatory значение не поддерживает раскрытие.
4. `AAM` задаёт authority domain, delegation/derivation relation, grant aggregation, caveat conjunction, deny/conflict precedence, lineage selection, quorum/approval independence и результат incomplete/incomparable inputs. Он задаёт Obligation identity, exact subject, phase (`pre`, `during`, `post`), responsible/enforcing party, due condition/deadline, required evidence, fulfillment/failure/unknown state и consequence; обязательная precondition со state не `fulfilled` не поддерживает permit. Monotonic attenuation проверяется над composed effective authority.
5. Freshness задаётся mechanism-neutral partial order с исходами `current`, `stale`, `future`, `incomparable`, `unknown`; только `current` может поддерживать dependent positive permit. Часы, окна и providers выбираются позже.
6. `LOM` различает `Logical Operation ID`, `Delivery Attempt ID` и `Execution Attempt ID`. Lifecycle является композицией как минимум delivery/acceptance, authorization, execution, protected disclosure, effect и knowledge projections; `unknown`, `partial` и `conflict` относятся к знанию об disclosure/effect и не превращаются в success/no-effect. Отсутствующее или истёкшее deduplication/effect knowledge является явным state и не превращает прежний Operation ID в новую исполнимую Operation.
7. Protected Disclosure имеет один abstract observation/commit event, а каждый Consequential Effect — один abstract commit/linearization event. В соответствующей точке одновременно проверяются exact Decision Subject, disclosure purpose/audience/necessity/classification либо effect identity, effective authority, due preconditions/Obligations, shared-limit state и relevant epochs; иначе требуется refinement proof, сохраняющий тот же allowed trace set. Более ранняя проверка без такой связи недостаточна.
8. Error algebra отделено от wire code и diagnostics и включает category, safety projection, retryability, terminality, disclosure class, causal subject и required evidence. Privacy projection может укрупнять внешний error, но не изменяет внутренний normative outcome.
9. Profile и Extension composition детерминированы. Applicability, multiplicity, scope, inheritance, compatibility, dependency, collision, order/commutativity и non-composition outcome заданы нормативно; порядок объявления не может неявно менять результат.
10. Artifact closure вычислима из root manifest: included kinds, transitive edge types, conditional dependencies, missing/cycle handling, digest domain, resolution instant, inheritance, invalidation и migration определены. Одинаковый closure root означает одинаковые members и meaning.
11. Version negotiation задаёт compatibility relation, offer/require constraints, deterministic selection/tie-break, transcript equivalence, downgrade floor, no-common-set и место migration относительно selection.
12. `APM` определяет typed semantic ports. Binding выдаёт attributed coverage/security/freshness/resource claims, но не Authority или Effect. Bridge в обоих направлениях и в каждой цепочке сохраняет source subject, Presenter, transformation path, provenance, loss и assurance ceiling без identity substitution или автоматического импорта/экспорта Authority; round-trip/loop не повышает meaning/assurance, а critical loss блокирует зависимое действие. External dependency имеет request/claim identity, freshness/cache, outage/partition/compromise, termination и substitution state semantics.
13. `ECM` задаёт evidence graph, subject/causal matching, completeness/redaction, conflict predicate, monotone knowledge refinement и допустимые операции Profile trust rule. Применимое противоречие не исчезает без нормативной resolution relation и сохранённого provenance.
14. `RCM`/`AAM`/`LOM` совместно задают membership ownership, epoch ordering, branch fork/join, approval validity, quorum snapshot point, aggregate merge и split-view outcome.
15. `AAM`/`LOM` совместно задают shared-limit lifecycle: limit identity/epoch, reservation identity, reserve, commit/consume, release, expiry, recovery, reconciliation и terminal state; связь с Logical Operation/Decision Subject/effect identity; crash/retry/partition/late-message behavior; coordinator/escrow substitution. `unknown`, stale или incomparable reconciliation не разрешает новый расход и не восстанавливает уже consumed capacity.
16. Compensation является новой причинно связанной, но отдельно разрешённой Logical Operation с собственными Decision Subject, current effective authority, Obligations, shared limits, commit event и Outcome. Она не наследует permit исходной Operation и не переписывает её историю.
17. `APM` различает untrusted executor и trusted resource-scoped reference monitor/effect owner. Положительный effect-safety claim требует, чтобы все in-scope mutation paths проходили через frozen enforcement boundary; полная compromise этой boundary либо неизвестный bypass приостанавливает зависимый claim и не может дать `pass`.

## 4. Stable assertion и falsifier discipline

- Stable assertion ID имеет форму `AA2-<artifact>-NNN`; ID никогда не переиспользуется с новым смыслом.
- Каждый assertion фиксирует quantified inputs, preconditions, allowed result/transition, forbidden result и связанные FR/NFR/EI/SBC/QAS.
- Каждый assertion имеет отдельный falsifier ID `AA2-F-<artifact>-NNN`, минимальный counterexample shape и ожидаемый non-pass result.
- Prose example не создаёт требование. Assertion и falsifier обязаны быть одинаково доступны обеим independent implementation teams.
- Documentary falsifiers обязательны в AA-2. Их executable реализация разрешена только после применимого AD-21 и не может молча добавить новое требование.
- Положительный AA-2 verdict требует 100% transition/relation coverage, отдельный falsifier для каждого security-critical assertion и отсутствие assertion, чья истина зависит от выбранного wire/crypto/runtime mechanism.

## 5. DVG-01–DVG-18 allocation и AA-2 acceptance

| Finding | AD / artifact / accountable owner | Required stable assertion и falsifier | AA-2 acceptance (Given / When / Then) |
| --- | --- | --- | --- |
| DVG-01 | AD-4 / `AIM` / Normative Information Model Owner | `AA2-AIM-001`; `AA2-F-AIM-001` varies absent/null/empty, order, number/time and ID equality | **Given** semantically equal and unequal abstract values, **when** two clean-room evaluators parse and compare them, **then** both derive the same valid/invalid result and equality relation. |
| DVG-02 | AD-4 / `RCM` / Role/Context Model Owner | `AA2-RCM-001`; `AA2-F-RCM-001` swaps owner/transport position and performs late role change | **Given** identical participants, contexts and epochs, **when** roles are assumed, changed or ended, **then** the same role relation and stale/conflict outcome is derived in every mandatory topology. |
| DVG-03 | AD-5 / `DSM` / Authority/Decision Model Owner | `AA2-DSM-001`; `AA2-F-DSM-001` mutates each subject dimension | **Given** a Decision Subject and one-field mutations, **when** equality/refinement is evaluated, **then** every mutation deterministically preserves identity, creates a permitted refinement, or creates a new subject. |
| DVG-04 | AD-6 / `LOM` / Lifecycle Model Owner | `AA2-LOM-001`; `AA2-F-LOM-001` reuses or replaces logical/attempt IDs incorrectly | **Given** retries, replays and multiple delivery/execution attempts, **when** identities are resolved, **then** one Logical Operation and distinct linked attempts are obtained without duplicate authorized Effect. |
| DVG-05 | AD-5 / `AAM` / Authority Model Owner | `AA2-AAM-001`; `AA2-F-AAM-001` composes grants, denies, caveats and shared limits in adversarial orders | **Given** the same authority lineage and policy inputs, **when** effective authority is composed, **then** allowed scope is order-independent, monotonically narrowed and identical. |
| DVG-06 | AD-4/5/6 / `AIM`,`AAM`,`LOM` / Authority and Lifecycle Owners | `AA2-AAM-002`; `AA2-F-AAM-002` supplies stale/future/incomparable/unknown epochs | **Given** two freshness/epoch views, **when** compared for a protected transition, **then** one normative ordering outcome is returned and non-current material cannot support permit. |
| DVG-07 | AD-6 / `LOM` / Lifecycle/Effect Model Owner | `AA2-LOM-002`; `AA2-F-LOM-002` exercises late, partial, conflicting and crash traces | **Given** the same ordered or concurrent events, **when** lifecycle/effect/knowledge projections are evaluated, **then** both evaluators produce the same allowed state set, terminality and recovery projection. |
| DVG-08 | AD-5/6 / `DSM`,`AAM`,`LOM` / Disclosure and Effect Boundary Owner | `AA2-LOM-003`; `AA2-F-LOM-003` changes subject/authority/purpose/epoch immediately before disclosure observation or effect commit | **Given** a Protected Disclosure or consequential commit trace, **when** a local linearization or claimed equivalent is checked, **then** all required values are current at the exact abstract observation/commit point or a trace-preserving refinement proof is absent and the claim fails. |
| DVG-09 | AD-6 / `LOM` / Error Semantics Owner | `AA2-LOM-004`; `AA2-F-LOM-004` confuses denial, unknown, dependency failure and incompatibility | **Given** the same failing condition and privacy policy, **when** normative and disclosed errors are projected, **then** category, retryability, terminality and safety outcome match while permitted diagnostic detail may be reduced. |
| DVG-10 | AD-4 / `CEM` / Profile Composition Owner | `AA2-CEM-001`; `AA2-F-CEM-001` permutes multiple applicable Profiles and branch scopes | **Given** a Profile set and context, **when** applicability/composition is computed, **then** one order-independent composed obligation set or typed incompatibility results. |
| DVG-11 | AD-4 / `CEM` / Extension Composition Owner | `AA2-CEM-002`; `AA2-F-CEM-002` permutes, collides and removes Extension dependencies | **Given** the same negotiated Extensions, **when** they compose, **then** order cannot change meaning unless normative order is explicit, and non-commuting/unknown critical combinations fail deterministically. |
| DVG-12 | AD-4 / `CEM` / Configuration and Evolution Owner | `AA2-CEM-003`; `AA2-F-CEM-003` changes root, transitive/conditional edge, cycle or missing member | **Given** one closure root and available artifacts, **when** closure is resolved, **then** every evaluator obtains the same exact member/digest set or the same typed closure failure. |
| DVG-13 | AD-4 / `CEM` / Negotiation/Evolution Owner | `AA2-CEM-004`; `AA2-F-CEM-004` permutes equivalent offers, ties, downgrade and no-common cases | **Given** identical authenticated offers/requirements, **when** selection is computed, **then** both sides derive one identical Negotiated Set or one identical refusal without hidden tie-breaking. |
| DVG-14 | AD-4/5/6 / `APM` / Binding Contract Owner with Security Architect | `AA2-APM-001`; `AA2-F-APM-001` promotes transport claim into identity/authority/effect | **Given** Binding port outputs, **when** Core consumes them, **then** each value is typed as observation/claim/input and no Binding fact alone authorizes disclosure or Effect. |
| DVG-15 | AD-4/5/6 / `APM` / Bridge Contract Owner | `AA2-APM-002`; `AA2-F-APM-002` exercises native-to-foreign, foreign-to-native, chained and round-trip mappings that substitute a subject or drop critical provenance | **Given** any directed or chained Bridge transformation, **when** identity, authority, effect and evidence are projected at every hop, **then** source subject, Presenter, path, loss and assurance ceiling remain explicit and no Authority/assurance is inflated, or the dependent action fails closed. |
| DVG-16 | AD-4/5/6 / `APM` / Dependency Contract Owner | `AA2-APM-003`; `AA2-F-APM-003` injects outage, partition, stale cache, compromise and substitution | **Given** the same dependency state and claim, **when** failure/substitution is evaluated, **then** the same dependent scope stops/degrades and no cached or substitute input silently expands authority. |
| DVG-17 | AD-5/6 / `ECM` / Evidence/Provenance Owner | `AA2-ECM-001`; `AA2-F-ECM-001` supplies conflicting, redacted, incomplete and causally unrelated claims | **Given** one evidence graph and trust-rule scope, **when** knowledge is derived, **then** both evaluators return the same success/partial/unknown/conflict projection and retain applicable contradiction/provenance. |
| DVG-18 | AD-4/5/6 / `RCM`,`AAM`,`LOM` / Multi-party Model Owner | `AA2-RCM-002`; `AA2-F-RCM-002` interleaves join/leave, approvals, branch merge, split view and commit | **Given** one membership/quorum trace, **when** epoch and approval validity are evaluated, **then** every evaluator chooses the same quorum snapshot, aggregate outcome and safe split-view result. |

AD-7 and the Formal Methods Lead/Conformance Lead apply to every row: they own assertion well-formedness, falsifier adequacy, trace coverage and the proof that separately constructed models do not admit different security-relevant projections.

### 5.1 Обязательные security/privacy closure assertions

Эти assertions дополняют DVG-01–DVG-18 и обязательны независимо от того, обнаружила ли их повторная divergence construction.

| Finding scope | AD / artifact / accountable owner | Required stable assertion и falsifier | AA-2 acceptance (Given / When / Then) |
| --- | --- | --- | --- |
| Protected Disclosure atomicity and purpose | AD-5/6 / `DSM`,`AAM`,`LOM` / Disclosure Boundary Owner | `AA2-DSM-002`, `AA2-LOM-005`; `AA2-F-DSM-002`, `AA2-F-LOM-005` mutate purpose, audience, necessity, classification, subject, authority or epoch immediately before observation | **Given** an intended disclosure and any one-field/race mutation, **when** the first content, metadata, timing or diagnostic signal becomes observable, **then** one current permit covers the exact disclosed projection at that point or disclosure is non-permit/cannot-establish. |
| Due Obligation lifecycle | AD-5/6 / `AAM`,`LOM` / Authority/Obligation Owner | `AA2-AAM-003`; `AA2-F-AAM-003` omits, reassigns, races or falsely satisfies each pre/during/post Obligation | **Given** exact Profile Obligations, **when** permit and lifecycle transitions are derived, **then** responsible/enforcing party, phase, due state, evidence and consequence are identical and an unmet/unknown mandatory precondition cannot permit. |
| Trusted enforcement boundary | AD-4/5/6 / `APM` / Effect Boundary Owner with Security Architect | `AA2-APM-004`; `AA2-F-APM-004` compromises executor, monitor or a bypass path independently | **Given** an untrusted executor and frozen reference-monitor/resource boundary, **when** compromise/bypass state is projected, **then** only mediated authorized traces support the effect-safety claim; monitor compromise or unknown bypass suspends it. |
| Shared-limit lifecycle | AD-5/6 / `AAM`,`LOM` / Shared-Limit Owner | `AA2-AAM-004`; `AA2-F-AAM-004` interleaves reserve/consume/release/expire/recover/reconcile with crash, retry, partition and coordinator substitution | **Given** one limit epoch and concurrent branches, **when** all interleavings are evaluated, **then** aggregate consumption never exceeds proved capacity and unknown/stale reconciliation cannot create capacity or permit. |
| Expired deduplication/effect knowledge | AD-6 / `LOM`,`ECM` / Lifecycle/Evidence Owner | `AA2-LOM-006`; `AA2-F-LOM-006` expires state before/after commit and retries after a lost response | **Given** reuse of one Operation ID/Decision Subject after required knowledge is unavailable, **when** execution eligibility is evaluated, **then** the result is typed unknown/non-permit or a proved no-duplicate outcome, never a silently fresh commit. |
| Separately authorized compensation | AD-5/6 / `DSM`,`AAM`,`LOM` / Compensation Boundary Owner | `AA2-LOM-007`; `AA2-F-LOM-007` reuses stale original authority, mutates compensation subject or repeats compensation | **Given** a compensation request, **when** its boundary is evaluated, **then** it has a new operation/subject/current permit/limits/Obligations/outcome and retains only a causal link to the original effect. |
| Bidirectional/chained Bridge safety | AD-4/5/6 / `APM` / Bridge Contract Owner | `AA2-APM-005`; `AA2-F-APM-005` composes opposite directions, multiple Bridges and loops with loss/identity changes | **Given** a Bridge path, **when** every hop and any re-entry are projected, **then** subject/Presenter/path/loss/ceiling remain explicit, Authority is separately validated and critical loss fails closed. |
| Independent observation plan | AD-7 / `AFC` / Conformance Lead | `AA2-AFC-001`; `AA2-F-AFC-001` removes, correlates or makes one observer path disagree | **Given** an applicable SBC involving Protected Disclosure or Consequential Effect, **when** verification obligations are cataloged, **then** at least two independently derived observer paths are required; missing independence or unresolved disagreement yields `invalid`/`inconclusive`, never pass. |

## 6. AA-2 Gate acceptance

AA-2 passes only when all conditions hold:

1. `AIM`, `RCM`, `DSM`, `AAM`, `LOM`, `ECM`, `CEM`, `APM` and `AFC` are frozen with exact versions, digests and dependency closure.
2. Every DVG-01–DVG-18 row and every §5.1 security/privacy row has its named assertion, falsifier, owner, cross-reference and review evidence; no row is closed only by prose intent.
3. Every normative state, relation and transition maps to at least one Asset, Context, FR/NFR/EI/SBC and QAS; every security-critical one has a forbidden trace.
4. The complete models cover agent↔service, agent↔agent, service↔service, reverse asynchronous, streaming/long-running, multi-party/delegated and replaceable-infrastructure topologies.
5. Two independently constructed documentary evaluators given the same normative inputs derive the same allowed outcome set, safety projection, artifact closure and error category. Any divergence is rejected by a specific assertion rather than resolved by author explanation.
6. AA-3/AA-4 mechanism choices can refine the typed ports and numeric slots without changing Core meaning. If a mechanism requires a semantic change, AA-2 and all affected evidence reopen.
7. No executable evidence is claimed unless the exact run was authorized by a frozen-approved AD-21 manifest; documentary completeness is not misrepresented as empirical interoperability.

A failed or inconclusive condition yields `redesign/block` for the affected AA-2 scope. It cannot be deferred into a Binding, Bridge, SDK, provider or conformance implementation.
