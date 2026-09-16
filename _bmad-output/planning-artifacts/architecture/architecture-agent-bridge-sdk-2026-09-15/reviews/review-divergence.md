# AA-1 adversarial divergence review

- **Lens:** adversarial — independently built units obeying the adopted decisions literally
- **Scope:** `ARCHITECTURE-SPINE.md` and all listed AA-1 companions
- **Method:** documentary thought experiment only; no executable model, prototype, benchmark or reference code was created
- **Reviewer independence:** internal AI/fresh-context review (`I1` only; not external recognition)
- **Review date:** 2026-09-16
- **Verdict:** `redesign/block` for AA-1 closure until the unsafe AA-2 underconstraints below have explicit owners, normative outputs and acceptance checks. This does not reopen the Native Architecture-First course decision.

## Adversarial construction

Two teams receive only the current AA-1 package and are asked to build the first documentary units one level below it:

- **Unit North — graph/event model.** Immutable facts and transitions form an event graph. Roles are message-scoped, an operation spans all retries, authority is computed by intersecting every applicable grant, outcomes are an orthogonal knowledge projection, Extensions are declarative constraints, and external providers return attributed claims only.
- **Unit South — aggregate/actor model.** Each interaction is a mutable aggregate with actor-local state. Roles persist for an interaction epoch, each processing attempt is an operation linked to a logical action, independent grants are unioned unless a deny applies, outcomes are lifecycle states, Extensions are ordered handlers, and provider adapters return normalized decisions.

Both units can satisfy the literal text that Core is role-neutral, authority attenuates, delivery differs from effect, unknown is not success, exact artifact closure is pinned, higher layers do not weaken Core, dependencies are explicit and critical loss fails closed. They nevertheless disagree on the same normative inputs. The disagreement is therefore not implementation freedom: it changes shared data, allowed transitions, authority, observable errors or the safety projection.

### Deferral classification used below

- **Unsafe AA-2 underconstraint:** AA-1 does not allocate a necessary semantic decision, define its acceptance property, or require a normative artifact capable of preventing divergence.
- **Intentional AA-1 deferral; mandatory AA-2 closure:** AA-1 correctly avoids choosing exact states or mechanisms now, but the item must become normative before separately built AA-2 units can be accepted.
- **Proper downstream mechanism deferral; abstract seam required earlier:** concrete technology or threat parameters belong to AA-3/AA-4/AA-5, while AA-2 must still fix the mechanism-independent meaning at the seam.

## Findings

### DVG-01 — No normative abstract data model for shared semantic objects

- **Classification:** Unsafe AA-2 underconstraint.
- **Location:** `ARCHITECTURE-SPINE.md` — Design Paradigm, AD-1/AD-2 and Consistency Conventions; `GLOSSARY-AND-CONTEXT.md` — §§2–8; `BASELINE-V0X-SCOPE.md` — §§3.1–3.12.
- **Trigger condition:** North represents participants, constraints and evidence as typed immutable relations with absent values distinct from explicit unknown; South represents nested records in which missing, null and empty are equivalent and list order is retained. Both preserve every named distinction, yet equality, cardinality, presence, ordering, numeric ranges, time values and identifier comparison differ.
- **Guard snippet:** Require AA-2 to publish a normative abstract information model, independent of wire encoding, with primitive semantic domains, presence/null/unknown rules, cardinalities, set/list/map ordering, equality, numeric and time domains, identifier comparison, graph-reference integrity, and invalid-value outcomes. Assign stable assertion IDs and make this model an input to every Profile, Extension and Binding mapping.
- **Potential consequence:** Two implementations accept different objects, calculate different Decision Subjects and artifact closures, or emit non-equivalent evidence before any Binding choice is involved.

### DVG-02 — Role assignment and semantic ownership have no transition relation

- **Classification:** Intentional AA-1 deferral; mandatory AA-2 closure, but current AA-2 acceptance criteria are too weak.
- **Location:** `GLOSSARY-AND-CONTEXT.md` — §2 Participants, roles and addressing; `BASELINE-V0X-SCOPE.md` — §3.1; `ARCHITECTURE-SPINE.md` — Deferred table, “Exact participant ... state machines.”
- **Trigger condition:** North permits one Participant to assume different roles per message and branch; South binds roles at Interaction-Epoch creation until an explicit membership transition. Both are role-neutral and do not infer roles from transport position, but they disagree whether a late responder may become Executor or Approver without opening a new epoch.
- **Guard snippet:** Make AD-4/AA-2 define the normative `Participant × Context × Role × Epoch` relation, who may create/change/end it, controller-equivalence and independence tests, allowed multi-role combinations, role-transition preconditions, and the effect of stale or conflicting role claims. Require swap and late-role conformance traces for every mandatory topology.
- **Potential consequence:** A party is authorized, counted toward quorum or exposed to data in one conforming unit and rejected in another.

### DVG-03 — Decision Subject equality and refinement are descriptive, not decidable

- **Classification:** Unsafe AA-2 underconstraint.
- **Location:** `GLOSSARY-AND-CONTEXT.md` — Decision Subject; `BASELINE-V0X-SCOPE.md` — §3.4; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-SEC-03 and QAS-COR-01.
- **Trigger condition:** North treats every field named in the glossary, including deadlines and schema version, as identity-bearing; South lets a Profile classify fields as noncritical and preserves identity when amount, time or audience is “narrowed.” Both can claim immutable meaning and bounded refinement, but they disagree whether an existing Consent, Approval or Authorization remains reusable.
- **Guard snippet:** Require AA-2 to define a total semantic projection for Decision Subject identity, a Profile-authorized criticality registry, exact equality and collision rules, and a refinement partial order that states which dimensions may narrow, who authorizes that policy, and when refinement creates a new subject. Any unclassified change MUST create a new Decision Subject.
- **Potential consequence:** Stale authority is replayed over changed meaning, or harmless narrowing is rejected inconsistently, breaking both safety and interoperability.

### DVG-04 — Operation Identity contradicts the retry/attempt boundary

- **Classification:** Unsafe AA-2 underconstraint caused by a local terminology conflict.
- **Location:** `GLOSSARY-AND-CONTEXT.md` — Operation ID (“one attempt/unit”) and Retry; `ARCHITECTURE-SPINE.md` — Constitutional Safety Rule 5; `BASELINE-V0X-SCOPE.md` — §3.6.
- **Trigger condition:** North keeps one Operation ID across every retry because Rule 5 says retry preserves Operation Identity. South creates a new Operation ID for each processing attempt because the glossary defines it as one attempt, linking attempts under an application action. Both preserve the same Decision Subject and prevent duplicate effect, but outcome retrieval and replay classification differ.
- **Guard snippet:** In AA-2, separate and normatively name logical operation, delivery attempt and execution attempt identities; define creation, reuse, correlation, retention and terminality for each. Replace the ambiguous glossary sentence and require identical retry/replay traces to resolve to the same logical operation and effect key.
- **Potential consequence:** The same retry is treated as a duplicate by one implementation and as a fresh executable operation by another.

### DVG-05 — Authority attenuation lacks a composition algebra

- **Classification:** Unsafe AA-2 underconstraint.
- **Location:** `ARCHITECTURE-SPINE.md` — Safety Rules 2, 6 and 8; `GLOSSARY-AND-CONTEXT.md` — Authority and Delegation; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-SEC-02 and QAS-SEC-05.
- **Trigger condition:** North intersects every grant in a lineage and treats incomparable constraints as non-permit; South unions separately issued capabilities for the same Actor and applies explicit denies afterward. Each individual delegation narrows its parent, so both satisfy literal monotonic attenuation, yet their effective authority differs when grants, caveats, shared limits or policy decisions compose.
- **Guard snippet:** AD-5/AA-2 must define the authority domain and derivation relation, grant aggregation, deny/conflict precedence, lineage selection, caveat conjunction, actor/controller equivalence, shared-limit reservation semantics, and the required result for incomplete or incomparable inputs. State monotonicity as a proof obligation over the composed effective authority, not only per delegation edge.
- **Potential consequence:** One unit permits an effect or disclosure that another correctly treats as unauthorized; aggregate amount/use limits may also be consumed differently.

### DVG-06 — Freshness and epoch are named without an abstract ordering model

- **Classification:** Proper AA-3 mechanism/parameter deferral; abstract seam required in AA-2.
- **Location:** `GLOSSARY-AND-CONTEXT.md` — Epoch, Authority and Context Boundaries; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-SEC-03 and downstream slot QA-N01; `ASSETS-ADVERSARIES-BOUNDARIES.md` — TM-07.
- **Trigger condition:** North decides freshness from monotonic issuer epochs and rejects incomparable views; South decides it from signed wall-clock validity plus a cached revocation window. The exact window is correctly deferred to AA-3, but the package does not say whether freshness is an ordered event relation, a timestamp predicate, a provider claim or a conjunction.
- **Guard snippet:** AA-2 must define mechanism-neutral epoch/freshness state, comparison outcomes (`current`, `stale`, `future`, `incomparable`, `unknown`), the transitions they gate, and the no-permit rule for non-current security material. AA-3 may then choose clocks, revocation windows and provider assumptions without changing those meanings.
- **Potential consequence:** The same grant remains usable after a policy/key/membership change in one implementation but not another.

### DVG-07 — Lifecycle and outcome are not fixed as one machine or orthogonal projections

- **Classification:** Intentional AA-1 deferral; mandatory AA-2 closure.
- **Location:** `ARCHITECTURE-SPINE.md` — Safety Rules 3–5 and Deferred table; `GLOSSARY-AND-CONTEXT.md` — §6; `BASELINE-V0X-SCOPE.md` — §3.6; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-COR-01–03.
- **Trigger condition:** North models delivery, authorization, execution, effect and knowledge as separate monotone dimensions, allowing `execution=completed` with `effect=unknown`. South uses a flat state enum in which `unknown`, `partial` and `conflict` are lifecycle states and may be terminal. Both keep the named distinctions and never call unknown success, but they accept different transitions and recovery updates.
- **Guard snippet:** AD-6/AA-2 must publish the normative product state space (or a proven equivalent), initial and terminal states, transition ownership, legal concurrency, precedence and merge rules, knowledge refinement, late evidence, crash recovery and projections exposed to Profiles. Every state/transition needs an assertion ID and forbidden-transition falsifier.
- **Potential consequence:** Identical late-completion, cancellation or conflicting-evidence traces yield incompatible outcomes and retry decisions.

### DVG-08 — The effect-boundary “proved atomic equivalent” has no protocol proof obligation

- **Classification:** Proper AA-3 enforcement-mechanism deferral; abstract safety obligation required in AA-2.
- **Location:** `ARCHITECTURE-SPINE.md` — AD-1 and Safety Rule 2; `ASSETS-ADVERSARIES-BOUNDARIES.md` — BND-06 and Boundary Invariants 1–2; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-SEC-03.
- **Trigger condition:** North calls a compare-and-swap over local authority epoch and effect record an atomic equivalent. South accepts a signed lease checked earlier by an external executor. Both document a rationale, but no common linearization event, stale-input relation or failure observation determines whether either proof is adequate.
- **Guard snippet:** AA-2 must state the abstract commit relation and invariant: exact Decision Subject, effective authority, relevant epochs/preconditions and effect identity are validated at one named linearization point or by a refinement proof preserving the same allowed traces. AA-3 then defines trusted components and compromise assumptions; AA-5 validates candidate realizations.
- **Potential consequence:** A TOCTOU window can be called conformant by one team and forbidden by another, defeating the zero-tolerance post-revoke requirement.

### DVG-09 — Error semantics do not determine interoperable failure observations

- **Classification:** Unsafe AA-2 underconstraint.
- **Location:** `ARCHITECTURE-SPINE.md` — Consistency Conventions, State and failure; `BASELINE-V0X-SCOPE.md` — §§5.1–5.2; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-SEC-01, QAS-PRI-03 and QAS-IMP-03.
- **Trigger condition:** North emits distinct normative outcomes for `authority-denied`, `authority-unknown`, `dependency-unavailable` and `closure-incompatible`; South emits one privacy-preserving `non-permit` plus optional local diagnostics. Both are typed internally and avoid leaking secrets, but peers cannot agree on retryability, terminality, assertion failure or required evidence.
- **Guard snippet:** AA-2 must define a normative error algebra separated from wire codes and diagnostics: category, safety projection, retryability, terminality, disclosure class, causal subject and evidence requirements. Define a privacy projection that permits deliberate coarsening without changing internal normative outcome; AA-4 Bindings map this algebra to wire errors.
- **Potential consequence:** Peers retry permanent denial, stop on recoverable dependency failure, leak policy state, or produce incomparable conformance results.

### DVG-10 — Profile selection and composition have no deterministic semantics

- **Classification:** Unsafe AA-2 underconstraint.
- **Location:** `ARCHITECTURE-SPINE.md` — AD-2 Profiles; `GLOSSARY-AND-CONTEXT.md` — Profile and Negotiated Set; `BASELINE-V0X-SCOPE.md` — §§5 and 11.
- **Trigger condition:** North permits multiple Profiles and intersects their restrictions; South selects exactly one primary Profile and treats additional Profiles as advisory named scopes. Both only narrow Core and can form a Negotiated Set, but they disagree when Information Exchange and Consequential Action obligations apply to one mixed interaction or branch.
- **Guard snippet:** AA-2 must specify Profile applicability, multiplicity, composition operator, conflict detection, precedence prohibition/allowance, inheritance, branch scoping and deterministic selection. A Profile manifest MUST declare compatible and incompatible peers and the composed result MUST be independent of declaration order.
- **Potential consequence:** A consequential disclosure or action is governed by the stronger Profile in one implementation and by a weaker or different Profile in another.

### DVG-11 — Extension composition is underspecified beyond namespace collision

- **Classification:** Intentional AA-1 deferral signaled by QAS-EVO-02, but mandatory AA-2 closure lacks explicit composition acceptance criteria.
- **Location:** `ARCHITECTURE-SPINE.md` — AD-2 Extensions; `BASELINE-V0X-SCOPE.md` — §6; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-EVO-02.
- **Trigger condition:** North interprets Extensions as commutative declarative constraints over Core state; South runs namespaced Extension handlers in negotiated order. Two non-redefining Extensions independently narrow allowed behavior, yet `X then Y` permits a different transition from `Y then X`; neither is a namespace collision or literal Core redefinition.
- **Guard snippet:** AA-2 must define Extension semantic inputs/outputs, composition and evaluation order, conflict and dependency resolution, criticality authority, whether commutativity is required, and the result of non-commuting combinations. Require pairwise and dependency-chain composition vectors, not only unknown/collision fixtures.
- **Potential consequence:** The same negotiated Extension closure produces different authority, state mutation or errors across implementations.

### DVG-12 — Artifact/dependency closure is pinned but not computable

- **Classification:** Unsafe AA-2 underconstraint affecting evolution and every conformance claim.
- **Location:** `ARCHITECTURE-SPINE.md` — AD-2 paragraph after dependency diagram and Consistency Conventions; `ADR-DISCIPLINE.md` — Stable identity and states; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-EVO-01.
- **Trigger condition:** North defines the operation closure as the transitive Core/Profile/Extension/Binding graph resolved at negotiation time. South also includes relevant Bridge, external-contract and security-policy artifacts, resolved at authorization time. Both pin exact versions and digests for the operation lifetime and never edit released meaning, but they pin different sets and disagree after a dependency update.
- **Guard snippet:** Define a normative closure manifest schema and deterministic closure algorithm: root artifact, included kinds, transitive edge types, optional/conditional dependencies, cycle and missing-node handling, digest domain, resolution instant, operation inheritance, policy/external-contract references, invalidation and safe migration. Require equal closure roots to imply equal member sets and semantics.
- **Potential consequence:** A claim remains valid under one implementation after a security-critical dependency changes while another correctly invalidates it; negotiation can also split-view without detectable root mismatch.

### DVG-13 — Version compatibility and negotiation do not define one selected result

- **Classification:** Proper AA-4 representation/mechanism deferral; compatibility semantics and deterministic selection are required in AA-2.
- **Location:** `GLOSSARY-AND-CONTEXT.md` — Negotiated Set; `BASELINE-V0X-SCOPE.md` — §§3.2 and 3.9; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-SEC-04, QAS-EVO-01 and downstream slot QA-N07.
- **Trigger condition:** North accepts the highest common semantic-major version and declared compatible minors; South requires exact artifact-version equality unless an explicit migration artifact is present. Both make incompatibility visible and do not silently drift, but identical offers produce refusal in one unit and a selected set in the other.
- **Guard snippet:** AA-2 must define the compatibility relation, offer/require constraints, total deterministic selection and tie-breaking, transcript equivalence, downgrade floor, no-common-set outcome and whether migrations participate before or after selection. AA-4 may set concrete version encodings and supported combination budgets.
- **Potential consequence:** Honest peers fail to connect or, worse, select different Profile/Extension/Binding closures from the same negotiation transcript.

### DVG-14 — Binding-to-Core semantic handoff is not typed

- **Classification:** Proper AA-4 mechanism deferral; abstract port required in AA-2/AA-3.
- **Location:** `ARCHITECTURE-SPINE.md` — AD-2 Bindings and Structural Seed; `BASELINE-V0X-SCOPE.md` — §7; `ASSETS-ADVERSARIES-BOUNDARIES.md` — BND-10.
- **Trigger condition:** North's Binding returns attributed transport/security claims that Core verifies under a trust rule. South's Binding returns a normalized authenticated Participant and freshness decision. Both preserve semantics and do not equate transport success with effect, but compromise, termination and covered-field behavior changes what Core treats as established.
- **Guard snippet:** Specify a mechanism-independent Binding port with typed untrusted input, authenticated-coverage claims, issuer/terminator identity, freshness/epoch evidence, ambiguity/loss flags, resource observations and failure categories. State which outputs are facts, claims or policy inputs and which Core transitions they may never authorize alone. AA-3 defines trust obligations; AA-4 chooses mechanisms.
- **Potential consequence:** The same channel authentication is treated as identity or authority in one implementation and merely as evidence in another.

### DVG-15 — Bridge subject, actor and provenance transformation can change authority

- **Classification:** Unsafe abstract Bridge underconstraint; concrete foreign mappings are correctly deferred.
- **Location:** `ARCHITECTURE-SPINE.md` — AD-2 Bridges; `BASELINE-V0X-SCOPE.md` — §8; `GLOSSARY-AND-CONTEXT.md` — Bridge; `ASSETS-ADVERSARIES-BOUNDARIES.md` — BND-11.
- **Trigger condition:** North makes the Bridge the Presenter and preserves the foreign principal only as an attributed subject. South maps the foreign principal into a native Participant/Actor while recording Bridge provenance. Both expose loss, cap assurance and fail on critical unmapped fields, yet native authority and audience checks operate on different actors.
- **Guard snippet:** Before concrete mappings, AA-2 must define the Bridge transformation contract for Principal/Participant/Actor/Presenter/Audience, Decision Subject identity, Operation identity, epochs and evidence provenance. Impersonation MUST be prohibited unless a named Profile defines a verifiable equivalence relation; otherwise the Bridge remains the Presenter and translated claims retain their foreign issuer and assurance ceiling.
- **Potential consequence:** A Bridge can gain native authority by identity substitution even though no field is visibly “lost” and assurance is nominally capped.

### DVG-16 — External dependency contracts list fields but not behavioral semantics

- **Classification:** Proper AA-3 provider/trust deferral; abstract failure and substitution state model required earlier.
- **Location:** `ARCHITECTURE-SPINE.md` — AD-2 External Systems and Dependency convention; `BASELINE-V0X-SCOPE.md` — §§3.11 and 9; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-RES-02 and QAS-DEC-02.
- **Trigger condition:** On policy-provider outage, North invalidates every dependent permit and returns `unknown`; South continues using a previously attributed policy claim until its declared expiry. Both explicitly declare availability, trust and failure semantics and neither expands authority, but the same operation has different progress and claim impact under the same outage.
- **Guard snippet:** AA-2 must define the external-dependency contract state machine: request/claim identities, freshness states, cache/reuse authority, outage/partition/compromise transitions, dependent-operation projection, termination and substitution equivalence. AA-3 supplies trust and compromise assumptions; a Profile may select stricter behavior but not invent the base meanings.
- **Potential consequence:** Provider failure changes permitted execution, retries and evidence validity incompatibly, while both implementations still claim provider replaceability.

### DVG-17 — Evidence aggregation and conflict handling lack a trust algebra

- **Classification:** Intentional AA-1 deferral; mandatory AA-2/AA-3 split must be made explicit.
- **Location:** `ARCHITECTURE-SPINE.md` — Safety Rule 9 and Evidence convention; `GLOSSARY-AND-CONTEXT.md` — §7; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-COR-03.
- **Trigger condition:** North computes aggregate assurance as the minimum of provenance-chain assurances and preserves every contradictory claim. South selects the freshest claim from the Profile's qualified effect attestor and records other claims as superseded. Both keep provenance and use a declared trust rule, but one returns Conflict and the other Success for the same evidence set.
- **Guard snippet:** AA-2 must define evidence graph structure, subject/causal matching, completeness and redaction markers, conflict predicate, monotone knowledge refinement and the limited operations a Profile trust rule may perform. AA-3 may qualify issuers and assurance assumptions, but no rule may erase an applicable contradiction without a normative resolution relation and retained provenance.
- **Potential consequence:** Identical evidence authorizes retry, compensation or final success differently and can inflate assurance through selection rather than proof.

### DVG-18 — Multi-party membership, branch and quorum epochs do not compose deterministically

- **Classification:** Intentional AA-1 deferral; mandatory AA-2 closure.
- **Location:** `BASELINE-V0X-SCOPE.md` — §§3.3 and 3.8; `GLOSSARY-AND-CONTEXT.md` — Branch and Epoch; `QUALITY-ATTRIBUTE-SCENARIOS.md` — QAS-SEC-05, QAS-PRI-02 and QAS-DEC-03.
- **Trigger condition:** North freezes membership and quorum at operation creation and requires a new operation after join/leave. South allows a membership-epoch transition before commit and re-evaluates existing approvals under the new set. Both bind aggregates to an epoch, preserve branch privacy and prevent overspend, but the same approvals reach quorum in only one unit.
- **Guard snippet:** AD-4/AD-5/AD-6 at AA-2 must define membership ownership, epoch creation and ordering, branch fork/join rules, approval validity across epoch changes, quorum snapshot point, aggregate merge, split-view outcome and privacy-safe evidence. Require join/leave/commit interleaving traces across multi-party and delegated topologies.
- **Potential consequence:** One implementation commits an effect with a quorum that another considers stale or incomplete.

## What is legitimately deferred

The following choices do **not** need to be pulled into AA-1 or prematurely selected in AA-2: wire encoding and framing, concrete canonical byte representation, transport, cryptographic algorithms/suites/providers, clock tolerances and numeric freshness windows, runtime language, repository shape, exact resource ceilings, concrete foreign-protocol mappings, conformance tooling, and production domain/SLO. The package correctly assigns those mainly to AA-3–AA-5 or later.

The review finding is narrower: those later choices need stable abstract ports and semantic obligations from AA-2. Otherwise a later Binding, Bridge, Profile or provider will silently become the place where Core meaning is invented.

## Enforceable closure condition

AA-1 should not close merely by adding these findings to a register. Before a positive verdict:

1. each unsafe item above must be assigned to AD-4, AD-5, AD-6, AD-7 or another named normative AA-2 artifact with an accountable owner and Gate;
2. AA-2 exit criteria must require the abstract data model, identity/equality relations, authority algebra, composed state machines, Profile/Extension composition, deterministic closure/negotiation, error algebra and typed Binding/Bridge/dependency seams;
3. each relation must have stable assertion IDs plus positive, negative and divergent-construction traces; and
4. reviewers must repeat this two-unit exercise and show that North and South either produce the same allowed outcome set and safety projection or are rejected by a specific normative assertion.

Until then, the package states the right safety intentions but does not yet constrain the next layer enough to guarantee a single protocol meaning.

## Final re-review — 2026-09-16

### Scope and method

The same two-unit construction was rerun against the updated `ARCHITECTURE-SPINE.md`, `AA-2-NORMATIVE-MODEL-CONTRACT.md`, `GLOSSARY-AND-CONTEXT.md` and the current AA-1 companions. Unit North remains an immutable graph/event interpretation; Unit South remains a mutable aggregate/actor interpretation. The test asks whether both units can still obey every adopted AA-1 rule yet reach different security-relevant results without one of them being rejected by a named AA-2 output, stable assertion, falsifier or Gate acceptance condition.

This was a documentary review only. No executable model, prototype or reference implementation was created. Reviewer independence remains internal AI/fresh-context (`I1`), not external recognition.

Primary reviewed working-copy digests:

| Artifact | SHA-256 |
| --- | --- |
| `ARCHITECTURE-SPINE.md` | `c2be77925461e71f1f6e4a3ef568e949e7b288814a36c0347607175f76950347` |
| `AA-2-NORMATIVE-MODEL-CONTRACT.md` | `19d1810fc76b4cb5823e0e97fa46763501f01cd3ae2d9069375e9f44fe49891a` |
| `GLOSSARY-AND-CONTEXT.md` | `4b83773f1088a3459b8c4b7462d85bc0af71388f67aa1dd332d204642601d642` |

Companion digests inspected for allocation and Gate consistency: `OWNER-ARCHITECTURE-GUIDE.md` `b8a001d4…41af0f2e`; `BASELINE-V0X-SCOPE.md` `9995b383…f93bde`; `QUALITY-ATTRIBUTE-SCENARIOS.md` `50cb194f…0c923`; `ASSETS-ADVERSARIES-BOUNDARIES.md` `435b477c…2d639d`; `ALLOCATION-MATRIX.md` `4b9058a1…38bff8`; `ADR-DISCIPLINE.md` `9fb33d80…b77dc5`; `LANDSCAPE-DISPOSITION.md` `45129cdc…526a4`; `AA-7-DIVERSITY-ELIGIBILITY.md` `833aee92…65066`; `AA-1-GATE-RECORD.md` `438e07f2…5b583b`.

### DVG closure replay

| Finding | Required AA-2 rejection point now present | Re-review result |
| --- | --- | --- |
| DVG-01 | `AIM`; `AA2-AIM-001` / `AA2-F-AIM-001`; exact abstract-value/equality acceptance | Adequately allocated; North/South data-shape disagreement cannot pass AA-2. |
| DVG-02 | `RCM`; `AA2-RCM-001` / `AA2-F-RCM-001`; role/context/epoch transition acceptance | Adequately allocated; role lifetime and ownership disagreement is forced into one normative relation. |
| DVG-03 | `DSM`; `AA2-DSM-001` / `AA2-F-DSM-001`; total Decision Subject equality/refinement acceptance | Adequately allocated; unclassified mutation must create a new subject. |
| DVG-04 | `LOM`; `AA2-LOM-001` / `AA2-F-LOM-001`; glossary separates Logical Operation, Delivery Attempt and Execution Attempt IDs | Adequately allocated; the original retry/attempt contradiction is removed. |
| DVG-05 | `AAM`; `AA2-AAM-001` / `AA2-F-AAM-001`; order-independent composed effective authority | Adequately allocated; union/intersection/deny choices cannot remain implementation-local. |
| DVG-06 | `AIM`/`AAM`/`LOM`; `AA2-AAM-002` / `AA2-F-AAM-002`; five-result freshness order | Adequately allocated; clock/provider mechanisms remain correctly deferred while semantic ordering is mandatory. |
| DVG-07 | `LOM`; `AA2-LOM-002` / `AA2-F-LOM-002`; composed lifecycle/effect/knowledge state acceptance | Adequately allocated; flat and orthogonal internal representations are allowed only if their allowed projections agree. |
| DVG-08 | `DSM`/`AAM`/`LOM`; `AA2-LOM-003` / `AA2-F-LOM-003`; abstract linearization or trace-preserving refinement proof | Adequately allocated; “atomic equivalent” is no longer a self-asserted implementation claim. |
| DVG-09 | `LOM`; `AA2-LOM-004` / `AA2-F-LOM-004`; error category/safety/retry/terminal/privacy projection acceptance | Adequately allocated; wire error representation remains correctly deferred. |
| DVG-10 | `CEM`; `AA2-CEM-001` / `AA2-F-CEM-001`; order-independent Profile applicability/composition | Adequately allocated; single-profile versus intersection behavior cannot both pass. |
| DVG-11 | `CEM`; `AA2-CEM-002` / `AA2-F-CEM-002`; Extension order/commutativity/dependency acceptance | Adequately allocated; ordered handlers cannot silently differ from declarative composition. |
| DVG-12 | `CEM`; `AA2-CEM-003` / `AA2-F-CEM-003`; deterministic root-to-member closure acceptance | Adequately allocated; included kinds, edges, cycles, missing nodes and invalidation must become computable. |
| DVG-13 | `CEM`; `AA2-CEM-004` / `AA2-F-CEM-004`; deterministic negotiation/tie/downgrade/no-common acceptance | Adequately allocated; identical transcripts must resolve identically before AA-4 encoding choices. |
| DVG-14 | `APM`; `AA2-APM-001` / `AA2-F-APM-001`; typed Binding claims with an explicit no-authority/no-effect rule | Adequately allocated; Binding mechanism remains deferred without permitting semantic invention. |
| DVG-15 | `APM`; `AA2-APM-002` / `AA2-F-APM-002`; Bridge Presenter/foreign subject/provenance/loss/ceiling acceptance | Adequately allocated; identity substitution or critical provenance loss is rejected. |
| DVG-16 | `APM`; `AA2-APM-003` / `AA2-F-APM-003`; dependency freshness/cache/outage/partition/compromise/substitution state acceptance | Adequately allocated; provider-specific continuation behavior cannot silently change Core outcomes. |
| DVG-17 | `ECM`; `AA2-ECM-001` / `AA2-F-ECM-001`; evidence graph/conflict/redaction/knowledge acceptance | Adequately allocated; trust rules cannot erase applicable contradiction or provenance. |
| DVG-18 | `RCM`/`AAM`/`LOM`; `AA2-RCM-002` / `AA2-F-RCM-002`; membership/quorum/epoch interleaving acceptance | Adequately allocated; membership snapshot and approval validity must resolve identically. |

AD-7 and `AFC` apply to every row, require stable non-reused IDs, quantified inputs, preconditions, allowed and forbidden results, traceability, a separate falsifier, and 100% transition/relation coverage. The AA-2 Gate additionally requires exact versions/digests/closure for all nine outputs and repeats the two-independent-evaluator test. Therefore none of the rows can be declared closed merely by producing prose or matching reference behavior.

### Verdict

**Pass for the adversarial divergence lens.** The previous `redesign/block` verdict is superseded for this lens by the updated package.

- DVG findings adequately allocated with enforceable AA-2 closure: **18/18**.
- Original unsafe AA-1 underconstraints still open: **0**.
- New unsafe AA-1 underconstraints found by the two-unit replay: **0**.
- Divergence blockers for AA-1 closure: **0**.
- Non-blocking divergence conditions: **0**.

This verdict confirms the constitution now constrains what AA-2 must produce; it does **not** claim that the nine AA-2 artifacts or their assertions have already been written or passed, does not authorize executable work, and does not replace the other required AA-1 review lenses or final Gate record.
