# AD-2 — Strict responsibility and dependency allocation

- **Status:** adopted by Project Owner; AA-1 Gate PASS 2026-09-16
- **Revision / canonical digest:** revision 1; canonical file digest is pinned in the final `AA-1-GATE-RECORD.md` and repository commit
- **Date / decision authority:** 2026-09-16 / Project Owner
- **Scope and closure Gate:** Core/Profile/Extension/Binding/Bridge/external-system/implementation/conformance boundaries / AA-1
- **Supersedes / superseded by:** supersedes no architecture ADR; refines AD-1 without changing its conditional hypothesis status
- **Requirements:** FR-1–FR-110; NFR-1–NFR-29; EI-01–EI-25; SBC-01–SBC-10
- **Quality scenarios and threat assumptions:** all AA-1 QAS families, especially provider substitution/failure, downgrade, bridge loss, reference-code divergence, extension collision, bounded resources and independent implementation
- **Binds:** all normative layers, external contracts, SDK/reference implementations, conformance artifacts and dependency closures
- **Prevents:** responsibility overlap, cyclic normative dependencies, hidden central/commercial dependencies, authority inflation, silent semantic loss and specification-by-implementation

## Context

Even with a valid Native Core, independent teams can diverge if the architecture does not assign where specialization, mechanisms, translation, external truth and verification live. A field named similarly in two layers does not guarantee identical meaning, and a successful transport or signed statement does not prove authority or an external effect.

The allocation is therefore a consistency rule, not a claim that the exact future Core content is already known. If later evidence shows a responsibility is not universal or cannot be implemented at its assigned layer, that responsibility must move through an explicit allocation-change ADR.

### Pinned evidence inputs

| Source | Source date | SHA-256 | Role and limitation |
| --- | --- | --- | --- |
| `technical-clean-slate-agentbridge-protocol-archite-2026-09-12/research.md` | evidence current as of 2026-09-12 | `a4004d4ddcd2e11430233b0f8cc00001711a37f2cdda8b6751bd70fbf111baba` | Supports modular Native semantics, reuse of mechanisms and isolated optional Bridges; leaves exact boundaries for architecture validation. |
| `technical-agentbridge-standards-gap-2026-09-11/research.md` | evidence current as of 2026-09-11; decision note updated 2026-09-12 | `e4ab81f7cb4174fea55385f9105a855861ef3cd37f6b1b6d2a236fce2bb42005` | Maps existing standards’ differing responsibility boundaries and warns that cross-standard glue is material; former direction recommendation is superseded. |
| `technical-agentbridge-oq-1-composition-challenger-2026-09-13/research.md` | evidence current as of 2026-09-13 | `fd562fd950db82d8da97cba04dee2d4d20d8a7f08596577105031e008f94496b` | Provides the strongest concrete contrary architecture: deployable compositions with public glue, and exposes their ownership/version/state seams. |

The reports establish live alternatives and known seams, not the correctness of this allocation. Named external technologies are not selected by this ADR.

## Options considered

### A. Strict layered allocation with acyclic dependencies — selected

Core owns candidate universal meaning; Profiles narrow it; Extensions add negotiated behavior; Bindings realize mechanisms; Bridges translate; external systems own external truth/enforcement through contracts; specifications remain normative and conformance remains independent.

- **Strongest advantage:** makes semantic ownership, dependency direction, failure responsibility and claim boundaries reviewable and independently testable.
- **Failure mode:** boundaries can become bureaucratic or artificial, and common behavior can be placed at the wrong layer.

### B. Permanent composition with shared glue layer

Use existing protocols as co-equal semantic owners and standardize only mappings between them.

- **Strongest advantage:** reuses mature protocol ownership and can reduce new normative surface.
- **Failure mode:** multiple owners, version closures and state machines can create cyclic dependencies, split truth and a glue layer that is a hidden mandatory protocol.

### C. Fat Core

Place domain objects, transport, identity, policy and evidence mechanisms in the Core.

- **Strongest advantage:** fewer declared layers and less negotiation in an initial implementation.
- **Failure mode:** universalizes domain/mechanism choices, increases lock-in and makes provider or runtime dependencies normative.

### D. Minimal envelope with plugins

Keep Core as routing metadata and let plugins define behavior.

- **Strongest advantage:** high apparent extensibility and fast local experimentation.
- **Failure mode:** plugins become incompatible private protocols, can weaken safety, and make the reference implementation the hidden source of meaning.

### E. Reference implementation as executable specification

Let one codebase decide ambiguous behavior.

- **Strongest advantage:** immediately concrete and easy to demonstrate.
- **Failure mode:** implementation accidents become normative, clean-room implementation is impaired and one runtime/provider gains hidden control.

## Decision / Rule

Adopt the following responsibility and dependency allocation, subject to later per-element validation:

1. **Core** owns only candidate universal, domain-neutral, independently testable interaction, authority, lifecycle/effect, evidence, evolution and safety semantics.
2. **Profiles** select, narrow and strengthen Core for a named scope; they cannot redefine Core or require one commercial provider.
3. **Extensions** add explicitly negotiated, namespaced, non-redefining behavior with declared dependencies, criticality and unknown handling.
4. **Bindings** preserve and realize selected semantics using replaceable transport, encoding, canonicalization and security mechanisms. Delivery/transport success never proves external effect.
5. **Bridges** are optional, directed, version-pinned translations with explicit provenance, semantic loss and assurance ceilings. Unrepresentable security-critical meaning fails closed for the dependent action.
6. **External Systems** provide identity, discovery, policy, domain truth, effect execution, shared-limit authority or evidence storage through explicit replaceable contracts. Their assertions are scoped inputs, not unconditional protocol truth.
7. **SDK/reference implementations** are non-normative conveniences and cannot supply missing mandatory meaning.
8. **Conformance** is a separate assurance system. Specifications remain normative; models, assertions, vectors and observers reveal divergence but cannot silently create requirements.
9. Normative dependencies are acyclic. Core never depends upward. Every dependency declares its controller, exact version/closure, provides/requires/terminates semantics, trust, availability, compromise, substitution and failure behavior.

### Policy versus hypothesis

- **Owner policy:** no hidden central/commercial dependency; no Bridge or foreign runtime is mandatory for Native conformance; specification primacy; replaceable infrastructure; higher layers cannot weaken the safety floor.
- **Technical hypothesis:** this particular set of layer boundaries is sufficient to prevent incompatible semantic ownership while remaining implementable and evolvable.
- **Not decided:** the concrete content of any layer, number or type of Bindings, transport/encoding/security mechanisms, provider contracts, Bridge targets or implementation stack.

## Verification and acceptance

AA-1 acceptance requires complete requirement allocation and independent review of responsibility, dependency, authority and claim boundaries.

Later acceptance requires:

1. every normative element has exactly one primary semantic owner and an explicit verification/closure route;
2. every Core element passes Universality and Removal Tests; every failed element is reallocated explicitly;
3. mechanical cycle checks over exact normative dependency closures;
4. Profile/Extension negative tests showing no Core redefinition, authority expansion or unsafe unknown handling;
5. Binding tests showing mandatory meaning survives selected mechanisms and resource/security budgets;
6. Bridge mappings that enumerate preserved, transformed and unrepresentable meaning and fail closed on critical loss;
7. provider-removal/substitution and compromise tests demonstrating the declared blast radius and no semantic change;
8. clean-room and differential implementation evidence showing expected outcomes are derivable without reference code or private explanation;
9. conformance-oracle review showing tests implement cited requirements rather than invent them.

No finite set of examples proves the allocation universally correct. It supports only the exact reviewed closure and supplies falsifiers for reallocation.

## Consequences

- More artifacts and explicit contracts are required than in a single-stack implementation.
- Domain and provider-specific behavior moves out of Core even when embedding it would simplify one deployment.
- Translation cost and loss are visible Bridge properties rather than hidden adapter behavior.
- Implementations may vary internally but cannot relocate normative meaning without an allocation-change ADR.
- Conformance tooling must remain traceable to specifications and cannot become the sole oracle.

## Residual risks and conditions

- **Boundary error:** semantics may be assigned to Core that belong in a Profile, or to a Binding that must be universal. Owner: Protocol Architect. Control: Universality/Removal and allocation review.
- **Layer leakage:** SDKs or Bridges may become de facto normative through adoption. Owner: Conformance/Governance. Control: clean-room tests, independent oracle and native demos without Bridges.
- **Dependency fiction:** a nominally optional provider may be operationally mandatory. Owner: Runtime/Dependency Architecture. Control: removal, substitution and outage evidence.
- **Bridge maintenance:** rapid foreign-protocol evolution may invalidate a mapping. Owner: Bridge owner. Control: exact version pin, expiry and material-upstream-change re-open.
- **Excess complexity:** strict layering may raise implementation cost. Owner: Architecture. Control: AA-4 complexity/resource evidence and removal of layers that do not prevent real divergence.

## Change and rollback

Moving responsibility among Core, Profile, Extension, Binding, Bridge or external systems is an allocation change. It requires a new ADR revision, dependency and threat impact, Removal Test, migration analysis and reopening of every affected Gate. A change cannot silently preserve an old conformance claim.

If evidence shows the layer model itself produces cycles, hidden mandatory dependencies or unavoidable semantic duplication, AD-2 reopens for redesign. Rollback means returning to the last frozen allocation closure, not inferring behavior from code.

## Dissent / conflicts / provenance

The strongest dissent is that a permanent composition with a well-specified glue profile may be simpler and better governed than a new layered suite. Another dissent is that strict categories can hide cross-cutting concerns or force artificial separation. OQ-1 C1/C2 remain the required comparison surface for these objections.

The evidence sources are public research artifacts with exact digests above. Owner policy determines independence and anti-capture constraints; the sufficiency of the allocation is a falsifiable technical hypothesis. Final internal reviewer scopes, independence limits, verdicts and canonical digests are recorded in `AA-1-GATE-RECORD.md`; later external claims still require the applicable I2/I3 evidence.
