# Commerce/payment protocols — round 2 digest

Accessed: 2026-09-13. Official primary sources only.

## Include/cut result

- **Include UCP v2026-08-25 (`cd78fb3`)** as commerce/discovery/lifecycle component.
- **Include AP2 v0.2.0 (`b4587ac`)** as optional consequential-action mandate/evidence layer bound to UCP Checkout.
- **Keep ACP 2026-04-17 (`9abf303`) as compatibility adapter**, not canonical co-equal layer.
- **Include x402 v2 conditionally** for paid API/data/compute/agent-service use, not retail state.
- **Visa TAP optional at network edge only** where contractual scheme trust is available; exclude from independently operable core.
- **Cut Mastercard Agent Pay** because no independently implementable public normative protocol was established.

## Verification findings

1. **UCP release and license verified; ecosystem consolidation signal corroborated by Stripe.** Latest release is v2026-08-25/`cd78fb3`, Apache-2.0. Stripe’s current commerce protocol page names UCP and documents checkout, identity linking, orders and token exchange; this is not proof ACP is formally deprecated. Sources: [UCP release](https://github.com/Universal-Commerce-Protocol/ucp/releases/tag/v2026-08-25), [Stripe UCP docs](https://docs.stripe.com/agentic-commerce/protocol). Publishers: UCP project, Stripe. Confidence: high. Class: version/ecosystem.

2. **UCP↔AP2 direction is explicit and asymmetric.** UCP provides commerce/Checkout; AP2 is an optional trust extension that binds mandates into UCP `/complete`. AP2 can conceptually use another commerce object but does not define catalog, checkout-update or transport APIs. Source: [UCP and AP2](https://ucp.dev/documentation/ucp-and-ap2/). Publisher: UCP Authors. Confidence: high. Class: compatibility/dependency.

3. **AP2 is independently implementable but its operational trust machinery and conformance remain incomplete.** Public Apache-2.0 repo includes schemas/SDK/scenarios, but install guidance points to GitHub and production needs trusted surface, keys/trust list, credential provider and processor integrations. No standalone AP2 TCK verified. Source: [AP2 v0.2.0](https://github.com/google-agentic-commerce/AP2/releases/tag/v0.2.0). Publisher: AP2 project. Confidence: high on release, medium on conformance. Class: implementation/maturity.

4. **Latest UCP specification is newer than documented conformance/sample baseline.** Conformance README targets 2026-04-08 and samples remained constrained below the SDK targeting v2026-08-25. The newest normative pin is correct, but deployability must be conditional on adapted/upstream conformance. Source: [UCP conformance suite](https://github.com/Universal-Commerce-Protocol/conformance). Publisher: UCP project. Current 2026-09. Confidence: high. Class: conformance contradiction.

5. **ACP adds installed-base reach, not a new semantic dimension.** Current main is later, but 2026-04-17/`9abf303` remains the stable snapshot. Its commerce surfaces overlap UCP; retain an adapter for existing OpenAI/ACP counterparties and do not make ACP types canonical. Source: [ACP commits](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/commits/main/). Publisher: ACP project. Confidence: high on status, medium-high on successor inference. Class: overlap/adoption.

6. **x402 adds a narrow Pareto dimension for accountless paid digital resources.** It spans HTTP/MCP/A2A payment gating but lacks cart, fulfillment, return/refund, booking and AP2-style mandate semantics. Stable immutable commit remains unverified. Source: [x402 v2 spec](https://github.com/x402-foundation/x402/blob/main/specs/x402-specification-v2.md). Publisher: x402 Foundation. Confidence: high on scope, medium on pin. Class: optional payment rail.

7. **Visa TAP provides unknown-agent recognition but fails independent-trust hard gate.** Public material and sample use HTTP Message Signatures and a key registry, but Visa-approved agents/keys and Product Terms remain a scheme-controlled dependency; canonicalization precision also needs review. Source: [Visa TAP specification](https://developer.visa.com/capabilities/trusted-agent-protocol/trusted-agent-protocol-specifications). Publisher: Visa. Confidence: high on dependency, medium on interoperability risk. Class: optional edge trust.

## Remaining blockers

UCP 2026-08-25 conformance, standalone AP2 TCK/trust-list/revocation, released lodging/booking, automated dispute/evidence retention, immutable x402 pin and Visa legal grants remain unverified. These must appear as explicit Challenger limitations/gates.
