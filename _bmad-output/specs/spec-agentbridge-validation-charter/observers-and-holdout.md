# Observer, sandbox and sealed holdout contract — OQ-3A proposal

Статус: **proposal-ready; not accepted; not frozen**.

## 1. Observer catalog

Observer фиксирует только заранее объявленные наблюдения. Он не создаёт нормативный смысл и не подменяет external truth утверждением кандидата.

| ID | Что наблюдает | Минимальный независимый источник | Failure consequence |
| --- | --- | --- | --- |
| OBS-01 | Входы/выходы, negotiation transcript, selected versions/profiles и correlation/causality | Harness-side immutable event record | Потеря/неоднозначность → затронутый cell invalid |
| OBS-02 | Authority, Decision Subject, Consent/Approval/Obligation и decision boundary | Oracle-side reconstructed decision record, не candidate status | Нельзя доказать permit/non-permit → invalid для safety claim |
| OBS-03 | Protected Disclosures и taint/canary propagation | Sink-side disclosure ledger + независимый taint marker | Blind spot или unexpected disclosure → invalid или candidate fail по подтверждённому effect |
| OBS-04 | Consequential commits, external Effect State и shared-limit consumption | Авторитетный isolated simulator ledger + независимый commit counter | Неполнота → invalid; запрещённый/duplicate effect → candidate fail |
| OBS-05 | CPU, memory, storage, queue/depth, allocations и work amplification | Environment-level resource meter вне candidate process | Missing interval/overflow → resource result invalid |
| OBS-06 | Network destinations, redirects, DNS/bridge paths и egress | Sandbox boundary log + deny-by-default enforcement record | Escape attempt → candidate fail; blind enforcement → run invalid/pause |
| OBS-07 | Logical time, schedule, partition, reorder и injected faults | Deterministic scheduler/fault-controller record | Schedule divergence → cell invalid |
| OBS-08 | Lifecycle state, retries, recovery, cancellation и branch epochs | External state-model trace reconstructed from events | Gap/conflict hidden → fail; unavailable trace → invalid |
| OBS-09 | Evidence, issuer/source, assurance, causal provenance и redaction gaps | Append-only evidence index independent of candidate claim | Missing provenance → claim narrowed/invalid; assurance inflation → fail |
| OBS-10 | Dependency/provider identity, version, composition order, outage and substitution | Environment manifest + provider boundary probes | Unobserved mandatory dependency → invalid; hidden dependency → fail |
| OBS-11 | Payload/metadata/timing/error/linkability disclosure | Channel-specific privacy probes + correlation analysis | Out-of-bound disclosure → fail; insufficient sensitivity → invalid |
| OBS-12 | Executed artifact, build/dependency closure и instrumentation interference | Immutable build/run manifest + control run comparison | Artifact mismatch or observer-induced semantic change → invalid |

## 2. Safety-critical dual observation

Каждый потенциальный Protected Disclosure или Consequential Action требует минимум двух не сводимых к одному candidate self-report путей:

1. authoritative sandbox/simulator state либо sink-side record;
2. независимый boundary/taint/commit observation.

Совпадение двух логов, полученных из одного candidate status, не считается независимостью. При расхождении сохраняются оба наблюдения; optimistic resolution запрещён, результат `invalid` либо `unknown` согласно frozen oracle.

## 3. Observer qualification

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

Калибровка имеет expected outcome, raw evidence и content digest. Сбой обязательной calibration блокирует все зависящие cells.

## 4. Detection window and completion

Каждый vector задаёт logical start/end, максимальную задержку simulated effect, post-termination observation window и явное completion event. Истечение окна без подтверждённой полноты даёт `unknown/invalid`, а не `established-none`.

Observer completeness требует:

- перечислить все заявленные effect/egress/disclosure channels и commit points;
- доказать coverage каждого через calibration;
- не иметь необъяснённых gaps, dropped records или correlation ambiguity;
- связать records с точным executed artifact, vector, seed, schedule и environment.

## 5. Sandbox and egress boundary

- Только synthetic identities/data и isolated effect simulators.
- External egress deny-by-default; allowlist содержит только назначенные synthetic endpoints.
- Redirect, DNS rebinding, delayed callback, bridge chain и alternate protocol escape проверяются явно.
- Production credentials, реальный customer data, платежи, бронирования, юридические обязательства и необратимые production effects запрещены.
- Попытка escape останавливает затронутый run и запускает evidence-preserving review; календарь или стоимость не разрешают продолжить небезопасно.

## 6. Holdout generation rule

Sealed holdout проверяет обобщение, но не содержит тайной нормы.

### Frozen inputs

- финальные EI-01–EI-25 и Oracle Outcome Contract;
- DS-1–DS-6 grammars и MP-01–MP-18;
- VC-1–VC-13 risk strata;
- применимые OQ-4 blocking classes, OQ-5 bounds и OQ-8 storage/access controls;
- versioned generator rule и exact sampling quotas ниже.

### Sampling quotas

Генерируются **39 holdout vectors: по 3 на каждую VC-family**:

1. `H-SINGLE` — один unseen boundary/adversary operator;
2. `H-COMPOUND` — пара операторов из разных risk dimensions с контролируемым interaction;
3. `H-PERMUTE` — role/topology/domain/version permutation, сохраняющая либо явно меняющая expected relation по MP.

Для VC-12 `H-COMPOUND` использует три оператора, включая один observer/resource/egress fault. Минимум один vector каждой семьи затрагивает safety/privacy boundary. Ни один exact public vector не копируется с заменой только косметического label.

### Generation and sealing

1. До candidate design замораживаются rule/version/quotas и допустимые operator combinations.
2. После OQ-4/OQ-5/OQ-8 Evidence Custodian по заранее принятой seed ceremony делает один фиксируемый draw, недоступный advocates/implementers, и создаёт exact vectors детерминированным генератором. Повторный draw допустим только при заранее определённом техническом failure и сохраняет первый seed/error в закрытом audit log.
3. Oracle Author выводит allowed/forbidden outcomes только из frozen Ledger/Oracle; независимый blind cross-checker проверяет все security-critical outcomes.
4. Exact vectors, seed и expected outcomes запечатываются; снаружи сохраняются manifest, generator/version, count, strata и content digest без раскрытия cases.
5. Holdout открывается один раз после freeze candidate contracts, implementation/build digests, public-corpus results и Statistical Analysis Plan.
6. Первичный результат сохраняется независимо от pass/fail. Исправление после открытия проверяется новым versioned replication tranche и не переписывает исходный holdout result.

## 7. Contamination and invalidation

Holdout cell/run invalid, если до разрешённого открытия candidate author/implementer получил exact case, seed, expected outcome или эквивалентную подсказку; digest/manifests не совпадают; generator изменён; oracle использовал candidate-specific object model; либо отсутствует обязательный blind cross-check.

Новая найденная атака не добавляется тайно в старый holdout. Она становится exploratory case, затем после независимого review — новым regression/replication tranche с отдельной версией.

## 8. OQ-3B deliverables before code

- 65+ exact public confirmatory vectors и 39 sealed holdout vectors;
- exact synthetic datasets, schedules, allowed/forbidden Outcome Tuples и observer contracts;
- calibration corpus OBS-01–OBS-12;
- manifest/digests, authorship, cross-check и access log;
- coverage report EI/FR/NFR × VC × MP × OBS;
- zero unresolved oracle contradictions or unobserved Safety Blocking channel.

OQ-3A freeze не заменяет OQ-3B: он фиксирует честный способ создания экзамена, но не разрешает код до появления полного corpus.
