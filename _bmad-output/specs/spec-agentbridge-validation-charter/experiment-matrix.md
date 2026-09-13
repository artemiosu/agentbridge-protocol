# Experiment matrix and oracle plan — draft

## Scenario families

Каждая семья выполняется для применимых N/C candidates с positive, negative, adversarial и compound-failure cases.

| ID | Topology/domain | Flow | Обязательная проверка |
| --- | --- | --- | --- |
| VC-1 | Cross-domain; participant↔participant | Discovery и negotiation | Bootstrap, transcript binding, versions/profiles/bindings, downgrade, stale/unknown mandatory semantics |
| VC-2 | Commerce; agent↔service | Ограниченный заказ без платежа | Exact subject, authority-to-commit, consent/approval/obligation, replay, lost response after simulated commit |
| VC-3 | Booking; agent↔service + reverse async | Изолированный hold/expiry/cancel | Long-running lifecycle, cancel/commit race, partial/unknown effect, compensation |
| VC-4 | Operations; service↔service/B2B | Симулированное maintenance/change action | Role reversal, policy boundaries, two-party approval, idempotency, audit provenance |
| VC-5 | Cross-domain; agent↔agent | Трёхзвенная делегация | Attenuation, subdelegation, caveats, branch splicing, presenter binding, revocation descendants |
| VC-6 | Cross-domain; multi-party | Параллельные ветви и aggregation | Quorum/all-of, separation of duty, partitioned shared limits, split-view, Interaction Epoch, partial aggregate |
| VC-7 | Cross-domain; reverse async | Disconnect/restart/retention expiry | Operation recovery, replay after deletion, duplicate/reorder, security epoch, stale/conflicting claims |
| VC-8 | Replaceable infrastructure | Замена discovery/identity/policy/audit provider | Binding properties, composition order, trust boundaries, outage isolation, unavailable critical state |
| VC-9 | Events/subscriptions/stream | Resume и slow consumer | Authority expiry, gaps/repeats, backpressure, bounds, termination, ordering scope |
| VC-10 | Version/profile/extension/binding | Staged upgrade и stack permutation | Mandatory-to-understand, unknown optional safety, non-commutativity, collision, deprecation, ossification |
| VC-11 | Compatibility bridges | Mapping к ≥2 внешним моделям, включая commerce | Semantic loss, authority/effect/evidence preservation, version drift, loop, claim scope |
| VC-12 | Все применимые | Compound adversarial/resource | Mutation, downgrade, confused deputy, replay storm, partition, crash, observer failure, egress escape, leakage, exhaustion |
| VC-13 | Cross-provider read-only | Получение и сопоставление данных | Machine-actionable semantics, provenance, comparability boundary, partial/unknown, privacy, no pair-specific adapters |

## Vector schema

Каждый concrete vector обязан содержать:

- immutable vector ID/version и связанные EI/FR/NFR;
- candidate-neutral preconditions, actors/roles, initial state и allowed-set;
- inputs, event/interleaving schedule и controlled faults;
- expected observable outcomes и explicitly forbidden outcomes;
- effect/egress channels, commit points, observer sources и detection window;
- privacy/resource ceilings и safe-stop condition;
- applicability/exclusion rationale по кандидатам;
- confirmatory или exploratory classification;
- oracle author, blind cross-checker и content digest;
- run validity и evidence artifacts.

## Oracle discipline

- Oracle schema, scenario templates, metamorphic properties, holdout-generation rule и decision owner замораживаются до candidate-specific design.
- Security-critical expected outcomes независимо выводятся либо blind cross-check; отсутствие cross-check даёт invalid/inconclusive.
- Candidate terminology не используется в expected outcome, если оно не является частью наблюдаемого wire claim.
- Новый red-team case создаёт отдельный versioned replication tranche и не пересчитывает старый confirmatory effect/CI.
- Любое изменение contract или oracle после результатов требует новой preregistration.

## Holdout and coverage

- Confirmatory corpus отделён от exploratory corpus.
- Sealed holdout формируется по risk-stratified rule и не содержит скрытых нормативных требований.
- Coverage учитывает `risk × feature × topology × mode × domain`.
- Каждый уникальный failure mechanism покрыт минимум одним vector; higher-order exclusion имеет rationale и ограничивает claim.
- Holdout открывается только Evidence Custodian после фиксации candidates, code digests и analysis plan.

## Observer and sandbox qualification

- До зачётного run перечисляются все effect/egress channels и commit points.
- Observer проходит positive-control/canary, mutation и failure calibration.
- External egress deny-by-default; разрешены только allowlisted synthetic endpoints и identities.
- Проверяются redirect, DNS, bridge escape, delayed effects и correlation ambiguity.
- Observer blindness, missing telemetry или ambiguous correlation инвалидируют cell/run.

## Required test methods

- Model/property/state tests.
- Mutation и malformed-input tests.
- Fuzzing в пределах безопасных resource caps.
- Deterministic concurrency/interleaving tests.
- Network/component fault injection.
- Cross-version and extension greasing.
- Differential interoperability.
- Recovery, retention-expiry и replay tests.

Точные vectors и datasets остаются OQ-3; этот файл определяет обязательную форму, но пока не закрывает вопрос.
