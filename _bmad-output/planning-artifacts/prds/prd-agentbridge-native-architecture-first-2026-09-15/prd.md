---
title: "PRD: AgentBridge — Native Architecture-First"
status: final
revision: 2
created: 2026-09-15
updated: 2026-09-15
visibility: private-repository
mode: approved-course-update
supersedes: ../prd-agent-bridge-sdk-2026-09-12/prd.md
governing_decision: ../../course-decision-native-architecture-first-2026-09-15.md
retained_requirements: retained-requirements-baseline.md
---

# PRD: AgentBridge — Native Architecture-First

## 0. Назначение и приоритет

Этот PRD — действующая продуктовая редакция AgentBridge. Он сохраняет подтверждённое видение и требования предыдущей редакции, но заменяет её outcome-neutral Gate 1 решением Project Owner: **Native AgentBridge Protocol проектируется и, после архитектурных ворот, реализуется**.

Course Decision заменяет только явно названные existential/comparative правила старого Gate 1. Retained policy baselines являются совместно обязательными ограничениями и не могут быть ослаблены обычной редакцией PRD или Charter. Их изменение требует versioned change record, затронутых независимых reviews и отдельного осознанного принятия Project Owner.

При отсутствии такого конфликта рабочий порядок чтения:

1. этот PRD;
2. Architecture Assurance Charter;
3. неизменяемый `retained-requirements-baseline.md` и retained EI/SBC/policy baselines старого Validation Charter;
4. исторический PRD и исследования как rationale/evidence, но не как текущие команды.

Приватные исходные документы остаются локальным контекстом и не входят в публикуемые артефакты.

## 1. Видение и проблема

AgentBridge должен стать открытым, role-neutral и direction-neutral protocol suite для будущего агентного интернета. Независимо созданные агенты, сервисы, API, backend-системы и инфраструктурные компоненты должны безопасно взаимодействовать без отдельного закрытого соглашения и протокольного адаптера для каждой пары.

Поддерживаемые семейства взаимодействий включают agent↔service, agent↔agent, service↔service/B2B, reverse asynchronous, streaming/long-running и multi-party/delegation flows. Организационные категории не являются постоянными wire-ролями: одна сторона может быть principal, delegate, initiator, responder, executor, approver, policy authority или attestor в разных контекстах.

Проблема существующего ландшафта состоит не только в разных форматах сообщений. Доставка сообщения, идентификация стороны, полномочие, согласие, исполнение, внешний эффект и доказательство результата часто смешиваются или остаются предметом частного соглашения. Для consequential actions это создаёт риск выполнения не того действия, не тем субъектом, с расширенными полномочиями, повторно либо с ложным представлением неизвестного результата как успеха.

Долгосрочная амбиция — стать общепринятой протокольной основой агентного интернета. Это цель, а не текущее заявление о доказанной универсальности, production readiness или статусе стандарта.

## 2. Подтверждённый продуктовый курс

### 2.1 Native Architecture-First

- Native Path не зависит от MCP, A2A, UCP, AgentBridge Cloud, обязательного broker/registry или другого agent-protocol runtime.
- Существующие стандарты используются как источники проверенных primitives, архитектурных уроков, failure modes и optional compatibility bridges.
- Architecture, formal/executable models, threat model и conformance design начинаются до reference implementation.
- Production-код не является источником нормы: спецификация и conformance имеют приоритет над reference behavior.
- Rust, Go, transport, encoding, cryptographic profile, SDK languages, cloud и repository layout остаются решениями Architecture, подтверждаемыми требованиями и испытаниями.

### 2.2 Значение самодостаточности

Самодостаточность означает целостный agent/application-level protocol suite, а не переизобретение Интернета. AgentBridge не создаёт собственные криптографические алгоритмы, глобальный identity provider, policy engine, payment rail или отраслевую систему истины. Проверенные нижние механизмы могут использоваться через точные заменяемые Bindings/Profiles после security/IP review.

### 2.3 Принцип визионерского проектирования

Сегодняшние паттерны не являются потолком. Необычное решение допустимо, если оно повышает безопасность, полноту, эффективность или evolvability и выдерживает threat modeling, formal/executable checks, independent implementation, conformance, operational analysis и сравнение с более простыми альтернативами. Новизна сама по себе ценностью не считается.

## 3. Пользователи и Jobs-to-be-Done

| Участник | Job-to-be-Done | Желаемый результат |
| --- | --- | --- |
| Разработчик/оператор агента | Подключать агента к независимым сервисам и агентам без отдельной семантической интеграции для каждого | Меньше pair-specific glue, предсказуемые состояния, безопасное выполнение |
| Разработчик/оператор сервиса, API или backend | Публиковать возможности и безопасно принимать взаимодействия от внешних программных участников | Управляемые полномочия, версии, диагностика и auditability |
| B2B-интегратор | Строить межорганизационные цепочки agent/API/backend | Единые роли, delegation, lifecycle, evidence и failure semantics |
| Principal — человек или организация | Делегировать ограниченные полномочия и контролировать значимые действия | Consent/approval, ограничения, revoke и проверяемый итог |
| Независимый implementer/auditor | Реализовать и проверить протокол без закрытых пояснений автора | Однозначная specification, self-runnable conformance и воспроизводимые claims |

Human UI и reasoning агента не входят в Core. Протокол переносит необходимые consent/approval/evidence semantics, но не определяет пользовательский интерфейс или внутреннее мышление агента.

## 4. Карта возможностей

F1–F8 и FR-1–FR-95 из предыдущего PRD сохраняют стабильные ID и нормативный смысл:

- **F1 / FR-1–FR-9:** участники, principals, роли, audience, scope, provenance и correlation;
- **F2 / FR-10–FR-19:** capabilities, discovery interface, negotiation, freshness и downgrade resistance;
- **F3 / FR-20–FR-29:** request/response, events, subscriptions, streaming, long-running, reverse и multi-party coordination;
- **F4 / FR-30–FR-42:** authority, delegation, consent/approval, attenuation, revoke и confused-deputy resistance;
- **F5 / FR-43–FR-55:** operation identity, lifecycle, effect, partial/unknown, retry, cancel, compensation и evidence;
- **F6 / FR-56–FR-67:** Core/Profile/Extension/Binding versions, compatibility и controlled evolution;
- **F7 / FR-68–FR-81:** независимый Native Path, replaceable Bindings и optional fail-closed Bridges;
- **F8 / FR-82–FR-95:** conformance, genealogy, independent implementability, negative/fault testing и scoped claims.

Точный источник, anchors и digests этих требований зафиксированы в `retained-requirements-baseline.md`. Исторический PRD остаётся доказательным источником, но устаревшие F9/Gate 1 положения не импортируются.

### 4.1 F9 — Architecture Assurance и controlled progression

FR-96–FR-110 сохраняют ID, но их прежняя comparative decision-lattice семантика superseded Course Decision от 2026-09-15.

#### FR-96 — Architecture Assurance Charter

До нормативного design и кода действует версионируемый Charter с входами, результатами, blockers, review roles, evidence и разрешёнными действиями каждого этапа.

#### FR-97 — Landscape learning без обязательной runtime-композиции

Актуальные стандарты и реализации регулярно проверяются для reuse, compatibility, known defects и bridge opportunities. Их популярность или наличие не отменяют Native direction и не делают их обязательными зависимостями.

#### FR-98 — Полный, но дисциплинированный Native Core

Core включает все доказанно общие обязанности, необходимые для безопасной одинаковой семантики в обязательных топологиях. Любой элемент проходит Allocation и Removal Test; доменная или инфраструктурная обязанность не включается в Core без необходимости.

#### FR-99 — Архитектурная traceability

Каждое нормативное решение связано с FR/NFR, EI/SBC, threat/quality scenario, ADR, verification method и владельцем. Изменение критического смысла требует новой версии и применимого review.

#### FR-100 — Cross-domain и cross-topology validation

Один Core проверяется во всех обязательных топологиях и минимум на коммерческом, некоммерческом и межорганизационном/agent-to-agent срезах. Разнообразие доменов не заменяет разнообразие топологий.

#### FR-101 — Непреодолимые safety, rights и evidence gates

Любой незакрытый SBC блокирует независимо от severity. Также блокируют Critical/High, обязательный gate failure, неустановленные права/license/patent/provenance, юридический запрет, отсутствие обязательной независимости, evidence или conformance. Такой blocker нельзя принять как residual risk.

#### FR-102 — Независимая реализуемость до reference behavior

До первого reference-кода замораживаются normative digest/package, implementation-independent oracle/conformance skeleton, access/exposure/genealogy и adjudication rules. Spec-only/clean-room track начинается до доступа к reference behavior либо в доказуемой изоляции.

#### FR-103 — Formal/executable verification критической семантики

Authority, delegation, lifecycle, effect, retry/replay, cancel/revoke, concurrency и failure semantics проверяются подходящей формальной или исполнимой моделью до соответствующей reference implementation.

#### FR-104 — Dependency и failure isolation

Core/Binding/Bridge boundaries должны явно показывать trust, availability, version и failure ownership. Отказ или compromise необязательной внешней системы не должен скрыто изменять Native semantics или расширять полномочия.

#### FR-105 — Performance и bounded resources

Transport/encoding/runtime решения принимаются по воспроизводимым benchmarks и обязательным пределам CPU, memory, state, I/O, payload, depth, fan-out, retries, amplification и time. Производительность не оправдывает ослабление correctness/security/interop.

#### FR-106 — Evolvability и безопасная миграция

Версии, обязательные/необязательные расширения, downgrade, staged upgrades, rollback, drift и bridge expiry имеют однозначные правила; неизвестность security-critical смысла завершается fail-closed.

#### FR-107 — Воспроизводимый evidence package

Решения сохраняют manifests/digests, версии входов, models, vectors, raw/derived results, negative/inconclusive evidence, dissent и scope claims. Публикация подчиняется отдельным rights/privacy/security gates.

#### FR-108 — Специализированные независимые reviews

Каждый Architecture Gate проходит применимые protocol, distributed systems, security/identity/privacy, formal methods, networking/performance, conformance/DX, governance/IP и cross-discipline red-team reviews. AI review — внутренний анализ, а не внешнее мировое признание.

#### FR-109 — Deterministic gate outcomes

Каждый gate выдаёт `pass`, `pass with conditions` или `redesign/block`. Blocker обязан назвать нарушенное правило, evidence, исправление и критерий закрытия. Dissent сохраняется, но не создаёт бесконечный цикл.

#### FR-110 — Scoped permission, redesign и emergency stop

Gate разрешает только следующий указанный scope. Failure обычно требует redesign, сужения, Profile/Binding allocation или defer. Emergency pause/stop проекта допустим при неустранимой небезопасности, юридической невозможности royalty-free Core, доказанной практически нереализуемой сложности либо невозможности независимой реализации. Недостаток ресурсов означает `pause / re-scope / seek resources / archive until funded`, а не техническое доказательство несостоятельности; окончательный stop требует определённого evidence, применимой I2-рекомендации и отдельного решения Project Owner.

## 5. Product goals и non-goals

### 5.1 Цели

1. Определить целостный role-neutral Native Core и безопасные границы Profiles/Bindings/Bridges.
2. Сделать authority → action → effect → evidence цепочку однозначной и проверяемой без ложного exactly-once.
3. Обеспечить прямую совместимость независимых реализаций без обязательной центральной платформы.
4. Проектировать conformance одновременно с нормативной семантикой.
5. Обеспечить безопасную эволюцию, заменяемость инфраструктуры и incremental migration.
6. Подготовить Developer Preview, затем ограниченное production use и внешнее adoption через разные evidence gates.
7. Создать устойчивую открытую экосистему и отдельный, не захватывающий Core путь коммерческой выгоды основателя.

### 5.2 Постоянные non-goals

- агент, UI, reasoning framework или универсальный workflow engine;
- универсальная доменная онтология;
- собственная криптография, identity provider, policy engine, payment rail или system of record;
- обязательный AgentBridge Cloud, broker, registry, commercial SDK или чужой agent runtime;
- обещание exactly-once либо признание подписи доказательством внешней истины;
- скрытое semantic loss в Bridge;
- закрытый conformance, pay-to-pass или protocol toll;
- реклама/paid ranking, влияющие на нормативный ответ, безопасность выбора или волю Principal;
- заявление о статусе индустриального стандарта до независимого adoption и нейтрального governance.

## 6. Нефункциональные требования

NFR-1–NFR-29 предыдущего PRD сохраняются. Architecture обязана сделать их измеримыми для выбранного scope. Без изменения ID сохраняются требования по:

- security/privacy и least authority;
- fail-closed uncertainty, replay/downgrade/TOCTOU safety;
- resilience, bounded resources, backpressure и recovery;
- performance без ослабления correctness;
- implementation/language/vendor neutrality;
- observability с доказанной полнотой effect observers;
- data, dependency, supply-chain и provenance controls;
- open royalty-free independent implementation;
- protocol/platform firewall и publication discipline.

## 7. Architecture Assurance Gates

| Gate | Результат | Что разрешает |
| --- | --- | --- |
| AA-0 Course Decision | Native Architecture-First утверждён | Новую редакцию PRD/Charter и Architecture |
| AA-1 Constitution | Границы, принципы, EI/SBC allocation, quality scenarios | Abstract normative design |
| AA-2 Normative Model | Проверенные roles/authority/lifecycle/effect/version models | Security architecture |
| AA-3 Security/Privacy/Trust | Threat model; 0 SBC/Critical/High/mandatory failures | Wire/binding exploration |
| AA-4 Wire/Binding/Runtime | Native Binding и bounded benchmark evidence | Conformance architecture |
| AA-5 Conformance | Assertions, vectors, observers, adversarial/fault coverage | Integrated review |
| AA-6 Architecture Baseline | Согласованный v0.x baseline и точный implementation scope | Non-production reference + clean-room implementation |
| AA-7 Implementation/Interop | Внутренний Implementation/Interop Candidate | Public-readiness review |
| AA-8 Public Draft | IP/governance/security/conformance ready | Публичный `0.x Developer Preview` |
| AA-9 Limited Production Safety | Отдельный domain/jurisdiction safety/compliance Charter | Только явно ограниченный production scope |
| AA-10 External Adoption | Независимое внедрение и retention; отдельный Adoption Charter | Ограниченные adoption claims |
| AA-11 Open-Standard Legitimacy | Независимый adoption + нейтральный change process | Обоснованный standards-body/industry-standard path |

Точные входы, blockers и anti-endless правила определяет Architecture Assurance Charter.

## 8. Success metrics и counter-metrics

### 8.1 Архитектурный успех

- 100% EI-01–EI-25 распределены и проверяемы;
- 0 unresolved SBC, Critical/High и mandatory gate failures в проходящем scope;
- 100% security-critical state transitions имеют model/test/review evidence;
- 100% normative obligations имеют conformance assertion, model либо обязательный review;
- bounded resource behavior определён для mandatory paths;
- две независимые реализации достигают заявленной interop без закрытых пояснений;
- Native Path работает без обязательной коммерческой/чужой runtime-зависимости;
- version/extension/bridge failures не создают silent downgrade.

### 8.2 Поздние отдельные доказательства

- Production safety не считается adoption.
- Adoption не считается willingness-to-pay.
- Минимальный исходный adoption floor — не менее четырёх независимо контролируемых implementations/deployments, если отдельный Charter не установит более строгий порог.
- `industry standard` требует отдельно определённого широкого adoption и нейтрального управления.
- Для каждого платного слоя нужны buyer, problem, value metric, willingness-to-pay, retention и kill-критерий.

### 8.3 Counter-metrics

- рост Core surface/mandatory dependencies;
- число закрытых пояснений, необходимых implementer;
- число несовместимых interpretations/bindings;
- доля non-conformant extensions и lossy bridges;
- Critical/High/SBC recurrence;
- resource amplification и tail regressions;
- зависимость adoption от компании основателя;
- доля коммерческих функций, без которых фактически невозможна базовая interop;
- incidents, exits, forks и claim downgrades.

## 9. Commercial and ecosystem boundary

Открытый стандарт и коммерческая компания основателя — связанные, но институционально разделённые системы.

Открытыми и royalty-free остаются normative Core, обязательные Base Bindings, security/interoperability-critical semantics, conformance rules/suite/vectors и достаточный client/operator SDK baseline.

Компания основателя может продавать добровольные design-partner integrations, support/SLA, hosted developer tooling, managed operations, enterprise security/compliance/on-prem, bridges/connectors, incident response, federated discovery/trust и позднее transaction/risk/dispute services. Ни один слой не получает особого голоса, закрытого conformance или обязательности для Native Path.

Отдельный Business Validation Track и будущий Business & Ecosystem Strategy обязаны определить Protocol/Platform Firewall, ownership/recusal/sponsor rules, portability/provider-exit, certification appeal, neutral funding, stewardship milestones и карточку доказательства каждого платного слоя. Их первичная редакция обязательна до самого раннего из событий: paid design-partner work, принятие external contributions, AA-8 publication, регистрация company/foundation либо передача protocol assets/rights.

## 10. Риски

| Риск | Обязательная реакция |
| --- | --- |
| Core пытается охватить весь будущий Интернет | Thin-waist allocation, Removal Test, domain semantics в Profiles |
| Подпись принимается за внешний факт | Issuer/claim/verifier/effect-source separation |
| Authority меняется между проверкой и эффектом | Проверка на effect boundary либо доказуемая связь; иначе explicit partial/unknown |
| Retry/cancel/partition создают double effect | Operation identity, durable state, no unsafe retry, fault models |
| Binding/Bridge ослабляет смысл | Version-scoped fail-closed mapping и declared loss |
| Reference code становится скрытой нормой | Spec-first digest, clean-room track, independent oracle |
| Privacy конфликтует с audit | Минимизация, selective evidence, retention/deletion и scoped disclosure |
| Неограниченный fan-out/state/queue | Aggregate ceilings, backpressure, safe-stop и evidence preservation |
| Техническое качество ошибочно принимается за adoption | AA-9/10/11 и Business Validation разделены |
| Коммерческая компания захватывает стандарт | Protocol/Platform Firewall, neutral governance, portability и succession |

## 11. Open decisions и следующий этап

Architecture должна решить, не предрешая ответ:

1. точную границу Core/Profile/Binding/Bridge;
2. минимальные role-neutral objects и state machines;
3. authority/effect/verifier contract и trust roots;
4. canonical representation и signing coverage;
5. Native transport/encoding Binding и fallback;
6. Rust, Go или другой reference runtime;
7. streaming/backpressure/resource model;
8. version/extension negotiation и migration;
9. conformance oracle, vectors и independent implementation package;
10. первый целостный implementation slice;
11. governance/IP controls до публичного draft;
12. технически допустимые candidate slices для будущего adoption wedge; выбор и проверка рыночного клина принадлежат Product/Business, а не Architecture.

Следующий BMAD-этап после финализации этой редакции и Charter — **BMAD Architecture**, начиная с AA-1 Architecture Constitution. Production implementation, public release, расходы и внешние договоры этим PRD не разрешаются.
