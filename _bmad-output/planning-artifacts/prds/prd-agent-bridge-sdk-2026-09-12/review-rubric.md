# PRD Quality Review — AgentBridge

## Overall verdict

PRD unusually strong in its safety posture, explicit non-goals, falsifiability, and preservation of the full protocol vision without authorizing production work. It is not yet safe to freeze as the sole basis for Gate 1: the central comparison is partly circular because Candidate C is required to satisfy a requirement system expressed in AgentBridge-native concepts, while the supposedly deterministic result function has overlapping outcomes and no economical early-exit path. Resolve the critical/high findings before freezing the detailed Validation Charter or authorizing experimental code.

**Finding count:** 1 critical, 7 high, 5 medium, 1 low.

## Decision-readiness — thin

The document makes real decisions rather than hiding them: the Native Core is a hypothesis; `profile`, `upstream`, and `stop` are legitimate results; Rust and all production mechanisms remain unselected; commercial layers cannot capture Core interoperability. OQ-1–OQ-8 also form an honest `No Start` boundary for experimental code.

The final decision machinery nevertheless has two material ambiguities. They do not prevent the next step of drafting a Charter, but they do prevent approving that Charter against this PRD without interpretation by its authors.

### Findings

- **high** The four-result function is not mutually exclusive (§4.9, FR-109; §11.1) — `profile` applies when the requirements can be expressed through an open profile, while `upstream` applies when one standard or a governed family can accept the semantics. Both can be true at once: a profile can close the gap now and also be a viable upstream contribution. `stop` can also overlap a technically possible but strategically unjustified profile. This conflicts with SM-6 and Gate 1 acceptance, which require exactly one result. *Fix:* define an ordered decision tree and tie-break rules, including whether the record may name one immediate direction plus a non-authorizing future recommendation.
- **high** Disproof of H-1 has no economical valid exit (§4.9, FR-102 and Gate 1 acceptance; §11.2 H-1; §11.5) — H-1 says a successful mapping to existing standards refutes the Native thesis and directs the project to `profile`, `upstream`, or `stop`, yet a “valid” Gate 1 still requires two independent implementations of each candidate, full F8 conformance, all topology/domain coverage, operations/performance/evolution measurements, and red-team review. The project could know that a Native Core is unnecessary but still be unable to record that result without building four implementations. *Fix:* preregister staged sufficiency/exit rules: an analytic/model-based refutation may authorize a bounded `profile`/`upstream`/`stop` decision; only surviving hypotheses advance to implementation and benchmark stages.

## Substance over theater — adequate

The vision, requirements, NFRs, and risk register are substantive. The document repeatedly distinguishes transport success from authority and external effect, includes negative consequences for almost every FR, and avoids market, adoption, and revenue theater. The stakeholder groups each correspond to a real job or downstream decision rather than decorative personas.

The main concern is not boilerplate but premature uniformity: all 110 FRs are treated as one undifferentiated mandatory surface even though Gate 1 is intended to discover the smallest irreducible core.

### Findings

- **high** The requirement catalogue does not distinguish evaluation invariants from proposed Core semantics and experiment-support capabilities (§4 F1–F9; SM-5; OQ-2) — `100%` coverage of FR-1–FR-95 makes every stated capability part of the admission bar before the Charter has identified the proposed irreducible subset. This can turn completeness into a proxy for value and pre-inflate the very thin waist H-2 is meant to test. *Fix:* classify each FR as `(a)` candidate-neutral safety/outcome invariant, `(b)` proposed irreducible Core semantic, `(c)` profile/binding/conformance support, or `(d)` later-stage requirement; require removal tests and an explicit reason before any `(b)` item enters Candidate N.
- **low** Historical coaching markers remain embedded throughout the operative document (status callouts after F1–F9 and §§5–11) — these are useful process history but add noise to a 2,000-line downstream artifact and can be mistaken for current handoff instructions. *Fix:* retain approval history in `.memlog.md`; replace the repeated markers with one concise approval/status block when the PRD is finalized.

## Strategic coherence — adequate

The PRD has a clear thesis: test whether a minimal, role-neutral semantic layer closes an irreducible safety/interoperability gap better than the strongest current composition. Features, metrics, counter-metrics, risks, and downstream gates all serve that thesis, and the document correctly separates technical evidence from adoption, demand, and revenue evidence.

The comparison target, however, is not yet independent enough from the proposed solution vocabulary. That threatens the evidentiary validity of the most important strategic decision.

### Findings

- **critical** Candidate C is judged against an AgentBridge-shaped oracle (§3 glossary; FR-1–FR-95; FR-101; H-1–H-2) — both candidates must pass “applicable FR-1–FR-95,” but those requirements are written with proposed AgentBridge abstractions such as `Negotiation Outcome`, `Decision Subject`, `Operation Identity`, `Effect State`, and the Core/Profile/Extension/Binding decomposition. Even though many consequences are genuinely solution-neutral, the current wording can count failure to reproduce AgentBridge’s conceptual model as an “irreducible gap.” That would make the Native result partly circular rather than demonstrate a missing industry outcome. *Fix:* before candidate design, create a candidate-neutral Evaluation Invariant Ledger stated only as observable stakeholder/safety outcomes and threat constraints, with provenance/rationale for each invariant. Map Candidate N and Candidate C independently to that ledger; differences in object model, layering, or names cannot score against either candidate unless they cause an observed invariant failure.

## Done-ness clarity — adequate

Most FRs are substantially better than conventional “shall support” requirements: each includes concrete positive and negative consequences, and feature-level validation gates define cross-implementation behavior. Numerical values that depend on environment are deliberately delegated to a frozen Charter, so their current absence is not itself a defect.

Two central measurements still lack enough semantic precision to prevent a false pass or false failure.

### Findings

- **high** “100% identical normative outcomes” does not define equality in the presence of permitted implementation choice (§8, SM-3 and SM-7; NFR-11; H-5) — NFR-11 permits bounded variability and implementation choice, while SM-3 requires identical outcomes. Two conformant implementations could select different permitted retry timing, diagnostic detail, optional extension handling, or valid scheduling and be counted as divergent; conversely, identical labels could hide different protected effects. *Fix:* make every vector define an equivalence class or allowed outcome set plus required/forbidden observations; compare semantic projections and side effects, not literal outcome identity.
- **medium** The zero-effect hard gate depends on an unqualified observer (§7.1; §8 SM-2; §10.1; OQ-3) — the PRD requires an independent effect observer but does not require evidence that it can see every protected disclosure or consequential effect in the vector’s scope. “Zero observed” can therefore mean “not instrumented.” *Fix:* the Charter must include an observer coverage model, control injections that prove detection, false-negative handling, and an `invalid/inconclusive` outcome when observation completeness is not established.

## Scope honesty — adequate

Exclusions are unusually explicit: no production specification, SDK, cloud, real payment, market claim, or public release is authorized; private sources remain protected. The document also acknowledges absent company access and distinguishes code/genealogy independence from real organizational adoption.

The Gate 1 workload is nevertheless too large to function as a minimal uncertainty-reduction experiment unless it is decomposed before OQ-7 is filled.

### Findings

- **high** Gate 1 is a full protocol program disguised as one validation gate (§7.1; F8 gate; FR-102–FR-107; VC-1–VC-13) — it requires four independent candidate implementations, at least two runtime environments, six topology classes, three domains, multiple modes, two profiles, extensions, native bindings and external bridges, a full conformance suite, fuzz/property/concurrency/fault/resource testing, performance benchmarking, migration analysis, and red-team review. With no company access and acknowledged small-team role overlap (R-15), the likely result is `No Start`, prolonged research, or shallow evidence across a huge matrix. *Fix:* decompose Gate 1 into preregistered substages with separate evidence sufficiency: 1A neutral invariants/gap mapping, 1B minimal model and adversarial counterexamples, 1C independent interop for only surviving semantics, 1D comparative operations/performance. Preserve all hard safety gates for any code that runs, but do not require downstream substages after an earlier hypothesis is validly falsified.
- **medium** Independence is sometimes organizationally implied but operationally simulated (§11.6; A-7; R-8) — the same person may occupy conflicting roles using “independent review contexts.” The document discloses the limitation, but labels such as “Independent Security Reviewer” or “Fresh-context Red Team” can still overstate the evidence. *Fix:* define graded independence levels (separate context, separate author, separate organization), require claims to name the achieved level, and reserve “independent review” without qualification for at least a separate human/organization as appropriate to the later gate.

## Downstream usability — thin

The glossary is strong, FR/NFR/SM/VC/H/R/A/DQ identifiers are contiguous and unique, and the explicit boundaries will help Architecture avoid inheriting early technology choices. For a chain-top artifact, however, downstream extraction is impaired by the document’s size and by the absence of an authoritative traceability structure.

### Findings

- **high** The PRD requires 100% coverage without providing a traceability contract (§4 feature gates; SM-5; §11.4; §11.7 OQ-3) — prose cross-references do not establish which hypothesis, risk, scenario, oracle, metric, and gate disposition proves each FR/NFR. A later team could reach “100%” using a different interpretation, or silently omit compound constraints that appear only in a consequence bullet. *Fix:* require a machine-checkable matrix with one row per stable FR/NFR and columns for invariant class, H/R source, VC vectors, positive/negative oracle, observer, metric, hard/soft status, candidate applicability, and evidence artifact; zero unmapped mandatory rows is a Charter freeze condition.
- **medium** The assumptions register does not round-trip all consequential design assumptions (§13.2 versus F8/F9 and §11) — A-1–A-12 omit, among others, that two implementations are enough evidence, that three domains adequately stress the thin waist, that effect observers can establish absence of effects, and that comparable implementation maturity/environments can be achieved. These assumptions drive the experiment but appear as fixed requirements rather than testable premises. *Fix:* add them to the assumptions register with owner, validation method, failure consequence, and gate at which they expire or must be revisited.
- **medium** “Public artifacts” conflicts with the current private-only publication boundary (§5.1–§5.3; FR-83; F8 gate; NFR-17; §10.4) — Gate 1 repeatedly requires implementations “from public Normative Artifacts” and a “public Suite,” while the PRD and experimental results remain local/private until explicit approval. This is resolvable, but an implementer cannot tell whether publication is a Gate 1 prerequisite or merely a later property test. *Fix:* distinguish `frozen shared artifacts available equally to Gate 1 implementers` from `publicly released artifacts`; test legal/technical publishability in Gate 1 without publishing until the owner approves.

## Shape fit — adequate

A capability/requirements shape is appropriate for an infrastructure protocol; consumer journeys would be artificial, and the JTBD set has named technical protagonists and observable evidence. The PRD also correctly avoids pretending that the indirect human principal uses AgentBridge directly.

The artifact has nevertheless crossed from a distilled product-requirements document into an early semantic specification and validation plan, which weakens its role as a stable decision spine.

### Findings

- **high** One 2,083-line PRD combines product thesis, 110 detailed behavioral requirements, feature-level conformance gates, NFRs, a Charter skeleton, risk register, downstream gate policy, and commercial governance (§§0–15) — this makes source extraction expensive, increases semantic drift risk, and encourages Architecture/Spec work to treat provisional AgentBridge concepts as already normative. *Fix:* keep the PRD as the stable outcome/decision layer; move the detailed FR consequence catalogue into a versioned requirements appendix and the H/VC/experiment mechanics into the Validation Charter, preserving stable IDs and bidirectional links. This is a packaging change, not a scope reduction.

## Mechanical notes

- **IDs:** FR-1–FR-110, NFR-1–NFR-29, SM-1–SM-14, VC-1–VC-13, H-1–H-7, R-1–R-18, A-1–A-12, and DQ-1–DQ-8 are contiguous and unique. No out-of-range FR cross-reference was found. OQ-1–OQ-8 are a numbered list rather than table definitions but are complete.
- **Glossary:** Domain nouns are defined comprehensively. Mixed Russian/English capitalization (`Core/core`, `Binding/binding`, `Domain Profile/Доменный профиль`, `Authority/полномочие`) is readable but should be normalized before a normative handoff.
- **Assumptions roundtrip:** there are no inline `[ASSUMPTION]` markers. Section 13 is explicit, but the unindexed assumptions identified above make the roundtrip incomplete.
- **UJ shape:** no user journeys are present; this is appropriate for the current protocol-capability PRD. JTBD-1–JTBD-8 each name a technical protagonist and evidence of success.
- **medium** The absolute freshness date will become stale (§11.3 Candidate C) — “not later than 2026-10-12” does not define what happens when the Charter is frozen later. *Fix:* use a maximum source-age window relative to Charter freeze and rerun date, while retaining the Gate 0 evidence date in history.
- **Frontmatter/status:** `status: draft` and the closing reviewer-gate marker are correct during review; both must be updated only after findings are triaged. The document contains no PRD-workspace `addendum.md`; the available reconciliation reports are review inputs, not an addendum to validate.

## Final re-review after fixes — 2026-09-13

Этот раздел оценивает текущую редакцию `prd.md` целиком и заменяет первоначальный verdict выше для решения о финализации. Проверка выполнена по семи измерениям `prd-validation-checklist.md` с особым вниманием к Essential Spine, непротиворечивости, стабильности ID, disposition открытых вопросов, phase blockers и validation-first scope.

### Итоговый verdict — PASS для финализации PRD

Текущий PRD пригоден как основание **только** для подготовки и утверждения подробного Validation Charter. Он не разрешает production-спецификацию, Architecture, SDK, публичный release или production-код до применимого Gate 1 Decision Record (§0.2, §7.2–§7.4, FR-110, §11.8). Небезопасной неоднозначности, которая могла бы дать положительный Gate 1 при неполных доказательствах, не обнаружено.

**Текущие блокирующие findings:** 0 Critical, 0 High.

### 1. Decision-readiness — strong

- FR-109 задаёт взаимно упорядоченную функцию `Validity → Eligibility → Existing-surface resolution → Native resolution → No viable direction`, разрешает ровно одно немедленное направление и отделяет `Gate Closed / No Decision` от продуктового результата.
- FR-110 и F9/Gate 1 acceptance ограничивают разрешённый следующий этап для каждого исхода. `native` требует полного 1A–1D, окончательные `profile/upstream` требуют 1C для выбранной C-семантики, а недостаток evidence не превращается в `stop`.
- §11.5 вводит экономичный, но доказательный early-exit: после воспроизводимого опровержения H-1 полный Native benchmark можно не строить; одного описательного сравнения документов недостаточно.

### 2. Substance over theater — strong

- §2.5, FR-96 и §11.7.1 требуют Allocation каждого FR/NFR и Removal Test каждого proposed Core элемента. Тем самым полнота Native Candidate больше не означает автоматического включения всех требований в Core.
- NFR и hard gates привязаны к предметным угрозам, наблюдаемым последствиям и evidence; рыночные, adoption- и финансовые обещания явно вынесены за Gate 1 (§6.2, §6.4, §8.5, A-10, Gates 3–5).
- Исторические coaching/checkpoint-отметки удалены из оперативного тела; остался только уместный `mode: coaching` во frontmatter.

### 3. Strategic coherence — strong

- Центральный тезис сформулирован одинаково в §0.1–§0.2, G-1–G-6, H-1–H-7 и §11.1: доказать либо опровергнуть отдельную нормативную роль AgentBridge, не назначая Native победителем заранее.
- Circular-oracle риск закрыт: candidate-neutral Evaluation Invariant Ledger должен быть заморожен до candidate-specific design, иметь независимое основание и не штрафовать различия object model/layering сами по себе (FR-96–FR-98, §11.5 Gate 1A, §11.7.1–§11.7.2).
- Composition Challenger Set и Primary C отделяют полный практический benchmark от обязательной попытки опровергнуть H-1 всеми жизнеспособными недоминируемыми конфигурациями (FR-97, §11.3).

### 4. Done-ness clarity — strong

- FR-1–FR-110 имеют проверяемые следствия; SM-1–SM-14 и VC-1–VC-13 задают измеримые доказательства, а значения, зависящие от среды, обязаны быть preregistered до кода (§8.1, FR-96, §11.7.3).
- Ложная literal-equality устранена: FR-86, SM-3, SM-7 и H-5 сравнивают allowed outcome sets/equivalence classes и safety-critical projections.
- Ложный zero-effect pass блокируется FR-101, SM-2, §10.1 и §11.7.2: полнота observer калибруется positive controls/canaries, а слепота или сбой дают `invalid/inconclusive`, не pass.

### 5. Scope honesty — strong

- §6.3–§6.4 и §7.3 явно исключают агентов/UI, production specification/Architecture/SDK, реальные последствия, коммерческие слои, market proof и публикацию приватных материалов.
- Gate 1 разложен на 1A–1D; работа выполняется только для surviving hypotheses, а §7.5 связывает любое расширение scope с конкретной гипотезой, hard gate, метрикой или falsification criterion.
- Ограниченная независимость малой команды честно названа в §11.6 и A-7; внешняя организационная независимость и adoption не имитируются и отложены до Gates 3–4.

### 6. Downstream usability — adequate

- §0.3 даёт два явных маршрута чтения и формирует Essential Spine: проблема/текущее решение (§0), видение (§1), capability map (§2), пользователи/JTBD (§5), цели/нецели (§6), Gate 1 scope (§7), метрики (§8), решения/предположения (§13), открытые решения (§14) и последующие gates (§15). Детальные FR/NFR и Charter-контракт отделены структурно, хотя остаются в одном файле.
- FR-96 и §11.7 требуют machine-checkable Requirement Allocation/traceability contract с нулём неразмещённых обязательств до freeze. Различие между frozen shared и фактически public artifacts теперь явно закреплено в §7.3, NFR-17 и §11.7.4.
- Определения и диапазоны ID сохранены: FR-1–FR-110, NFR-1–NFR-29, SM-1–SM-14, SM-C1–SM-C8, VC-1–VC-13, H-1–H-7, R-1–R-18, A-1–A-12, OQ-1–OQ-8 и DQ-1–DQ-8 непрерывны и уникальны; числовых ссылок вне диапазонов не найдено.

### 7. Shape fit — adequate

- Для chain-top протокольного продукта capability/requirements shape уместнее искусственных consumer journeys. JTBD-1–JTBD-8 задают технических protagonists и наблюдаемый результат, а Validation Charter отделён как следующий подробный артефакт (§11, §11.8).
- Документ остаётся объёмным, но §0.3 создаёт извлекаемый decision spine, стабильные ID позволяют ссылаться на нормативные обязанности, а подробность не выдаётся за утверждённую production-спецификацию. Физическое разбиение на PRD и versioned requirements appendix можно выполнить позже как редакционную упаковку с сохранением ID; это не blocker текущей финализации.

### Essential Spine, противоречия и open-item disposition

- **Essential Spine:** присутствует и достаточен для принятия решения; §0.2 и §11.8 однозначно разрешают только Charter, а §0.3 указывает минимальный путь чтения.
- **Противоречия:** материальных противоречий между vision, scope, metrics, FR/NFR и Gate logic не обнаружено. Остаточное употребление старого единственного термина `Strongest Composition Challenger` (§2.3, ввод F9, G-5, §7.1 и определение Irreducible Gap) следует при следующей редакционной правке нормализовать до `Composition Challenger Set` / `Primary C`; FR-97, FR-109 и §11.3 уже однозначно задают нормативную логику, поэтому это не Critical/High ambiguity.
- **Open items:** OQ-1–OQ-8 имеют owner и срок возврата, блокируют экспериментальный код, но обоснованно не блокируют финализацию PRD (§14.1). DQ-1–DQ-8 явно отложены до Gate 1 или последующих ворот (§14.2). Незаполненный обязательный параметр означает `No Start` (§11.7, §14.1).
- **Phase blockers:** цепочка разрешений непрерывна: finalized PRD → frozen Charter → disposable Gate 1 artifacts → применимый Decision Record → только соответствующая ветвь дальнейшей работы (§7.4, FR-110, §11.8, §15.2). Production work, public release и claims не могут обойти эту цепочку.
- **Validation-first fit:** scope измеряет наличие irreducible gap, безопасность, независимую реализуемость и practical advantage; не пытается одновременно доказать adoption, рынок, monetization или production readiness (§6–§8, §15.1).

### Оставшиеся проблемы

Critical/High проблем нет. Перед будущим нормативным handoff полезны только неблокирующие редакционные действия: унифицировать `Strongest Composition Challenger` с актуальной терминологией Set/Primary C и при необходимости вынести подробный каталог требований в versioned appendix без перенумерации стабильных ID.
