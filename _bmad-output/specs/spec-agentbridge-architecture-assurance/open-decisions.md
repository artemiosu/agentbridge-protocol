# Open decisions

Открытый вопрос блокирует только указанный Gate. Решение о Native direction повторно не открывается.

| ID | Решение | Блокирует | Status |
| --- | --- | --- | --- |
| AD-1 | Architecture principles и non-negotiable invariants | AA-1 | open — next |
| AD-2 | Core/Profile/Extension/Binding/Bridge boundaries | AA-1 | open — next |
| AD-3 | Quality-attribute scenarios и Architecture scope v0.x | AA-1 | open — next |
| AD-4 | Role/context/interaction semantic model | AA-2 | open |
| AD-5 | Authority/delegation/consent/effect/evidence model | AA-2/AA-3 | open |
| AD-6 | Lifecycle/retry/replay/cancel/revoke/compensation state machines | AA-2 | open |
| AD-7 | Formal/executable modeling method and coverage | AA-2 | open |
| AD-8 | Trust/data/enforcement/privacy boundaries | AA-3 | open |
| AD-9 | Canonical/signable representation and crypto profile | AA-3/AA-4 | open |
| AD-10 | Native transport/encoding Binding and fallback | AA-4 | open |
| AD-11 | Reference runtime/language: Rust, Go or other | AA-4 | open |
| AD-12 | Aggregate resource ceilings and benchmark protocol | AA-4 | open |
| AD-13 | Conformance assertions/vectors/oracle/observer architecture | AA-5 | open |
| AD-14 | Spec-only/reference separation and genealogy package | AA-6 | open |
| AD-15 | Specification/code/conformance licenses and patent policy | AA-8 | open; qualified legal review required |
| AD-16 | Trademark/name clearance and marks policy | AA-8 | blocked; no external brand use |
| AD-17 | Minimal neutral governance and stewardship milestones | AA-8/AA-11 | open |
| AD-18 | First limited production domain/jurisdiction | AA-9 | deferred |
| AD-19 | Adoption Charter and first external wedge | AA-10 | deferred |
| AD-20 | Business & Ecosystem Strategy / Protocol-Platform Firewall | Самое раннее из: paid design-partner work, external contributions, AA-8 publication, company/foundation/asset action | open in parallel after AA-1 framing |
| AD-21 | Pre-Prototype Control Manifest: exact rights, dependencies, stores, egress, resources, observers, approvals and expiry | Любая executable model/prototype | open; documentary AA-1 design разрешён |

Текущий следующий шаг: AA-1 через BMAD Architecture. Он создаёт Constitution, boundaries, quality scenarios, allocation и ADR discipline; код не начинается.
