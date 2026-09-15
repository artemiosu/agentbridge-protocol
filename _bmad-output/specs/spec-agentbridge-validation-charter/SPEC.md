---
id: SPEC-agentbridge-validation-charter
status: superseded-retained-baselines
created: 2026-09-13
updated: 2026-09-15
superseded_by: ../spec-agentbridge-architecture-assurance/SPEC.md
companions:
  - validation-charter.md
  - evaluation-ledger.md
  - requirement-allocation.md
  - candidate-manifests.md
  - experiment-matrix.md
  - oracle-and-outcomes.md
  - atomic-coverage-inventory.md
  - scenario-corpus.md
  - observers-and-holdout.md
  - safety-blocking-and-risk.md
  - oq4-freeze-manifest.md
  - oq3-freeze-manifest.md
  - oq1-freeze-manifest.md
  - measurement-and-decision.md
  - oq5a-freeze-manifest.md
  - roles-evidence-and-governance.md
  - oq6a-freeze-manifest.md
  - budget-and-feasibility.md
  - review-budget-and-feasibility.md
  - oq7a-freeze-manifest.md
  - rights-storage-and-publication.md
  - review-rights-storage-and-publication.md
  - oq8a-freeze-manifest.md
  - open-decisions.md
  - review-evaluation-ledger.md
  - ../../planning-artifacts/prds/prd-agent-bridge-sdk-2026-09-12/prd.md
  - ../../planning-artifacts/research/technical-clean-slate-agentbridge-protocol-archite-2026-09-12/research.md
  - ../../planning-artifacts/research/technical-agentbridge-oq-1-composition-challenger-2026-09-13/research.md
sources: []
---

> **Исторический Charter.** 15 сентября 2026 года его развилка `native/profile/upstream/stop` superseded решением Project Owner о Native Architecture-First. Замороженные EI/SBC, safety, evidence, independence, IP и publication policies сохраняются как обязательные baselines. Этот Charter больше не управляет направлением проекта и не разрешает старый Candidate experiment/code.

# AgentBridge Gate 1 Validation Charter

## Why

AgentBridge претендует на самостоятельный, role-neutral протокольный Core для будущего агентного интернета. Публичное техническое исследование выявило вероятный незакрытый разрыв между существующими инициативами, но дало только среднюю уверенность. Нужен заранее зафиксированный и воспроизводимый эксперимент, который честно проверит, оправдан ли Native Core, либо ту же ценность безопаснее получить через профиль существующих стандартов, изменение одного upstream-проекта или отказ от отдельного Core.

## Capabilities

- **CAP-1 — Независимая основа сравнения**
  - **intent:** Команда фиксирует candidate-neutral Evaluation Invariant Ledger и распределяет каждое обязательное FR/NFR до проектирования кандидатов.
  - **success:** Каждый инвариант имеет независимое основание, наблюдаемый результат и границу claim; не остаётся ни одного нераспределённого FR/NFR.

- **CAP-2 — Сильнейшие честные альтернативы**
  - **intent:** Команда формирует актуальный Composition Challenger Set по заранее установленным правилам поиска и отбора.
  - **success:** Выбран реально развёртываемый Primary C и сохранены только недоминируемые условные альтернативы, для которых вся недостающая реализация считается стоимостью кандидата; версии, границы, glue и основания выбора раскрыты.

- **CAP-3 — Полноценная Native-гипотеза**
  - **intent:** Команда описывает внутренне целостный Native Candidate для всех доказанно общих инвариантов без искусственного ограничения его размера.
  - **success:** Каждый предложенный Core-элемент имеет allocation и проходит Removal Test; доменная и инфраструктурная семантика не попадает в Core без необходимости.

- **CAP-4 — Неподстроенные тесты**
  - **intent:** Команда фиксирует oracle, сценарии, expected outcomes и sealed holdout до candidate-specific design.
  - **success:** Все критические outcomes независимо выведены или перепроверены вслепую; изменение oracle аннулирует затронутый confirmatory result и требует новой preregistration.

- **CAP-5 — Безопасный эксперимент**
  - **intent:** Команда проверяет consequential semantics только в изолированной синтетической среде с полным наблюдением эффектов.
  - **success:** Нет реальных платежей, бронирований, юридических обязательств, production credentials/data или неучтённого egress; слепота observer всегда даёт invalid, а не pass.

- **CAP-6 — Независимая реализуемость**
  - **intent:** Независимые implementers реализуют frozen semantics без общей protocol-semantic codebase и закрытых пояснений.
  - **success:** Для каждого кандидата, требующего финальной проверки, две независимые реализации достигают 100% allowed-set и safety-projection agreement при нуле Safety Blocking Findings.

- **CAP-7 — Честные измерения**
  - **intent:** Команда раздельно измеряет внутреннюю сложность протокольной модели и практическую стоимость с лучшими существующими инструментами.
  - **success:** Preregistered estimands, одинаковые assurance/scope, common-substrate и best-deployable планы дают воспроизводимые результаты с заранее установленными uncertainty и non-inferiority rules.

- **CAP-8 — Экспертный совет до кода**
  - **intent:** Независимые экспертные перспективы оценивают аналитическую часть и объясняют владельцу проекта, стоит ли оплачивать практический эксперимент.
  - **success:** До кода создан Expert Advisory Record с рекомендацией `proceed`, `revise-research` или `pause-stop`, доказательствами, неопределённостью и dissent; окончательное решение остаётся за Project Owner.

- **CAP-9 — Детерминированное решение**
  - **intent:** Gate Chair применяет заранее зафиксированную последовательность `validity → eligibility → existing-surface → native → no viable direction`.
  - **success:** Gate 1 выдаёт ровно одно направление `native`, `profile`, `upstream` или `stop`, либо `Gate Closed / No Decision`; средний балл не компенсирует hard-gate failure.

- **CAP-10 — Проверяемая история доказательств**
  - **intent:** Команда сохраняет точные входы, реализации, запуски, отрицательные результаты и решения так, чтобы независимая сторона могла повторить вывод.
  - **success:** Evidence package имеет immutable manifests/digests, provenance, raw results и append-only index; публикация происходит только по отдельному разрешению Project Owner.

## Constraints

- Финальный PRD имеет приоритет; Charter может уточнять процедуру, но не ослаблять FR, NFR, hard gates, No Start или scope.
- Candidate-neutral Ledger EI-01–EI-25 frozen Project Owner 2026-09-13 после blind review с 0 Critical/High; смысловое изменение автоматически снимает freeze до повторного review и принятия.
- Composition Challenger identity frozen как OQ1-M1: C1 — Primary C, C2 — GNAP authority challenger, ANP 1.1 — reserve; AGNTCY проверяется отдельным ограниченным инфраструктурным экспериментом, но не становится обязательной runtime dependency.
- OQ-1 не разрешает проектирование или код: точные executable dependency closures, реализации, public mappings, licenses/SBOM и тестовые задания остаются заблокированы до зависимых решений OQ-3B/OQ-8.
- Native Core проектируется полноценно; существующие стандарты являются сильнейшими альтернативами и необязательными bridges, а не автоматически обязательными runtime-зависимостями.
- Описательное сравнение не может окончательно остановить Native-гипотезу. Требуется воспроизводимое executable/model evidence и работающий proof of concept решающих инвариантов.
- `native` нельзя выбрать раньше полного Gate 1A–1D. Окончательные `profile` или `upstream` требуют независимой реализации выбранной C-семантики.
- Rust, Go, Python, TypeScript, transport, encoding, cryptography provider, cloud и repository layout остаются переменными эксперимента, а не решениями Charter.
- Все consequential actions выполняются только в sandbox/simulator; production effects, credentials и customer data запрещены.
- Приватные исходные документы не входят в evidence package, Git или публичные материалы.
- Любой незаполненный обязательный параметр OQ-1–OQ-8 сохраняет `draft-no-start`.

## Non-goals

- Не создавать production-спецификацию AgentBridge, SDK, Architecture, cloud, registry, marketplace, payments или advertising.
- Не доказывать рыночный спрос, статус индустриального стандарта, финансовые прогнозы или будущую оценку компании.
- Не выбирать победителя по эстетике архитектуры, числу внешних протоколов, популярности, stars/downloads или предпочтению языка.
- Не изобретать собственную криптографию, глобальный identity provider, policy engine, payment rail или отраслевую систему истины.
- Не публиковать Charter, evidence или производные документы без отдельного разрешения владельца проекта.

## Success signal

Подготовительный этап завершён, когда все OQ-1–OQ-8 закрыты, каждый FR/NFR распределён, кандидаты и oracle заморожены до их реализации, независимые роли назначены, safety/evidence проверки пройдены и Project Owner осознанно меняет статус Charter на `frozen`. Успех самого Gate 1 — воспроизводимое обоснованное направление, даже если результатом станет не Native Core.

## Assumptions

- Gate 0 research от 2026-09-12 считается достаточно свежим для первого draft; перед freeze и каждым зачётным rerun применяется максимум 30 дней либо более строгое обоснованное окно.
- На первом этапе допустимо разделять конфликтующие функции независимыми fresh-context review, но это ограничивает claim и не заменяет внешнюю организационную независимость Gate 3.
- Точные числовые пороги Material Advantage, ресурсы и состав команды ещё не утверждены.

## Open Questions

- **OQ-2B:** Какие требования после freeze Ledger и oracle действительно требуют Native Core, а какие должны остаться в Profile, Binding/Bridge, conformance или более поздних слоях?
- **OQ-3B:** Какие exact vectors, datasets, expected outcomes, calibration cases и sealed holdout создать после OQ-4/OQ-5/OQ-8, но до OQ-2B, Candidate design и экспериментального кода?
- **OQ-4 downstream closure:** Как exact OQ-3B vectors/observers, OQ-5 ceilings, OQ-6 roles и OQ-8 controls операционализируют frozen SBC-01–SBC-10 без ослабления принятой safety policy?
- **OQ-5B:** Какие exact N/τ, team mix, budgets/costs, cells, schedules, tail operations и absolute bounds заморозить после OQ-6/OQ-7/neutral pilot и до OQ-3B?
- **OQ-6B:** Какие реальные identities/controllers/providers/stores и access-control tests обеспечат требуемый I2 после OQ-7/OQ-8?
- **OQ-7A:** Какой поэтапный бюджет и какие pause/re-scope rules допустимы без ослабления hard gates?
- **OQ-7B-Pilot / OQ-7B-Confirmatory:** Какие точные лимиты сначала разрешат neutral pilot, а после него — полный Gate 1 вместе с OQ-5B/OQ-6B?
- **OQ-8A:** Какие неизменяемые license/IP, storage, disclosure и publication правила обязательны?
- **OQ-8B-Pilot / OQ-8B-Confirmatory:** Какие exact licenses/terms, agreements, stores, regions, keys, retention и release boundaries действуют сначала для pilot, затем для полного Gate 1?
