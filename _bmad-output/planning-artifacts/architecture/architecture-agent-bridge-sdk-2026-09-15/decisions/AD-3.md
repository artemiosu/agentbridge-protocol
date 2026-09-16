# AD-3 — Horizontally complete, vertically bounded Architecture Baseline v0.x

- **Status:** adopted and amended by Project Owner; AA-1 Gate PASS 2026-09-16
- **Revision / canonical digest:** revision 1 including AD-3A; canonical file digest is pinned in the final `AA-1-GATE-RECORD.md` and repository commit
- **Date / decision authority:** AD-3 adopted 2026-09-16; AD-3A adopted 2026-09-16 / Project Owner
- **Scope and closure Gate:** Architecture Baseline v0.x scope, quality-scenario coverage and AA-6/AA-7 claim boundary / AA-1
- **Supersedes / superseded by:** AD-3A amends AD-3 validation breadth without enlarging the mandatory Native runtime waist
- **Requirements:** FR-1–FR-110; NFR-1–NFR-29; retained F1–F8 acceptance clauses; EI-01–EI-25; SBC-01–SBC-10
- **Quality scenarios and threat assumptions:** all QAS in `QUALITY-ATTRIBUTE-SCENARIOS.md`; all mandatory topology families; finite AA-7 validation cannot establish universal future-domain completeness
- **Binds:** Baseline v0.x contents, deferred decisions, QAS traceability, AA-6 readiness claim and AA-7 implementation/interop evidence matrix
- **Prevents:** irreversible Core lock-in from one happy path, unbounded ecosystem scope, premature stack selection and production/performance/adoption/standard-status claims

## Context

The architecture must be broad enough to expose contradictions in role-neutral semantics before reference code hardens them, but bounded enough to finish and falsify. A narrow agent-to-service happy path could hide incompatibility in reverse, delegated, multi-party or service-to-service flows. Attempting to build every domain, provider, Binding and Bridge would make v0.x unfinishable and convert assumptions into accidental standards.

The chosen scope is therefore **horizontal coverage as a design obligation** and **vertical limitation as a delivery boundary**. “Horizontally complete” names the required abstract coverage target for the frozen topology/invariant set. It is not a claim that universal completeness has already been proved.

### Pinned evidence inputs

| Source | Source date | SHA-256 | Role and limitation |
| --- | --- | --- | --- |
| `technical-clean-slate-agentbridge-protocol-archite-2026-09-12/research.md` | evidence current as of 2026-09-12 | `a4004d4ddcd2e11430233b0f8cc00001711a37f2cdda8b6751bd70fbf111baba` | Supports validation across role-neutral topologies, optional Bridges, independent implementations and measured technology choices; warns that the minimum Core and advantage are unproved. |
| `technical-agentbridge-standards-gap-2026-09-11/research.md` | evidence current as of 2026-09-11; decision note updated 2026-09-12 | `e4ab81f7cb4174fea55385f9105a855861ef3cd37f6b1b6d2a236fce2bb42005` | Supplies the standards landscape and dynamic-claim freshness map; its superseded direction recommendation does not determine v0.x scope. |
| `technical-agentbridge-oq-1-composition-challenger-2026-09-13/research.md` | evidence current as of 2026-09-13 | `fd562fd950db82d8da97cba04dee2d4d20d8a7f08596577105031e008f94496b` | Supplies concrete composition challengers and gaps that the baseline must test honestly; does not select AgentBridge architecture or prove Native superiority. |

The exact counts added by AD-3A are retained validation policy adopted by the Project Owner. They are coverage/falsification requirements, not research-proven minimum sample sizes and not universal proof.

## Options considered

### A. Narrow consequential-action slice only

Specify and implement one high-risk agent-to-service path before broader abstract modeling.

- **Strongest advantage:** fastest path to executable feedback.
- **Failure mode:** hardens roles, state and mechanism assumptions that fail in reverse, delegated, multi-party, agent-agent or service-service flows.

### B. Horizontally complete, vertically bounded baseline — selected

Model all retained topology families and safety semantics abstractly, while limiting concrete mechanisms, domains and implementation claims.

- **Strongest advantage:** exposes semantic contradictions before code without attempting a whole ecosystem.
- **Failure mode:** abstract breadth may still be too large, or may appear complete without sufficient empirical diversity.

### C. Full ecosystem v0.x

Include multiple production domains, transports, SDKs, providers, Bridges, cloud infrastructure and deployment architecture.

- **Strongest advantage:** demonstrates an end-to-end product surface.
- **Failure mode:** unbounded scope, premature technology lock-in, weak evidence per component and conflation of protocol with commercial platform.

### D. Specification-only baseline with one implementation

Finish the abstract design and validate it through one reference implementation.

- **Strongest advantage:** lower cost and simpler coordination.
- **Failure mode:** reference behavior becomes the hidden standard and transport/domain assumptions remain invisible.

### E. Composition-first baseline

Use OQ-1 C1/C2 as the baseline and defer a Native path.

- **Strongest advantage:** maximum immediate reuse and strong contrary control.
- **Failure mode:** violates the adopted Native Architecture-First direction and may hide glue/state ownership as external dependencies. C1/C2 remain comparison evidence rather than mandatory runtime substrates.

## Decision / Rule

Adopt a **horizontally complete target, vertically bounded Architecture Baseline v0.x**:

1. The abstract design must address agent-service, agent-agent, service-service/B2B, reverse-asynchronous, delegated and multi-party interaction using one consistent role/context/safety vocabulary.
2. Baseline v0.x targets the full retained abstract semantics for interaction, authority, lifecycle/effect, evidence, failure, evolution and bounded resources. Each proposed Core element remains conditional on AD-1/AD-2 validation; this ADR does not declare the universal Core proved.
3. By AA-6 the baseline designates:
   - one complete open, provider-neutral mandatory Native Base Binding selected at AA-4 from preregistered requirements, threat analysis, conformance prototypes and measurements;
   - two domain-neutral verification Profiles: Information Exchange and Consequential Action;
   - an additive Extension model;
   - an optional fail-closed Bridge contract;
   - implementation-independent conformance architecture.
4. AA-1 fixes falsifiable scenarios and zero-tolerance safety outcomes. Evidence-dependent workload, resource and performance values are frozen only at their assigned Gate.
5. AA-6 may claim only **Architecture Ready for Controlled Non-Production Implementation** for the exact frozen closure. It does not establish independent interoperability, production performance, production safety, adoption or standard status.
6. AA-7 may claim only the exact scoped implementation/interoperability results demonstrated by its frozen evidence matrix. It **tests and may falsify** retained breadth; it does not prove universal breadth across future domains, topologies, adversaries or protocol revisions.

### AD-3A — Adopted amendment: stricter AA-7 empirical matrix

Without enlarging the mandatory Native runtime waist, AA-7 must additionally:

1. implement a second distinct experimental Native Binding to test transport/mechanism independence;
2. instantiate two unrelated non-production Domain Profiles in addition to the two domain-neutral verification Profiles;
3. exercise two independently defined Extensions;
4. test optional directed Bridge mappings to at least two distinct foreign protocol models with explicit loss, provenance and assurance ceilings.

The second Binding, Domain Profiles, Extensions and Bridges are validation artifacts. They do not become Core, mandatory runtime dependencies, production commitments, adoption evidence or preferred commercial integrations merely by passing their tests.

### Policy versus hypothesis

- **Owner policy:** Native Architecture-First; broad role/direction-neutral scope; the retained AA-7 matrix in AD-3A; no premature stack, production, adoption or standards claims.
- **Technical hypothesis:** one bounded abstract baseline can remain coherent across the frozen topology set and can be implemented independently through more than one Binding and unrelated Profile/Extension/Bridge examples.
- **Not decided:** exact state machines, transport, encoding, canonical form, cryptographic suite, runtime/language, repository layout, production domain/jurisdiction/SLO, concrete Bridge products or market wedge.

## Verification and acceptance

AA-1 acceptance requires complete QAS/requirement/allocation traceability, clear deferred-decision owners and reviewers’ confirmation that the scope is both coherent and honestly bounded.

Subsequent acceptance requires:

1. AA-2 models and proof obligations cover every mandatory topology and distinguish delivery, acceptance, authorization, execution, effect and observation;
2. AA-3 threat/privacy/trust analysis closes all applicable mandatory blockers;
3. AA-4 chooses the Base Binding and resource budgets from frozen candidate criteria and reproducible evidence, with a re-open path if no single candidate can carry mandatory meaning;
4. AA-5 defines traceable assertions, negative/fault vectors, qualified observers and scoped verdicts;
5. AA-6 freezes an internally consistent exact artifact/dependency closure and authorizes only the named non-production implementation scope;
6. AA-7 runs the AD-3A matrix with two independently developed implementation lineages, no shared protocol-semantic code or private explanation, and publishes exact environments, versions, genealogy and raw evidence internally;
7. every AA-7 result is expressed as scoped support, contradiction or inconclusive evidence—never as universal proof.

Failure in one topology, Profile, Binding, Extension or Bridge mapping reopens the affected semantics/allocation and may narrow or redesign the baseline. It cannot be averaged away by passes elsewhere.

## Consequences

- Architecture work precedes reference implementation and must make contradictions visible across the entire retained topology set.
- Concrete stack and mechanism decisions remain delayed until their evidence Gates.
- AA-7 costs more because it requires independent lineages and deliberately diverse validation artifacts.
- The mandatory Native runtime can remain small even though the validation surface is broad.
- A finite validation matrix improves falsification power but limits claims to the tested closure.

## Residual risks and conditions

- **Abstract overreach:** the baseline may be too broad to specify coherently. Owner: Chief Architect. Control: staged models, explicit non-goals and reallocation/narrowing on contradiction.
- **False breadth confidence:** finite examples may be mistaken for universal proof. Owner: Gate owner. Control: scoped claim language and exact evidence closure.
- **Single-Base-Binding premise:** no candidate may satisfy all mandatory environments and meanings. Owner: AA-4 Binding Architect. Control: preregistered criteria and mandatory AD-3 re-open rather than forced selection.
- **Validation representativeness:** two Profiles/Extensions/Bridges may not sample the hardest cases. Owner: Conformance Lead. Control: unrelated selection rationale, adversarial review and explicit limits.
- **Cost/schedule pressure:** breadth may tempt early code or reduced independence. Owner: Gate owner. Control: AD-21 and Gate permissions; resource shortage causes pause/re-scope, not evidence inflation.
- **Upstream change:** Bridge targets and composition challengers may materially change before AA-7. Owner: Evolution/Bridge owners. Control: fresh official version verification and re-freeze before use.

## Change and rollback

AD-3 scope changes require a versioned amendment naming affected requirements, QAS, artifacts and Gates. AD-3A remains part of this decision lineage; it is not a separate route to mandatory runtime expansion.

If no single Base Binding satisfies frozen mandatory requirements, or if the horizontal model is contradictory, AD-3 reopens before AA-6. Permitted outcomes include reallocation, narrower claim scope, multiple mandatory Bindings with explicit negotiation, or baseline redesign; selecting a failing candidate to preserve the current wording is forbidden.

Rollback returns to the last frozen scope and invalidates dependent readiness/interoperability claims. Released semantics and evidence remain preserved with their original scope.

## Dissent / conflicts / provenance

The strongest dissent is that the scope remains too broad before executable evidence and may optimize for theoretical completeness. The opposite dissent is that two Bindings, two Domain Profiles, two Extensions and two Bridge mappings are insufficient to demonstrate breadth. This ADR accepts both concerns by treating the matrix as bounded falsification evidence, not proof of universal completeness.

A further contrary position is that OQ-1 C1/C2 compositions should be the baseline. They remain required comparison evidence but not mandatory Native dependencies under the 2026-09-15 course decision.

The pinned research artifacts use public sources and do not import private documents as evidence. The exact AD-3A counts are an Owner-adopted validation policy. Final internal reviewer scopes, independence limits, verdicts and canonical digests are recorded in `AA-1-GATE-RECORD.md`; later external claims still require the applicable I2/I3 evidence.
