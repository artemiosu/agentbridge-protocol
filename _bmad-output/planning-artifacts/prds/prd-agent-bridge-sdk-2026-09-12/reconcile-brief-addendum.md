---
title: "Input Reconciliation: Product Brief Addendum → AgentBridge PRD"
status: reconciliation
created: 2026-09-13
visibility: private-local
input: "Approved Product Brief Addendum and architecture handoff"
target: "PRD: AgentBridge"
---

# Reconciliation: Product Brief Addendum

## Verdict

The PRD preserves and materially strengthens the Addendum’s decisive product direction: AgentBridge is a role- and direction-neutral protocol plus future SDK ecosystem; a self-contained native semantic Core is the primary falsifiable hypothesis; external agent protocols are comparison material and optional fail-closed bridges; and no language, wire format, central service, or commercial layer is preselected. Later user decisions are reflected correctly, especially the outcome-neutral Gate 1 and the replacement of company participation with code/genealogy independence at this stage.

No PRD-workspace `addendum.md` exists at reconciliation time. Three downstream obligations from the Brief Addendum are not preserved with enough specificity in `prd.md` and should be carried forward before final status.

## Substance captured

| Addendum substance | PRD evidence | Assessment |
| --- | --- | --- |
| Native AgentBridge Core is a hypothesis, not a predetermined winner | §§1–2, F9, §8, §11 | Captured and strengthened by explicit `native/profile/upstream/stop` outcomes |
| Required role/direction-neutral topology breadth | §1; F1–F9; SM-4; VC-1–VC-12 | Captured with executable coverage requirements |
| MCP, A2A, OpenAPI/Arazzo, UCP, ACP, AP2, and related work are challengers/bridges rather than mandatory Core dependencies | §1.3; F7; F9; §10.3; §11 | Captured |
| Native Path has no mandatory cloud, broker, registry, gateway, bridge, or provider | FR-68–FR-81; NFR-7, NFR-28; H-4 | Captured |
| Reuse proven foundational primitives without reinventing transport, cryptography, identity providers, policy engines, payment rails, or systems of record | §1.3; F7 exclusions; NFR-6; §10.3 | Captured |
| Bridges must expose mapping limits and fail closed on security-critical semantic loss | FR-75–FR-81; SM-10, SM-12 | Captured and elaborated |
| Authority, consent/approval, lifecycle/effect, evidence, versioning, conformance, and independent implementability are first-class | F4–F8; NFRs; SM-1–SM-14 | Captured and elaborated |
| Rust, formats, transports, SDK languages, cloud, and repository architecture remain hypotheses | §§6–7, 10.3, 13–14 | Captured |
| Market, adoption, pricing, commission, revenue, valuation, fundraising, and standards-status numbers are unverified | §§6, 8, 13–14 | Captured |
| Open/royalty-free normative basis, runnable self-conformance, and no mandatory paid interoperability service | FR-83, FR-95; NFR-27–NFR-28; §6.3 | Captured |
| Private sources and unapproved results must not be published | §7.3; SM-14; §10.4; R-12 | Captured and strengthened |

## Intentionally superseded or deferred

1. **External implementers in Gate 1.** The Addendum named first external implementers as a PRD input, but the later user decision states that company access is unavailable. The PRD correctly requires two independently authored implementations and genealogy disclosure for Gate 1, while organizational independence and adoption evidence are deferred to Gates 3–4 (§§5.4, 8.5, 11.6, 13.2). This must never be presented as external adoption.
2. **Exact numeric thresholds, time/resource ceilings, assistance limits, and named owners.** The Addendum asked the PRD to determine them. The accepted PRD instead makes them mandatory frozen fields of a separate detailed Validation Charter (§§11.7, 14.1), with `No Start` while any field is unresolved. This is an explicit sequencing decision, not an omission.
3. **Production mechanism and stack choices.** The Architecture handoff’s wire formats, transports, trust mechanisms, SDK strategy, and repository structure remain deferred until a permitting Gate 1 result (§6.4, §7.2, DQ-1). This is correct; the future reconciliation obligation is addressed in Priority Gap 1.
4. **Commercial and growth sequence.** Open specification/SDK → adoption → hosted/transaction/enterprise layers → institutionalization remains an unvalidated strategic option. The PRD correctly keeps market, governance, and monetization work outside Gate 1 (§§6.2–6.4, 8.5, DQ-2–DQ-8).

## Priority gaps

### P1 — Preserve the conditional Architecture/SDK handoff

The PRD defers production architecture correctly, but DQ-1 does not retain the Addendum’s concrete handoff discipline: Architecture must define the layered/thin-waist boundary, role and topology matrices, semantic lifecycle, trust model, version/binding strategy, and SDK independence plan using Gate 1 evidence. It must also examine every inherited repository proposal and explicitly **accept, change, or defer** it with rationale. The conditional sequence—minimal normative Core and native binding, conformance artifacts, one full reference SDK, then a genuinely independent implementation before broad multi-language SDK expansion—could otherwise disappear.

**Reconciliation action:** carry this material into a private PRD addendum/downstream gate map as conditional post-Gate-1 guidance, not as approved architecture, stack, or Gate 1 scope.

### P2 — Restore the precise commercial-neutrality firewall

FR-83/FR-95 and NFR-27/NFR-28 protect open self-conformance and optional commercial services, but the Addendum’s sharper downstream rules are diluted: paid audit must not be pay-to-pass and should permit multiple independent auditors; transaction commission is acceptable only for a voluntarily chosen service that actually processes the transaction; marketplace and advertising require a governance firewall and must not influence normative protocol answers, conformance verdicts, or user offer selection.

**Reconciliation action:** preserve these as Gate 5/governance guardrails. They are not validated revenue models and do not authorize implementation.

### P3 — Preserve the evidence ladder for future adoption and monetization

The PRD distinguishes technical evidence from adoption and willingness-to-pay in §§5–6 and §8.5, but it does not retain the Addendum’s operational four-level glossary—technical, adoption, demand, and revenue evidence—or the required paid-layer card `buyer → paid problem → capability → value metric → prerequisite → disconfirming evidence`. Without this handoff, future GitHub activity or protocol traffic could be mistaken for demand or revenue evidence.

**Reconciliation action:** place the evidence ladder and paid-layer card in the downstream gate map or future GTM/commercial PRD; do not turn them into Gate 1 metrics.

## Contradictions and resolution status

- **Native Core vs reuse/composition:** resolved. AgentBridge owns the candidate semantic Core while reusing audited foundational primitives via bindings; composition remains a fair challenger and may win.
- **Broad universal direction vs narrow first slice:** resolved. Consequential agent↔service is the deepest initial wedge, while all required topology classes remain in Gate 1 coverage.
- **Self-contained protocol vs full-stack reinvention:** resolved. Self-contained means no mandatory foreign agent runtime, not replacement of transport, cryptography, identity, policy, or domain systems.
- **Independent implementation vs no company access:** resolved by later user authority as technical/genealogy independence now and external organizational/adoption evidence later.
- **Open neutral infrastructure vs centralized monetization:** resolved for the Core by NFR-27–NFR-28 and permanent non-goals; the detailed firewall remains P2.
- **Fast/multi-language delivery vs safety and evidence:** resolved in favor of validation gates and deferred production SDK choices.

## Valuable rationale/options at risk of silent loss

- AgentBridge is intended to become a **protocol-and-SDK ecosystem**, not remain only a research Core. A positive Gate 1 must still authorize only the next bounded design stage.
- Technical conformance, organizational adoption, deliberate demand, and paid renewal are different evidence classes; none substitutes for the next.
- The original repository layout and technology list are inputs for comparison, not defaults. Future Architecture must explain every retained or rejected element.
- Governance and commercial services must earn trust without taxing, ranking, or controlling basic interoperability.
