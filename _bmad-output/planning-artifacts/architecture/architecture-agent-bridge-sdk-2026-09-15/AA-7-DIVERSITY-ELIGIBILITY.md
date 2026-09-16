---
title: AgentBridge AD-22 — AA-7 Diversity Eligibility Contract
status: proposed
created: 2026-09-16
updated: 2026-09-16
scope: pre-AA-7 selection of AD-3A validation artifacts
first_blocking_gate: AA-6 exit / before AA-7 artifact selection or implementation
owner: Conformance Independence Lead
---

# AD-22 — AA-7 Diversity Eligibility Contract

- **Status:** proposed; must be adopted and frozen no later than AA-6 exit and before any AA-7 candidate implementation or qualifying run.
- **Binds:** the second experimental Native Binding, two unrelated non-production Domain Profiles, two independently defined Extensions and optional Bridges to at least two distinct foreign protocol models required by AD-3A and retained F6–F8.
- **Prevents:** satisfying the breadth test with aliases, wrappers, forks, shared hidden semantics, one domain split into two labels, Extensions designed as a coordinated pair, or two versions of the same foreign semantic family.
- **Rule:** every proposed candidate receives a deterministic eligibility record against the tests below. Any unmet, unknown or disputed mandatory test makes the candidate ineligible; it cannot be accepted as residual risk for AA-7 selection.

## 1. Common eligibility record

Every candidate record MUST pin artifact kind, stable ID, exact revision/digest, owner/controller, provenance, normative dependencies, shared code/specification ancestry, claimed diversity axes, selector, technical reviewer, conflicts, row-level evidence, expiry and claim scope.

Selectors and authors may supply evidence but may not be the sole eligibility approver for their own candidate. At least one separated Conformance Independence reviewer must reproduce the result. Undisclosed common control, shared semantic code or material authorship conflict invalidates eligibility and all dependent runs.

## 2. Second experimental Native Binding

The experimental Binding and mandatory Base Binding are materially distinct only if all are true:

1. both independently satisfy the same frozen Binding Contract and Core/Profile safety projection;
2. neither is a thin wrapper, gateway configuration or alternate endpoint over the other's normative Binding implementation;
3. they differ on **at least two** reviewed mechanism axes, including at least one of A–C: **A** transport interaction model; **B** encoding and canonical/signable representation; **C** security-mechanism composition; **D** framing/multiplexing/flow control/backpressure; **E** failure/fallback/recovery mapping;
4. the differing axes exercise different downgrade, ambiguity, ordering, resource or recovery risks recorded before implementation; and
5. shared vectors or conformance infrastructure contain no shared protocol-semantic implementation code.

A library swap, version bump, serialization option or adapter that leaves the reviewed risk model unchanged is not a distinct Binding.

## 3. Two unrelated non-production Domain Profiles

The Domain Profiles are unrelated only if all are true:

1. they represent different domain purposes and share neither a domain ontology, system of record nor primary business workflow;
2. their Principal/resource/effect-source sets and at least two authority, privacy, lifecycle, evidence or failure constraints materially differ;
3. one cannot be obtained by renaming entities or tightening numeric limits in the other;
4. neither is a specialization, version, test mode or companion phase of the other; and
5. both exercise the same Core invariants without adding domain meaning to Core.

Commercial ownership labels alone do not establish domain diversity.

## 4. Two independently defined Extensions

The Extensions are independently defined only if all are true:

1. each has a separate normative artifact, stable namespace/controller, requirements, lifecycle and conformance scope;
2. neither requires the other, shares private schema/semantic code with it, or was split from one mechanism solely to satisfy the count;
3. separate accountable authors/controllers define the behavior and disclose common organizational or funding control;
4. each can be implemented, negotiated, ignored/rejected as applicable, versioned and withdrawn independently; and
5. the pair exercises different criticality behavior: at least one safely ignorable additive Extension and at least one mandatory-to-understand security-relevant Extension that fails closed when unknown.

Common review infrastructure is allowed; shared private explanation or protocol-semantic implementation is not.

## 5. Two distinct foreign protocol-model families

Foreign models are distinct only if all are true:

1. they have separately governed normative authorities and independent version lineages;
2. they are not versions, profiles, forks, vendor editions or alternate Bindings of the same family;
3. their primary semantic abstractions differ materially—for example tool invocation, task lifecycle, workflow description, authorization grant, commerce transaction or evidence/transparency;
4. each Bridge has its own directed version-pinned mapping, loss matrix, assurance ceiling and conformance claim; and
5. neither mapping is a pass-through to the other foreign model.

Two models from one semantic family are presumed ineligible unless independently reviewed evidence shows materially different state/authority/effect translation risks.

## 6. Deterministic decision and closure

The Conformance Independence Lead maintains a matrix with one row per mandatory test and only two terminal results: `eligible` or `ineligible`. `Eligible with conditions`, subjective scoring and post-run reclassification are prohibited. Criteria, digests, risk hypotheses and reviewer conflicts are frozen before qualifying implementation evidence is observed.

AD-22 closes only when every selected artifact has a reproducible eligibility record; conflicts and separated verdicts are present; every mandatory test passes; the Architecture Research Lead refreshes `LANDSCAPE-DISPOSITION.md`; and AA-6 records the exact eligible set or a controlled selection procedure with no unresolved ambiguity.

Any material candidate revision, ownership/control change, shared-code discovery, protocol-family reclassification or changed threat assumption reopens eligibility before dependent evidence can count.
