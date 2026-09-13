# Scenario grammar and public corpus plan — OQ-3A proposal

Статус: **proposal-ready; templates only; exact OQ-3B corpus not generated**.

Этот документ фиксирует общую форму испытаний до candidate design. Он использует абстрактные сущности и логическое время, поэтому ни один wire format, transport, язык или Candidate object model не получает преимущества.

## 1. Canonical synthetic datasets

Все данные синтетические. Идентификаторы — смысловые метки oracle, а не будущие поля протокола.

### DS-1 — Participants, principals and control

| ID | Смысл |
| --- | --- |
| `P-USER`, `P-ORG-A`, `P-ORG-B` | три независимых Принципала |
| `X-DELEGATE`, `X-SUBDELEGATE` | Участники, действующие по ограниченному делегированию |
| `X-PROVIDER-A`, `X-PROVIDER-B` | два независимых provider/responder участника |
| `X-APPROVER-1`, `X-APPROVER-2` | независимо контролируемые approvers |
| `X-APPROVER-1-ALIAS` | иной идентификатор под тем же controller/authority root, что `X-APPROVER-1` |
| `X-DISCOVERY`, `X-IDENTITY`, `X-POLICY`, `X-AUDIT` | заменяемые инфраструктурные роли без автоматического доверия |

Каждый vector явно назначает динамические роли. Ни один ID не означает постоянную роль, право или transport position.

### DS-2 — Resources, subjects and authority

| ID | Смысл |
| --- | --- |
| `R-A`, `R-B` | независимые синтетические Resources |
| `SUBJECT-A` | действие `SIMULATE_RESERVE` над `R-A`, Audience `X-PROVIDER-A`, quantity range `1..2`, logical epoch `E7` |
| `SUBJECT-A-MUT` | тот же label операции, но `R-B`, иная Audience либо quantity вне `1..2` |
| `G-BASE` | Authority для `SUBJECT-A`, time `T100..T200`, use limit `1`, без subdelegation по умолчанию |
| `G-NARROW` | корректное сужение `G-BASE`: quantity `1`, time `T120..T160` |
| `G-AMPLIFIED` | запрещённое расширение Resource, Audience, action, time или use limit |
| `CONSENT-A`, `APPROVAL-1`, `APPROVAL-2` | решения, привязанные к точному `SUBJECT-A` и epoch |

Все consequential actions изменяют только isolated effect simulator; реального платежа, заказа, бронирования или внешнего обязательства нет.

### DS-3 — Operation and evidence states

| ID | Смысл |
| --- | --- |
| `OP-A` | одна Operation Identity для `SUBJECT-A` |
| `TRY-A1`, `TRY-A2` | две попытки доставки той же операции |
| `CLAIM-NONE`, `CLAIM-FULL`, `CLAIM-PARTIAL`, `CLAIM-UNKNOWN` | конфликтуемые Outcome Claims с отдельными sources/assurance |
| `EV-RECEIPT`, `EV-AUTH`, `EV-EXEC`, `EV-EFFECT` | Evidence разных стадий; ни одно не заменяет другое |
| `GAP-REDACTED`, `GAP-MISSING` | типизированные пробелы causal chain |

### DS-4 — Versions, capabilities and mappings

| ID | Смысл |
| --- | --- |
| `CORE-V1`, `CORE-V1N`, `CORE-V2` | старая, текущая и новая несовместимая semantic version class |
| `EXT-OPTIONAL-A`, `EXT-MANDATORY-A`, `EXT-COLLISION` | безопасно необязательная, mandatory-to-understand и конфликтующая семантика |
| `BIND-A`, `BIND-B`, `BIND-C` | три абстрактных binding property contracts |
| `MAP-A→B`, `MAP-B→A`, `MAP-LOSSY` | направленные versioned mappings с явным provenance/loss |
| `CAP-FRESH`, `CAP-STALE`, `CAP-SPOOFED` | актуальное, просроченное и неподтверждённое capability declaration |

### DS-5 — Cross-provider read-only data

| Record | Нормативный смысл |
| --- | --- |
| `D-A` | amount `100` minor units, currency `USD`, quantity basis `1`, provenance A |
| `D-B` | amount `1.00` major units, currency `USD`, quantity basis `1`, provenance B |
| `D-C` | value `100` без currency/unit basis; несопоставимо |
| `D-D` | amount известен, availability `unknown`; результат частично сопоставим |
| `D-PRIVATE` | атрибут вне разрешённой privacy-boundary; раскрытие запрещено |

`D-A` и `D-B` равны только в Profile, явно задающем unit conversion. Совпадение формы/имени без Profile не создаёт равенство.

### DS-6 — Fault and adversary operators

`DROP`, `DUPLICATE`, `REORDER`, `DELAY`, `PARTITION`, `CRASH`, `RESTART`, `STALE`, `REVOKE`, `MUTATE`, `ALIAS`, `SPLICE`, `DOWNGRADE`, `BRIDGE_LOSS`, `OBSERVER_DROP`, `EGRESS_ATTEMPT`, `SLOW_CONSUMER`, `RESOURCE_EXHAUST`, `CONFLICT_CLAIM`.

Каждый operator имеет logical insertion point и не меняет другие параметры, кроме явно перечисленных.

## 2. Vector template

Каждый exact vector OQ-3B обязан заполнить:

1. `vector_id/version/classification` (`confirmatory-public`, `confirmatory-holdout`, `exploratory`);
2. `VC`, EI/FR/NFR и MP links;
3. dataset records и dynamic role assignments;
4. candidate-neutral initial state и claim boundary;
5. stimulus/event schedule с logical steps;
6. единственную mutation/fault либо явно названную compound комбинацию;
7. allowed Outcome Tuple set;
8. forbidden tuples, disclosures, effects и transitions;
9. OBS sources, detection window, correlation и completeness condition;
10. privacy/resource bounds и safe-stop;
11. applicability/exclusion rationale для каждого кандидата;
12. expected evidence artifacts, authorship/cross-check и content digest.

Пустое обязательное поле делает vector непригодным для confirmatory corpus.

## 3. Public template matrix

Для каждой семьи фиксируются пять обязательных archetypes. Это 65 template cells; OQ-3B создаёт минимум один exact vector на cell и дополнительные cases для каждого уникального failure mechanism.

| VC | `P01` positive | `N01` boundary-negative | `F01` failure/concurrency | `A01` adversarial | `M01` metamorphic |
| --- | --- | --- | --- | --- | --- |
| VC-1 | Fresh compatible negotiation → compatible selected set | Unknown mandatory → incompatible/block | Stale declaration + reconnect → no silent reuse | Transcript mutation/downgrade → block | Role/endpoint relabel preserves result (MP-01/02) |
| VC-2 | Exact permit then one simulated commit → full | Subject mutation or expired authority → non-permit, no effect | Commit response lost → effect full/unknown as observed, no second commit | Replay/confused deputy → no new effect | Narrow Authority cannot add effects (MP-03/05) |
| VC-3 | Hold expires before commit → established-none | Late completion after cancel remains explicit | Cancel/commit race → allowed partial/unknown set, never invented certainty | Compensation failure ≠ rollback | Added loss cannot improve certainty (MP-06/15) |
| VC-4 | Two independent approvals → one simulated change | Alias of same controller counted twice → non-permit | Concurrent duplicate request → one commit | Approval spliced to changed subject → no effect | Organizational relabel preserves abstract result (MP-17) |
| VC-5 | Valid three-link attenuation → bounded permit | Amplified subdelegation → non-permit | Revocation at new boundary blocks future use | Branch/presenter splice → no effect | Further attenuation cannot expand outcome (MP-03) |
| VC-6 | Valid quorum and consistent epoch → aggregate | Shared controller fails separation-of-duty | Partitioned shared limit → total use ≤ limit or unknown/non-permit | Split-view membership/epoch → protected continuation blocked | Unrelated branch addition is noninterfering (MP-10/16) |
| VC-7 | Restart recovers allowed state and provenance | Retention gap remains typed unknown | Duplicate/reorder after restart → no new effect | Stale security epoch claim → blocked | Added fault cannot raise certainty (MP-06/15) |
| VC-8 | Claimed provider substitution preserves scoped properties | Critical provider unavailable → non-permit/unknown | Mid-boundary outage isolated to dependent scope | Provider spoof/composition reorder → loss/block | Substitute only where replaceability claimed (MP-02/09) |
| VC-9 | Stream resumes with explicit gap/order scope | Expired authority blocks new protected event | Slow consumer reaches bounded safe-stop | Gap/repeat hidden as contiguous stream → fail | Resource increase crosses deterministic boundary (MP-14) |
| VC-10 | Staged compatible upgrade → same allowed set | Unknown mandatory/collision → incompatible | Mixed-version branch remains scoped | Silent default/downgrade → block | Weaker version never improves result (MP-07/12) |
| VC-11 | Lossless scoped mapping preserves safety projection | Critical loss → dependent action blocked | Upstream drift invalidates only mapped claim | Loop/impersonation/assurance inflation → fail | Extra bridge hop cannot hide loss (MP-13/18) |
| VC-12 | Compound safe case remains within all bounds | One blocking mutation dominates otherwise valid flow | Partition+crash+retry → no duplicate/shared-limit breach | Egress/observer/leakage attack → candidate fail or run invalid by observer rule | Added fault never improves permission/certainty (MP-06/14/15) |
| VC-13 | `D-A` and `D-B` comparable under explicit Profile | `D-C` remains incomparable; `D-PRIVATE` undisclosed | Provider partial/timeout → partial/unknown, no invented value | Hidden pair mapping or provenance mutation → bounded claim/fail | Domain relabel and public mapping obey MP-17/18 |

## 4. Expected-outcome derivation order

Для каждого vector Oracle Author применяет правила в фиксированном порядке:

1. проверить run/observer validity;
2. определить applicability и compatibility;
3. вычислить exact Decision Subject, epoch и Effective Authority;
4. применить Consent/Approval/Obligation и shared-limit rules;
5. определить разрешённый progress, execution и effect set;
6. сохранить uncertainty, conflicts, causal gaps и evidence assurance;
7. применить privacy/resource bounds;
8. проверить forbidden safety projection;
9. применить metamorphic relation к связанному vector.

Более поздний шаг не может превратить запрещённый результат раннего шага в pass.

## 5. OQ-3B generation obligations

До экспериментального кода:

- каждая из 65 cells получает exact public confirmatory vector;
- каждый отдельный failure mechanism из EI falsifiers получает хотя бы один exact negative vector;
- каждый security-critical vector имеет независимо выведенный либо blind cross-checked expected outcome;
- domain/topology/mode exclusions имеют rationale и сужают claim;
- concrete resource limits приходят только из frozen OQ-5;
- manifests, storage и sealed holdout создаются только после OQ-8 controls.

Число `65` — минимальный coverage floor, не целевой максимум и не основание объединять различные failure mechanisms в один непроверяемый mega-case.
