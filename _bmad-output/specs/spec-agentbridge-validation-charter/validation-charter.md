---
title: AgentBridge Gate 1 Validation Charter
status: superseded-retained-baselines
created: 2026-09-13
updated: 2026-09-15
superseded_by: ../spec-agentbridge-architecture-assurance/architecture-assurance-charter.md
governing_spec: SPEC.md
---

# Validation Charter

> **Историческая редакция.** Existential Gate 1 superseded 15 сентября 2026 года. Документ сохраняет доказательную историю и принятые policy baselines, но не определяет дальнейшую развилку проекта.

## 1. Простое объяснение

Этот Charter — договор о том, **как проверить идею до затрат на полноценную разработку**. Он не уменьшает замысел AgentBridge: Native Core должен быть спроектирован полноценно. Charter запрещает подгонять тесты под понравившийся результат и заранее определяет, какие доказательства разрешат продолжить проект, изменить его форму или остановить отдельный Core.

Текущий статус — **draft / No Start**. Это означает: структуру проверки уже можно обсуждать и уточнять, но писать экспериментальный код пока нельзя.

## 2. Решение Gate 1

Gate 1 отвечает на один вопрос:

> Существует ли у AgentBridge необходимая самостоятельная нормативная роль, которую нельзя безопасно и однозначно получить из актуальных стандартов, и даёт ли полноценный Native Core практически значимое преимущество без ухудшения безопасности, совместимости и реализуемости?

Допустимые итоговые направления:

| Исход | Простое значение | Что разрешается дальше |
| --- | --- | --- |
| `native` | Отдельный Core действительно нужен и практически оправдан | Architecture и нормативный draft только доказанного scope |
| `profile` | Нужна AgentBridge-композиция существующих extension points, но не отдельный runtime Core | Проектирование profile, mappings и conformance |
| `upstream` | Пробел лучше закрыть изменением одного существующего стандарта | Подготовка upstream proposal и reusable tests |
| `stop` | Безопасного и практически оправданного отдельного направления нет | Архивирование гипотезы до появления новых доказательств |
| `Gate Closed / No Decision` | Доказательств недостаточно или эксперимент недействителен | Исправление Charter/эксперимента; production остаётся запрещённым |

## 3. Обязательные входные условия

Перед сменой статуса на `frozen` должны быть выполнены все условия:

- OQ-1–OQ-8 имеют утверждённые ответы и owners.
- Evaluation Invariant Ledger имеет независимое основание и claim boundary для каждого элемента.
- Requirement Allocation Matrix покрывает каждый FR-1–FR-110 и NFR-1–NFR-29.
- Oracle schema, metamorphic properties, scenario templates и holdout-generation rule заморожены до candidate-specific design.
- Security-critical expected outcomes независимо выведены или blind cross-checked.
- Candidate N и Composition Challenger Set имеют immutable manifests; весь custom glue раскрыт.
- Safety Blocking Class из `safety-blocking-and-risk.md`, observer coverage и sandbox/egress controls приняты Security Reviewer и Project Owner.
- Primary endpoint, secondary endpoints, thresholds, margins, uncertainty zone и Statistical Analysis Plan утверждены до раскрытия comparative results.
- Роли, конфликты, genealogy/exposure rules и correction budget утверждены.
- Dependencies/IP/storage/publication boundaries проверены.
- Project Owner получил понятное объяснение последствий и подписал freeze.

Ноль незаполненных обязательных полей — жёсткое условие. Срок или бюджет не позволяют обойти его.

## 4. Этапы

### Gate 1A — Frame and map

1. Зафиксировать claim boundary и Evaluation Invariant Ledger.
2. Завершить Requirement Allocation Matrix и Removal Test rules.
3. Зафиксировать search/inclusion protocol, freshness window и Pareto selection.
4. Заморозить candidate-neutral oracle framework и coverage design.
5. Определить hard gates, metrics, budgets, roles, conflicts и invalidation rules.
6. Сформировать Candidate manifests без проектирования их внутреннего решения раньше Ledger/oracle.

### Expert Advisory Checkpoint

После аналитической части и до кода независимые экспертные перспективы оценивают:

- доказанность gap;
- силу Challenger Set;
- правдоподобие Native Candidate;
- безопасность и реализуемость проверки;
- ожидаемую стоимость и главные факты, способные опровергнуть проект;
- будущие adoption/governance риски.

Результат — `proceed`, `revise-research` или `pause-stop`. Совет не заменяет эксперимент и не отнимает решение у Project Owner.

OQ-7 разделяет две точки: ранний внутренний I1 `A0` после OQ-8 отвечает, стоит ли искать ресурсы на полную проверку; формальный независимый I2 `A1` после OQ-7B-Confirmatory/OQ-5B/OQ-6B-Confirmatory/OQ-3B/OQ-2B даёт мотивированную недирективную гипотезу финансирования Native-эксперимента до кода. Окончательный выбор направления остаётся результатом Gate 1.

### Gate 1B — Model and try to refute

- Построить coherent models N и C против замороженного oracle.
- Выполнить mapping, threat analysis, counterexamples и Removal Tests.
- Проверить решающие инварианты работающим PoC в изолированной среде.
- Разделить confirmatory и exploratory evidence.

Gate 1B может только предварительно опровергнуть Native-гипотезу и назвать provisional C-направление. Он не разрешает production и не может окончательно выбрать `profile/upstream` без Gate 1C.

### Gate 1C — Implement, conform and interoperate

- Создать минимум две независимые реализации каждого surviving кандидата.
- Для provisional `profile/upstream` создать минимум две независимые реализации frozen C composition semantics.
- Выполнить self-conformance, differential interoperability, cross-version, negative/adversarial, concurrency, fault и resource tests.
- Зафиксировать genealogy, prior exposure, помощь и ambiguities.

### Gate 1D — Measure and challenge

- Выполнить integration-cost, dependency/failure, performance/resource и evolvability experiments.
- Разделить common-substrate и best-deployable benchmarks.
- Открыть sealed holdout только по preregistered процедуре.
- Провести fresh-context security review и red team.
- Повторить затронутый scope после принятого blocking finding.

### Decide

Gate Chair применяет decision lattice из `measurement-and-decision.md`. Project Owner принимает Decision Record, но не может превратить failed hard gate в pass без нового Charter и rerun.

## 5. Неослабляемые hard gates

- Ноль Safety Blocking Findings по SBC-01–SBC-10; severity, performance, cost и aggregate score не отменяют этот запрет.
- Ноль запрещённых consequential effects и protected disclosures при подтверждённой полноте observation.
- 100% agreement по allowed-set и safety projection для independent implementations.
- Неизвестный, частичный, denied и incompatible outcome не превращаются в success.
- Authority повторно проверяется и связывается с точным irreversible commit.
- Общий лимит при partition не расходуется дважды; иначе только `non-permit/unknown`.
- Неизвестная security-relevant семантика, downgrade или lossy bridge приводит к fail-closed.
- Неполный observer, corpus, manifest, genealogy или evidence делает результат invalid/inconclusive.

## 6. Паритет кандидатов

- Одинаковые outcomes, threats, safety floor, scenario inputs и применимые observers.
- Одинаковые budgets исправлений и максимум циклов.
- Проверенные lower-level transports, cryptography, identity и policy mechanisms доступны обоим.
- Зрелость существующего tooling учитывается в practical cost, но не скрывает intrinsic burden.
- Native не штрафуется за явную общую семантику, если C повторяет её как pair-specific glue.
- Composition не штрафуется за число стандартов, если остаётся безопаснее и проще.
- Любое изменение contract/oracle после результатов требует нового preregistration.

## 7. Валидность, остановка и изменения

| Состояние | Когда применяется | Последствие |
| --- | --- | --- |
| `candidate disqualified` | Кандидат нарушил hard gate | Он не может быть положительным выбором в текущем run |
| `run invalid` | Нарушена процедура, observer или integrity данных | Затронутый результат не используется |
| `experiment paused` | Безопасность, ресурсы или внешняя зависимость требуют остановки | Анализ причины до продолжения |
| `Gate Closed / No Decision` | Evidence неполно, несопоставимо или inconclusive | Production остаётся запрещённым |
| `stop` | Валидные данные показывают отсутствие жизнеспособного направления | Отдельная гипотеза Core архивируется |

Изменение Charter после freeze создаёт новую версию, сохраняет старую, указывает причину и определяет, какие результаты аннулированы.

## 8. Freeze record

| Проверка | Owner | Статус |
| --- | --- | --- |
| Ledger и allocation полны | Gate Chair | open |
| Composition Challenger identities, boundaries и базовые версии выбраны | Gate Chair + Challenger Advocate | **OQ1-M1 frozen 2026-09-13; все independent reviews PASS** |
| Executable Candidate manifests, dependency closures и implementation pins заморожены до design/code | Gate Chair + Challenger/Native Advocates | open; blocked by OQ-3B/OQ-8 |
| OQ-3A verdict/inventory/schema/scenario/observer/holdout rules заморожены до Candidate design | Conformance Lead + Security Reviewer | **frozen 2026-09-13; blind review PASS 0 Critical/High** |
| OQ-3B exact corpus/calibration/sealed holdout заморожены до кода | Conformance Lead + Evidence Custodian | open |
| Safety Blocking Class и residual-risk model приняты | Security Reviewer + Project Owner | **OQ-4 frozen 2026-09-13** |
| Exact observer/sandbox model принят | Security Reviewer | open / OQ-3B, OQ-5, OQ-8 |
| OQ-5A measurement policy принята | Methodology + Fairness + Security Reviewers; Project Owner | **frozen 2026-09-13** |
| OQ-5B exact N/τ, teams, budgets, cells и schedules заморожены | Gate Chair + Project Owner | blocked by OQ-6/OQ-7/neutral pilot |
| OQ-6A role/access/independence policy принята | Governance + Fairness + Security Reviewers; Project Owner | **frozen 2026-09-13** |
| OQ-6B actual roster и I2 barriers доказаны | Gate Chair + Evidence Custodian | blocked by OQ-7/OQ-8 |
| OQ-7A budget/feasibility policy принята | Project Owner + Gate Chair | **frozen 2026-09-14; three independent reviews PASS; cash authorization USD 0** |
| OQ-7B-Pilot exact authorized pilot budget и reserve заморожены | Project Owner + independent budget/governance approver | blocked by OQ-8 |
| OQ-7B-Confirmatory exact Gate 1 budgets и resource limits заморожены | Project Owner + independent budget/governance approver | blocked by neutral pilot/OQ-5B/OQ-6B |
| Metrics/SAP/budgets приняты | Gate Chair + Project Owner | open |
| Independence/genealogy приняты | Gate Chair | open |
| OQ-8A rights/storage/disclosure/publication policy принята | Independent License/IP + Governance/Adoption + Security/Privacy Reviewers; Evidence Custodian + Project Owner | **frozen 2026-09-15; three independent reviews PASS** |
| OQ-8B-Pilot exact rights/stores/agreements заморожены | Evidence Custodian + independent IP/Security reviewers | blocked by OQ-8A |
| OQ-8B-Confirmatory exact dependency/IP/storage manifest заморожен | Evidence Custodian + independent IP/Security reviewers | blocked by pilot/candidate closure |
| Expert Advisory Record рассмотрен | Project Owner | open |
| Осознанное разрешение начать код | Project Owner | open |

Charter получает статус `frozen` только после закрытия всех строк. Сейчас OQ-2A, OQ-3A, OQ-1, OQ-4, OQ-5A, OQ-6A, OQ-7A и OQ-8A закрыты, но OQ-8B и остальные operational строки сохраняют `draft-no-start`.
