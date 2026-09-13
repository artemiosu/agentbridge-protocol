# Независимая проверка безопасности и distributed-systems safety: AgentBridge PRD

**Объект:** `prd.md`, версия от 2026-09-13  
**Линза:** protocol security, authority/delegation, consequential effects, privacy, bridges, resilience и безопасность Gate 1  
**Статус проверки:** завершена  
**Вердикт:** **Changes Required перед финализацией PRD и freeze Validation Charter**

## 1. Итог

PRD задаёт необычно сильную основу: Участник отделён от Принципала и Endpoint; identity, authentication, Authority, Consent, Approval, исполнение и внешний эффект не смешиваются; `unknown` и `partial` являются первоклассными состояниями; exactly-once и абсолютная истинность Evidence не обещаются; bridges обязаны быть наблюдаемыми и fail-closed; Native Path не требует центрального сервиса; conformance проверяет запрещённые эффекты, а не только форму сообщений.

Однако в текущем тексте остаются четыре критические неоднозначности, при которых небезопасный кандидат теоретически может пройти Gate 1:

1. hard gate блокирует только findings, названные `critical`, но PRD не запрещает классифицировать воспроизводимый high-impact safety defect ниже этого уровня;
2. Authority не обязана повторно связываться с окончательно разрешённым Resource и состоянием непосредственно в точке необратимого commit;
3. общий use/amount/rate limit может быть одновременно израсходован разделёнными ветвями при partition без установленного безопасного механизма либо обязательного non-permit;
4. «0 наблюдаемых эффектов» может оказаться ложным pass, если effect observer неполон, слеп к одному каналу либо сам отказал.

Это не требует выбирать токены, криптографию, wire format, transport, хранилище или алгоритм консенсуса в PRD. Нужны только обязательные safety-инварианты и условия валидности эксперимента; конкретные механизмы по-прежнему выбираются в Charter, Profiles и Bindings.

### Распределение findings

| Уровень | Количество |
| --- | ---: |
| Critical | 4 |
| High | 8 |
| Medium | 4 |
| Low | 0 |
| **Всего** | **16** |

## 2. Оценка falsifiability Gate 1

### Что уже проверяемо

- **FR-101, SM-2 и H-3** задают нулевую терпимость к запрещённым раскрытиям, действиям, расширению Authority, ложной определённости Effect State, повышению Evidence Assurance и security-critical Semantic Loss.
- **FR-102, SM-3, SM-5, H-5** требуют независимых реализаций, одинаковых нормативных outcomes и отсутствия закрытых пояснений.
- **FR-88–FR-92, VC-1–VC-13** требуют mutation, concurrency, fault, bridge, downgrade, privacy и resource cases с воспроизводимым evidence.
- **FR-109–FR-110** дают outcome-neutral функцию решения и оставляют Gate закрытым при неполном evidence.

### Что пока допускает ложный pass

Hard gates ещё не полностью falsifiable, пока Charter может свободно определить severity rubric, область effect observation, допустимую freshness, shared-limit enforcement и privacy scope. OQ-3/OQ-4 правильно переносят числовые значения и конкретные механизмы в Charter, но PRD должен заранее установить неослабляемые правила классификации и валидности наблюдений. Иначе Charter способен сделать safety assertion формально истинным за счёт узкой области наблюдения или мягкой категории дефекта.

## 3. Findings

### SEC-01 — Critical: severity rubric может пропустить небезопасный дефект

**Где:** H-3 (§11.2), FR-101, SM-2, SM-13, NFR-17, OQ-4.  
**Условие:** воспроизводимая уязвимость приводит к опасному пути, но получает уровень `high`, `medium` либо иной неблокирующий уровень, потому что только `critical` явно запрещает положительный выбор кандидата.  
**Последствие:** Gate 1 может выбрать кандидат с известным путём несанкционированного эффекта или disclosure.  
**Ограниченная правка требования:** определить неослабляемый **Safety Blocking Class**: любой воспроизводимый unauthorized Protected Disclosure/Consequential Action, расширение Authority, обход обязательного Consent/Approval/Obligation, ложное понижение `unknown/partial`, повышение Evidence Assurance или неоднозначность, при которой две допустимые интерпретации расходятся между permit/non-permit либо по эффекту, блокирует кандидата независимо от выбранного severity label. Дополнительно требовать `0 unresolved findings at or above the preregistered blocking threshold`; Charter может уточнять rubric, но не выводить эти классы из hard gate.

### SEC-02 — Critical: отсутствует обязательная проверка Authority в точке domain commit

**Где:** FR-31, FR-34, FR-37, FR-39, FR-45, FR-48; VC-2–VC-4 и VC-12.  
**Условие:** Authority проверена при принятии или начале исполнения, после чего Resource разрешается через изменившийся alias/redirect/lookup, меняются security-critical параметры, истекает либо отзывается полномочие, и исполнитель достигает необратимого commit без новой связанной проверки.  
**Последствие:** классическая TOCTOU-ошибка позволяет выполнить уже не разрешённое действие или действие над другим Resource.  
**Ограниченная правка требования:** потребовать, чтобы для каждого необратимого или отдельно значимого effect boundary точный, уже разрешённый Decision Subject и актуальный Effective Authority проверялись **в точке commit либо атомарно связывались с ней**. Любое повторное разрешение alias, изменение Resource/parameters/policy epoch или невозможность доказать связь даёт non-permit до эффекта либо честный `unknown` после неоднозначного сбоя. Charter должен добавить interleavings `authorize → mutate/revoke/redirect → commit`.

### SEC-03 — Critical: shared limits небезопасны при параллелизме и partition

**Где:** FR-32, FR-36, FR-38, FR-47, FR-50; NFR-8, NFR-10, NFR-15; VC-6 и VC-12.  
**Условие:** две ветви, исполнители или partitions одновременно считают один остаток amount/count/rate/use limit доступным и по отдельности разрешают действия, совместно превышающие полномочие.  
**Последствие:** double-spend полномочия, превышение бюджета либо многократное использование one-shot authority при локально «правильных» решениях.  
**Ограниченная правка требования:** явно потребовать: общий непартиционированный лимит может разрешать effect только при доказуемом single-consumption/shared-state enforcement, безопасном предварительном разбиении полномочия на непересекающиеся доли либо эквивалентном механизме применимого Binding/Profile. Если это невозможно при текущем partition/freshness state, результат обязан быть non-permit/unknown, а не optimistic permit. Не выбирать конкретную координационную технологию. Добавить duplicate/reorder/crash/partition vectors с одновременным исчерпанием общего лимита и проверкой фактической суммы эффектов.

### SEC-04 — Critical: неполный effect observer способен доказать ложный ноль

**Где:** FR-90, FR-101, SM-2, SM-5, §10.1, OQ-3; VC-2–VC-4 и VC-12.  
**Условие:** кандидат создаёт запрещённый побочный эффект через канал, который observer не измеряет, либо observer задерживается, падает или неверно сопоставляет Operation Identity; отчёт всё равно содержит «0 наблюдаемых effects».  
**Последствие:** false negative делает hard safety gate нефальсифицируемым и пропускает небезопасный кандидат.  
**Ограниченная правка требования:** для каждого consequential scenario Charter обязан перечислить **полную модель возможных effect channels и commit points**, независимые источники наблюдения, detection window и correlation rules. Перед зачётным запуском observer калибруется seed-инъекциями известных разрешённых и запрещённых эффектов. Слепота, потеря данных, неоднозначная корреляция или отказ observer инвалидируют cell/run; отсутствие наблюдения не считается отсутствием эффекта.

### SEC-05 — High: lifecycle security identity не задан как самостоятельный проверяемый объект

**Где:** FR-1–FR-4, FR-7, FR-31, FR-40, FR-71; NFR-3, NFR-5; VC-7, VC-8, VC-12.  
**Условие:** тот же контекстный Participant identifier после rotation, reassignment, recovery или смены controller/credential ошибочно наследует прежние identity/Authority assertions.  
**Последствие:** новый controller либо скомпрометированный старый credential продолжает действовать как прежний actor.  
**Ограниченная правка требования:** потребовать от применимого Identity/Security Binding представлять identity assertion как scoped claim с issuer/provenance, subject, audience/context, validity/freshness/status, assurance и различимым security epoch/controller state. Rotation, reassignment, recovery и смена Binding не переносят Authority автоматически. Добавить stale credential, identifier reassignment, key/controller rotation и rollback cases; конкретный identity формат не выбирать.

### SEC-06 — High: negotiation допускает split-view одного Interaction Context

**Где:** FR-14–FR-17, FR-64, FR-71, NFR-3; VC-1, VC-10, VC-12.  
**Условие:** посредник даёт разным участникам разные offers/outcomes либо смешивает предложение одной сессии с подтверждением другой; каждый участник локально видит допустимый Negotiation Outcome.  
**Последствие:** стороны защищают и интерпретируют одну Операцию по разным версиям, assurance floor или mandatory semantics.  
**Ограниченная правка требования:** все security-relevant стороны зависимой ветви должны вывести или подтвердить один и тот же **семантически однозначный Negotiation Outcome**, связанный с соответствующими offers/constraints, Interaction Context и Operation. Несовпадение, mix-and-match или неполная end-to-end binding блокирует зависимое раскрытие/effect. Не требовать конкретного byte digest; добавить split-view, transcript splicing, participant add/remove и MITM truncation vectors.

### SEC-07 — High: Obligations не имеют обязательной enforcement/lifecycle семантики

**Где:** определение Obligation (§3), FR-30–FR-32, FR-35–FR-39, FR-45–FR-50; VC-2–VC-6, VC-12.  
**Условие:** решение содержит обязательство «до», «во время» или «после» действия, но реализации расходятся, когда оно считается выполненным, кто это подтверждает и что происходит при его невозможности либо нарушении.  
**Последствие:** действие выполняется до обязательной защиты, продолжает выполняться после её нарушения либо ошибочно считается авторизованным задним числом.  
**Ограниченная правка требования:** Obligation должна задавать enforcement phase/boundary, responsible/enforcing party, evidence/assurance needed for satisfaction, expiry/failure state и effect on future steps. Невыполненная prerequisite блокирует commit; нарушение ongoing obligation блокирует последующие защищённые шаги; post-effect obligation не ретроактивно авторизует прошлый эффект и отражается отдельным state/evidence/compensation path. Добавить omitted, falsely satisfied, late, failed и conflicting-obligation vectors.

### SEC-08 — High: `partial` недостаточно для безопасного восстановления составного эффекта

**Где:** FR-46, FR-48–FR-55; VC-2–VC-7 и VC-12.  
**Условие:** Операция меняет несколько Resources либо проходит несколько commit points, но результат сообщает только `partial` без нормативного описания затронутых и неизвестных effect dimensions.  
**Последствие:** retry или Compensation повторяет уже возникший подэффект либо оставляет другой эффект необработанным.  
**Ограниченная правка требования:** применимый Profile должен определять минимальные effect dimensions/commit boundaries, необходимые для безопасного решения о retry, continuation и Compensation. Outcome Claim обязан сохранять для каждой существенной части `none/full/partial/unknown` либо явно сообщать, что более точное восстановление невозможно; агрегирование не может скрывать неизвестный подэффект. Добавить partially committed multi-resource vectors.

### SEC-09 — High: privacy hard gate не ограничивает scope metadata/linkability claims

**Где:** FR-4, FR-12, FR-41, FR-52, FR-55, FR-72, FR-90; NFR-4, NFR-20, NFR-23, NFR-24; SM-2, SM-14; VC-12, VC-13.  
**Условие:** содержимое минимизировано, но размер, частота, timing, error distinction, persistent correlation либо branch/bridge metadata раскрывают наличие Capability, identity relation, policy state или частное действие.  
**Последствие:** система проходит content-oriented privacy tests, сохраняя практический enumeration или cross-context tracking channel.  
**Ограниченная правка требования:** каждый Profile/Binding/Conformance Claim должен явно перечислять защищаемые privacy properties и допустимые metadata leakage bounds в пределах threat model. Privacy corpus включает response-size/timing/error-oracle, identifier linkability, branch inference и bridge correlation там, где соответствующее свойство заявлено. Незаявленная защита не предполагается, но claim нельзя формулировать шире проверенной области.

### SEC-10 — High: изоляция эксперимента не закрывает ошибочный внешний egress

**Где:** §6.4, §10.1, NFR-25; VC-2–VC-4, VC-8, VC-11, VC-12.  
**Условие:** тестовая реализация при ошибке discovery/redirect/DNS/bridge configuration обращается к реальному endpoint или покидает стенд без production credential.  
**Последствие:** непреднамеренное внешнее действие, сканирование, раскрытие тестовых данных или влияние на третью сторону несмотря на формальный запрет production secrets.  
**Ограниченная правка требования:** зачётный environment должен иметь deny-by-default egress либо эквивалентную доказуемую изоляцию, allowlisted synthetic endpoints/identities, resource quotas, safe-stop/kill capability, teardown/cleanup и проверку redirect/DNS/bridge escape. Любое внешнее соединение вне разрешённой модели инвалидирует запуск и регистрируется как safety incident.

### SEC-11 — High: stale security state проверяется, но rollback state не выделен

**Где:** FR-18, FR-37–FR-40, FR-47, FR-51, FR-65, FR-74, FR-81, FR-89; NFR-8, NFR-10, NFR-29; VC-7, VC-8, VC-10, VC-12.  
**Условие:** после crash/restart или восстановления snapshot система возвращается к состоянию до revocation, consumption, security withdrawal или provider change, но локальная freshness проверка считает snapshot последовательным.  
**Последствие:** воскрешение Authority, повторный расход use limit, принятие withdrawn semantics или старого trust state.  
**Ограниченная правка требования:** security-critical state должен иметь различимый monotonic/epoch lineage или эквивалентное основание для обнаружения rollback в пределах заявленного trust model. Если актуальность относительно внешнего authoritative state не устанавливается, защищённый шаг получает non-permit/unknown. Добавить snapshot rollback, stale replica promotion и provider rollback tests; механизм хранения не фиксировать.

### SEC-12 — High: «bounded» resource limit может быть формально конечным, но небезопасным

**Где:** FR-23, FR-29, FR-74, FR-80, FR-90; NFR-9, NFR-12, NFR-15; VC-9, VC-12; §11.7.  
**Условие:** parser/decompressor/signature validation/delegation traversal/bridge chain/error reporting имеют огромный, хотя конечный limit, либо pre-auth workload одного adversary вытесняет корректные взаимодействия.  
**Последствие:** conformant на бумаге реализация подвержена CPU, memory, storage или network amplification DoS.  
**Ограниченная правка требования:** Charter должен задать измеримые pre-auth и post-auth work/amplification budgets, per-context и aggregate limits, cancellation/deadline behavior и минимальную progress/isolation гарантию для корректных flows. Resource corpus должен охватывать oversized/fragmented/compressed payload, nested/branching claims, multi-signature/credential lists, diagnostic amplification, retry storms и shared-provider exhaustion. Finite-but-impractical bound не проходит preregistered threshold.

### SEC-13 — Medium: не всякое bridge-преобразование обязано иметь accountable actor

**Где:** FR-72, FR-75, FR-79, FR-80; VC-11.  
**Условие:** bridge представлен как «преобразование», а не Участник, и выполняет security-critical interpretation внутри доверенной среды без собственной attributable identity/version/assurance.  
**Последствие:** невозможно однозначно установить, кто изменил смысл, завершил end-to-end protection или создал новое assertion.  
**Ограниченная правка требования:** любое не-прозрачное security-critical преобразование должно иметь наблюдаемую implementation identity/version, operator/trust boundary, provenance и собственные assertions; оно не обязано быть полноценным network Participant, но не может быть анонимным в Evidence/trace. Transparent claim допускается только при доказанном отсутствии изменения защищаемого смысла.

### SEC-14 — Medium: oracle и observer сами не входят в conformance trust analysis

**Где:** FR-83–FR-94, FR-107–FR-108, §11.5–§11.6.  
**Условие:** model oracle, expected outcome generator или effect observer содержит ту же ошибочную интерпретацию, что и кандидат, либо происходит из общей semantic codebase.  
**Последствие:** две реализации и Suite согласованно подтверждают небезопасную норму или пропускают эффект.  
**Ограниченная правка требования:** genealogy и независимость должны охватывать security-critical oracles/observers, а не только кандидатов. Для hard gates необходимы review их моделей, mutation/seed calibration и хотя бы один независимый способ перекрёстной проверки критических outcomes. Общая oracle codebase раскрывается как limitation и не доказывает независимую семантику.

### SEC-15 — Medium: inventory dependencies не связывает evidence с фактически исполненным артефактом

**Где:** FR-92, FR-107, NFR-26, NFR-29, §10.2.  
**Условие:** source/dependency versions перечислены, но benchmark/conformance запуск использует иной build, изменённый binary/container либо подменённый fixture.  
**Последствие:** Gate 1 evidence невоспроизводим или относится не к опубликованному кандидату.  
**Ограниченная правка требования:** evidence package должен связывать каждый результат с точными source, dependency lock, build configuration, executable/runtime artifact и Suite/fixture identities через tamper-evident идентификаторы либо эквивалентную проверяемую provenance chain. Не обязательно требовать конкретную подпись или полностью reproducible build, но независимый rerun должен обнаруживать несоответствие артефакта.

### SEC-16 — Medium: mid-stream expiry/revocation не выделены в обязательный сценарий

**Где:** FR-22–FR-24, FR-37, FR-39, FR-51; NFR-8; VC-7, VC-9, VC-12.  
**Условие:** долгоживущая Subscription/Stream продолжает доставлять Protected Disclosure после expiry/revocation/identity epoch change либо смешивает элементы до и после новой policy boundary.  
**Последствие:** утечка данных или ложное наследование Authority через уже установленный канал.  
**Ограниченная правка требования:** Profile должен определить reauthorization/freshness boundaries для длительных защищённых delivery flows; элементы до и после смены security epoch остаются различимыми. Добавить expiry/revocation/provider-change во время Stream, buffered old elements после reconnect и concurrent unsubscribe/delivery.

## 4. Обязательные правки до принятия reviewer gate

Для закрытия security reviewer gate достаточно внести требования SEC-01–SEC-04 в PRD. Они являются блокирующими, потому что ограничивают саму возможность считать hard gate пройденным. SEC-05–SEC-12 следует либо внести в PRD сейчас, либо преобразовать в обязательные, не удаляемые пункты detailed Validation Charter с явной ссылкой из PRD. SEC-13–SEC-16 допустимо закрыть в Charter, если они получают testable disposition и не исключаются без security rationale.

Минимальный критерий повторного review:

- Safety Blocking Class не зависит от произвольного severity label;
- authorization-to-commit TOCTOU и shared-limit partition имеют обязательный fail-closed исход;
- effect observer проходит calibration/completeness gate;
- каждая из остальных 12 угроз связана с требованием, VC vector либо обоснованным `not applicable`;
- при незакрытом critical finding PRD не получает финальный security acceptance.

## 5. Редакционная и структурная заметка

Документ существует, чтобы независимые реализаторы и владельцы Gate 1 могли построить и опровергнуть техническую гипотезу без закрытого знания. Для этой цели выбранная pyramid/reference-гибридная структура работает, но security-critical правила распределены между F4/F5/F7/F8/F9, NFR, Metrics, Guardrails и Charter. Не сокращать эту семантику. Перед финализацией полезно добавить компактный индекс `Safety invariant → FR/NFR → VC → hard-gate evidence`; он снизит риск, что Charter пропустит одно из обязательств. Языковое смешение русского и английского последовательно и не создаёт самостоятельной уязвимости, но нормативные термины должны иметь одну каноническую форму в последующей спецификации.

## 6. Итоговый security decision

**Не отклонять концепцию PRD. Не разрешать финальный security pass в текущей редакции.** После внесения четырёх Critical и привязки остальных findings к обязательным Charter vectors документ может перейти к повторной узкой проверке. Ни одно finding не требует преждевременно выбирать Rust, transport, token, PKI, canonical encoding, storage или централизованный coordinator.

## Re-review after fixes — 2026-09-13

### Итоговый вердикт

**Pass для финализации PRD; экспериментальный код по-прежнему запрещён до freeze подробного Validation Charter.** Все четыре прежних Critical и все восемь High закрыты на уровне обязательных требований PRD. Один прежний Medium закрыт частично и должен быть явно завершён в Charter; он не блокирует финализацию PRD, потому что §11.7 уже запрещает начать эксперимент с незаполненными обязательными параметрами.

| Категория после повторной проверки | Critical | High | Medium |
| --- | ---: | ---: | ---: |
| Неразрешённые блокеры PRD | **0** | **0** | **0** |
| Частично закрыто; обязательно завершить в Charter | 0 | 0 | **1** |

### Карта SEC-01–SEC-16

| Finding | Статус | Проверенное закрытие | Что остаётся для подробного Charter |
| --- | --- | --- | --- |
| **SEC-01** | **Resolved** | §3 определяет неизменяемый Safety Blocking Class; FR-101 запрещает его понижение severity rubric; SM-2 и H-3 требуют ноль таких findings; §11.7 замораживает класс до кода | Только конкретная более строгая rubric и порядок triage; базовый blocking class менять нельзя |
| **SEC-02** | **Resolved** | FR-31 связывает окончательный Resource/parameters с Decision Subject; FR-37 требует актуальный Effective Authority в каждой значимой domain-commit точке и повторную проверку при alias/parameter/epoch/revocation drift; VC-2 и §11.7 требуют TOCTOU interleavings | Перечислить commit boundaries каждого validation profile и expected outcomes |
| **SEC-03** | **Resolved** | FR-38 допускает shared effect только при доказуемом single-consumption, заранее непересекающихся квотах либо эквивалентном механизме; иначе `non-permit/unknown`; VC-6/VC-12 и §11.7 требуют concurrent partition vectors | Выбрать механизм отдельно для каждого Candidate/Profile и задать fault schedule |
| **SEC-04** | **Resolved** | FR-101 и SM-2 объявляют невалидный observer не нулём, а invalid result; §10.1 требует model всех effect/egress channels, independent sources, detection window, correlation, canary/positive-control и fault calibration; §11.7 сохраняет это обязательным до кода | Зафиксировать конкретные observers, каналы, окна и калибровочные инъекции по каждому сценарию |
| **SEC-05** | **Resolved** | FR-4 требует scoped identity assertion с issuer/provenance, audience/context, freshness/status, assurance и security epoch/controller state; rotation/reassignment/recovery/rollback/Binding change не переносят Authority; NFR-8, VC-7 и §11.7 покрывают stale/rollback cases | Определить Binding-specific assertions, epochs и vectors без выбора глобальной identity-системы |
| **SEC-06** | **Resolved** | FR-14 вводит Bootstrap Contract; FR-17 связывает полный transcript, Outcome, Interaction Epoch и dependency closure и блокирует split-view; VC-1/VC-6 и §11.7 требуют соответствующие проверки | Определить representation-independent equality/refinement oracle и конкретные transcript-splicing cases |
| **SEC-07** | **Resolved** | FR-30 требует для Obligation enforcement phase/boundary, responsible/enforcing party, satisfaction evidence, expiry/failure и влияние на следующие шаги; prerequisite блокирует commit; §11.7 требует obligation-lifecycle vectors | Развернуть `before/during/after`, false/late/conflicting satisfaction и failure paths в corpus |
| **SEC-08** | **Resolved** | FR-46 требует Profile-specific effect dimensions/commit boundaries и отдельный `none/full/partial/unknown` для существенных частей; агрегат не скрывает unknown; §11.7 требует multi-resource vectors | Зафиксировать effect decomposition каждого validation profile и recovery/compensation oracle |
| **SEC-09** | **Resolved** | NFR-4 ограничивает privacy claim metadata, timing/error oracles, linkability и branch/bridge correlation; VC-12 и §11.7 требуют bounds и leakage vectors; claim не может быть шире проверенного | Задать измеримые bounds только для реально заявленных privacy properties каждого Profile/Binding |
| **SEC-10** | **Resolved at PRD contract** | VC-12 включает external-egress escape; §11.7 делает deny-by-default egress либо доказуемый эквивалент, allowlist, synthetic identities, redirect/DNS/bridge escape, quotas, safe-stop и cleanup обязательными до кода | Реализовать и доказать конкретную sandbox isolation; это корректно относится к Charter/environment, а не к PRD architecture |
| **SEC-11** | **Resolved** | FR-4 запрещает перенос Authority через rollback identity state; FR-47 задаёт replay window после retention loss; NFR-8 запрещает принимать старый security/policy/identity epoch и требует `non-permit/unknown`, если monotonic freshness не доказана; VC-7 и §11.7 требуют rollback vectors | Выбрать candidate-specific freshness/epoch basis и snapshot/stale-replica cases |
| **SEC-12** | **Resolved at PRD contract** | NFR-9 требует количественные достижимые limits, early size/depth checks и safe exhaustion outcome; NFR-15 учитывает fan-out/shared bottlenecks; VC-9/VC-12 и §11.7 требуют resource cases и safe ceilings | Заморозить pre-/post-trust budgets, aggregate concurrency/amplification ceilings и допустимый progress в workloads |
| **SEC-13** | **Resolved** | FR-1 различает внешнюю систему и протокольно наблюдаемого Участника; FR-79 связывает security/outcome-relevant transformation с accountable actor/implementation, mapping version и Authority и запрещает анонимно наследовать source assurance | Назначить exact attribution fields и bridge-path vectors в Mapping/Binding, не меняя Core преждевременно |
| **SEC-14** | **Partially resolved — Charter mandatory** | NFR-22 включает oracle/observer/harness в trust/fault analysis и требует provenance/calibration; Gate 1B требует candidate-neutral oracles; Gate 1D — independent/fresh review; §11.7 замораживает oracle manifests, digests, calibration и change rules | Charter должен явно потребовать genealogy/independent derivation либо независимый cross-check security-blocking expected outcomes. Общий oracle code не должен сам доказывать независимость. При отсутствии такого cross-check соответствующий hard-gate cell остаётся `invalid/inconclusive` |
| **SEC-15** | **Resolved** | NFR-22 и NFR-26 связывают evidence с content identity, фактической dependency closure, build provenance, locks/SBOM и реально исполненным immutable artifact; §11.7 требует immutable manifests и append-only raw index | Выбрать конкретные идентификаторы/build-capture procedures и проверить независимый rerun |
| **SEC-16** | **Resolved** | FR-37 требует recheck на значимых границах длительного interaction; VC-9 явно включает mid-stream authority expiry/revocation; §11.7 требует identity/security epoch и mid-stream vectors | Определить delivery/re-authorization boundaries, buffered-old-item и unsubscribe/delivery schedules |

### Повторная проверка возможного ложного Gate 1 pass

В обновлённом PRD небезопасная неоднозначность из прежних SEC-01–SEC-16 больше не может легально получить положительный Gate 1 verdict:

- Safety Blocking Class не зависит от выбранного severity label;
- невалидные observer/oracle/harness данные дают `invalid/inconclusive`, а не pass;
- authority-to-commit, shared-limit partition, split-view и stale-state rollback имеют fail-closed правила;
- `unknown/partial`, privacy scope и Evidence Assurance нельзя молча повысить;
- незаполненный параметр §11.7 означает No Start;
- неполный evidence означает `Gate Closed / No Decision`, а не `stop` или положительный выбор кандидата.

Формулировка Safety Blocking Class использует слово «воспроизводимый», но регрессии здесь не возникло: FR-92 запрещает rerun-until-pass для nondeterministic outcome, SM-2 требует ноль фактически наблюдавшихся запрещённых эффектов, а правила observer uncertainty оставляют run invalid. Единичный необъяснённый safety event нельзя удалить из evidence либо превратить в pass.

### Истинно оставшиеся блокеры

**Перед финализацией PRD:** отсутствуют по области SEC-01–SEC-16.  
**Перед freeze подробного Validation Charter:** закрыть SEC-14 явным правилом независимости/cross-check security-critical oracle; заполнить все конкретные parameters §11.7, включая observers, commit points, resource ceilings, sandbox isolation, identity epochs и exact vectors.  
**Перед экспериментальным кодом:** Charter должен быть frozen; любой пропуск означает No Start согласно §11.7 и §11.8.

### Проверка регрессий

Новых Critical или High security-регрессий, внесённых исправлениями, не обнаружено. Добавленные Bootstrap Contract, Interaction Epoch, Safety Blocking Class и Gate 1A–1D не ослабили разделение identity/Authority/effect/evidence и не зафиксировали преждевременный cryptographic, transport, storage либо coordination mechanism.
