# Digest: UCP/ACP freshness correction

Accessed: 2026-09-12

## Findings

1. The current official UCP release is **v2026-08-25**, not v2026-04-08. The official announcements page records the August release and links its release notes; the versioned specification is live. GitHub's Releases UI still lists v2026-04-08 as latest, so repository release metadata is stale or incomplete and must not override the official published specification.
   - Sources: UCP Authors, announcements — https://ucp.dev/documentation/announcements/ ; UCP Authors, versioned specification — https://ucp.dev/2026-08-25/specification/overview/
   - Status: released 2026-08-25; accessed 2026-09-12
   - Confidence/class: high; version-compatibility

2. UCP v2026-08-25 materially strengthens the composition precedent. It adds or documents request constraints and their lifecycle, action trust/execution boundaries, exact-version profile selection, per-request version validation, namespace authority binding, and a closed set of signature-covered request components including body metadata and idempotency key. It explicitly says an external Action interaction does not prove the gated effect succeeded. This narrows but does not eliminate the AgentBridge hypothesis: UCP remains commerce-oriented and leaves operation-specific outcome semantics to each capability.
   - Source: UCP Authors, versioned specification — https://ucp.dev/2026-08-25/specification/overview/
   - Status: released 2026-08-25
   - Confidence/class: high for normative facts; medium for AgentBridge inference

3. The acronym **ACP is ambiguous and must be split into two rows**:
   - BeeAI **Agent Communication Protocol** has been merged into A2A and is now a migration concern.
   - OpenAI/Stripe **Agentic Commerce Protocol** is a separate beta commerce standard, with latest stable artifacts dated 2026-04-17 and unreleased work continuing. It covers buyers/agents/businesses, checkout, carts, feeds, orders, authentication, payments, capability negotiation, MCP integration, and extensions; it is not a general domain-neutral agent-to-service action protocol.
   - Sources: BeeAI / LF AI & Data repository — https://github.com/i-am-bee/acp ; OpenAI and Stripe repository — https://github.com/agentic-commerce-protocol/agentic-commerce-protocol
   - Status: BeeAI migration notice current; Agentic Commerce Protocol beta/current, latest stable snapshot 2026-04-17
   - Confidence/class: high; landscape/version-compatibility

## Decision impact

- UCP is stronger contrary evidence than the first-round digest represented. A permanent composition/profile is credible for commerce, and AgentBridge should not duplicate UCP's domain objects or payment model.
- UCP's rapid evolution and GitHub/web release-metadata mismatch also demonstrate why optional bridges must pin official versioned artifacts and why AgentBridge cannot inherit external version state implicitly.
- The Gate 0 recommendation changes in confidence, not direction: a native **domain-neutral action/effect/authority thin waist** remains plausible, while commerce semantics belong in a UCP or Agentic Commerce Protocol bridge/profile rather than AgentBridge core.

