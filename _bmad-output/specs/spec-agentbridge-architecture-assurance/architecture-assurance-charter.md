---
title: AgentBridge Architecture Assurance Charter
status: active-aa0-complete
created: 2026-09-15
updated: 2026-09-15
governing_spec: SPEC.md
---

# Architecture Assurance Charter

## 1. Простое объяснение

Native AgentBridge будет проектироваться. Этот Charter проверяет качество и безопасность конкретной архитектуры. Он может вернуть решение на переработку, сузить первую версию или заблокировать релиз, но не запускает заново обычное голосование «делать ли протокол».

## 2. Универсальное правило Gate

Каждый Gate имеет фиксированные входы, выходы и reviewers. Его результат:

- `pass` — все обязательные условия выполнены;
- `pass with conditions` — следующий этап разрешён только в явно ограниченном scope; conditions не могут содержать обязательный blocker;
- `redesign/block` — следующий этап запрещён до указанного исправления, scope reduction или нового evidence.

Blocker обязан назвать нарушенное требование, evidence, required correction и closure test. Любой SBC, Critical/High, mandatory gate failure, unresolved rights/license/patent/provenance, legal prohibition или отсутствие требуемых independence/evidence/conformance блокирует соответствующий artifact, use, release или claim.

## 3. Последовательность

### AA-0 — Course Decision — complete

Вход: утверждённое решение Project Owner.

Выход: новый PRD и этот Charter; existential Gate 1 superseded; retained policy baselines сохранены.

### AA-1 — Architecture Constitution

Входы:

- текущий PRD;
- Course Decision;
- retained FR-1–FR-95, NFR-1–NFR-29, EI-01–EI-25, SBC-01–SBC-10;
- approved clean-slate research и standards-gap evidence.

Обязательные результаты:

1. Architecture principles и non-negotiable invariants.
2. Граница Core/Profile/Extension/Binding/Bridge/external infrastructure.
3. Quality-attribute scenarios: security, privacy, correctness, resilience, performance, evolvability, implementability, decentralization.
4. Единый glossary и context boundaries.
5. Allocation Matrix FR/NFR/EI/SBC → responsibility + verification.
6. ADR template/register и change discipline.
7. Целостный scope Architecture Baseline v0.x.
8. Предварительные assets, adversary assumptions и trust/enforcement boundaries, достаточные для начала normative model.

Exit: 100% обязательных требований распределены; нет hidden central/commercial dependency или universal-domain ontology в Core.

### AA-2 — Abstract Normative Model

Обязательные результаты:

- participant/principal/role/audience/context model;
- capability/version/extension negotiation;
- interaction modes/correlation/causation;
- authority/delegation/consent/approval/revoke model;
- operation/lifecycle/effect/evidence/error state machines;
- retry/replay/cancel/expiry/partial/compensation/recovery semantics;
- reverse asynchronous и multi-party semantics;
- formal/executable verification plan and results for critical transitions.

AA-2 и AA-3 образуют контролируемую совместную итерацию: AA-2 создаёт модель на явно записанных threat/trust assumptions; AA-3 проверяет и уточняет их. Изменение trust/enforcement boundary автоматически повторно открывает затронутую часть AA-2.

Exit AA-2 становится окончательным только вместе с согласованным AA-3: нет противоречивых критических состояний; unknown/partial не превращаются в success; authority не расширяется; все critical transitions сопоставлены EI/SBC.

### AA-3 — Security, Privacy and Trust Architecture

Обязательные результаты:

- assets, actors, trust/data/enforcement boundaries;
- misuse/abuse cases, threat/failure trees;
- проверка authority на effect boundary либо доказуемая связь; если глобальная atomicity невозможна — explicit non-atomic/partial/unknown semantics;
- абстрактные signing/verifier requirements: какие смыслы и поля обязательно покрываются, mutation/ambiguity prohibitions, freshness и verifier obligations; конкретная canonical representation выбирается в AA-4;
- privacy, metadata, timing, linkability, retention/deletion model;
- compromise, key rotation, recovery, incident and supply-chain model;
- prevention/detection/recovery mapping для каждого SBC.

Exit: 0 unresolved SBC, Critical/High и mandatory failures в проходящем scope; residual acceptance только для неблокирующих рисков.

### AA-4 — Wire, Binding and Runtime Architecture

Documentary design разрешён. Executable model/prototype разрешается только после frozen Pre-Prototype Control Manifest: exact scope/classification, artifacts, direct/transitive dependencies, SBOM/licenses, stores/regions/keys/access/retention, synthetic-data rule, deny-by-default egress, secrets prohibition, resource ceilings, observer/sandbox qualification, named owner/reviewers, approval digest и expiry.

Обязательные результаты:

- сравнение transport/encoding/runtime candidates на общей модели;
- предварительный shortlist Native Bindings;
- кандидатная concrete canonical/signable representation;
- streaming/backpressure/fallback/cancellation behavior;
- aggregate CPU/memory/state/I/O/payload/depth/fan-out/retry/amplification/time ceilings;
- deterministic downgrade/unknown mandatory behavior;
- reproducible benchmark record только для свойств с уже определённым oracle; conformance divergence аннулирует соответствующий benchmark;
- provisional ADR shortlist для reference stack.

Exit: shortlist не основан на предпочтении или раннем private source; применимые correctness/security/interoperability guardrails пройдены. После AA-5 выбранные representation/Binding проходят scoped AA-3 security re-review, а окончательный ADR утверждается в AA-6.

### AA-5 — Conformance Architecture

Обязательные результаты:

- normative assertion catalog;
- positive/negative/golden vectors;
- property/fuzz/differential/concurrency/fault plans;
- observer qualification и evidence requirements;
- coverage map к FR/NFR/EI/SBC;
- scoped claims/expiry/revalidation rules;
- implementation-independent oracle skeleton.

Exit: неполный observer никогда не даёт pass; критические paths имеют negative/fault coverage; reference implementation не требуется для определения ожидаемого результата. AA-4 benchmarks пересчитываются/отклоняются при conformance divergence.

### AA-6 — Integrated Architecture Baseline

Обязательные результаты:

- cross-discipline review полной системы;
- Architecture Baseline v0.x и digest, включая окончательный ADR Native Binding/reference stack после AA-3 scoped re-review и AA-5 conformance checks;
- open-item register: `fix now`, `profile`, `defer`, `accepted non-blocking residual risk`;
- exact first-implementation scope;
- equal spec package, clean-room separation, reference-behavior access, genealogy/exposure и adjudication plan;
- applicable rights/dependency/SBOM/sandbox/egress/resource/observer/retention controls.

Exit: 0 unresolved mandatory blockers; Project Owner принимает только неблокирующие residual risks; разрешается AA-7 non-production implementation.

### AA-7 — Implementation, Interoperability and Hardening

Reference и spec-only implementations создаются без общей protocol-semantic codebase. Clean-room track начинает работу до reference behavior либо в доказуемой изоляции.

Exit: внутренний Implementation/Interop Candidate проходит applicable conformance, interop, security, resilience, performance, upgrade и genealogy checks.

### AA-8 — Public Draft Readiness

Требуются specification/code/conformance licenses, patent commitments, contribution provenance, trademark clearance, security disclosure, reproducible releases, minimal neutral governance, open proposals/decisions/dissent, recusal/appeal/emergency path, split control of keys/namespace/marks/oracle и publication authorization.

Exit: публичный `0.x Developer Preview`, но не production-ready или industry-standard claim.

### AA-9 — Limited Production Safety

Отдельный domain/jurisdiction safety, privacy, compliance и liability Charter разрешает только указанный production scope.

### AA-10 — External Adoption

Отдельный Adoption Charter проверяет unassisted external implementation, migration, retention и organizational/code/operational independence. Исходный floor: ≥4 independently controlled implementations/deployments, если Charter не обоснует более строгий порог.

### AA-11 — Open-Standard Legitimacy

Требуются широкое независимое adoption, работающий neutral change process, succession/continuity и objective stewardship milestones. Статус индустриального стандарта не self-declared.

## 4. Правила против затягивания

- Один review рассматривает заранее названный scope.
- Review выдаёт findings с severity, violated requirement, evidence, fix и closure test.
- Не более двух полных review cycles одного baseline; затем Chief Architect исправляет, сужает или откладывает зависимую возможность.
- Нерешённый обязательный blocker нельзя defer так, чтобы пройти Gate.
- Medium/Low исправляются либо получают owner/revisit condition/expiry.
- Dissent сохраняется и не удаляется, но не запускает бесконечный цикл сам по себе.
- Решение открывается заново только при material new evidence: vulnerability, formal contradiction, model/interop failure, material dependency/legal change.

## 5. Reference implementation boundary

До AA-6 reference implementation запрещена. Disposable model/prototype не считается reference, не получает compatibility promise и удаляется/архивируется по установленной policy.

Первый implementation идёт вертикальными срезами:

1. negotiation и safe refusal;
2. information exchange;
3. consequential action;
4. retry/replay/unknown outcome;
5. delegation/revoke;
6. reverse async или multi-party;
7. optional fail-closed bridge.

## 6. Emergency boundary

Emergency pause/stop проекта возможен при неустранимой небезопасности, юридической невозможности royalty-free Core, доказанной практически нереализуемой сложности либо невозможности независимой реализации. Недостаток ресурсов приводит к `pause / re-scope / seek resources / archive until funded`, а не считается техническим опровержением. Полный stop требует определённого evidence, применимой I2-рекомендации и отдельного решения Project Owner. Это аварийная граница, не регулярный повторный выбор направления.
