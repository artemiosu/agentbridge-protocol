# OQ-5 — Measurement and decision plan

Статус: **OQ-5A policy frozen by Project Owner on 2026-09-13**. Methodology, fairness and security reviews: PASS, везде 0 findings. OQ-5B operational freeze остаётся будущим шагом; документ не разрешает Candidate design или код.

## 1. Простое правило и намеренная policy

Сначала кандидат проходит безопасность, корректность и независимую реализуемость; только затем сравниваются усилия и производительность. Weighted score запрещён.

По принятому PRD действует **necessity-first policy**: новый Native Core рассматривается только если существующие открытые extension/profile surfaces не могут безопасно и однозначно закрыть общий gap. Даже намного более быстрый Native не выбирается как новый Core, если открытый `upstream` или `profile` полностью решает задачу. Это намеренный барьер против создания лишнего стандарта, а не нейтральный конкурс красоты; Project Owner должен отдельно понимать и принять эту policy.

Если H-1 пройден, практически значимая разница равна 20%, и вся 95% confidence interval должна находиться за границей. Пересечение границы даёт `Gate Closed / No Decision`.

## 2. Eligibility

До comparative analysis требуются: ноль SBC-01–SBC-10; ноль unresolved Critical/High; ноль forbidden disclosures/effects при валидных observers; 100% allowed-set/safety-projection agreement; полный mandatory coverage; reproducible manifests; applicable candidate eligibility. Failure дисквалифицирует artifact/scope, но не доказывает победу другого. `invalid`, `inconclusive`, timeout и noncompletion не превращаются в низкую стоимость.

## 3. Primary estimand

### 3.1 Единица и population

- **Randomization/analysis unit:** независимая команда, реализующая один и тот же полный frozen claim scope. Slices — clustered observations внутри команды, не самостоятельные units.
- **Population:** все рандомизированные команды по intention-to-treat (ITT). Candidate-specific обязательства добавляют работу и не удаляют общий task scope.
- **Primary contrast:** Candidate N против frozen Primary C1 из OQ1-M1.
- **Coverage components:** commerce agent↔service, booking async, non-commerce B2B, agent↔agent/multi-party и cross-version/bridge входят в полный scope каждой команды; они не являются отдельными статистическими units и не получают искусственного распределения shared effort.
- **Aggregate:** primary outcome — полные person-hours каждой команды. Experience blocks используются для assignment и заранее фиксированного covariate adjustment; arm contrast — adjusted mean difference of team-level `log(RTTSI)`, exponentiation даёт ratio `R=N/C1`.

### 3.2 RTTSI — решающее измерение

**Restricted Time to Safe Interoperability (RTTSI)** — все candidate-required human person-hours от получения frozen package до первого воспроизводимого safe pass полного acceptance set в пределах frozen horizon `τ`.

Включаются implementation, design, configuration, custom glue, integration, security/conformance review, debugging, clarification, rework, candidate-specific coordination и operations work. Elapsed waiting, machine time и direct monetary cost записываются отдельно. Unattended time не маскирует активный труд.

Каждая assigned team входит в estimator:

- safe completion до `τ` → фактические person-hours;
- candidate-caused noncompletion/withdrawal после exposure → Material Advantage для этого кандидата не доказана; capped `τ` сохраняется только для описательного sensitivity и не смешивается с завершившими командами;
- hard safety failure → eligibility fail, не числовое преимущество;
- independently proven harness failure → outcome-blind replacement по §11; до замены block invalid;
- administrative withdrawal, не связанный с кандидатом → один заранее разрешённый matched replacement; sensitivity assigns `τ`;
- unknown attribution → inconclusive, не complete-case exclusion.

До effort superiority действует completion gate: каждая non-replaced ITT team обязана получить safe pass до `τ`. Любая candidate-caused noncompletion закрывает H-6 для соответствующего кандидата; быстрые завершения это не компенсируют. Точный `τ` выбирается по outcome-blind neutral pilot и замораживается до Candidate package exposure; одинаков для N/C1 и не ниже p90 neutral-pilot completion time с headroom 50%.

### 3.3 Два estimands и boundaries

1. **Primary practical:** best-deployable RTTSI с заранее pinned публичным tooling/dependency closure.
2. **Required sensitivity:** common-substrate RTTSI при одинаковом lower substrate и равном effective security/privacy/observer floor. Если floor сохранить нельзя, estimand `inapplicable/inconclusive`, а не выполняется с ослабленной защитой.

Для `R=RTTSI_N/RTTSI_C1`:

- Native material advantage: upper 95% CI primary `≤0.80`; common-substrate upper CI `≤1.10`.
- C1 material advantage: lower 95% CI primary `≥1.25`; common-substrate lower CI `≥1/1.10`.
- Practical equivalence: entire primary CI strictly inside `(0.80, 1.25)` и ни одна superiority boundary не достигнута.
- Equality boundary принадлежит superiority rule; любое иное пересечение — uncertain.

Sensitivity report показывает 10/15/25/30%, но decision не меняет. Не хватает общего safe scope или precision — H-6 не доказана. Open, versioned, independently implementable profile/mapping входит в RTTSI C1, но сам по себе **не доказывает H-1**. H-1 evidence дают pair-specific/hidden agreements, обязательный дополнительный runtime либо невозможность open profile/upstream в ≥2 доменах и ≥3 topology classes.

## 4. Challenger and tooling fairness

- C1 — единственный confirmatory deploy-today contrast.
- C2 — заранее фиксированный authority-plane sensitivity/falsification analysis; не заменяет C1 post hoc и не входит в deploy-today RTTSI до полного eligible manifest. Если C2 станет вторым confirmatory contrast, требуется новая preregistration и closed-testing multiplicity rule.
- AGNTCY — отдельный bounded augmentation/substitution A/B для C1. Его effort, dependencies и failures учитываются, но он не подменяет C1 и не входит в primary ratio.
- Если AGNTCY меняет C1 RTTSI point estimate минимум на 10%, mandatory dependency/failure topology либо hard-gate outcome, OQ-1 автоматически unfreeze и C1 manifest выбирается заново до confirmatory run. Иначе это sensitivity evidence; post-hoc substitution запрещена.
- ANP активируется только новым OQ-1 version/freshness review.
- Успех C2 в authority-plane блокирует claim об irreducible authority gap и требует полного C1-with-GNAP closure/retest; провал C2 относится только к authority plane и не опровергает Composition целиком.
- До assignment замораживаются tooling cutoff date, exact versions/digests, paid access, configuration search/tuning budget, candidate-package preparation budget, training/help package и advocate sign-off. Переключение после результатов запрещено.
- Новый Native authoring/tooling effort и накопленный C1 ecosystem effort показываются отдельно; best-deployable вывод ограничен датой эксперимента и не объявляется внутренним качеством архитектуры.
- Neutral competence task, prior-exposure questionnaire и matched/block randomization предшествуют assignment. Замораживаются imbalance tolerance, одинаковые training/help budgets, AI-assistance policy/log и blind adjustment model.

## 5. Secondary endpoints

### SE-1 — Safe recovery

Для каждого assigned fault: origin — injection time; event — oracle-allowed externally observed safe stable state, сохраняющийся frozen stability window; horizon — scenario deadline. SE-1 — одна последовательная composite hypothesis: (1) 100% non-replaced assigned faults в каждой mandatory cell достигают allowed state до horizon; candidate-fail/unsafe запускает OQ-4, а unresolved invalid/inconclusive after replacement даёт No Decision; (2) затем conditional p95 harm ratio claimant/other имеет upper one-sided 95% CI `≤1.25` во всех cells. Superiority требует upper two-sided 95% CI `≤0.80`. P95 не усредняется между fault families/environments. Эта composite hypothesis — один Holm family member; SE-2 — второй.

### SE-2 — Staged-change RTTSI

Scenario-specific human effort по frozen matrix: staged upgrade, rollback, provider replacement и upstream drift. Общие cases используют одинаковое semantic change; candidate-specific upstream drift учитывается отдельно. H-7 требует прохождения всей матрицы и не выводится из одного bundled change.

Для RTTSI secondary применяются boundaries §3.3; Holm correction контролирует два secondary tests. Secondary не компенсируют primary miss.

## 6. Cellwise performance/resource guardrails

Каждая mandatory `environment × workload × state` cell проходит без компенсации другими cells. Harm ratio нормализуется для claimant: cost-like `claimant/other`, throughput `other/claimant`. Upper preregistered one-sided 95% CI обязан быть не выше margin:

| Guardrail | Margin |
| --- | ---: |
| p99 latency | 1.20 |
| p99.9 latency | 1.20 |
| inverse throughput/core | 1.15 |
| CPU/operation | 1.20 |
| peak RSS и retained state | 1.20 |
| allocations и wire bytes | 1.25 |
| setup/handshake/reconnect | 1.25 |

Это intersection-union gate: пройти должны все cells/guardrails, поэтому отдельная multiplicity correction для утверждения совместного pass не нужна. Assigned/accepted operations, а не только completed, образуют denominators; dropped/timeout/unknown/failed остаются в frozen two-part estimator. Safety cells и absolute ceilings §8 проходят раньше относительного сравнения.

Минимальная operational feasibility для warm E1, 1 KiB, concurrency 32: p99 processing overhead `≤100 ms`, p99.9 `≤250 ms`, throughput `≥100` safely accepted operations/s/core. В остальных mandatory cells exact absolute bounds определяются из frozen deadline/resource envelope OQ-5B; до их заполнения practical justification не выдаётся.

## 7. Environments and cell design

Run manifest фиксирует hardware, OS/kernel, runtime/compiler, dependencies, power, CPU pinning, UTC и background load.

- E1: RTT 0.5 ms, loss/reorder 0%; E2: 20 ms, 0.1%/0.1%; E3: 100 ms, 1%/0.5%; E4: 250 ms, 3%/1% и frozen partition schedule.
- cold/warm/resumed; concurrency 1/32/256; control payload 1 KiB/64 KiB/1 MiB inclusive; unary/stream/fan-out/long-idle/slow-consumer/failure-before-and-after-simulated-effect.
- Safety boundaries receive full coverage. Performance uses a preregistered balanced fractional design covering every main effect and named two-way interactions; no extrapolation creates pass for an unmeasured mandatory cell.
- OQ-3B freezes the exact mandatory cell table, packet schedules, partition duration, block duration, operations/block and tolerances before Candidate design.
- Machine inference requires simulation-validated nested/block bootstrap, ≥30 independent blocks per measured cell, frozen reset/seed/stationarity rules and enough effective observations for each tail precision target. Insufficient p99/p99.9 precision gives uncertainty.

## 8. Aggregate safety ceilings

Ceilings are inclusive (`≤` accepted; first `>` safe-stops before proportional work). Они относятся к experiment profile, не универсальному production Profile.

| Boundary | Absolute ceiling |
| --- | --- |
| Control message / nesting | 1 MiB / 32 levels |
| Chain / fan-out | 8 hops / 64 active branches |
| Endpoint concurrency / buffered items | 256 / 1,024 |
| Whole Operation Identity retries | 3 total across all hops/branches |
| Whole operation messages / wire amplification | 256 messages / `≤4 × accepted input bytes + 256 KiB` |
| CPU work | 2 core-s normal; 10 core-s hostile-boundary operation |
| Process memory / retained interaction state | 2 GiB RSS / 64 MiB per interaction and 4 GiB per endpoint store |
| Sync new-work window | 30 s; async 24 h simulated deadline |
| Performance block / safety case wall time | 30 min / 10 min real, excluding declared simulated time |
| Supplementary diagnostic event/projection | 64 KiB / 16 MiB per run |
| Mandatory evidence event/count/rate | 16 KiB / 1,024 records per Operation Identity / 64 MiB/s per run |
| Mandatory evidence retained | 16 MiB per operation / 4 GiB per cell / 64 GiB per run |
| Unlisted disclosure/egress; production secrets/data/effects | exactly 0 |

Automatic retry uses the same Operation Identity and is allowed only with proven no-commit or replay-safe semantics. Unknown effect forbids new execution until explicit reconciliation. Timeout stops only new work/egress, remains unknown/partial, preserves late-effect observation and never asserts rollback or established-none.

Budgets compose monotonically: each accepted pre-boundary increment cannot reduce safety, raise assurance or bypass another budget. Candidate-caused exceedance is `candidate-fail`/SBC-07; independently proven harness cause is `run-invalid`; unknown cause is `inconclusive`. Safe-stop preserves mandatory evidence.

Only supplementary diagnostics may truncate with an explicit marker. Normative observer/raw audit evidence is immutable and never truncated; anticipated loss pauses before loss, and any gap follows OQ-3A attribution.

Перед run резервируется весь frozen evidence budget плюс 20% reserve. На первой границе прекращаются admission и новая candidate work до потери записи, retained prefix sealed, overflow отмечается. Dropped/gapped mandatory record никогда не даёт pass: candidate-caused flood/evasion → SBC-07 и при применимости SBC-08; proven harness underprovision/failure → `run-invalid`; unknown cause → `inconclusive`. Retention duration задаёт OQ-8 отдельно.

Best-deployable tooling uses pinned offline closure where possible; only synthetic allowlisted endpoints, no telemetry/update egress and no production credentials/data/effects. Violation follows SBC-06/SBC-09.

## 9. Privacy and observer qualification

For every `claim/profile/binding/channel/commit point`, OQ-3B freezes allowed disclosure set, audience, purpose, data class, metadata/timing/error/linkability bounds, retention/deletion/redaction/residency, detection window and correlation. Allowlisting a channel never authorizes its content.

- Deterministic forbidden-effect/disclosure canaries: exhaustive applicable mutation set and **0 false negatives**; any miss blocks the observer/channel.
- Когда deterministic proof невозможен: отдельно для каждого independent observer path × channel × commit point минимум 300 seeded positive injections и 300 negative controls; one-sided exact Clopper–Pearson 95% lower sensitivity bound ≥99% и upper false-positive bound ≤1%. Pooling допустим только по preregistered proof of exchangeability. Все проверки образуют intersection-union gate; miss блокирует dependent cell.
- Crash/restart continuity: 100 injected continuity cases, 0 silent gaps; saturation calibration at 80%, 100% and first-over-ceiling; non-interference control must preserve allowed Outcome Tuple.
- Missing/failed calibration slot blocks dependent cells. Для protected disclosure/effect сохраняются два failure-domain-independent paths.

Holdout — coverage gate: 39/39 valid cases must pass; any `candidate-fail` fails tranche. После outcome-blind authorized replication unresolved `invalid/inconclusive` даёт `Gate Closed / No Decision`; holdout miss не усредняется.

## 10. Sample-size and precision plan

- Four units/candidate remain only interoperability feasibility floor, **not** inferential sample size.
- Outcome-blind neutral pilot и conservative variance envelope без Candidate N/C exposure питают simulation. SESOI/null boundary остаётся `R=0.80`; design alternative — `R_alt=0.65` (для C1 reciprocal `1/0.65`). Simulation fixes `τ`, per-arm N и experience-block allocation для ≥90% probability подтвердить boundary при `R_alt`, two-sided α=0.05, validated ≥95% CI coverage и target CI ratio width ≤1.20.
- Inferential floor is 12 independent teams/arm; maximum 24/arm. Cluster bootstrap is forbidden below 12 and must pass simulation diagnostics. If required N exceeds 24 or OQ-6/OQ-7 cannot support it, scope is reduced and re-reviewed or result is No Decision; rigor is not lowered.
- OQ-6 фиксирует одинаковый team size/role mix. Exact N/τ замораживаются до candidate assignment. Same team не реализует оба candidates. Reused implementers в SE-2 declared and modeled as repeated measures.
- Tail design uses harm-margin 1.20 as null и `1.05` как design alternative, ≥90% simulated power, ≥95% CI coverage и CI ratio width ≤1.15 для p99 и p99.9 отдельно. Если effective tail sample не достигает precision в frozen blocks/operations, cell prospectively infeasible и scope пересматривается до exposure.

## 11. Invalidation, replacement and evidence

- Outcome-blind authority classifies invalidity from raw evidence before comparative outcomes.
- Per cell: at most one matched replacement for independently proven technical invalidity. Repeated invalidity gives No Decision for affected claim.
- Whole-tranche contamination permits one pre-authorized fresh tranche with new seed, same artifact/dependency versions and preserved original result; contamination recurrence gives No Decision.
- Candidate-caused failure/noncompletion is never replaced or excluded.
- Raw distributions and mandatory evidence are retained in restricted immutable package. External release requires OQ-8, security/privacy/redaction review and Project Owner approval.
- Direct cost is reported at frozen USD prices and workload: licenses, cloud/service usage, machine-hours and mandatory manual-service labor. It is a mandatory descriptive guardrail: hidden/unbounded/unavailable cost blocks practical claim; otherwise it cannot compensate RTTSI.
- OQ-7 до exposure фиксирует currency date/source, amortization, availability, maximum direct cost per implementation и per million operations. Пока количественный cost slot не заполнен, OQ-5B и practical claim остаются No Start.
- Exact vulnerability triage/notification clocks remain assigned to OQ-8; until filled, OQ-4 downstream closure and Charter start remain blocked.

## 12. Decision lattice

1. Invalid/incomparable/insufficient-precision evidence → `Gate Closed / No Decision`.
2. Hard-gate failure → candidate artifact/scope disqualified.
3. Necessity-first: if viable C closes Ledger through an open independently implementable surface, choose `upstream` when the complete change fits one recognized, active, RF extension surface with accountable governance/conformance; otherwise choose open `profile`. Efficiency cannot override this PRD rule.
4. `native` only when all viable C fail H-1 criteria, N passes H-1–H-7, primary Material Advantage passes and every guardrail passes.
5. `stop` only with complete valid evidence when N fails and neither upstream nor profile offers a safe practically justified direction.

Recognized upstream surface identity, scope, RF/IP terms, maintainer activity, change process and conformance path are frozen in OQ-8/OQ-3B. Budget exhaustion is pause/re-scope/No Decision, never `stop`.

Результат `native` доказывает только техническую и практическую оправданность отдельного Core в frozen experimental scope; он не доказывает market adoption, жизнеспособность бизнес-модели или превращение AgentBridge в индустриальный стандарт — для этого требуются отдельные исследования и внешние доказательства.

## 13. OQ-5A/OQ-5B freeze criteria

**OQ-5A** может быть заморожена после re-review и принятия Project Owner policy этого документа: estimands, necessity-first, 20% boundary, completion gate, challenger roles, guardrail logic, ceilings, calibration и invalidation rules.

**OQ-5B** после OQ-6/OQ-7 и neutral pilot заполняет exact N/τ, team mix, budget/cost slots, cell table, schedules, tail operations и absolute bounds; затем эти значения атомарно входят в OQ-3B freeze до Candidate design. OQ-5A не зависит циклически от OQ-3B, а OQ-3B не начинается без OQ-5B.

Любое outcome-affecting изменение OQ-5A/OQ-5B unfreezes соответствующий scope, требует новой preregistration, fairness/security review и affected rerun. Project Owner должен consciously accept necessity-first policy, 20% boundary и No Decision rule.

До operational freeze OQ-5B весь Charter остаётся `draft / No Start`.

### OQ-5A freeze record

- **Frozen scope:** policy и exact values, уже заданные в §§1–12; OQ-5B slots явно исключены.
- **Authority/date:** Project Owner, 2026-09-13.
- **Independent reviews:** methodology PASS, fairness PASS, security PASS; Critical/High/Medium/Low — 0/0/0/0 в каждом финальном review.
- **Change control:** любое смысловое изменение OQ-5A немедленно возвращает её в `open`, создаёт новую version/preregistration и требует затронутых independent reviews плюс повторного принятия Project Owner.
- **Не разрешено:** OQ-5B, OQ-3B, Candidate design, экспериментальный или production code.
