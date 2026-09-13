# Digest: Gate 0 red team

Accessed: 2026-09-12. Fresh-context review; public primary sources only.

## Adversarial verdict

The native-core conclusion is supportable only as a **conditional hypothesis**, not as authority to publish a new normative wire protocol. GNAP, A2A, MCP, OpenAPI and current UCP already cover substantial surface. The next experiment must first attempt the strongest narrow safety-profile composition and prove precisely which mandatory semantics cannot be expressed safely. Permanent composition has not been empirically defeated.

## Strongest counterevidence

1. GNAP access rights, A2A task semantics, MCP self-describing requests/extensions and OpenAPI operations collectively cover much of discovery, invocation, grants and asynchronous delivery. A new core can easily duplicate them.
   - Sources: RFC 9635 — https://www.rfc-editor.org/rfc/rfc9635.html ; A2A 1.0 announcement — https://a2a-protocol.org/latest/announcing-1.0/ ; A2A releases — https://github.com/a2aproject/A2A/releases ; MCP release — https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/blog/content/posts/2026-07-28-spec-ga/index.md ; OpenAPI 3.2 — https://spec.openapis.org/oas/v3.2.0.html

2. UCP 2026-08-25 is a concrete semantic-layer-over-bindings counterexample: generic Actions, constraints, policies, consent and thin REST/MCP/A2A bindings. It shows that a semantic layer need not invent a wholly separate transport. It remains domain-oriented and delegates specific Action effect semantics to extensions.
   - Sources: UCP v2026-08-25 — https://ucp.dev/2026-08-25/specification/overview/ ; announcements — https://ucp.dev/documentation/announcements/

3. AP2 already uses signed mandates and receipts for a high-risk domain. A general AgentBridge object must show cross-domain value beyond renaming this domain-profile pattern.
   - Source: AP2 v0.2 — https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md

4. Adoption economics strongly favor incremental deployment. An architecturally cleaner native protocol can still fail if both ends must move together.
   - Sources: RFC 5218 — https://www.rfc-editor.org/rfc/rfc5218.html ; RFC 1958 — https://www.rfc-editor.org/rfc/rfc1958.html

5. The candidate thin waist risks being either too thin to add safety or too thick to standardize domain effects. End-to-end correctness and duplicate suppression still require application participation.
   - Source: Saltzer, Reed, Clark, “End-to-End Arguments in System Design” — https://web.mit.edu/Saltzer/www/publications/endtoend/endtoendA4.pdf

6. Conversely, a bridge-free pilot can be safer than an adapter-heavy pilot. OSI security-label guidance warns that gateways may need to interpret or transform labels, creating consistency and failure concerns.
   - Source: RFC 1457 — https://www.rfc-editor.org/rfc/rfc1457.html

## Required falsification experiments

- **Composition challenge:** first express the proposed invariants using existing extension/profile mechanisms. Native core is justified only by concrete unrepresentable or ambiguously represented mandatory semantics.
- **Thin-waist test:** keep universal objects small; if a common object is only an opaque domain payload, the core's value is unproven.
- **Independent differential test:** two teams implement a native and a composed path without a shared protocol library; compare authorization/effect outcomes under replay, mutation, partition and cancel races.
- **Bridge loss test:** every mapping proves preserved meaning and assurance or fails closed.
- **Adoption test:** compare endpoint changes, integration effort and steady-state dependency/latency.

## Impact on recommendation

Option C remains the preferred architecture **hypothesis**, because the approved hard requirements demand native independence and current standards do not own the full domain-neutral action-to-effect seam. Confidence is reduced from high to medium. The next artifact must be a validation charter/requirements document with a composition challenger and automatic stop conditions, not a production specification.
