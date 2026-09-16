---
title: AgentBridge AA-1 Quality-Attribute Scenarios
status: final
created: 2026-09-16
updated: 2026-09-16
scope: Architecture Baseline v0.x
governing_decisions:
  - AD-1
  - AD-2
  - AD-3
---

# Quality-Attribute Scenarios

## 1. Назначение и правила применения

Эти сценарии превращают качества AgentBridge в проверяемые архитектурные обязательства. Они являются конституционными целями AA-1, а не заявлением о том, что реализация уже существует или прошла проверку.

Каждый сценарий обязан получить на последующих Gates:

1. точный claim scope и применимый Profile/Binding;
2. замороженные входы, assumptions и artifact/dependency closure;
3. квалифицированный observer и метод получения evidence;
4. измеримый verdict `pass`, `fail`, `invalid` или `inconclusive`;
5. traceability к FR/NFR, EI и SBC;
6. точные численные пределы там, где они зависят от модели, threat analysis либо wire/runtime решения.

Общие AA-1 правила:

- подтверждённое in-scope событие любого SBC имеет допустимое количество **0**;
- `unknown`, `partial`, conflict, timeout или отсутствие ответа не повышаются до success/no-effect;
- неполный observer не может выдать `pass`;
- для каждого применимого SBC, где возможен Protected Disclosure или Consequential Effect, обязательны минимум два независимо выведенных observer paths; общий oracle/codebase сам по себе не доказывает независимость, а отсутствие требуемого пути или неразрешённое расхождение даёт `invalid`/`inconclusive`, никогда не `pass`;
- safety/security/privacy/correctness не ослабляются ради performance или availability;
- значения, помеченные `TBD@AA-n`, являются обязательным downstream slot, а не необязательной заметкой;
- сценарий считается закрытым только evidence указанного Gate, а не мнением или поведением reference implementation.

Documentary specification, state-transition design and proof-obligation drafting in AA-2 are allowed. Any executable model, model-checker run, prototype, benchmark harness, fuzz/fault execution or other executable verification remains blocked until the concrete Pre-Prototype Control Manifest required by AD-21 is frozen and approved for that exact scope.

### Owner and pass-evidence map

| QAS family | Accountable owner | Required pass evidence |
| --- | --- | --- |
| `SEC` | Security Architect with Protocol Architect | normative model/proof obligations, scoped threat analysis, independent security verdict, minimum two independently derived observer paths for applicable protected disclosure/effect SBC outcomes and AA-5 negative/fault evidence |
| `PRI` | Privacy/Security Architect | data-flow and observer inventory, disclosure-purpose relation, leakage/linkability bounds, privacy review and AA-5 vectors |
| `COR` | Protocol Architect | allowed-state/trace model, assertion mapping and differential/fault evidence |
| `RES` | Distributed-Systems/Runtime Architect | failure model, resource envelope, recovery/fault evidence and independent review |
| `PERF` | Performance/Networking Reviewer | preregistered workload/budgets, conformant benchmark artifacts and raw reproducible results |
| `EVO` | Protocol Evolution/Governance Owner | version/migration matrix, artifact digests, compatibility verdict and invalidation/revalidation evidence |
| `IMP` | Conformance Lead | frozen normative package, coverage map, genealogy/access record and independent implementation evidence |
| `DEC` | Chief Architect | dependency graph, provider-cut/substitution evidence and independent decentralization review |

The accountable owner prepares closure but cannot self-approve it where the Charter requires independence. Every evidence record cites the QAS ID, exact artifact/dependency digest, applicable FR/NFR/EI/SBC IDs, observer and Gate verdict.

## 2. Security

### QAS-SEC-01 — Недействительное или чужое полномочие

- **Stimulus/adversary:** сторона предъявляет подделанный, истёкший, отозванный, украденный либо предназначенный другому actor, audience, resource, context или epoch permit.
- **Context:** protected disclosure либо consequential-effect boundary во всех обязательных топологиях.
- **Expected response:** переход через frozen disclosure/effect reference monitor блокируется; mediated disclosure/effect отсутствует; отказ типизирован; сохраняется только разрешённое диагностическое evidence. Untrusted executor не является reference monitor; compromise либо неизвестный bypass финальной enforcement boundary приостанавливает зависимый positive claim.
- **Measure:** запрещённых protocol-mediated disclosure/effect — `0`; 100% authority dimensions проверены до permit; ambiguous authority даёт non-permit; неизвестных bypass paths при positive claim — `0`.
- **Verification:** AA-2 transition invariants; AA-3 misuse/threat analysis; AA-5 negative, mutation and boundary vectors.
- **Gate:** constitutional target AA-1; state semantics AA-2; security closure AA-3; conformance closure AA-5.
- **Traceability:** FR-9, FR-30–FR-37, FR-39–FR-42; NFR-2–NFR-5, NFR-10; EI-05–EI-08, EI-18; SBC-01, SBC-02.

### QAS-SEC-02 — Монотонное сужение делегирования

- **Stimulus/adversary:** delegate, intermediary, Extension или Bridge в любом направлении/цепочке пытается расширить action, resource, audience, amount, duration, use count, context либо transferable/delegable scope.
- **Context:** один или несколько delegation/translation hops, включая native-to-foreign, foreign-to-native, chained и round-trip Bridge paths.
- **Expected response:** derivation отклоняется; effective authority является только допустимым сужением родительского authority; source subject, Presenter, transformation path, provenance, loss и assurance ceiling сохраняются; critical loss блокирует зависимое действие.
- **Measure:** reachable states с authority amplification — `0`; 100% нормативных authority dimensions участвуют в derivation relation.
- **Verification:** AA-2 formal attenuation relation/model checking; AA-3 compromise analysis; AA-5 property and bridge vectors.
- **Gate:** AA-1/2/3/5.
- **Traceability:** FR-32–FR-34, FR-38, FR-40, FR-42, FR-69–FR-78; NFR-2–NFR-6; EI-06, EI-16, EI-17; SBC-01, SBC-05.

### QAS-SEC-03 — Revoke/expiry/subject race на disclosure/effect boundary

- **Stimulus/adversary:** revoke, expiry, purpose, audience, classification, policy, security epoch, precondition либо Decision Subject меняется между ранней проверкой и Protected Disclosure observation/commit либо Consequential Effect commit.
- **Context:** concurrent, long-running либо partitioned protected disclosure/consequential operation.
- **Expected response:** актуальность проверяется непосредственно в точке первого наблюдаемого content/metadata/timing/diagnostic signal и в точке effect commit либо доказуемо атомарно связана с соответствующим событием; иначе disclosure/effect блокируется, а незавершённое состояние остаётся explicit partial/unknown.
- **Measure:** запрещённых post-revoke/post-change disclosures/commits — `0` во всех in-scope interleavings; допустимое freshness/revocation window — `TBD@AA-3` по Profile.
- **Verification:** AA-2 concurrency model; AA-3 boundary/atomicity proof obligations; AA-5 race and fault vectors.
- **Gate:** AA-1/2/3/5.
- **Traceability:** FR-31, FR-37, FR-39, FR-43, FR-46, FR-48; NFR-2, NFR-3, NFR-8, NFR-10; EI-05, EI-08; SBC-01, SBC-02.

### QAS-SEC-04 — Downgrade, split-view и unknown mandatory semantics

- **Stimulus/adversary:** участник или посредник удаляет mandatory capability, подменяет Profile/Binding/version, replay-ит stale declaration или показывает сторонам разные security-relevant transcripts/epochs.
- **Context:** bootstrap, negotiation, reconnect, membership change или staged upgrade.
- **Expected response:** несовместимость обнаруживается до зависимого защищённого обмена; continuation блокируется; transcript/epoch mismatch остаётся наблюдаемым.
- **Measure:** защищённых действий после неразрешённого downgrade/split-view — `0`; unknown mandatory semantics всегда fail-closed.
- **Verification:** AA-2 negotiation/epoch model; AA-3 transcript protection requirements; AA-4 concrete binding; AA-5 downgrade/split-view corpus.
- **Gate:** AA-1–AA-5.
- **Traceability:** FR-14–FR-18, FR-56, FR-58–FR-65, FR-71, FR-81, FR-91, FR-95; NFR-2, NFR-3, NFR-6, NFR-10, NFR-18, NFR-19, NFR-29; EI-02, EI-03, EI-15, EI-18, EI-23; SBC-05, SBC-10.

### QAS-SEC-05 — Конкурентное расходование общего лимита

- **Stimulus/adversary:** несколько ветвей одновременно расходуют общий amount/use/rate/quorum limit, включая partition и replay.
- **Context:** multi-party или delegated operation.
- **Expected response:** reserve/commit-consume/release/expire/recover/reconcile transitions связаны с exact limit epoch, reservation, Logical Operation, Decision Subject и effect identity; совокупный расход не превышает доказуемый остаток; stale/unknown reconciliation, crash, retry, partition или coordinator substitution не создают capacity; при отсутствии безопасного доказательства — non-permit/unknown. Profile явно выбирает coordinator, escrow/partitioned quota либо fail-closed.
- **Measure:** overspend и capacity resurrection — `0`; выбранная coordination assumption, lifecycle и blast radius определены для 100% shared limits.
- **Verification:** AA-2 full shared-limit lifecycle model; AA-3 trust/failure analysis; AA-5 concurrency/crash/partition/substitution vectors.
- **Gate:** AA-1/2/3/5.
- **Traceability:** FR-25, FR-36, FR-38, FR-41, FR-50; NFR-2, NFR-10, NFR-15; EI-08, EI-09, EI-18; SBC-03.

## 3. Privacy

### QAS-PRI-01 — Минимально необходимое раскрытие

- **Stimulus/adversary:** сообщение, evidence или diagnostic пытается раскрыть дополнительный content, identifier, membership, metadata или linkability signal.
- **Context:** любой observer/channel и protected-disclosure boundary.
- **Expected response:** AA-2 mechanism-neutral relation связывает exact disclosed subject/fields, frozen purpose, recipient/observer, necessity basis, classification, allowed linkability и epoch; разрешается только covered disclosure, остальное удаляется, скрывается либо передача блокируется. Missing/conflicting/unknown mandatory purpose или necessity даёт non-permit/cannot-establish.
- **Measure:** 100% раскрываемых элементов имеют purpose, recipient/observer, necessity, classification, retention и allowed linkability; непокрытых элементов — `0`.
- **Verification:** AA-2 disclosure authorization relation and one-field falsifiers; AA-3 data-flow/privacy inventory; AA-4 wire analysis; AA-5 leakage and negative vectors.
- **Gate:** AA-1/2/3/4/5.
- **Traceability:** FR-4, FR-12, FR-40, FR-41, FR-55, FR-72; NFR-4, NFR-20, NFR-23–NFR-25; EI-19, EI-25; SBC-06, SBC-10.

### QAS-PRI-02 — Изоляция ветвей, tenants и несвязанных участников

- **Stimulus/adversary:** участник, aggregator или observer пытается восстановить несвязанную branch, membership, tenant либо correlation path.
- **Context:** multi-party, quorum, delegation и aggregated evidence.
- **Expected response:** раскрывается только causally dependent subset; aggregate связывает использованные epochs без раскрытия несвязанных ветвей.
- **Measure:** forbidden cross-boundary disclosure/linkability — `0` в frozen adversary model; точный allowed linkability set — `TBD@AA-3`.
- **Verification:** AA-2 dependency/causality model; AA-3 privacy threat analysis; AA-5 cross-branch vectors.
- **Gate:** AA-1/2/3/5.
- **Traceability:** FR-4, FR-7, FR-25, FR-41, FR-50, FR-55; NFR-4, NFR-20, NFR-23, NFR-24; EI-04, EI-18, EI-19; SBC-06.

### QAS-PRI-03 — Errors, sizes и timing как канал утечки

- **Stimulus/adversary:** наблюдатель сравнивает error forms, response sizes, timing, retries или fallback paths.
- **Context:** authorization failure, discovery, negotiation и protected operation.
- **Expected response:** observer не получает secret, resource existence или policy detail сверх разрешённого Profile disclosure bound.
- **Measure:** secret-bearing diagnostics — `0`; допустимые timing/size/error leakage bounds — `TBD@AA-3/AA-4`.
- **Verification:** AA-3 side-channel scope; AA-4 representation/runtime design; AA-5 differential measurements.
- **Gate:** AA-1/3/4/5.
- **Traceability:** FR-19, FR-29, FR-40, FR-52, FR-55, FR-72; NFR-4, NFR-9, NFR-20, NFR-23, NFR-25; EI-19, EI-20, EI-25; SBC-06, SBC-07.

### QAS-PRI-04 — Retention, deletion и redaction без ложного assurance

- **Stimulus/adversary:** истекает retention либо действует deletion/redaction requirement; часть causal evidence становится недоступна.
- **Context:** external evidence/diagnostic stores.
- **Expected response:** применимые данные удаляются/редактируются; оставшийся claim явно показывает неполноту и не повышает assurance.
- **Measure:** 100% retained classes имеют purpose, owner, expiry и deletion/redaction behavior; ложных complete/deleted claims — `0`; сроки — `TBD@AA-3` по Profile.
- **Verification:** AA-3 lifecycle/privacy policy; AA-5 store/observer evidence review.
- **Gate:** AA-1/3/5.
- **Traceability:** FR-51, FR-53–FR-55; NFR-4, NFR-20, NFR-21, NFR-23–NFR-25; EI-13, EI-19, EI-25; SBC-06, SBC-08.

## 4. Correctness

### QAS-COR-01 — Lost response и safe retry

- **Stimulus/failure:** effect мог произойти, но ответ потерян; инициатор повторяет operation.
- **Context:** consequential action при message loss, timeout или crash.
- **Expected response:** тот же operation/Decision Subject не создаёт новый commit; если deduplication/effect knowledge отсутствует либо истёкло или прошлый effect нельзя доказать, возвращается typed unknown/non-permit (либо доказанный no-duplicate outcome), а не разрешение считать operation новой.
- **Measure:** duplicate effect — `0` в заявленном scope; silently-fresh execution after knowledge expiry — `0`; false success/no-effect — `0`; retention/idempotency scope — `TBD@AA-2/AA-3`.
- **Verification:** AA-2 lifecycle model; AA-3 effect-boundary obligations; AA-5 replay/crash/fault vectors.
- **Gate:** AA-1/2/3/5.
- **Traceability:** FR-43, FR-46, FR-47, FR-51–FR-54; NFR-2, NFR-8, NFR-10, NFR-11, NFR-21; EI-10, EI-11, EI-13; SBC-04, SBC-08.

### QAS-COR-02 — Cancel, expiry, late completion и compensation

- **Stimulus/failure:** cancel/expiry пересекается с commit; completion приходит поздно; compensation не выполняется.
- **Context:** asynchronous/long-running operation.
- **Expected response:** история не переписывается; late effect виден; compensation — отдельная причинно связанная и отдельно разрешённая operation с собственными Logical Operation ID, Decision Subject, current authority, Obligations, limits, commit boundary и outcome; permit исходной operation не наследуется.
- **Measure:** traces, где cancel доказывает отсутствие уже возможного effect, failed compensation объявляет восстановление либо compensation использует stale/inherited authority, — `0`; unauthorized/repeated compensation commits — `0`.
- **Verification:** AA-2 state machine/model checking; AA-5 concurrency and delayed-delivery vectors.
- **Gate:** AA-1/2/5.
- **Traceability:** FR-28, FR-44–FR-50; NFR-8, NFR-10, NFR-11, NFR-21; EI-10–EI-12; SBC-04.

### QAS-COR-03 — Конфликтующие evidence claims

- **Stimulus/adversary:** два атрибутированных источника утверждают несовместимые outcomes либо отсутствует causal link.
- **Context:** distributed/multi-party effect observation.
- **Expected response:** conflict/gap остаётся явным до нормативно допустимого разрешения; оптимистичный claim автоматически не выбирается.
- **Measure:** assurance inflation и automatic-success paths — `0`; 100% accepted claims имеют требуемый issuer/subject/observation/freshness/causality scope.
- **Verification:** AA-2 evidence model; AA-3 verifier/trust model; AA-5 conflict corpus.
- **Gate:** AA-1/2/3/5.
- **Traceability:** FR-46, FR-51, FR-53–FR-55; NFR-8, NFR-11, NFR-20, NFR-21, NFR-24; EI-10, EI-13, EI-25; SBC-04, SBC-08.

### QAS-COR-04 — Семантическая эквивалентность Bindings

- **Stimulus/failure:** одна Core-operation переносится разными Native Bindings или independent implementations.
- **Context:** одинаковые normative inputs, Profile и environmental assumptions.
- **Expected response:** реализации получают один allowed outcome set и одинаковую safety projection.
- **Measure:** differential conformance не выявляет различий authority/effect/evidence semantics; расхождения — `0` в claim scope.
- **Verification:** AA-2 allowed-set model; AA-4 bindings; AA-5 differential suite.
- **Gate:** AA-1/2/4/5.
- **Traceability:** FR-57, FR-69–FR-71, FR-82, FR-86, FR-91; NFR-3, NFR-11, NFR-14, NFR-16–NFR-19; EI-14–EI-16, EI-22; SBC-05, SBC-09.

## 5. Resilience

### QAS-RES-01 — Hostile input и bounded safe-stop

- **Stimulus/adversary:** чрезмерные size, depth, fan-out, rate, retries, malformed input либо decompression/parser abuse.
- **Context:** любой externally reachable parser/endpoint.
- **Expected response:** bounded rejection/backpressure/safe-stop; нет unbounded work, storage, queue, amplification, deadlock или permissive fallback.
- **Measure:** CPU/memory/state/I/O/payload/depth/fan-out/retry/amplification/time остаются ниже frozen ceilings `TBD@AA-4`; запрещённых fallback — `0`.
- **Verification:** AA-3 abuse model; AA-4 aggregate budgets; AA-5 fuzz/load/adversarial tests.
- **Gate:** AA-1/3/4/5.
- **Traceability:** FR-23, FR-29, FR-74, FR-80, FR-88–FR-90; NFR-2, NFR-8–NFR-10, NFR-13; EI-20, EI-25; SBC-07, SBC-10.

### QAS-RES-02 — Отказ или compromise необязательной зависимости

- **Stimulus/failure:** registry, IdP, broker, audit store, policy provider, Bridge или cloud недоступен, stale либо compromised.
- **Context:** Native operation с объявленной dependency closure.
- **Expected response:** останавливается или деградирует только зависимый scope; authority не расширяется; Native Core semantics не меняется.
- **Measure:** observed blast radius не превышает declared scope; скрытых mandatory dependencies и permissive fallback — `0`.
- **Verification:** AA-3 dependency/compromise analysis; AA-5 fault injection and provider-substitution tests.
- **Gate:** AA-1/3/5.
- **Traceability:** FR-11, FR-18, FR-73–FR-75, FR-81, FR-95; NFR-5, NFR-7, NFR-8, NFR-15, NFR-26, NFR-28, NFR-29; EI-16, EI-20, EI-21, EI-23; SBC-05, SBC-07, SBC-09.

### QAS-RES-03 — Slow consumer и backpressure

- **Stimulus/failure:** consumer перестаёт читать, downstream замедляется либо stream остаётся полуоткрытым.
- **Context:** streaming, subscriptions, reverse async и long-running flows.
- **Expected response:** bounded buffering, explicit backpressure/cancel/safe-stop; критическое evidence не теряется и не подменяется success.
- **Measure:** queue/state/time находятся в ceilings `TBD@AA-4`; retry amplification и deadlock — `0`.
- **Verification:** AA-2 termination semantics; AA-4 runtime rules; AA-5 stress/fault tests.
- **Gate:** AA-1/2/4/5.
- **Traceability:** FR-23, FR-24, FR-28, FR-29; NFR-8–NFR-10, NFR-13; EI-04, EI-20; SBC-07.

### QAS-RES-04 — Crash/recovery и новый security epoch

- **Stimulus/failure:** participant/executor перезапускается либо восстанавливается после compromise.
- **Context:** незавершённая или ранее разрешённая operation.
- **Expected response:** старые grants/claims не приобретают новую силу; state восстанавливается как exact known state либо explicit unknown; effect автоматически не повторяется.
- **Measure:** stale-epoch authorization, silent state invention и duplicate commit — `0`; recovery durability scope — `TBD@AA-2/AA-3`.
- **Verification:** AA-2 recovery model; AA-3 compromise plan; AA-5 crash/recovery vectors.
- **Gate:** AA-1/2/3/5.
- **Traceability:** FR-18, FR-39, FR-43–FR-48, FR-51, FR-65, FR-95; NFR-5, NFR-8, NFR-10, NFR-18, NFR-29; EI-06, EI-10, EI-11, EI-18, EI-23; SBC-01, SBC-04, SBC-05.

## 6. Performance

### QAS-PERF-01 — Нагрузка без ослабления semantics

- **Stimulus:** нагрузка растёт до и выше заявленного operating envelope.
- **Context:** conformant Native Binding и frozen Profile/scenario mix.
- **Expected response:** до предела сохраняется заявленная latency/throughput; после предела включается bounded backpressure/safe-stop, но не security/correctness downgrade.
- **Measure:** throughput, p50/p95/p99 latency, error/backpressure rate и resource/unit — `TBD@AA-4`; SBC/semantic divergence при любой нагрузке — `0`.
- **Verification:** AA-4 reproducible benchmark только после определения oracle; AA-5 conformance-before/after-load check.
- **Gate:** target AA-1; numeric envelope AA-4; validity AA-5.
- **Traceability:** FR-23, FR-29, FR-74, FR-89, FR-90; NFR-9, NFR-12, NFR-13, NFR-15; EI-10, EI-20; SBC-04, SBC-07.

### QAS-PERF-02 — Ограниченная стоимость сложных flows

- **Stimulus/adversary:** глубокая delegation chain, большая evidence graph, multi-party fan-out либо extension/profile composition.
- **Context:** worst-case legal input и hostile boundary input.
- **Expected response:** cost растёт только в объявленной bounded function; превышение ceilings отклоняется до необратимого эффекта.
- **Measure:** maximum depth/fan-out/payload/state и asymptotic/empirical cost — `TBD@AA-4`; unbounded amplification — `0`.
- **Verification:** AA-2 structural model; AA-4 complexity/budget analysis; AA-5 boundary/fuzz benchmarks.
- **Gate:** AA-1/2/4/5.
- **Traceability:** FR-25, FR-33, FR-50, FR-61, FR-80, FR-89, FR-90; NFR-9, NFR-10, NFR-12, NFR-13, NFR-15; EI-20, EI-25; SBC-07.

## 7. Evolvability

### QAS-EVO-01 — Staged upgrade, coexistence и rollback

- **Stimulus:** стороны обновляются независимо, откатываются либо временно используют разные версии.
- **Context:** declared compatibility scope.
- **Expected response:** общая область согласуется явно; несовместимость видна; прежний conformance/security claim не переносится на изменённый closure.
- **Measure:** silent semantic drift — `0`; 100% claims связаны с exact version/artifact/dependency closure; допустимое число combinations/steps — `TBD@AA-4`.
- **Verification:** AA-2 version model; AA-4 migration design; AA-5 compatibility matrix.
- **Gate:** AA-1/2/4/5.
- **Traceability:** FR-18, FR-56–FR-67, FR-81, FR-91, FR-94, FR-95; NFR-18, NFR-19, NFR-26, NFR-29; EI-15, EI-18, EI-23; SBC-05, SBC-10.

### QAS-EVO-02 — Extension collision и обязательная критичность

- **Stimulus:** неизвестное Extension, namespace collision или Extension пытается переопределить Core/Profile safety meaning.
- **Context:** negotiation и message processing.
- **Expected response:** non-critical Extension игнорируется только в доказанно безопасной области; critical/mandatory unknown либо redefinition блокируется.
- **Measure:** accepted Core redefinition/collision — `0`; 100% Extensions имеют owner, namespace, version и criticality behavior.
- **Verification:** AA-2 extension semantics; AA-5 collision/unknown vectors.
- **Gate:** AA-1/2/5.
- **Traceability:** FR-15, FR-16, FR-58–FR-61, FR-64, FR-67, FR-88, FR-91; NFR-2, NFR-6, NFR-18, NFR-19; EI-14, EI-15; SBC-05.

### QAS-EVO-03 — Замена security primitive/provider и expiry Bridge

- **Stimulus:** algorithm/provider уязвим, assumption меняется либо Bridge выходит за поддерживаемую version window.
- **Context:** active deployments и stored claims.
- **Expected response:** затронутый scope/claim приостанавливается; миграция не повышает authority/assurance и не требует flag day без явного Profile решения.
- **Measure:** stale claim после invalidating change — `0`; migration/bridge expiry window — `TBD@AA-3/AA-4`.
- **Verification:** AA-3 crypto/trust agility policy; AA-4 binding/bridge plan; AA-5 migration vectors.
- **Gate:** AA-1/3/4/5.
- **Traceability:** FR-65, FR-69–FR-81, FR-91, FR-95; NFR-3, NFR-5–NFR-7, NFR-18, NFR-19, NFR-26, NFR-29; EI-16, EI-17, EI-23; SBC-05, SBC-09, SBC-10.

## 8. Implementability

### QAS-IMP-01 — Clean-room independent implementation

- **Stimulus:** независимая команда получает только frozen normative package и одинаково доступные conformance artifacts.
- **Context:** без reference behavior, private mapping и закрытых пояснений.
- **Expected response:** команда реализует заявленный scope и достигает interop с другой независимой реализацией.
- **Measure:** обязательных private explanations/shared protocol-semantic code — `0`; normative obligations с assertion/model/review coverage — 100%.
- **Verification:** AA-5 package/oracle review; AA-7 independent implementation and interop run.
- **Gate:** target AA-1; package AA-5; empirical closure AA-7.
- **Traceability:** FR-82–FR-95; NFR-11, NFR-14, NFR-16, NFR-17, NFR-22, NFR-27; EI-14, EI-22, EI-25; SBC-08, SBC-09.

### QAS-IMP-02 — Language/runtime neutrality

- **Stimulus:** реализации используют разные языки, numeric/time libraries, concurrency и runtime models.
- **Context:** одинаковая Core/Profile/Binding version.
- **Expected response:** parsers, state transitions и emitted evidence совпадают в allowed-set/safety projection.
- **Measure:** language-dependent normative ambiguity — `0`; differential failures в claim scope — `0`.
- **Verification:** AA-4 representation constraints; AA-5 language-neutral vectors/differential testing; AA-7 independent implementations.
- **Gate:** AA-1/4/5/7.
- **Traceability:** FR-57, FR-69–FR-71, FR-82, FR-85–FR-91; NFR-11, NFR-14, NFR-16–NFR-19; EI-14–EI-16, EI-22; SBC-05, SBC-09.

### QAS-IMP-03 — Диагностируемый отказ без скрытой помощи автора

- **Stimulus:** implementation получает invalid/unknown input или conformance failure.
- **Context:** development и operation в пределах privacy policy.
- **Expected response:** normative error category, violated assertion и минимальный reproducible trace определимы без vendor-only knowledge.
- **Measure:** unclassified mandatory failures — `0`; reproducible failing traces для observed failures — 100%.
- **Verification:** AA-5 assertion catalog, observer qualification and evidence schema.
- **Gate:** AA-1/5.
- **Traceability:** FR-19, FR-52, FR-82–FR-84, FR-92–FR-95; NFR-17, NFR-20–NFR-22; EI-22, EI-25; SBC-08, SBC-09.

## 9. Decentralization

### QAS-DEC-01 — Native operation без центральной AgentBridge-службы

- **Stimulus:** AgentBridge Cloud, общий project registry и broker отсутствуют.
- **Context:** две независимо управляемые реализации с допустимым заранее установленным либо заменяемым trust/discovery context.
- **Expected response:** стороны согласуют Native Binding и выполняют заявленный Core flow напрямую.
- **Measure:** обращения к project-operated mandatory online service — `0`; скрытых коммерческих разрешений — `0`.
- **Verification:** AA-4 dependency graph; AA-5 architecture assertion; AA-7 offline/direct interop run.
- **Gate:** AA-1/4/5/7.
- **Traceability:** FR-11, FR-68, FR-73–FR-75, FR-82–FR-85; NFR-7, NFR-15–NFR-17, NFR-27, NFR-28; EI-21, EI-22; SBC-09.

### QAS-DEC-02 — Замена внешнего provider

- **Stimulus:** сторона заменяет IdP, discovery, policy, evidence либо trust provider.
- **Context:** тот же Core/Profile claim при выполнении объявленного contract.
- **Expected response:** меняется только явно зависимый scope; Core semantics и независимая сторона не требуют proprietary changes.
- **Measure:** provider substitution сохраняет safety projection; hidden closed mapping/dependency — `0`.
- **Verification:** AA-3 trust/dependency contract; AA-5 substitution/fault tests.
- **Gate:** AA-1/3/5.
- **Traceability:** FR-11, FR-69–FR-74, FR-81, FR-91, FR-95; NFR-3, NFR-5–NFR-8, NFR-15, NFR-18, NFR-26, NFR-28, NFR-29; EI-16, EI-21–EI-23; SBC-05, SBC-09.

### QAS-DEC-03 — Локальная координация не становится глобальным центром

- **Stimulus:** operation требует shared-limit authority, effect owner, quorum либо policy authority.
- **Context:** конкретный resource/Profile.
- **Expected response:** coordinator явно ограничен ресурсом/scope; его отказ не делает его обязательной инфраструктурой для несвязанных Native flows.
- **Measure:** dependency graph не содержит единого runtime node/provider, обязательного для всех Native flows; undeclared blast-radius expansion — `0`.
- **Verification:** AA-2 ownership model; AA-3 boundary analysis; AA-6 integrated dependency review.
- **Gate:** AA-1/2/3/6.
- **Traceability:** FR-25, FR-36, FR-38, FR-50, FR-73, FR-74; NFR-7, NFR-10, NFR-15; EI-09, EI-18, EI-21; SBC-03, SBC-09.

## 10. Downstream numeric closure register

| Slot | Что должно быть заморожено | Gate |
| --- | --- | --- |
| QA-N01 | authority/revocation freshness и clock uncertainty по Profile | AA-3 |
| QA-N02 | privacy leakage/linkability/timing bounds и retention | AA-3/AA-4 |
| QA-N03 | exact operation/idempotency/evidence retention scope, explicit expired-dedup/effect-knowledge outcome и no-silently-fresh rule | AA-2/AA-3 |
| QA-N04 | payload, nesting, fan-out, frame, stream и message ceilings | AA-4 |
| QA-N05 | CPU, memory, state, storage, I/O, queue, retry, amplification и wait ceilings | AA-4 |
| QA-N06 | conformant benchmark scenario mix, throughput и latency percentiles | AA-4 |
| QA-N07 | version combinations, migration steps, rollback и Bridge expiry windows | AA-4 |
| QA-N08 | assertion/vector/model/review coverage, observer qualification и minimum two independently derived observer paths for every applicable protected disclosure/effect SBC outcome; disagreement handling | AA-5 |
| QA-N09 | independent implementation/interop claim scope | AA-5/AA-7 |

Ни один `TBD` не может быть заполнен произвольной целью после просмотра удобных результатов: значение и метод измерения замораживаются до соответствующего qualifying run.
