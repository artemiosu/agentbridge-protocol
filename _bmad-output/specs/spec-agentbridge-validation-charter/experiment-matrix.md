# Experiment matrix — OQ-3A proposal

Статус: **blind re-review PASS; awaiting Project Owner re-acceptance; not frozen**.

Этот файл — индекс candidate-neutral экзамена. Нормативная детализация разделена между:

- `oracle-and-outcomes.md` — total verdict, allowed/forbidden outcomes, safety projection, metamorphic properties и EI coverage;
- `atomic-coverage-inventory.md` — неделимые MUST/safety obligations и closure rule;
- `scenario-corpus.md` — synthetic datasets, vector template и 65-cell shared-confirmatory template matrix;
- `observers-and-holdout.md` — OBS-01–OBS-12, sandbox, calibration и правило 39 sealed holdout vectors.

## Scenario families

Каждая семья выполняется для применимых N/C candidates с positive, boundary-negative, failure/concurrency, adversarial и metamorphic classes.

| ID | Topology/domain | Flow | Обязательная проверка |
| --- | --- | --- | --- |
| VC-1 | Cross-domain; participant↔participant | Discovery и negotiation | Bootstrap, transcript binding, versions/profiles/bindings, downgrade, stale/unknown mandatory semantics |
| VC-2 | Commerce; agent↔service | Ограниченный заказ без платежа | Exact subject, authority-to-commit, consent/approval/obligation, replay, lost response after simulated commit |
| VC-3 | Booking; agent↔service + reverse async | Изолированный hold/expiry/cancel | Long-running lifecycle, cancel/commit race, partial/unknown effect, compensation |
| VC-4 | Operations; service↔service/B2B | Симулированное maintenance/change action | Role reversal, policy boundaries, independent approval, idempotency, audit provenance |
| VC-5 | Cross-domain; agent↔agent | Трёхзвенная делегация | Attenuation, subdelegation, caveats, branch splicing, presenter binding, revocation descendants |
| VC-6 | Cross-domain; multi-party | Параллельные ветви и aggregation | Quorum/all-of, separation of duty, shared limits, split-view, Interaction Epoch, partial aggregate |
| VC-7 | Cross-domain; reverse async | Disconnect/restart/retention expiry | Operation recovery, replay after deletion, duplicate/reorder, security epoch, conflicting claims |
| VC-8 | Replaceable infrastructure | Замена discovery/identity/policy/audit provider | Binding properties, composition order, trust boundaries, outage isolation, unavailable critical state |
| VC-9 | Events/subscriptions/stream | Resume и slow consumer | Authority expiry, gaps/repeats, backpressure, bounds, termination, ordering scope |
| VC-10 | Version/profile/extension/binding | Staged upgrade и stack permutation | Mandatory-to-understand, unknown optional safety, non-commutativity, collision, deprecation, ossification |
| VC-11 | Compatibility bridges | Mapping к ≥2 внешним моделям, включая commerce | Semantic loss, authority/effect/evidence preservation, version drift, loop, claim scope |
| VC-12 | Все применимые | Compound adversarial/resource | Mutation, downgrade, confused deputy, replay storm, partition, crash, observer failure, egress, leakage, exhaustion |
| VC-13 | Cross-provider read-only | Получение и сопоставление данных | Machine-actionable semantics, provenance, comparability boundary, partial/unknown, privacy, mapping scope |

## OQ-3 split

- **OQ-3A — candidate-neutral exam contract:** до Candidate N/C design принять и freeze Oracle Record, Outcome Tuple/verdict, AC inventory, MP-01–MP-24, DS grammars, vector template, OBS contracts и holdout-generation rule.
- **OQ-3B — exact corpus:** после зависимых OQ-4/OQ-5/OQ-8, но до OQ-2B, Candidate design и любого экспериментального кода, создать/cross-check exact datasets/vectors/outcomes/calibration и sealed holdout.

## OQ-3A acceptance criteria

- Ни один outcome, dataset, MP, observer или holdout rule не требует Candidate N/C object model, wire shape, transport или языка.
- Каждому AC atom назначены required coverage types, metamorphic и observer routes; 65 cells — structural floor, не coverage proof.
- `unknown`, `invalid`, `non-permit`, `incompatible`, delivery, execution и effect не смешиваются.
- Запрещённый эффект не компенсируется статусом, производительностью или aggregate score.
- Observer uncertainty всегда даёт invalid/unknown согласно oracle, но не false zero.
- Holdout не содержит скрытой нормы, не доступен implementers и не переписывается после открытия.
- Security-critical expected outcomes требуют независимого blind cross-check.
- OQ-3B полностью завершается до Candidate design; все outcome-affecting degrees of freedom frozen заранее.
- Independent review не получает Candidate N/C design; Project Owner осознанно принимает исправленную редакцию до freeze.

До выполнения этих критериев OQ-3A и весь Charter остаются `draft-no-start`.
