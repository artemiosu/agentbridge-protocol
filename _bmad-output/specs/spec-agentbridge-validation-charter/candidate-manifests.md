# Candidate manifests — provisional

Ни один кандидат ниже не заморожен. Таблица фиксирует стартовые гипотезы из Gate 0, а не объявляет их победителями или обязательными зависимостями AgentBridge. Exact immutable references, licenses, implementations и glue должны быть проверены непосредственно перед freeze.

## Selection protocol

1. Сначала заморозить Ledger, oracle framework и search/inclusion rules.
2. По первичным источникам обновить standards landscape в пределах установленного freshness window.
3. Включать только end-to-end deployable конфигурации, способные претендовать на один и тот же frozen scope.
4. Для каждой конфигурации раскрыть новые objects, states, mappings, policy, storage, trust и runtime dependencies как custom glue cost.
5. Исключить доминируемую конфигурацию только с письменным Pareto rationale.
6. Выбрать Primary C лексикографически: hard-gate eligibility → Ledger coverage → минимальный новый нормативный glue → deployability/independence → practical cost.
7. Сохранить до двух остальных недоминируемых C для sensitivity H-1.

## Candidate N — Native AgentBridge

| Поле | Draft value |
| --- | --- |
| Manifest ID | `N-draft-0` |
| Normative role | Полноценный role-neutral semantic Core + необходимые Native Bindings/Profiles/Extensions для frozen scenarios |
| Forbidden hidden dependency | Любой обязательный внешний agent runtime, AgentBridge cloud/broker/registry или единственный provider |
| Lower-level reuse | Стандартные transport, cryptography, identity и policy primitives через явные Bindings |
| Core admission | Только Allocation + Removal Test против frozen Ledger |
| Current status | design-blocked до Ledger/oracle freeze |

## Provisional Composition Challenger Set

| ID | Назначение | Gate 0 baseline components | Почему может быть недоминируемым | Что требуется до freeze |
| --- | --- | --- | --- | --- |
| C-GENERAL | Наиболее полное общее agent/service/B2B покрытие | A2A 1.0.x; MCP 2026-07-28 surface; OpenAPI 3.2.0; Arazzo 1.1.0; применимые OAuth RAR/Token Exchange/DPoP, GNAP RFC 9635, AuthZEN 1.0 и evidence mechanism | Сильнейшее совокупное покрытие lifecycle, tools/API, authority/policy и evidence | Exact patch/commit/RFC refs; executable composition; полный glue/state/trust inventory |
| C-TOOL | Практичный tool/API-first baseline | MCP 2026-07-28; OpenAPI 3.2.0/Arazzo 1.1.0; применимый authority/policy/evidence stack | Может иметь меньшую runtime и integration сложность для service/tool flows | Доказать применимость ко всем заявленным topology classes либо ограничить claim и исключить по правилам |
| C-COMMERCE | Сильнейшее доменное возражение в commerce | UCP 2026-08-25; Agentic Commerce Protocol beta snapshot 2026-04-17; AP2 v0.2; применимые auth/evidence primitives | Самая зрелая проверка того, не дублирует ли Native commerce semantics | Exact refs и совместимость; определить, является ли end-to-end candidate или обязательным sensitivity/bridge case |

## Mandatory manifest fields

Для N и каждой C-конфигурации до freeze нужны:

- immutable component versions/commits и retrieval date;
- licenses, patent/IP status и allowed experimental use;
- normative surfaces и явно исключённые функции;
- required runtime components и failure domains;
- identity, authority, policy, evidence и trust assumptions;
- custom objects, state, glue, mappings и storage;
- supported topologies/modes/domains и claim boundary;
- implementations/libraries и shared genealogy;
- security hazards и known unresolved issues;
- build/dependency lock, SBOM и content digest;
- owner, reviewer и change history.

## Open decision

После freshness review Gate Chair должен либо подтвердить этот набор, либо заменить его по selection protocol. До этого OQ-1 открыт, Primary C не выбран, а Candidate-specific implementation запрещена.
