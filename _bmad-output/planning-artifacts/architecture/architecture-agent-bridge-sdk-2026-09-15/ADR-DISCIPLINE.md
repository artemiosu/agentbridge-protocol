---
title: AgentBridge Architecture Decision Discipline
status: final
created: 2026-09-16
updated: 2026-09-16
scope: AA-1 through AA-11 architecture decisions
---

# Architecture Decision Discipline

## 1. Purpose and authority

An Architecture Decision Record (ADR) fixes a non-obvious choice that independently built parts could otherwise make incompatibly. ADRs do not replace the PRD, frozen EI/SBC baselines, Architecture Assurance Charter or normative protocol specification. Their precedence is:

1. current Project Owner course decision and current PRD;
2. frozen retained requirements, EI/SBC and adopted policy baselines;
3. Architecture Assurance Charter and passed Gate records;
4. adopted ADRs and Architecture Spine;
5. normative specifications/models/conformance artifacts implementing those decisions;
6. SDKs and reference code, which are never normative.

An ADR cannot waive an SBC, Critical/High finding, legal/IP prohibition, mandatory evidence/independence failure or Gate condition. The superseded existential/comparative Gate 1 cannot be reintroduced through an ADR.

## 2. Stable identity and states

- ID form is `AD-N`; assigned IDs are permanent, never renumbered, reused or deleted.
- Each record has an immutable revision, canonical content digest and links to predecessor/successor evidence.
- A released revision is not edited in place. Editorial correction receives an audit note; semantic change creates a new revision or superseding ADR.
- Allowed states: `proposed`, `adopted`, `adopted-with-conditions`, `superseded`, `withdrawn`, `security-suspended`, `deferred`.
- `superseded` preserves history and points to the replacement. `withdrawn` means the decision was never authorized for use. `security-suspended` immediately narrows permission but does not rewrite history.
- Artifact versions, dependency closure and digests referenced by an ADR are exact. Floating references cannot support a Gate pass.

## 3. ADR template

```markdown
# AD-N — Short decision title

- Status:
- Revision / canonical digest:
- Date / decision authority:
- Scope and closure Gate:
- Supersedes / superseded by:
- Requirements: FR / NFR / EI / SBC IDs
- Quality scenarios and threat assumptions:
- Binds:
- Prevents:

## Context
The incompatibility or non-obvious trade-off requiring one shared decision.

## Options considered
Realistic alternatives, strongest advantages, failure modes and evidence.

## Decision / Rule
Normative architectural rule, boundaries, dependency direction and allowed variation.

## Verification and acceptance
Model/assertion/vector/review/benchmark, owner, exact pass condition and evidence links.

## Consequences
Benefits, costs, migration impact, operational burden and claim limitations.

## Residual risks and conditions
Only non-blocking risks: owner, compensating control, expiry and revisit trigger.

## Change and rollback
Compatibility class, migration/rollback path and affected artifacts/Gates.

## Dissent / conflicts / provenance
Reviewers, recusals, unresolved dissent, source and rights status.
```

No field may say only `TBD` at the Gate that the decision blocks. Unknown mandatory meaning closes the Gate rather than being inferred from code.

## 4. Current register

| ID | Decision | Current state | First blocking Gate |
| --- | --- | --- | --- |
| AD-1 | Semantic Hourglass paradigm and non-negotiable principles | adopted by Project Owner | AA-1 |
| AD-2 | Core/Profile/Extension/Binding/Bridge boundaries | adopted by Project Owner | AA-1 |
| AD-3 | Quality scenarios and Architecture Baseline v0.x scope; AD-3A adds the stricter AA-7 cross-Binding/Profile/Extension/Bridge evidence matrix without adding mandatory runtime dependencies | adopted and amended by Project Owner | AA-1 |
| AD-4 | Role/context/interaction semantic model | proposed | AA-2 |
| AD-5 | Authority/delegation/consent/effect/evidence model | proposed | AA-2 / AA-3 |
| AD-6 | Lifecycle/retry/replay/cancel/revoke/compensation machines | proposed | AA-2 |
| AD-7 | Formal/executable method and coverage | proposed | AA-2 |
| AD-8 | Trust/data/enforcement/privacy boundaries | proposed | AA-3 |
| AD-9 | Signing/verifier obligations, then canonical/signable representation and cryptographic profile | proposed; staged across AA-3/AA-4/AA-5/AA-6 | AA-3 |
| AD-10 | Native transport/encoding Base Binding and fallback | proposed | AA-4 |
| AD-11 | Reference runtime/language | proposed hypothesis only; Rust/Go have no priority | AA-4 |
| AD-12 | Aggregate resource ceilings and benchmark protocol | proposed | AA-4 |
| AD-13 | Conformance assertions/vectors/oracle/observer architecture | proposed | AA-5 |
| AD-14 | Spec-only/reference separation and genealogy package | proposed | AA-6 |
| AD-15 | Scoped experimental rights, then permanent public licenses and patent policy | proposed; staged, qualified legal review required for public terms | Before executable work / AA-6 / AA-8 |
| AD-16 | Trademark/name and marks policy | blocked pending clearance; no external brand use | AA-8 |
| AD-17 | Neutral governance and stewardship milestones | proposed | AA-8 / AA-11 |
| AD-18 | First limited-production domain/jurisdiction | deferred | AA-9 |
| AD-19 | Adoption Charter and first external wedge | deferred | AA-10 |
| AD-20 | Business & Ecosystem Strategy / Protocol-Platform Firewall | proposed parallel track | Earliest protected business/public event |
| AD-21 | Pre-Prototype Control Manifest | proposed; blocks every executable model/prototype | Before any executable work |
| AD-22 | Objective eligibility of AA-7 Binding/Profile/Extension/Bridge diversity candidates | proposed; deterministic preregistration required | AA-6 exit / before AA-7 candidate selection or exposure |

The register is an index, not a substitute for individual records or Gate evidence.

### Staged boundary for AD-9

AD-9 is one decision lineage with distinct Gate outputs; an earlier stage cannot silently authorize a later one.

| Stage | Permitted decision | Explicitly not decided yet |
| --- | --- | --- |
| AA-3 | Abstract signing/verifier requirements: which meanings and fields require coverage, mutation/ambiguity prohibitions, freshness and verifier obligations | Concrete canonical representation, algorithm, suite, provider or wire encoding |
| AA-4 | Candidate canonical/signable representations and reviewed cryptographic profiles compared under frozen AD-21 controls where execution is used | Final adoption or a conformance claim |
| AA-5 | Mutation, downgrade, ambiguity, cross-implementation and negative vectors validate the candidate representation/profile | Integrated-baseline permission or public security claim |
| AA-6 | Final representation/profile ADR may be adopted only after scoped AA-3 security re-review and AA-5 conformance checks | Production, publication or standards claim |

### Staged boundary for AD-15

Rights needed to run a controlled experiment are narrower than permanent public licensing and patent commitments.

| Stage | Required rights decision | Claim boundary |
| --- | --- | --- |
| Before any executable model/prototype | The concrete AD-21 instance establishes the scoped right to obtain, store, use, modify and execute every artifact/dependency, with license notices, provenance, SBOM and patent/trademark exclusions for that exact run | No public distribution right, permanent license choice or patent commitment is implied |
| AA-6, before AA-7 implementation | The integrated implementation package establishes rights sufficient to create, test, store and internally share the exact non-production reference/spec-only scope and dependencies | No public release, external contribution intake or production permission |
| AA-8 | Qualified legal/IP review closes permanent public specification, code and conformance licenses, patent commitments, contribution provenance and publication terms | Only the exact approved public artifacts and claims are authorized |

Unknown or disputed rights block the affected execution, implementation or publication at its earliest applicable stage. They cannot be deferred to AA-8 if the artifact must be used earlier.

## 5. Adoption and decision authority

1. Chief Architect drafts the ADR and traceability to requirements, scenarios and alternatives.
2. Applicable independent reviewers issue scoped findings: protocol/distributed systems, security/privacy/identity, formal methods, networking/performance, conformance/DX, governance/IP or cross-discipline red team.
3. The Gate owner verifies evidence and records `pass`, `pass with conditions` or `redesign/block`.
4. Project Owner consciously adopts product direction and may accept only documented non-blocking residual risk after the required expert recommendation.
5. Affected mandatory blockers are not waivable by Project Owner, Chief Architect, sponsor, maintainer or majority vote.
6. Conflicted reviewers disclose and recuse where required. Dissent remains attached to the record and does not itself create an endless cycle.

AI councils are internal analysis. A requirement for external independence is satisfied only by competent, separated reviewers with fixed scope, conflict disclosure and signed verdict.

## 6. Change classification

Every proposed change receives one class before work begins:

- **Editorial:** no change to behavior, guarantee, pass condition or claim. Audit note and digest refresh; no semantic Gate reopening.
- **Compatible semantic addition:** old conformant behavior remains valid and all common invariants hold. New artifact version, affected conformance update and staged-migration review.
- **Breaking semantic change:** changes allowed outcomes, defaults, criticality, authority/effect/evidence, security floor or required dependencies. New distinguishable version; affected Gates and migration review reopen.
- **Security correction:** immediate scoped suspension may occur; classification as fix, erratum or new version follows triage and never silently changes an old released norm.
- **Allocation change:** moves responsibility among Core/Profile/Extension/Binding/Bridge/external systems. Requires Removal Test, dependency/threat impact and all affected Gate reviews.

Parsing compatibility, unchanged syntax or a SemVer label does not prove semantic compatibility.

## 7. Supersession, promotion and removal

### Supersession

- A superseding ADR names every replaced rule and affected artifact/Gate.
- Old IDs, content, evidence, dissent and claims remain discoverable.
- Existing operations remain interpreted under their pinned normative closure unless an explicit safe migration is completed.
- No supersession broadens a prior claim without new evidence.

### Promotion to Core

A Profile/Extension mechanism may enter Core only when all are true:

1. it is domain-neutral and required across the mandatory topology set;
2. divergent implementations would break safety or semantic interoperability;
3. the same result cannot remain safely outside Core;
4. at least two independent implementations and cross-domain evidence support stable meaning;
5. negative/adversarial conformance and migration impact are known;
6. it has no vendor/provider/Bridge dependency and meets open RF rights policy;
7. Architecture, security, conformance and governance reviews pass.

Popularity, sponsorship, installed base, traffic, commercial use or one reference implementation are never sufficient evidence.

### Removal, deprecation and withdrawal

- Released meaning is never erased or identifier-reused.
- Removal from Core requires a new distinguishable Core version, replacement/migration analysis and preservation of historical interpretation.
- Deprecation limits future use but does not alter in-flight or historical operations.
- Security withdrawal may stop new or continuing protected steps when policy requires, but cannot declare that past effects disappeared.
- Binding fallback requires a new explicit negotiation above the security floor.
- A Bridge claim expires and must be revalidated when either pinned side, mapping assumption or threat boundary changes.

## 8. Emergency and security path

1. Record immutable intake, affected versions/digests/claims and available raw evidence.
2. Immediately suspend only the smallest demonstrably affected artifact, claim or operation scope; fail closed where scope is unknown.
3. Preserve evidence and notify authorized maintainers/reviewers under the disclosure policy; no public release is implied.
4. Apply conflict-of-interest recusal and independent classification.
5. Perform root-cause and scope-impact analysis; classify `candidate fix`, `protocol erratum`, `new version`, `observer/harness defect` or `new threat`.
6. Issue a new artifact version/digest and rerun affected vectors, adjacent boundaries and frozen regression corpus.
7. Obtain independent closure and a new Decision Record before restoration.
8. Run post-incident review; never delete the original failure or dissent.

Emergency authority may pause unsafe use. It may not quietly rewrite the norm, grant a vendor exception, convert unknown to pass or weaken an SBC.

## 9. Mandatory re-open triggers

An adopted ADR reopens only for material evidence, including:

- violation or newly discovered conflict with an FR/NFR/EI/SBC;
- formal counterexample or contradictory reachable state;
- two conformant implementations diverge on a security-critical projection;
- implementation requires closed explanation, pair-specific semantic patch or shared protocol code;
- Binding cannot represent mandatory Core meaning or misses its resource/security budget;
- Profile/Extension/Bridge changes Core meaning, expands authority or creates silent downgrade;
- staged upgrade requires a flag day or makes historical/in-flight meaning ambiguous;
- an external service becomes a de facto Native-path dependency;
- vulnerability, primitive/dependency compromise, legal/IP restriction or material upstream change invalidates an assumption;
- evidence provenance, observer completeness, independence or reproducibility fails;
- scope, topology, domain, jurisdiction or production claim expands beyond reviewed evidence.

Ordinary disagreement, preference for a different technology, popularity or lack of funding is not by itself a technical re-open trigger. Resource shortage may pause or re-scope work under the Charter.

## 10. Closure record

Each Gate decision lists adopted ADR revisions/digests, exact artifacts, reviewers/recusals, findings and closure evidence. `Pass with conditions` grants only the named next scope and cannot contain an unresolved mandatory blocker. Any later revision automatically invalidates dependent Gate evidence until its declared impact analysis and reruns complete.
