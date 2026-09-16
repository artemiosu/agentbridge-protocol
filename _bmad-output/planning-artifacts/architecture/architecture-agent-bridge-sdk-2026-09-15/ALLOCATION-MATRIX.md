---
title: AgentBridge AA-1 Requirement Allocation Matrix
status: final
created: 2026-09-16
updated: 2026-09-16
scope: current FR-1–FR-110, retained NFR-1–NFR-29, EI-01–EI-25, SBC-01–SBC-10
sources:
  - ../../prds/prd-agentbridge-native-architecture-first-2026-09-15/prd.md
  - ../../prds/prd-agentbridge-native-architecture-first-2026-09-15/retained-requirements-baseline.md
  - ../../../specs/spec-agentbridge-architecture-assurance/architecture-assurance-charter.md
  - ../../../specs/spec-agentbridge-validation-charter/evaluation-ledger.md
  - ../../../specs/spec-agentbridge-validation-charter/safety-blocking-and-risk.md
---

# AA-1 Requirement Allocation Matrix

## 1. Правило чтения

Матрица распределяет ответственность, но не сокращает нормативный текст. Полные формулировки FR/NFR берутся только из frozen Retained Requirements Baseline, EI — из frozen Evaluation Invariant Ledger, SBC — из frozen Safety Blocking policy. Диапазон используется лишь там, где все входящие ID имеют одинаковые primary owner, supporting layer, artifact family, verification class и closure gate; каждый ID диапазона считается отдельной аудируемой обязанностью.

`Closure gate` фиксирует staged closure, а не взаимозаменяемые альтернативы. В ячейке с несколькими Gate каждый явно помеченный этап обязателен: `semantic` / `normative-model` закрывает абстрактное значение в AA-2; `threat`, `trust` или `mechanism` закрывает соответствующую архитектурную обязанность в AA-3/AA-4; `conformance` закрывает проверяемый assertion/vector/observer contract в AA-5; `integrated` закрывает согласованную baseline в AA-6. Первый перечисленный этап — самое раннее обязательное закрытие своей части; поздний этап не заменяет ранний. Поздние изменения всё равно повторно открывают все затронутые этапы. Ни одна строка не разрешает код, production или публикацию.

### Сокращения артефактов

| Код | Нормативный артефакт |
| --- | --- |
| `CON` | AA-1 Constitution / Architecture Spine и ADR |
| `NM` | AA-2 Abstract Normative Model и state/trace models |
| `STA` | AA-3 Security, Privacy and Trust Architecture |
| `BC` | AA-4 Binding Contract, Base Binding и resource/performance envelope |
| `BR` | AA-4 Bridge Contract и mapping manifest |
| `CA` | AA-5 assertion catalog, models, vectors, independently derived observers и claim rules; применимые SBC-пути подчиняются frozen observer rule ниже |
| `IB` | AA-6 Integrated Architecture Baseline и exact implementation package |
| `II` | AA-7 reference/spec-only implementations и interop evidence |
| `PUB` | AA-8 rights, governance, publication и marks package |
| `PROD` | AA-9 scoped production-safety package |

## 2. Retained functional requirements FR-1–FR-95

| IDs | Primary responsibility | Supporting layer | Normative artifact | Verification | Closure gate |
| --- | --- | --- | --- | --- | --- |
| FR-1 | Core role-neutral Participant model | Profiles/Bindings preserve role neutrality | `NM`, `CA` | Role/owner/topology swap model and vectors | AA-2 / AA-5 |
| FR-2 | Core Participant/Endpoint/owner/transport-position separation | Bindings carry endpoint and transport context | `NM`, `BC`, `CA` | Multi-endpoint, shared-endpoint and reverse-position traces | AA-2 / AA-4 / AA-5 |
| FR-3 | Core Participant/Principal separation | Profiles constrain represented/affected Principals | `NM`, `STA`, `CA` | Participant=Principal and delegated Participant≠Principal vectors | AA-2 / AA-3 / AA-5 |
| FR-4 | Core contextual identifier semantics | Profiles/Bindings enforce minimization, namespace and epoch bounds | `NM`, `STA`, `CA` | Namespace collision, unlinkability, rotation/recovery and privacy review | AA-2 / AA-3 / AA-5 |
| FR-5 | Core dynamic, multidimensional Role model | Profiles define domain roles without changing Core roles | `NM`, `CA` | Role combination/change/branch and no-implicit-authority vectors | AA-2 / AA-5 |
| FR-6 | Core Audience/Resource/scope separation | Profiles narrow; Bindings carry protected scope | `NM`, `STA`, `CA` | Audience/resource/context substitution and multi-recipient vectors | AA-2 / AA-3 / AA-5 |
| FR-7 | Core authorship/subject/intermediary provenance distinctions | Bindings/Bridges preserve transformations; external issuers remain bounded | `NM`, `STA`, `CA` | Relay/author/endorsement confusion and provider substitution vectors | AA-2 / AA-3 / AA-5 |
| FR-8 | Core Correlation/Causality separation | Bindings convey declared links and ordering scope | `NM`, `BC`, `CA` | Reverse, parallel, duplicate and reordered trace models | AA-2 / AA-4 / AA-5 |
| FR-9 | Core fail-closed context completeness | Profiles define applicability; security boundary blocks ambiguity | `NM`, `STA`, `CA` | Missing/conflicting/unknown context negative corpus | AA-2 / AA-3 / AA-5 |
| FR-10 | Core capability semantics | Profiles/Extensions declare bounded capabilities | `NM` | Declaration parsing and semantic-equivalence vectors | AA-2 / AA-5 |
| FR-11 | Core discovery contract | Replaceable external discovery systems; Binding supplies reachability | `CON`, `NM`, `BC` | Provider-removal/replacement test; preconfigured Native path | AA-4 |
| FR-12 | Capability disclosure and minimization semantics | Profiles set disclosure policy | `NM`, `STA`, `CA` | Selective-disclosure, over-disclosure and resource-bound vectors | AA-3 / AA-5 |
| FR-13 | Capability trust and freshness semantics | External trust/policy evaluates attributed declarations | `NM`, `STA`, `CA` | Stale, untrusted, hostile and non-permit declaration vectors | AA-3 / AA-5 |
| FR-14 | Deterministic capability/version negotiation | Profiles and Bindings declare exact constraints | `NM`, `CA` | Equivalent-offer, mismatch and deterministic-selection vectors | AA-2 / AA-5 |
| FR-15 | Mandatory/optional capability and Extension negotiation | Extensions declare criticality and dependencies | `NM`, `CA` | Unknown-mandatory, optional, dependency and downgrade vectors | AA-2 / AA-5 |
| FR-16 | Negotiated-set completeness and agreement | Profiles/Extensions preserve exact closure | `NM`, `CA` | Split-view, omitted-element and transcript-equivalence vectors | AA-2 / AA-5 |
| FR-17 | Binding and security-mechanism negotiation | Bindings expose compatible protected properties | `NM`, `BC`, `CA` | Binding mismatch, unsupported security property and safe-refusal vectors | AA-2 / AA-4 / AA-5 |
| FR-18 | Freshness, expiry and renegotiation | Evolution/security layers supply epoch and invalidation inputs | `NM`, `STA`, `CA` | Stale/revoked declaration, reconnect and revalidation matrix | AA-2 / AA-3 / AA-5 |
| FR-19 | Negotiation evidence, privacy and failure outcome | Bindings carry protected transcript; diagnostics remain bounded | `NM`, `STA`, `BC`, `CA` | Transcript binding, privacy projection, mismatch and downgrade corpus | AA-2 / AA-3 / AA-4 / AA-5 |
| FR-20 | Request/response semantic mode | Binding transports messages without changing interaction meaning | `NM`, `BC`, `CA` | Request/response correlation, privacy and failure traces | AA-2 / AA-4 / AA-5 |
| FR-21 | Event/subscription semantic mode | Binding carries events and reconnect state | `NM`, `BC`, `CA` | Subscription, unsolicited event, reconnect and causal-gap traces | AA-2 / AA-4 / AA-5 |
| FR-22 | Interaction-mode conformance contract | Conformance observes exact mode semantics | `NM`, `CA` | Independent mode classification and diagnostic-failure vectors | AA-2 / AA-5 |
| FR-23 | Core bounded-stream semantics | Binding supplies flow control/backpressure | `NM`, `BC`, `CA` | Slow-consumer, overflow and cancellation tests against resource ceilings | AA-4 / AA-5 |
| FR-24 | Core long-running and reverse-asynchronous semantics | Binding supplies callback/poll/event reachability without redefining roles | `NM`, `BC`, `CA` | Disconnect/reconnect, delayed result and role/transport reversal traces | AA-2 / AA-4 / AA-5 |
| FR-25 | Core multi-party branch/aggregation semantics | Profiles constrain membership, quorum and aggregate outcomes | `NM`, `STA`, `CA` | Branch/join, partial branch, epoch and aggregate-result models | AA-2 / AA-3 / AA-5 |
| FR-26 | Core negotiation-without-commitment semantics | Profiles define when a proposal may become a decision | `NM`, `STA`, `CA` | Proposal/counter-proposal/acceptance/authority confusion vectors | AA-2 / AA-3 / AA-5 |
| FR-27 | Core delivery and ordering semantics | Binding declares delivery/order guarantees and limits | `NM`, `BC`, `CA` | Duplicate, reorder, late and incompatible-guarantee vectors | AA-2 / AA-4 / AA-5 |
| FR-28 | Core coordinated termination semantics | Profiles refine obligations; Binding carries termination signals | `NM`, `BC`, `CA` | Cancel/close/late-event/partial-branch termination traces | AA-2 / AA-4 / AA-5 |
| FR-29 | Core safe failure outcome | Binding/external dependency adapters implement bounded recovery | `NM`, `BC`, `CA` | Crash, partition, dependency-failure and safe-stop fault injection | AA-4 / AA-5 |
| FR-30 | Explicit authority subject and scope | Profiles define policy constraints | `NM`, `STA`, `CA` | Unauthorized-subject/scope and confused-deputy vectors | AA-2 / AA-3 / AA-5 |
| FR-31 | Decision Subject integrity at authorization/effect | Effect boundary supplies current subject/preconditions | `NM`, `STA`, `CA` | Subject mutation, TOCTOU and stale-approval vectors | AA-2 / AA-3 / AA-5 |
| FR-32 | Delegation lineage and monotonic attenuation | Profiles/external issuers constrain grants | `NM`, `STA`, `CA` | Formal attenuation and lineage-substitution vectors | AA-2 / AA-3 / AA-5 |
| FR-33 | Delegation limits and bounded resource use | Shared-limit authority supplies reservations where required | `NM`, `STA`, `CA` | Amount/use/rate/depth and concurrent-spend vectors | AA-2 / AA-3 / AA-5 |
| FR-34 | Consent and Approval distinctions | Profiles define required approvers and independence | `NM`, `STA`, `CA` | Consent/approval/proposal confusion and quorum vectors | AA-2 / AA-3 / AA-5 |
| FR-35 | Authorization decision semantics | External policy supplies scoped attributed input only | `NM`, `STA`, `CA` | Permit/non-permit/cannot-establish and unauthorized-effect vectors | AA-2 / AA-3 / AA-5 |
| FR-36 | Core decision composition and actor separation | Profiles define quorum and independence policy | `NM`, `STA` | Aliasing, collusion-boundary, incomplete-quorum and branch-composition models | AA-3 |
| FR-37 | Core authority freshness and boundary recheck | Profiles/external policy define freshness inputs | `NM`, `STA`, `CA` | TOCTOU, stale-policy and effect-boundary recheck model | AA-2 / AA-3 / AA-5 |
| FR-38 | Core replay and bounded-use authority semantics | Bindings preserve operation/use identity; external shared-limit authority where required | `NM`, `STA`, `BC`, `CA` | Replay, concurrent use-count/amount and retention-expiry vectors | AA-2 / AA-3 / AA-4 / AA-5 |
| FR-39 | Core revoke and in-flight semantics | Profiles define policy; external authority supplies status/epoch | `NM`, `STA`, `CA` | Revoke/commit/partition/late-completion traces | AA-2 / AA-3 / AA-5 |
| FR-40 | Core compromise-containment semantics | Profiles/Bindings constrain blast radius and recovery epochs | `NM`, `STA`, `BC`, `CA` | Credential/endpoint/provider compromise and recovery review | AA-2 / AA-3 / AA-4 / AA-5 |
| FR-41 | Privacy-minimizing authority proof contract | Profiles/Bindings constrain disclosure and linkability | `NM`, `STA`, `CA` | Selective proof, over-disclosure and cross-context correlation vectors | AA-2 / AA-3 / AA-5 |
| FR-42 | End-to-end authority/assurance preservation | Bindings and Bridges preserve or explicitly reject/lower bounded claims | `STA`, `BC`, `BR`, `CA` | Coverage, caveat-loss, termination and bridge-loss adversarial corpus | AA-3 / AA-4 / AA-5 |
| FR-43 | Core Operation Identity | Bindings preserve identity; endpoint state makes it durable in claim scope | `NM`, `BC`, `CA` | Collision, reuse, retry and changed-subject vectors | AA-2 / AA-4 / AA-5 |
| FR-44 | Core extensible lifecycle semantics | Profiles refine only within Core allowed states | `NM`, `CA` | Allowed/forbidden state-transition model | AA-2 / AA-5 |
| FR-45 | Core delivery/acceptance/authorization/execution/effect separation | External executor/effect source supplies bounded observations | `NM`, `STA`, `CA` | Stage-confusion and false-effect vectors | AA-2 / AA-3 / AA-5 |
| FR-46 | Core Effect State and uncertainty semantics | Profiles refine outcomes; external effect source remains domain authority | `NM`, `STA`, `CA` | Partial/unknown/conflict/timeout model and corpus | AA-2 / AA-3 / AA-5 |
| FR-47 | Core retry/deduplication/recovery semantics | Binding and durable endpoint state implement bounded recovery | `NM`, `BC`, `CA` | Lost-response, duplicate, restart and unsafe-retry faults | AA-2 / AA-4 / AA-5 |
| FR-48 | Core cancel/expiry/race semantics | Profiles constrain; Binding transports cancellation | `NM`, `STA`, `BC`, `CA` | Cancel/commit, expiry and late-completion interleavings | AA-2 / AA-3 / AA-4 / AA-5 |
| FR-49 | Core Compensation-as-new-Operation semantics | Profiles define compensating action; external effect source reports outcome | `NM`, `STA`, `CA` | Failed/partial compensation and history-preservation traces | AA-2 / AA-3 / AA-5 |
| FR-50 | Core composite/multi-party lifecycle semantics | Profiles define branch/quorum composition | `NM`, `STA`, `CA` | Partial branch, aggregate, dependency and recovery models | AA-2 / AA-3 / AA-5 |
| FR-51 | Core freshness/provenance/conflicting-observation semantics | Profiles set trust; external effect/evidence sources remain attributed | `NM`, `STA`, `CA` | Stale, equivocal and conflicting-observation corpus | AA-2 / AA-3 / AA-5 |
| FR-52 | Core structured effect-aware error semantics | Bindings map transport errors without inventing effect certainty | `NM`, `BC`, `CA` | Timeout/transport/application/effect-state error mapping vectors | AA-2 / AA-4 / AA-5 |
| FR-53 | Core Outcome Claim/Evidence subject binding | External issuers/storage provide bounded claims | `NM`, `STA`, `CA` | Subject/operation/decision/branch substitution vectors | AA-2 / AA-3 / AA-5 |
| FR-54 | Core Assurance and external-truth boundary | Profiles define verifier/trust rules; external systems remain claim sources | `NM`, `STA`, `CA` | Signature/log/receipt truth-inflation and issuer-confusion tests | AA-2 / AA-3 / AA-5 |
| FR-55 | Core privacy-bounded causal history | Profiles/storage/observers enforce disclosure and lifecycle | `NM`, `STA`, `CA` | Missing/redacted/conflicting history and causal reconstruction tests | AA-2 / AA-3 / AA-5 |
| FR-56 | Normative artifact identity/version/digest | All artifact kinds publish stable identity and status | `CON`, `NM`, `CA` | Identity reuse, digest and unknown-version vectors | AA-2 / AA-5 |
| FR-57 | Semantic compatibility relation | Bindings/Profiles/Extensions declare compatibility | `CON`, `NM`, `CA` | Allowed-set equivalence and staged-version vectors | AA-2 / AA-5 |
| FR-58 | Mandatory/optional/security-critical classification | Every extensible artifact declares unknown handling | `CON`, `NM`, `CA` | Unknown critical/optional and reclassification vectors | AA-2 / AA-5 |
| FR-59 | Extension lifecycle and namespace contract | Neutral governance plus vendor-scoped namespaces | `CON`, `CA`, `PUB` | Collision, identifier reuse, status and ownership/provenance review | AA-5; governance closure AA-8 |
| FR-60 | Core security floor | Profiles/Extensions/Bindings may only narrow or strengthen | `CON`, `NM`, `CA` | Override/downgrade mutation corpus; dependency lint | AA-2 / AA-5 |
| FR-61 | Extension dependency and composition contract | Core supplies invariant waist | `CON`, `NM`, `CA` | Dependency cycle/order/conflict and resource-bound vectors | AA-2 / AA-5 |
| FR-62 | Domain Profile boundary | Profiles define independent domain meaning outside Core | `CON`, `NM`, `CA` | Opaque/domain leakage and universality/removal review | AA-2 / AA-5 |
| FR-63 | Cross-Profile interaction contract | Bridges handle foreign mappings; Profiles declare overlap | `CON`, `NM`, `CA` | Cross-profile scope, transition and equivalence vectors | AA-2 / AA-5 |
| FR-64 | Deterministic negotiated artifact closure | All artifacts declare exact dependencies | `NM`, `CA` | Closure resolution, missing/cycle and pinning vectors | AA-2 / AA-5 |
| FR-65 | Deprecation, withdrawal, migration and rollback | Security/evolution layers invalidate affected claims | `NM`, `STA`, `CA` | Staged upgrade, in-flight pinning, rollback and withdrawal matrix | AA-2 / AA-3 / AA-5 |
| FR-66 | Normative change and errata discipline | Governance and conformance preserve immutable history | `CON`, `CA`, `PUB` | Digest, revision, errata and migration audit | AA-5; publication closure AA-8 |
| FR-67 | Promotion/removal discipline | Governance exercises Extensions before promotion | `CON`, `CA`, `PUB` | GREASE-like exercise, universality/removal and provenance review | AA-5; publication closure AA-8 |
| FR-68 | Native path eligibility | At least one complete open Base Binding; no Bridge/provider/SDK dependency | `CON`, `BC`, `II` | Dependency-cut test and direct interop without foreign runtime/platform | AA-4 design; empirical AA-7 |
| FR-69 | Binding Contract | Core supplies abstract protected meaning | `BC`, `STA`, `CA` | Typed port and coverage contract review | AA-4 / AA-5 |
| FR-70 | Binding semantic invariance | Security Profile supplies reviewed primitives | `BC`, `STA`, `CA` | Cross-Binding allowed-set/safety equivalence | AA-4 / AA-5 |
| FR-71 | Binding coverage completeness | Conformance enumerates protected fields and transitions | `BC`, `STA`, `CA` | Canonical/signable coverage and mutation proof | AA-4 / AA-5 |
| FR-72 | Binding trust/privacy boundary | Adapter preserves attributed context and disclosure limits | `BC`, `STA`, `CA` | Termination, metadata, linkability and boundary vectors | AA-4 / AA-5 |
| FR-73 | Replaceable-provider isolation | External providers remain behind explicit contracts | `BC`, `STA`, `CA` | Provider substitution, compromise and blast-radius tests | AA-4 / AA-5 |
| FR-74 | Safe fallback and dependency failure | Core defines safe dependent outcome | `BC`, `STA`, `CA` | Outage, fallback, downgrade and resource tests | AA-4 / AA-5 |
| FR-75 | Bridge eligibility and optionality | Core never depends on Bridge | `BR`, `STA`, `CA` | Dependency-cut and eligibility review | AA-4 / AA-5 |
| FR-76 | Bridge direction/version pinning | External protocol versions are exact | `BR`, `STA`, `CA` | Direction reversal and version-drift vectors | AA-4 / AA-5 |
| FR-77 | Bridge semantic-loss mapping | Unrepresentable critical meaning fails closed | `BR`, `STA`, `CA` | Total/partial/unmapped loss matrix | AA-4 / AA-5 |
| FR-78 | Bridge assurance ceiling | Translation cannot inflate authority/assurance | `BR`, `STA`, `CA` | Caveat loss and assurance-inflation vectors | AA-4 / AA-5 |
| FR-79 | Bridge provenance and loop safety | Transformations preserve attributed lineage | `BR`, `STA`, `CA` | Provenance, impersonation, replay and loop vectors | AA-4 / AA-5 |
| FR-80 | Bridge resource/failure isolation | Edge adapter has bounded failure behavior | `BR`, `STA`, `CA` | Size/depth/retry/amplification/outage vectors | AA-4 / AA-5 |
| FR-81 | Bridge expiry and Native isolation | Mapping changes invalidate only dependent claims | `BR`, `STA`, `CA` | Expiry, upstream drift and Native dependency-cut tests | AA-4 / AA-5 |
| FR-82 | Scoped conformance-claim model | Specifications remain semantic authority | `CA`, `PUB` | Exact artifact/scope/expiry claim audit | AA-5; rights closure AA-8 |
| FR-83 | Open self-runnable Suite | Rights/governance provide equal access | `CA`, `PUB` | Independent fresh-environment self-run | AA-5; rights closure AA-8 |
| FR-84 | Complete normative-obligation coverage | Models/assertions/reviews cover every obligation | `CA` | 100% obligation-to-assertion/model/review map | AA-5 |
| FR-85 | Independent implementation lineage | `IB` controls access and genealogy | `CA`, `IB`, `II` | Shared-code/private-explanation/genealogy audit | AA-6 design; empirical AA-7 |
| FR-86 | Differential interoperability | Implementations remain code-independent | `CA`, `IB`, `II` | Mutual differential run with zero pair-specific fixes | AA-6 design; empirical AA-7 |
| FR-87 | Mandatory topology matrix | Scenario corpus covers retained topologies | `CA`, `IB`, `II` | Cross-role/topology implementation matrix | AA-6 design; empirical AA-7 |
| FR-88 | Negative/adversarial coverage | Security corpus owns prohibited paths | `CA` | Mutation and adversarial Suite | AA-5 |
| FR-89 | Property/fuzz/resource coverage | Harness explores structural boundaries | `CA` | Property, fuzz and resource-bound Suite | AA-5 |
| FR-90 | Concurrency/fault coverage | Lifecycle corpus owns interleavings | `CA` | Concurrency, crash, partition and fault Suite | AA-5 |
| FR-91 | Version-combination coverage | Binding/Bridge corpora cover staged evolution | `CA` | Compatible/incompatible staged-version matrix | AA-5 |
| FR-92 | Reproducible evidence | Evidence custody preserves inputs and raw results | `CA`, `IB` | Fresh-environment rerun and provenance audit | AA-5 / AA-6 |
| FR-93 | Specification primacy | Reference behavior remains non-normative | `CA`, `IB` | Oracle/spec divergence and adjudication review | AA-5 / AA-6 |
| FR-94 | Suite evolution | Governance versions assertions/vectors safely | `CA`, `IB`, `PUB` | Suite migration, regression and history audit | AA-5 / AA-6; publication closure AA-8 |
| FR-95 | Scoped validity, expiry and revalidation | Claims bind exact closure/environment | `CA`, `IB`, `PUB` | Observer qualification, expiry and invalidation audit | AA-5 / AA-6; publication closure AA-8 |

Audit coverage: the disjoint rows above cover every integer ID from FR-1 through FR-95 exactly once.

### 2.1 Retained F1–F8 empirical acceptance obligations

The heading-bounded retained source includes the Feature validation gates and their acceptance detail, not only the numbered FR paragraphs. Earlier model, security, Binding and conformance-design evidence is necessary but does not replace the following empirical obligations.

The table below is an index, not a paraphrased replacement. Each row incorporates the complete `F1 validation gate` through `F8 validation gate` paragraphs inside the frozen FR-1–FR-95 selector recorded by `retained-requirements-baseline.md`, extract SHA-256 `0bdf52d56f9edee19159434c65ad3b4e0df2033f2ed22ded7d0f3ec7f514be7a`. Controlling conditions retained in full include: independently authored implementations with no shared AgentBridge protocol library; for F8, no shared semantic generator and materially different language/runtime environments; the pre-implementation royalty-free experimental grant; executable/model-based/independent-review coverage for every applicable FR-1–FR-81 obligation; the complete negative/fault corpora; and the rule that Bridge runs are separate and never count as either Independent Implementation or proof of Native independence. Where the index is shorter than the frozen paragraph, the frozen paragraph governs.

| Feature | Required empirical evidence | Accountable owner | Closure gate |
| --- | --- | --- | --- |
| F1 / FR-1–FR-9 | Two independently created implementations without shared protocol-semantic code reconstruct Participants, Principals, scoped Roles, Audience, Resources, Correlation and Causality across every retained topology, including negative and privacy-preserving cases. | Independent Implementation Lead | AA-7 |
| F2 / FR-10–FR-19 | The independent pair reaches the same negotiation outcome for direct discovery, preconfiguration and replaceable discovery, including mismatch, unknown mandatory meaning, selective disclosure, stale/revoked declarations, downgrade and multi-party disagreement. | Negotiation Model Owner + Independent Implementation Lead | AA-7 |
| F3 / FR-20–FR-29 | The independent pair exercises request/response, event/subscription, stream, long-running reverse flow, multi-party branching/aggregation and negotiation in at least three mandatory topologies, including the retained negative/fault corpus. | Interaction Model Owner + Independent Implementation Lead | AA-7 |
| F4 / FR-30–FR-42 | The independent pair agrees on `permit / non-permit / cannot-establish`, produces no protected effect for the latter two, and covers the retained authority topologies, provider substitution and compatibility-bridge case. | Authority/Security Owner + Independent Implementation Lead | AA-7 |
| F5 / FR-43–FR-55 | The independent pair agrees on lifecycle/effect classification and retry, cancellation, Compensation and Evidence limits across the retained direct, delegated, B2B, reverse, multi-party and replaceable audit/trust cases. | Lifecycle/Evidence Owner + Independent Implementation Lead | AA-7 |
| F6 / FR-56–FR-67 | The independent pair checks old/new Core, two AD-22-eligible independently defined Extensions, at least two AD-22-eligible unrelated Domain Profiles and two eligible Native Bindings, including the retained evolution/downgrade/ossification corpus. | Evolution Owner + Independent Implementation Lead | AD-22 at AA-6 exit; evidence AA-7 |
| F7 / FR-68–FR-81 | The independent pair interoperates through two AD-22-eligible distinct Native Bindings; at least two variants each of critical identity/trust and policy/audit mechanisms are substituted; eligible Bridges to at least two distinct foreign semantic-model families are tested separately. | Binding/Bridge Owner + Independent Implementation Lead | AD-22 at AA-6 exit; evidence AA-7 |
| F8 / FR-82–FR-95 | Two independent implementations in materially different language/runtime environments run the same frozen self-runnable Suite and mutual differential interop across all retained topology/mode coverage, two eligible Domain Profiles, two eligible Extensions and two eligible Native Bindings; prohibited observable effects remain zero. | Conformance Owner + Independent Implementation Lead | AD-22 at AA-6 exit; evidence AA-7 |

> **Resolved by adopted AD-3A:** one open provider-neutral Base Binding remains the mandatory v0.x path, while AA-7 also tests a second distinct experimental Native Binding, two unrelated non-production Domain Profiles, two independently defined Extensions and optional Bridges to at least two foreign protocol models. These validation artifacts do not become Core, mandatory runtime dependencies, production commitments or adoption claims.

### 2.2 Pre-AA-7 diversity eligibility

Proposed **AD-22**, recorded in `AA-7-DIVERSITY-ELIGIBILITY.md`, is a blocking selection contract at AA-6 exit. It defines deterministic `eligible`/`ineligible` tests for the material distinction of Bindings, domain independence of Profiles, independent definition of Extensions and semantic-family distinction of foreign Bridge targets. No F6–F8 evidence counts if the candidate set was selected before AD-22 was frozen, has an unresolved conflict disclosure, contains undisclosed shared semantic code, or relies on a cosmetic distinction.

## 3. Current Architecture Assurance requirements FR-96–FR-110

These IDs use the current Native Architecture-First PRD semantics. They do not import the superseded comparative Gate 1 meanings formerly attached to the same numbers.

| ID | Primary responsibility | Supporting layer | Normative artifact | Verification | Accountable owner | Closure gate |
| --- | --- | --- | --- | --- | --- | --- |
| FR-96 | Versioned Architecture Assurance Charter and stage controls | Every Gate supplies inputs, outputs, blockers, evidence and permission | `CON`, `IB` | Charter completeness/version/digest audit and Gate-record review | Architecture Assurance Lead | AA-1; recurrent at every Gate |
| FR-97 | Continuous landscape learning without runtime dependence | Binding/Bridge research and dependency review | `CON`, `BC`, `BR`, `IB`; `LANDSCAPE-DISPOSITION.md` | Pinned landscape review, reuse/reject/defer/Bridge disposition log and dependency-cut check | Architecture Research Lead | AA-1 control; refresh before each affected AD/Gate |
| FR-98 | Complete but disciplined Native Core allocation | Profiles/Extensions/Bindings externalize non-universal duties | `CON`, `NM`, `CA`, `IB` | Universality and Removal Test for every Core element | Chief Architect | AA-1 rule; integrated closure AA-6 |
| FR-99 | End-to-end architecture traceability | All normative and assurance artifacts | `CON`, `NM`, `STA`, `BC`, `BR`, `CA`, `IB` | Mechanical 100% ID/link coverage plus semantic review after every material change | Traceability/Configuration Owner | AA-1 initial; recurrent at every Gate |
| FR-100 | Cross-domain and cross-topology validation | Profiles, scenario corpus and independent implementations | `NM`, `STA`, `CA`, `II`; AD-22 | Mandatory topologies plus AD-22-eligible unrelated commercial, non-commercial and inter-organizational/agent-to-agent slices | Conformance Owner | Eligibility AA-6 exit; design AA-5/AA-6; empirical AA-7 |
| FR-101 | Non-waivable safety, rights and evidence gates | Independent reviewers and Gate authority | `CON`, `IB`, `PUB` | Zero unresolved SBC/Critical/High/mandatory/rights/independence/evidence/conformance blockers | Gate Owner + applicable Independent Review Chairs | Every passing Gate |
| FR-102 | Independent implementability before reference behavior | Spec-only track, oracle/Suite, genealogy and access controls | `CA`, `IB`, `II` | Frozen digest/package, equal access, isolation/genealogy audit and adjudication rules | Conformance Independence Lead | AA-6 exit / before AA-7 reference work |
| FR-103 | Formal/executable verification of critical semantics | Security and conformance reviews consume model evidence | `NM`, `STA`, `CA` | Model/property coverage for authority, delegation, lifecycle, effect, retry/replay, cancel/revoke, concurrency and failure | Formal Methods Lead | AA-2/AA-3/AA-5; before corresponding AA-7 code |
| FR-104 | Dependency and failure isolation | External systems, Bindings and Bridges declare bounded contracts | `CON`, `STA`, `BC`, `BR`, `CA`, `IB` | Outage/compromise/substitution/blast-radius and semantic-invariance tests | Distributed Systems Lead + Security Lead | AA-3/AA-4/AA-5; integrated AA-6 |
| FR-105 | Performance and bounded resources | Binding/runtime candidates and conformance harness | `BC`, `CA`, `II` | Preregistered budgets and benchmarks for all required resource dimensions; safety gates first | Performance/Resource Lead | Numeric design AA-4; validity AA-5; empirical AA-7 |
| FR-106 | Evolvability and safe migration | Versioned Core/Profile/Extension/Binding/Bridge artifacts | `CON`, `NM`, `BC`, `BR`, `CA`, `IB` | Staged upgrade, rollback, drift, downgrade, withdrawal and Bridge-expiry matrix | Protocol Evolution Lead | AA-2/AA-4/AA-5; integrated AA-6 |
| FR-107 | Reproducible evidence package | Evidence custody, implementation runs and publication controls | `CA`, `IB`, `II`, `PUB` | Manifests/digests, inputs, models, vectors, raw/derived results, negative/inconclusive evidence, dissent and scoped claims | Evidence Custodian | Schema AA-5; package AA-6/AA-7; publication AA-8 |
| FR-108 | Specialized independent reviews | All Gate artifacts and conflict/recusal controls | `CON`, `IB`, `PUB` | Signed scoped findings from every applicable review discipline; AI review never satisfies external independence | Review Coordinator + Gate Owner | Every Gate before verdict |
| FR-109 | Deterministic Gate outcomes | Gate records and blocker closure evidence | `CON`, `IB` | Exactly `pass`, `pass with conditions` or `redesign/block`; every blocker names rule, evidence, correction and closure criterion | Gate Owner | Every Gate |
| FR-110 | Scoped permission, redesign and emergency stop | Project Owner decision and narrow emergency authority | `CON`, `IB` | Gate record names only the next permitted scope; pause/stop path preserves required evidence and authority | Gate Owner; Project Owner for final stop | Every Gate / emergency path |

Audit coverage: the retained rows cover FR-1–FR-95 exactly once, and the current Architecture Assurance rows cover FR-96–FR-110 exactly once. Together they cover every current FR ID from FR-1 through FR-110.

## 4. Retained non-functional requirements NFR-1–NFR-29

| IDs | Primary responsibility | Supporting layer | Normative artifact | Verification | Closure gate |
| --- | --- | --- | --- | --- | --- |
| NFR-1 | Security architecture: complete scoped threat model | Every Profile/Binding/Bridge/claim supplies scoped assumptions | `STA`, `CA` | Asset/adversary/boundary/misuse coverage and independent security review | AA-3 / AA-5 |
| NFR-2 | Core fail-closed security-critical uncertainty | Profiles narrow; all lower layers preserve non-permit/unknown | `NM`, `STA`, `CA` | Formal invariant plus timeout/fallback/auth/receipt/cache/retry/bridge negative vectors | AA-2 / AA-3 / AA-5 |
| NFR-3 | End-to-end integrity of protected meaning | Security Binding and Bridges preserve full covered subject or fail closed | `NM`, `STA`, `BC`, `BR`, `CA` | Coverage/canonicalization/mutation/intermediary/bridge-loss proof and vectors | AA-2 / AA-3 / AA-4 / AA-5 |
| NFR-4 | Least privilege and minimum disclosure | Profiles, Bindings and observers enforce purpose/linkability bounds | `NM`, `STA`, `CA` | Authority minimization, disclosure/data-flow and metadata/timing/linkability review | AA-2 / AA-3 / AA-5 |
| NFR-5 | Bounded compromise blast radius and honest residual risks | Profiles/Bindings constrain scope; threat model states non-goals | `STA`, `BC`, `CA` | Endpoint/credential/participant/provider/bridge compromise analysis and fault tests | AA-3 / AA-4 / AA-5 |
| NFR-6 | Security Binding/profile discipline | Reviewed external primitives; no new cryptographic algorithms | `STA`, `BC`, `CA` | Primitive/version/assumption review and downgrade/coverage vectors | AA-3 / AA-4 / AA-5 |
| NFR-7 | Native decentralization invariant | Replaceable external infrastructure and Base Binding | `CON`, `BC`, `II` | Platform/provider dependency-cut and direct-path test | AA-4 design; empirical AA-7 |
| NFR-8 | Core failure/recovery semantics | Binding and durable endpoint state | `NM`, `CA` | Crash/restart/partition/duplicate/reorder/cancel-revoke fault corpus | AA-2 / AA-5 |
| NFR-9 | Bounded resource consumption | Binding runtime and Profiles set scoped ceilings | `BC`, `CA` | Complexity analysis, hostile input and aggregate resource ceilings | AA-4 / AA-5 |
| NFR-10 | Realistic time/order/partition semantics | Normative model and Binding expose uncertainty | `NM`, `BC`, `CA` | Clock/order/partition/concurrency models and fault vectors | AA-2 / AA-4 / AA-5 |
| NFR-11 | Core deterministic allowed outcomes | Conformance oracle/equivalence classes | `NM`, `CA` | Cross-model and differential safety-projection agreement | AA-2 / AA-5 |
| NFR-12 | Preregistered performance budget | Binding/runtime candidates | `BC`, `CA` | Cold/warm/saturated/fault p50/p95/p99 and throughput benchmarks | AA-4 / AA-5 |
| NFR-13 | Resource/correctness guardrail | Binding/runtime candidates | `BC`, `CA` | CPU/memory/state/I/O/resource-per-unit benchmarks with safety gates first | AA-4 / AA-5 |
| NFR-14 | Language/runtime/environment portability | Base Binding plus independent implementations | `BC`, `IB`, `II` | At least two materially different runtime environments; no hidden runtime assumption | AA-6 design; empirical AA-7 |
| NFR-15 | Scale without mandatory centralization | Federated endpoints and replaceable infrastructure | `CON`, `BC`, `II` | Coordinator-removal, fan-out/shared-limit cost and horizontal scale tests | AA-4 design; empirical AA-7 |
| NFR-16 | Language/vendor neutrality | Open specification/conformance package and diverse implementations | `CON`, `CA`, `IB`, `II`, `PUB` | Different-language/runtime implementation with no vendor-only dependency | AA-6 design; empirical AA-7; rights AA-8 |
| NFR-17 | Self-sufficient normative contract | Equal-access specification/conformance package | `CON`, `CA`, `IB`, `PUB` | Fresh-team comprehension; zero closed explanations/shared semantic code | AA-6 design; empirical AA-7; rights AA-8 |
| NFR-18 | Staged coexistence, migration and rollback | Versioned Core/Profile/Binding artifacts | `CON`, `NM`, `CA` | Coexistence, drift, rollback and no-flag-day matrix | AA-2 / AA-5 |
| NFR-19 | Bounded Extension evolution | Extension manifests and governance | `CON`, `NM`, `CA` | Unknown-element, collision, dependency and composition matrix | AA-2 / AA-5 |
| NFR-20 | Privacy-safe diagnostics | Profiles/Bindings constrain disclosed failures | `NM`, `STA`, `CA` | Failure localization, redaction, metadata and linkability vectors | AA-3 / AA-5 |
| NFR-21 | Causal provenance and reconstruction | Core evidence contract and observers | `NM`, `STA`, `CA` | Causal-chain, missing/conflicting evidence and recovery vectors | AA-3 / AA-5 |
| NFR-22 | Non-interfering reproducible measurement | Qualified observers/harness and evidence custody | `CA`, `IB` | Calibration, A/B instrumentation check, fresh rerun and immutable provenance; for every applicable SBC evaluation where Protected Disclosure or Consequential Effect is possible, at least two independently derived observer paths, with a missing path or unresolved disagreement classified invalid/inconclusive and never pass | AA-5 conformance; AA-6 integrated |
| NFR-23 | Contextual data minimization | Profiles, Bindings, fixtures, logs and evidence controls | `STA`, `CA`, `PUB` | Synthetic/authorized-data audit and cross-context/bridge propagation review | AA-3 / AA-5; publication AA-8 |
| NFR-24 | Representable data lifecycle without one global policy | Profiles/Bindings and evidence stores carry classification/retention/deletion/residency/redaction constraints | `NM`, `STA`, `BC`, `CA`, `PUB` | Constraint representation plus retention/deletion/residency/redaction and false-fact tests | AA-2 / AA-3 / AA-4 / AA-5; publication AA-8 |
| NFR-25 | Secrets and sensitive-artifact protection | Binding/build/runtime, repository, diagnostics and publication controls | `STA`, `BC`, `CA`, `PUB` | Secret scan, fixture/log/error-output review and out-of-artifact key-management check | AA-3 / AA-4 / AA-5; publication AA-8 |
| NFR-26 | Dependency inventory and executed-artifact provenance | Binding/runtime/Suite build systems | `BC`, `CA`, `IB` | Locked dependency closure, SBOM/license/content digest and executed-artifact audit | AA-4 / AA-6 |
| NFR-27 | Open royalty-free normative and implementation foundation | Governance/IP, AD-21 execution controls, implementation-access controls and publication package | `CON`, `IB`, `PUB` | Before every executable model/prototype: scoped obtain/store/use/modify/execute rights, notices, provenance, SBOM and exclusions in frozen-approved AD-21; before AA-7: equal-access RF experimental implementation grant; before public draft: permanent specification/conformance/contribution/patent terms | Architecture constraint AA-1; per-run pre-executable closure through AD-21; implementation-rights closure AA-6 exit / pre-AA-7; permanent publication closure AA-8 |
| NFR-28 | No hidden commercial dependency | Protocol/Platform Firewall and provider-independent Native path | `CON`, `BC`, `CA`, `II`, `PUB` | Paywall, permission, certification, cloud/provider and SDK dependency-cut tests | Architecture constraint AA-1; design AA-6; empirical AA-7; public controls AA-8 |
| NFR-29 | Security change and claim invalidation | Security/governance/conformance | `STA`, `CA`, `PUB` | Vulnerability intake, scope-impact, suspension/restoration and post-incident exercise | AA-5; publication closure AA-8 |

Audit coverage: the disjoint rows above cover every integer ID from NFR-1 through NFR-29 exactly once.

## 5. Frozen Evaluation Invariants EI-01–EI-25

| ID | Primary responsibility | Supporting layer | Normative artifact | Verification | Closure gate |
| --- | --- | --- | --- | --- | --- |
| EI-01 | Core participant/context model | Profiles/Bindings | `NM`, `CA` | Role/owner/transport-position swaps and ambiguous-context vectors | AA-2 / AA-5 |
| EI-02 | Core bootstrap/negotiation | Security Binding | `NM`, `STA`, `CA` | Circular negotiation, transcript-equivalence and downgrade vectors | AA-2 semantic; AA-3 threat/mechanism; AA-5 conformance |
| EI-03 | Core discovery/capability negotiation | External discovery; Profiles/Extensions | `NM`, `CA` | Stale declaration, unsupported mandatory and non-permit vectors | AA-2 / AA-5 |
| EI-04 | Core interaction/correlation/causality | Binding | `NM`, `CA` | Reconnect/reorder/reverse/branch trace models | AA-2 / AA-5 |
| EI-05 | Core Decision Subject identity | Profiles define domain equality/refinement | `NM`, `STA`, `CA` | Security-critical mutation and reuse vectors | AA-2 semantic; AA-3 threat/mechanism; AA-5 conformance |
| EI-06 | Core attenuating authority chain | Profiles/external authority issuers; Bindings/Bridges preserve | `NM`, `STA`, `CA` | Formal monotonicity plus caveat-loss/confused-deputy/epoch vectors | AA-2 semantic; AA-3 threat/trust/mechanism; AA-5 conformance |
| EI-07 | Core decision-stage distinctions | Profiles define quorum/obligations | `NM`, `STA`, `CA` | Proposal/consent/approval confusion and independence vectors | AA-2 semantic; AA-3 trust/mechanism; AA-5 conformance |
| EI-08 | Core disclosure/effect boundary check | Effect adapter/external executor | `NM`, `STA`, `CA` | TOCTOU/revoke/expiry/policy race model and vectors | AA-2 semantic; AA-3 threat/trust/mechanism; AA-5 conformance |
| EI-09 | Core shared-limit semantics | External shared-limit authority where needed | `NM`, `STA`, `CA` | Concurrent branch/partition overspend model | AA-2 semantic; AA-3 trust/mechanism; AA-5 conformance |
| EI-10 | Core lifecycle/effect projection | Profiles/external effect sources | `NM`, `CA` | Conflicting/partial/unknown/timeout state corpus | AA-2 / AA-5 |
| EI-11 | Core operation/retry semantics | Durable endpoint state | `NM`, `CA` | Lost-response, replay, changed-subject and retention-expiry faults | AA-2 / AA-5 |
| EI-12 | Core cancel/expiry/compensation semantics | Profiles refine compensation | `NM`, `CA` | Cancel/commit race, late completion and failed-compensation traces | AA-2 / AA-5 |
| EI-13 | Core evidence/provenance contract | External issuers/storage; Profiles set assurance | `NM`, `STA`, `CA` | Issuer/source/freshness/conflict/redaction/causality vectors | AA-2 semantic; AA-3 threat/trust; AA-5 conformance |
| EI-14 | Core semantic equivalence boundary | Profiles and Bridges provide scoped mappings | `NM`, `CA` | Differential allowed-set and incompatible-semantics tests | AA-2 / AA-5 |
| EI-15 | Core evolution/criticality rules | Profiles/Extensions/governance | `CON`, `NM`, `CA` | Unknown critical/optional, collision, dependency and downgrade tests | AA-2 / AA-5 |
| EI-16 | Binding/security composition contract | Reviewed lower-layer primitives | `STA`, `BC`, `CA` | Coverage, order, mutation, fallback and provider-substitution analysis | AA-4 / AA-5 |
| EI-17 | Bridge contract | External protocol mappings | `BR`, `STA`, `CA` | Directed/versioned loss, drift, loop, impersonation and assurance tests | AA-4 / AA-5 |
| EI-18 | Core multi-party epoch/dependency closure | Profiles define membership/quorum | `NM`, `STA`, `CA` | Join/leave/split-view/branch-epoch aggregation models | AA-2 semantic; AA-3 threat/trust/mechanism; AA-5 conformance |
| EI-19 | Privacy architecture | Profiles/Bindings/Conformance claims | `STA`, `CA` | Per-observer disclosure, metadata/linkability and lifecycle review | AA-3 / AA-5 |
| EI-20 | Bounded failure/resource architecture | Binding runtime and external adapters | `BC`, `CA` | Hostile size/depth/rate, slow consumer, outage and amplification tests | AA-4 / AA-5 |
| EI-21 | Decentralization/dependency architecture | Replaceable external systems and Base Binding | `CON`, `BC`, `II` | Provider outage/replacement and no-purchase Native path | AA-4 design; empirical AA-7 |
| EI-22 | Conformance/independent implementation architecture | Rights/governance and independent implementations | `CA`, `IB`, `II`, `PUB` | Coverage, observer qualification, genealogy and fresh-team interop | AA-6 design; empirical AA-7; rights AA-8 |
| EI-23 | Artifact/version/claim lifecycle | All versioned artifacts and governance | `CON`, `CA`, `PUB` | Coexistence, migration, drift, vulnerability invalidation and rerun | AA-5; publication AA-8 |
| EI-24 | Scoped cross-provider comparability | Profiles and Bridges | `NM`, `BR`, `CA` | Units/schema/provenance/partial-boundary comparison vectors | AA-5 |
| EI-25 | Threat/evidence/measurement boundary | Security, Conformance and evidence custody | `STA`, `CA`, `IB` | Threat coverage, non-interference, executed-artifact provenance and failing trace | AA-3 / AA-6 |

Audit coverage: EI-01–EI-25 are listed individually; their frozen meaning is not modified.

## 6. Frozen Safety Blocking Classes SBC-01–SBC-10

Every confirmed in-scope SBC blocks at any severity. Project Owner cannot accept it as residual risk.

Frozen observer rule: every SBC evaluation where Protected Disclosure or Consequential Effect is possible requires at least two independently derived observer paths. Shared oracle code or a common derivation does not establish independence. A missing required path or unresolved disagreement between the paths makes the evaluation invalid/inconclusive, never pass. This rule applies to every `CA` conformance route and later integrated rerun; no later Gate may relax it.

| ID | Primary responsibility | Supporting layer | Normative artifact | Verification | Closure gate |
| --- | --- | --- | --- | --- | --- |
| SBC-01 | Core authority safety | Profiles, security Binding, effect adapter | `NM`, `STA`, `CA` | Unauthorized effect/disclosure, amplification, stale/revoked and confused-deputy corpus | AA-2 normative-model; AA-3 threat/trust/mechanism; AA-5 conformance |
| SBC-02 | Core boundary atomicity | External effect/disclosure enforcement | `NM`, `STA`, `CA` | TOCTOU/revoke/expiry/subject mutation model and fault tests | AA-2 normative-model; AA-3 threat/trust/mechanism; AA-5 conformance |
| SBC-03 | Shared-limit safety contract | Profile plus external shared-limit authority | `NM`, `STA`, `CA` | Concurrent/partitioned overspend and quorum tests | AA-2 normative-model; AA-3 threat/trust/mechanism; AA-5 conformance |
| SBC-04 | Core lifecycle/effect truthfulness | Durable endpoint/effect source | `NM`, `CA` | Lost response, duplicate, unknown/partial, cancel and compensation faults | AA-2 / AA-5 |
| SBC-05 | End-to-end semantic integrity | Profiles/Extensions/Bindings/Bridges | `STA`, `BC`, `BR`, `CA` | Unknown mandatory, downgrade, mapping loss, ambiguity and impersonation tests | AA-4 / AA-5 |
| SBC-06 | Privacy/isolation architecture | Profiles, Binding, sandbox/storage controls | `STA`, `CA`, `PUB` | Data-flow, tenant/branch/egress, metadata and retention/deletion tests | AA-3 / AA-5; publication AA-8 |
| SBC-07 | Resource safety architecture | Binding/runtime and external adapters | `BC`, `CA` | Size/depth/rate/queue/retry/amplification/deadlock ceilings | AA-4 / AA-5 |
| SBC-08 | Evidence integrity architecture | Evidence issuers/storage, observers and custody | `NM`, `STA`, `CA`, `IB` | Provenance/equivocation/observer-independence and immutable-trace audit | AA-2 normative-model; AA-3 threat/trust; AA-5 conformance; AA-6 integrated |
| SBC-09 | Independence/dependency architecture | Governance, build provenance and implementations | `CON`, `CA`, `IB`, `II`, `PUB` | Hidden dependency, shared code, private mapping, SBOM and genealogy audit | AA-6 design; empirical AA-7; rights AA-8 |
| SBC-10 | Claim-boundary governance | All scoped artifacts and reviewers | `STA`, `CA`, `IB` | Assumption/dependency completeness, threat coverage and scope-impact review | AA-6 |

Audit coverage: SBC-01–SBC-10 are listed individually; closure follows the frozen six-step correction/retest process and preserves failed evidence.

## 7. Accountable-owner and bidirectional traceability companion

This companion completes FR-99 traceability for AA-1. It is an index in both directions: a requirement reader follows the row to its owner, AD, QAS and preliminary threat/boundary records; a QAS, AD, AST or BND reader finds every linked requirement by reverse lookup in this table. `AD-4+` entries are downstream decision slots from `ADR-DISCIPLINE.md`; their proposed status does not make them adopted. Exact model/assertion/vector identifiers are added at the owning downstream Gate without replacing these links.

### 7.1 Functional requirements

| IDs | Accountable owner role | Governing/required AD | Quality scenarios | Preliminary assets / boundaries |
| --- | --- | --- | --- | --- |
| FR-1 | Role/Context Model Owner | AD-1, AD-4 | QAS-IMP-01, QAS-IMP-02 | AST-13; BND-03, BND-07 |
| FR-2 | Role/Context Model Owner | AD-1, AD-4, AD-10 | QAS-IMP-02, QAS-DEC-01 | AST-05; BND-03, BND-07 |
| FR-3 | Role/Context Model Owner | AD-1, AD-4, AD-5 | QAS-SEC-01 | AST-01, AST-02; BND-01, BND-07 |
| FR-4 | Identity/Privacy Architecture Owner | AD-4, AD-8, AD-9 | QAS-PRI-01, QAS-PRI-02, QAS-RES-04 | AST-05, AST-10; BND-05, BND-09, BND-14 |
| FR-5 | Role/Context Model Owner | AD-1, AD-4 | QAS-SEC-01, QAS-IMP-01 | AST-01; BND-01, BND-07 |
| FR-6 | Authority/Privacy Architecture Owner | AD-4, AD-5, AD-8 | QAS-SEC-01, QAS-PRI-01 | AST-02, AST-04; BND-05, BND-06 |
| FR-7 | Evidence/Provenance Owner | AD-4, AD-8, AD-13 | QAS-COR-03, QAS-DEC-02 | AST-09; BND-07, BND-12 |
| FR-8 | Interaction Model Owner | AD-4, AD-6 | QAS-PRI-02, QAS-COR-02 | AST-03, AST-08; BND-03, BND-15 |
| FR-9 | Role/Context Model Owner + Security Lead | AD-4, AD-8, AD-13 | QAS-SEC-01, QAS-PRI-01 | AST-01–AST-05; BND-03, BND-05, BND-06 |
| FR-10 | Capability Model Owner | AD-4 | QAS-SEC-01, QAS-SEC-03–05, QAS-COR-01–02, QAS-RES-01, QAS-RES-03–04, QAS-PERF-02, QAS-DEC-03 | AST-01, AST-03, AST-07, AST-08, AST-12; BND-01, BND-03, BND-06, BND-13 |
| FR-11 | Discovery/Dependency Owner | AD-2, AD-4, AD-10 | QAS-COR-01–04, QAS-RES-02, QAS-IMP-01–02, QAS-DEC-01–02 | AST-10, AST-13; BND-09, BND-10, BND-15 |
| FR-12 | Capability/Privacy Owner | AD-4, AD-8 | QAS-PRI-01, QAS-PERF-01–02 | AST-04, AST-05, AST-12; BND-05, BND-14 |
| FR-13 | Capability/Trust Owner | AD-4, AD-8 | QAS-RES-01, QAS-RES-03, QAS-PERF-01–02 | AST-10, AST-12; BND-09, BND-10 |
| FR-14 | Negotiation Model Owner | AD-4, AD-13 | QAS-SEC-04, QAS-COR-04, QAS-IMP-01–02 | AST-11, AST-13; BND-04, BND-15 |
| FR-15 | Negotiation/Extension Owner | AD-2, AD-4, AD-13 | QAS-SEC-04–05, QAS-RES-02, QAS-PERF-01–02, QAS-EVO-02, QAS-DEC-01–03 | AST-11–AST-13; BND-04, BND-09, BND-10, BND-16 |
| FR-16 | Negotiation/Extension Owner | AD-2, AD-4, AD-13 | QAS-SEC-04, QAS-COR-04, QAS-EVO-02, QAS-IMP-01–02 | AST-11, AST-13; BND-04, BND-15, BND-16 |
| FR-17 | Negotiation/Binding Owner | AD-4, AD-9, AD-10 | QAS-SEC-04, QAS-IMP-01, QAS-IMP-03, QAS-DEC-01 | AST-10, AST-11, AST-13; BND-04, BND-09, BND-10 |
| FR-18 | Negotiation/Evolution Owner | AD-4, AD-8, AD-13 | QAS-SEC-04, QAS-RES-02, QAS-RES-04, QAS-EVO-01–03, QAS-DEC-02 | AST-10, AST-11, AST-13; BND-04, BND-08, BND-09, BND-16 |
| FR-19 | Negotiation/Evolution Owner | AD-4, AD-8–AD-10, AD-13 | QAS-SEC-04, QAS-PRI-03, QAS-COR-04, QAS-EVO-01–03, QAS-IMP-02–03 | AST-04, AST-11–AST-13; BND-04, BND-10, BND-15, BND-16 |
| FR-20 | Interaction/Privacy Owner | AD-4, AD-6, AD-8 | QAS-PRI-01–04, QAS-COR-03, QAS-IMP-03 | AST-04, AST-08, AST-09; BND-03, BND-05, BND-12, BND-14 |
| FR-21 | Interaction/Lifecycle Owner | AD-4, AD-6 | QAS-PRI-04, QAS-COR-01–03 | AST-08, AST-09; BND-03, BND-06, BND-12 |
| FR-22 | Interaction/Conformance Owner | AD-4, AD-6, AD-13 | QAS-IMP-01, QAS-IMP-03 | AST-08, AST-13; BND-03, BND-15 |
| FR-23 | Streaming/Resource Owner | AD-6, AD-10, AD-12 | QAS-PRI-01–04, QAS-RES-01, QAS-RES-03, QAS-PERF-01 | AST-04, AST-08, AST-12; BND-03, BND-05, BND-10, BND-14 |
| FR-24 | Interaction/Binding Owner | AD-4, AD-6, AD-10 | QAS-COR-02, QAS-RES-03 | AST-08, AST-12; BND-03, BND-10 |
| FR-25 | Multi-party Model Owner | AD-4, AD-5, AD-6 | QAS-SEC-05, QAS-PRI-02 | AST-07, AST-08; BND-13, BND-14 |
| FR-26 | Authority/Decision Model Owner | AD-4, AD-5 | QAS-SEC-01, QAS-SEC-05 | AST-01, AST-03; BND-01, BND-06 |
| FR-27 | Interaction/Binding Owner | AD-4, AD-6, AD-10 | QAS-COR-01, QAS-RES-04 | AST-08, AST-12; BND-03, BND-10 |
| FR-28 | Interaction/Lifecycle Owner | AD-6, AD-10 | QAS-COR-02, QAS-RES-03 | AST-08, AST-12; BND-03, BND-06 |
| FR-29 | Resilience/Binding Owner | AD-6, AD-10, AD-12 | QAS-RES-01, QAS-RES-02, QAS-RES-04 | AST-08, AST-12; BND-03, BND-10 |
| FR-30 | Authority/Decision Model Owner | AD-5, AD-8 | QAS-SEC-01 | AST-01, AST-02; BND-01, BND-06 |
| FR-31 | Authority/Decision Model Owner | AD-5, AD-8 | QAS-SEC-01, QAS-SEC-03 | AST-01, AST-02, AST-06; BND-01, BND-06 |
| FR-32 | Delegation Model Owner | AD-5, AD-8 | QAS-SEC-01–02 | AST-01, AST-02; BND-01, BND-02, BND-06 |
| FR-33 | Delegation/Resource Owner | AD-5, AD-8, AD-12 | QAS-SEC-01–02, QAS-PERF-02 | AST-01, AST-02, AST-12; BND-01, BND-02, BND-06 |
| FR-34 | Consent/Approval Model Owner | AD-5, AD-8 | QAS-SEC-01–02 | AST-01, AST-02, AST-06; BND-01, BND-02, BND-06 |
| FR-35 | Authorization Model Owner | AD-5, AD-8 | QAS-SEC-01 | AST-01, AST-02, AST-06; BND-01, BND-06 |
| FR-36 | Multi-party Authority Owner | AD-5, AD-8 | QAS-SEC-05, QAS-DEC-03 | AST-01, AST-07; BND-08, BND-13 |
| FR-37 | Authority/Security Owner | AD-5, AD-8 | QAS-SEC-03, QAS-RES-04 | AST-02, AST-10; BND-06, BND-08 |
| FR-38 | Authority/Security Owner | AD-5, AD-6, AD-8 | QAS-SEC-02, QAS-SEC-05, QAS-COR-01 | AST-02, AST-07; BND-02, BND-13 |
| FR-39 | Authority/Security Owner | AD-5, AD-6, AD-8 | QAS-SEC-03, QAS-COR-02 | AST-02, AST-10; BND-06, BND-08 |
| FR-40 | Security/Recovery Owner | AD-8, AD-9 | QAS-RES-02, QAS-RES-04 | AST-10; BND-08–BND-10 |
| FR-41 | Authority/Privacy Owner | AD-5, AD-8, AD-9 | QAS-PRI-01, QAS-PRI-02 | AST-02, AST-04, AST-05; BND-05, BND-09 |
| FR-42 | Security/Binding/Bridge Owner | AD-8–AD-10 | QAS-SEC-02, QAS-COR-04, QAS-EVO-03 | AST-02, AST-11; BND-10, BND-11 |
| FR-43 | Lifecycle Model Owner | AD-6, AD-10 | QAS-COR-01, QAS-RES-04 | AST-03, AST-08; BND-03, BND-06 |
| FR-44 | Lifecycle Model Owner | AD-6 | QAS-COR-01, QAS-COR-02 | AST-08; BND-06 |
| FR-45 | Lifecycle/Effect Owner | AD-5, AD-6 | QAS-SEC-03, QAS-COR-01 | AST-06, AST-08; BND-06 |
| FR-46 | Lifecycle/Effect Owner | AD-6, AD-8 | QAS-COR-01, QAS-COR-03 | AST-06, AST-08; BND-06, BND-12 |
| FR-47 | Lifecycle/Binding Owner | AD-6, AD-10 | QAS-COR-01, QAS-RES-04 | AST-03, AST-08; BND-03, BND-06 |
| FR-48 | Lifecycle/Security Owner | AD-5, AD-6, AD-8 | QAS-SEC-03, QAS-COR-02 | AST-02, AST-08; BND-06 |
| FR-49 | Lifecycle/Authority Owner | AD-5, AD-6 | QAS-COR-02 | AST-01, AST-06, AST-08; BND-01, BND-06 |
| FR-50 | Multi-party Lifecycle Owner | AD-5, AD-6 | QAS-SEC-05, QAS-COR-02 | AST-07, AST-08; BND-13, BND-14 |
| FR-51 | Evidence/Provenance Owner | AD-6, AD-8, AD-13 | QAS-COR-03, QAS-RES-04 | AST-08, AST-09; BND-12 |
| FR-52 | Lifecycle/Binding Owner | AD-6, AD-10, AD-13 | QAS-PRI-03, QAS-IMP-03 | AST-08, AST-12; BND-03, BND-10 |
| FR-53 | Evidence/Provenance Owner | AD-5, AD-6, AD-13 | QAS-COR-03 | AST-03, AST-09; BND-12 |
| FR-54 | Trust/Evidence Owner | AD-8, AD-9, AD-13 | QAS-COR-03, QAS-DEC-02 | AST-09, AST-10; BND-09, BND-12 |
| FR-55 | Evidence/Privacy Owner | AD-8, AD-13 | QAS-PRI-04, QAS-COR-03 | AST-04, AST-09; BND-12, BND-14 |
| FR-56 | Artifact Identity Owner | AD-2, AD-13 | QAS-SEC-04, QAS-EVO-01 | AST-11, AST-13; BND-04, BND-16 |
| FR-57 | Semantic Compatibility Owner | AD-2, AD-13 | QAS-COR-04, QAS-EVO-01, QAS-IMP-02 | AST-11, AST-13; BND-04, BND-15, BND-16 |
| FR-58 | Criticality/Evolution Owner | AD-2, AD-13 | QAS-SEC-04, QAS-EVO-01–02 | AST-11, AST-13; BND-04, BND-16 |
| FR-59 | Extension Namespace/Governance Owner | AD-2, AD-13, AD-17 | QAS-SEC-04, QAS-EVO-01–02 | AST-11, AST-14; BND-04, BND-16 |
| FR-60 | Protocol Safety-Floor Owner | AD-1, AD-2, AD-13 | QAS-SEC-04, QAS-EVO-01–02 | AST-11, AST-13; BND-04, BND-16 |
| FR-61 | Extension Dependency Owner | AD-2, AD-13 | QAS-SEC-04, QAS-PERF-02, QAS-EVO-01–02 | AST-11–AST-13; BND-04, BND-16 |
| FR-62 | Profile Domain-Boundary Owner | AD-2, AD-13 | QAS-SEC-04, QAS-EVO-01 | AST-11, AST-13; BND-04, BND-16 |
| FR-63 | Cross-Profile Contract Owner | AD-2, AD-13 | QAS-SEC-04, QAS-EVO-01 | AST-11, AST-13; BND-04, BND-16 |
| FR-64 | Negotiated Closure Owner | AD-2, AD-13 | QAS-SEC-04, QAS-EVO-01–02 | AST-11, AST-13; BND-04, BND-16 |
| FR-65 | Migration/Withdrawal Owner | AD-8, AD-9, AD-13 | QAS-SEC-04, QAS-RES-04, QAS-EVO-01, QAS-EVO-03 | AST-10, AST-11, AST-13; BND-04, BND-08, BND-16 |
| FR-66 | Normative Change Owner | AD-2, AD-13, AD-17 | QAS-EVO-01 | AST-11, AST-13, AST-14; BND-04, BND-16 |
| FR-67 | Promotion/Removal Owner | AD-2, AD-13, AD-17 | QAS-EVO-01–02 | AST-11, AST-13, AST-14; BND-04, BND-16 |
| FR-68 | Native Path Eligibility Owner | AD-2, AD-10, AD-15 | QAS-DEC-01 | AST-13, AST-14; BND-09, BND-10, BND-16 |
| FR-69 | Binding Contract Owner | AD-9, AD-10 | QAS-SEC-02, QAS-COR-04, QAS-EVO-03, QAS-IMP-02, QAS-DEC-02 | AST-02, AST-10–AST-12; BND-09, BND-10 |
| FR-70 | Binding Semantic-Invariance Owner | AD-9, AD-10 | QAS-SEC-02, QAS-COR-04, QAS-EVO-03, QAS-IMP-02, QAS-DEC-02 | AST-02, AST-10–AST-12; BND-09, BND-10 |
| FR-71 | Binding Coverage Owner | AD-9, AD-10, AD-13 | QAS-SEC-02, QAS-SEC-04, QAS-COR-04, QAS-EVO-03, QAS-IMP-02, QAS-DEC-02 | AST-02, AST-10–AST-13; BND-09, BND-10, BND-15 |
| FR-72 | Binding Boundary/Privacy Owner | AD-8–AD-10 | QAS-SEC-02, QAS-PRI-01, QAS-PRI-03, QAS-EVO-03, QAS-DEC-02 | AST-02, AST-04, AST-10–AST-12; BND-05, BND-09, BND-10 |
| FR-73 | Provider Isolation Owner | AD-2, AD-8, AD-10 | QAS-SEC-02, QAS-RES-02, QAS-EVO-03, QAS-DEC-01–03 | AST-10–AST-12; BND-09, BND-10, BND-13 |
| FR-74 | Fallback/Dependency Owner | AD-2, AD-8, AD-10, AD-12 | QAS-SEC-02, QAS-RES-01–02, QAS-PERF-01, QAS-EVO-03, QAS-DEC-01–03 | AST-10–AST-12; BND-09, BND-10, BND-13 |
| FR-75 | Bridge Eligibility Owner | AD-2, AD-8, AD-22 | QAS-SEC-02, QAS-RES-02, QAS-EVO-03, QAS-DEC-01 | AST-10, AST-11, AST-13; BND-11 |
| FR-76 | Bridge Direction/Version Owner | AD-2, AD-8, AD-13 | QAS-SEC-02, QAS-EVO-03 | AST-10, AST-11; BND-11 |
| FR-77 | Bridge Loss-Mapping Owner | AD-2, AD-8, AD-13 | QAS-SEC-02, QAS-EVO-03 | AST-02, AST-09–AST-11; BND-11 |
| FR-78 | Bridge Assurance Owner | AD-2, AD-8, AD-9, AD-13 | QAS-SEC-02, QAS-EVO-03 | AST-02, AST-09–AST-11; BND-11 |
| FR-79 | Bridge Provenance/Loop Owner | AD-2, AD-8, AD-13 | QAS-EVO-03 | AST-09–AST-11; BND-11 |
| FR-80 | Bridge Resource/Failure Owner | AD-2, AD-8, AD-12, AD-13 | QAS-RES-01, QAS-PERF-02, QAS-EVO-03 | AST-10–AST-12; BND-11 |
| FR-81 | Bridge Expiry/Isolation Owner | AD-2, AD-8, AD-13 | QAS-SEC-04, QAS-RES-02, QAS-EVO-01, QAS-EVO-03, QAS-DEC-02 | AST-10, AST-11, AST-13; BND-11 |
| FR-82 | Conformance Claim Owner | AD-3, AD-13 | QAS-COR-04, QAS-IMP-01–03, QAS-DEC-01 | AST-13, AST-14; BND-15, BND-16 |
| FR-83 | Open Suite Owner | AD-13, AD-15, AD-17 | QAS-IMP-01, QAS-IMP-03, QAS-DEC-01 | AST-13, AST-14; BND-15, BND-16 |
| FR-84 | Obligation Coverage Owner | AD-13 | QAS-IMP-01, QAS-IMP-03, QAS-DEC-01 | AST-13; BND-15 |
| FR-85 | Implementation Independence Owner | AD-14, AD-15 | QAS-IMP-01–02, QAS-DEC-01 | AST-13, AST-14; BND-15, BND-16 |
| FR-86 | Differential Interop Owner | AD-13, AD-14 | QAS-COR-04, QAS-IMP-01–02 | AST-13; BND-07, BND-15 |
| FR-87 | Topology Matrix Owner | AD-3, AD-13, AD-14 | QAS-IMP-01–02 | AST-13; BND-07, BND-15 |
| FR-88 | Negative/Adversarial Suite Owner | AD-8, AD-13 | QAS-RES-01, QAS-EVO-02, QAS-IMP-01–02 | AST-10–AST-13; BND-15 |
| FR-89 | Property/Fuzz Owner | AD-12, AD-13 | QAS-RES-01, QAS-PERF-01–02, QAS-IMP-01–02 | AST-12, AST-13; BND-10, BND-15 |
| FR-90 | Concurrency/Fault Owner | AD-6, AD-12, AD-13 | QAS-RES-01, QAS-PERF-01–02, QAS-IMP-01–02 | AST-08, AST-12, AST-13; BND-06, BND-10, BND-15 |
| FR-91 | Version-Combination Owner | AD-9, AD-10, AD-13 | QAS-SEC-04, QAS-COR-04, QAS-EVO-01–03, QAS-IMP-01–02, QAS-DEC-02 | AST-10, AST-11, AST-13; BND-04, BND-10, BND-11, BND-15 |
| FR-92 | Reproducibility Owner | AD-13, AD-14 | QAS-IMP-01, QAS-IMP-03 | AST-13; BND-15 |
| FR-93 | Specification Primacy Owner | AD-13, AD-14 | QAS-IMP-01, QAS-IMP-03 | AST-13; BND-15, BND-16 |
| FR-94 | Suite Evolution Owner | AD-13, AD-14, AD-17 | QAS-EVO-01, QAS-IMP-01, QAS-IMP-03 | AST-13, AST-14; BND-15, BND-16 |
| FR-95 | Scoped Claim Validity Owner | AD-8, AD-13, AD-14, AD-17 | QAS-SEC-04, QAS-RES-02, QAS-RES-04, QAS-EVO-01, QAS-EVO-03, QAS-IMP-01, QAS-IMP-03, QAS-DEC-02 | AST-10, AST-13, AST-14; BND-08–BND-12, BND-15, BND-16 |
| FR-96 | Architecture Assurance Lead | AD-1–AD-3, AD-21 | QAS-SEC-01–05, QAS-PRI-01–04, QAS-COR-01–04, QAS-RES-01–04, QAS-PERF-01–02, QAS-EVO-01–03, QAS-IMP-01–03, QAS-DEC-01–03 | AST-13, AST-14; BND-16 |
| FR-97 | Architecture Research Lead | AD-2, AD-3, AD-9, AD-10, AD-22 | QAS-RES-02, QAS-EVO-03, QAS-DEC-02 | `LANDSCAPE-DISPOSITION.md`; AST-10, AST-11, AST-13; BND-09–BND-11 |
| FR-98 | Chief Architect | AD-1–AD-3 | QAS-IMP-01, QAS-DEC-01, QAS-DEC-03 | AST-13, AST-14; BND-16 |
| FR-99 | Traceability/Configuration Owner | AD-1–AD-22 | QAS-SEC-01–05, QAS-PRI-01–04, QAS-COR-01–04, QAS-RES-01–04, QAS-PERF-01–02, QAS-EVO-01–03, QAS-IMP-01–03, QAS-DEC-01–03 | AST-01–AST-14; BND-01–BND-16 |
| FR-100 | Conformance Owner | AD-3, AD-13, AD-14, AD-22 | QAS-IMP-01, QAS-IMP-02, QAS-DEC-01 | AST-13; BND-07, BND-15; `AA-7-DIVERSITY-ELIGIBILITY.md` |
| FR-101 | Gate Owner + applicable Independent Review Chairs | AD-1–AD-3, AD-8, AD-13–AD-17, AD-21 | QAS-SEC-01–05, QAS-PRI-01–04, QAS-RES-01–04, QAS-IMP-01–03 | AST-01–AST-14; BND-01–BND-16 |
| FR-102 | Conformance Independence Lead | AD-13–AD-15, AD-21 | QAS-IMP-01, QAS-IMP-02 | AST-13, AST-14; BND-15, BND-16 |
| FR-103 | Formal Methods Lead | AD-5–AD-7 | QAS-SEC-02, QAS-SEC-03, QAS-COR-01, QAS-COR-02 | AST-02, AST-03, AST-06–AST-08; BND-02, BND-06, BND-13 |
| FR-104 | Distributed Systems Lead + Security Lead | AD-8–AD-10, AD-12 | QAS-RES-02, QAS-RES-04, QAS-DEC-02 | AST-10–AST-12; BND-08–BND-13 |
| FR-105 | Performance/Resource Lead | AD-10–AD-13 | QAS-RES-01, QAS-RES-03, QAS-PERF-01, QAS-PERF-02 | AST-12, AST-13; BND-03, BND-10, BND-15 |
| FR-106 | Protocol Evolution Lead | AD-2, AD-3, AD-9, AD-10, AD-13 | QAS-SEC-04, QAS-EVO-01–03 | AST-10, AST-11, AST-13; BND-04, BND-10, BND-11, BND-16 |
| FR-107 | Evidence Custodian | AD-13, AD-14 | QAS-COR-03, QAS-IMP-03 | AST-09, AST-13; BND-12, BND-15 |
| FR-108 | Review Coordinator + Gate Owner | AD-1–AD-3, AD-7–AD-21 | QAS-SEC-01–05, QAS-PRI-01–04, QAS-COR-01–04, QAS-RES-01–04, QAS-PERF-01–02, QAS-EVO-01–03, QAS-IMP-01–03, QAS-DEC-01–03 | AST-01–AST-14; BND-01–BND-16 |
| FR-109 | Gate Owner | AD-1–AD-21 | QAS-SEC-01–05, QAS-PRI-01–04, QAS-COR-01–04, QAS-RES-01–04, QAS-PERF-01–02, QAS-EVO-01–03, QAS-IMP-01–03, QAS-DEC-01–03 | AST-13, AST-14; BND-15, BND-16 |
| FR-110 | Gate Owner; Project Owner for final stop | AD-1–AD-3, AD-15–AD-21 | QAS-SEC-01–05, QAS-PRI-01–04, QAS-COR-01–04, QAS-RES-01–04, QAS-PERF-01–02, QAS-EVO-01–03, QAS-IMP-01–03, QAS-DEC-01–03 | AST-13, AST-14; BND-15, BND-16 |

### 7.2 Non-functional requirements

| IDs | Accountable owner role | Governing/required AD | Quality scenarios | Preliminary assets / boundaries |
| --- | --- | --- | --- | --- |
| NFR-1 | Security Lead | AD-8 | QAS-SEC-01–05, QAS-PRI-01–04, QAS-RES-01–04 | AST-01–AST-14; BND-01–BND-16 |
| NFR-2 | Security Lead + Normative Model Owner | AD-5, AD-8 | QAS-SEC-01–05 | AST-01–AST-11; BND-03–BND-13 |
| NFR-3 | Security Binding Owner | AD-8–AD-10 | QAS-SEC-02, QAS-SEC-04, QAS-COR-04 | AST-02, AST-03, AST-09–AST-11; BND-10, BND-11 |
| NFR-4 | Privacy Lead | AD-5, AD-8 | QAS-PRI-01–04 | AST-04, AST-05, AST-09; BND-05, BND-12, BND-14 |
| NFR-5 | Security Lead | AD-8 | QAS-RES-02, QAS-RES-04 | AST-02, AST-04, AST-10; BND-08–BND-11 |
| NFR-6 | Security Binding Owner | AD-8–AD-10 | QAS-SEC-02, QAS-SEC-04, QAS-EVO-02–03, QAS-DEC-02 | AST-02, AST-10, AST-11; BND-09, BND-10 |
| NFR-7 | Native Dependency Owner | AD-2, AD-8, AD-10 | QAS-RES-02, QAS-EVO-03, QAS-DEC-01–03 | AST-10, AST-11, AST-13; BND-09, BND-10, BND-13 |
| NFR-8 | Lifecycle/Recovery Owner | AD-6, AD-8 | QAS-SEC-03, QAS-COR-01–03, QAS-RES-01–04, QAS-DEC-02 | AST-02, AST-03, AST-06, AST-08, AST-10, AST-12; BND-03, BND-06, BND-08, BND-13 |
| NFR-9 | Resource/Privacy Owner | AD-8, AD-12 | QAS-PRI-03, QAS-RES-01, QAS-RES-03, QAS-PERF-01–02 | AST-04, AST-12; BND-03, BND-05, BND-10 |
| NFR-10 | Time/Ordering/Partition Owner | AD-4–AD-6, AD-8, AD-12 | QAS-SEC-01, QAS-SEC-03–05, QAS-COR-01–02, QAS-RES-01, QAS-RES-03–04, QAS-PERF-02, QAS-DEC-03 | AST-02, AST-03, AST-06–AST-08, AST-12; BND-03, BND-06, BND-13 |
| NFR-11 | Allowed-Outcome Determinism Owner | AD-4, AD-6, AD-7, AD-13 | QAS-COR-01–04, QAS-IMP-01–02 | AST-03, AST-06, AST-08, AST-13; BND-06, BND-15 |
| NFR-12 | Performance Budget Owner | AD-10–AD-12 | QAS-PERF-01–02 | AST-12; BND-03, BND-10 |
| NFR-13 | Resource Guardrail Owner | AD-10–AD-12 | QAS-RES-01, QAS-RES-03, QAS-PERF-01–02 | AST-12; BND-03, BND-10 |
| NFR-14 | Runtime Portability Owner | AD-10, AD-11, AD-14, AD-22 | QAS-COR-04, QAS-IMP-01–02 | AST-13; BND-10, BND-15; `AA-7-DIVERSITY-ELIGIBILITY.md` |
| NFR-15 | Decentralized Scale Owner | AD-2, AD-10, AD-12 | QAS-SEC-05, QAS-RES-02, QAS-PERF-01–02, QAS-DEC-01–03 | AST-07, AST-10, AST-12, AST-13; BND-09, BND-10, BND-13 |
| NFR-16 | Language/Vendor Neutrality Owner | AD-13–AD-15, AD-22 | QAS-COR-04, QAS-IMP-01–02, QAS-DEC-01 | AST-13, AST-14; BND-15, BND-16; `AA-7-DIVERSITY-ELIGIBILITY.md` |
| NFR-17 | Self-Sufficient Contract Owner | AD-13–AD-16 | QAS-COR-04, QAS-IMP-01–03, QAS-DEC-01 | AST-13, AST-14; BND-15, BND-16 |
| NFR-18 | Staged Evolution Owner | AD-2, AD-3, AD-13 | QAS-SEC-04, QAS-COR-04, QAS-RES-04, QAS-EVO-01–03, QAS-IMP-02, QAS-DEC-02 | AST-10, AST-11, AST-13, AST-14; BND-04, BND-08, BND-11, BND-16 |
| NFR-19 | Extension Evolution Owner | AD-2, AD-3, AD-13 | QAS-SEC-04, QAS-COR-04, QAS-EVO-01–03, QAS-IMP-02 | AST-11, AST-13, AST-14; BND-04, BND-11, BND-16 |
| NFR-20 | Privacy-Safe Diagnostics Owner | AD-8, AD-13 | QAS-PRI-01–04, QAS-COR-03, QAS-IMP-03 | AST-04, AST-05, AST-09, AST-13; BND-05, BND-12, BND-14, BND-15 |
| NFR-21 | Causal Provenance Owner | AD-6, AD-8, AD-13 | QAS-PRI-04, QAS-COR-01–03, QAS-IMP-03 | AST-08, AST-09, AST-13; BND-06, BND-12, BND-15 |
| NFR-22 | Measurement Independence Owner | AD-12–AD-14 | Every applicable Protected Disclosure / Consequential Effect SBC route; QAS-IMP-01, QAS-IMP-03 | AST-12, AST-13; BND-15 |
| NFR-23 | Privacy/Data Governance Owner | AD-8, AD-13, AD-15 | QAS-PRI-01, QAS-PRI-02 | AST-04, AST-05, AST-09; BND-05, BND-12, BND-14 |
| NFR-24 | Privacy/Data Governance Owner | AD-8, AD-10, AD-13 | QAS-PRI-04, QAS-COR-03 | AST-04, AST-09; BND-05, BND-12 |
| NFR-25 | Security/Supply-chain Owner | AD-8–AD-10, AD-15 | QAS-PRI-03, QAS-RES-02 | AST-04, AST-10, AST-13; BND-03, BND-10, BND-15 |
| NFR-26 | Supply-chain/Evidence Owner | AD-10–AD-14 | QAS-RES-02, QAS-IMP-01, QAS-IMP-02 | AST-10, AST-13; BND-10, BND-15, BND-16 |
| NFR-27 | IP/Rights Owner + Gate Owner | AD-15 | QAS-IMP-01, QAS-DEC-01 | AST-13, AST-14; BND-15, BND-16 |
| NFR-28 | Protocol/Platform Firewall Owner | AD-17, AD-20 | QAS-DEC-01, QAS-DEC-02 | AST-13, AST-14; BND-09, BND-16 |
| NFR-29 | Security Response Owner | AD-8, AD-13, AD-15, AD-17 | QAS-EVO-03, QAS-RES-02 | AST-10, AST-13, AST-14; BND-08–BND-12, BND-16 |

### 7.3 Evaluation invariants

| ID | Accountable owner role | Governing/required AD | Quality scenarios | Preliminary assets / boundaries | Verification | Closure Gate |
| --- | --- | --- | --- | --- | --- | --- |
| EI-01 | Role/Context Model Owner | AD-1, AD-4 | QAS-IMP-01, QAS-IMP-02, QAS-DEC-01 | AST-05, AST-13; BND-03, BND-07 | Role/owner/transport-position swaps and ambiguous-context vectors | AA-2 / AA-5 |
| EI-02 | Negotiation/Security Owner | AD-4, AD-8, AD-9 | QAS-SEC-04 | AST-10, AST-11; BND-04, BND-10 | Circular negotiation, transcript-equivalence and downgrade vectors | AA-2 semantic; AA-3 threat/mechanism; AA-5 conformance |
| EI-03 | Capability/Discovery Owner | AD-4, AD-10, AD-13 | QAS-SEC-04 | AST-10, AST-11; BND-04, BND-09 | Stale declaration, unsupported mandatory and non-permit vectors | AA-2 / AA-5 |
| EI-04 | Interaction Model Owner | AD-4, AD-6 | QAS-PRI-02, QAS-RES-03 | AST-05, AST-08; BND-03, BND-14 | Reconnect/reorder/reverse/branch trace models | AA-2 / AA-5 |
| EI-05 | Decision Subject Owner | AD-5, AD-6 | QAS-SEC-01, QAS-SEC-03 | AST-01, AST-03, AST-06; BND-01, BND-06 | Security-critical mutation and reuse vectors | AA-2 semantic; AA-3 threat/mechanism; AA-5 conformance |
| EI-06 | Delegation/Authority Owner | AD-5, AD-8 | QAS-SEC-01, QAS-SEC-02, QAS-RES-04 | AST-02, AST-10; BND-02, BND-09 | Formal monotonicity plus caveat-loss/confused-deputy/epoch vectors | AA-2 semantic; AA-3 threat/trust/mechanism; AA-5 conformance |
| EI-07 | Consent/Approval Owner | AD-5, AD-8 | QAS-SEC-01 | AST-01, AST-02; BND-01, BND-08 | Proposal/consent/approval confusion and independence vectors | AA-2 semantic; AA-3 trust/mechanism; AA-5 conformance |
| EI-08 | Effect Boundary Owner | AD-5, AD-6, AD-8 | QAS-SEC-01, QAS-SEC-03, QAS-SEC-05 | AST-02, AST-06; BND-05, BND-06 | TOCTOU/revoke/expiry/policy race model and vectors | AA-2 semantic; AA-3 threat/trust/mechanism; AA-5 conformance |
| EI-09 | Shared-Limit Authority Owner | AD-5, AD-8, AD-12 | QAS-SEC-05, QAS-DEC-03 | AST-07; BND-13 | Concurrent branch/partition overspend model | AA-2 semantic; AA-3 trust/mechanism; AA-5 conformance |
| EI-10 | Lifecycle/Effect Owner | AD-6, AD-13 | QAS-COR-01, QAS-COR-02, QAS-COR-03, QAS-RES-04, QAS-PERF-01 | AST-06, AST-08; BND-06, BND-12 | Conflicting/partial/unknown/timeout state corpus | AA-2 / AA-5 |
| EI-11 | Operation/Retry Owner | AD-6, AD-13 | QAS-COR-01, QAS-COR-02, QAS-RES-04 | AST-03, AST-08; BND-03, BND-06 | Lost-response, replay, changed-subject and retention-expiry faults | AA-2 / AA-5 |
| EI-12 | Cancellation/Compensation Owner | AD-6, AD-13 | QAS-COR-02 | AST-06, AST-08; BND-06 | Cancel/commit race, late completion and failed-compensation traces | AA-2 / AA-5 |
| EI-13 | Evidence/Provenance Owner | AD-5, AD-6, AD-8, AD-13 | QAS-PRI-04, QAS-COR-01, QAS-COR-03 | AST-09; BND-12 | Issuer/source/freshness/conflict/redaction/causality vectors | AA-2 semantic; AA-3 threat/trust; AA-5 conformance |
| EI-14 | Semantic Compatibility Owner | AD-2, AD-4, AD-13 | QAS-COR-04, QAS-EVO-02, QAS-IMP-01, QAS-IMP-02 | AST-11, AST-13; BND-04, BND-15 | Differential allowed-set and incompatible-semantics tests | AA-2 / AA-5 |
| EI-15 | Criticality/Evolution Owner | AD-2, AD-4, AD-13 | QAS-SEC-04, QAS-COR-04, QAS-EVO-01, QAS-EVO-02, QAS-IMP-02 | AST-11, AST-13; BND-04, BND-16 | Unknown critical/optional, collision, dependency and downgrade tests | AA-2 / AA-5 |
| EI-16 | Binding/Security Composition Owner | AD-8, AD-9, AD-10, AD-13 | QAS-SEC-02, QAS-COR-04, QAS-RES-02, QAS-EVO-03, QAS-IMP-02, QAS-DEC-02 | AST-02, AST-10, AST-11; BND-09, BND-10 | Coverage, order, mutation, fallback and provider-substitution analysis | AA-4 / AA-5 |
| EI-17 | Bridge Contract Owner | AD-2, AD-8, AD-13 | QAS-SEC-02, QAS-EVO-03 | AST-09, AST-10, AST-11; BND-11 | Directed/versioned loss, drift, loop, impersonation and assurance tests | AA-4 / AA-5 |
| EI-18 | Multi-Party/Epoch Owner | AD-4, AD-5, AD-6, AD-8 | QAS-SEC-01, QAS-SEC-04, QAS-SEC-05, QAS-PRI-02, QAS-RES-04, QAS-EVO-01, QAS-DEC-03 | AST-07, AST-08, AST-11; BND-04, BND-13, BND-14 | Join/leave/split-view/branch-epoch aggregation models | AA-2 semantic; AA-3 threat/trust/mechanism; AA-5 conformance |
| EI-19 | Privacy Architecture Owner | AD-5, AD-8, AD-13 | QAS-PRI-01, QAS-PRI-02, QAS-PRI-03, QAS-PRI-04 | AST-04, AST-05, AST-09; BND-05, BND-12, BND-14 | Per-observer disclosure, metadata/linkability and lifecycle review | AA-3 / AA-5 |
| EI-20 | Resource/Failure Owner | AD-10, AD-12, AD-13 | QAS-PRI-03, QAS-RES-01, QAS-RES-02, QAS-RES-03, QAS-PERF-01, QAS-PERF-02 | AST-12; BND-03, BND-10 | Hostile size/depth/rate, slow consumer, outage and amplification tests | AA-4 / AA-5 |
| EI-21 | Decentralization/Dependency Owner | AD-2, AD-10, AD-12 | QAS-RES-02, QAS-DEC-01, QAS-DEC-02, QAS-DEC-03 | AST-10, AST-13; BND-09, BND-10, BND-13 | Provider outage/replacement and no-purchase Native path | AA-4 design; empirical AA-7 |
| EI-22 | Conformance Independence Lead | AD-13, AD-14, AD-15, AD-22 | QAS-COR-04, QAS-IMP-01, QAS-IMP-02, QAS-IMP-03, QAS-DEC-01, QAS-DEC-02 | AST-13, AST-14; BND-15, BND-16 | Coverage, observer qualification, genealogy and fresh-team interop | AA-6 design; empirical AA-7; rights AA-8 |
| EI-23 | Artifact/Claim Lifecycle Owner | AD-2, AD-13, AD-17 | QAS-SEC-04, QAS-RES-02, QAS-RES-04, QAS-EVO-01, QAS-EVO-03, QAS-DEC-02 | AST-10, AST-11, AST-13, AST-14; BND-04, BND-08, BND-16 | Coexistence, migration, drift, vulnerability invalidation and rerun | AA-5; publication AA-8 |
| EI-24 | Cross-Provider Comparability Owner | AD-4, AD-8, AD-13 | QAS-COR-03, QAS-COR-04, QAS-DEC-02 | AST-04, AST-09, AST-11; BND-07, BND-11, BND-12 | Units/schema/provenance/partial-boundary comparison vectors | AA-5 |
| EI-25 | Evidence/Measurement Boundary Owner | AD-8, AD-12, AD-13, AD-14 | QAS-PRI-01, QAS-PRI-03, QAS-PRI-04, QAS-COR-03, QAS-RES-01, QAS-PERF-02, QAS-IMP-01, QAS-IMP-03 | AST-09, AST-12, AST-13; BND-12, BND-15 | Threat coverage, non-interference, executed-artifact provenance and failing trace | AA-3 / AA-6 |

### 7.4 Safety Blocking Classes

| ID | Accountable owner role | Governing/required AD | Quality scenarios | Preliminary assets / boundaries | Verification | Closure Gate |
| --- | --- | --- | --- | --- | --- | --- |
| SBC-01 | Authority/Security Owner | AD-5, AD-8, AD-13 | QAS-SEC-01, QAS-SEC-02, QAS-SEC-03, QAS-RES-04 | AST-01, AST-02, AST-06; BND-01, BND-02, BND-06 | Unauthorized effect/disclosure, amplification, stale/revoked and confused-deputy corpus | AA-2 normative-model; AA-3 threat/trust/mechanism; AA-5 conformance |
| SBC-02 | Effect Boundary Owner | AD-5, AD-6, AD-8 | QAS-SEC-01, QAS-SEC-03 | AST-02, AST-03, AST-06; BND-05, BND-06 | TOCTOU/revoke/expiry/subject mutation model and fault tests | AA-2 normative-model; AA-3 threat/trust/mechanism; AA-5 conformance |
| SBC-03 | Shared-Limit Safety Owner | AD-5, AD-8, AD-12 | QAS-SEC-05, QAS-DEC-03 | AST-07; BND-13 | Concurrent/partitioned overspend and quorum tests | AA-2 normative-model; AA-3 threat/trust/mechanism; AA-5 conformance |
| SBC-04 | Lifecycle/Effect Safety Owner | AD-6, AD-13 | QAS-COR-01, QAS-COR-02, QAS-COR-03, QAS-RES-04, QAS-PERF-01 | AST-06, AST-08; BND-06, BND-12 | Lost response, duplicate, unknown/partial, cancel and compensation faults | AA-2 / AA-5 |
| SBC-05 | Semantic Integrity Owner | AD-2, AD-8, AD-9, AD-10, AD-13 | QAS-SEC-02, QAS-SEC-04, QAS-COR-04, QAS-RES-02, QAS-RES-04, QAS-EVO-01, QAS-EVO-02, QAS-EVO-03, QAS-IMP-02, QAS-DEC-02 | AST-02, AST-10, AST-11, AST-13; BND-04, BND-10, BND-11 | Unknown mandatory, downgrade, mapping loss, ambiguity and impersonation tests | AA-4 / AA-5 |
| SBC-06 | Privacy/Isolation Owner | AD-8, AD-13, AD-15 | QAS-PRI-01, QAS-PRI-02, QAS-PRI-03, QAS-PRI-04 | AST-04, AST-05, AST-09; BND-05, BND-12, BND-14 | Data-flow, tenant/branch/egress, metadata and retention/deletion tests | AA-3 / AA-5; publication AA-8 |
| SBC-07 | Resource Safety Owner | AD-10, AD-12, AD-13 | QAS-PRI-03, QAS-RES-01, QAS-RES-02, QAS-RES-03, QAS-PERF-01, QAS-PERF-02 | AST-12; BND-03, BND-10 | Size/depth/rate/queue/retry/amplification/deadlock ceilings | AA-4 / AA-5 |
| SBC-08 | Evidence Integrity Owner | AD-8, AD-13, AD-14 | QAS-PRI-04, QAS-COR-01, QAS-COR-03, QAS-IMP-01, QAS-IMP-03 | AST-09, AST-13; BND-12, BND-15 | Provenance/equivocation/observer-independence and immutable-trace audit | AA-2 normative-model; AA-3 threat/trust; AA-5 conformance; AA-6 integrated |
| SBC-09 | Independence/Dependency Owner | AD-2, AD-10, AD-13, AD-14, AD-15, AD-22 | QAS-COR-04, QAS-RES-02, QAS-EVO-03, QAS-IMP-01, QAS-IMP-02, QAS-IMP-03, QAS-DEC-01, QAS-DEC-02, QAS-DEC-03 | AST-10, AST-13, AST-14; BND-09, BND-10, BND-15, BND-16 | Hidden dependency, shared code, private mapping, SBOM and genealogy audit | AA-6 design; empirical AA-7; rights AA-8 |
| SBC-10 | Claim-Boundary Governance Owner | AD-8, AD-13, AD-14, AD-17 | QAS-SEC-04, QAS-PRI-01, QAS-RES-01, QAS-EVO-01, QAS-EVO-03 | AST-11, AST-13, AST-14; BND-11, BND-15, BND-16 | Assumption/dependency completeness, threat coverage and scope-impact review | AA-6 |

The owner role is accountable for maintaining the links and obtaining evidence; it does not replace the independent reviewer or Gate authority. A material change to any linked requirement, AD, QAS, threat assumption or boundary reopens every row found by this reverse index.

## 8. Gate and history rule

The superseded comparative/existential Gate 1, Candidate N/C arms, RTTSI ranking and outcomes `native/profile/upstream/stop` have no authority in this matrix. Their general safety, evidence, independence and reproducibility policies survive only where explicitly adopted by the current PRD and Architecture Assurance Charter. No row may be interpreted as reopening whether AgentBridge Native design begins. A failure causes correction, scope reduction, Profile/Binding allocation, suspension or the Charter's narrowly defined emergency path.

Before AA-1 closes, reviewers must confirm mechanically that every current FR-1–FR-110, retained NFR-1–NFR-29, EI-01–EI-25 and SBC-01–SBC-10 ID is covered exactly once in its normative allocation section, that retained F1–F8 acceptance clauses are preserved, and semantically that no remaining range contains differing allocation or closure treatment.
