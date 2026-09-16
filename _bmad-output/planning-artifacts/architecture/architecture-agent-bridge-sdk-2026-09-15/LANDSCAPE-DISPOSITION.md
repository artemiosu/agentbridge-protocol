---
title: AgentBridge AA-1 — Landscape Evidence and Disposition Record
status: final
created: 2026-09-16
updated: 2026-09-16
scope: FR-97 AA-1 control for AD-1 through AD-3A
owner: Architecture Research Lead
---

# Landscape Evidence and Disposition Record

## 1. Purpose and evidence pins

This record closes the initial FR-97 control without making any external protocol a Native runtime dependency. It records what AA-1 reuses, rejects as a dependency, defers, or treats as a possible optional Bridge. A disposition is an architecture input, not a claim that the cited landscape remains current indefinitely.

| Evidence | Evidence date | Canonical SHA-256 |
| --- | --- | --- |
| `../../research/technical-clean-slate-agentbridge-protocol-archite-2026-09-12/research.md` | 2026-09-12 | `a4004d4ddcd2e11430233b0f8cc00001711a37f2cdda8b6751bd70fbf111baba` |
| `../../research/technical-agentbridge-standards-gap-2026-09-11/research.md` | researched 2026-09-11; updated 2026-09-12 | `e4ab81f7cb4174fea55385f9105a855861ef3cd37f6b1b6d2a236fce2bb42005` |

The second report's standards map and gap evidence remain inputs; its superseded A2-via-B recommendation is not governing direction. The clean-slate report and the Project Owner's later Native Architecture-First decision govern the current disposition.

## 2. Mechanism disposition

| Mechanism / family | Disposition for Native architecture | Reuse boundary | Defect or risk carried into design | Bridge disposition |
| --- | --- | --- | --- | --- |
| OAuth RAR, Token Exchange and DPoP | **Reuse lower-layer primitives where a selected security Profile proves fit** | Authority material, actor/subject expression and sender constraint; never Core action/effect truth | Application-specific action meaning, incomplete payload binding and confusing authentication with authority | No generic OAuth Bridge; Profile/Binding integration only |
| GNAP | **Reuse/evaluate as replaceable authority provider** | Grant, continuation, key binding and revoke inputs | Resource action/effect remain outside its core; revoke does not undo effect | Optional authority-provider adapter; not Native dependency |
| AuthZEN | **Reuse/evaluate as replaceable policy interface** | External policy decision input | PDP decision is not enforcement or evidence of effect | Optional policy adapter; not Core or mandatory service |
| OpenAPI and Arazzo | **Reuse descriptions under trust and provenance controls** | Capability/action schemas and workflow descriptions | Description is not execution truth; workflow does not supply atomicity, compensation or proof of effect | Optional capability/workflow mapping candidate |
| SCITT and transparency/evidence systems | **Reuse/evaluate as evidence container or store** | Attributed statements, receipts and verifier-selected trust | Registration/signature does not establish external truth | Optional evidence-store adapter; no global ledger requirement |
| Existing transport, encoding and cryptographic standards | **Reuse proven primitives after AA-3/AA-4 review** | Binding mechanisms selected by evidence | Version, canonicalization, downgrade, resource and composition defects require tests | Not a protocol Bridge merely because a Binding uses them |
| MCP | **Reject as mandatory Native substrate** | Evidence source and possible edge translation only | Tool/context boundary does not supply complete authority-to-effect semantics | Deferred optional Bridge candidate after Native semantics stabilize |
| A2A | **Reject as mandatory Native substrate** | Evidence source and possible task/lifecycle translation | Task/identity/cancellation semantics do not establish the complete Native contract | Deferred optional Bridge candidate after Native semantics stabilize |
| UCP | **Reject as mandatory Native substrate or universal Core** | Commerce evidence, reusable patterns and possible domain mapping | Commerce-specific capability/outcome model must not define universal meaning | Deferred optional commerce Bridge/Profile mapping candidate |
| Agentic Commerce Protocol and AP2 | **Reject as mandatory Native substrate or payment truth** | Commerce/payment evidence and possible domain mapping | Beta/domain-specific scope; authorization/payment records do not prove arbitrary external effect | Deferred optional commerce/payment Bridge or Extension mapping candidate |
| x402 and payment rails | **Reject from Core and Native dependency closure** | External payment mechanism only | Payment settlement does not define principal delegation or general action lifecycle | Optional external payment adapter; Bridge only if a mapping is justified |
| Proprietary cloud, broker, registry, commercial SDK or AgentBridge-operated service | **Reject as mandatory dependency** | May exist only behind replaceable explicit contracts | Capture, availability, permission and hidden-paywall risk | Never required for Native conformance or interoperability |
| New AgentBridge cryptographic algorithms, global IdP, policy engine, ledger or domain source of truth | **Reject** | None | Unnecessary security and centralization risk | Not eligible as Bridge rationale |

## 3. Dependency-cut result

**PASS for AD-1 through AD-3A at AA-1.** Removing every foreign agent protocol, AgentBridge-operated online service, proprietary SDK, central registry/broker and optional Bridge leaves a coherent Native path: two independently governed endpoints can implement the normative Core/Profile closure through an open Native Binding and run the independent conformance scope. External identity, discovery, policy, time, effect and evidence services may be required by a particular Profile, but each is behind an explicit replaceable contract and its assertion remains bounded.

This is a documentary architecture result. It does not prove the selected Base Binding, provider substitution or independent interoperability; those close at AA-4 through AA-7.

## 4. Refresh and change control

The **Architecture Research Lead** owns this record. A refresh is mandatory before an affected ADR/Gate when:

- a cited protocol publishes a material normative revision or changes governance/licensing;
- a candidate becomes a proposed Native dependency, Base Binding component or named Bridge target;
- implementation evidence reveals a missing primitive, incompatible assumption or hidden dependency;
- a security advisory, rights/provenance issue or provider policy invalidates a reuse assumption;
- AD-22 selects the AA-7 diversity candidates; or
- more than 90 days have elapsed since the last landscape check at a Gate that relies on it.

Each refresh appends the reviewed revision/digest, changed disposition, affected AD/requirements, dependency-cut impact and reviewer. It cannot silently turn a deferred Bridge or external provider into a Native dependency.
