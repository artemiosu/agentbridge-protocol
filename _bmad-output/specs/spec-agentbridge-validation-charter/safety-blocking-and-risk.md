# OQ-4 — Safety Blocking Class и residual risk

Статус: **OQ-4 safety policy frozen by Project Owner on 2026-09-13**. Independent security review: PASS, Critical 0, High 0, Medium 0, Low 0. Этот freeze не разрешает экспериментальный код и не замораживает весь Charter.

## 1. Простое правило

У результата есть две независимые оценки:

1. **Blocking class:** нарушена ли гарантия, без которой протокол опасен или доказательство недостоверно.
2. **Severity:** насколько срочно и широко исправлять проблему.

Если ответ по первой оси — «да», кандидат нельзя выбрать положительно при любом severity, среднем балле, быстродействии, стоимости или удобстве. Severity не является способом отменить запрет.

## 2. Неизменяемые классы

| ID | Автоматически блокирующее событие | Связь с Ledger |
| --- | --- | --- |
| **SBC-01 — Authority breach** | Protected Disclosure или Consequential Effect без действующего явного permit; расширение Authority; обход Consent, Approval или due Obligation; confused deputy; использование отозванных, истёкших, чужих либо stale полномочий | EI-05–EI-08, EI-18 |
| **SBC-02 — Disclosure/effect boundary race** | Разрешённый subject, authority, policy, epoch или precondition изменился, но Protected Disclosure или Consequential Effect произошёл без проверки на самой границе либо доказуемо эквивалентной атомарной связи проверки с disclosure/commit | EI-05, EI-08 |
| **SBC-03 — Shared-limit overspend** | Общий amount/use/rate/quorum limit использован повторно либо разрешён при partition/конкуренции, когда безопасный остаток доказать нельзя | EI-08, EI-09, EI-18 |
| **SBC-04 — False effect certainty** | `unknown`, `partial`, conflict, timeout, missing response/receipt либо неоднозначный эффект представлены как success/zero; retry/replay создаёт повторный commit; compensation скрывает или переписывает историю | EI-10–EI-13 |
| **SBC-05 — Security semantic loss** | Version, extension, profile, bridge или composition теряет обязательный security meaning, принимает unknown mandatory semantics, допускает silent downgrade, permit/non-permit ambiguity, effect ambiguity, assurance inflation или impersonation | EI-02, EI-03, EI-13–EI-18, EI-23 |
| **SBC-06 — Isolation or privacy escape** | Данные, metadata, linkability signal, состояние ветви, credential либо внешний/production effect выходят за frozen purpose, disclosure, tenant, branch, sandbox или egress boundary; нарушен classification/retention/deletion/residency/redaction bound с запрещённым хранением, раскрытием, корреляцией либо ложным claim удаления/редактирования | EI-04, EI-19, EI-25 |
| **SBC-07 — Unsafe resource behavior** | Hostile size/depth/rate, отказ или partition приводит к unbounded work/amplification/queue/retry/storage, deadlock без bounded safe-stop либо permissive fallback; точные ceilings задаёт OQ-5 | EI-20, EI-25 |
| **SBC-08 — Evidence integrity failure** | Evidence, provenance, causal history, artifact identity или security-relevant observation подделаны, скрыты, equivocated или повышают assurance без основания; кандидат уклоняется от наблюдения или делает результат некоррелируемым | EI-11–EI-13, EI-22, EI-25 |
| **SBC-09 — Independence or hidden dependency failure** | Положительный claim требует скрытой обязательной/закрытой зависимости, pair-specific mapping, общего protocol code или недоступных пояснений; кандидат скрывает/подменяет dependency либо candidate-caused действие приводит к несоответствию исполненного artifact/dependency closure проверенному manifest | EI-21–EI-23 |
| **SBC-10 — Candidate claim-boundary escape** | Candidate-specific скрытая assumption/dependency, omission/equivocation либо подтверждённая in-scope угроза, trust/dependency failure или обязательная комбинация выводит поведение кандидата за заявленный и проверенный safety boundary | EI-24, EI-25 |

Один finding может относиться к нескольким SBC. Таблица может быть сделана строже до freeze, но ни один класс нельзя удалить, сузить или превратить в неблокирующий без изменения финального PRD и повторного полного review.

## 3. Когда finding считается блокирующим

- Одного подтверждённого in-scope запрещённого события достаточно. Статистическое усреднение и повторный удачный запуск его не стирают.
- Не требуется реальный ущерб в production или повторение самого события: достаточно сохранённого raw trace с независимо подтверждённой candidate attribution, независимо подтверждённого counterexample либо reachable state в пределах frozen adversary/threat model. Неповторившееся и надёжно не объяснённое событие остаётся `inconclusive` и никогда не становится pass.
- Каждый security-critical expected outcome и каждый closure finding получают независимую derivation либо blind cross-check. Общий oracle codebase сам по себе независимость не доказывает.
- Finding привязывается к точным candidate artifact/version, scenario/vector, claim scope, environment, evidence и SBC ID. Нельзя переносить fail на другой artifact без проверки и нельзя переносить pass на более широкий scope.

## 4. Candidate failure и недостоверный эксперимент — разные вещи

| Ситуация | Вердикт |
| --- | --- |
| Qualified observer и независимый путь показывают запрещённое событие кандидата | `candidate-fail` + Safety Blocking Finding |
| Кандидат omitted/corrupted/equivocated обязательный сигнал либо сделал его некоррелируемым | `candidate-fail` + применимый SBC |
| Независимо доказано, что сломался observer, harness или environment, а не кандидат | `run-invalid`; это не pass и не finding кандидата |
| Из-за независимо доказанного сбоя harness/environment запущен неправильный artifact | `run-invalid`; при неустановленной причине — `inconclusive` |
| Причину или attribution надёжно установить нельзя | `inconclusive`; положительный claim закрыт |
| Ошибка найдена в frozen oracle/Charter/harness и влияет на смысл | Затронутая часть unfreeze; результаты приостанавливаются до review и rerun |
| Существенно неполна общая frozen threat model | Затронутая часть unfreeze; affected results `invalid/inconclusive`, затем scope-impact review и rerun; candidate-fail допустим только при независимо доказанной candidate-caused omission/equivocation |

Спор рассматривает outcome-blind adjudicator по raw evidence. Неразрешённый спор остаётся `inconclusive`, а не превращается голосованием в pass.

## 5. Severity rubric

Severity используется для отдельного hard gate, triage, сроков реакции, disclosure и масштаба retest. До эксперимента единый неизменяемый порог устанавливается на **High**: любой unresolved Critical или High finding является `other hard-gate failure` и запрещает положительный выбор, даже если finding не относится к SBC. После просмотра результатов нельзя менять порог или классификационные правила без новой preregistration и повторной проверки затронутых evidence.

| Severity | Значение |
| --- | --- |
| **Critical** | Системное, массовое либо cross-boundary воздействие; компрометация authority/trust root или ложное доказательство безопасности в широком масштабе |
| **High** | Существенное воздействие с ограниченным blast radius либо значимыми prerequisites |
| **Medium** | Локальное, редкое или сильно ограниченное воздействие при сохранении meaningful security consequence |
| **Low** | Минимальное воздействие, hygiene или documentation weakness |

`blocking_class=yes/no` определяется отдельно и раньше severity. Поэтому любой SBC блокирует при Critical, High, Medium или Low; дополнительно любой unresolved Critical/High finding блокирует по severity hard gate.

## 6. Исправление и повторная проверка

Safety Blocking Finding не удаляется из evidence history. Для закрытия нужны:

1. новый неизменяемый `artifact version + digest + dependency closure`; текстового patch или повторного удачного запуска недостаточно;
2. root-cause и scope-impact analysis;
3. повтор affected vectors, ближайших boundaries и frozen regression corpus по frozen OQ-3 rerun/replication rules, включая новый replication tranche после раскрытия holdout, когда применимо;
4. повторная observer/oracle calibration, если они затронуты;
5. независимое подтверждение closure и отсутствие нового SBC;
6. Decision Record со ссылками на старое и новое evidence.

До завершения всех шести шагов кандидат остаётся неeligible для положительного решения. Если все кандидаты заблокированы, результат — `Gate Closed / No Decision`, а не автоматическая победа другого направления и не автоматический `stop`.

## 7. Residual-risk acceptance

Нельзя принять как residual risk:

- любой SBC или другой hard-gate failure;
- неизвестность, возникшую из-за неполного observer/oracle/harness;
- риск, расширяющий claim за проверенный scope;
- исключение, нужное только одному кандидату после просмотра результатов.

Допустимо временно принять только явно описанный неблокирующий либо действительно out-of-scope риск. Запись обязана содержать scope, evidence, rationale, blast radius, compensating controls, owner, Security Reviewer, dissent, expiry/review trigger и влияние на публичный claim. Project Owner принимает риск только после рекомендации Security Reviewer; ни один участник не может принять его единолично или молча.

## 8. Responsible disclosure и emergency path

- Потенциальный SBC сначала получает versioned intake record, подтверждение получения и сохраняется в ограниченном evidence store для private triage без изменения raw evidence.
- Конфликт интересов требует recusal; затронутому advocate нельзя единолично классифицировать, закрывать или публиковать finding.
- Triage различает `candidate fix`, `protocol erratum`, `new version`, `observer/harness defect` и `new threat`; классификация не меняет исходный факт.
- Frozen policy и точные triage/notification clocks задаются OQ-5/OQ-8; затронутые maintainers, providers и implementers уведомляются в применимых пределах.
- Security Reviewer даёт документированную disclosure-рекомендацию. Project Owner решает о публикации только в рамках frozen disclosure policy, OQ-8 и применимых legal/coordination duties; публичное раскрытие не является условием немедленной приостановки опасного claim/artifact.
- Emergency action может приостановить claim, artifact или run, но не может тихо переписать норму, стереть dissent/evidence или дать одному vendor исключение.
- Restoration требует выполненного §6, явного нового Decision Record и обязательного post-incident review.

## 9. Freeze record и downstream closure

**Frozen scope OQ-4:** SBC-01–SBC-10, отдельный High severity hard-gate threshold, attribution/verdict rules, closure/retest, residual-risk acceptance и responsible-disclosure rules этого документа.

- **Authority/date:** Project Owner, 2026-09-13.
- **Independent review:** PASS; Critical 0, High 0, Medium 0, Low 0.
- **Change control:** редакционная правка допустима с audit note; смысловое изменение немедленно возвращает OQ-4 в `open` и требует нового независимого security review и принятия Project Owner.
- **Не разрешено этим freeze:** Candidate design, экспериментальный или production code, а также ослабление PRD/EI/OQ-3A.

Для полного Charter freeze следующие шаги обязаны операционализировать OQ-4 без изменения frozen safety policy:

- для каждого SBC есть route в OQ-3B vectors, oracle и минимум два независимых observer paths там, где возможен protected disclosure/effect;
- OQ-5 задаёт точные resource/privacy/time ceilings, не ослабляя SBC-06/SBC-07;
- OQ-6 назначает независимые роли и adjudication;
- OQ-8 задаёт storage, license/IP и disclosure controls;

Если downstream-работа требует смыслового изменения safety policy, OQ-4 автоматически снимается с freeze. До закрытия всех downstream slots весь Charter остаётся `draft / No Start`.
