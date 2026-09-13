# Roles, evidence and governance — draft

## Roles

| Роль | Обязанность | Запрет/ограничение | Назначение |
| --- | --- | --- | --- |
| Project Owner | Утверждает Charter, budgets, residual risk, Decision Record и публикацию | Не превращает failed hard gate в pass | пользователь |
| Gate Chair / Method Steward | Контролирует preregistration, parity, completeness и decision lattice | Не выбирает победителя по предпочтению | open |
| Native Advocate | Создаёт strongest-good-faith N | Не судит единолично свой gap/defects | open |
| Challenger Advocate | Создаёт strongest-good-faith C Set | Не ослабляет scope/security; раскрывает glue | open |
| Independent Implementers | Реализуют frozen contracts и фиксируют effort/ambiguities | Не разделяют protocol-semantic code или закрытые инструкции | open |
| Security Reviewer | Определяет safety rubric и блокирует unsafe claims | Не выбирает продуктовый исход вне evidence | open |
| Conformance Lead | Владеет suite, coverage и triage | Не превращает reference behavior в норму | open |
| Evidence Custodian | Хранит manifests, raw data, exclusions, traces и IP/privacy controls | Не удаляет неудобные результаты и не публикует без разрешения | open |
| Fresh-context Red Team | Ищет counterexamples, stronger C и bias | Не меняет criteria post hoc | open |
| Expert Advisory Panel | Даёт понятную non-binding рекомендацию до кода | Не выдаёт мнение за Gate evidence | open |

Один человек может временно совмещать роли только с disclosure. Native Advocate, Challenger Advocate и final safety/review perspective должны быть разделены минимум независимыми fresh-context reviews. Это не считается внешней организационной независимостью.

## Independence and contamination policy

- Единица независимости N — frozen AgentBridge contract; C — frozen composition/glue semantics.
- Shared upstream libraries допустимы для C при раскрытой genealogy, но не доказывают независимость композиции.
- До assignment фиксируются prior experience, candidate/code/fixture exposure и доступные материалы.
- AI/tooling prompts, consultations, clarifications и copied fragments журналируются.
- Implementers получают одинаково доступный start/training package.
- Закрытое пояснение считается ambiguity и integration cost.
- Критическое расхождение требует blind adjudication или третьей реализации; иначе `inconclusive`.

## Correction policy

- Для N и C одинаковый correction budget и максимум циклов.
- Defect классифицируется до просмотра comparative metrics: implementation, harness, specification или hypothesis.
- Implementation/harness fix вызывает rerun затронутого scope и regression corpus.
- Contract/oracle/hypothesis change требует новой preregistration и не смешивается с прежними confirmatory results.

## Evidence package

Обязательные элементы:

- frozen Charter и decision rules;
- candidate/oracle/suite/data manifests и content digests;
- exact source/dependency versions, lockfiles/SBOM и build provenance;
- implementation genealogy, exposure и assistance logs;
- environment/calibration/time records;
- append-only raw-result index, traces и exclusions;
- statistical treatment, failure analysis и known limitations;
- security/red-team/advisory records и dissent;
- Gate 1 Decision Record.

Неблагоприятные и invalid results сохраняются с причиной. Если часть evidence нельзя раскрыть, claim явно сужается.

## Privacy, storage and publication

- Только synthetic/minimized data; production credentials и customer data запрещены.
- Private foundational files не копируются в manifests, evidence или Git.
- Frozen artifacts для implementers не означают публичную публикацию.
- До публикации обязательны secret, privacy, IP/license, trademark/name и claims checks.
- Публикация требует отдельного явного решения Project Owner.
- До соответствующих gates запрещены claims `industry standard`, `production ready`, `secure by default`, `10-minute integration` и подтверждённые market/financial numbers.

## IP baseline before experiment

До добавления dependency фиксируются license, source, version, patent/IP notice и право на экспериментальное использование. До первого публичного normative draft или внешнего contribution должны действовать specification/test licenses, contribution provenance, patent disclosure и применимое royalty-free/non-assert commitment без field-of-use discrimination.

Точные choices и owners остаются OQ-6 и OQ-8.
