# Независимая проверка методологии Gate 1

**Роль reviewer:** experimental methodology, benchmarking и falsification  
**Проверенный артефакт:** `prd.md`, статус draft, обновлён 2026-09-13  
**Область:** только валидность сравнительного эксперимента и функции решения `native/profile/upstream/stop`; это не архитектурное и не security-review  
**Решение:** **NEEDS REVISION — блокирует freeze подробного Validation Charter и начало Gate 1, но не отвергает продуктовую гипотезу**

## 1. Итоговый вердикт

PRD задаёт необычно сильную основу научной добросовестности: отрицательный исход признан успехом процесса (§8.1, SM-C1), hard gates нельзя компенсировать aggregate score (FR-96, FR-101, SM-2), версии и raw evidence должны сохраняться (FR-92, FR-107), а неполное evidence оставляет Gate закрытым (FR-109, §7.4). Эти положения стоит сохранить.

Однако в текущей редакции два добросовестных Gate Chairs могут получить разные результаты на одних данных. Основные причины: неоперациональное понятие единственного «сильнейшего» Challenger, пересекающиеся исходы FR-109, возможность выбрать Material Advantage из большого числа метрик после наблюдения результатов и совместное проектирование Native Candidate, требований и corpus. Поэтому слово «детерминированная» в FR-109 пока не подтверждается процедурой.

До начала кода нужно устранить все Critical и High findings. Большинство точных чисел и процедур допустимо вынести в подробный Validation Charter, но PRD должен явно обязать Charter закрыть соответствующий класс смещения.

## 2. Сводка находок

| Severity | Количество | Значение |
| --- | ---: | --- |
| **Critical** | **4** | Может изменить победителя или сделать исход логически неидентифицируемым |
| **High** | **8** | Существенно ослабляет causal/construct validity или воспроизводимость |
| **Medium** | **3** | Не обязательно меняет исход, но создаёт спорный либо хрупкий claim |
| **Всего** | **15** | — |

## 3. Blocking findings

### EM-01 — Critical: единственный Strongest Composition Challenger не определён как измеримый объект

**Где:** FR-97; §11.3 Candidate C; H-1; R-1; OQ-1.

**Проблема:** для многомерных критериев не обязательно существует один «сильнейший» вариант. Одна композиция может быть лучшей по safety и maturity, другая — по dependency surface, третья — по coverage. Выбор одного C после landscape review способен случайно или намеренно исключить неудобный baseline. Обратная крайность — «Frankenstein composition», которая заимствует лучший компонент для каждой ячейки, но не образует реально развёртываемую, совместимую систему.

**Ограниченное исправление (PRD + Charter):**

1. До Candidate N зафиксировать search/inclusion protocol, дату отсечения, обязательные источники, maturity/license/deployability критерии и semantic coverage matrix.
2. Разрешить не более трёх недоминируемых конфигураций `C1…Ck`: минимальную композицию, strongest-coverage композицию и практический ecosystem baseline, только если они действительно различаются.
3. Заморозить один Primary C по заранее заданному lexicographic правилу, но провести sensitivity check против остальных Pareto-неуступающих C.
4. Разрешить `native` только если H-1 остаётся верной против **каждого** жизнеспособного C; достаточно одного безопасно закрывающего требования C, чтобы H-1 считалась опровергнутой.
5. Требовать, чтобы каждый C был end-to-end deployable, внутренне совместим и license-compatible; нельзя собирать виртуального Challenger из взаимоисключающих режимов.

### EM-02 — Critical: FR-109 не задаёт взаимоисключающую decision lattice; H-1 частично циклична

**Где:** определение Irreducible Gap в §3; H-1; FR-97–FR-98; FR-109; SM-6; SM-8.

**Проблема:** Candidate C допускает новый normative profile и glue, а Irreducible Gap определяется через потребность в «новом нормативном объекте». Сам факт необходимости такого объекта может быть классифицирован и как аргумент за `native`, и как AgentBridge-profile поверх существующих стандартов. `profile` и `upstream` также могут одновременно быть технически возможны. `stop` смешивает отсутствие общей семантики, небезопасность Native и чрезмерную стоимость. Сейчас нет приоритета исходов и tie-breaker.

**Ограниченное исправление (PRD):** заменить prose-выбор на последовательную lattice:

1. Неполное/несопоставимое evidence → **No Decision / Gate Closed**, это не `stop`.
2. Если N нарушает hard gate → `native` запрещён.
3. Если хотя бы один жизнеспособный C проходит все обязательные semantic и hard gates без нового самостоятельного agent-runtime/state-machine, H-1 опровергнута: один подходящий upstream extension surface → `upstream`; в остальных случаях → `profile`.
4. Только если все жизнеспособные C воспроизводимо провалены на одном общем candidate-neutral invariant, а N проходит H-1–H-7 → `native`.
5. Если N не проходит и ни `profile`, ни `upstream` не дают безопасного решения → `stop`.

Дополнительно определить: `profile` — AgentBridge-maintained нормативная композиция существующих extension points без самостоятельного runtime Core; `upstream` — конкретный change set, полностью помещающийся в один заранее признанный upstream surface; `native` — новая общая state/authority/effect semantics, которой некуда корректно встроиться. Наличие «нового имени/объекта» само по себе не является evidence.

### EM-03 — Critical: H-6 позволяет выбрать выигрыш post hoc из множества метрик

**Где:** H-6; FR-103–FR-106; NFR-12–NFR-13; SM-9–SM-12; FR-109.

**Проблема:** Material Advantage может быть заявлено по correctness, effort, dependency surface, performance, resources **либо** evolvability. При десятках endpoints и workload cells почти неизбежно найдётся случайная победа хотя бы в одной ячейке. «Без недопустимой регрессии» пока не имеет формализованных non-inferiority margins. Correctness одновременно является hard gate и кандидатом на сравнительное преимущество, что смешивает допуск и ranking.

**Ограниченное исправление (PRD + Charter):**

- исключить correctness/security/privacy из H-6: это только hard eligibility gates;
- выбрать до кода один primary practical endpoint (рекомендуется повторяемая стоимость достижения safe semantic interop) и максимум два key secondary endpoints;
- для остальных важных измерений задать non-inferiority margins и порядок обязательного прохождения;
- использовать hierarchical/closed testing или поправку multiplicity; exploratory metrics маркировать отдельно и не использовать для FR-109;
- определить минимально практически значимый эффект, confidence interval и правило пограничной/эквивалентной зоны;
- не сводить разные конструкции в единый weighted score.

### EM-04 — Critical: Native Candidate, критерии и corpus могут совместно переобучиться друг на друга

**Где:** F1–F8 как основание Candidate N; FR-98; FR-100; §11.4 VC-1–VC-13; §11.5 Model; SM-5; FR-108.

**Проблема:** команда формулирует требования, затем создаёт N точно под них и теми же понятиями строит oracle. C — существующая система, не оптимизированная под этот словарь. 100% coverage такого corpus доказывает согласованность с собственной моделью, но не обязательно внешний irreducible gap или generalization. Fresh red-team после результатов не заменяет confirmatory holdout.

**Ограниченное исправление (PRD + Charter):**

1. До объектов N/C заморозить candidate-neutral **Semantic Outcome Model**: входные факты, разрешённые/запрещённые эффекты, допустимые outcomes и наблюдаемые инварианты без AgentBridge-specific wire/object names.
2. Для каждого invariant указать независимое основание: threat/misuse case, наблюдаемая интеграционная проблема, публичная норма либо cross-domain requirement; Native design не может быть единственным источником требования.
3. Challenger Advocate и Conformance Lead независимо подтверждают, что oracle не предполагает механизм N.
4. Зарезервировать заранее определённую долю risk-stratified scenario templates и mutations как sealed generalization holdout. Генератор и ограничения замораживаются до кода; конкретные экземпляры раскрываются после freeze реализаций. Скрытые нормативные правила запрещены, после решения holdout публикуется вместе с evidence.
5. Явно различать confirmatory corpus и exploratory red-team cases.

## 4. High findings

### EM-05 — High: отсутствует обязательная структура Statistical Analysis Plan

**Где:** FR-96; FR-105; NFR-12; §11.7; OQ-5; SM-7; SM-11.

**Проблема:** требование заморозить confidence/tolerance rules полезно, но не требует sample-size rationale, единицы независимого наблюдения, randomization, blocking, outlier/exclusion policy, обработки autocorrelation и multiplicity. p99 без достаточного effective sample size легко становится шумом; тысячи запросов внутри одного process run не являются тысячами независимых повторов.

**Исправление (Charter, с обязательной ссылкой из PRD):** отдельный SAP до кода: estimand и unit of analysis для каждой метрики; precision/power rationale; минимум независимых run blocks; случайный либо counterbalanced порядок кандидатов; одинаковые warm-up/cool-down; paired blocks по host/network; handling censored/failed requests; bootstrap/quantile CI; заранее заданные exclusions; multiplicity rule; raw-data retention. Для p99 требовать достаточно effective observations как минимум для 100 ожидаемых tail observations либо более строгий расчёт CI. Последовательное наблюдение результатов требует заранее заданного alpha-spending или запрета раннего подтверждающего вывода.

### EM-06 — High: «две независимые реализации» не контролируют learning, team skill и shared-tool contamination

**Где:** FR-85; F8 validation gate; FR-102; §11.6; A-7.

**Проблема:** две реализации — хороший минимум для interop, но недостаточный experimental replicate. Не определено, строят ли те же люди оба кандидата, видят ли они чужой код/fixtures, имеют ли разный опыт с MCP/A2A и N, используют ли один LLM/semantic generator. «Независимые review-контексты» одного человека не равны независимым авторам. При одном расхождении нельзя отделить defect стандарта от skill outlier.

**Исправление (Charter):** заморозить assignment и contamination policy; записать prior experience, AI/tooling и консультации; запретить обмен semantic code и candidate-specific решениями до freeze; если одни люди участвуют в обоих треках — counterbalance порядок и отделить washout/knowledge disclosure. Shared upstream libraries у C допустимы только как явно заявленные dependency, а независимость оценивается для composition semantics; где возможно, использовать две разные upstream implementations. Любое критичное расхождение требует третьей реализации/слепого adjudication либо даёт `inconclusive`, а не проигрыш стандарта. Не называть Gate 1 организационно независимым без внешних участников.

### EM-07 — High: integration complexity смешивает внутреннюю сложность с текущей зрелостью экосистемы

**Где:** FR-99; FR-103; H-6; SM-9; FR-106.

**Проблема:** время и число defects зависят от опыта реализатора, качества документации, наличия библиотек и зрелости стандарта. N может получить преимущество как маленький специально построенный prototype; C — преимущество через зрелые SDK. Оба эффекта реальны, но отвечают на разные вопросы. При `n=2` нельзя переносить время реализации на популяцию разработчиков.

**Исправление (Charter):** разделить два estimand: (A) intrinsic semantic/operational burden в контролируемой greenfield реализации и (B) practical 2026 deployment cost с лучшими доступными публичными библиотеками. Задать единый start package, training budget, момент старта/финиша, active-effort logging, clarification taxonomy и независимое двойное кодирование glue/ambiguity. Effort при малом числе команд считать case-study evidence с uncertainty, не универсальным средним. DX/adoption inference оставить Gate 3.

### EM-08 — High: performance parity требует двух разных benchmark-плоскостей

**Где:** FR-99; FR-104–FR-105; NFR-12–NFR-15; SM-10–SM-11.

**Проблема:** одинаковый environment не устраняет различия transport, encoding, security primitive, runtime maturity и число hops. Сравнение только best-of-stack смешивает semantic overhead с чужими библиотеками; сравнение только на общем substrate может скрыть реальную deployment cost композиции.

**Исправление (Charter):** две неперемешиваемые серии: (1) **common-substrate** — одинаковые transport/encoding/security strength/runtime class для оценки добавочной semantic cost; (2) **best-deployable** — лучший добросовестный stack каждого кандидата для практической system cost. Отдельно измерять transport, crypto, serialization, provider и semantic processing; публиковать оба результата без объединённого score. Security/assurance floor и functional scope остаются одинаковыми.

### EM-09 — High: равная возможность исправления defects не имеет лимита и допускает adaptive tuning

**Где:** FR-99; §10.2; правила паритета §11.3; §11.5 Implement/Measure; SM-1.

**Проблема:** после просмотра тестов один кандидат можно многократно оптимизировать, назвав изменения «implementation defects», а изменение другого признать «contract change». Не заданы число циклов, time budget, freeze commit и blinded defect classification.

**Исправление (Charter):** одинаковый correction budget (время и максимум циклов), triage до раскрытия сравнительных метрик, taxonomy `implementation defect / harness defect / normative ambiguity / hypothesis change`, независимый adjudicator и immutable freeze digest перед confirmatory runs. Любая contract/oracle change аннулирует симметрично все затронутые cells; performance tuning после просмотра confirmatory result допускается только в новом зарегистрированном run.

### EM-10 — High: representativeness заявлена, но sampling/coverage rule допускает cherry-picking комбинаций

**Где:** FR-87; FR-100; §11.4; SM-4–SM-5.

**Проблема:** шесть topologies и три domains — сильная рамка, но правило «обосновать исключение» не задаёт минимального покрытия взаимодействий risk × feature × topology × mode. Можно выполнить каждый элемент по одному разу, не проверив опасные сочетания: multi-party + revocation + partition, bridge + reverse async + unknown effect, stream + authority expiry и т. п. Три домена — purposive sample, а не доказательство будущей универсальности.

**Исправление (Charter):** traceability matrix `invariant/threat → FR/NFR → topology → mode → domain → positive/negative/compound vector → oracle`; обязательное покрытие каждого уникального failure mechanism и заранее выбранное pairwise/covering-array правило для взаимодействий. Указать rationale для omitted higher-order combinations до кода. Claims ограничить проверенными классами; не выводить «универсальность» из числа доменов.

### EM-11 — High: не разделены safety stop, futility, invalidation, budget exhaustion и итог `stop`

**Где:** FR-101; FR-109–FR-110; §10.5; §11.5; §11.7; OQ-7.

**Проблема:** critical defect останавливает переход этапа, бюджет ведёт к stop/re-scope, а `stop` является содержательным результатом. Без taxonomy ранняя остановка может превратиться в удобный итог или, наоборот, отрицательное evidence будет ошибочно названо «incomplete». Не определены симметричные futility boundaries и предел invalid runs.

**Исправление (PRD + Charter):** определить пять разных статусов: `candidate disqualified`, `run invalid`, `experiment paused`, `Gate closed/inconclusive`, итоговый `stop`. Immediate safety stop прекращает опасный run, но не стирает evidence; budget exhaustion без полного evidence = Gate closed; `stop` допустим только после валидного falsification всех разрешённых направлений. Задать до кода ceilings invalid runs, correction cycles, resource/time budget и симметричные futility rules. Нельзя рано объявить `native` по первым успешным ячейкам.

### EM-12 — High: нулевой prohibited-effect результат зависит от недоказанной полноты наблюдателей

**Где:** FR-90; FR-101; SM-2; §10.1; §11.4; §11.7 OQ-3.

**Проблема:** «0 наблюдаемых эффектов» может означать отсутствие эффекта или слепую зону effect observer. То же относится к privacy leakage. Общий simulator способен одинаково скрыть defect обоих кандидатов.

**Исправление (Charter):** закрытая модель тестовой среды с инвентарём всех effect/egress surfaces; независимые canary и taint markers; append-only observer log; positive-control тесты, которые обязаны обнаруживаться; fault/mutation тесты самих observers; отдельная реализация oracle/observer от candidate code; запрет положительного safety verdict при неполном telemetry coverage. Ошибка observer инвалидирует cell, а не превращает её в pass.

## 5. Medium findings

### EM-13 — Medium: red-team смешивает confirmatory и exploratory evidence

**Где:** FR-108; §11.5 Challenge; §11.6 Fresh-context Red Team; §10.2.

**Проблема:** red team видит полный evidence и может создать новый counterexample. Это ценная exploratory работа, но новый тест, выбранный по наблюдённым результатам, нельзя статистически смешивать с исходным confirmatory corpus.

**Исправление (Charter):** red team заранее получает frozen threat/requirements, но не comparative labels до подачи первой атаки; findings маркируются exploratory. Accepted finding создаёт новый preregistered replication tranche с собственным version ID; исходные результаты сохраняются, а новый tranche не используется для пересчёта старого p-value/CI. Независимость указывается фактически: иной человек/организация либо только fresh-context simulation.

### EM-14 — Medium: reproducibility package недостаточно закреплён на уровне неизменяемых артефактов

**Где:** FR-92; FR-107; NFR-22; SM-7; SM-14.

**Проблема:** версии, seed и environment предусмотрены, но нет обязательного content-addressed snapshot, build provenance и calibration record. Floating dependencies или изменившийся upstream способны сделать формально «тот же» rerun другим экспериментом.

**Исправление (Charter):** content digests для spec/candidates/suite/data; lockfiles и SBOM; source/archive snapshots разрешённых upstream материалов; build provenance; machine-readable environment и benchmark calibration; UTC timestamps; append-only raw result index; отдельный manifest публичного и приватного evidence. Воспроизведение допускает новый hardware только в заранее заданных tolerance bounds.

### EM-15 — Medium: требование того же Gate direction при rerun хрупко около порога

**Где:** SM-7; H-6; NFR-12; FR-105.

**Проблема:** два корректных rerun рядом с threshold могут дать разные направления из-за sampling error. Требование «тот же Gate direction» без grey zone стимулирует повторные запуски до желаемого результата или чрезмерно широкие margins.

**Исправление (PRD + Charter):** ввести preregistered indifference/uncertainty zone. Если confidence interval пересекает decision boundary, результат `inconclusive/Gate closed`, а не winner. Провести sensitivity analysis к разумным margins и environment blocks; rerun подтверждает применение той же decision rule и совместимые effect estimates, а не обязательно бинарную метку при пограничных данных.

## 6. Обязательный минимальный пакет перед freeze Charter

Gate 1 можно начинать только после появления восьми конкретных артефактов:

1. Candidate-neutral Semantic Outcome Model и traceability к внешним основаниям.
2. Landscape search protocol, Pareto shortlist C и immutable Candidate manifests.
3. Взаимоисключающая decision lattice с tie-breakers и статусом `No Decision`.
4. Confirmatory/exploratory split, sealed holdout и coverage design.
5. Statistical Analysis Plan с units, sample sizes, randomization, margins и multiplicity.
6. Independence/assignment/correction policy и genealogy manifests.
7. Common-substrate и best-deployable benchmark plans.
8. Stop/invalidation taxonomy, observer-validation plan и reproducibility manifest.

## 7. Acceptance recommendation

**PRD может перейти к следующей редакции**, если EM-01–EM-04 будут внесены непосредственно в определения/FR/SM и PRD явно потребует закрыть EM-05–EM-12 в подробном Validation Charter. EM-13–EM-15 допустимо закрыть в Charter, но их disposition должно быть записано до его freeze.

После исправлений рекомендуется повторная узкая методологическая проверка только изменённых §3, §4.9, §8, §10–§11 и связанного подробного Charter. До этого текущий PRD достаточен для продолжения планирования, но **не достаточен для честного запуска сравнительного кода или вынесения Gate 1 результата**.

## Re-review after fixes

**Дата повторной проверки:** 2026-09-13  
**Общий результат:** прежние Critical-находки сняты; большинство High-находок закрыто обязательствами PRD. Остались **0 Critical и 3 High**: две частично/полностью незакрытые прежние проблемы и одна новая регрессия в staged early-exit logic. Точные числовые значения по-прежнему корректно отложены в frozen Validation Charter.

| Finding | Статус | Доказательство в текущем PRD / остаток |
| --- | --- | --- |
| **EM-01 — Challenger как неопределённый единственный максимум** | **Resolved** | В §3 введены `Composition Challenger Set` и `Primary Composition Challenger`; FR-97 требует frozen search/inclusion protocol, 1–3 Pareto-недоминируемых end-to-end deployable конфигурации, lexicographic Primary C и sensitivity; `native` должен выдержать каждый жизнеспособный C. |
| **EM-02 — пересекающиеся исходы и цикличная H-1** | **Resolved** | FR-109 теперь задаёт упорядоченную lattice `Validity → Eligibility → Existing-surface → Native → No viable direction`, отделяет `Gate Closed / No Decision` от `stop` и устанавливает tie-break `upstream` перед `profile`. FR-96/FR-97 требуют candidate-neutral Ledger и запрещают считать Native naming/layering доказательством gap. |
| **EM-03 — post-hoc выбор Material Advantage** | **Resolved** | Определение Material Advantage в §3, H-6 и §11.7 требуют один primary practical endpoint, не более двух key secondary, practically-significant threshold, confidence interval, non-inferiority margins, uncertainty zone и multiplicity control; correctness/security/privacy отделены как eligibility gates. |
| **EM-04 — совместное переобучение N, требований и corpus** | **Partial — High остаётся** | FR-96 и Gate 1A теперь замораживают Evaluation Invariant Ledger, provenance, Allocation Matrix и scenario/coverage design **до design objects N/C**; §11.7 добавляет sealed holdout и confirmatory/exploratory split. Но Gate 1B по-прежнему предлагает разрабатывать candidate models и candidate-neutral oracle одновременно, а полные vectors/expected outcomes обязаны быть frozen только «до кода». Candidate-specific model ещё может повлиять на oracle до confirmatory freeze. Нужна одна короткая норма: oracle schema, metamorphic properties, scenario templates, holdout-generation rule и expected-outcome authority замораживаются до candidate-specific model design; любое последующее изменение получает новый preregistration и независимый blind review. |
| **EM-05 — нет структуры Statistical Analysis Plan** | **Resolved** | §11.7 прямо требует SAP с estimands, unit of analysis, sample-size/precision rationale, independent run blocks, randomization/counterbalancing, exclusions, censored/failure handling, tail confidence и raw-data retention; H-6/SM-7 задают confidence и uncertainty logic. |
| **EM-06 — слабый контроль независимости реализаций** | **Resolved для Gate 1C** | FR-102 определяет единицу независимости C и shared upstream genealogy; §11.7 требует assignment/contamination policy, prior experience, exposure, AI/tooling/consultation logs, blind adjudication/третью реализацию либо `inconclusive`. §11.6 честно ограничивает claim при отсутствии организационной независимости. Отдельная early-exit регрессия вынесена как EM-16 ниже. |
| **EM-07 — integration complexity смешивает intrinsic burden и ecosystem maturity** | **Unresolved — High** | FR-99 требует измерять/контролировать maturity и skill, а §11.7 задаёт общий primary endpoint «совокупная стоимость достижения safe semantic interoperability». Но PRD не требует раздельно оценивать (A) intrinsic greenfield semantic/operational burden и (B) practical current-deployment cost с лучшими публичными SDK. Без этого H-6 может приписать выигрыш архитектуре, хотя его создала зрелость инструментов, либо наоборот. Добавить в §11.7 два раздельных integration-cost estimand, единый start/training package, active-effort/clarification taxonomy и запрет population/DX claims при малом числе implementers. |
| **EM-08 — performance смешивает semantic overhead и deployable stack** | **Resolved** | Gate 1D и §11.7 требуют отдельные `common-substrate` и `best-deployable` benchmark plans без единого weighted score; FR-99 сохраняет общий assurance/scope и контроль environment. |
| **EM-09 — неограниченное adaptive defect fixing** | **Resolved** | §11.7 требует одинаковый correction budget, максимум циклов, taxonomy implementation/harness/specification/hypothesis, adjudication до comparative metrics и новый preregistration при contract/oracle change; FR-96 сохраняет историю invalidation. |
| **EM-10 — scenario representativeness допускает cherry-picking** | **Resolved** | §11.7 требует risk-stratified holdout, coverage `risk × feature × topology × mode × domain`, traceability/covering rule и preregistered rationale исключённых higher-order combinations с ограничением claim; VC-2–VC-12 дополнены опасными compound cases. |
| **EM-11 — stop/invalidation/inconclusive смешаны** | **Resolved** | FR-109 различает validity, candidate disqualification, Gate Closed и final `stop`; Gate 1A–1D и §11.7 требуют отдельные criteria для `candidate disqualified`, `run invalid`, `paused`, `inconclusive` и `stop`, включая early-exit/sufficiency. |
| **EM-12 — нулевые эффекты при слепом observer** | **Resolved** | FR-101, SM-2, §10.1 и §11.7 требуют полного inventory effect/egress channels, positive controls, canary/taint markers, fault/mutation tests, detection windows и правило `observer uncertainty → invalid`. |
| **EM-13 — red-team смешивает confirmatory и exploratory evidence** | **Partial — Medium** | §11.7 теперь требует confirmatory/exploratory split, а FR-108 запрещает post-hoc смену критериев. Однако прямо не сказано, что новый red-team case образует отдельный versioned replication tranche и не пересчитывает старый confirmatory effect/CI. Это желательно уточнить в Charter, но не блокирует финализацию PRD. |
| **EM-14 — слабая неизменяемость reproduction package** | **Resolved** | NFR-22 и §11.7 требуют content digests, immutable manifests, build provenance, dependency locks/SBOM, calibration, UTC time, environments и append-only raw-result index. |
| **EM-15 — бинарный rerun возле порога** | **Resolved** | SM-7 и §11.7 вводят uncertainty/indifference zone: пересечение границы оставляет Gate закрытым; rerun обязан воспроизвести equivalence/safety verdicts и совместимые effect estimates по той же rule, а не принудительно тот же winner label. |
| **EM-16 — новый: Gate 1B может вынести финальный `profile/upstream/stop` без independent implementability winning direction** | **Unresolved — High** | §11.5 Gate 1B и F9 acceptance разрешают полный ранний Decision Record после model/executable proof of concept, не требуя Gate 1C. Это полезно для раннего опровержения `native`, но способно окончательно выбрать C-направление, которое ещё не выдержало FR-102, Differential Interoperability и independent interpretation. Исправление: Gate 1B может выдать только `Native hypothesis falsified / provisional direction`; для финального `profile` или `upstream` выполнить Gate 1C хотя бы для winning C semantics (две независимые composition implementations и hard-gate interop). Полный N benchmark после надёжного опровержения H-1 можно не строить. Ранний `stop` допустим только при заранее определённом валидном falsification, не основанном на неиспытанной реализуемости C. |

### Вердикт повторной проверки

Исправления существенно повысили достоверность дизайна: **все четыре прежних Critical устранены**. PRD почти готов с точки зрения экспериментальной методологии, но перед финализацией следует закрыть три High-пункта:

1. полностью отделить candidate-neutral oracle от проектирования кандидатов (**EM-04**);
2. разделить intrinsic и practical integration-cost estimands (**EM-07**);
3. не превращать Gate 1B model/PoC в окончательный выбор `profile/upstream` без независимой реализации выигравшей композиции (**EM-16**).

Простой вердикт: **план эксперимента стал сильным и беспристрастным по замыслу, но пока в нём остаются три лазейки, способные выбрать неверное направление. После трёх точечных исправлений методологический reviewer может рекомендовать финализацию PRD.**

## Final narrow re-review

**Дата:** 2026-09-13  
**Проверенный scope:** только последние изменения по EM-04, EM-07, EM-16 и связанной security-находке SEC-14.

| Finding | Итог | Краткое доказательство в PRD |
| --- | --- | --- |
| **EM-04 — независимость candidate-neutral oracle** | **Resolved** | Gate 1A (§11.5) теперь замораживает oracle schema, metamorphic properties, scenario/holdout-generation rules и expected-outcome authority **до** design objects N/C. §11.7 повторяет этот порядок, требует независимого вывода либо blind cross-check security-critical outcomes и переводит результат без cross-check в `invalid/inconclusive`. Последующее изменение oracle требует нового preregistration и blind review. |
| **EM-07 — intrinsic burden против ecosystem maturity** | **Resolved** | FR-103 раздельно определяет `intrinsic greenfield burden` и `practical current-deployment cost`, запрещает смешивать зрелость экосистемы с простотой протокола, требует одинаковые start/training rules и раздельный учёт effort/help/rework. §11.7 делает оба estimand обязательными и ограничивает population/DX claims при малом числе implementers. |
| **EM-16 — преждевременный финальный исход Gate 1B** | **Resolved** | FR-109 разрешает окончательный `profile/upstream` только после independent implementation и hard-gate interoperability выбранной C-семантики. §11.5 и F9 acceptance теперь делают исход 1B лишь `Native hypothesis falsified` + provisional direction; Gate 1C требует две независимые composition implementations. Полный Native benchmark можно не строить после надёжного опровержения H-1, что сохраняет эффективность без ослабления доказательства winning direction. |
| **SEC-14 — доверие и независимая проверка oracle/observer** | **Resolved at PRD contract** | NFR-22 включает oracle/observer/harness в trust и fault analysis; §10.1 требует независимый effect observer, positive-control/canary calibration и invalidation при слепоте; §11.7 требует immutable oracle manifests, независимый derivation/blind cross-check security-critical expected outcomes и `invalid/inconclusive` при его отсутствии. Конкретная genealogy и реализация cross-check корректно остаются обязательными параметрами подробного Charter. |

**Регрессии:** в проверенном узком scope новых Critical или High проблем не обнаружено.

**Блокеры финализации PRD:** **0 Critical, 0 High** по методологии эксперимента и проверенному аспекту SEC-14.

**Простой вердикт:** эти последние правки закрывают оставшиеся опасные лазейки. PRD можно финализировать; перед экспериментальным кодом всё равно необходимо заполнить и заморозить подробный Validation Charter — это предусмотренный следующий этап, а не незакрытый дефект PRD.
