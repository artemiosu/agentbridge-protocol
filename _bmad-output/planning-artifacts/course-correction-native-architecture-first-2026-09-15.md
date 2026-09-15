---
title: "AgentBridge Course Correction: Native Architecture-First"
status: approved
created: 2026-09-15
visibility: private-repository
decision_owner: Project Owner
change_class: major-pre-implementation
approved: 2026-09-15
---

# AgentBridge Course Correction: Native Architecture-First

## 1. Решение простыми словами

Project Owner определил продуктовый курс: **самостоятельный Native AgentBridge Protocol будет спроектирован и реализован**. Повторная проверка вопроса «создавать ли проект вообще» прекращается.

Следующая цель — не как можно быстрее написать код, а сначала получить целостную, проверяемую и рассчитанную на развитие архитектуру. Проверки и экспертные советы сохраняются, но теперь отвечают на конкретные вопросы:

1. безопасна ли предложенная архитектура;
2. что обязано входить в Core, а что должно остаться Profile, Binding, Bridge или внешней инфраструктурой;
3. какие решения необходимо исправить до реализации;
4. какой ограниченный scope разрешено реализовать;
5. какие заявления о безопасности, универсальности, adoption и статусе стандарта уже доказаны, а какие ещё нет.

Ни один совет больше не возвращает проект к исходному голосованию `native/profile/upstream/stop`. Он может потребовать переработать конкретное решение, сузить первый релиз, исключить опасную возможность или заблокировать конкретный релиз.

## 2. Почему требуется смена курса

Текущие PRD и Validation Charter были сознательно построены как outcome-neutral испытание. Они запрещают полную Architecture до Gate 1 и допускают остановку Native Core. Это больше не соответствует решению Project Owner.

При этом выполненная работа не потеряна:

- Evaluation Invariants EI-01–EI-25 становятся обязательными требованиями архитектуры;
- Safety Blocking Classes SBC-01–SBC-10 становятся безусловными блокерами небезопасного дизайна и релиза;
- принятые правила измерений, независимости, IP, хранения и публикации сохраняются как источник требований и доказательная история;
- исследования MCP, A2A, OpenAPI/Arazzo, UCP, ACP, AP2, GNAP, AuthZEN и других инициатив используются для поиска полезных механизмов, рисков, ошибок и optional fail-closed bridges, но не делают эти инициативы обязательными runtime-зависимостями Native Path.

## 3. Новая формула проекта

**Native, architecture-first, evidence-assured.**

- `Native`: две нативные реализации способны взаимодействовать без обязательного MCP, A2A, UCP, AgentBridge Cloud, broker, registry или иного чужого agent-protocol runtime.
- `Architecture-first`: нормативный смысл, trust boundaries, state machines, security, evolution и conformance проектируются до production-кода.
- `Evidence-assured`: существенные решения проверяются моделями, прототипами, независимыми реализациями, adversarial tests и специализированными review.
- `Open and neutral`: Core, обязательные Bindings и conformance доступны для независимой royalty-free реализации; коммерческие сервисы добровольны и заменяемы.
- `Future-ready`: архитектура не пытается угадать все будущие сценарии, а безопасно обрабатывает неизвестность, поддерживает управляемые расширения и допускает замену инфраструктуры.

Гарантия «100% идеальной архитектуры» невозможна для новой сложной технологии. Рабочая цель — архитектура, в которой критические правила формальны и проверяемы, неизвестность завершается безопасно, ошибки можно обнаружить до реального ущерба, а спорные части можно заменить без разрушения Core.

## 4. Что меняется и что не меняется

| Было | Становится |
| --- | --- |
| Gate 1 выбирает `native/profile/upstream/stop` | Project Owner выбрал проектирование Native; ворота проверяют готовность конкретной архитектуры |
| Полная Architecture запрещена до Gate 1 | Архитектурное проектирование разрешается после утверждения этой смены курса |
| Все незакрытые OQ блокируют любое проектирование и код | Frozen policy используется сразу; точные operational параметры закрываются перед тем этапом, которому они нужны |
| Composition Challenger может остановить Native | Существующие решения служат источниками reuse, comparison, compatibility и failure lessons |
| Неопределённость ведёт к общему `No Start` | Конкретный риск блокирует только соответствующий design, prototype, release или claim |
| Неуспех одного теста способен остановить отдельный Core | Обычно он требует redesign, scope reduction, профильного выделения или отказа от конкретной возможности |

Не меняются:

- role-neutral и direction-neutral охват agent↔service, agent↔agent, service↔service/B2B, reverse async и multi-party flows;
- различение Participant, Principal, Role, authority, consent/approval, delivery, execution, effect и evidence;
- fail-closed поведение при security-critical неизвестности;
- отсутствие обещания exactly-once;
- честные `unknown`, `partial`, cancellation, compensation и recovery semantics;
- отсутствие собственной криптографии, глобального identity provider, payment rail или отраслевого источника истины;
- заменяемость transport, identity, trust, policy, audit и discovery infrastructure;
- необязательность bridges и явное объявление semantic loss;
- bounded resource use, privacy, versioning, provenance, независимая реализуемость и открытый conformance;
- запрет реальных consequential effects, production credentials и customer data до специальных поздних ворот.

## 5. Новая система Architecture Assurance Gates

### AA-0 — Course Decision

Результат: принята эта смена курса. Исторические документы сохраняются; выпускается новая ревизия PRD и новый Architecture Assurance Charter.

### AA-1 — Architecture Constitution

Обязательные артефакты:

- границы `Core / Profiles / Extensions / Bindings / Bridges / external infrastructure`;
- архитектурные принципы и non-negotiable invariants;
- quality-attribute scenarios;
- единый словарь;
- карта EI/SBC → архитектурная ответственность;
- ADR-процесс и реестр открытых решений;
- минимальный, но целостный scope первой версии.

Выход: все принятые инварианты распределены; Core не содержит универсальную доменную онтологию, обязательную коммерческую платформу или скрытую центральную точку.

### AA-2 — Abstract Normative Model

Обязательные артефакты:

- participants, principals, roles, audience и interaction context;
- interaction modes и correlation;
- capability/version/extension negotiation;
- action, authority, delegation, consent и approval model;
- lifecycle/effect/evidence/error state machines;
- retry, replay, cancel, revoke, expiry, partial effect, compensation и recovery rules;
- модель multi-party и reverse asynchronous flows.

Критические части проверяются исполнимой или формальной моделью. Конкретный инструмент выбирается по задаче, а не заранее.

Выход: нет противоречивых критических переходов; неизвестный эффект не превращается в успех; каждый опасный переход сопоставлен с EI/SBC.

### AA-3 — Security, Privacy and Trust Architecture

Обязательные артефакты:

- threat model и misuse/abuse cases;
- trust, data и enforcement boundaries;
- attack/failure trees;
- проверка authority непосредственно на границе внешнего эффекта либо доказуемая связь между проверкой и эффектом; если глобальная атомарность невозможна — явные non-atomic, `partial` и `unknown` semantics;
- canonicalization/signing/verifier profiles на проверенных primitives;
- privacy/linkability/metadata model;
- compromise, key rotation, recovery и incident model;
- dependency/supply-chain policy.

Выход: ноль любых незакрытых SBC, ноль незакрытых Critical/High и ноль иных обязательных gate failures для выбранного scope. Residual-risk acceptance применима только к рискам вне этих категорий; иначе опасная возможность исправляется, исключается или откладывается вместе со всем зависимым scope.

### AA-4 — Wire, Binding and Runtime Architecture

Сравниваются transport, encoding, streaming, browser/server/constrained environments, fallback, backpressure и языки реализации. Для этого разрешены только ограниченные disposable-прототипы и исполнимые модели: в sandbox, на синтетических данных, без production credentials/эффектов, после применимой проверки прав, dependency manifest/SBOM, изолированного хранения, deny-by-default egress, resource ceilings, observers и retention/deletion rules. Они не являются reference или production implementation.

Выход:

- выбран как минимум один полный Native Binding;
- детерминированно определено подписываемое содержание;
- определены лимиты размера, глубины, времени, памяти, очередей, fan-out, retries и amplification;
- downgrade/unknown mandatory elements обрабатываются fail-closed;
- выбор Rust, Go или другого стека подтверждён требованиями, прототипами и benchmarks.

### AA-5 — Conformance and Independent Implementability

Обязательные артефакты:

- normative assertions;
- позитивные и негативные vectors;
- property, fuzz, differential, concurrency и fault-injection plans;
- observer и evidence model;
- coverage map;
- rules for conformance/security claims.

Выход: каждое критическое требование связано с тестом, моделью или обязательным независимым review; неполное наблюдение не может дать `pass`; тесты не зависят от reference implementation.

### AA-6 — Integrated Architecture Baseline

Все предыдущие решения проверяются вместе, включая противоречия между security, privacy, performance, evolvability и usability.

Выход:

- Architecture Baseline v0.x;
- ноль любых открытых SBC, ноль открытых Critical/High и ноль иных обязательных gate failures;
- остальные вопросы имеют статус `fix now`, `profile`, `defer` или `accepted residual risk` с owner и сроком;
- точная граница первой reference implementation;
- разрешение только на non-production implementation выбранного scope.

### AA-7 — Implementation, Interoperability and Hardening

Создаются reference implementation и как минимум одна spec-only/clean-room реализация без общей protocol-semantic codebase. До первого reference-кода замораживаются digest нормативного baseline, одинаковый пакет материалов, независимый oracle/conformance skeleton, раздельные команды/контексты, правила доступа к reference behavior, exposure/genealogy log и порядок adjudication расхождений. Clean-room track начинает работу по замороженной спецификации до доступа к reference behavior либо действует в доказуемо изолированном режиме. Проводятся conformance, interop, security, resilience, performance и upgrade tests.

Выход: внутренний Implementation/Interop Candidate либо возврат конкретных требований на redesign.

### AA-8 — Public Draft and Ecosystem Readiness

До публикации проверяются specification/IP licenses, patent commitments, contribution rules, trademark, disclosure, reproducible conformance package и минимальный governance charter. До публичного draft уже действуют открытые proposals/diffs/decisions/dissent, conflict disclosure/recusal, appeal, emergency-security path, разделённый контроль release keys, namespace, protocol marks и conformance oracle, а также заранее определённые milestones перехода к более независимому управлению.

Выход: публичный `0.x Developer Preview`. Он не называется production-ready или индустриальным стандартом.

### AA-9 — Limited Production Safety

Отдельный safety/compliance charter разрешает только явно ограниченные production-сценарии, домены и юрисдикции. Техническая готовность не считается доказательством adoption, коммерческого спроса или статуса стандарта.

### AA-10 — External Adoption

Внешние команды реализуют протокол без закрытых пояснений автора; реальные пилоты доказывают single-player value, interop, migration и retention. До широких adoption claims действует отдельный Adoption Charter; минимальный исходный ориентир — не менее четырёх независимо контролируемых implementations/deployments, если Charter не установит более строгий, доказательно обоснованный порог.

### AA-11 — Open-Standard Legitimacy

Нейтральный steward/consortium/standards-body path запускается по заранее установленным milestones. Заявление об индустриальном стандарте требует независимого adoption и реально работающего нейтрального change process, а не только сильной архитектуры или проекта основателя.

### BV-1…BV-n — Параллельный Business Validation Track

Коммерческий спрос проверяется отдельно от технических и adoption-ворот: конкретный покупатель, проблема, измеримая ценность, willingness-to-pay, повторное использование/продление и kill-критерий каждого платного слоя. Платное исследование и non-production design-partner work возможно раньше production, но не даёт спонсору особых нормативных прав и не разрешает production promises.

## 6. Условия допуска к первой reference implementation

Реализация разрешается после AA-6, если одновременно:

1. утверждена Architecture Baseline;
2. определён минимальный нормативный контракт;
3. пройдены threat/security reviews;
4. критические authority/effect/state модели проверены;
5. выбран Native Binding;
6. определены canonicalization, version negotiation и resource ceilings;
7. существует независимый от реализации conformance skeleton с негативными vectors;
8. нет открытых SBC и Critical/High в реализуемом scope;
9. проверены лицензии и provenance;
10. reference implementation объявлена ненормативной;
11. заморожены digest/spec package, независимый oracle/conformance skeleton, разделение команд, доступ к reference behavior, genealogy/exposure и adjudication; clean-room/spec-only track запускается до reference behavior либо доказуемо изолирован;
12. production effects, customer data, реальные credentials и необратимые действия запрещены.

Реализация идёт вертикальными срезами: negotiation/safe refusal → information exchange → consequential action → retry/replay/unknown → delegation/revoke → reverse async или multi-party → optional fail-closed bridge.

## 7. Экспертные консилиумы

Проводятся специализированные советы, а не один бесконечный общий совет:

1. protocol semantics и distributed systems;
2. identity, authorization, delegation, consent и applied cryptography;
3. privacy, abuse, malicious agents и safety;
4. formal methods, state machines, concurrency и fault semantics;
5. networking, encoding, performance и resource safety;
6. conformance, interoperability, SDK implementability и developer experience;
7. versioning, extensions, upgrade и ecosystem compatibility;
8. open standards governance, IP, patent, trademark и antitrust;
9. adoption, migration и product value;
10. OSS business model, ecosystem и conflict-of-interest firewall;
11. cross-discipline red team перед реализацией и перед публичным draft.

Правила против затягивания:

- у каждого совета один зафиксированный предмет, входы и срок;
- результат — `pass`, `pass with conditions` или `redesign/block`;
- каждый blocker обязан назвать нарушенное требование, доказательство, исправление и критерий закрытия;
- любой SBC, Critical/High, обязательный gate failure, неустановленное право/license/patent/provenance, юридический запрет, отсутствие требуемой независимости, evidence или conformance блокирует соответствующий artifact, use, release или claim;
- Medium/Low исправляются либо принимаются/переносятся с owner и сроком;
- не более двух полных циклов review на один baseline; затем Chief Architect выбирает исправление, сужение scope или defer зависимой возможности, но нерешённый обязательный blocker нельзя принять как residual risk или перенести так, чтобы пройти gate;
- dissent сохраняется, но сам по себе не запускает бесконечный цикл;
- старое решение открывается только при новом материальном evidence: уязвимость, формальное противоречие, провал interop/model, существенное изменение зависимости;
- Project Owner принимает продуктовый риск, но не может отменить критический security/IP запрет одним решением.

AI-консилиумы дают внутренний широкий review, но не считаются реальной независимой мировой экспертизой. Внешняя проверка считается независимой только при документированных компетенции, независимом controller/employer или ином достаточном separation, раскрытии конфликтов, фиксированном scope и подписанном verdict. Внешние специалисты привлекаются поэтапно: security/formal/protocol review до Developer Preview; IP/trademark counsel до публичного выпуска; независимые implementers до interop claims; внешние auditors и governance participants до production/standard claims. Их отсутствие не блокирует внутреннее проектирование.

## 8. Коммерческая стратегия основателя

### 8.1 Основной принцип

**Открытый стандарт создаёт рынок; компания основателя продаёт лучший, но не обязательный способ безопасно использовать этот рынок.**

Личная коммерческая выгода возникает прежде всего из значительной доли основателя в коммерческой компании, её продуктов, клиентов, компетенций, бренда и дистрибуции, а не из платы за право использовать протокол.

### 8.2 Две разделённые системы

Нейтральная система стандарта:

- открытая normative specification;
- royalty-free право независимой реализации;
- обязательные Base Bindings;
- бесплатные conformance rules, suite и test vectors;
- как минимум одна полноценная reference implementation и достаточные базовые SDK;
- открытая история изменений, security advisories и совместимость;
- правила protocol mark, которые не требуют покупки коммерческой услуги;
- со временем — независимый steward.

Коммерческая компания основателя:

- paid design-partner integrations и security readiness;
- enterprise support, обучение и SLA;
- hosted sandbox/conformance/diagnostics;
- managed gateway/control plane, policy, authority, observability и audit operations;
- private cloud/on-prem, IAM/SIEM/HSM и compliance packages;
- managed bridges/connectors и migration tooling;
- incident response и threat intelligence;
- заменяемые federated discovery/trust services;
- позднее — transaction/risk/dispute services, только если компания реально создаёт эту ценность и принимает соответствующие обязанности.

### 8.3 Лестница монетизации

1. Платные design-partner discovery и non-production интеграции без особых нормативных прав, эксклюзивных функций или привилегированного conformance.
2. Enterprise support, обучение и архитектурная помощь.
3. Hosted developer platform: sandbox, diagnostics, compatibility monitoring.
4. Managed production platform с SLA и portability.
5. Security/compliance/on-prem products.
6. Добровольная сертификация при открытых критериях и нескольких аудиторах.
7. Федеративные discovery/trust/reputation services.
8. Transaction/risk/dispute services за реально оказываемую услугу.
9. Marketplace только после portability, governance и прозрачного ranking.

Реклама, paid ranking, обязательная комиссия «за использование протокола», единственная платная сертификация, закрытый conformance или обязательный cloud не являются рекомендуемой основой бизнеса: они разрушают доверие к нейтральному стандарту.

### 8.4 Защищаемые активы компании

- доля основателя и корректно оформленные права компании;
- скорость и качество реализации;
- эксплуатационная надёжность и security operations;
- migration know-how, integrations и partnerships;
- лучший developer experience;
- enterprise trust, support и договорная ответственность;
- proprietary operations/management plane с открытыми protocol interfaces и export/portability;
- агрегированная обезличенная threat intelligence;
- отдельный коммерческий бренд.

На раннем этапе фонд не создаётся без сообщества. До публичного выпуска должны быть определены code/spec licenses, patent commitments, contribution terms, trademark и возможность будущей передачи essential protocol assets нейтральному steward. Инвестор или покупатель компании не должен иметь возможность отозвать уже выданные права на Core.

Отдельный Protocol/Platform Firewall обязан определить владельцев specification, namespace и marks; recusal и related-party/sponsor rules; запрет покупки roadmap, embargo или conformance verdict; независимую certification appeal; разделение protocol evidence и commercial data; отсутствие специальных прав коммерческой платформы; export/portability и provider-exit tests. Финансирование нейтральной части не покупает голос или технический результат.

Будущий Business & Ecosystem Strategy обязан также:

- определить минимальный открытый client/operator SDK baseline, исключающий платную монополию на security- или interoperability-critical функции;
- потребовать федерацию, переносимость, прозрачный ranking и appeal для discovery/trust/reputation services;
- разрешать threat intelligence только при законном основании, минимизации, защите от повторной идентификации и запрете скрытого сбора protocol traffic;
- задать объективные milestones передачи essential assets более независимому управлению;
- определить модель финансирования нейтрального стандарта, не продающую голоса или приоритет;
- создать для каждого платного слоя карточку `покупатель → проблема → измеримая ценность → willingness-to-pay → retention → kill-критерий`.

### 8.5 Маховик принятия

1. Спроектировать Core вместе с conformance, а не после него.
2. Доказать независимую реализацию и interop.
3. Выбрать первый клин: безопасная цепочка `authority → action → effect → evidence`.
4. Проверить её минимум в коммерческом и некоммерческом домене.
5. Дать первой организации полезность без массовой сети: auditability, safe retries, authority controls и bridges к существующим API.
6. Выпустить честный Developer Preview.
7. Проводить interoperability events и публиковать разрешённые отрицательные результаты.
8. Получить внешние пилоты, повторное использование и retention без постоянной помощи автора.
9. Передать изменение нормы нейтральному процессу.
10. Продвигать optional bridges к существующим экосистемам как путь миграции.
11. Добиваться включения протокола в platform requirements, SDK и enterprise procurement.
12. Идти в подходящий standards body только с running code, независимыми реализациями и adoption evidence.

## 9. Что способно остановить конкретный дизайн или релиз

- неоднозначность между разрешённым и запрещённым действием;
- скрытое расширение полномочий;
- replay/double effect или ложное подтверждение внешнего результата;
- fail-open version/extension/bridge behavior;
- обязательная коммерческая или центральная точка;
- невозможность независимой реализации;
- конфликт privacy и audit без безопасного разрешения;
- неограниченные очереди, retry storms, fan-out/state explosion или resource exhaustion;
- небезопасный upgrade либо фрагментация совместимости;
- patent/license restriction, несовместимый с royalty-free Core.

Обычно последствие — redesign, уменьшение scope, перенос в Profile/Binding либо defer. Emergency pause/stop всего проекта остаётся допустим при неустранимой небезопасности, юридической невозможности royalty-free Core, доказанной практически нереализуемой сложности или невозможности независимой реализации. Недостаток ресурсов означает `pause / re-scope / seek resources / archive until funded`, а не техническое опровержение. Полный stop требует определённого evidence, применимой I2-рекомендации и отдельного решения Project Owner. Это аварийная граница, а не повторное плановое голосование о существовании проекта.

## 10. Изменения утверждённых документов после принятия

1. Создать Course Decision Record с owner rationale и датой.
2. Выпустить новую ревизию PRD, сохранив историю прежней версии.
3. Сохранить traceability постоянных FR-ID: прежнюю outcome-neutral семантику Gate 1/FR-110 пометить superseded данным Course Decision и выпустить явно версионированную замену, а не бесследно удалить историю.
4. Перевести A-1 из вопроса о разрешении проекта в вопрос о точной границе Native Core.
5. Разрешить Architecture, formal models, threat model, conformance design и bounded research prototypes до production implementation.
6. Сохранить frozen OQ-2A–OQ-8A как policy baselines, классифицировав их положения как `retained`, `adapted for architecture assurance` или `historical/superseded`; пометить старый Validation Charter `superseded for existential decision, retained as evidence and requirements source`.
7. Создать новый Architecture Assurance Charter с traceability к EI/SBC/FR/NFR.
8. Создать отдельные Architecture и Business & Ecosystem Strategy artifacts.
9. Сохранить поздние Gate для независимой реализации, public release, production safety, adoption, governance и commercial validation.
10. Не фиксировать Rust, transport, encoding, cloud, SDK languages, repository layout, pricing или corporate structure до соответствующего design/research/legal решения.

## 11. Влияние и классификация изменения

Класс: **Major pre-implementation course correction**.

Эпики, stories, Sprint Status и implementation code ещё не созданы. Поэтому стандартный sprint-oriented Correct Course не применяется буквально; изменение должно быть внесено сначала в PRD/Charter, затем выполнен BMAD Architecture, после чего создаются epics/stories.

Ответственные:

- Project Owner: утверждает курс, product scope, residual business risks и коммерческие принципы;
- PM: обновляет PRD и evidence gates;
- Chief Architect: создаёт Architecture Constitution/Baseline и сохраняет целостность решений;
- Security/Privacy leads: владеют SBC review и блокируют небезопасный scope/release;
- Standards/IP counsel: проверяют RF, contributions, patents, trademarks и governance перед publication;
- Business/Strategy lead: создаёт Business & Ecosystem Strategy и проверяет willingness-to-pay;
- независимые implementers/reviewers: подтверждают spec-only implementability и interoperability.

## 12. Явные границы текущего разрешения

Принятие этого документа разрешает:

- обновить Product Brief/PRD/Charter;
- начать BMAD Architecture;
- создавать формальные модели, threat models, decision records и conformance design;
- создавать bounded disposable architecture prototypes и исполнимые модели только после применимых rights/storage/egress/resource/observer controls; планировать external reviews.

Оно пока не разрешает:

- production implementation или публичный production release;
- реальные consequential effects, customer data или production credentials;
- денежные расходы и внешние договоры без отдельного согласия Project Owner;
- публичные заявления `industry standard`, `production-ready`, `secure` или доказанную рыночную/финансовую оценку;
- публикацию приватных исходных документов;
- регистрацию компании, фонда, товарного знака или передачу прав без отдельного решения и юридической проверки.

## 13. Решение, требуемое от Project Owner

После понятного объяснения принять либо отклонить следующую формулировку:

> Утверждаю курс Native Architecture-First. Вопрос о создании AgentBridge Native Protocol закрыт положительно. Сохраняю принятые safety, independence, evidence и IP requirements как обязательные ограничения. Разрешаю обновить PRD и заменить existential Gate 1 на Architecture Assurance Gates, после чего перейти к BMAD Architecture. Коммерческая стратегия строится через отдельную компанию основателя и добровольные заменяемые сервисы, не превращая открытый Core в платную или централизованную зависимость.
