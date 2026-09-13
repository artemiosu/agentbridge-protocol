---
type: input-reconciliation
input: "Gate 0 technical research: Clean-Slate AgentBridge Protocol"
input_path: "/home/art/projects/agentbridge-protocol/_bmad-output/planning-artifacts/research/technical-clean-slate-agentbridge-protocol-archite-2026-09-12/research.md"
target_path: "/home/art/projects/agentbridge-protocol/_bmad-output/planning-artifacts/prds/prd-agent-bridge-sdk-2026-09-12/prd.md"
reviewed: 2026-09-13
verdict: "substantially reconciled; four bounded carry-forward gaps"
---

# Reconciliation: Gate 0 technical research → AgentBridge PRD

## Scope and verdict

The Gate 0 research was compared with the current PRD. No `addendum.md` exists in the PRD workspace, so there was no PRD addendum to reconcile.

The PRD substantially preserves the evidence-qualified Gate 0 conclusion without treating it as a proven product fact: native semantic Core plus optional fail-closed bridges remains a candidate to falsify; the Strongest Composition Challenger receives parity; `profile`, `upstream`, and `stop` remain valid outcomes; production specification, architecture, SDK, wire format, transport, and language choices remain prohibited before Gate 1. The PRD also preserves role/topology neutrality, thin-waist and native-path tests, fail-closed bridge semantics, evidence limits, independent implementation, adversarial conformance, reproducible benchmarking, and the distinction between technical validation and market adoption.

No research hypothesis was found to have been silently converted into an unconditional production decision. The detailed FR/NFR set is consistently scoped as the contract to test the candidates, not as an already validated production protocol.

## Priority gaps

### G1 — A commerce bridge is no longer an explicit Gate 1 obligation (high)

- **Gate 0 evidence/decision:** Research §10 explicitly requires one non-commerce profile and one commerce bridge; §2 and §9 identify current UCP/AP2/commerce work as the strongest concrete objection to a separate AgentBridge Core.
- **PRD state:** §7.1 and VC-11 require mappings to at least two external protocol models, while FR-100 covers a commerce domain. Neither requires one of the tested bridges to be commerce-oriented. The requirement could therefore be satisfied with, for example, MCP and A2A mappings only.
- **Required disposition:** The detailed Validation Charter should require at least one current commerce-oriented bridge/mapping (candidate selected after refresh from UCP, commerce ACP, AP2, or a stronger successor), or record an evidence-based reason why another candidate supersedes that test. This does not preselect the Core outcome.

### G2 — Source confidence and the exact staleness map are only implicit (high before Charter freeze)

- **Gate 0 evidence/decision:** The fresh-context red team reduced confidence in the native-Core hypothesis to **medium**. Research §12 sets a 2026-10-12 refresh deadline for the fast-moving MCP/A2A/OpenAPI/Arazzo/UCP/ACP/AP2 and issue-tracker surface, and separately tracks agentproto, AIPF, agentic overlay, and DAWN.
- **PRD state:** FR-97, R-13, OQ-1, and the freeze step correctly require an updated landscape, but do not retain the current confidence label, the dated baseline, or the complete emerging-initiative watchlist.
- **Required disposition:** The detailed Charter should cite the Gate 0 baseline as medium-confidence and source-dated, explicitly inventory both mature and emerging overlap candidates, and perform a fresh review before freeze whenever the applicable research deadline has passed. An unchanged baseline must be recorded as a checked result, not assumed.

### G3 — Known conditional transport/encoding hazards need explicit disposition in the detailed Charter (high if the relevant binding is selected)

- **Gate 0 evidence/decision:** Research §§5–6 carries specific conditional hazards and benchmark cells: no consequential effect through replayable QUIC 0-RTT; TCP fallback and blocked-UDP behavior for HTTP/3; bounded decompression and incremental parsing; flow-control deadlock; canonicalization/numeric/Unicode ambiguity; p99.9 and recovery/resource measurements.
- **PRD state:** NFR-3, NFR-6, NFR-9, NFR-12–NFR-14, FR-105, VC-9, VC-12, and OQ-5 preserve the general safety and benchmark intent, appropriately without selecting a transport or encoding. They do not require the Charter to disposition every applicable known-hazard cell from Gate 0, and FR-105 names p50/p95/p99 but not p99.9.
- **Required disposition:** When OQ-5 is resolved, the Charter should use the Gate 0 benchmark/threat matrix as a mandatory input and mark each conditional cell `included`, `not applicable`, or `excluded with rationale`. This is a Charter carry-forward, not grounds to fix HTTP/3, QUIC, JSON, CBOR, Rust, or Go in the PRD.

### G4 — The later “industry standard” claim threshold lost one concrete research criterion (medium; deferred beyond Gate 1)

- **Gate 0 evidence/decision:** Research §7 says an industry-standard claim requires at least two independently developed client implementations and two service implementations, public results, neutral change control, and royalty-free IP rules.
- **PRD state:** §10.4 correctly prohibits premature claims; NFR-27 and DQ-2 preserve open/RF and governance concerns; A-7, R-8, R-16, §8.5, and DQ-8 correctly defer organizational independence and adoption. The exact `2 client + 2 service` external implementation threshold is not carried forward.
- **Required disposition:** Preserve this as a candidate Gate 3/4 adoption/governance criterion (subject to later research), explicitly distinct from Gate 1's code/genealogy independence. It must not be used to imply company participation or industry acceptance during Gate 1.

## Reconciled without gap

- The comparative landscape is used as a capability-boundary map, not as evidence that existing standards are defective or insecure.
- Issue-tracker observations are not repeated as adjudicated vulnerabilities.
- The clean-slate rationale is correctly limited to an independent application/agent semantic layer; lower-layer transports, cryptography, identity, policy, receipts, and domain systems remain reusable or replaceable.
- The weighted Gate 0 score is not promoted into a requirement; the PRD replaces it with preregistered, outcome-neutral evidence gates.
- Rust, Go, encodings, transports, SDK languages, cloud, registry, payment rail, and domain ontology remain unselected.
- Security-critical ambiguity, authority expansion, false effect certainty, evidence overclaim, and bridge semantic loss remain hard-fail conditions.
- The absence of company access is transparently handled as a Gate 1 evidence limitation rather than misrepresented as organizational independence or adoption.

