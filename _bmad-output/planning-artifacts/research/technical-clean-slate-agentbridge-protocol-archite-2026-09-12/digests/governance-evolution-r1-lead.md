# Digest: protocol architecture, evolution, adoption, and governance

Accessed: 2026-09-12

## Findings

1. **Claim:** A native AgentBridge semantic/processing model can remain layered over existing Internet transports without becoming a composition of existing agent protocols. Internet architectural guidance places end-to-end integrity and security at endpoints, favors modularity, warns against circular dependencies, and recommends reusing an existing solution only when it has successfully solved the same problem and no good technical reason for improvement exists.
   - Source: RFC 1958, “Architectural Principles of the Internet” — https://www.rfc-editor.org/rfc/rfc1958.html
   - Publisher: IAB / RFC Editor
   - Pub date/status: 1996-06, Informational RFC
   - Confidence: high
   - Class: architecture-pattern

2. **Claim:** Making MCP/A2A/UCP or multiple gateways mandatory network elements would add delivery-path coupling; however, a compile-time library, format binding, or optional edge adapter is not automatically an extra network hop. The second sentence is a bounded architectural inference, not a direct RFC statement.
   - Source: RFC 3439, “Some Internet Architectural Guidelines and Philosophy” — https://www.rfc-editor.org/rfc/rfc3439.html
   - Publisher: IETF / RFC Editor
   - Pub date/status: 2002-12, Informational RFC
   - Confidence: high for component-path complexity; medium for the AgentBridge inference
   - Class: architecture-pattern

3. **Claim:** Protocol evolution fails when implementations handle unknown extensions unpredictably; maintaining explicit invariants and actively exercising extension points helps resist ossification, but no mechanism guarantees evolvability.
   - Source: RFC 9170, “Long-Term Viability of Protocol Extension Mechanisms” — https://www.rfc-editor.org/rfc/rfc9170.html
   - Publisher: IAB / RFC Editor
   - Pub date/status: 2022-01, Informational RFC
   - Confidence: high
   - Class: protocol-evolution

4. **Claim:** Liberal acceptance of malformed or ambiguous behavior can entrench bug-for-bug compatibility. New protocols should specify error handling and enforce compliance rather than silently accepting lossy or semantically ambiguous bridges.
   - Source: RFC 9413, “Maintaining Robust Protocols” — https://www.rfc-editor.org/rfc/rfc9413.html
   - Publisher: IETF / RFC Editor
   - Pub date/status: 2023-06, Informational RFC
   - Confidence: high
   - Class: protocol-failure

5. **Claim:** Technical superiority is insufficient for standard adoption. Positive net value, incremental deployability, backward compatibility, open code, unrestricted implementation, an openly available stable specification, and open maintenance processes are documented success factors. A native AgentBridge therefore needs bridges and a narrow early value path for deployment even if those bridges are not architectural dependencies.
   - Source: RFC 5218, “What Makes for a Successful Protocol?” — https://www.rfc-editor.org/rfc/rfc5218.html
   - Publisher: IAB / RFC Editor
   - Pub date/status: 2008-07, Informational RFC based on case studies
   - Confidence: high for the factors; medium for the AgentBridge inference
   - Class: adoption

6. **Claim:** Independent implementations should use different people, organizations, code, and protocol libraries; otherwise apparent interoperability may result from shared code or out-of-band knowledge. An AgentBridge conformance gate should record implementation genealogy and actual feature interoperability.
   - Source: RFC 5657, “Guidance on Interoperation and Implementation Reports” — https://www.rfc-editor.org/rfc/rfc5657.html
   - Publisher: IETF / RFC Editor
   - Pub date/status: 2009-09, Best Current Practice
   - Confidence: high
   - Class: conformance

7. **Claim:** Credible standards governance requires open review, technical competence, rough consensus/running code, explicit contribution/IP rules, and a royalty-free implementation objective. These controls should exist before AgentBridge claims neutral standard status, but they need not block private research and experiments.
   - Sources:
     - IETF “Guide to the IETF standards process” — https://www.ietf.org/process/process/
     - W3C Patent Policy (2025-05-15) — https://www.w3.org/policies/patent-policy/
   - Publishers: IETF; W3C
   - Pub date/status: current guide accessed 2026-09-12; stable policy 2025-05-15
   - Confidence: high
   - Class: governance

## Contrary evidence

- RFC 1958 also advises avoiding duplication when a prior design already solved the same problem, and RFC 5218 says narrowing scope and lowering deployment cost improve success. These are strong arguments against a broad new protocol unless the clean-slate study demonstrates concrete semantic/security gains and an incrementally deployable path.
- RFC 5218 explicitly notes that freely available code can matter more than technical superiority. An architecturally cleaner native protocol could still lose to established ecosystems.
- Optional bridges improve deployability but can ossify into de facto mandatory behavior; they require separate compatibility tests and a rule that the native protocol remains sufficient by itself.

## Leads

- Apply RFC 9170-style invariants and active extension testing to AgentBridge negotiation.
- Use RFC 9413 strictness for security-critical objects and explicit fail-closed bridge behavior.
- Require independent implementation genealogy and deployment evidence, not only a shared SDK conformance pass.
- Treat neutral IP/process design as an adoption deliverable after technical viability, not as proof of viability.

## Not found within this round

- No primary source was found that proves a clean-slate agent protocol will outperform a compositional design in general.
- No primary source establishes a universal optimal governance home for agent protocols.
