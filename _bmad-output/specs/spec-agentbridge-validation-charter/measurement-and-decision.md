# Measurement and decision plan — draft

## 1. Eligibility before comparison

Кандидат сравнивается по преимуществам только после прохождения применимых correctness, safety, privacy и independent-implementability hard gates. Aggregate score запрещён.

Неизменяемые gates:

- 0 Safety Blocking Findings;
- 0 forbidden effects/disclosures при подтверждённой полноте observation;
- 100% allowed-set и safety-projection agreement независимых реализаций;
- 100% покрытие обязательных normative rules применимыми positive/negative tests;
- reproducible candidate, oracle, environment и raw-result manifests;
- отсутствие обязательной скрытой runtime/provider зависимости для Native Path.

## 2. Primary and secondary endpoints

### Primary endpoint

Повторяемая совокупная стоимость достижения **safe semantic interoperability** для frozen scope.

Она измеряется раздельно:

1. **Intrinsic greenfield burden** — новая semantic/operational сложность на общем минимальном substrate.
2. **Practical current-deployment cost** — фактическая стоимость с лучшими публичными SDK/tooling каждого кандидата.

Для обоих estimands отдельно учитываются active effort, elapsed waiting, clarification/help, retraining, rework, custom glue, mandatory services и ambiguity/defect resolution. Малое внутреннее число implementers даёт только case-level evidence.

### Максимум два key secondary endpoints

- Operational failure/dependency burden: обязательные components, failure domains, recovery paths, blast radius и version matrix.
- Evolvability/deployment burden: staged upgrade, new profile/extension, provider replacement, upstream drift, rollback и migration coordination.

Точные practically-significant thresholds и non-inferiority margins — OQ-5 и должны быть frozen до реализации.

## 3. Performance/resource plans

### Common-substrate

Изолировать стоимость Core semantics при одинаковых transport, encoding, cryptography, hardware/network и workload.

### Best-deployable

Дать каждому кандидату лучшие разумно доступные production-like tooling и настройки при одинаковых correctness/security obligations.

Метрики: p50/p95/p99/p99.9 latency, throughput/QPS per core, CPU, RSS, allocations, wire bytes, setup/handshake, recovery time и saturation behavior. Быстрый unsafe/incorrect result не учитывается.

Минимальная environment matrix наследует Gate 0: cold/warm/resumed; несколько RTT/loss/reorder режимов; unary/stream/fan-out/long-idle; payload size/depth/unknown/hostile; slow consumer; failure before/after simulated effect; bounded retry. Exact values требуют OQ-5.

## 4. Statistical Analysis Plan requirements

До кода зафиксировать:

- estimand и unit of analysis для каждой метрики;
- sample-size/precision rationale;
- independent run blocks, warm-up и randomization/counterbalancing;
- exclusions, censored/failure handling и missing-data rules;
- confidence intervals для tail и integration measures;
- practically-significant threshold, non-inferiority margins и uncertainty zone;
- multiplicity control для максимум двух secondary endpoints;
- повторный run criterion и совместимость effect estimates;
- raw-data retention и immutable analysis version.

Если confidence interval пересекает decision boundary, результат остаётся `Gate Closed / No Decision`; дополнительные запуски не могут задним числом менять правило.

## 5. Decision lattice

1. **Validity:** incomplete, contaminated, incomparable или irreproducible evidence → `Gate Closed / No Decision`.
2. **Eligibility:** hard-gate failure → соответствующий кандидат disqualified.
3. **Existing-surface resolution:** если жизнеспособная C закрывает весь Ledger без нового runtime Core:
   - `upstream`, если change set целиком помещается в один признанный extension surface и не требует AgentBridge-maintained normative composition;
   - иначе `profile`, если нужна открытая AgentBridge-maintained composition/mapping/conformance.
4. **Native resolution:** `native` только если каждая жизнеспособная C воспроизводимо провалена хотя бы по одному общему инварианту, а N проходит H-1–H-7 и Material Advantage.
5. **No viable direction:** `stop` только при валидном evidence, когда N не проходит и ни upstream, ни profile не дают безопасного практически оправданного решения.

Tie-break: когда одновременно технически возможны upstream и profile, непосредственным направлением является upstream; profile записывается только как неавторизующая fallback-рекомендация.

## 6. Early-exit limits

- Gate 1B может выдать только `Native hypothesis falsified` и provisional C direction после работающего PoC и воспроизводимого adversarial evidence.
- Окончательные `profile/upstream` требуют двух независимых реализаций выбранной C semantics и применимых hard gates в Gate 1C.
- Ранний `stop` допустим только по заранее определённому falsifier, не зависящему от непроверенной C implementability.
- `native` всегда требует полного Gate 1A–1D.
- Budget exhaustion означает pause/re-scope или No Decision, но не `stop` и не pass.

## 7. Decision Record

Финальный record содержит Charter/version, candidates, hypotheses, outcomes, hard-gate status, raw-evidence links, uncertainty, dissent, invalidations, claim boundary и ровно одно разрешённое следующее направление. Project Owner утверждает решение и любую публикацию.
