# Expert review program

## Review councils

| Council | Основной предмет | Первый обязательный Gate |
| --- | --- | --- |
| Protocol semantics / distributed systems | roles, state, ordering, failure, multi-party | AA-1/AA-2 |
| Identity / authorization / delegation / cryptography | authority, consent, verifier, canonical signing | AA-2/AA-3 |
| Privacy / abuse / malicious agents | disclosure, linkability, misuse and recovery | AA-3 |
| Formal methods / concurrency | state invariants, races, partitions, model checks | AA-2/AA-3 |
| Networking / encoding / performance | wire, fallback, backpressure, ceilings | AA-4 |
| Conformance / interop / SDK DX | assertions, vectors, independent implementability | AA-5/AA-7 |
| Versioning / evolution / ecosystem compatibility | upgrades, extensions, bridge drift | AA-2/AA-5 |
| Standards governance / IP / trademark / antitrust | RF rights, change control, marks, capture | AA-8 |
| Adoption / migration / product value | single-player value and external implementation | AA-10 |
| OSS business / platform firewall | voluntary paid layers and conflicts | Business Validation |
| Cross-discipline red team | interaction of all risks and local optimizations | AA-6 and AA-8 |

## Independence labels

- `I1 internal`: AI/fresh-context or same-controller review; useful for finding defects, not external recognition.
- `I2 independent review`: documented competence, independent control/failure domain, conflict disclosure, fixed scope, evidence access and signed verdict.
- `I3 external implementation/adoption`: independent organization implements/operates without private author assistance.

## Required output

Каждый review сохраняет scope, inputs/digests, reviewers/independence, method, findings, dissent, verdict и closure evidence. Допустимые verdicts: `pass`, `pass with conditions`, `redesign/block`.

## Минимальная независимость по этапам

| Gate | Минимум для внутреннего продвижения | Что дополнительно блокирует публичный/внешний claim |
| --- | --- | --- |
| AA-1 | Несколько раздельных I1 lenses: protocol, security, evolvability, product | I2 не требуется для documentary AA-1 |
| AA-2 | I1 protocol/distributed-systems + formal-methods + security co-review | Нельзя делать external/formal-security claim |
| AA-3 | I1 security/privacy/identity + formal review; separate dissent record | I2 protocol/security/formal verdicts обязательны до AA-8 |
| AA-4 | I1 networking/performance/security/conformance review; controlled evidence | I2 при новом security-critical binding mechanism до AA-8 |
| AA-5 | I1 conformance/interop/fairness/security review | I2 conformance/protocol review до AA-8 |
| AA-6 | Cross-discipline I1 PASS разрешает только non-production internal implementation | Не является внешним признанием или production evidence |
| AA-7 | Spec-only separation и genealogy; claim ограничен фактическим controller level | I2 reviews и independently controlled implementation нужны для external claim |
| AA-8 | Named I2 protocol, security/formal, conformance и IP/legal reviewers; exact roster/conflicts/access/signed verdicts | Незакрытый I2 requirement блокирует publication |
| AA-10 | I3 independently controlled implementations/deployments | Обязательно для adoption claim |

Если I2/I3 недоступен раньше требуемого Gate, работа может продолжаться только в явно внутреннем scope; применимый внешний release/claim остаётся blocked, а не автоматически получает pass или вечный общий stop.
