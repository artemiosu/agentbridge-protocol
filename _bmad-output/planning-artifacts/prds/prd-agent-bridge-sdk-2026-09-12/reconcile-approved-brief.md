---
title: "Input Reconciliation: Approved Product Brief → AgentBridge PRD"
status: reconciliation
created: 2026-09-13
visibility: private-local
input: "Approved Product Brief and Product Brief Addendum"
target: "PRD: AgentBridge"
---

# Reconciliation: approved Product Brief

## Verdict

The PRD preserves the decisive product direction and materially expands the Gate 1 contract. No distortion was found in the definition of AgentBridge, the native-core hypothesis, required topologies, safety posture, or outcome-neutral decision rule. Three downstream guardrails from the approved Brief/Addendum are not preserved with enough specificity and should be restored before the PRD is marked final.

## Substance captured

| Brief / Addendum substance | PRD evidence | Result |
| --- | --- | --- |
| AgentBridge is an open role- and direction-neutral protocol/SDK ecosystem, not an agent, UI, shopping product, or proprietary aggregator | §§1, 2, 5, 6 | Captured and strengthened |
| Required topology breadth: agent↔service, agent↔agent, service↔service/B2B, reverse async, multi-party/delegated, replaceable infrastructure | §§1–5; FR-1–FR-95; VC-1–VC-12 | Captured with executable acceptance language |
| Native semantic Core is a hypothesis; existing initiatives are evidence, challengers, and optional fail-closed bridges rather than mandatory dependencies | §§1–2; F7; F9; §11 | Captured without prejudging Gate 1 |
| Reuse proven lower-layer transport, crypto, identity, and policy primitives; do not reinvent the full stack | §1.3; F7; NFR-6; §§10–11 | Captured |
| Authority, delegation, consent/approval, lifecycle/effect, evidence, versions, extensions, conformance, and independent implementation | F1–F8; NFRs; success metrics | Captured and substantially elaborated |
| Gate 1 compares Native with the strongest good-faith composition and permits `native`, `profile`, `upstream`, or `stop` | F9; SM-1–SM-14; §11 | Captured and strengthened |
| Rust, wire format, transport, SDK languages, cloud, and repository architecture are hypotheses, not decisions | §§6–7, 10, 13–14 | Captured |
| Market size, adoption dates, pricing, commissions, revenue, valuation, and standard status are unverified | §§6, 8, 13–14 | Captured |
| Open/royalty-free normative basis, self-conformance, no mandatory commercial service or central intermediary | FR-83; F7; NFR-7, NFR-27–28; §§6, 9–10 | Captured |

## Intentionally superseded or deferred

1. **Exact Gate 1 numeric thresholds, calendar/resource budgets, implementer-help limit, and named decision owners.** The Brief/Addendum said the PRD should fix them. The later accepted PRD instead requires a separate detailed, frozen Validation Charter to supply them before any experimental code (§§8, 11.7, 14.1). This is a deliberate sequencing change accepted by the user, not an omission; `No Start` applies while any value is missing.
2. **External-company participation in Gate 1.** The Brief initially called for first external implementers. The later user decision states that company access is unavailable. The PRD therefore tests code/genealogy independence at Gate 1 and explicitly defers organizational independence and adoption evidence to Gates 3–4 (§§5.2, 8.5, 11.6, 13.2). It must not be represented as external adoption.
3. **First-year calendar target.** The Brief’s “several independent organizations in production” remains a qualitative adoption outcome, while the fixed one-year framing is removed because no schedule is evidenced (§§6.2, 8.4–8.5). This is consistent with the later evidence-first decisions.
4. **Production mechanism choices and original repository layout.** These remain deferred until a permitting Gate 1 result and Architecture (§§6.4, 7.2, 14.2). Deferral is correct; the obligation to reconcile the proposed repository layout later is part of Priority Gap 2 below.

## Priority gaps

### P1 — The downstream Gates 2–5 are referenced but not defined

The approved Brief defines distinct gates for cross-domain safety/expressiveness, external independent implementability, adoption/governance, and commercial demand, including what each gate authorizes and what failure requires. The PRD mentions Gates 2–5 only as destinations (§§6.2, 8.5, 13–14). Without a compact downstream gate map, future work could treat a positive technical Gate 1 as permission to skip independent adoption, governance, or demand evidence.

**Required reconciliation:** carry forward a non-implementation appendix/table with each Gate 2–5 purpose, minimum evidence, authorization, and failure action. It must remain explicitly revisable by later dedicated PRDs.

### P2 — The conditional post-Gate-1 product/MVP and Architecture handoff are under-specified

The approved Brief/Addendum preserves the product identity as **protocol plus SDK ecosystem** and conditionally sequences: normative minimal Core draft, at least one native binding, conformance suite/test vectors, one full reference SDK, then a genuinely independent second-language implementation; broader Python/TypeScript/Go SDKs follow contract stabilization. It also requires Architecture to compare every proposed repository element with the selected architecture rather than inherit the original layout. The PRD currently authorizes only Architecture plus a minimal normative draft after `native` (§14.2/DQ-1 and FR-110), so the SDK/conformance milestone and repository-reconciliation obligation could disappear.

**Required reconciliation:** preserve these as a conditional downstream boundary—not as approved stack or Gate 1 work—and explicitly require Architecture to accept/change/defer each inherited repository proposal with rationale.

### P3 — The commercial-neutrality validation discipline is only partially preserved

The PRD correctly forbids a protocol toll, mandatory cloud, closed conformance, and premature monetization. It does not retain the Addendum’s operational test for any paid layer: `buyer → paid problem → capability → value metric → prerequisite → disconfirming evidence`; nor does it state as precisely that a transaction commission is permissible only for a voluntarily selected service that actually processes the transaction, and that marketplace/advertising require a governance firewall and may not influence normative answers, conformance, or user choice.

**Required reconciliation:** add these as deferred Gate 5/governance guardrails, not as current product requirements or validated business models.

## Contradictions and resolution status

- **Native self-contained Core vs composition/profile.** Resolved as an outcome-neutral hypothesis: native is tested first-class, but `profile`, `upstream`, and `stop` remain valid evidence-based outcomes.
- **Broad future protocol vs narrow first validation wedge.** Resolved: consequential agent↔service is the deepest initial slice, while all required topology classes remain in the Gate 1 matrix.
- **Independent implementation vs no company access.** Resolved by distinguishing code/genealogy independence at Gate 1 from organizational independence/adoption at Gates 3–4.
- **Open neutral standard vs centralized monetization.** Core constraints are resolved in favor of open, royalty-free, non-mandatory infrastructure; detailed commercial firewall requirements remain the P3 carry-forward gap.
- **PRD must contain final numeric thresholds vs separate detailed Charter.** Superseded by the later accepted two-artifact sequence, with frozen values required before code.

## Qualitative intent at risk of silent loss

The main risk is not loss of the ambitious native-protocol vision; the PRD preserves that strongly. The risk is losing the disciplined path **after** Gate 1: a protocol-and-SDK ecosystem could be reduced to a research core, or a technically positive result could be mistaken for permission to launch governance, ecosystem, and monetization layers. Closing P1–P3 preserves the original ambition while retaining the later evidence-first safeguards.
