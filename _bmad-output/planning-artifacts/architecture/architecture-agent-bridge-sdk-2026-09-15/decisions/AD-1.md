# AD-1 — Semantic Hourglass and federated state-machine Core

- **Status:** adopted by Project Owner; AA-1 Gate PASS 2026-09-16
- **Revision / canonical digest:** revision 1; canonical file digest is pinned in the final `AA-1-GATE-RECORD.md` and repository commit
- **Date / decision authority:** 2026-09-16 / Project Owner
- **Scope and closure Gate:** Architecture Baseline v0.x design paradigm / AA-1
- **Supersedes / superseded by:** supersedes no architecture ADR; constrained by the approved Native Architecture-First course decision dated 2026-09-15
- **Requirements:** FR-1–FR-110; NFR-1–NFR-29; EI-01–EI-25; SBC-01–SBC-10
- **Quality scenarios and threat assumptions:** all AA-1 QAS families; untrusted network/input/remote participants; compromised participants, delegates, intermediaries and providers; crash, partition, replay, reordering, partial effect, stale state and resource exhaustion
- **Binds:** Core semantics, normative artifacts, Profiles, Extensions, Bindings, Bridges, SDK/reference implementations and conformance claims
- **Prevents:** a universal domain Core, an empty envelope, transport/vendor/runtime coupling, plugin override of safety semantics and reference-code normativity

## Context

Separately implemented participants need one implementation-independent meaning for authority, action, effect, evidence, failure and evolution. If that meaning is left to Bindings, SDKs, providers or pair-specific adapters, implementations can agree on delivery while disagreeing on whether an action was authorized, what effect occurred or what a receipt proves.

The approved product direction requires a Native AgentBridge path rather than a mandatory composition of foreign agent protocols. That policy does **not** prove that one minimal semantic waist is universally complete. The existence, minimality and cross-topology consistency of that waist remain technical hypotheses to be tested through later Gates.

### Pinned evidence inputs

| Source | Source date | SHA-256 | Role and limitation |
| --- | --- | --- | --- |
| `technical-clean-slate-agentbridge-protocol-archite-2026-09-12/research.md` | evidence current as of 2026-09-12 | `a4004d4ddcd2e11430233b0f8cc00001711a37f2cdda8b6751bd70fbf111baba` | Supports testing a Native semantic Core with optional Bridges; explicitly says the exact minimum Core and its advantage over composition are unproved hypotheses. |
| `technical-agentbridge-standards-gap-2026-09-11/research.md` | evidence current as of 2026-09-11; decision note updated 2026-09-12 | `e4ab81f7cb4174fea55385f9105a855861ef3cd37f6b1b6d2a236fce2bb42005` | Supports a remaining cross-standard action/authority/effect/evidence seam; its former architecture recommendation is superseded and is not treated as authority for this decision. |
| `technical-agentbridge-oq-1-composition-challenger-2026-09-13/research.md` | evidence current as of 2026-09-13 | `fd562fd950db82d8da97cba04dee2d4d20d8a7f08596577105031e008f94496b` | Establishes C1/C2 as strong composition challengers and contrary evidence against assuming a new Core is necessary or superior. |

Dynamic claims about named external protocols retain the refresh/invalidation rules in those reports. These sources justify the hypothesis and comparison surface; they do not prove future interoperability, security, adoption or universal completeness.

## Options considered

### A. Semantic Hourglass Native Core — selected

A small role- and direction-neutral semantic waist owns only the meaning that independent parties must share. Domain specialization and implementation mechanisms remain outside it.

- **Strongest advantage:** one native semantic authority for cross-topology safety and interoperability without making a foreign protocol or provider mandatory.
- **Failure mode:** the waist may be empty, too broad, internally inconsistent across topologies or merely duplicate a composition profile.

### B. Permanent composition/profile over existing protocols — strongest alternative

Use the OQ-1 C1/C2 compositions as the permanent substrate and add public glue/profile rules.

- **Strongest advantage:** reuses mature specifications, implementations and governance.
- **Failure mode:** versioned cross-protocol glue may itself become the mandatory semantic protocol, with multiple state owners and upgrade/failure boundaries. OQ-1 does not establish that the compositions already preserve every retained invariant.

### C. Full native monolithic stack

Own semantic, transport, identity, policy, cryptographic and domain mechanisms in one native suite.

- **Strongest advantage:** local control and fewer semantic translations.
- **Failure mode:** reinvents mature mechanisms, expands security and governance scope, increases adoption cost and creates a fat Core.

### D. No separate Core; upstream or stop

Contribute missing elements to existing initiatives and build no AgentBridge Core.

- **Strongest advantage:** least fragmentation and smallest new standards surface.
- **Failure mode:** no single upstream currently owns the full retained cross-topology seam. The 2026-09-15 course decision removes this as the ordinary project direction, while specific AgentBridge semantics must still be removed or reallocated if evidence shows they add no independent value.

## Decision / Rule

Adopt **Semantic Hourglass Protocol Suite** as the v0.x design paradigm.

1. The Core is the candidate minimal set of role- and direction-neutral, implementation-independent safety and interoperability semantics required for independently built participants to reach the same allowed outcome.
2. End-to-end authority and effect checks remain at the protected disclosure/effect boundary or use a later-proved atomic equivalent. Delivery acknowledgement, intermediary assertion or infrastructure availability cannot replace them.
3. Higher layers may narrow or strengthen Core obligations but cannot weaken, reinterpret or bypass them. Core has no upward runtime dependency on a Profile, Extension, Binding, Bridge, SDK, provider or commercial service.
4. “Capability-secure federated state machines” names a **design target**: explicit, attenuated authority and independently operated state holders with normative transition rules. It is not a present security certification or proof claim.
5. The Core’s universality, completeness and minimality are not established by this ADR. Every proposed Core element must later pass the Universality and Removal Tests, strongest-composition comparison, cross-topology analysis and required independent evidence.

### Policy versus hypothesis

- **Owner policy:** pursue a self-contained Native Architecture-First path; preserve role/direction neutrality, provider independence, safety floors and specification primacy; do not make foreign protocols or commercial infrastructure mandatory.
- **Technical hypothesis:** one coherent minimal semantic waist can preserve the retained invariants across the mandatory topology set better than leaving those semantics to Profiles, glue or external standards.
- **Not decided:** exact Core objects and state machines, trust/cryptographic mechanisms, transport, encoding, runtime, repository structure, production domain or standard status.

## Verification and acceptance

AD-1 may close AA-1 only when its scope, evidence inputs, contrary evidence, owner-policy boundary and downstream falsification duties receive the required protocol/distributed-systems, security/privacy/authority, evolvability/dependency and product/scope reviews.

The technical hypothesis remains conditional until later Gates provide:

1. documentary AA-2 models showing consistent roles, authority, lifecycle, effect, evidence, failure and evolution semantics across every mandatory topology;
2. Universality and Removal Test results for every proposed Core element;
3. an explicit strongest-composition challenger analysis demonstrating each retained Core element is necessary or safer as Core rather than Profile/glue;
4. AA-3 threat/privacy review and zero unresolved applicable SBC/Critical/High blockers;
5. AA-5 assertions and negative/fault vectors for the exact normative closure;
6. AA-7 independent implementations without shared protocol-semantic code or private explanation, with scoped interoperation and divergence evidence.

Passing these checks supports only the exact tested scope; it does not prove universal completeness for all future domains or topologies.

## Consequences

- Protocol meaning remains separable from transport, provider, runtime and foreign protocols.
- Core proposals carry a higher proof burden than Profile or Extension proposals.
- Existing standards remain evidence, reusable mechanisms and optional Bridge targets rather than mandatory Native runtime dependencies.
- AA-2 must test the Core boundary rather than merely elaborate it.
- Some duplication of public glue semantics may be justified only when it creates one independently implementable native contract and survives the comparison tests.

## Residual risks and conditions

- **Empty-waist risk:** the Core may add no necessary semantics beyond existing compositions. Owner: Protocol Architect. Expiry: AA-2/AA-6 allocation closure.
- **Fat-Core risk:** domain or mechanism semantics may be mislabeled universal. Owner: Protocol Architect with independent reviewers. Control: Universality/Removal Tests and Profile allocation.
- **Cross-topology contradiction:** one state model may not preserve the same safety meaning across all required flows. Owner: Formal/Protocol Architecture. Control: topology-indexed models and counterexample review.
- **Composition convergence:** upstream standards may close the identified seam. Owner: Evolution/Governance. Control: source refresh and mandatory re-open on material upstream change.
- **Security overclaim:** the paradigm name may be mistaken for demonstrated security. Owner: Security Architect. Control: scoped claims and prohibition on security claims before applicable Gates.

None of these risks may be accepted as proof that the hypothesis passed; they remain closure obligations at their assigned Gates.

## Change and rollback

Failure of a Core element’s Universality or Removal Test moves that element to a Profile, Extension, Binding or external contract through an allocation-change ADR. Evidence that no coherent necessary waist exists reopens AD-1 for redesign of the specific architecture; the Project Owner’s broader Native project direction is not silently reopened by this record.

Any adopted semantic change creates a new distinguishable revision and reopens affected models, threat analysis, conformance artifacts and Gate evidence. Released identifiers are not reused with changed meaning.

## Dissent / conflicts / provenance

The strongest dissent is that C1/C2-style composition may already provide the needed mechanisms and that AgentBridge glue could be a profile rather than a new Core. A second dissent is that one universal waist may be either too weak for safety or too broad for unrelated domains. Both remain live falsification obligations.

The three pinned reports use public sources and do not incorporate private source documents as evidence. Owner adoption selects product direction and constraints; it does not convert research hypotheses into verified technical facts. Final internal reviewer scopes, independence limits, verdicts and canonical digests are recorded in `AA-1-GATE-RECORD.md`; later external claims still require the applicable I2/I3 evidence.
