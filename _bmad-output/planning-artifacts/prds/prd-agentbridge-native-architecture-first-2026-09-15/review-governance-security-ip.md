---
title: "Independent Review: Governance, Security, IP and Adoption Controls"
status: final-pass
created: 2026-09-15
updated: 2026-09-15
review_scope: governance-security-ip-adoption
independence: I1-internal-fresh-context
verdict: PASS
findings:
  critical: 0
  high: 0
  medium: 0
  low: 0
---

# Independent Review: Governance, Security, IP and Adoption Controls

## 1. Purpose and scope

This review assessed whether the Native Architecture-First course package preserves the previously frozen governance, security, independence, evidence, rights, publication and anti-capture controls while replacing only the former existential `native/profile/upstream/stop` decision.

The review specifically checked:

- authority boundaries between the Project Owner, Chief Architect, security/privacy reviewers, IP/legal reviewers and independent implementers;
- non-overridability of mandatory security, legal, rights, evidence, independence and conformance blockers;
- retention and change control of frozen OQ policy baselines;
- controls for executable models and disposable prototypes before reference implementation;
- publication, trademark, patent, provenance and external-claim boundaries;
- independence levels for internal review, independent expert review and external adoption;
- prevention of endless analysis without allowing time limits to bypass a blocker;
- prevention of founder, sponsor, vendor, platform, certification or commercial capture of the open standard;
- separation of technical readiness, production safety, adoption, commercial demand and open-standard legitimacy.

## 2. Files reviewed

### Governing course and product documents

- `../../course-decision-native-architecture-first-2026-09-15.md`
- `../../course-correction-native-architecture-first-2026-09-15.md`
- `prd.md`
- `retained-requirements-baseline.md`

### Active Architecture Assurance package

- `../../../specs/spec-agentbridge-architecture-assurance/SPEC.md`
- `../../../specs/spec-agentbridge-architecture-assurance/architecture-assurance-charter.md`
- `../../../specs/spec-agentbridge-architecture-assurance/adopted-policy-baselines.md`
- `../../../specs/spec-agentbridge-architecture-assurance/review-program.md`
- `../../../specs/spec-agentbridge-architecture-assurance/open-decisions.md`
- `../../../specs/spec-agentbridge-architecture-assurance/prototype-control-manifest.md`
- `../../../specs/spec-agentbridge-architecture-assurance/owner-guide.md`

### Superseded but retained source package

- `../prd-agent-bridge-sdk-2026-09-12/prd.md`
- `../../../specs/spec-agentbridge-validation-charter/SPEC.md`
- `../../../specs/spec-agentbridge-validation-charter/validation-charter.md`
- `../../../specs/spec-agentbridge-validation-charter/open-decisions.md`
- `../../../specs/spec-agentbridge-validation-charter/evaluation-ledger.md`
- `../../../specs/spec-agentbridge-validation-charter/safety-blocking-and-risk.md`
- `../../../specs/spec-agentbridge-validation-charter/observers-and-holdout.md`
- `../../../specs/spec-agentbridge-validation-charter/measurement-and-decision.md`
- `../../../specs/spec-agentbridge-validation-charter/roles-evidence-and-governance.md`
- `../../../specs/spec-agentbridge-validation-charter/budget-and-feasibility.md`
- `../../../specs/spec-agentbridge-validation-charter/rights-storage-and-publication.md`
- applicable OQ freeze manifests in the same historical package.

The private foundational source documents were treated only as protected context. Their contents were not copied into this report.

## 3. Method

The review used a targeted adversarial consistency check rather than a general editorial review:

1. Compared the new precedence and supersession rules against the frozen OQ baselines.
2. Traced mandatory blockers through the Course Decision, current PRD, Architecture Assurance SPEC and Charter.
3. Tested whether an ordinary PRD edit, owner decision, review-cycle limit, resource shortage or scope deferral could bypass a mandatory blocker.
4. Compared pre-prototype permissions with the exact rights, storage, egress, dependency, evidence and independence requirements retained from OQ-6A/OQ-8A.
5. Checked the minimum independence level required at internal, publication and adoption stages.
6. Checked publication and commercial boundaries for pay-to-pass, protocol toll, required cloud, privileged namespace/marks/oracle control and sponsor influence.
7. Verified that the superseded documents remain historical and cannot re-enable the former existential Gate 1.
8. Recomputed the retained source and extract SHA-256 digests recorded in `retained-requirements-baseline.md`.

This was an **I1 internal fresh-context review**. It is useful for finding contradictions and verifying document consistency, but it is not a substitute for the I2/I3 external reviews required by the active review programme.

## 4. Initial findings and closure evidence

### H-1 — Precedence could allow frozen policies to be weakened

**Initial issue:** The first draft placed the current PRD and Charter above retained baselines without a sufficiently explicit non-weakening rule. A later ordinary edit could therefore have overridden a frozen security, independence or publication policy.

**Closure:** The active PRD and Architecture Assurance SPEC now state that the Course Decision supersedes only the named existential/comparative rules. Retained baselines are jointly mandatory and cannot be weakened by an ordinary PRD or Charter revision. Any semantic change requires a versioned change record, affected independent reviews and separate conscious acceptance by the Project Owner.

**Status:** Closed.

### H-2 — Executable prototypes could start under undefined “OQ-8B-like” controls

**Initial issue:** The first draft allowed bounded models/prototypes under a general reference to OQ-8B-like controls, without an exact blocking decision or manifest.

**Closure:** AD-21 and `prototype-control-manifest.md` now prohibit every executable model or prototype until a concrete instance freezes exact scope, inputs, direct/transitive dependencies, SBOM/licenses, rights, environment, stores, regions, keys, access, retention, synthetic data, secrets, deny-by-default egress, resource ceilings, observers, incident handling, reproducibility, reviewers, authorization digest and expiry. Any material change returns the instance to `incomplete/no-use`. Documentary architecture work remains permitted.

**Status:** Closed.

### H-3 — Resource shortage could be interpreted as technical project failure

**Initial issue:** The first draft included absence of resources among emergency stop conditions, conflicting with retained OQ-7A, where insufficient resources require pause, re-scope or resource seeking rather than a technical verdict.

**Closure:** The Course Decision, PRD and Charter now consistently state that insufficient resources cause `pause / re-scope / seek resources / archive until funded`. Final project stop requires defined evidence, an applicable I2 recommendation and a separate Project Owner decision. An advisory council cannot stop the project by itself.

**Status:** Closed.

### M-1 — Minimum review independence was not assigned to Gates

**Initial issue:** I1/I2/I3 were described, but the first draft did not state which level was sufficient at each Gate. Internal AI review could therefore have been misread as external independent assurance.

**Closure:** `review-program.md` now defines a Gate-by-Gate independence matrix. I1 permits only internal architecture progression; AA-8 requires named I2 protocol, security/formal, conformance and IP/legal reviewers with exact roster, conflicts, access and signed verdicts; AA-10 requires I3 independently controlled implementations/deployments for adoption claims. Missing I2/I3 blocks the applicable release or claim, not internal documentary work.

**Status:** Closed.

### M-2 — Retained FR/NFR text lived only in a superseded document

**Initial issue:** The new PRD summarized FR-1–FR-95 and NFR-1–NFR-29 while their complete acceptance details remained only in a document marked superseded, creating a risk of downstream omission or silent modification.

**Closure:** `retained-requirements-baseline.md` now records the exact source revision, heading-bounded selectors, snapshot lines, whole-file SHA-256 and extract SHA-256 values. The manifest is an active companion and requires a new version, exact diff, affected independent reviews and conscious Project Owner acceptance for any semantic change.

Digest verification performed during final review:

- historical PRD whole file: `3bd3025053c633d157ee267ad7388420a1c24d7e8c9facc9f6266e57e6c4eb4c` — match;
- FR-1–FR-95 extract: `0bdf52d56f9edee19159434c65ad3b4e0df2033f2ed22ded7d0f3ec7f514be7a` — match;
- NFR-1–NFR-29 extract: `bcf681353c2eb0799b25ccc54750c726b21c0a683cfe1943fad631f9aebca78d` — match.

**Status:** Closed.

## 5. Final assessment

### Authority and blockers

- The Project Owner controls product course, permitted scope, spending, external commitments and acceptance of non-blocking business/residual risks.
- The Project Owner cannot unilaterally convert an SBC, Critical/High, legal, rights, independence, evidence or conformance failure into a pass.
- Mandatory blockers cannot be accepted as residual risk or deferred in order to pass a Gate.
- Review-cycle limits prevent endless analysis but do not create an expiry for mandatory blockers.

### Rights, independence and publication

- OQ-6A and OQ-8A remain active policy constraints in their retained scope.
- Unknown or incompatible rights, licenses, patents or provenance remain `No Use`; failed publication controls remain `No Release`.
- Public release remains subject to exact rights, security/privacy, IP, trademark, provenance, disclosure and governance checks, including the retained multi-party authorization requirements.
- AI/fresh-context councils remain I1 and are not represented as real independent world expertise.
- External publication and adoption claims require the applicable I2/I3 evidence.

### Anti-capture and adoption

- Native interoperability cannot require AgentBridge Cloud, a commercial SDK, broker, registry or other founder-controlled platform.
- Normative Core, mandatory Base Bindings, security/interoperability-critical semantics and conformance remain independently implementable on royalty-free terms.
- Pay-to-pass, protocol toll, privileged sponsor influence, required cloud and advertising influence over normative or safety decisions remain prohibited.
- Production safety, external adoption, commercial demand and open-standard legitimacy are separate Gates.
- Adoption claims require an Adoption Charter and independently controlled implementations/deployments; industry-standard status cannot be self-declared.

### Supersession integrity

- The former Validation Charter is clearly marked historical and retained only for policy/evidence traceability.
- Its old `native/profile/upstream/stop` decision lattice and general Architecture prohibition no longer control the project.
- Its frozen EI/SBC and applicable safety, evidence, independence, rights and publication policies remain enforceable through the active package.

## 6. Final verdict

**PASS**

| Severity | Open findings |
| --- | ---: |
| Critical | 0 |
| High | 0 |
| Medium | 0 |
| Low | 0 |

The reviewed package is internally consistent for its present purpose: it authorizes AA-1 documentary Architecture work, preserves mandatory frozen controls, prevents review-cycle or owner-authority bypass, and does not authorize executable prototypes, reference implementation, publication, production use or external claims beyond their stated Gates.

## 7. Residual downstream Gates — not current findings

The PASS does not pre-approve later work. The following remain mandatory downstream decisions or evidence:

- **AD-21 / before every executable model or prototype:** exact frozen Pre-Prototype Control Manifest and its required reviews/authorization.
- **AA-2/AA-3:** formal/executable verification of authority, lifecycle, effect, retry/replay, cancellation, revocation, concurrency and failure semantics.
- **AA-6:** exact implementation scope, digest, clean-room separation, genealogy/exposure, adjudication and applicable rights/storage/resource controls.
- **AA-7:** real conformance, interoperability, resilience, performance, upgrade and implementation-independence evidence.
- **AA-8:** qualified I2 protocol, security/formal, conformance and IP/legal reviews; exact specification/code/test licenses and patent commitments; trademark/name clearance; publication authorization; neutral governance and protocol/platform anti-capture controls.
- **AA-9:** separate domain- and jurisdiction-specific production safety, privacy, compliance and liability Charter.
- **AA-10:** I3 external implementations/deployments, unassisted integration, migration and retention evidence under an Adoption Charter.
- **AA-11:** functioning neutral change process, continuity/succession and evidence sufficient for any standards-body or industry-standard claim.
- **Business Validation:** buyer/problem/value/willingness-to-pay/retention/kill evidence for each paid layer and a completed Protocol/Platform Firewall.
- **Any spending, contract, brand registration, public disclosure or transfer of rights:** separate Project Owner authorization plus applicable legal, security, privacy and IP controls.

Failure to complete a downstream Gate limits the corresponding action or claim; it does not invalidate the present PASS for AA-1 documentary Architecture work.
