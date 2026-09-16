---
title: AgentBridge AA-1 Preliminary Assets, Adversaries and Boundaries
status: final-aa1-preliminary
created: 2026-09-16
updated: 2026-09-16
scope: inputs to AA-2 normative model and AA-3 threat model
governing_decisions:
  - AD-1
  - AD-2
---

# Preliminary Assets, Adversaries and Boundaries

## 1. Статус и назначение

Это предварительный security/assurance inventory, достаточный для начала AA-2 Abstract Normative Model. Он не является завершённой threat model. AA-3 обязан подтвердить completeness, назначить owners, определить точные capabilities/assumptions и заполнить completion slots в конце документа.

Базовая позиция:

- сеть, входные сообщения и удалённые Participants не являются доверенными по умолчанию;
- identity, подпись, transport protection и наличие evidence не равны authority или внешней истине;
- доверие локально, ограничено purpose/scope/time/epoch и раскрывается в claim;
- effect owner либо его явно уполномоченный reference monitor остаётся enforcement authority конкретного consequential effect;
- Core не требует глобального trust root, ledger, registry, broker, IdP или AgentBridge Cloud.

## 2. Предварительные assets

| ID | Asset | Требуемые свойства | Основные EI/SBC |
| --- | --- | --- | --- |
| AST-01 | Намерение, Consent и решения Principal | точный subject, добровольность/актуальность в Profile scope, attribution, non-reuse после изменения | EI-05, EI-07, EI-08; SBC-01/02 |
| AST-02 | Authority grants и delegation lineage | integrity, provenance, attenuation, audience/resource/context binding, freshness, revoke/epoch safety | EI-06–EI-09; SBC-01–03 |
| AST-03 | Decision Subject и operation identity | semantic immutability, collision/reuse resistance, exact version/profile scope | EI-05, EI-11, EI-14; SBC-04/05 |
| AST-04 | Protected content и domain data | confidentiality, integrity, purpose limitation, minimization, residency/retention constraints | EI-19, EI-25; SBC-06 |
| AST-05 | Identifiers, metadata, timing и linkability | bounded disclosure/correlation, branch/tenant isolation | EI-04, EI-18, EI-19; SBC-06 |
| AST-06 | Реальный resource и consequential effect | authorized mutation only, boundary-local enforcement, honest partial/unknown state | EI-08, EI-10–EI-13; SBC-01/02/04 |
| AST-07 | Shared amounts, quotas, uses и quorum | no overspend/double use, explicit coordination ownership, partition safety | EI-09, EI-18; SBC-03 |
| AST-08 | Lifecycle, effect и outcome state | integrity, durable scope, causal consistency, no false certainty, crash/replay safety | EI-10–EI-12; SBC-04 |
| AST-09 | Evidence, provenance и causal history | attribution, integrity, completeness claim, conflict visibility, privacy-bounded observability | EI-13, EI-19, EI-22, EI-25; SBC-06/08 |
| AST-10 | Trust roots, credentials, keys и security epochs | controlled issuance/use/rotation/recovery, algorithm/provider agility, compromise containment | EI-06, EI-16, EI-18, EI-23; SBC-01/05/09 |
| AST-11 | Negotiation transcript и selected Profile/Binding/version | freshness, agreement, downgrade/split-view resistance, exact dependency closure | EI-02, EI-03, EI-15, EI-18, EI-23; SBC-05/10 |
| AST-12 | Availability и bounded compute resources | bounded CPU/memory/state/storage/I/O/network/wait, backpressure, safe-stop | EI-20, EI-25; SBC-07 |
| AST-13 | Normative specification и conformance artifacts | integrity, version/digest identity, equal access, reproducibility, separation from reference behavior | EI-22, EI-23, EI-25; SBC-08/09/10 |
| AST-14 | Namespace, release provenance и governance authority | non-equivocation, accountable change, succession, no hidden vendor control | EI-21–EI-23; SBC-08/09 |

AA-2 обязан связывать каждый critical state variable и transition минимум с одним Asset. AA-3 обязан определить для каждого Asset confidentiality/integrity/availability/privacy/authority/evidence objectives и допустимый residual scope.

## 3. Adversary и failure model

### 3.1 Намеренные adversaries

| ID | Adversary | Предварительные capabilities | Не предполагается автоматически |
| --- | --- | --- | --- |
| ADV-01 | Сетевой attacker | observe, drop, delay, duplicate, reorder, replay, inject и correlate traffic | компрометация корректной современной криптографии |
| ADV-02 | Malicious/compromised Participant или agent | отправлять legal/illegal messages, лгать, equivocate, скрывать state/evidence, злоупотреблять собственным authority | authority на чужие resources |
| ADV-03 | Compromised delegate/credential holder | использовать выданные ему материалы вне intended context, collude, proxy/relay | право расширять исходное grant |
| ADV-04 | Confused deputy inducer | подменять subject/audience/resource/context и использовать более привилегированного исполнителя | успешный effect при корректной boundary verification |
| ADV-05 | Malicious/buggy untrusted executor или evidence issuer | попытаться выполнить иной effect, обойти reference monitor, выдать ложный/неполный claim, equivocate | тождество с trusted resource reference monitor; автоматическое доверие relying party; положительный effect-safety claim при неизвестном bypass либо полной compromise финальной enforcement boundary |
| ADV-06 | Colluding approvers/Sybil/common controller | имитировать независимый quorum, скрывать conflict of interest | доказуемая организационная независимость без внешнего evidence |
| ADV-07 | Compromised infrastructure provider | давать stale/false identity, discovery, policy, time, registry, broker или audit information; deny service | право менять Core semantics |
| ADV-08 | Malicious Bridge/intermediary | терять/менять fields, скрывать semantic loss, loop/impersonate/downgrade | повышение assurance исходного сообщения |
| ADV-09 | Insider/operator | злоупотреблять keys, logs, privileged interfaces, deployment/configuration access | бесследное действие при корректных split-control/audit controls |
| ADV-10 | Supply-chain attacker | подменять dependency, build, artifact, update, SBOM либо release channel | право перенести прежний claim на новый closure |
| ADV-11 | Resource-exhaustion attacker | максимизировать legal/illegal size, depth, fan-out, streams, retries, evidence и concurrency | unbounded resource budget |
| ADV-12 | Privacy observer | коррелировать identifiers, branches, timing, sizes, diagnostics и storage behavior | доступ к содержимому за защищённой boundary без отдельного compromise |

### 3.2 Ненамеренные и средовые failures

| ID | Failure | Обязательное моделирование |
| --- | --- | --- |
| FLT-01 | честная, но ошибочная independent implementation | divergent parsing/state/unknown handling, numeric/time edge cases |
| FLT-02 | crash/restart/state loss | before/during/after commit, lost response, duplicate retry |
| FLT-03 | partition и asymmetric reachability | stale authority/policy, split view, shared-limit safety |
| FLT-04 | message loss/duplication/reordering/delay | all interaction modes and reconnect paths |
| FLT-05 | clock skew/jump/rollback | expiry, freshness, ordering and evidence timestamps |
| FLT-06 | stale caches/declarations | discovery, capability, key, policy, revoke and version state |
| FLT-07 | slow consumer/backpressure failure | queues, streams, cancellation, termination and evidence preservation |
| FLT-08 | partial external effect | timeout/unknown, compensation, late completion and conflicting observation |
| FLT-09 | provider outage or incompatible upgrade | declared blast radius, fallback and substitution |
| FLT-10 | observer/harness defect | invalid/inconclusive verdict rather than candidate pass/fail inflation |

### 3.3 Предварительно out-of-scope, но только после явной записи

Ни одна категория не считается out-of-scope молча. AA-3 может ограничить конкретный Profile claim, но обязан назвать rationale и consequence. В частности, physical compromise, endpoint malware, global traffic analysis, cryptographic break, legal coercion и actual human collusion не обещаются как полностью предотвращённые без отдельного Profile; их влияние на claims всё равно должно быть раскрыто.

## 4. Trust, data и enforcement boundaries

| ID | Boundary | Что пересекает границу | Кто/что применяет решение | Опасность при ошибке | EI/SBC |
| --- | --- | --- | --- | --- | --- |
| BND-01 | Principal ↔ agent/delegate | intent, consent, authority, constraints | Principal-side approval/authority mechanism по Profile | agent выдаёт своё решение за волю Principal | EI-05–08; SBC-01/02 |
| BND-02 | Каждый delegation hop | attenuated authority и provenance | downstream verifier + issuer/lineage validation | privilege amplification, credential substitution | EI-06; SBC-01/05 |
| BND-03 | Participant runtime ↔ untrusted input | protocol messages, extensions, evidence | parser, state machine, resource guard | parser/resource abuse, state confusion | EI-15, EI-20; SBC-05/07 |
| BND-04 | Bootstrap/negotiation | capabilities, Profiles, Bindings, versions, epochs | negotiation verifier | circular security selection, downgrade, split view | EI-02/03/18; SBC-05 |
| BND-05 | Protected disclosure observation/commit | content, identifiers, metadata, diagnostics, timing/linkability signals; exact purpose/audience/necessity/classification | trusted disclosure reference monitor/enforcement point | early check races first observable signal; unauthorized data/linkability escape | EI-08/19; SBC-01/02/06 |
| BND-06A | Untrusted executor ↔ trusted resource-scoped reference monitor | requested Decision Subject/effect, presented authority, Obligations, shared-limit material | trusted reference monitor under frozen Profile/dependency closure | executor performs/substitutes an effect or bypasses enforcement | EI-05–11; SBC-01–04/10 |
| BND-06B | Trusted reference monitor ↔ irreversible resource commit | exact authorized effect, preconditions, limit consumption, commit identity | effect owner or explicitly authorized trusted reference monitor controlling every in-scope mutation path | compromised monitor, unknown bypass, unauthorized/double/changed external effect | EI-05–11/25; SBC-01–04/10 |
| BND-07 | Между administrative domains | identity, policy, authority/evidence claims | каждый local relying-party verifier | assumed common policy/trust that does not exist | EI-01/13/16; SBC-05/09/10 |
| BND-08 | Profile policy authority ↔ verifier | classification, quorum, freshness, allowed suites, disclosure rules | verifier по exact Profile/version | policy response ошибочно считается Core truth | EI-07/16/23; SBC-01/05/09 |
| BND-09 | External identity/trust provider ↔ relying party | identity/key/status assertions | relying party under declared trust contract | identity подменяет authority; stale/compromised assertion | EI-06/16/21; SBC-01/05/09 |
| BND-10 | Binding security termination/proxy ↔ Core semantics | decoded content, authenticated context, coverage metadata | Binding adapter + Core verifier | signature/channel terminates too early; critical field uncovered | EI-16; SBC-05/09 |
| BND-11 | Bridge input ↔ translated output in either direction or chain | source subject, Presenter, directed mapped semantics, transformation path, provenance, declared loss/ceiling | each Bridge + receiving verifier at every hop/re-entry | authority/assurance inflation, hidden loss, impersonation, round-trip/loop laundering | EI-17; SBC-05/09/10 |
| BND-12 | Evidence issuer ↔ store ↔ verifier/observer | claims, causal links, retention/redaction state | verifier; store enforces storage policy only | log/signature accepted as external truth; evidence tampering/privacy leak | EI-13/19/25; SBC-06/08 |
| BND-13 | Shared-limit authority ↔ competing branches | limit/reservation identity, reserve, commit/consume, release, expiry, recovery, reconciliation, epochs | named resource-scoped coordinator/escrow authority | overspend or capacity resurrection under crash/retry/race/partition/substitution | EI-09/11/18; SBC-03/04 |
| BND-14 | Tenant/branch/privacy boundary | state, metadata, correlation, diagnostics | deployment isolation + Profile policy | cross-tenant/branch leakage | EI-18/19; SBC-06 |
| BND-15 | Conformance observers/harness ↔ implementation | stimuli, independently derived observations, raw evidence, verdict inputs | qualified independent observers/adjudicator; minimum two paths for every applicable protected disclosure/effect SBC outcome | common-mode observer defect, harness changes race/outcome, hides failure or resolves disagreement optimistically | EI-22/25; SBC-08/09/10 |
| BND-16 | Normative spec/governance ↔ SDK/reference code | requirements, errata, releases, implementation behavior | open change process + conformance, never reference behavior alone | code silently becomes norm; vendor capture | EI-22/23; SBC-09/10 |

## 5. Boundary invariants для AA-2

1. Пересечение BND-05, BND-06A или BND-06B требует актуального explicit permit на точный Decision Subject; BND-05 дополнительно требует exact purpose, audience/observer, necessity, classification и allowed linkability.
2. Проверка раньше BND-05 либо BND-06B допустима только при доказуемой атомарной связи соответственно с первым observable disclosure signal либо resource commit.
3. BND-09 authentication/identity assertion никогда сама не создаёт authority.
4. BND-10 transport acknowledgement никогда не создаёт external-effect evidence.
5. BND-11 в native-to-foreign, foreign-to-native, chained и round-trip paths не подменяет source subject/Presenter, не импортирует/экспортирует Authority без отдельной проверки, не повышает assurance и блокирует зависимое действие при потере security-critical смысла.
6. BND-12 evidence остаётся scoped claim issuer, даже если оно подписано или хранится в immutable log.
7. BND-13 допускает reserve/commit/consume только при доказуемом safe remainder; release/expiry/recovery/reconciliation не воскрешают consumed capacity, а stale/unknown state всегда fail-closed.
8. Отказ за BND-08–BND-13 не расширяет authority и не изменяет Native Core semantics.
9. Каждая boundary имеет version/epoch, controller, provides/requires, failure owner и observable safe outcome.
10. Неизвестная либо неприменимая boundary assumption делает зависимый positive claim недопустимым.
11. Untrusted executor не считается BND-06B reference monitor. Положительный effect-safety claim требует, чтобы все in-scope mutation paths проходили через frozen BND-06B; полная compromise этой boundary либо неизвестный bypass приостанавливает claim, а не превращается в доказательство безопасности.
12. Для каждого применимого SBC, где возможен Protected Disclosure или Consequential Effect, BND-15 требует минимум два независимо выведенных observer paths; shared oracle/codebase не доказывает независимость, отсутствие пути или неразрешённое расхождение даёт `invalid`/`inconclusive`, никогда не `pass`.

## 6. Обязательная boundary record schema для AA-3

Для каждой BND-записи AA-3 заполняет:

| Поле | Требование |
| --- | --- |
| Scope/Profile | точные flows, domains и версии |
| Controller/owner | независимый controller и конфликт интересов |
| Assets | AST IDs и security/privacy objectives |
| Inputs/outputs | data, metadata, claims, side channels |
| Provides/requires | гарантии и необходимые assumptions |
| Enforcement point | кто технически может permit/deny/observe/commit; untrusted executor и trusted final reference monitor разделены; перечислены все bypass/mutation paths |
| Trust basis | почему verifier доверяет ровно этому утверждению |
| Freshness/epoch | точное правило и clock assumptions |
| Compromise capability | что может сделать полностью скомпрометированная сторона |
| Outage/partition behavior | safe-stop, degraded scope и blast radius |
| Detection/evidence | required signals и минимум два independently derived qualified observer paths для каждого применимого protected-disclosure/effect SBC outcome; common-mode dependencies и disagreement handling |
| Recovery | rotation, revoke, revalidation, claim suspension |
| Privacy | disclosure/linkability/retention/deletion bounds |
| Residual risk | только неблокирующий, owner и expiry |
| Verification | model/assertion/vector/review и Gate |

## 7. AA-3 completion slots

| Slot | Обязательное завершение | Блокирует |
| --- | --- | --- |
| TM-01 | Frozen claim scope и Profile-specific protected actions/disclosures | финализацию AA-3 |
| TM-02 | Asset objectives, owners и impact levels для AST-01–AST-14 | финализацию AA-3 |
| TM-03 | Полные adversary capabilities, collusion и compromise assumptions | финализацию AA-3 |
| TM-04 | Trust/data/enforcement diagrams и заполненные records BND-01–BND-16, включая разделённые BND-06A/B, все bypass/mutation paths и honest claim suspension при monitor compromise | финализацию AA-3 |
| TM-05 | Misuse/abuse cases и threat/failure trees | финализацию AA-3 |
| TM-06 | Prevention/detection/recovery mapping для SBC-01–SBC-10 | финализацию AA-3 |
| TM-07 | Freshness, revoke, clock и security-epoch policy | затронутые authority/effect claims |
| TM-08 | Privacy disclosure/linkability/metadata/timing/retention bounds | затронутые privacy claims |
| TM-09 | Shared-limit coordination/escrow/fail-closed choices и полный reserve/commit-consume/release/expire/recover/reconcile/substitution lifecycle | shared-limit flows |
| TM-10 | Key/credential/algorithm/provider compromise и migration plan | security-profile/binding shortlist |
| TM-11 | Supply-chain, build, dependency and release trust model | executable prototype/reference scope |
| TM-12 | Residual-risk register; unresolved SBC/Critical/High count | AA-3 exit; required count is `0` |

Если новая угроза, boundary или asset materially меняет AA-2 state/transition semantics, затронутая часть AA-2 автоматически открывается повторно до согласования.
