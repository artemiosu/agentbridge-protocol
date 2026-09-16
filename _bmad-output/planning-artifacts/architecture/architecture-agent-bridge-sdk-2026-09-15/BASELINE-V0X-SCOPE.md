---
title: AgentBridge AA-1 — Architecture Baseline v0.x Scope
status: final
created: 2026-09-16
updated: 2026-09-16
scope: horizontally-complete-vertically-bounded
authority:
  - AD-1
  - AD-2
  - AD-3
  - AD-22 (proposed; blocks AA-7 candidate selection)
---

# Architecture Baseline v0.x Scope

## 1. Решение обычным языком

Architecture Baseline v0.x проектируется **горизонтально полным, но вертикально ограниченным**.

Это означает:

- архитектура заранее охватывает общие правила, которые опасно добавлять после выпуска: роли, полномочия, асинхронность, повторы, неизвестный результат, делегирование, multi-party, версии, privacy, resource bounds и conformance;
- первая версия не пытается одновременно стать торговой моделью, банком, identity provider, policy engine, глобальным registry, облачной платформой или набором всех возможных Bridges;
- полнота Architecture не равна полноте будущей реализации: AA-6 утверждает проверенную архитектурную основу, а AA-7 отдельно проверяет две реализации и interoperability.

Этот scope сохраняет долгосрочную универсальность AgentBridge без преждевременного расширения Core.

## 2. Обязательные свойства baseline

Baseline строится как:

1. **Layered Semantic Thin Waist** — небольшой общий нормативный контракт между разнообразными системами сверху и механизмами снизу.
2. **Normative State-Machine Microkernel** — implementation-neutral факты, состояния, допустимые переходы и invariants, а не поведение одного SDK.
3. **Ports/Adapters Perimeter** — Profiles, Extensions, Bindings, Bridges и external infrastructure подключаются через явные contracts и не становятся скрытыми зависимостями Core.
4. **End-to-End Authority and Effect Rule** — transport acknowledgement, intermediary или подпись не заменяют проверку Authority и Effect на применимой границе.

## 3. Включённые нормативные model slices

### 3.1 Participants, roles and contexts

- различимые Participant, Principal, Actor, Presenter, Audience и Resource;
- динамические роли без привязки к организации или transport position;
- context/epoch/provenance boundaries;
- role-neutral и direction-neutral semantics во всех обязательных топологиях.

### 3.2 Bootstrap, capabilities and negotiation

- защита, необходимая до интерпретации negotiation;
- Core/Profile/Extension/Binding versions и requirements;
- freshness, mandatory/optional, deterministic selection и safe refusal;
- transcript binding и downgrade resistance;
- discovery interface без обязательного registry/provider.

### 3.3 Interaction graph

- request/response, events/subscriptions, streaming и long-running work;
- reverse asynchronous flows;
- multi-party branches;
- message/exchange/interaction/branch identity;
- correlation, causation, delivery, ordering и termination scope;
- reconnect/cancel/backpressure semantics на абстрактном уровне.

### 3.4 Decision Subject

- точная неизменная семантическая идентичность значимого решения/действия;
- допустимое refinement только в заранее ограниченном диапазоне;
- новое security-critical значение создаёт новый Decision Subject;
- отдельные Message ID, Operation ID и Decision Subject.

### 3.5 Authority, delegation and decisions

- явная authority chain и monotonic attenuation;
- actor/audience/resource/context/time/use/amount/rate constraints;
- Consent, Approval, Authorization и Obligations как разные стадии;
- quorum/all-of и independence requirements;
- revoke, expiry, policy/context change и compromise epochs;
- проверка на Protected Disclosure и Consequential Effect boundary либо доказуемо эквивалентная атомарная связь;
- contract с внешней authority для shared limits, когда локально доказать безопасный остаток невозможно.

### 3.6 Operation, lifecycle, effect and recovery

- различимые delivery, acceptance, authorization, execution, effect и outcome;
- состояния success, failure, Partial, Unknown и Conflict без ложной определённости;
- retry/replay, timeout и outcome retrieval;
- cancel, expiry и late completion;
- Compensation как новая отдельно разрешённая Operation;
- recovery без переписывания истории;
- явный отказ от общего обещания exactly-once.

Точный state-machine набор и композиция локальных автоматов определяются в AA-2 и проверяются формальной/исполняемой моделью. AA-1 не закрепляет непроверенный список wire states.

### 3.7 Evidence contract

- Claim, Evidence, Receipt, issuer/source, Observer/Attestor, subject, freshness, provenance и Assurance;
- связь с Decision Subject, Authority, branches, transformations, retries и effects;
- missing, redacted и conflicting evidence;
- assurance changes только по заранее нормативному trust rule;
- privacy-bounded causal reconstruction;
- отсутствие обязательного глобального журнала, ledger или blockchain.

### 3.8 Multi-party consistency

- dependency closure, membership и branch epochs;
- join/leave/branch/quorum changes;
- split-view safe behavior;
- aggregate outcomes, связанные с использованными epochs;
- branch privacy;
- внешняя сериализация shared limits только через явный contract.

### 3.9 Evolution and migration

- Core/Profile/Extension/Binding/Bridge version boundaries;
- critical/mandatory unknown handling;
- extension dependency closure и collision rules;
- coexistence старых и новых версий;
- migration, rollback, claim expiry и revalidation;
- vulnerability, compromise, erratum и dependency-change invalidation;
- отсутствие silent drift и обязательного flag day.

### 3.10 Privacy and bounded resources

- purpose/disclosure/linkability boundaries;
- metadata, timing, diagnostic и error-channel assumptions;
- classification, retention, deletion, residency и redaction obligations;
- bounded CPU, memory, state, storage, network, wait, payload, nesting, fan-out, retry и amplification;
- backpressure и deterministic safe-stop.

Численные ceilings принадлежат конкретному Profile/Binding и выбираются позднее на evidence, а не предполагаются AA-1.

### 3.11 Dependency and failure isolation

Каждая зависимость описывает:

- `provides`, `requires`, `terminates`;
- controller, trust/data/enforcement boundaries;
- version и compatibility scope;
- outage, partition, compromise и recovery behavior;
- substitution claim и его evidence;
- допустимый claim после отказа.

Отказ необязательного provider или Bridge не изменяет Native semantics и не расширяет Authority.

### 3.12 Conformance and claim model

- normative assertion catalog;
- positive, negative, boundary, adversarial, state и fault paths;
- property, fuzz, differential, concurrency и fault plans;
- observer qualification и evidence requirements;
- implementation-independent oracle skeleton;
- artifact/version/dependency/environment-bound claims;
- genealogy, expiry и revalidation;
- prevention/detection/recovery coverage SBC-01–SBC-10.

## 4. Обязательные топологии и проверочные домены

Один Core обязан быть непротиворечивым для:

1. agent ↔ service;
2. agent ↔ agent;
3. service/backend ↔ service/backend;
4. reverse asynchronous flow;
5. streaming/long-running flow;
6. multi-party/delegated chain;
7. взаимодействия с заменяемой external infrastructure.

До AA-6 общая модель проверяется минимум на трёх несвязанных классах сценариев:

- коммерческий consequential action;
- некоммерческое информационное/координационное взаимодействие;
- межорганизационная либо agent-to-agent delegated chain.

Эти сценарии проверяют переносимость Core, но не добавляют универсальную отраслевую онтологию.

## 5. Два начальных Profile

### 5.1 Information Exchange Profile v0.x

Назначение: доказать, что безопасный базовый обмен не обязан нести всю тяжесть consequential-action workflow.

Минимально задаёт:

- поддерживаемые interaction modes;
- correlation/causation scope;
- disclosure/privacy floor;
- freshness, limits, termination и errors;
- Evidence, необходимое для delivery/receipt claims;
- обязательные conformance assertions.

Он не разрешает представлять доставку информации как Consequential Effect.

### 5.2 Consequential Action Profile v0.x

Назначение: проверить полный путь `Authority → Decision Subject → Execution → Effect → Evidence`.

Минимально задаёт:

- required authority/delegation/consent/approval information;
- effect boundaries и verifier obligations;
- operation lifecycle и outcome projection;
- retry/replay/Unknown/Partial/Conflict;
- revoke, expiry, cancellation и Compensation;
- multi-party/quorum/delegation scenarios;
- SBC-01–SBC-10 coverage;
- conformance и evidence floor.

Profile остаётся domain-neutral. Торговые, банковские, гостиничные и иные операции определяются поздними domain Profiles/Extensions либо внешними системами.

## 6. Extension scope

Baseline включает общий Extension contract:

- namespace и authority;
- semantic version;
- prerequisites/dependency closure;
- optional/mandatory и safety-critical classification;
- negotiation и collision rules;
- unknown handling;
- effect на shared safety projection;
- conformance, migration и expiry.

Для проверки механизма нужны два conformance fixture, не обязательно публичные продуктовые Extensions:

1. безопасно игнорируемая additive Extension;
2. mandatory-to-understand security-relevant Extension, незнание которой приводит к fail-closed.

Extension не может переопределить Core/Profile, расширить Authority либо молча стать обязательной.

## 7. Native Binding scope

AA-1 определяет абстрактный Binding contract, но не выбирает язык, transport или encoding.

К AA-6 должна быть утверждена одна полная Native Binding, включающая:

- transport и fallback scope;
- encoding и canonical/signable representation;
- framing и message boundaries;
- conveyance identity/authority/evidence material;
- streaming, backpressure и cancellation behavior;
- transport/wire error mapping;
- resource ceilings и hostile-input handling;
- security mechanism composition;
- version negotiation и downgrade behavior;
- reproducible benchmark и conformance evidence.

Выбор проводится в AA-4, проходит scoped AA-3 security re-review и AA-5 conformance checks, затем утверждается в AA-6. Rust, Go, HTTP/2, HTTP/3, JSON, CBOR и любые другие кандидаты до этого остаются гипотезами.

Core не должен зависеть от конкретной Native Binding. По принятой поправке AD-3A AA-7 обязан проверить вторую, отличающуюся экспериментальную Native Binding. Эта ограниченная проверка способна выявить привязку к первому транспорту и дать scoped supporting evidence, но не доказывает независимость для всех будущих Bindings. Экспериментальная Binding не становится вторым обязательным Base Binding. Расхождение нормативного смысла между Bindings является провалом проверки, а не основанием создать два варианта Core.

## 8. Bridge scope

Baseline включает Bridge architecture, но не требует production Bridge к конкретному внешнему протоколу до AA-6.

Bridge contract обязан задавать:

- source/target protocol и точные versions;
- направление mapping;
- provenance;
- total/partial/unmapped semantics;
- security-critical semantic-loss matrix;
- assurance ceiling;
- identity/authority/effect/evidence translation limits;
- loop, impersonation, replay и version-drift protection;
- failure isolation и safe refusal;
- отдельный Bridge conformance claim.

Bridge не входит в Native Path. Наличие AgentBridge conformance не означает Bridge conformance и наоборот. По принятой поправке AD-3A AA-7 тестирует необязательные Bridges как минимум к двум различным внешним протокольным моделям. Они существуют только для проверки translation/loss/assurance contract, не становятся зависимостями AgentBridge и не заменяют отдельную scoped проверку прямого Native interoperability. Та же AA-7 matrix использует два несвязанных non-production Domain Profiles и два независимо определённых Extensions в дополнение к двум domain-neutral verification Profiles Architecture Baseline.

## 9. External infrastructure boundary

Вне Core остаются:

- identity/credential issuers и verifiers;
- policy engines и Policy Authorities;
- discovery/registry providers;
- domain systems of record;
- effect executors;
- payment rails;
- shared-limit authorities;
- evidence/audit stores;
- time, transparency и attestation services;
- AgentBridge Cloud и иные коммерческие платформы.

AgentBridge определяет interface requirements, claims, failure semantics и verification obligations, но не объявляет один provider обязательным и не принимает его утверждение за безусловную внешнюю истину.

## 10. Что исключено или отложено

Architecture Baseline v0.x не включает:

- универсальную отраслевую онтологию;
- product, booking, banking, healthcare, advertising или government data models;
- собственные cryptographic algorithms;
- глобальный identity provider, policy language, registry, broker или ledger;
- payment rail, escrow или dispute-resolution system;
- обязательный AgentBridge Cloud;
- внутреннюю архитектуру коммерческой платформы;
- SDK suite для заранее выбранных языков;
- production deployment architecture;
- полный набор transport/encoding Bindings;
- полный набор MCP/A2A/UCP/AP2/OpenAPI и иных Bridges;
- автоматическое доказательство внешней domain truth;
- production, adoption или industry-standard claims;
- коммерческие тарифы, комиссии, рекламу и marketplace как Core semantics.

Отложенное решение получает owner, зависимый Gate и revisit condition; оно не остаётся скрытым предположением реализации.

## 11. Allocation и verification completeness

AA-6 не может выдать положительный verdict, пока:

1. 100% current FR-1–FR-110 and retained NFR-1–NFR-29 have a primary owner, supporting layers, normative artifact, verification method, accountable owner role, AD/QAS/threat-boundary trace and closure Gate; retained F1–F8 validation-gate acceptance clauses are tracked in addition to the numbered IDs.
2. 100% EI-01–EI-25 имеют архитектурное правило, positive path, отдельный falsifier и применимое topology/domain coverage.
3. Каждый SBC-01–SBC-10 имеет prevention, detection, recovery, evidence, blocker attribution и closure route.
4. Каждый элемент Core прошёл Universality Test и Removal Test.
5. Ни одно обязательное значение не существует только в SDK, reference behavior, Bridge, provider или частном пояснении автора.
6. Все обязательные топологии имеют непротиворечивые scenarios и context mappings.
7. Каждый внешний dependency имеет failure, compromise, substitution и claim-impact semantics.
8. Profile/Extension/Binding/Bridge не ослабляют Core и имеют отдельные conformance scopes.
9. Независимый implementer может вывести expected result из равнодоступного normative package без reference behavior.
10. Каждый открытый вопрос классифицирован как blocker конкретного Gate, Profile/Binding-specific либо deferred с revisit condition.

## 12. Universality и Removal gates для Core

Элемент допускается в Core, только если:

- он нужен всем обязательным топологиям либо их общей safety-модели;
- имеет один смысл в нескольких несвязанных доменах;
- без него независимые реализации могут принять несовместимые решения;
- его нельзя безопасно локализовать в Profile;
- он не требует конкретного transport, encoding, provider или domain source of truth;
- он независимо проверяем;
- он не делает необязательную инфраструктуру фактически обязательной.

Removal Test спрашивает: если убрать элемент из Core, сохранятся ли native interoperability, EI/SBC, одинаковый allowed set и safety projection без pair-specific соглашения? Если да, элемент переносится из Core.

## 13. Граница AA-6 и AA-7

Before AA-6 authorizes AA-7 candidate selection or implementation, proposed **AD-22** in `AA-7-DIVERSITY-ELIGIBILITY.md` must be adopted and frozen. It supplies deterministic eligibility tests for the second experimental Binding, unrelated Domain Profiles, independently defined Extensions and distinct foreign protocol-model families. Unknown, disputed or conflicted eligibility is `ineligible`, not a condition accepted after observing AA-7 results.

### Что доказывает AA-6

AA-6 подтверждает только, что:

- интегрированная Architecture Baseline v0.x непротиворечива в утверждённом scope;
- normative model, threat model, Profiles, выбранная Native Binding и conformance architecture согласованы;
- все обязательные architecture blockers закрыты;
- точно определён scope первой non-production реализации;
- reference/spec-only separation, rights, dependency, sandbox, resource и evidence controls готовы.

For rights, “ready” is a hard pre-implementation condition: before AA-6 may authorize AA-7 work, every assigned implementer must have equal access and a royalty-free experimental implementation grant without field-of-use restrictions, with provenance and applicable patent/license blockers closed. Permanent public specification/conformance licenses, contribution terms, RF/non-assert commitments and publication/marks controls remain AA-8 conditions. Accordingly, AD-15 has two blocking milestones even if its register entry is later presented as one publication decision: **experimental rights at AA-6 exit / pre-AA-7**, and **permanent publication terms at AA-8**.

AA-6 **не доказывает**, что две реализации действительно совместимы, что производительность достаточна в production или что внешние пользователи смогут реализовать протокол без помощи.

### Что проверяет AA-7

AA-7 создаёт reference и spec-only implementations без общей protocol-semantic codebase и проверяет:

- independent implementability;
- bidirectional interoperability;
- conformance и negative/fault behavior;
- security, resilience, performance и upgrade properties;
- genealogy и отсутствие скрытой reference dependence.

AA-7 must also execute the retained F1–F8 acceptance matrix recorded in `ALLOCATION-MATRIX.md`. Under adopted amendment AD-3A this includes the mandatory Base Binding plus a second distinct experimental Native Binding, two unrelated non-production Domain Profiles, two independently defined Extensions and separately tested optional Bridge mappings to at least two foreign protocol models.

The words `distinct`, `unrelated` and `independently defined` have the binding eligibility meaning in AD-22. Cosmetic wrappers, renamed domain schemas, coordinated Extension splits, protocol versions/forks and undisclosed shared semantic code do not satisfy the matrix.

Успешный AA-7 даёт только внутренний **non-production Implementation/Interop Candidate**. Он не разрешает публичный Developer Preview (AA-8), production use (AA-9), adoption claim (AA-10) или industry-standard claim (AA-11).

## 14. Итоговая граница

Baseline v0.x является полным фундаментом общих semantics, а не полной реализацией будущей экосистемы. Он обязан сделать ошибки Core обнаруживаемыми до кода и оставить домены, механизмы и поставщиков заменяемыми без потери safety meaning.
