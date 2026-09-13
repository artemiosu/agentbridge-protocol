---
title: "Техническое исследование: Clean-Slate AgentBridge Protocol"
type: technical
shape: select
topic: "Clean-Slate AgentBridge Protocol Architecture, Security & Performance"
decision: "Выбрать native AgentBridge, profile/composition, native core с optional bridges либо upstream/stop"
source: "native web research using primary public sources; post-research scope clarification is a user-confirmed project requirement, not external evidence"
status: complete
preset: deep
validation: high
claims_verified: 30
claims_unverified: 10
claims_overturned: 2
claims_total: 42
created: 2026-09-12
updated: 2026-09-12
---

# Техническое исследование: Clean-Slate AgentBridge Protocol

## Executive summary

**Gate 0: условно выбрать вариант 3 — нативное семантическое ядро AgentBridge с необязательными, изолированными и fail-closed bridges — как следующую проверяемую гипотезу.** Две нативные реализации должны взаимодействовать напрямую без MCP, A2A, UCP, AgentBridge Cloud, broker или иного agent-протокольного runtime. Это сохраняет первоначальную цель полноценного самостоятельного протокола, но не требует изобретать собственные транспорт, криптографию, OAuth-замену, платёжный rail или отраслевой источник истины.

После завершения исследования пользователь подтвердил более широкий product scope: AgentBridge должен быть нейтральным к ролям и направлениям и охватывать agent↔service, agent↔agent, service↔service/B2B, обратные асинхронные потоки, многосторонние цепочки и взаимодействие участников с заменяемой discovery/trust/policy/audit-инфраструктурой через явно определённые bindings. Это подтверждённое требование проекта, **а не новый результат публичного исследования**. Поэтому вывод Gate 0 сохраняет выбранную архитектурную форму, а следующий PRD обязан проверить, способен ли один минимальный core действительно сохранять общие инварианты во всех этих топологиях. Consequential agent-to-service action остаётся первым высокорисковым проверочным срезом, но не границей архитектуры.

Главный технический аргумент — не «существующие протоколы плохие», а **у них иные границы ответственности**. MCP стандартизирует контекст и tools; A2A — agent-to-agent tasks; OpenAPI/Arazzo — описание API и workflows; OAuth/GNAP/AuthZEN — выдачу и проверку полномочий; UCP и два commerce-ориентированных стека ACP/AP2 — коммерческие сценарии. Ни один из изученных общих стандартов не определяет единую нейтральную к предметной области модель обработки, которая сквозным образом связывает точную идентичность действия, цепочку делегирования, одобрение, исполнение, неизвестный или частичный эффект, компенсацию и проверяемое утверждение о результате [9][11][14][15][19][21][26][27][30][50][51].

Одновременно вариант «идеальный полностью новый стек без bridges» проигрывает. Он немного проще по обязательному runtime path, но создаёт худшую миграцию, повышает риск неофициальных небезопасных адаптеров и игнорирует доказанные факторы принятия протоколов: инкрементальное развёртывание, совместимость, открытые реализации и положительная ценность для раннего участника [2][5]. **Необязательный bridge не входит в нативный AgentBridge и не обязательно добавляет сетевой переход.** Это пограничный компонент с отдельной матрицей версий и запретом на скрытое понижение гарантий.

Ключевая оговорка: исследование подтверждает **архитектурную гипотезу**, но ещё не доказывает рыночное принятие и не позволяет выбрать конкретный формат передачи данных, транспорт или язык реализации, включая Rust. Публичные данные не позволяют честно утверждать, что Rust быстрее Go на репрезентативной AgentBridge-нагрузке, что HTTP/3 всегда лучше HTTP/2 или что CBOR всегда лучше JSON [33][34][36][37][38][39][41][42]. Эти решения должны пройти воспроизводимые benchmarks и cross-language conformance.

Рекомендуемая граница AgentBridge — **тонкое, полноценное и самостоятельное role-neutral interaction core**, внутри которого профиль «действие — полномочие — эффект» задаёт строгие обязательные гарантии для значимых действий. Общий core должен быть пригоден для запросов и ответов, событий и подписок, потоковой и длительной работы, согласования, обратных сообщений и многостороннего взаимодействия; конкретный минимальный состав ещё предстоит доказать. Доменные объекты торговли, гостиниц, банков и иных отраслей остаются в профилях; UCP, Agentic Commerce Protocol и AP2 подключаются через bridges там, где их смысл можно сохранить. Если независимые прототипы покажут, что необходимый core полностью выражается существующими стандартами без новых обязательных для понимания объектов или распадается на несовместимые модели для разных топологий, работу над отдельным ядром следует остановить либо сузить, а изменения направить в существующие инициативы.

## 1. Вопрос, метод и границы доказательства

Исследование сравнивает четыре пути:

1. **Native без официальных bridges** — самостоятельный AgentBridge suite и только нативный путь.
2. **Permanent profile/composition** — AgentBridge как обязательный профиль или композиция MCP/A2A/UCP/OpenAPI/GNAP и других стандартов.
3. **Native core + optional bridges** — самостоятельная processing model и нативный путь; совместимость находится на краях и не ослабляет core.
4. **Upstream/stop** — не создавать отдельный core, а вносить недостающие элементы в существующие инициативы.

Жёсткие критерии отбора были зафиксированы до формирования выводов: нативная независимость; protocol+SDK, а не агент/интерфейс/агрегатор; открытая royalty-free implementability; end-to-end сохранение security-critical meaning; fail-closed bridges; отсутствие собственной криптографии, глобального IdP, платёжного rail или отраслевой системы записи.

Последующее уточнение пользователя расширило intended scope до role-neutral и direction-neutral взаимодействия во всех основных топологиях. Оно не меняет источников или ретроспективно установленные критерии Gate 0 и не считается внешним доказательством; оно создаёт дополнительное обязательство проверки для PRD и прототипов.

Материал собран по состоянию на **12 сентября 2026 года** из RFC, официальных спецификаций, репозиториев, issue trackers, оригинальных исследований и standards-body records. Issue reports используются только как подтверждение существования заявленной проблемы, а не как доказанная уязвимость. Отсутствие функции означает лишь, что она не найдена в просмотренной нормативной поверхности. Приватные документы проекта определяли вопрос и требования, но не использовались как доказательства.

## 2. Что уже закрыто существующими инициативами

| Инициатива | Сильная сторона | Граница для AgentBridge | Вывод для архитектуры |
| --- | --- | --- | --- |
| MCP 2026-07-28 | Stateless requests, tools/context, capability/version negotiation, extensions | Tasks — отдельное расширение; общий action-to-effect contract отсутствует в рассмотренной поверхности | Необязательный tool bridge, не native substrate [9][10] |
| A2A 1.0.x | Rich task lifecycle, streaming/polling/webhooks, несколько bindings | Identity внешняя; cancellation best-effort; idempotency SendMessage optional | Необязательный agent/task bridge [11][12][13] |
| OpenAPI 3.2 | Описание HTTP interfaces и security schemes | Не определяет runtime effect, cancellation, rollback или idempotency | Источник capability/action schema, не processing model [14] |
| Arazzo 1.1 | Последовательности API-вызовов, criteria, retry/goto/end | Нет общей atomicity, compensation, exactly-once или proof-of-effect | Workflow bridge; исполнять только доверенные descriptions [15] |
| OAuth RAR / Token Exchange / DPoP | Typed authorization details, actor vocabulary, sender-constrained tokens | Значение action и downstream effect остаются profile/application-specific; DPoP не подписывает payload | Повторно использовать primitives, не отдавать им action semantics [21][22][23] |
| AuthZEN 1.0 | Унифицированный PDP/PEP authorization decision API | Не является action protocol; obligations и enforcement требуют профиля | External policy decision binding [26] |
| GNAP RFC 9635 | Clean-slate grant/consent lifecycle, key-bound continuation, management | Resource action/effects вне core; revoke не отменяет свершившийся эффект | Сильный authority provider/bridge и контрольный кандидат [27] |
| UCP 2026-08-25 | Commerce capabilities, exact-version profiles, constraints, action trust boundaries, signatures, REST/MCP/A2A composition | Commerce-oriented; outcome/effect semantics capability-specific | Не дублировать commerce; использовать как bridge/profile [17][50] |
| Agent Communication Protocol (BeeAI) | Исторический agent communication protocol | Вошёл в A2A | Только migration bridge; не путать с commerce ACP [18] |
| Agentic Commerce Protocol (OpenAI/Stripe) | Checkout/cart/feed/order/auth/payment, schemas и MCP integration | Beta, commerce-specific | Отдельный commerce bridge; не общий core [51] |
| AP2 v0.2 | Mandates и payment authorization/receipt patterns | Требует underlying commerce protocol; не покрывает общий execution lifecycle | Payment-security extension/bridge [19][20] |
| SCITT | Подписанные statements, transparency receipts, verifier-selected trust | Receipt доказывает регистрацию утверждения, не истинность внешнего эффекта | Контейнер evidence, не oracle [30] |

### Важное обновление UCP

Первый проход увидел GitHub Releases, где `v2026-04-08` по-прежнему показан как latest. Официальный сайт UCP, однако, публикует `v2026-08-25` и живую versioned specification. Поэтому версия `2026-04-08` как текущая **опровергнута**; актуальная — `2026-08-25` [50].

Новая редакция — сильнейшее возражение против отдельного AgentBridge в commerce. Она формализует request constraints и их lifecycle, отделяет Action surface от доказательства эффекта, проверяет namespace authority, выбирает exact versions и требует покрывать signatures критические request components. Но успешный preflight не означает успешное действие, а смысл результата остаётся у capability. Следовательно, AgentBridge не должен копировать UCP, но domain-neutral action/effect contract всё ещё не получен автоматически.

### Что говорят issue trackers — и чего они не говорят

MCP, A2A, UCP и AP2 имеют открытые reports о conformance fixtures, version alignment, idempotency across bindings, lifecycle mappings, signature examples, replay/context и sample logging [10][13][17][20]. Их существование показывает реальные зоны риска: спецификация, SDK, bridge и TCK могут расходиться. Но открытый issue — не adjudicated security advisory. В этом отчёте эти данные обосновывают negative tests, version pinning и isolation, а не утверждение, что соответствующий стандарт «небезопасен».

## 3. Архитектурные принципы clean-slate design

### 3.1 Самодостаточность на правильном слое

Самодостаточный AgentBridge означает, что его нативные peers понимают друг друга без чужого agent-протокола. Это не означает собственные TCP/QUIC, TLS, подписи, OAuth или платёжные сети. Интернет-архитектура помещает функции, требующие знания приложения, в endpoints, рекомендует модульность и предостерегает от циклических зависимостей; уже решённую задачу следует переиспользовать, если нет технической причины улучшать её [1].

С учётом source-backed архитектурных выводов и отдельно обозначенного последующего требования пользователя предлагается следующая, ещё не доказанная и подлежащая проверке в PRD граница:

- **AgentBridge определяет** общую role-neutral модель взаимодействия и корреляции, динамические роли участников, согласование возможностей, запросы/ответы, события/подписки, длительную и потоковую работу, ошибки и правила соответствия. Для значимых действий он также определяет семантику действия, привязку полномочий, жизненный цикл, повторы, отмену, отзыв, частичный эффект и словарь доказательств.
- **AgentBridge профилирует или переиспользует** проверенные transports, canonical encodings, signatures, token/grant mechanisms, policy engines и transparency receipts.
- **AgentBridge не владеет** бизнес-истиной конкретного домена. Банк, merchant, hotel system или независимый attestor определяет допустимый proof source; подпись подтверждает автора утверждения, а не истинность мира.

Организационные категории не закрепляются в wire model. Один participant может в конкретном обмене быть principal, initiator/requester, delegate, responder/provider, executor, approver, resource owner, policy authority или attestor, совмещать несколько ролей и менять их между взаимодействиями. Обязательные семейства топологий для проверки: agent↔service, agent↔agent, service↔service/B2B, reverse asynchronous flow, multi-party/delegation chain и participant↔replaceable discovery/trust/policy/audit infrastructure. Human UI остаётся вне core, но consent, approval, policy и evidence могут передаваться протоколом.

### 3.2 Обязательные инварианты будущего core

Это требования к последующему design, а не готовая спецификация:

1. Ни один внешний эффект не разрешён до явного применимого authorization/approval state.
2. Одобрение связывает action type, canonical parameters, actor chain, resource/audience, constraints, schema/version, expiry и operation ID; существенное изменение требует нового одобрения.
3. Делегирование сохраняет или уменьшает полномочия; principal, actor, client, delegate, executor, issuer и service не смешиваются.
4. Неизвестные critical fields, obligations, action types, versions, algorithms или bridge mappings приводят к отказу.
5. Нет скрытого downgrade к bearer, unsigned, nonce-free, weaker-version или lossy behavior.
6. Операция с внешним эффектом имеет устойчивый идентификатор, защищённый от коллизий: повтор того же запроса возвращает сохранённый результат, а запрос с тем же идентификатором и другим телом отклоняется.
7. Неоднозначный сетевой исход порождает `unknown/pending`, а не предполагаемый успех и не автоматический unsafe retry.
8. Cancellation, no-more-work, partial effect, compensation и compensation failure — разные состояния; revocation ограничивает будущую authority и не стирает прошлое.
9. Terminal event неизменяем; compensation/dispute — новые связанные события.
10. Receipt сообщает, кто и что утверждает; verifier policy определяет допустимого issuer/attestor, proof type, freshness и trust root.
11. Bridge не может повысить assurance и обязан отклонить невыразимый смысл.
12. Конечный автомат проверяется в условиях повторного воспроизведения, изменения порядка, дублирования, сбоя и перезапуска, сетевого разделения, устаревшей политики, конкурентной отмены и компрометации посредника.

RAR, DPoP, HTTP Message Signatures и JCS полезны лишь внутри точного AgentBridge profile: RAR не задаёт сравнение произвольных action objects; DPoP не связывает payload; HTTP signatures требуют явного набора covered components; JCS детерминирует JSON bytes, но не семантику, Unicode equivalence или произвольную числовую точность [21][23][24][25]. Формальный анализ OAuth показывает, что комбинация известных primitives всё равно может содержать логические атаки без явных предположений и модели [31].

### 3.3 Эволюция и conformance

Неизвестные расширения часто приводят к окостенению протокола; механизмы расширения нужно проверять с первого релиза, разделяя критические и игнорируемые элементы [3]. Одновременно liberal acceptance закрепляет несовместимость и bug-for-bug behavior [4]. Поэтому AgentBridge должен сочетать:

- строгий parsing и fail-closed semantics для security-critical данных;
- однозначную обработку unknown optional extensions;
- greasing/extension exercises в conformance suite;
- независимые реализации, созданные разными организациями и не имеющие общей кодовой основы, а не два wrapper над общей библиотекой [6];
- negative conformance на malformed/canonicalization/downgrade/replay/state-race случаи;
- открытый change process и royalty-free IP/patent policy до заявления о нейтральном стандарте [7][8].

## 4. Кандидатные архитектуры

### A. Native suite без официальных bridges

Нативные messages, state machine, security profile, discovery interface, transports и SDK; официальных mappings к MCP/A2A/UCP нет.

**Плюсы:** самый короткий обязательный путь, единая семантика, минимальная собственная version matrix. **Минусы:** почти нулевая инкрементальная миграция; экосистема создаст неофициальные adapters без security rules; проекту придётся одновременно доказать core, tooling и adoption. Это не делает протокол безопаснее автоматически — единый design может содержать единую логическую ошибку [31].

### B. Permanent profile/composition

AgentBridge фиксирует версии и связывает вызовы/идентификаторы/ошибки MCP, A2A, OpenAPI/Arazzo, UCP и authorization profiles.

**Плюсы:** максимальное использование существующих SDK, discovery и adoption channels. **Минусы:** ownership end-to-end semantics распадается; upstream lifecycle/version changes становятся частью AgentBridge correctness; exact action и effect могут менять смысл при mapping. Если нативные peers обязаны запускать эти протоколы, вариант нарушает hard gate независимости. Он остаётся разумным domain profile для commerce, но не основанием общего core.

### C. Native semantic core + optional fail-closed bridges

Рекомендуемый кандидат состоит из семи логических слоёв:

1. **Role-neutral native envelope** — идентичность и корреляция взаимодействия, динамический role context, участники и адресация без привязки к организационному типу.
2. **Negotiation и interaction modes** — protocol/profile versions, required capabilities/extensions, limits, downgrade rules, request/response, events/subscriptions, streaming и long-running work.
3. **Action/authority kernel** — operation identity, canonical action reference, actor/delegation context, constraints, authorization/approval references для взаимодействий со значимым эффектом.
4. **Normative lifecycle** — accepted/executing/unknown/succeeded/failed/partial/compensated как различимые классы outcome; точный набор состояний и его применимость к разным interaction modes ещё предстоит определить формальной моделью.
5. **Evidence contract** — append-only event/receipt chain с явным issuer, claim, subject/action binding и verifier requirements.
6. **Native bindings** — минимум один обязательный interoperable transport/encoding profile; выбор HTTP/2, HTTP/3, JSON/JCS, deterministic CBOR или иного набора остаётся benchmark decision.
7. **Domain profiles и edge bridges** — UCP/Agentic Commerce/AP2 для торговли и платежей; MCP/A2A/OpenAPI/GNAP/AuthZEN для соответствующих границ. Bridge запускается только после точного version/capability negotiation и не входит в native path.

Это полноценный protocol suite на agent/application layer, а не wrapper. Его «тонкость» означает маленький набор универсальных инвариантов, а не зависимость от чужого runtime.

### D. Upstream/stop

Контрольный исход: не создавать отдельный core; публиковать test corpus и направлять необходимые changes владельцам соответствующих стандартов.

Это самый дешёвый путь и лучший при отсутствии отдельного ownership gap. Однако сегодня он оставляет общий action-to-effect seam без одного владельца. Вариант должен автоматически победить, если независимые прототипы не обнаружат ни одного необходимого нового mandatory-to-understand объекта либо если этот объект естественно и согласованно принимается одним upstream.

## 5. Threat model и failure trees

### 5.1 Корневой риск: выполнено не то, что было разрешено

```text
wrong / unauthorized / unverifiable effect
├─ action identity changed
│  ├─ schema/version drift
│  ├─ JSON numeric/Unicode/duplicate-member ambiguity
│  └─ signature omitted a meaningful component
├─ authority confused or expanded
│  ├─ principal/actor/audience mix-up
│  ├─ delegation caveat dropped
│  └─ bearer/downgrade fallback
├─ execution outcome ambiguous
│  ├─ request replayed after lost response
│  ├─ operation ID reused with different body
│  ├─ cancel raced with external effect
│  └─ partial effect reported as failure or success
├─ evidence overclaimed
│  ├─ signed assertion treated as external truth
│  ├─ untrusted/stale attestor
│  └─ compensation assumed to restore prior state
└─ bridge weakened meaning
   ├─ upstream field/state has no lossless mapping
   ├─ version/capability silently downgraded
   └─ upstream TCK passed while cross-protocol invariant failed
```

Технические контрмеры следуют прямо из дерева: canonical action identity; full covered-components profile; issuer/audience/actor separation; durable deduplication и outcome retrieval; `unknown` state после ambiguous failure; explicit partial/compensation events; verifier policy; fail-closed mapping; negative cross-protocol tests [23][24][27][28][30][32]. Macaroon-style caveats подтверждают реализуемость монотонного attenuation, но их смысл и revoke behavior всё равно прикладные [29].

### 5.2 Корневой риск: отказ или перегрузка

```text
unavailable / unstable action path
├─ mandatory intermediary or registry unavailable
├─ handshake / negotiation adds per-call round trips
├─ UDP blocked or path changed without TCP fallback
├─ receiver waits for a complete message beyond flow-control credit
├─ slow consumer exhausts unbounded buffers
├─ compressed hostile payload exhausts memory/CPU
└─ retry storm amplifies an unknown outcome
```

Нативное ядро с необязательными пограничными bridges изолирует сбой внешней системы от нативного пути. Но HTTP/3 не является автоматическим решением: он убирает TCP cross-stream head-of-line blocking, сохраняя connection-wide congestion и QPACK dependencies; UDP может быть заблокирован, поэтому нужен TCP fallback [34]. Управление потоком QUIC может привести к взаимной блокировке на уровне framing приложения, если данные не обрабатываются по мере поступления или буферы не ограничены [35].

## 6. Производительность, transport, encoding и языки

### Что уже можно зафиксировать

- Side-effecting operations не используют QUIC 0-RTT: early data replayable; исключение возможно только для явно replay-safe controls [33].
- Backpressure, message limits, incremental parsing, cancellation while blocked и resource ceilings входят в protocol contract.
- AgentBridge должен сделать compression явно согласуемым, пороговым и ограниченным по ресурсам. RFC-backed требования: не смешивать несовместимые compression contexts и ограничивать окно zstd; предел распаковки и pre-allocation checks остаются предлагаемыми AgentBridge controls [34][40].
- Signable representation обязана быть canonical. Deterministic CBOR возможен только как явный profile [36]; raw Protobuf bytes не являются canonical across builds/schemas/libraries [37].
- Данные согласования и профиля кэшируются или предоставляются заранее; постоянный дополнительный control round trip при каждом вызове допустим только при наличии результатов измерений.

### Что не доказано

Два независимых исследования encoding за 2026 год показывают преимущества binary formats в своих нагрузках, но называют разных победителей; перенос на agent RPC не доказан [38][39]. Поэтому нельзя сейчас объявлять CBOR/Protobuf/MessagePack «идеальным» форматом.

Rust и Go оба остаются кандидатами reference core. У Rust async behavior зависит от runtime и несёт compatibility/maintenance tradeoffs [41]. У Go GC создаёт workload-dependent CPU, memory и latency tradeoffs [42]. Python и TypeScript остаются важными independent SDK/interop implementations, а не эталоном производительности. Методология gRPC показывает необходимость идентичных cross-language workers и saturated/unconstrained измерений, но не доказывает текущего победителя [43].

### Обязательная benchmark matrix до выбора

| Измерение | Cases |
| --- | --- |
| Transport | H2+TLS/TCP; H3+QUIC; cold/warm; resumed без early data; UDP blocked/fallback |
| Network | RTT 0.2/5/30/100/250 ms; loss 0/0.1/1/3/5%; reorder; constrained MTU; migration/NAT rebinding |
| RPC | unary, request/response/bidirectional streams, fan-out, long idle, mid-message cancel |
| Payload | 128 B–4 MiB; flat/nested; extreme keys; binary; unknown fields; hostile depth/size |
| Encoding | minified JSON, JCS JSON, deterministic CBOR, MessagePack, schema-driven binary |
| Crypto | parse/canonicalize/hash/sign/verify раздельно; valid/invalid; Unicode/numeric/unknown-field edges |
| Backpressure | slow consumer, bounded buffers, message over credit, interdependent streams, cancel while blocked |
| Recovery | failure before send, after effect, after headers, mid-stream; dedup hit/miss; retry amplification |
| Implementations | Rust с pinned runtime, Go, Node/TypeScript, Python; одинаковые fixtures/crypto/settings |
| Metrics | p50/p95/p99/p99.9, QPS/core, CPU, RSS, allocations, wire bytes, handshake RTT, recovery time |
| Correctness | golden-vector equality, malformed rejection, state/error equivalence, no replayed effect |
| Reproducibility | pinned toolchains, hardware/kernel/ciphers/congestion, warm-up, sample count, raw data |

Минимальные gates: 100% golden-vector equivalence; отсутствие bounded-flow deadlock; memory cap под hostile input; отсутствие side effect через 0-RTT replay; рабочий TCP fallback; optimization принимается только при заранее заданном материальном выигрыше без регрессии correctness, p99, RSS или compatibility.

## 7. Adoption, governance и растущее окно стандартизации

История интернет-протоколов не поддерживает тезис «идеальная архитектура сама станет стандартом». Положительная ценность для раннего adopter, incremental deployment, backward compatibility, open code/spec и открытое сопровождение — отдельные условия успеха [5]. Поэтому вариант C сочетает независимость нативного пути с безопасным migration path.

Поле быстро заполняется. Individual drafts по agent protocol use cases, Agent Interoperability Protocol Framework и agentic overlay уже описывают gaps, layered suites и local adaptation; это доказательство активности, но не IETF consensus [44][45][46]. После IETF 126 WG-forming BoF инициатива agentproto перешла в статус Proposed WG с initial chartering/internal Steering Group–IAB review; WG ещё не утверждена. BoF не принял первоначальный scope, а willingness to implement был слабее willingness to review/write [47][48]. Proposed DAWN также находится на этапе initial chartering и может занять generic discovery [49].

Следствия:

- уникальная роль AgentBridge должна быть доказана как role-neutral native interoperability waist для основных топологий agentic Internet, включая взаимодействие с заменяемой discovery/trust/policy/audit-инфраструктурой, со строгим authority/effect/evidence contract для значимых действий; это может стать протокольной основой широкой инфраструктуры, но не означает владение всеми транспортами, identity systems, policy engines, registries, ledgers или domain systems;
- global discovery mechanism должен быть replaceable; AgentBridge задаёт interface/invariants, но не строит обязательный proprietary registry;
- до заявления «industry standard» нужны минимум две независимо разработанные client implementations и две service implementations, публичные results, neutral change control и royalty-free IP rules [6][7][8];
- проект должен взаимодействовать с IETF/OpenID/LF/OAI/UCP work, но не ждать их завершения и не выдавать собственную раннюю спецификацию за признанный стандарт.

## 8. Взвешенное решение Gate 0

Шкала 0–5; итог нормирован к 100. Баллы — **экспертная эвристика по доказательствам**, а не измеренная производительность или вероятность рыночного успеха. Вариант B отклонён по жёсткому критерию независимо от балла. Вариант D — условный сценарий остановки: он активируется при falsification native-core hypothesis.

| Критерий | Вес | A: native без bridges | B: permanent composition | C: native + optional bridges | D: upstream/stop |
| --- | ---: | ---: | ---: | ---: | ---: |
| End-to-end security/semantic correctness | 25% | 4.2 | 3.2 | **4.4** | 3.6 |
| Failure isolation/operational simplicity | 15% | **4.4** | 2.8 | 4.1 | 4.0 |
| Performance/scalability potential | 15% | **4.4** | 3.3 | 4.2 | 3.8 |
| Independent implementability/conformance | 15% | 3.2 | 3.8 | **4.0** | 4.2 |
| Interoperability/migration | 10% | 1.5 | **4.6** | 4.4 | **4.6** |
| Extensibility/evolvability | 10% | 3.6 | 3.0 | **4.2** | 3.5 |
| Governance/adoption credibility | 7% | 2.0 | 4.0 | 3.4 | **4.3** |
| Reversibility | 3% | 2.5 | 4.2 | 4.0 | **4.7** |
| **Weighted total** | **100%** | **71.5** | **69.0 — rejected** | **83.3 — conditional winner** | **79.0 — conditional stop** |

### Интерпретация

- **C выигрывает условно**, потому что сохраняет нативную end-to-end ownership и почти всю миграционную ценность bridges, не включая их в доверенную базу нативного пути.
- **D — реальный runner-up**, а не декоративный вариант. Он дешевле и опирается на зрелые ecosystems. Он выигрывает, если эксперимент не обнаружит отдельного cross-domain core или если upstream примет его целиком.
- **A уступает C** не из-за технической невозможности, а из-за adoption и неизбежного появления неуправляемых adapters.
- **B нарушает hard gate**: обязательная композиция не позволяет двум AgentBridge implementations работать без внешних agent protocols и распределяет correctness по их version matrices.

### Cost и lock-in

| Путь | Первичная стоимость | Долгосрочный lock-in |
| --- | --- | --- |
| A | Очень высокая: весь ecosystem bootstrap | К собственному stack и неофициальным adapters |
| B | Низкая/средняя | Высокий semantic/version lock-in ко многим upstreams |
| C | Средняя/высокая: native kernel + conformance + bridges | Ограничен чёткими adapter seams; native path остаётся независимым |
| D | Низкая | К решениям и темпу upstream; собственного protocol asset нет |

Самый дешёвый reversibility hedge для C: определить bridge API и transport bindings как replaceable modules, а до стабилизации держать native kernel минимальным и не публиковать 1.0. Любое требование, не подтверждённое двумя независимыми реализациями, остаётся experimental.

## 9. Red team: сильнейшие возражения и falsification

Fresh-context red team **снизил уверенность с высокой до средней**. Он не опроверг возможность native core, но показал, что permanent composition ещё не побеждён экспериментально и что следующий шаг не должен быть написанием production specification.

### Возражение: существующая композиция уже достаточна

GNAP + AuthZEN + A2A + OpenAPI и UCP/AP2 в commerce дают почти все строительные блоки [11][14][19][26][27][50]. UCP уже показывает semantic layer с thin REST/MCP/A2A bindings; AP2 связывает mandates и receipts в высокорисковом домене. Это серьёзное возражение: AgentBridge может оказаться новым названием для safety profile.

Ответ не должен быть риторическим — сначала нужно построить сильнейший composition challenger. Если две независимые реализации безопасно получают одинаковый action/effect contract без нового mandatory AgentBridge object/state/error, отдельный core не нужен.

### Возражение: «тонкая талия» пока может быть пустой или слишком толстой

Слишком тонкий core повторит envelope/task metadata и не обеспечит безопасность; слишком толстый попытается универсализировать domain effects, compensation и evidence без знания приложения. Классический end-to-end analysis показывает, что correctness и duplicate suppression всё равно требуют участия приложения на концах [52]. Поэтому ценность core должна быть доказана конкретными cross-domain invariants и non-goals, а не широтой обещания.

### Возражение: новый protocol ухудшит фрагментацию

IETF уже обсуждает agent communication, а UCP активно расширяется за пределы shopping [44][45][47][48][50]. Отдельный бренд без независимых implementers и upstream engagement снизит шансы на adoption. Поэтому AgentBridge до выполнения evidence gates — research protocol и test corpus, не «стандарт индустрии» как свершившийся факт.

### Возражение: optional bridges станут фактически обязательными

Adoption может закрепить самый популярный bridge и протащить его ограничения обратно в core. Более того, guidance по security labels предупреждает, что gateways, вынужденные интерпретировать и преобразовывать security meaning, создают consistency/failure concerns [53]. Контрмера: native conformance никогда не требует bridge; native demos и interop events работают без него; bridge-specific extensions не входят в core namespace без общего нативного use case.

### Обязательные falsification tests

1. **Composition challenge:** выразить все proposed invariants через strongest existing profiles. Native object допускается только после документированного unrepresentable или ambiguous case.
2. **Thin-waist test:** небольшой common vocabulary должен иметь одинаковое нормативное значение как минимум в commerce, booking и non-commerce operational action. Оpaque domain payload не подтверждает ценность core.
3. **Independent differential test:** две команды без общей protocol library реализуют native и composed paths; сравниваются authorization/effect outcomes при replay, mutation, partition и cancel races.
4. **Bridge loss test:** каждая mapping доказывает preserved meaning/assurance или отказывает закрыто.
5. **Adoption test:** измеряются endpoint changes, integration effort, round trips и operational dependencies, а не только code size SDK.

Перейти к D (upstream/stop), если выполняется хотя бы одно из условий:

- независимые experiments не находят необходимой семантики сверх существующих profiles;
- один upstream принимает весь предполагаемый core с теми же invariants и interoperable tests;
- не удаётся получить независимо разработанные реализации, способные менять protocol roles и подтверждать native interoperability как минимум в agent↔service, agent↔agent и service↔service/B2B сценариях, а также в одном reverse или multi-party flow и одном взаимодействии с заменяемой discovery/trust/policy/audit-инфраструктурой;
- native C не выигрывает у B по заранее установленным correctness/failure tests или добавляет материальный latency/operational cost без компенсирующей гарантии;
- domain profiles требуют взаимоисключающих значений core, то есть domain-neutral waist не существует.

Вернуться от C к A можно только если bridges системно заставляют ослаблять native semantics даже при полной изоляции, а adoption evidence показывает готовность к чистому переходу. Сегодня таких доказательств нет.

## 10. Следующая разрешённая работа

Gate 0 разрешает **не спецификацию production-протокола**, а validation-first PRD как следующий BMAD-артефакт. Он должен зафиксировать архитектурные требования и доказательные эксперименты, включая:

1. Минимальный ownership statement и role/topology matrix AgentBridge core: что он стандартизирует и что сознательно оставляет transports, identity providers, policy engines, domains и ledgers.
2. Минимальная interaction model для request/response, events/subscriptions, streaming, reverse asynchronous flow и multi-party/delegated chains без фиксации организационных ролей.
3. Формальная модель одного consequential-action lifecycle с replay, reorder, duplicate, partition, crash/restart и cancel race.
4. Threat model и security goals, включая confused deputy, downgrade, canonicalization, stale authority и false evidence во всех обязательных топологиях.
5. Independently implementable native prototype contracts без общей protocol library, проверенные как минимум в agent↔service, agent↔agent и service↔service/B2B сценариях, а также в одном reverse или multi-party flow и одном взаимодействии с заменяемой discovery/trust/policy/audit-инфраструктурой.
6. Один non-commerce profile и один commerce bridge для проверки горизонтальности и mapping loss.
7. Conformance corpus: golden vectors, negative tests, property tests, fuzzing, differential tests и implementation genealogy.
8. Benchmark protocol и acceptance thresholds из раздела 6.
9. Stop/go gates для перехода к полноценной specification и Architecture.

Не разрешено фиксировать Rust, HTTP/3, CBOR, JSON-only, microservices, конкретную cloud stack, payment gateway или три языка SDK до соответствующих измерений и требований. Не разрешено проектировать universal domain ontology.

## 11. Открытые вопросы

1. Каков минимальный role-neutral interaction core, общий для agent↔service, agent↔agent, service↔service/B2B и взаимодействия с заменяемой инфраструктурой, но не превращающийся в пустой envelope или универсальную доменную онтологию?
2. Как формально различить «executor утверждает успех» и «внешний эффект подтверждён допустимым attestor»?
3. Какая минимальная state machine полна, но не обещает невозможное exactly-once execution?
4. Какие identity/authority credentials обязательны для базовой interop, а какие остаются bindings?
5. Каким должен быть native discovery bootstrap, если DAWN или другая инициатива возьмёт global discovery?
6. Где проходит граница между core error classes и domain outcome codes?
7. Как доказать lossless bridge mapping и как machine-readably объявлять loss/assurance downgrade?
8. Может ли один обязательный encoding/profile обеспечить browser, server и constrained environments, или нужны два равноправных bindings?
9. Rust или Go должен быть первым performance reference — и нужен ли отдельный Rust core вообще после benchmark?
10. Какая governance transition запускается после первых независимых implementations и кто владеет trademark/namespace до неё?
11. Какие ordering, correlation, delivery и termination semantics действительно общие для reverse asynchronous, streaming, one-to-many и multi-party flows?

## 12. Карта устаревания

Динамические утверждения отслеживаются отдельно от стабильных RFC. Самые быстрые изменения ожидаются в версиях MCP/A2A/UCP/ACP/AP2 и в IETF agent work; их нужно перепроверить **не позднее 12 октября 2026 года**. Issue reports также требуют повторной проверки статуса, поскольку открытый report может быть исправлен или отклонён.

| Группа | Класс | Проверено | Перепроверить |
| --- | --- | --- | --- |
| MCP, A2A, OpenAPI/Arazzo, UCP, оба ACP, AP2 | version/compatibility | 2026-09-12 | **2026-10-12** |
| Текущие conformance/security issue reports | protocol-failure | 2026-09-12 | **2026-10-12** |
| agentproto, AIPF, overlay, DAWN | AI-adjacent landscape | 2026-09-12 | 2026-12-12 |
| Governance/ecosystem status | ecosystem | 2026-09-12 | 2027-03-12 |
| Encoding/runtime benchmark leads | benchmark evidence | 2026-09-12 | 2027-09-12 или раньше при выборе реализации |

Стабильный RFC не становится неверным от возраста; пересматривать нужно его роль относительно новых profiles и implementations. Любое начало specification после 12 октября требует Refresh текущей version/compatibility части.

## 13. Источники

Дата доступа для всех источников — **2026-09-12**.

| № | Поддерживаемый вывод и ссылка | Издатель | Публикация/статус | Уверенность |
| ---: | --- | --- | --- | --- |
| [1] | [RFC 1958](https://www.rfc-editor.org/rfc/rfc1958.html) — end-to-end, modularity, reuse boundary | IAB / RFC Editor | 1996-06, Informational | высокая |
| [2] | [RFC 3439](https://www.rfc-editor.org/rfc/rfc3439.html) — service-path complexity | IETF / RFC Editor | 2002-12, Informational | высокая; AgentBridge inference средняя |
| [3] | [RFC 9170](https://www.rfc-editor.org/rfc/rfc9170.html) — extension viability и ossification | IAB / RFC Editor | 2022-01, Informational | высокая |
| [4] | [RFC 9413](https://www.rfc-editor.org/rfc/rfc9413.html) — опасность permissive ambiguity | IETF / RFC Editor | 2023-06, Informational | высокая |
| [5] | [RFC 5218](https://www.rfc-editor.org/rfc/rfc5218.html) — факторы принятия протоколов | IAB / RFC Editor | 2008-07, Informational | высокая |
| [6] | [RFC 5657](https://www.rfc-editor.org/rfc/rfc5657.html) — независимость interop implementations | IETF / RFC Editor | 2009-09, BCP | высокая |
| [7] | [Guide to the IETF standards process](https://www.ietf.org/process/process/) — open process, consensus, running code | IETF | current-as-served | высокая |
| [8] | [W3C Patent Policy](https://www.w3.org/policies/patent-policy/) — royalty-free implementation objective | W3C | 2025-05-15 | высокая |
| [9] | [MCP 2026-07-28 release](https://blog.modelcontextprotocol.io/posts/2026-07-28/) — stateless requests, Tasks/extensions boundary | MCP project | 2026-07-28 | высокая |
| [10] | [MCP conformance issues](https://github.com/modelcontextprotocol/conformance/issues) — current reported fixture/coverage/concurrency defects | MCP project | living tracker | средняя; reports unadjudicated |
| [11] | [A2A specification](https://github.com/a2aproject/A2A/blob/main/docs/specification.md) — tasks, idempotency/cancellation/identity boundaries | A2A / Linux Foundation | current main | высокая |
| [12] | [A2A releases](https://github.com/a2aproject/A2A/releases) — v1.0.x churn/pinning | A2A / Linux Foundation | v1.0.1, 2026-05-28 | высокая |
| [13] | [A2A TCK issues](https://github.com/a2aproject/a2a-tck/issues) — reported conformance defects | A2A / Linux Foundation | living tracker | средняя; reports unadjudicated |
| [14] | [OpenAPI 3.2.0](https://spec.openapis.org/oas/v3.2.0.html) — API-description scope | OpenAPI Initiative | 2025-09-19 | высокая |
| [15] | [Arazzo 1.1.0](https://spec.openapis.org/arazzo/latest.html) — workflow/error/security boundaries | OpenAPI Initiative | 2026-05-17 | высокая |
| [17] | [UCP issues](https://github.com/Universal-Commerce-Protocol/ucp/issues) — reported cross-binding/version/signature defects | UCP project | living tracker | средняя; reports unadjudicated |
| [18] | [BeeAI Agent Communication Protocol](https://github.com/i-am-bee/acp) — merged into A2A | BeeAI / LF AI & Data | migration notice current | высокая |
| [19] | [AP2 v0.2 specification](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md) — mandates/receipts и commerce boundary | Google Agentic Commerce | v0.2, current main | высокая |
| [20] | [AP2 issues](https://github.com/google-agentic-commerce/AP2/issues) — reported replay/context/evidence/sample risks | Google Agentic Commerce | living tracker | средняя; reports unadjudicated |
| [21] | [RFC 9396: RAR](https://www.rfc-editor.org/rfc/rfc9396.html) — typed authorization, profile-defined semantics | IETF / RFC Editor | 2023-05, Proposed Standard | высокая |
| [22] | [RFC 8693: Token Exchange](https://www.rfc-editor.org/rfc/rfc8693.html) — subject/actor vocabulary и boundaries | IETF / RFC Editor | 2020-01, Proposed Standard | высокая |
| [23] | [RFC 9449: DPoP](https://www.rfc-editor.org/rfc/rfc9449.html) — sender binding, replay/downgrade limits | IETF / RFC Editor | 2023-09, Proposed Standard | высокая |
| [24] | [RFC 9421: HTTP Message Signatures](https://www.rfc-editor.org/rfc/rfc9421.html) — application profile obligations | IETF / RFC Editor | 2024-02, Proposed Standard | высокая |
| [25] | [RFC 8785: JCS](https://www.rfc-editor.org/rfc/rfc8785.html) — deterministic JSON and constraints | RFC Editor | 2020-06, Informational | высокая |
| [26] | [AuthZEN Authorization API 1.0](https://openid.net/specs/authorization-api-1_0.html) — PDP/PEP decision API | OpenID Foundation | Final, 2026-01-12 | высокая |
| [27] | [RFC 9635: GNAP](https://www.rfc-editor.org/rfc/rfc9635.html) — grants, consent, continuation и downstream boundary | IETF / RFC Editor | 2024-10, Proposed Standard | высокая |
| [28] | [RFC 9110: HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110.html) — idempotency/retry boundary | IETF / RFC Editor | 2022-06, Internet Standard | высокая |
| [29] | [Macaroons](https://research.google/pubs/macaroons-cookies-with-contextual-caveats-for-decentralized-authorization-in-the-cloud/) — decentralized attenuation | Google Research / NDSS | 2014 | высокая для primitive; средняя для переноса |
| [30] | [RFC 9943: SCITT](https://datatracker.ietf.org/doc/rfc9943/) — transparency receipts, not external truth | IETF / RFC Editor | 2026-06, Proposed Standard | высокая |
| [31] | [Formal Security Analysis of OAuth 2.0](https://publ.sec.uni-stuttgart.de/fettkuestersschmitz-ccs-2016.pdf) — protocol-logic attacks and verified fixes | Fett, Küsters, Schmitz / ACM CCS | 2016-10 | высокая |
| [32] | [FAPI 2.0 conformance](https://openid.net/fapi2-0-final-conformance-tests-available/) — formal profile plus executable negative tests | OpenID Foundation | 2025-07-09 | высокая |
| [33] | [RFC 9001: QUIC TLS](https://www.rfc-editor.org/rfc/rfc9001.html) — 0-RTT replay | IETF / RFC Editor | 2021-05, Standards Track | высокая |
| [34] | [RFC 9114: HTTP/3](https://www.rfc-editor.org/rfc/rfc9114.html) — multiplexing, QPACK и fallback | IETF / RFC Editor | 2022-06, Standards Track | высокая |
| [35] | [RFC 9308: QUIC Applicability](https://www.rfc-editor.org/rfc/rfc9308.html) — flow-control deadlocks | IETF / RFC Editor | 2022-09, Informational | высокая |
| [36] | [RFC 8949: CBOR](https://www.rfc-editor.org/rfc/rfc8949.html) — deterministic encoding profile | IETF / RFC Editor | 2020-12, Standards Track | высокая |
| [37] | [Proto Serialization Is Not Canonical](https://protobuf.dev/programming-guides/serialization-not-canonical/) — deterministic is not canonical | Protocol Buffers / Google | current official docs | высокая |
| [38] | [A Leaner and Faster Web](https://arxiv.org/abs/2512.12067) — CBOR result on one workload | IEEE TNSM / authors | 2026-08 | средняя; transfer unverified |
| [39] | [Optimizing data transmission efficiency in IoT systems](https://doi.org/10.1063/5.0322487) — different binary-format strengths | AIP Conference Proceedings | 2026-01 | низкая-средняя для agent RPC |
| [40] | [RFC 9659: zstd content coding](https://www.rfc-editor.org/rfc/rfc9659.html) — bounded decoder window | IETF / RFC Editor | 2024-09, Informational | высокая |
| [41] | [State of Async Rust](https://rust-lang.github.io/async-book/01_getting_started/03_state_of_async_rust.html) — runtime/compatibility tradeoffs | Rust Project | current official docs | высокая для tradeoffs |
| [42] | [Go GC Guide](https://go.dev/doc/gc-guide) — GC CPU/memory/latency tradeoffs | Go Project | current official docs | высокая для tradeoffs |
| [43] | [gRPC Benchmarking](https://grpc.io/docs/guides/benchmarking/) — cross-language benchmark methodology | gRPC / CNCF | living documentation | средняя |
| [44] | [agentproto use cases draft](https://datatracker.ietf.org/doc/draft-rosenberg-agentproto-usecases/) — recognized partial coverage/gaps | IETF Datatracker / individual authors | draft-00, 2026-07-04 | средняя; no IETF consensus |
| [45] | [Agent Interoperability Protocol Framework draft](https://datatracker.ietf.org/doc/draft-zahed-agent-comm-framework/) — overlapping broad suite | IETF Datatracker / individual authors | draft-01, 2026-07-19 | средняя; no IETF consensus |
| [46] | [Agentic Overlay Network Architecture draft](https://datatracker.ietf.org/doc/draft-xu-agentic-overlay-network-architecture/) — local adaptation/control-path separation | IETF Datatracker / individual authors | draft-00, 2026-05-08 | средняя; no IETF consensus |
| [47] | [agentproto group status](https://datatracker.ietf.org/wg/agentproto/about/) — Proposed WG, initial chartering, не утверждённая WG | IETF Datatracker | current 2026-09-12 | высокая |
| [48] | [IETF 126 agentproto minutes](https://datatracker.ietf.org/meeting/126/materials/minutes-126-agentproto-202607230700-01) — support, rejected scope, implementation signal | IETF | 2026-07-23 | высокая |
| [49] | [Proposed DAWN charter](https://datatracker.ietf.org/doc/charter-ietf-dawn/) — possible generic discovery overlap | IETF Datatracker | revision 00-07, updated 2026-09-11; initial chartering | высокая для status |
| [50] | [UCP announcements](https://ucp.dev/documentation/announcements/) и [v2026-08-25 specification](https://ucp.dev/2026-08-25/specification/overview/) — current version, constraints, actions, signatures, negotiation | UCP Authors | released 2026-08-25 | высокая |
| [51] | [Agentic Commerce Protocol](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) — отдельный beta commerce ACP | OpenAI / Stripe | beta; stable snapshot 2026-04-17 | высокая |
| [52] | [End-to-End Arguments in System Design](https://web.mit.edu/Saltzer/www/publications/endtoend/endtoendA4.pdf) — application participation in end-to-end correctness | Saltzer, Reed, Clark / ACM TOCS | 1984 | высокая |
| [53] | [RFC 1457](https://www.rfc-editor.org/rfc/rfc1457.html) — security-label gateway consistency concerns | IETF / RFC Editor | 1993-05, Experimental | средняя для architectural transfer |
