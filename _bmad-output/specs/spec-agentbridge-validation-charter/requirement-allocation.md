# Requirement Allocation Matrix — draft

Эта матрица обеспечивает индивидуальный учёт всех обязательных FR/NFR. Каждая строка получила предлагаемое primary allocation и связь с Ledger, но остаётся `proposed` до независимой проверки и принятия. Allocation указывает владельца нормативной обязанности, а не конкретный формат или компонент реализации.

| Requirement | Краткое название из PRD | Proposed allocation | Primary EI / rationale | Freeze status |
| --- | --- | --- | --- | --- |
| FR-1 | Role-neutral представление Участников | proposed Core semantic | EI-01 | proposed |
| FR-2 | Независимость Участника от Endpoint и transport-позиции | proposed Core semantic | EI-01 | proposed |
| FR-3 | Разделение Участника и Принципала | proposed Core semantic | EI-01 | proposed |
| FR-4 | Контекстные идентификаторы и минимальное раскрытие | proposed Core semantic | EI-19 | proposed |
| FR-5 | Динамические и многомерные Роли | proposed Core semantic | EI-01 | proposed |
| FR-6 | Независимые Audience, Resource и область применимости | proposed Core semantic | EI-01 | proposed |
| FR-7 | Многостороннее происхождение контекста | proposed Core semantic | EI-18 | proposed |
| FR-8 | Разделение Correlation и Causality | proposed Core semantic | EI-04 | proposed |
| FR-9 | Fail-closed при неоднозначном обязательном контексте | proposed Core semantic | EI-01 | proposed |
| FR-10 | Машиночитаемое объявление Capabilities | proposed Core semantic | EI-03 | proposed |
| FR-11 | Заменяемые механизмы Discovery | proposed Core semantic | EI-21 | proposed |
| FR-12 | Ограниченное и поэтапное раскрытие Capabilities | proposed Core semantic | EI-19 | proposed |
| FR-13 | Объявление не является разрешением или гарантией | proposed Core semantic | EI-03 | proposed |
| FR-14 | Явное согласование совместимого набора | proposed Core semantic | EI-02 | proposed |
| FR-15 | Различение обязательной и необязательной семантики | proposed Core semantic | EI-15 | proposed |
| FR-16 | Защита от скрытого downgrade | proposed Core semantic | EI-03 | proposed |
| FR-17 | Привязка Negotiation Outcome к взаимодействию | proposed Core semantic | EI-02 | proposed |
| FR-18 | Freshness, замена и отзыв объявлений | proposed Core semantic | EI-03, EI-23 | proposed |
| FR-19 | Явный и диагностируемый исход несовместимости | proposed Core semantic | EI-03 | proposed |
| FR-20 | Явный Interaction Mode и общий Контекст | proposed Core semantic | EI-04 | proposed |
| FR-21 | Request/Response без транспортных предположений | proposed Core semantic | EI-04 | proposed |
| FR-22 | Event и Subscription | proposed Core semantic | EI-04 | proposed |
| FR-23 | Stream с явными границами и backpressure | proposed Core semantic | EI-20 | proposed |
| FR-24 | Long-running и reverse asynchronous взаимодействия | proposed Core semantic | EI-04, EI-10 | proposed |
| FR-25 | Multi-party, branching и aggregation | proposed Core semantic | EI-18 | proposed |
| FR-26 | Переговоры без неявного обязательства | proposed Core semantic | EI-03 | proposed |
| FR-27 | Явные delivery и ordering semantics | proposed Core semantic | EI-04 | proposed |
| FR-28 | Координируемое завершение и прекращение работы | proposed Core semantic | EI-12 | proposed |
| FR-29 | Bounded failure и восстановление взаимодействия | proposed Core semantic | EI-20 | proposed |
| FR-30 | Явный authority gate и запрет неявного полномочия | proposed Core semantic | EI-06 | proposed |
| FR-31 | Точная привязка к Предмету решения | proposed Core semantic | EI-05, EI-08 | proposed |
| FR-32 | Монотонное сужение делегированных полномочий | proposed Core semantic | EI-06 | proposed |
| FR-33 | Непрерывность и целостность цепочки делегирования | proposed Core semantic | EI-06 | proposed |
| FR-34 | Защита от confused deputy и ambient authority | proposed Core semantic | EI-06 | proposed |
| FR-35 | Разделение Consent, Approval, авторизации и фактического эффекта | proposed Core semantic | EI-07, EI-10 | proposed |
| FR-36 | Многосторонняя композиция решений и separation of duty | proposed Core semantic | EI-07, EI-18 | proposed |
| FR-37 | Актуальность и повторная проверка полномочий | proposed Core semantic | EI-06, EI-08 | proposed |
| FR-38 | Защита от replay и ограниченное использование полномочия | proposed Core semantic | EI-09, EI-11 | proposed |
| FR-39 | Отзыв и явно ограниченная in-flight семантика | proposed Core semantic | EI-06, EI-12 | proposed |
| FR-40 | Ограничение последствий компрометации | proposed Core semantic | EI-06, EI-19 | proposed |
| FR-41 | Privacy-minimizing подтверждение полномочий | proposed Core semantic | EI-06, EI-19 | proposed |
| FR-42 | Сохранение полномочий и assurance в bindings и bridges | proposed Core semantic | EI-06, EI-16, EI-17 | proposed |
| FR-43 | Контекстно однозначная идентичность Операции | proposed Core semantic | EI-05, EI-11 | proposed |
| FR-44 | Явная и расширяемая lifecycle-семантика | proposed Core semantic | EI-10 | proposed |
| FR-45 | Разделение стадий принятия, исполнения и эффекта | proposed Core semantic | EI-10 | proposed |
| FR-46 | Явный Effect State и запрет ложной определённости | proposed Core semantic | EI-10 | proposed |
| FR-47 | Безопасные retries, deduplication и восстановление состояния | proposed Core semantic | EI-11 | proposed |
| FR-48 | Отмена, expiry и гонки завершения | proposed Core semantic | EI-12 | proposed |
| FR-49 | Compensation как отдельная Операция | proposed Core semantic | EI-12 | proposed |
| FR-50 | Жизненный цикл многосторонних и составных Операций | proposed Core semantic | EI-10, EI-18 | proposed |
| FR-51 | Freshness, provenance и конфликт наблюдений состояния | proposed Core semantic | EI-10, EI-13 | proposed |
| FR-52 | Структурированные ошибки с effect-aware семантикой | proposed Core semantic | EI-10 | proposed |
| FR-53 | Точная привязка Outcome Claim и Evidence | proposed Core semantic | EI-13 | proposed |
| FR-54 | Уровень assurance и граница внешней истины | proposed Core semantic | EI-13 | proposed |
| FR-55 | Восстанавливаемая история с privacy-boundary | proposed Core semantic | EI-13, EI-19 | proposed |
| FR-56 | Раздельная идентичность эволюционирующих слоёв | proposed Core semantic | EI-15, EI-23 | proposed |
| FR-57 | Совместимость определяется семантикой, а не формой | proposed Core semantic | EI-14 | proposed |
| FR-58 | Явная критичность и обработка неизвестной семантики | proposed Core semantic | EI-15 | proposed |
| FR-59 | Уникальность, неизменность смысла и lifecycle Extensions | Profile/Extension | EI-15, EI-23 | proposed |
| FR-60 | Core-инварианты и security floor не переопределяются | proposed Core semantic | EI-15 | proposed |
| FR-61 | Явные зависимости, конфликты и композиция Extensions | Profile/Extension | EI-15, EI-16 | proposed |
| FR-62 | Доменная специализация без универсальной онтологии | Profile/Extension | EI-14 | proposed |
| FR-63 | Явные границы межпрофильного взаимодействия | Profile/Extension | EI-14 | proposed |
| FR-64 | Согласование полного version/profile set и защита от downgrade | proposed Core semantic | EI-03, EI-15 | proposed |
| FR-65 | Управляемые deprecation, withdrawal и migration | proposed Core semantic | EI-15, EI-23 | proposed |
| FR-66 | Воспроизводимые нормативные ссылки и errata | conformance/experiment | EI-15, EI-23 | proposed |
| FR-67 | Evidence-based продвижение и защита от ossification | conformance/experiment | EI-15, EI-23 | proposed |
| FR-68 | Независимый Native Path | candidate-specific eligibility | n/a — Native-only eligibility; excluded from neutral Ledger | proposed |
| FR-69 | Нормативный Binding Contract | Binding/Bridge | EI-16 | proposed |
| FR-70 | Инвариантность смысла при смене Binding | Binding/Bridge | EI-16 | proposed |
| FR-71 | Полное покрытие security-critical представления | Binding/Bridge | EI-16, EI-19 | proposed |
| FR-72 | Явные Trust, data и enforcement boundaries | Binding/Bridge | EI-16, EI-19 | proposed |
| FR-73 | Согласуемая заменяемость механизмов и провайдеров | Binding/Bridge | EI-21 | proposed |
| FR-74 | Изоляция отказов и безопасный fallback | Binding/Bridge | EI-16, EI-20, EI-21 | proposed |
| FR-75 | Необязательность и наблюдаемость Compatibility Bridge | Binding/Bridge | EI-17, EI-21 | proposed |
| FR-76 | Полный, направленный и проверяемый Semantic Mapping | Binding/Bridge | EI-17 | proposed |
| FR-77 | Fail-closed при Semantic Loss | Binding/Bridge | EI-17 | proposed |
| FR-78 | Сохранение Authority, lifecycle и Evidence через bridge | Binding/Bridge | EI-06, EI-10, EI-13, EI-17 | proposed |
| FR-79 | Provenance преобразования и запрет скрытого impersonation | Binding/Bridge | EI-13, EI-17 | proposed |
| FR-80 | Ограниченная композиция и защита от bridge loops | Binding/Bridge | EI-17, EI-20 | proposed |
| FR-81 | Version drift и ограниченные compatibility claims | Binding/Bridge | EI-17, EI-23 | proposed |
| FR-82 | Точно ограниченный Conformance Claim | conformance/experiment | EI-22 | proposed |
| FR-83 | Открытый и независимо запускаемый Conformance Suite | conformance/experiment | EI-22 | proposed |
| FR-84 | Полнота покрытия нормативных обязательств | conformance/experiment | EI-22 | proposed |
| FR-85 | Проверяемая независимость реализаций | conformance/experiment | EI-22 | proposed |
| FR-86 | Differential Interoperability по семантическому результату | conformance/experiment | EI-14, EI-22 | proposed |
| FR-87 | Репрезентативная матрица ролей, режимов и топологий | conformance/experiment | EI-22 | proposed |
| FR-88 | Негативные, adversarial и mutation-проверки | conformance/experiment | EI-22 | proposed |
| FR-89 | State-model, concurrency и fault-injection проверки | conformance/experiment | EI-04, EI-10, EI-20, EI-22 | proposed |
| FR-90 | Проверка сквозных safety, privacy и resource invariants | conformance/experiment | EI-05–EI-20, EI-22 | proposed |
| FR-91 | Матрица версий, Profiles, Extensions, Bindings и bridges | conformance/experiment | EI-15–EI-17, EI-23 | proposed |
| FR-92 | Воспроизводимый verdict и диагностический evidence package | conformance/experiment | EI-13, EI-22 | proposed |
| FR-93 | Спецификация выше Reference Implementation | conformance/experiment | EI-22 | proposed |
| FR-94 | Проверяемое развитие самого Conformance Suite | conformance/experiment | EI-22, EI-23 | proposed |
| FR-95 | Ограниченная действительность и честные пределы Conformance | conformance/experiment | EI-22, EI-23 | proposed |
| FR-96 | Предварительно утверждённый Validation Charter | conformance/experiment | all applicable EI | proposed |
| FR-97 | Актуальный Composition Challenger Set | conformance/experiment | all applicable EI | proposed |
| FR-98 | Полнота, достаточность и независимость Native Candidate | conformance/experiment | EI-01–EI-24 | proposed |
| FR-99 | Симметричный и контролируемый эксперимент | conformance/experiment | all applicable EI | proposed |
| FR-100 | Общий cross-domain и cross-topology corpus | conformance/experiment | EI-01–EI-24 | proposed |
| FR-101 | Непреодолимые correctness, security и privacy gates | conformance/experiment | EI-05–EI-20 | proposed |
| FR-102 | Симметричная независимая реализуемость | conformance/experiment | EI-22 | proposed |
| FR-103 | Измеримая стоимость интеграции и семантическая сложность | conformance/experiment | EI-22 | proposed |
| FR-104 | Отказоустойчивость и стоимость обязательных зависимостей | conformance/experiment | EI-20, EI-21 | proposed |
| FR-105 | Воспроизводимые performance и resource benchmarks | conformance/experiment | EI-20 | proposed |
| FR-106 | Evolvability, migration и incremental deployment | conformance/experiment | EI-23 | proposed |
| FR-107 | Открытый воспроизводимый evidence package | conformance/experiment | EI-13, EI-22 | proposed |
| FR-108 | Независимый red-team и управление конфликтами интересов | conformance/experiment | EI-22 | proposed |
| FR-109 | Детерминированная функция решения `native/profile/upstream/stop` | conformance/experiment | EI-01–EI-25 | proposed |
| FR-110 | Falsification, invalidation и разрешённый следующий этап | conformance/experiment | EI-01–EI-25 | proposed |
| NFR-1 | Полнота и явность threat model | candidate-neutral invariant | EI-25 | proposed |
| NFR-2 | Fail-closed safety invariant | candidate-neutral invariant | EI-03, EI-06, EI-10, EI-15, EI-17 | proposed |
| NFR-3 | End-to-end целостность защищаемого смысла | candidate-neutral invariant | EI-05, EI-06, EI-10, EI-13, EI-16, EI-17 | proposed |
| NFR-4 | Least privilege и минимальное раскрытие | candidate-neutral invariant | EI-06, EI-19 | proposed |
| NFR-5 | Ограниченный blast radius и честные residual risks | candidate-neutral invariant | EI-06, EI-19, EI-21, EI-25 | proposed |
| NFR-6 | Проверенные security primitives и downgrade resistance | Binding/Bridge | EI-02, EI-03, EI-16, EI-25 | proposed |
| NFR-7 | Отсутствие обязательной центральной availability dependency | candidate-neutral invariant | EI-21 | proposed |
| NFR-8 | Явное восстановление после неопределённого сбоя | candidate-neutral invariant | EI-10, EI-11, EI-12 | proposed |
| NFR-9 | Ограниченность ресурсов и работы | candidate-neutral invariant | EI-20 | proposed |
| NFR-10 | Реалистичная time/order/partition модель | candidate-neutral invariant | EI-04, EI-09, EI-18 | proposed |
| NFR-11 | Детерминированность нормативного результата | candidate-neutral invariant | EI-14, EI-22 | proposed |
| NFR-12 | Preregistered performance budgets | conformance/experiment | EI-20 | proposed |
| NFR-13 | Производительность не покупается нарушением корректности | candidate-neutral invariant | EI-10, EI-20 | proposed |
| NFR-14 | Переносимость на различающиеся environments | candidate-neutral invariant | EI-22 | proposed |
| NFR-15 | Масштабирование без обязательной централизации | candidate-neutral invariant | EI-21 | proposed |
| NFR-16 | Независимость от языка, vendor и deployment form | candidate-neutral invariant | EI-22 | proposed |
| NFR-17 | Самодостаточность публичного контракта | candidate-neutral invariant | EI-22 | proposed |
| NFR-18 | Staged evolution без flag day | candidate-neutral invariant | EI-23 | proposed |
| NFR-19 | Ограниченная расширяемость без semantic drift | candidate-neutral invariant | EI-15, EI-23 | proposed |
| NFR-20 | Диагностируемость без информационной утечки | candidate-neutral invariant | EI-13, EI-19 | proposed |
| NFR-21 | Causal provenance и реконструкция | candidate-neutral invariant | EI-04, EI-13 | proposed |
| NFR-22 | Ненарушающее и воспроизводимое измерение | conformance/experiment | EI-13, EI-22 | proposed |
| NFR-23 | Контекстная минимизация данных | candidate-neutral invariant | EI-19 | proposed |
| NFR-24 | Представимость lifecycle данных без глобальной retention policy | Profile/Extension | EI-10, EI-13, EI-19 | proposed |
| NFR-25 | Защита secrets и чувствительных артефактов | conformance/experiment | EI-19, EI-25 | proposed |
| NFR-26 | Инвентаризация и проверяемость зависимостей | conformance/experiment | EI-16, EI-22, EI-23, EI-25 | proposed |
| NFR-27 | Открытая и royalty-free нормативная основа | candidate-neutral invariant | EI-22 | proposed |
| NFR-28 | Отсутствие скрытой коммерческой зависимости | candidate-neutral invariant | EI-21 | proposed |
| NFR-29 | Security change и claim invalidation | candidate-neutral invariant | EI-15, EI-23 | proposed |

## Допустимые allocation values

- candidate-neutral invariant;
- candidate-specific eligibility;
- proposed Core semantic;
- Profile/Extension;
- Binding/Bridge;
- conformance/experiment;
- later-stage.

Для каждого proposed Core элемента дополнительно обязателен Removal Test. Freeze разрешён только при нуле строк со статусом `unassigned` или `proposed`, при независимом review и наличии ссылки на positive/negative oracle, observer, scenario и evidence artifact.
