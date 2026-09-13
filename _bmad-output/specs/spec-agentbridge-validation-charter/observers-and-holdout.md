# Observer, sandbox and sealed holdout contract — OQ-3A proposal

Статус: **OQ-3A frozen by Project Owner on 2026-09-13; OQ-3B not frozen**.

## 1. Observer catalog

Observer фиксирует только заранее объявленные наблюдения. Он не создаёт нормативный смысл и не подменяет external truth утверждением кандидата.
Все последствия в таблице подчиняются §3: candidate-caused omission/corruption всегда `candidate-fail`, а `run-invalid` требует независимого доказательства observer/harness failure.

| ID | Что наблюдает | Минимальный независимый источник | Failure consequence |
| --- | --- | --- | --- |
| OBS-01 | Входы/выходы, negotiation transcript, selected versions/profiles и correlation/causality | Harness-side immutable event record | Применить source-attribution rule: harness fault → invalid; candidate omission/equivocation → fail |
| OBS-02 | Authority, Decision Subject, Consent/Approval/Obligation и decision boundary | Oracle-side reconstructed decision record, не candidate status | Применить source-attribution rule; отсутствие mandatory candidate evidence не становится invalid |
| OBS-03 | Protected Disclosures и taint/canary propagation | Sink-side disclosure ledger + независимый taint marker | Blind spot или unexpected disclosure → invalid или candidate fail по подтверждённому effect |
| OBS-04 | Consequential commits, external Effect State и shared-limit consumption | Авторитетный isolated simulator ledger + независимый commit counter | Неполнота → invalid; запрещённый/duplicate effect → candidate fail |
| OBS-05 | CPU, memory, storage, queue/depth, allocations и work amplification | Environment-level resource meter вне candidate process | Missing interval/overflow → resource result invalid |
| OBS-06 | Network destinations, redirects, DNS/bridge paths и egress | Sandbox boundary log + deny-by-default enforcement record | Escape attempt → candidate fail; blind enforcement → run invalid/pause |
| OBS-07 | Logical time, schedule, partition, reorder и injected faults | Deterministic scheduler/fault-controller record | Schedule divergence → cell invalid |
| OBS-08 | Lifecycle state, retries, recovery, cancellation и branch epochs | External state-model trace reconstructed from events | Gap/conflict hidden либо candidate-required trace absent → fail; independently failed observer → invalid |
| OBS-09 | Evidence, issuer/source, assurance, causal provenance и redaction gaps | Append-only evidence index independent of candidate claim | Candidate-required provenance missing/inflated → fail; independently failed index → invalid |
| OBS-10 | Dependency/provider identity, version, composition order, outage and substitution | Environment manifest + provider boundary probes | Unobserved mandatory dependency → invalid; hidden dependency → fail |
| OBS-11 | Payload/metadata/timing/error/linkability disclosure | Channel-specific privacy probes + correlation analysis | Out-of-bound disclosure → fail; insufficient sensitivity → invalid |
| OBS-12 | Executed artifact, build/dependency closure и instrumentation interference | Immutable build/run manifest + control run comparison | Artifact mismatch or observer-induced semantic change → invalid |

## 2. Safety-critical dual observation

Каждый потенциальный Protected Disclosure или Consequential Action требует минимум двух не сводимых к одному candidate self-report путей:

1. authoritative sandbox/simulator state либо sink-side record;
2. независимый boundary/taint/commit observation.

Совпадение двух логов, полученных из одного candidate status, не считается независимостью. Пути независимы только когда один непроверенный event source, hook/transformation, correlation key, storage path, controller или administrative authority не может скрыть либо сфабриковать оба результата. Canaries вводятся на уровне или ниже каждой disclosure/effect/egress boundary и commit point. При расхождении сохраняются оба наблюдения; optimistic resolution запрещён, применяется source-attribution rule.

## 3. Source attribution

- Доказанный сбой qualified harness/observer/environment, возникший независимо от candidate output, даёт `run-invalid`.
- Если qualified observer не получил обязательный результат потому, что кандидат его omitted, corrupted, equivocated либо сделал uncorrelatable, это `candidate-fail`.
- Attribution основывается на raw independent paths и заранее объявленных fault controls, а не на итоговом результате кандидата.
- Спор рассматривает outcome-blind adjudicator. Неразрешённый спор даёт `inconclusive` для соответствующего mandatory scope, не удаляется и не поддерживает положительный Gate.

## 4. Observer qualification

До confirmatory run каждый применимый OBS проходит versioned calibration:

- positive control: наблюдает известный разрешённый эффект;
- forbidden-effect canary: обнаруживает специально введённый запрещённый эффект/раскрытие;
- omission: потеря одного канала определяется как incompleteness;
- mutation: неправильные actor/subject/audience/epoch/correlation не сливаются с правильными;
- delayed effect: observation window охватывает поздний simulator commit;
- crash/restart: observer сохраняет либо честно теряет continuity;
- collision: одинаковые внешние labels разных операций не объединяются;
- non-interference: включение observer не меняет allowed Outcome Tuple и не сериализует проверяемую race;
- resource saturation: observer сам не скрывает exhaustion и измеряет собственную стоимость отдельно.

Для каждого `OBS × channel/commit point` manifest до confirmatory run фиксирует qualification vector IDs, deterministic completeness proof либо число повторов, sensitivity/false-positive/false-negative tolerance для статистических измерений, continuity/saturation limits, действительный environment/artifact scope, срок действия и независимого approver. Значения tolerance/repetitions заполняются frozen OQ-5 slots без изменения правила. Калибровка имеет expected outcome, raw evidence и content digest. Сбой, просрочка или изменение scope обязательной calibration блокирует все зависящие cells.

## 5. Detection window and completion

Каждый vector задаёт logical start/end, максимальную задержку simulated effect, post-termination observation window и явное completion event. Истечение окна без подтверждённой полноты даёт `unknown/invalid`, а не `established-none`.

Observer completeness требует:

- перечислить все заявленные effect/egress/disclosure channels и commit points;
- доказать coverage каждого через calibration;
- не иметь необъяснённых gaps, dropped records или correlation ambiguity;
- связать records с точным executed artifact, vector, seed, schedule и environment.

## 6. Sandbox and egress boundary

- Только synthetic identities/data и isolated effect simulators.
- External egress deny-by-default; allowlist содержит только назначенные synthetic endpoints.
- Redirect, DNS rebinding, delayed callback, bridge chain и alternate protocol escape проверяются явно.
- Production credentials, реальный customer data, платежи, бронирования, юридические обязательства и необратимые production effects запрещены.
- Попытка escape останавливает затронутый run и запускает evidence-preserving review; календарь или стоимость не разрешают продолжить небезопасно.

## 7. Holdout generation rule

Sealed holdout проверяет обобщение, но не содержит тайной нормы.

### Frozen inputs

- финальные EI-01–EI-25 и Oracle Outcome Contract;
- DS-1–DS-6 grammars, atomic coverage inventory и MP-01–MP-24;
- VC-1–VC-13 risk strata;
- применимые OQ-4 blocking classes, OQ-5 bounds и OQ-8 storage/access controls;
- versioned generator rule и exact sampling quotas ниже.

### Sampling quotas

Генерируются **39 holdout vectors: по 3 на каждую VC-family**:

1. `H-SINGLE` — один unseen boundary/adversary operator;
2. `H-COMPOUND` — пара операторов из разных risk dimensions с контролируемым interaction;
3. `H-PERMUTE` — role/topology/domain/version permutation, сохраняющая либо явно меняющая expected relation по MP.

Для VC-12 `H-COMPOUND` использует три оператора, включая один observer/resource/egress fault. Минимум один vector каждой семьи затрагивает safety/privacy boundary. Ни один exact shared-confirmatory vector не копируется с заменой только косметического label.

### Estimand, coverage and decision role

Holdout оценивает переносимость заранее заявленного conformance/safety поведения на unseen combinations из того же frozen claim universe; он не оценивает весь будущий интернет. Число 39 получается как три разные generalization probes для каждой из 13 обязательных VC families; это минимальный manipulation-resistant coverage floor, а не самостоятельная статистическая оценка редких мировых событий.

Generator использует stratified covering rule по `risk × AC atom × feature × topology × mode × domain`: каждый VC получает три класса выше; каждый safety/privacy VC — safety case; все mandatory risk strata получают ненулевую заранее записанную probability/quota; high-risk atoms распределяются так, чтобы ни один выбранный result не мог исключить целый risk dimension. Exact strata matrix, probabilities и OQ-5 precision/decision thresholds заполняются до seed и входят в preregistration. Cases считаются near-duplicate, если совпадают AC/failure mechanism, abstract event graph, boundary и expected relation после удаления labels; такие cases не удовлетворяют разные quotas.

Любой Safety Blocking miss применяется по OQ-4 без усреднения. Обычный mandatory conformance miss проваливает соответствующий claim либо весь tranche по frozen OQ-5/SAP rule. Invalid/inconclusive strata нельзя исключить или перевзвесить после открытия; они требуют заранее предусмотренной replication либо дают `Gate Closed / No Decision`.

### Generation and sealing

1. До Candidate design замораживаются generator identity/version, validation predicates, rule/quotas, заполненные OQ-4/OQ-5/OQ-8 slots и допустимые operator combinations.
2. После заполнения slots seed ceremony использует минимум два независимых outcome-neutral entropy contributions либо эквивалентно невыбираемый публично проверяемый source. Custodian и независимый witness фиксируют commitments до reveal; generator делает один draw. Reroll разрешён только для заранее перечисленных structural failures (`generator crash before manifest`, `cryptographic commitment mismatch`, `case violates frozen grammar/duplicate predicate`) и никогда из-за outcome; первый seed/error сохраняется.
3. Oracle Author выводит allowed/forbidden outcomes только из frozen Ledger/Oracle; независимый blind cross-checker проверяет все security-critical outcomes.
4. Exact vectors, seed и expected outcomes запечатываются; снаружи сохраняются manifest, generator/version, count, strata и content digest без раскрытия cases.
5. Holdout открывается один раз после freeze candidate contracts, implementation/build digests, shared-confirmatory corpus results и Statistical Analysis Plan.
6. Первичный результат сохраняется независимо от pass/fail. Исправление после открытия проверяется новым versioned replication tranche и не переписывает исходный holdout result.

Oracle Author, cross-checker, Custodian и witness не могут быть Candidate advocate/implementer, outcome decision owner либо подчиняться одному outcome owner для этой функции. Atomic OQ-3B preregistration и sealing завершаются до OQ-2B/Candidate design; они не получают Candidate designs, outputs, comparative metrics или advocate communications. Действуют least-access compartments, recusal, immutable access/communication log и запрет передачи exact cases, seed, derived hints, event graph, operator combination или expected relation. Будущие implementers получают только shared-confirmatory corpus и frozen interfaces. Все outcome-affecting degrees of freedom фиксируются до Candidate design.

## 8. Contamination and invalidation

Contamination classes фиксируются до seed: `case disclosure`, `seed/generator-state disclosure`, `derived structural hint`, `unauthorized privileged communication`, `artifact/digest mismatch`, `candidate-shaped oracle`, `missing cross-check`. Outcome-blind adjudicator применяет заранее указанную область: доказанная single-case leak заражает весь stratum; seed/generator-state либо системная role/access failure заражает весь holdout. Заражённый первичный case/tranche остаётся в историческом primary result, никогда не заменяется выборочно и не поддерживает положительное решение; запускается отдельный preregistered replication tranche.

Новая найденная атака не добавляется тайно в старый holdout. Она становится exploratory case, затем после независимого review — новым regression/replication tranche с отдельной версией.

## 9. OQ-3B deliverables before code

- 65+ exact shared-confirmatory vectors, полное AC coverage и 39 sealed holdout vectors;
- exact synthetic datasets, schedules, allowed/forbidden Outcome Tuples и observer contracts;
- calibration corpus OBS-01–OBS-12;
- manifest/digests, authorship, cross-check и access log;
- coverage report AC/EI/FR/NFR × VC × MP × OBS;
- zero unresolved oracle contradictions or unobserved Safety Blocking channel.

OQ-3A freeze не заменяет OQ-3B: он фиксирует честный способ создания экзамена, но не разрешает код до появления полного corpus.
