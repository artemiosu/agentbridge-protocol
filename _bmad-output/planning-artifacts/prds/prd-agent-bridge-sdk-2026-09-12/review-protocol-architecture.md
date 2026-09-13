# Независимая проверка PRD: архитектура протокола и interoperability

**Объект проверки:** `prd.md`  
**Роль проверяющего:** независимый Internet protocol architect / interoperability reviewer  
**Дата:** 2026-09-13  
**Назначение документа:** помочь владельцу PRD решить, достаточно ли требований для подготовки подробного Validation Charter, не принимая преждевременно production-архитектуру.

## Вердикт

**Условно принять после ограниченных исправлений.** PRD задаёт необычно сильный validation-first каркас: он сохраняет гипотезу самостоятельного Native Core, не превращает существующие agent-протоколы в обязательные зависимости, отделяет доменные профили и заменяемую инфраструктуру, требует независимых реализаций и допускает результаты `profile`, `upstream` и `stop` наравне с `native`.

Однако документ пока не должен переходить непосредственно к замораживанию Validation Charter. Один конфликт распределённых гарантий является критическим, а семь архитектурных неоднозначностей способны либо разрастить Candidate N в полный стек, либо сместить сравнение с Candidate C, либо породить несовместимые «conformant» реализации. Все предлагаемые ниже исправления ограничены уровнем требований и Charter: они не выбирают wire format, transport, язык, SDK или production-компоненты.

### Количество findings

| Уровень | Количество |
| --- | ---: |
| Критический | 1 |
| Высокий | 7 |
| Средний | 5 |
| Низкий | 1 |
| **Всего** | **14** |

## Что следует сохранить

- §1.3, FR-68 и NFR-7 дают правильное определение самодостаточности: отсутствие обязательного чужого agent runtime не означает переизобретение transport, cryptography, identity, policy или systems of record.
- FR-60, FR-62–FR-67 и SM-C2 создают хорошую защиту от универсальной онтологии и бесконтрольного роста Core.
- FR-75–FR-81 чётко отделяют Compatibility Bridge от Native Path и запрещают скрывать semantic loss, provenance и downgrade.
- FR-82–FR-95 делают публичную норму, независимую интерпретацию и ограниченный conformance claim важнее Reference Implementation.
- FR-96–FR-110 правильно формулируют Gate 1 как проверку технической роли, а не как заранее организованную победу Native Candidate.

## Findings и ограниченные исправления

### PA-01 — Строгий общий лимит в разделённых ветвях несовместим с отсутствием обязательной координации

- **Уровень:** критический
- **Локация:** FR-25; FR-32; FR-38; NFR-10; NFR-15
- **Условие:** параллельные или разделённые partition ветви должны совместно не превысить общий числовой предел полномочия, но Core одновременно не требует координатора, общего журнала или синхронной доступности authority-компонента.
- **Последствие:** две корректные реализации могут обе локально разрешить действие и совместно превысить лимит; обещание становится невыполнимым в асинхронной распределённой модели.
- **Ограниченное исправление:** добавить инвариант: строгий общий лимит при конкурентном использовании гарантируется только при заранее непересекающемся разбиении полномочия/бюджета либо через явно согласованный механизм сериализации или координации. Если ни одно основание недоступно, параллельное использование должно давать `non-permit`/`unknown`, а не локальное разрешение. Добавить в VC-5/VC-6 partition-сценарий «две ветви исчерпывают один лимит».

### PA-02 — Отсутствует явное распределение FR по Thin Waist, Profile, Binding и системе доказательств

- **Уровень:** высокий
- **Локация:** §2.1–§2.3; F1–F8; FR-98; H-2; OQ-2
- **Условие:** F1–F5 названы кандидатным ядром, а FR-98 требует Candidate N реализовать общие инварианты F1–F8. При этом F3 включает request/response, subscriptions, streams, long-running и multi-party, F4 — богатую authority-модель, F5 — lifecycle/evidence, а F8 относится к доказательству, не к runtime Core.
- **Последствие:** Charter может принять весь каталог желаемых свойств за обязательный wire/runtime waist; Candidate N станет толстым протоколом, а сравнение будет проверять объём реализации вместо минимального irreducible gap.
- **Ограниченное исправление:** до OQ-2 обязать создать **Requirement Allocation Matrix** для каждого FR: `universal Core invariant`, `base/interaction profile`, `domain profile`, `extension`, `binding contract`, `conformance-only` или `experiment-only`. Для любого элемента Core потребовать removal test: его удаление должно разрушать общий safety/interoperability-инвариант минимум в двух несвязанных доменах и трёх topology classes. F8 не должен становиться runtime-семантикой Candidate N.

### PA-03 — Negotiation пытается согласовать собственный Binding без определённой bootstrap-границы

- **Уровень:** высокий
- **Локация:** FR-14–FR-17; FR-64; FR-68–FR-73; VC-1
- **Условие:** стороны должны согласовать версии, Profiles, Extensions, Bindings и assurance до зависимого обмена, но им уже нужны общие framing/encoding, защита freshness и anti-downgrade, чтобы одинаково интерпретировать и защитить само согласование.
- **Последствие:** возможны несовместимые pre-negotiation defaults, downgrade до установления защищённого транскрипта или скрытый обязательный bootstrap runtime.
- **Ограниченное исправление:** потребовать от каждого Native Binding определить минимальный bootstrap contract: как стороны получают начальную интерпретацию, какие элементы не подлежат самосогласованию, как transcript/offer set связывается с итоговым Negotiation Outcome и как обнаруживаются downgrade и split view. Bootstrap может быть in-band или out-of-band, но всегда является явной частью dependency и conformance claim.

### PA-04 — Не определена граница между общей безопасностью Core и доменной проверкой semantic equality/refinement

- **Уровень:** высокий
- **Локация:** FR-10; FR-31–FR-32; FR-42; FR-57; FR-62–FR-63; NFR-3
- **Условие:** Core должен проверять точный Decision Subject, равенство security-critical смысла и монотонное сужение ограничений между разными представлениями, но доменная семантика намеренно находится в Profiles и не должна становиться универсальной онтологией.
- **Последствие:** либо Core вынужден понимать цены, количества, временные окна и произвольные доменные предикаты, либо реализации по-разному решают, является ли делегирование подмножеством и эквивалентны ли два предмета решения.
- **Ограниченное исправление:** закрепить разделение ответственности: Core проверяет идентичность/неизменность выбранных нормативных артефактов, provenance, criticality и fail-closed результат; конкретный Profile обязан задать ограниченную, детерминированную и независимо тестируемую семантику equality/refinement для своих security-critical значений. При отсутствии такого правила преобразование, делегирование или cross-representation comparison запрещаются. Это не требует выбора schema language.

### PA-05 — `Binding` объединяет несколько разных слоёв без правил композиции их свойств

- **Уровень:** высокий
- **Локация:** определение Binding в §3; FR-69–FR-74; F7 validation gate; FR-91; H-4/H-7
- **Условие:** одним понятием охвачены transport, encoding, identity, trust, policy, audit и security mechanisms. PRD требует их заменяемости, но не требует объявлять, какие свойства каждый Binding предоставляет, потребляет, усиливает, завершает или делает недействительными при совместном использовании.
- **Последствие:** два отдельно conformant Binding могут образовать небезопасный stack; provider replacement может изменить identity namespace, policy truth или assurance, хотя transport semantics формально сохранились.
- **Ограниченное исправление:** добавить к Binding Contract типизированные `provides/requires/terminates` свойства, порядок/зависимости композиции, результирующие Trust Boundaries и правило вычисления итогового assurance. Заменяемость проверять относительно явно выбранного property contract и сценария, а не как глобальную взаимозаменяемость. Включить non-commutative binding stack и combined-failure vectors в VC-8/VC-10.

### PA-06 — Симметрия Independent Implementations для Candidate C определена неоднозначно

- **Уровень:** высокий
- **Локация:** FR-85; FR-97; FR-99; FR-102; правила паритета §11.3
- **Условие:** для каждого кандидата нужны две реализации без общей protocol-semantic codebase, но Candidate C должен добросовестно использовать зрелые реализации существующих стандартов. Неясно, считается ли общий upstream SDK запрещённым semantic code или допустимой нижележащей библиотекой.
- **Последствие:** Challenger либо искусственно заставят заново реализовать несколько стандартов, либо две одинаковые оболочки над одним composition engine ошибочно засчитают как независимую интерпретацию.
- **Ограниченное исправление:** определить единицу независимости симметрично: для Candidate N независимо реализуется AgentBridge contract; для Candidate C — замороженный composition profile и glue semantics. Одинаковые зрелые upstream libraries допустимы и раскрываются как общая genealogy, но не доказывают независимость composition semantics. Отдельно повторить ключевые vectors с альтернативной upstream implementation там, где её поведение влияет на вывод.

### PA-07 — Multi-party Negotiation не защищён от split view и изменения состава

- **Уровень:** высокий
- **Локация:** FR-14; FR-17; FR-25; FR-36; FR-50; FR-61; FR-64; VC-6/VC-7
- **Условие:** ветви могут иметь собственные Negotiation Outcome и работать без центрального координатора, но не задано, как агрегатор и зависимые стороны обнаруживают разные представления состава участников, версий, профилей или quorum после join/leave/reconnect.
- **Последствие:** посредник способен показать разным ветвям разные обязательные наборы; aggregate success или multi-party approval формируется из несовместимых epoch/context views.
- **Ограниченное исправление:** потребовать branch-local outcome с явной версией/epoch и криптографически либо эквивалентно проверяемой привязкой к exact dependency closure; aggregate claim обязан перечислять использованные branch outcomes. Изменение состава создаёт новый epoch/контекст и не действует задним числом. Добавить equivocation, concurrent join/leave и stale aggregate vectors.

### PA-08 — Незнакомая «необязательная» семантика требует невозможного доказательства безопасности

- **Уровень:** высокий
- **Локация:** FR-15; FR-58; FR-60; FR-88; NFR-19
- **Условие:** реализация может проигнорировать неизвестный optional element только при доказанном отсутствии влияния на обязательную семантику. Но не понимающая Extension сторона не может вывести свойства неизвестного смысла из заявления отправителя.
- **Последствие:** разные реализации либо отвергнут все расширения, либо доверятся attacker-controlled criticality и пропустят скрытый security-critical смысл.
- **Ограниченное исправление:** criticality и processing rule должны определяться известным нормативным extension point/Profile, а не произвольной маркировкой сообщения. Неизвестный элемент разрешено игнорировать только в области, где известная базовая норма заранее гарантирует observational irrelevance для выбранного outcome; во всех остальных случаях — reject/non-permit. Отдельно определить правила relay preservation для неизвестных элементов.

### PA-09 — Не каждый заменяемый инфраструктурный компонент должен становиться AgentBridge Participant

- **Уровень:** средний
- **Локация:** FR-1; FR-7; topology `participant↔replaceable infrastructure`; FR-72–FR-74; VC-8
- **Условие:** общая Participant model включает discovery/identity/trust/policy/audit components, хотя §1.3 и F7 трактуют их как внешние заменяемые механизмы через Bindings.
- **Последствие:** Charter может потребовать AgentBridge endpoint и роли от CA, policy engine, audit store или иной системы, тем самым переизобретая их интерфейсы и создавая обязательные adapters.
- **Ограниченное исправление:** уточнить: инфраструктура является Participant только когда сама участвует в AgentBridge interaction и делает протокольно наблюдаемое assertion/decision. Иначе Binding обязан лишь представить её output, trust boundary и provenance; внешняя система не обязана реализовывать AgentBridge.

### PA-10 — «100% одинаковых outcomes» конфликтует с явно разрешённой вариативностью

- **Уровень:** средний
- **Локация:** FR-86; NFR-11; SM-3; SM-7; H-5
- **Условие:** NFR-11 допускает ограниченную nondeterminism и implementation choice, тогда как SM-3 требует 100% одинаковых нормативных outcomes.
- **Последствие:** допустимые различия могут дать ложный fail; либо oracle сузит протокол до поведения Reference Implementation.
- **Ограниченное исправление:** определить agreement как принадлежность одной нормативной equivalence class/allowed outcome set плюс полное совпадение safety-critical classification (`permit/non-permit/unknown`, Effect State floor, assurance floor). Literal equality требовать только там, где норма задаёт единственный результат; различие байтов, порядка безопасно необязательных элементов и локальной диагностики не считать semantic divergence.

### PA-11 — Историческая интерпретация не гарантирована без замороженной транзитивной dependency closure

- **Уровень:** средний
- **Локация:** FR-56; FR-61; FR-64–FR-66; FR-81; FR-92
- **Условие:** взаимодействие должно оставаться интерпретируемым по прежней норме, однако указание верхнеуровневых версий не гарантирует неизменность всех Profiles, Extensions, Bindings, mappings, errata и зависимостей.
- **Последствие:** одинаковая ссылка позднее разрешится в другое содержание или исчезнет; Evidence и historical conformance потеряют однозначный смысл.
- **Ограниченное исправление:** Negotiation Outcome и evidence package должны однозначно фиксировать полную транзитивную closure нормативных артефактов и их неизменность/получаемость эквивалентным content binding. Механизм хранения или адресации оставить Architecture, но тестировать исчезновение origin, изменённый артефакт и dependency substitution.

### PA-12 — Bounded retention оставляет незащищённым replay после удаления deduplication state

- **Уровень:** средний
- **Локация:** FR-38; FR-43; FR-47; FR-55; NFR-9; VC-7/VC-12
- **Условие:** реализация обязана ограничивать retention, но PRD не задаёт безопасное поведение при повторном появлении старой Operation Identity после удаления deduplication/authority state.
- **Последствие:** старое сообщение может быть принято как новая операция либо новая операция с повторно использованным ID ошибочно приклеится к прежней истории.
- **Ограниченное исправление:** потребовать явную acceptance/replay window semantics. После утраты состояния операция не может молча получить fresh semantics: реализация возвращает `unknown/conflict/non-permit` либо использует согласованное непереиспользуемое пространство и проверяемое основание свежести. Добавить vectors после garbage collection, clock uncertainty и delayed bridge replay.

### PA-13 — Автоматическое получение test vectors из Semantic Mapping является преждевременным архитектурным ограничением

- **Уровень:** средний
- **Локация:** FR-76, проверяемое следствие 4
- **Условие:** любой bridge mapping обязан позволять автоматически получить test vectors, хотя PRD намеренно не выбирает формальный mapping language или executable model.
- **Последствие:** качественное, но неисполняемое нормативное mapping-описание окажется неконформным либо проект преждевременно выберет DSL/toolchain.
- **Ограниченное исправление:** заменить требование автоматичности на двустороннюю traceability: каждое mapping rule и каждый unmapped/security-critical case связан с позитивным/негативным vector и ожидаемым outcome. Машинная генерация остаётся предпочтительной экспериментальной техникой, но не обязательной архитектурой Gate 1.

### PA-14 — Ключевая карта архитектурных инвариантов скрыта внутри 24 323 слов

- **Уровень:** низкий
- **Локация:** структура документа в целом; особенно §2, §4, §8–§11
- **Условие:** thin-waist границы, hard gates, слои и decision outcomes повторяются во многих разделах, но отсутствует один короткий индекс `invariant → owning layer → validation scenario → decision impact`.
- **Последствие:** Charter authors могут пропустить конфликт или ошибочно отнести профильное требование к Core, несмотря на качественные локальные формулировки.
- **Ограниченное исправление:** добавить после §2 одностраничную traceability/allocation таблицу и ссылаться на неё из Charter. Не сокращать подробные FR и «Не входит» границы: для независимых реализаторов они полезнее, чем экономия объёма. Чистое увеличение — ориентировочно 200–300 слов; понимание важнее формального сокращения.

## Edge-case coverage, который следует добавить в Validation Charter

1. Две partition-ветви одновременно расходуют один общий authority limit.
2. Negotiation начинается до выбора Binding; посредник удаляет сильное bootstrap-предложение.
3. Multi-party участники получают разные version/profile closure; join/leave происходит во время approval.
4. Неизвестный optional element проходит через relay, который не умеет его сохранить.
5. Два отдельно conformant Binding меняют итоговое assurance при перестановке порядка.
6. Policy/identity provider replacement меняет наблюдаемую истину, но не Core interpretation.
7. Старое сообщение возвращается после удаления deduplication state и expiry replay window.
8. Нормативный артефакт или mapping по прежнему адресу изменён/недоступен.
9. Candidate C использует общий upstream SDK, но две независимые composition semantics.
10. Две реализации выбирают разные разрешённые optional outcomes при одинаковой safety classification.

## Минимальный пакет правок до freeze Validation Charter

1. Закрыть PA-01 нормативным ограничением общей квоты.
2. Добавить Requirement Allocation Matrix и removal test из PA-02.
3. Зафиксировать bootstrap boundary и transcript binding из PA-03.
4. Развести Core и Profile responsibility для equality/refinement по PA-04.
5. Определить composition property contract для Bindings по PA-05.
6. Уточнить единицу независимости Candidate C по PA-06.
7. Добавить multi-party epoch/split-view invariant по PA-07.
8. Исправить unknown-optional rule по PA-08.

Остальные findings допустимо закрыть в подробном Validation Charter, если PRD явно делает их обязательными Charter decisions и сохраняет правило `No Start` при незаполненном пункте.

## Итоговая оценка

После указанных ограниченных правок модель требований способна привести к минимальному role/direction-neutral thin waist, а не к универсальной онтологии или новому полному Internet stack. Наиболее ценное качество PRD — он рассматривает Native Core как опровержимую гипотезу и требует самодостаточности на semantic/runtime уровне, одновременно позволяя переиспользовать проверенные нижележащие механизмы через явные Bindings. Наиболее опасное место — попытка сделать слишком много распределённых и доменных гарантий непосредственной обязанностью Core до определения их allocation и условий выполнимости.

## Re-review after fixes

**Дата повторной проверки:** 2026-09-13  
**Результат:** 13 findings закрыты на уровне PRD; 1 низкий structural finding закрыт частично. Новых Critical/High архитектурных регрессий в изменённых требованиях не обнаружено. Фактическая реализация этих правил остаётся предметом frozen Validation Charter и Gate 1 evidence, а не считается доказанной текстом PRD.

| Finding | Статус | Краткое доказательство в текущем PRD |
| --- | --- | --- |
| **PA-01 — shared limits без координации** | **Resolved** | FR-38 теперь разрешает общий непартиционированный лимит только при `single-consumption/shared-state enforcement`, непересекающихся долях либо эквивалентном механизме; при partition/freshness uncertainty требуется `non-permit/unknown`. Риск включён в VC-6 и обязательные Charter parameters §11.7. |
| **PA-02 — нет allocation для Thin Waist** | **Resolved** | §2.5 вводит Requirement Allocation и Removal Test; глоссарий определяет оба понятия; FR-96 требует ноль неразмещённых FR/NFR до freeze; FR-98 допускает в Core всю и только доказанно необходимую общую семантику. F8 может быть размещён как `conformance/experiment`, а не runtime Core. |
| **PA-03 — bootstrap circularity** | **Resolved** | Глоссарий определяет Bootstrap Contract; FR-14 запрещает сообщению выбирать ещё не интерпретируемые bootstrap-свойства; FR-17 связывает outcome с полным transcript/bootstrap; FR-73 применяет bootstrap к Binding negotiation; VC-1 и §11.7 требуют отдельные vectors/rules. |
| **PA-04 — Core/Profile equality/refinement boundary** | **Resolved** | FR-62 теперь явно оставляет Core identity/provenance/criticality/fail-closed обязанности, а Domain Profile обязан предоставить ограниченную детерминированную equality/refinement semantics; отсутствие правила даёт запрет. Это также обязательный параметр §11.7. |
| **PA-05 — нет композиции Binding properties** | **Resolved** | FR-69 вводит типизированные `provides/requires/terminates`, composition order/dependencies, combined Trust Boundaries и resultant assurance. Non-commutative/combined-failure проверки включены в VC-8/VC-10 и §11.7. |
| **PA-06 — неясная независимость Candidate C** | **Resolved** | FR-102 определяет единицу независимости C как frozen composition profile/glue semantics, разрешает раскрытые shared upstream libraries и требует альтернативную upstream implementation для критичных зависимых vectors, когда возможно. §11.7 фиксирует contamination/genealogy policy. |
| **PA-07 — multi-party split view/membership** | **Resolved** | Глоссарий вводит Interaction Epoch; FR-17 связывает каждую ветвь с epoch/dependency closure и блокирует split view; VC-6 добавляет concurrent join/leave, split-view и stale aggregate checks; §11.7 требует эти правила до кода. |
| **PA-08 — невозможное доказательство unknown optional safety** | **Resolved** | FR-15 и FR-58 разрешают игнорирование только в известной нормативной extension point с заранее гарантированной observational irrelevance; attacker-controlled `optional` недостаточен, иначе применяется reject/non-permit. Правило повторено как No Start parameter §11.7. |
| **PA-09 — инфраструктура всегда Participant** | **Resolved** | FR-1 уточняет: внешний discovery/identity/trust/policy/audit component является Participant только при собственном AgentBridge Interaction или протокольно наблюдаемом assertion/decision; иначе Binding представляет output/provenance/Trust Boundary без требования AgentBridge implementation. |
| **PA-10 — literal outcome equality против вариативности** | **Resolved** | FR-86, SM-3/SM-7 и NFR-11 используют allowed outcome set/equivalence class и одинаковую safety-critical projection; буквальное равенство требуется лишь при единственном нормативном результате. |
| **PA-11 — нет immutable transitive artifact closure** | **Resolved** | FR-64 фиксирует полную транзитивную dependency closure через immutable content identity или эквивалентную связь; NFR-26 связывает evidence с фактически исполненным immutable artifact и dependency closure; §11.7 требует immutable manifests/digests/provenance. |
| **PA-12 — replay после bounded retention** | **Resolved** | FR-47 вводит acceptance/replay window и запрещает fresh semantics после удаления state без доказуемой freshness/non-reuse; VC-7 и §11.7 требуют replay-after-retention vectors. |
| **PA-13 — обязательный executable mapping DSL** | **Resolved** | FR-76 заменяет автоматическую генерацию на двустороннюю traceability mapping rules/unmapped cases к vectors; автоматическая генерация объявлена необязательной техникой. |
| **PA-14 — нет короткого invariant/allocation index** | **Partial** | §2.5 даёт короткую дисциплину Core/Thin Waist, а FR-96 и §11.7 делают полный Ledger/Allocation Matrix обязательными до Candidate design. Однако сам текущий PRD по-прежнему не содержит компактной таблицы `invariant → layer → scenario → decision impact`; это usability-риск низкого уровня, не blocker. |

### Проверка регрессий

- Исправления не выбрали конкретные wire format, transport, язык, SDK, cryptographic algorithm или production topology.
- Requirement Allocation не превращён в искусственный лимит размера: §2.5/FR-98 требуют полноценный Core в доказанном scope и одновременно Removal Test против разрастания.
- Bootstrap Contract не стал полномочием или обязательным чужим runtime: это минимальная граница безопасного negotiation, а FR-68/NFR-7 по-прежнему требуют независимый Native Path.
- Binding property contracts не обещают глобальную взаимозаменяемость провайдеров: несовместимая замена требует нового Negotiation Outcome, а результирующие assurance/Trust Boundaries проверяются явно.
- Независимость Candidate C уточнена без предоставления ему бесплатной скрытой composition semantics и без требования заново реализовать все upstream standards.

### Повторный вердикт

**Архитектурный reviewer gate пройден.** На уровне PRD прежние Critical/High findings закрыты; документ можно финализировать и использовать для подготовки подробного Validation Charter. Это разрешение не означает, что Native Core уже доказан или что можно начинать production Architecture: правила должны быть конкретизированы и frozen до экспериментального кода, а затем подтверждены Gate 1 evidence.
