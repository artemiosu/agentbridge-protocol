---
title: "Техническое исследование: незакрытый пробел стандартов для AgentBridge"
type: technical
shape: select
topic: "AgentBridge standards gap"
decision: "landscape evidence retained; A2-via-B recommendation superseded; clean-slate comparison pending"
source: "native web research; primary sources only"
status: complete
decision_status: superseded
preset: standard
validation: high
claims_verified: 32
claims_unverified: 3
claims_disputed: 1
claims_overturned: 3
claims_total: 39
created: 2026-09-11
updated: 2026-09-12
---

# Техническое исследование: незакрытый пробел стандартов для AgentBridge

> **Обновление решения от 2026-09-12.** Карта стандартов, источники и выявленные пробелы этого отчёта сохраняются как доказательная база. Архитектурная рекомендация «A2 через B» более не действует: исходная постановка искала остаток между существующими инициативами и тем самым системно отдавала преимущество композиции, но не сравнивала её с полноценным нативным AgentBridge по архитектуре, безопасности, отказоустойчивости и производительности. Утверждён новый следующий этап — отдельный clean-slate technical Deep Recon. До него native AgentBridge является основной проверяемой гипотезой, а существующие agent-протоколы — объектами аудита и необязательными compatibility bridges, не обязательными зависимостями.

## Executive summary

**Исторический вывод в рамках исходной постановки:** отчёт рекомендовал A2 через B — самостоятельный AgentBridge semantic core/protocol suite, выявляемый ограниченным экспериментом совместимого профиля. Этот вывод теперь считается неполным и не управляет дальнейшим планом, поскольку исследование не включало равноценный clean-slate native design. Сохраняется только граница против монолитного переизобретения собственных криптографических алгоритмов, глобальной identity-инфраструктуры и отраслевых источников истины. Рабочее имя AgentBridge допустимо для исследования, тестовых векторов и координации. До появления независимых реализаций и признанного управления его нельзя позиционировать как новый индустриальный стандарт.

Исследование подтвердило реальный, но узкий пробел. MCP стандартизирует связь приложения с инструментами и контекстом; A2A — взаимодействие агентов и жизненный цикл задач; OpenAPI и Arazzo — описание HTTP-операций и многошаговых процессов; OAuth/GNAP/AuthZEN — выдачу прав и решения авторизации; UCP, Agentic Commerce Protocol, AP2 и x402 — отдельные части агентной коммерции и платежей. Ни один из изученных стабильных общих стандартов не обеспечивает для **произвольного — не только коммерческого — действия** сквозную связь его точного определения с делегированными полномочиями, одобрением, исполнением и подтверждённым результатом, а также общий межпротокольный тест соответствия. [1][2][6][10][11][12][13][19][22][26]

Это не означает, что нужен новый core масштаба MCP или A2A. Большая часть такого core уже существует. Более того, UCP + AP2 покрывают значительную часть паттерна в commerce/payment-контуре, включая связывание mandate и authorization/payment receipts, но не независимое доказательство произвольного внешнего эффекта; AuthZEN активно расширяется в сторону approvals, obligations и MCP-binding. [13][14][19][20][27] Следовательно, допустимая область AgentBridge ограничивается швом между стандартами: единая идентичность действия, проверяемая связь «полномочие → одобрение → исполнение → доказательство результата», точные правила отображения в MCP/A2A/OpenAPI/Arazzo/OAuth и переносимые тестовые векторы.

Red-team изменил первоначальную формулировку. Если профиль вводит обязательные новые объекты, состояния, ошибки, криптографические receipts и правила обработки, он архитектурно становится **узким семантическим протокольным слоем**, даже если документ называется profile. В терминологии RFC 6906 профиль представления не должен менять базовую семантику для обработчика без знания профиля; перенос этой границы на AgentBridge — архитектурная аналогия, а не нормативная классификация RFC. RFC 6709 относит новые сообщения и изменённое поведение к существенным расширениям. [32][33] Поэтому слово «тонкий» допустимо только пока AgentBridge выбирает и связывает уже существующие семантики. Любой действительно новый mandatory-to-understand элемент должен быть честно признан расширением и предложен в соответствующий upstream-процесс.

В исходных границах взвешенная оценка дала: **A2 через B — 78/100, C — 70/100, постоянный B-only — 69/100, A1 — 48/100**. Это экспертная decision heuristic, а не измеренная рыночная статистика. Оценка не включала отдельный вариант «нативный многоуровневый AgentBridge с собственным interaction path и необязательными adapters» и использовала критерий композиции, который заранее благоприятствовал B. Поэтому числа сохраняются только как историческая оценка прежней постановки и не могут выбирать архитектуру.

### Что разрешает и чего не разрешает этот отчёт

**Подтверждено как материал следующего исследования:** каноническая модель и ссылка/хэш действия; связь principal/delegate/policy/approval с действием; различие между принятием авторизации и фактическим исполнением; переносимые идентификаторы и ошибки; правила replay, idempotency, cancellation, revocation и partial effects; общие тестовые векторы.

**Отчёт не разрешает создавать сейчас:** спецификацию, SDK или код; собственные криптографические алгоритмы; глобальную identity-инфраструктуру; обязательный broker/cloud/registry; универсальную доменную онтологию; новый payment rail. Нужны ли AgentBridge собственные native transport/RPC/task semantics, должен решить clean-slate Deep Recon, а не прежняя предпосылка композиции.

## 1. Вопрос, границы и метод

Исследование выбирает один путь:

1. **A1 — монолитный core**, переизобретающий нижележащие инфраструктурные слои;
2. **A2 через B — самостоятельный semantic core**, выявляемый и проверяемый через совместимый профиль, bindings и supplemental conformance;
3. **B-only — постоянный профиль/glue layer** без собственного semantic core;
4. **C — остановка отдельного протокола и вклад только в upstream-инициативы**.

Критерии и веса утверждены до сбора выводов: уникальность пробела 30%, композиция 20%, безопасность/делегирование/проверяемость 20%, принятие/governance 15%, стоимость 10%, обратимость 5%.

Исследование выполнено по состоянию на **11 сентября 2026 года**. Использованы только первичные публичные материалы: официальные спецификации, RFC, репозитории, governance-документы и материалы организаций-держателей стандартов. Приватные документы AgentBridge задавали вопросы, но не использовались как доказательства. Проверка включала три независимых направления, второй раунд уточнений, три проверки ключевых утверждений и три red-team атаки на итоговую гипотезу. Формулировка «функция отсутствует» здесь означает только, что функция не найдена в просмотренной нормативной поверхности; это не доказывает её отсутствия во всех реализациях.

## 2. Карта существующих стандартов

| Инициатива | Что она уже стандартизирует | Существенная граница для AgentBridge | Зрелость на дату исследования |
| --- | --- | --- | --- |
| **MCP** | Host/client/server, tools/resources/prompts, capability negotiation, JSON-RPC, HTTP authorization; отдельно — async Tasks | Протокол рекомендует согласие пользователя, но не может обеспечить его сам; OAuth-профиль даёт доступ к ресурсу, а не переносимое одобрение точного действия; Tasks не служат доказательством внешнего эффекта | Core действующий; Tasks в просмотренной версии — draft [1][2][3] |
| **A2A** | Agent Card, skills, сообщения, Task/Artifact, streaming/polling/push, cancellation, несколько bindings, `AUTH_REQUIRED` handoff | Scope, представление, срок и отзыв авторизации, а также identity операции оставлены issuer/implementation/extension; task-level handoff стандартизирован, но переносимая credential/evidence chain делегированных полномочий — нет | v1.0, patch-релизы продолжаются [6][7] |
| **OpenAPI** | Контракт HTTP API: операции, схемы, ответы, security schemes | Описывает, но не исполняет; не связывает одобрение с эффектом | OAS 3.2 — опубликованный стандарт [10] |
| **Arazzo** | Workflow, шаги, зависимости, retry/branch, success criteria поверх OpenAPI/AsyncAPI | Не обеспечивает security enforcement или криптографическое подтверждение фактического изменения мира | Опубликована 1.1.0; редакционная разработка продолжается, поэтому конкретный артефакт надо фиксировать [11] |
| **GNAP** | Интерактивная выдача делегированного grant, continuation, key-bound access tokens, revoke | Типы действий и связь с результатом остаются прикладными | RFC 9635, Proposed Standard [12] |
| **OAuth RAR / Token Exchange / DPoP** | Структурированные authorization details; subject/actor chain; sender-constrained HTTP requests | Семантика `authorization_details` определяется профилем; actor chain информационна; DPoP базово не подписывает body/query и не является одобрением действия | Стабильные RFC [22][23][24] |
| **AuthZEN** | Унифицированный запрос `subject/resource/action/context` и решение PDP→PEP | Final API решает «можно/нельзя», но не исполняет действие и не выдаёт effect receipt; approvals/obligations/MCP-binding ещё развиваются | Authorization API 1.0 Final; смежные профили — WG drafts [26][27] |
| **UCP** | Версионированные commerce capabilities/profiles и bindings для REST/MCP/A2A; связь с AP2 | Сильный прецедент композиции, но домен — commerce, а не произвольные действия | Стабильные датированные срезы; актуальный на дату исследования — 2026-08-25 [13][14] |
| **OpenAI/Stripe Agentic Commerce Protocol** | Checkout/order, idempotency, delegated payment; стабильный датированный snapshot | Commerce-specific; проект всё ещё маркируется beta | Stable snapshot 2026-04-17 внутри beta-проекта [16][17][18] |
| **AP2** | Checkout/payment mandates, human-present и autonomous authorization, hash-binding, signed authorization/payment records | Generic authorization skeleton перспективен, но текущие типы — payment/checkout; общий receipt подтверждает решение авторизации, не любой внешний эффект | v0.2; передан на развитие в FIDO, не финальный общий стандарт [19][20][34] |
| **x402** | Payment-required, verification и settlement поверх разных transports | Не задаёт human-principal delegation или общий action lifecycle | v2, living specification [21] |

### Вывод из карты

Транспорт, discovery, описание операций, task lifecycle, grant mechanics, policy decision и платежные доказательства уже имеют сильные основы. Пробел находится не внутри одного из этих слоёв, а в их **композиции для consequential actions**. Создание ещё одного универсального способа вызвать tool/task/API почти наверняка дублировало бы MCP, A2A и OpenAPI. Одновременно полная остановка оставляет каждой паре реализаций частную договорённость о том, какое именно действие было разрешено и что считать доказанным результатом.

## 3. Что именно остаётся незакрытым

### 3.1 Сквозной цикл

| Этап | Лучшее существующее покрытие | Остаточный пробел |
| --- | --- | --- |
| Discovery / capabilities | MCP discovery и tool lists; A2A Agent Card/skills; OpenAPI descriptions | Нет общей нормативной связи `A2A skill ↔ MCP tool ↔ OpenAPI operation ↔ Arazzo workflow ↔ authorization resource` |
| Intent / constraints | Аргументы tool/API; Arazzo inputs/criteria; RAR `authorization_details` | Нет переносимой идентичности точного действия и единого digest, к которому привязываются последующие артефакты |
| Delegated authority | OAuth/GNAP; Token Exchange; AP2 mandates | Нет общего межпротокольного доказательства «этот delegate вправе выполнить именно эту версию действия в этих пределах» |
| Approval | MCP elicitation; A2A `AUTH_REQUIRED`; GNAP interaction; AP2 approval | Нет общего immutable approval resolution с expiry, consumption, amendment и re-approval rules |
| Execution | MCP Tool, A2A Task, HTTP operation, Arazzo workflow | Нет нормативного отображения одной и той же операции между носителями и общего правила idempotency/correlation |
| Result / error | MCP result/JSON-RPC error; A2A Task/Artifact; HTTP response; Arazzo criteria | Не различены единообразно: запрос принят, действие началось, действие завершилось, внешний эффект произошёл, эффект частичный/компенсирован |
| Evidence / audit | AP2 signed records; HTTP signatures; protocol histories | Нет общего verifier contract, связывающего действие, полномочие, исполнителя, результат и внешний эффект |
| Conformance | Отдельные MCP/A2A/UCP suites | Нет общего теста цепочки authorization → approval → execution → effect через разные протоколы [5][9][15] |

### 3.2 Точная формулировка пробела

**Незакрытый пробел — переносимый cross-protocol contract для consequential action**, который позволяет двум независимым реализациям согласиться:

- что является одним и тем же действием при представлении как MCP Tool, A2A Task, OpenAPI operation или Arazzo workflow;
- кто principal, кто delegate и какая policy/version действовала;
- какие параметры и ограничения были разрешены и требовалось ли новое одобрение после изменения;
- что именно означает каждый этап выполнения, ошибка, отмена, повтор и частичный эффект;
- какой receipt подтверждает только решение/приём, а какой — наблюдаемое исполнение или эффект;
- какие подписи, ключи, canonicalization и verifier rules обязательны;
- какие тесты доказывают согласованность двух независимых реализаций.

RAR уже способен переносить точные типизированные параметры авторизации, но намеренно оставляет их семантику прикладному профилю; OpenID4VP аналогично поддерживает typed transaction data, однако конкретное связывание транзакции требует принятого профиля. [22][25] Спецификация HTTP Message Signatures требует, чтобы конкретное приложение или профиль определяли подписываемые компоненты, ключи, алгоритмы, request-response binding и ошибки; JCS даёт возможную канонизацию JSON, но выбор и область применения остаются нормативным решением профиля. [30][31] Следовательно, недостающий слой — не новый криптографический алгоритм, а совместимое применение существующих примитивов.

### 3.3 Чего исследование не доказало

- Не доказано, что рынку нужен отдельный бренд AgentBridge.
- Не доказано, что один domain-neutral vocabulary сможет выразить все отрасли без профилей домена.
- Криптографический receipt сам по себе не доказывает истинность внешнего мира; он подтверждает подписанное утверждение, а доверие к утверждению зависит от issuer, evidence и verifier policy.
- Не доказано, что MCP/A2A/OpenID/IETF отклонят необходимые расширения; напротив, у них есть формальные пути инкубации. [4][8][27]
- Не найдено публичной авторитетной cross-vendor pass matrix, доказывающей сегодня end-to-end совместимость всей цепочки. Наличие TCK не равно adoption.

## 4. Проверка на некоммерческом сценарии

Чтобы не принять commerce-решение за универсальное, пробел проверен на примере сценария: **уполномоченный агент разворачивает новую версию сервиса в production**.

1. Компания публикует capability через A2A Agent Card и/или OpenAPI description.
2. Действие идентифицируется ссылкой на конкретную OpenAPI operation либо Arazzo workflow и фиксируется digest версии контракта.
3. Параметры deployment и ограничения среды выражаются типизированным action object; RAR или GNAP передаёт их в авторизационный контур.
4. Решение политики авторизации может быть получено через AuthZEN; интерактивное подтверждение — через существующий OAuth/GNAP/A2A handoff.
5. A2A ведёт внешний Task lifecycle; MCP при необходимости остаётся внутренним адаптером к инструменту развёртывания.
6. Исполняющая сторона возвращает результат и evidence, привязанные к исходному action digest, principal/delegate, task и версии policy.

Первые пять пунктов уже можно собрать из действующих стандартов. В шестом возникает разрыв: HTTP 2xx, A2A `completed` и MCP task completion не доказывают одинаковую вещь, а cancellation может быть cooperative/eventual и не гарантировать остановку side effects. [3][6] OpenAPI/Arazzo способны описать ожидаемый ответ и критерии, но не превращают их в независимую аттестацию внешнего результата. [10][11]

Этот пример подтверждает общий шов за пределами покупки. Но он также показывает, что семантика «эффект действительно произошёл» зависит от домена: deployment receipt должен ссылаться на deployment control plane или attestor; банковский перевод — на ledger/payment rail; бронирование — на reservation system. AgentBridge не должен притворяться универсальным источником истины. Он может стандартизировать **структуру доказательства, идентичность и verifier contract**, а domain profile — допустимый issuer и значение effect claims.

## 5. Где проходит граница между profile и новым core

### Настоящий профиль

Профиль остаётся лёгким, если он:

- фиксирует версии и совместимые наборы уже существующих стандартов;
- выбирает существующие поля/URI и уточняет их использование;
- задаёт отображения между native objects без нового обязательного processing model;
- публикует примеры и supplemental tests;
- допускает безопасную обработку участником, который профиль не понимает.

### Узкий семантический протокол

Работа становится отдельным протокольным слоем, если она требует новых mandatory-to-understand Action, ApprovalResolution или ExecutionReceipt, добавляет состояния/ошибки, определяет новое поведение retry/cancel/revoke либо требует собственной negotiation/version registry. По архитектурной аналогии с границей профиля представления в RFC 6906 и по критериям расширений RFC 6709 такую работу следует считать существенным расширением, а не простой аннотацией. [32][33]

**Практическое правило:** сначала попытаться выразить каждое требование через существующие extension/profile points. Если два независимых прототипа не могут получить одинаковый безопасный результат без нового обязательного объекта, этот объект становится кандидатом в A2 semantic core и одновременно проверяется на возможность размещения у владельца соответствующего upstream-слоя. Основание для собственного AgentBridge semantic core появляется только тогда, когда обязательный cross-standard object нельзя отнести ни к одному upstream-проекту и необходимость этого объекта подтверждена несколькими независимыми реализациями. Таким образом, B служит методом безопасного рождения A2, а не конечной архитектурой из адаптеров.

## 6. Governance, adoption и conformance

MCP и A2A уже имеют официальные extension-пути. MCP работает под управлением LF Projects и допускает экспериментальные и официальные независимо версионируемые расширения; A2A требует для официального продвижения reference implementation, adoption evidence, поддержку maintainers и решение TSC. [4][8][36] OpenAPI прямо позволяет инкубировать идеи через `x-*`, а затем вносить формальное предложение с голосованием. [37] UCP демонстрирует практику технического совета и протоколируемых решений, но просмотренные материалы не устанавливают полноценную конституцию IP/governance. [38] Это означает, что безопасная стратегия — **external incubation, затем upstream contribution**, а не преждевременное «upstream-first» без работающего доказательства и не постоянный параллельный стандарт.

MCP и A2A публикуют исполняемые conformance/TCK-инструменты, а UCP показывает, как запускать независимый от языка набор тестов для произвольного merchant server. [5][9][15] Но optional extension может не входить в core conformance, SDK не обязан его поддерживать, а результат зависит от зафиксированной версии и fixtures. Поэтому AgentBridge badge сам по себе ничего не доказывает. Публичное утверждение о совместимости должно включать:

- точные версии всех стандартов и test harness commits;
- исходные fixtures, expected failures и skipped capabilities;
- сырые machine-readable результаты;
- как минимум две независимо поддерживаемые реализации с каждой стороны взаимодействия;
- проверку downgrade, unknown extension, mutation, replay, cancellation, revocation и partial effects.

Отдельный AgentBridge governance без признанного change authority создаёт дополнительный namespace, release matrix и место принятия решений. RFC 6709 предупреждает, что нескоординированные расширения и плохо спроектированные профили способны создавать несовместимые вариации. [33] Поэтому до независимого adoption AgentBridge должен быть **research/test-corpus label**, а не орган стандартизации. Для перехода к самостоятельному стандарту нужны нейтральный charter, IP/patent и antitrust policy, открытое голосование, namespace/trademark rules, succession и конфликт-разрешение.

## 7. Red-team: сильнейшие возражения

### Возражение 1: пробел уже почти закрыт

UCP уже связывает commerce semantics с REST, MCP и A2A, а AP2 добавляет mandate/receipt-модель. AuthZEN Final закрывает policy-decision API, а новые WG drafts целятся в approval, obligations и MCP. [13][14][19][26][27] Для commerce/payment новый AgentBridge слой действительно был бы преимущественно дублированием. Возражение не опровергает наличие пробела в общем deployment-подобном сценарии, но сильно сужает допустимый scope.

### Возражение 2: «профиль» — скрытый core

Если безопасность зависит от обязательного понимания новых объектов, это processing model, а не простая mapping-таблица. RFC 6906 даёт полезную, но не универсальную для всех протоколов аналогию этой границы. [30][32][33] Возражение принято: итоговая рекомендация больше не называет будущий результат обязательно «тонким». Эксперимент должен отдельно доказать, какая часть — profile, а какая — новый semantic extension.

### Возражение 3: отдельный AgentBridge усилит фрагментацию

Существующие держатели стандартов уже предлагают incubation/governance, а AuthZEN активно занимает авторизационный шов. [4][8][27][35] Это главное возражение против отдельного постоянного слоя. Оно не требует немедленно остановить исследование, но требует автоматического перехода к C при отсутствии независимых adopters, upstream sponsorship или доказанного ownership gap.

### Что опровергло бы стратегию A2 через B

Стратегия должна быть отменена в пользу C, если:

- два независимых end-to-end прототипа достигают одинаковой безопасной семантики только native-средствами MCP/A2A/OpenAPI/OAuth/AuthZEN;
- upstream-группы уже принимают и согласованно закрывают все выявленные объекты;
- не удаётся получить хотя бы две независимые клиентские реализации и две независимо поддерживаемые реализации сервисов/инструментов;
- supplemental conformance не предсказывает реальную cross-vendor interoperability.

A2 core может перейти из стратегической гипотезы в нормативный проект только если повторные реализации обнаружат обязательную семантику, которую невозможно корректно разместить ни в одном upstream extension/profile, а участники нескольких экосистем признают отдельного владельца композиционного слоя. A1 не открывается этим результатом: переизобретение проверенных нижних слоёв остаётся вне цели.

## 8. Историческое сравнение вариантов в исходной постановке

Шкала 0–5; итог нормирован к 100. Оценки — прозрачная экспертная эвристика по собранным доказательствам, а не статистическое измерение.

| Критерий | Вес | A1: монолит | A2 через B | B-only | C: только upstream |
| --- | ---: | ---: | ---: | ---: | ---: |
| Уникальность незакрытого пробела | 30% | 3.0 | 4.0 | 3.0 | 2.5 |
| Композиция с существующими стандартами | 20% | 2.0 | 4.5 | 4.5 | 4.5 |
| Безопасность, делегирование, проверяемость | 20% | 3.5 | 4.5 | 3.5 | 3.0 |
| Adoption и нейтральное governance | 15% | 1.5 | 3.0 | 2.5 | 4.0 |
| Стоимость реализации/сопровождения | 10% | 1.0 | 2.5 | 3.5 | 4.5 |
| Обратимость | 5% | 1.5 | 4.0 | 4.0 | 4.0 |
| **Итог** | **100%** | **48** | **78** | **69** | **70** |

### Историческая интерпретация, не являющаяся текущим решением

- **A1 проигрывает:** слишком много уже закрытой поверхности; высокий риск единой критической ошибки, дублирования и низкая вероятность принятия.
- **A2 через B выигрывает условно:** собственная каноническая семантика устраняет опасные межслойные расхождения, а B-эксперимент не позволяет преждевременно угадать состав core.
- **B-only уступает:** постоянный glue layer рискует накопить version matrix, semantic drift и неясное ownership без цельных end-to-end инвариантов.
- **C остаётся обязательным stop-outcome:** это правильный исход, если эксперимент покажет, что собственный semantic core не требуется.

## 9. Новый рекомендуемый доказательный этап

До Validation Charter, PRD, протокольной спецификации, SDK или полной Architecture следует выполнить **Clean-Slate AgentBridge Protocol Architecture, Security & Performance Teardown**:

1. Провести defect/constraint-аудит MCP, A2A, OpenAPI/Arazzo, UCP, ACP, AP2, AuthZEN и смежных инициатив по спецификациям, security guidance, issue trackers, известным interoperability failures и доступным данным реализаций.
2. Определить собственные обязательные end-to-end-инварианты AgentBridge: capabilities, action identity, delegation, consent/approval, lifecycle, idempotency, cancellation, revocation, partial effects, evidence, errors, negotiation и conformance.
3. На равных сравнить native AgentBridge, composition/profile, native core с необязательными bridges и upstream/stop-вариант. Не считать reuse автоматически более безопасным, а clean-slate — автоматически более быстрым.
4. Сформировать несколько candidate architectures с trust boundaries, state machines, failure trees, downgrade/confused-deputy/replay анализом и явными зависимостями.
5. Отделить допустимое reuse проверенных transport/identity/authorization/cryptographic primitives от обязательной семантической или runtime-зависимости от чужих agent-протоколов.
6. Подготовить benchmark matrix для latency p50/p95/p99, throughput, CPU, memory, wire bytes, round trips, handshake/crypto cost, streaming/backpressure и recovery при потерях/повторах; сравнить кандидаты wire format/transport и роль Rust, Go, TypeScript и Python.
7. Определить verification programme: формальная модель критических state machines, test vectors, negative conformance, property tests, fuzzing, независимые реализации и внешний security review.
8. Завершить архитектурным Gate 0: выбрать проверяемую гипотезу, указать доказательства, остаточные риски, falsification criteria и условия перехода к Validation Charter/PRD.

Существующие стандарты в этом этапе являются evidence/control corpus и кандидатами для необязательных bridges. Две нативные реализации AgentBridge не должны зависеть от них или обязательного AgentBridge-посредника. Это исследование **не** разрешает создавать `SPECIFICATION.md`, JSON Schema, SDK, полную архитектуру или код.

## 10. Открытые вопросы

1. Какова минимальная полноценная native protocol surface, достаточная для независимого agent-to-service взаимодействия без MCP/A2A/UCP runtime?
2. Как формально отличить «execution accepted», «execution completed» и «external effect verified» без ложного обещания истины внешнего мира?
3. Какие foundational primitives можно безопасно переиспользовать, не передавая внешнему стандарту контроль над семантикой и жизненным циклом AgentBridge?
4. Где bridges допускают lossless mapping, а где обязаны fail closed?
5. Какие trust anchors и attestors допустимы для разных доменов?
6. Как система должна безопасно деградировать при неизвестном profile/extension?
7. Как измерить, что общий conformance-пакет предсказывает реальную интероперабельность, а не только прохождение собственных fixtures?
8. Какой путь к нейтральному governance реалистичен для самостоятельного protocol suite?
9. Какой wire format/transport и какой язык reference core подтверждаются измерениями, а не предположениями?

## 11. Карта устаревания

Карта вычислена из реестра динамических утверждений с окнами в один месяц для version/compatibility, три месяца для scope и шесть месяцев для ecosystem. На дату отчёта просроченных проверок нет.

| Группа утверждений | Класс | Проверено как current-as-served | Перепроверить не позднее |
| --- | --- | --- | --- |
| MCP core/auth/Tasks; A2A spec/releases/extensions/TCK; OpenAPI/Arazzo | version/compatibility | 2026-09-11 | **2026-10-11** |
| UCP/ACP/AP2/x402; AuthZEN и IETF agent drafts | version/compatibility | 2026-09-11 | **2026-10-11** |
| Отсутствие стабильного общего authorization-to-effect profile в ограниченном поиске | scope | 2026-09-11 | 2026-12-11 |
| Governance, conformance и публичные adoption-сигналы | ecosystem | 2026-09-11 | 2027-03-11 |

Следующая обязательная перепроверка — **11 октября 2026 года**. Стабильные RFC не «протухают» как нормативный текст, но их роль в экосистеме следует пересматривать вместе с новыми профилями.

Особенно быстро могут изменить решение: MCP Tasks; A2A patch/spec synchronisation; последующие редакции Arazzo; AuthZEN approval/obligation/MCP drafts; UCP stable releases; AP2/FIDO; IETF work по agent authorization. Черновик Agent Operation Authorization уже принят рабочей группой WIMSE, но остаётся work in progress и не представляет IETF consensus. [28][34]

## 12. Источники

Все источники доступны публично; дата доступа для всех — **2026-09-11**.

| № | Источник и поддерживаемый вывод | Издатель | Публикация/статус | Уверенность |
| ---: | --- | --- | --- | --- |
| [1] | [MCP Specification 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28) — роли, JSON-RPC, capabilities, safety boundary | MCP / LF Projects | revision 2026-07-28 | высокая |
| [2] | [MCP Authorization](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization) — OAuth resource access/scopes, не action approval | MCP / LF Projects | revision 2026-07-28 | высокая |
| [3] | [MCP Tasks draft](https://tasks.extensions.modelcontextprotocol.io/specification/draft/tasks) — lifecycle, result/error/cancel distinctions | MCP Tasks extension | draft, revision 2026-07-28 | высокая |
| [4] | [MCP SEP-2133 Extensions](https://modelcontextprotocol.io/seps/2133-extensions) — official/experimental extension governance | MCP | Final; created 2025-01-21 | высокая |
| [5] | [MCP Conformance](https://github.com/modelcontextprotocol/conformance) — external client/server harness и version pinning | MCP | living repository | высокая |
| [6] | [A2A Specification v1.0.0](https://a2a-protocol.org/v1.0.0/specification/) — tasks, artifacts, bindings, auth boundary | A2A / Linux Foundation | 2026-03-12 | высокая |
| [7] | [A2A Releases](https://github.com/a2aproject/A2A/releases) — v1.0.1 patch и необходимость pinning | A2A / Linux Foundation | v1.0.1, 2026-05-26 | высокая |
| [8] | [A2A Extension & Binding Governance](https://a2a-protocol.org/latest/topics/extension-and-binding-governance/) — incubation, adoption и TSC gates | A2A / Linux Foundation | current-as-served | высокая |
| [9] | [A2A TCK](https://github.com/a2aproject/a2a-tck/blob/main/README.md) — тест внешних endpoints и отчёты | A2A / Linux Foundation | living repository | высокая |
| [10] | [OpenAPI Specification 3.2.0](https://spec.openapis.org/oas/latest.html) — HTTP API description, не execution/attestation | OpenAPI Initiative | 2025-09-19 | высокая |
| [11] | [Arazzo Specification](https://spec.openapis.org/arazzo/latest.html) — workflows/criteria без security enforcement/effect proof | OpenAPI Initiative | published 1.1.0; living editorial work | высокая с оговоркой версии |
| [12] | [RFC 9635: GNAP](https://www.rfc-editor.org/info/rfc9635/) — delegated grant mechanics | IETF / RFC Editor | October 2024, Proposed Standard | высокая |
| [13] | [UCP Specification 2026-08-25](https://ucp.dev/2026-08-25/specification/overview/) — commerce profiles/capabilities/bindings | UCP project | stable 2026-08-25 | высокая |
| [14] | [UCP A2A Checkout Binding](https://ucp.dev/latest/specification/checkout-a2a/) — реальная cross-protocol commerce composition | UCP project | latest stable surface | высокая |
| [15] | [UCP Conformance](https://github.com/Universal-Commerce-Protocol/conformance/blob/main/README.md) — language-agnostic tests against merchant servers | UCP project | living repository | высокая |
| [16] | [Agentic Commerce Protocol repository](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) — beta и stable snapshots | OpenAI / Stripe ACP project | beta; stable 2026-04-17 snapshot | высокая |
| [17] | [ACP Agentic Checkout OpenAPI](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.agentic_checkout.yaml) — checkout/order/idempotency | ACP project | 2026-04-17 | высокая |
| [18] | [ACP Delegate Payment OpenAPI](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/openapi/openapi.delegate_payment.yaml) — constrained delegated payment | ACP project | 2026-04-17 | высокая |
| [19] | [AP2 v0.2 Specification](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md) — mandates, bindings, receipts, commerce scope | Google Agentic Commerce / AP2 | pre-1.0 living spec | высокая |
| [20] | [AP2 Agent Authorization](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/agent_authorization.md) — reusable authorization skeleton и текущие границы | Google Agentic Commerce / AP2 | living document | высокая |
| [21] | [x402 Specification v2](https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md) — payment verification/settlement, не general delegation | x402 Foundation | version 2, living spec | высокая |
| [22] | [RFC 9396: OAuth Rich Authorization Requests](https://www.rfc-editor.org/rfc/rfc9396.html) — typed authorization details, profile-defined semantics | IETF / RFC Editor | May 2023, Standards Track | высокая |
| [23] | [RFC 8693: OAuth Token Exchange](https://www.rfc-editor.org/rfc/rfc8693.html) — subject/current/prior actor representation | IETF / RFC Editor | January 2020, Standards Track | высокая |
| [24] | [RFC 9449: DPoP](https://www.rfc-editor.org/rfc/rfc9449.html) — sender binding, base proof excludes query/body | IETF / RFC Editor | September 2023, Standards Track | высокая |
| [25] | [OpenID for Verifiable Presentations 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) — verifier binding и typed transaction data | OpenID Foundation | Final 1.0, 2025-07 | высокая |
| [26] | [AuthZEN Authorization API 1.0 Final](https://openid.net/specs/authorization-api-1_0-final.html) — subject/resource/action/context decision API | OpenID Foundation | Final, 2026-01-11 | высокая |
| [27] | [AuthZEN Specifications](https://openid.net/wg/authzen/specifications/) — approval, obligations, COAZ/MCP work | OpenID Foundation | Final API + active WG drafts | высокая для статуса, средняя для будущего результата |
| [28] | [Agent Operation Authorization draft](https://datatracker.ietf.org/doc/draft-klrc-aiagent-auth/) — emerging fine-grained token, не execution receipt | IETF WIMSE WG / Datatracker | draft-03, WG-adopted work in progress | средняя |
| [30] | [RFC 9421: HTTP Message Signatures](https://www.rfc-editor.org/rfc/rfc9421.html) — обязательные profile-specific signature/verifier rules | IETF / RFC Editor | February 2024, Standards Track | высокая |
| [31] | [RFC 8785: JSON Canonicalization Scheme](https://www.rfc-editor.org/rfc/rfc8785.html) — deterministic JSON for hashing/signing | RFC Editor, Independent Stream | June 2020 | высокая |
| [32] | [RFC 6906: Profile Link Relation](https://www.rfc-editor.org/rfc/rfc6906.html) — профиль не меняет базовую семантику для unaware processor | RFC Editor, Independent Stream | March 2013 | высокая |
| [33] | [RFC 6709: Protocol Extension Design](https://www.rfc-editor.org/rfc/rfc6709.html) — major extensions, fragmentation и change authority | IAB / RFC Editor | September 2012 | высокая |
| [34] | [FIDO to develop standards for trusted AI-agent interactions](https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/) — AP2 contribution under review | FIDO Alliance | 2026-04-28 | высокая для передачи, средняя для будущего результата |
| [35] | [Architectural Requirements for Supporting AI Agents on the Internet](https://www.ietf.org/archive/id/draft-daniel-ai-agent-internet-architecture-01.html) — anti-duplication direction, not consensus | IETF Internet-Draft repository | 2026-08-28, individual draft | средняя |
| [36] | [MCP Governance](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/GOVERNANCE.md) — LF governance and contribution terms | MCP / LF Projects | living repository | высокая |
| [37] | [OpenAPI contribution process](https://github.com/OAI/OpenAPI-Specification/blob/main/CONTRIBUTING.md) — `x-*` incubation and formal proposal path | OpenAPI Initiative / Linux Foundation | living policy | высокая |
| [38] | [UCP Tech Council minutes](https://github.com/Universal-Commerce-Protocol/meeting-minutes/blob/main/tc/2026/2026-03-13.md) — council/election/proposal practice, not full constitution | UCP project | 2026-03-13 | высокая для наблюдаемой практики |
