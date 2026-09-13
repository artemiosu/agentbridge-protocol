---
title: "Product Brief Addendum: AgentBridge"
status: final
created: 2026-09-10
updated: 2026-09-12
visibility: private-local
---

# Product Brief Addendum: AgentBridge

Этот внутренний справочник отделяет утверждённое уточнение стратегии от деталей исходных материалов, которые не должны перегружать Product Brief. Исходные гипотезы ниже не считаются утверждёнными решениями и не разрешают реализацию.

## Утверждённая стратегия: native AgentBridge, адаптеры необязательны

- **Native AgentBridge — основная проверяемая гипотеза:** самостоятельный protocol suite с канонической общей семантикой взаимодействия и отдельным строгим контуром значимых действий — полномочия, одобрение, исполнение, evidence и errors, — собственным native interaction path и end-to-end conformance.
- **Role-neutral и direction-neutral scope подтверждён:** единый core должен поддерживать agent↔service, agent↔agent, service↔service/B2B, обратные асинхронные сообщения, многосторонние цепочки и взаимодействие участников с заменяемой инфраструктурой discovery, trust, policy и audit через явно определённые bindings — независимо от владельца участника. «Клиент», «бизнес» и «частное лицо» — организационный контекст, а не фиксированные протокольные роли.
- **MCP, A2A, OpenAPI/Arazzo, UCP, ACP, AP2 и смежные инициативы — не обязательный фундамент:** они используются как defect/control corpus, сравнительная база и возможные необязательные compatibility bridges на границах системы.
- **Нативные участники не зависят от внешнего agent-protocol runtime:** взаимодействие не должно требовать MCP/A2A/UCP, AgentBridge Cloud, обязательного broker, централизованного registry или централизованного gateway.
- **Full-stack reinvention исключён:** AgentBridge не создаёт собственные криптографические алгоритмы, глобальные identity providers, payment rails или отраслевые системы истины. Проверенные transport, identity и authorization primitives могут переиспользоваться только после аудита и через точный normative profile/binding.
- Compatibility bridge не может ослаблять нативные инварианты или молча терять полномочия, ограничения, состояния и evidence. Невозможность lossless mapping должна завершаться явной ошибкой, а не best-effort преобразованием.
- Стандартная модель должна быть самодостаточной для независимой реализации и conformance; конкретные форматы, transport bindings и языки реализации пока не выбраны.

## Разрешённый следующий шаг

Clean-slate technical Deep Recon и архитектурный Gate 0 завершены: условно выбран native AgentBridge core с optional fail-closed bridges. Следующий обязательный этап — validation-first PRD, который должен проверить role-neutral core против strongest composition challenger и сохранить автоматический profile/upstream/stop outcome. Production-спецификация, полная Architecture, SDK и код остаются закрыты до последующих gates из Product Brief.

## Исходный контекст, не утверждённый проектом

### Стратегический вариант

В приватных материалах предложена последовательность: открытая спецификация и SDK → распространение через разработчиков и сообщество → hosted cloud → транзакционные и enterprise-сервисы → институционализация стандарта. Логика «open-source adoption wedge перед коммерческими слоями» сохраняется как вариант, но календарные сроки и переходы заменяются evidence-based gates.

Контент-каналы, сообщества, партнёрства с agent frameworks и commerce-платформами, outbound, fundraising, международная экспансия, W3C/IETF, сертификация и IPO относятся к будущим документам по GTM, roadmap и governance. Они не образуют согласованный план до доказательства ядра протокола.

### Неподтверждённые числовые гипотезы

- Оценки рынка и потока агентных транзакций, включая ориентир $5 трлн к 2035 году, не имеют подтверждённой методологии.
- Сроки достижения установок, stars, участников сообщества, партнёрств, платящих клиентов и MRR — цели, а не прогнозы.
- Тарифы freemium/SaaS, квоты, комиссия 1–2%, enterprise-контракты, рекламная выручка и transaction volume не опираются на подтверждённые willingness to pay или unit economics.
- Ожидания seed-раунда, последующих оценок, миллиардной выручки, IPO и капитализации $1–2 трлн — долгосрочные сценарии, а не основания для продуктовых решений.
- В исходной арифметике комиссия 1–2% от заявленного объёма транзакций не согласована с указанной выручкой. Возможно, подразумевалась доля обрабатываемого потока, но она не определена.

### Противоречия, которые должны оставаться видимыми

- Открытая нейтральная инфраструктура и независимое управление против централизованного коммерческого контроля, «монополии», mandatory gateway, комиссии и sponsored ranking.
- Исходное сужение до прямого agent-to-service протокола против подтверждённого role-neutral охвата agent↔service, agent↔agent, service↔service/B2B, многосторонних цепочек и взаимодействия с заменяемой инфраструктурой; поисковик или агрегатор при этом остаётся отдельным продуктом, а не функцией core.
- Доменно-независимое ядро против заранее заданных операций Search/Buy/Book/Auth и одновременного охвата retail, travel, banking и других регулируемых отраслей.
- Четырёхнедельный фундамент против production-grade безопасности, независимой совместимости, нескольких SDK и содержательной стандартизации.
- Python-first против одновременного MVP на Python/JavaScript/Go и отдельного требования четырёх языков.
- Требование 20+ примеров против отдельного минимального критерия 5+ сценариев; 100% test coverage против не менее 80%.
- Лицензия кода MIT/Apache 2.0 против отсутствующей политики лицензирования и интеллектуальной собственности для нормативной спецификации.
- «SaaS исчезнет» как контент-тезис против SaaS как ранней модели монетизации.

## Architecture handoff — только после Gates 0–1

Этот раздел является условной передачей будущему Architecture-этапу и не разрешает полное проектирование до доказательства Gates 0–1.

| Тема | Зафиксированная граница | Решение, которое предстоит принять | Требуемое доказательство или артефакт |
| --- | --- | --- | --- |
| Идентичность продукта | AgentBridge — протокол и SDK, не агент, UI или вертикальное приложение; native protocol suite — основная гипотеза; внешние agent-протоколы — необязательные bridges; full-stack reinvention исключён | Где заканчивается native AgentBridge core и начинаются foundational bindings, adapters, domain profiles и коммерческие сервисы | Явная layered/thin-waist модель границ и architecture decision records (ADR) |
| Участники и роли | Core нейтрален к направлению и владельцу; client/server — роли конкретного обмена; участник может быть principal, initiator/requester, delegate, responder/provider, executor, approver, resource owner, policy authority или attestor | Минимальная ролевая модель, допустимое совмещение и смена ролей, identity/trust boundaries | Role matrix и независимые interop-сценарии без организационных допущений |
| Топологии взаимодействия | Обязательный охват: agent↔service, agent↔agent, service↔service/B2B, reverse asynchronous, multi-party delegation и participant↔replaceable discovery/trust/policy/audit infrastructure; ни одна топология не требует центрального посредника | Какие primitives действительно общие для direct, asynchronous, streaming, one-to-many, delegated и infrastructure-bound chains | Общие инварианты трактуются и проверяются одинаково, а дополнительные применяются по роли и interaction mode без частных соглашений для отдельных топологий |
| Семантический цикл | Нужны capabilities; interaction context; request/event и constraints; result/evidence/error; для делегирования и значимых действий дополнительно нужны authority, consent/approval и action lifecycle | Порядок сообщений, state model и граница между обязательным AgentBridge semantic core и extensions | Сквозные и негативные сценарии в нескольких доменах |
| Совместимость | AgentBridge владеет нативными end-to-end-инвариантами; существующие agent-протоколы не являются обязательными зависимостями | Что составляет native core, какие foundational primitives переиспользовать, какие bridges возможны без потерь и когда mapping должен fail closed | Defect/constraint matrix, native и composition prototypes, тесты semantic equivalence и downgrade resistance |
| Trust и безопасность | Полномочия принципала и проверяемый результат обязательны | Identity, authentication, authorization, consent, delegation, revocation, audit, privacy, fraud, liability и disputes | Threat model, misuse cases и security review |
| Данные и версии | Формат и транспорт не выбраны | Wire/content formats, schema, extensibility, idempotency, errors, version negotiation и backward compatibility | Test vectors, compatibility policy и conformance suite |
| SDK и репозиторий | Один полный reference SDK, затем независимая реализация; остальные SDK после стабилизации; Rust — кандидат, не решение | Первый язык, API surface, code generation, FFI/portable-core strategy и структура репозитория | Независимый от языка контракт, benchmark matrix и независимый interop test |
| Качество | Недоказуемая гарантия «без ошибок» заменена проверяемой дисциплиной | Risk-based verification, automation и release gates | Conformance, независимые реализации, CI и многоступенчатое ревью |

Предложенная в приватных материалах структура репозитория должна быть явно сопоставлена с выбранной архитектурой. Каждый элемент нужно подтвердить, изменить или отложить с обоснованием.

### Преждевременные варианты для сравнения

- JSON-only, JSON Schema, YAML, OpenAPI/Swagger, binary encodings и semantic versioning.
- HTTP/2, HTTP/3, QUIC, WebTransport или иной native transport binding.
- OAuth2, JWT, API keys, TLS 1.3+ и шифрование без разделения транспорта, идентичности, авторизации и политики.
- Rust reference core и/или одновременный Python/TypeScript/Go; FastAPI/Django/Flask/Node/Express/Next; FFI и автогенерация из ORM или БД.
- Microservices, PostgreSQL, Redis, RabbitMQ/Kafka, Prometheus/Grafana, ELK, AWS/GCP и масштаб «миллионы запросов в секунду».
- Stripe/PayPal/crypto, PCI DSS, real-time ad auction и behavioral targeting.

Ни один из этих вариантов не выбран в Product Brief.

## Governance и коммерциализация: дополнительные ограничения

- Открытой процедуры self-conformance должно быть достаточно для подтверждения совместимости; платный аудит остаётся необязательной услугой и не может работать по модели pay-to-pass.
- Нормативные правила conformance и использования официальных marks должны определяться нейтральной governance-моделью; платные проверки могут выполнять несколько независимых аудиторов.
- Совместимость не должна требовать AgentBridge Cloud, marketplace, gateway или другого централизованного посредника.
- Комиссия возможна только за добровольно выбранный сервис, который реально обрабатывает транзакцию, а не за использование протокола.
- Marketplace, transaction services и реклама требуют governance firewall. Реклама не должна влиять на нормативные ответы протокола, результаты проверки совместимости или выбор предложения пользователем; её запуск откладывается до появления независимого управления.

## Глоссарий доказательств

- **Technical evidence:** независимые реализации проходят единый публичный conformance-набор.
- **Adoption evidence:** внешние команды реализуют протокол, повторно используют его и сохраняют интеграцию без постоянной помощи авторов.
- **Demand evidence:** команда осознанно выбирает AgentBridge вместо альтернатив и расширяет использование.
- **Revenue evidence:** отдельный budget owner добровольно покупает и продлевает коммерческую надстройку. Stars, скачивания и production traffic не доказывают willingness-to-pay.

Перед разработкой надстройки требуется карточка: `buyer → paid problem → capability → value metric → prerequisite → disconfirming evidence`. Платная надстройка должна решать проблему, которую намеренно не закрывают бесплатная открытая спецификация и независимая реализация протокольного ядра.

## Downstream handoff

| Этап | Вход и обязательное решение | Не переносить как факт |
| --- | --- | --- |
| Clean-slate technical Deep Recon — завершён | Условно выбран native core + optional fail-closed bridges; зафиксированы ограничения, threat/failure analysis, benchmark plan и falsification criteria | Что Rust, новый wire protocol или clean-slate design автоматически быстрее/безопаснее; что существующие стандарты обязаны стать зависимостями |
| PRD — следующий обязательный этап | Validation Charter; role/topology matrix; выбранная архитектурная гипотеза; первые внешние implementers; reference-домены и направления взаимодействия; измеримая стоимость несовместимости; outcomes и acceptance criteria для capability discovery, events, intent, consent, delegation, approval, idempotency, errors, result verification, audit и version negotiation | Спрос, универсальность native core во всех направлениях, его окончательный состав и выбранный wire format |
| Architecture — только после Gates 0–1 | Defect/constraint matrix, threat model, benchmarks и таблица Architecture handoff выше; обоснованный выбор границ, formats, trust, versions, SDK и repo structure | Любой преждевременный стек, Rust как обязательный язык или исходную структуру репозитория как обязательную |
| Governance — после Gate 3 | RFC-процесс, IP policy, change control, conformance, marks, независимые реализации и путь к нейтральному управлению | Контроль одной компании, платный доступ к совместимости или гарантированное принятие организацией по стандартизации |
| Roadmap/GTM — после соответствующих Gates 4–5 | Evidence gates перед SDK, demos, content, partnerships, hosted services и fundraising; отдельная проверка willingness-to-pay | Исходные сроки, каналы, партнёров, цены, выручку и инвестиционные показатели как прогноз |

В validation-first PRD необходимо превратить результат Gate 0 в проверяемые требования, числовые пороги и сроки последующих gates, определить допустимый объём помощи независимому реализатору и владельца решения о продолжении или остановке работ. Отдельно следует проверить, может ли один минимальный role-neutral core одинаково сохранять общие инварианты во всех обязательных топологиях и корректно применять дополнительные инварианты по роли и interaction mode; иначе scope пересматривается до спецификации. До публичного запуска нужно проверить название, домены, пакеты и товарные знаки; при конфликте проект переименовывается.
