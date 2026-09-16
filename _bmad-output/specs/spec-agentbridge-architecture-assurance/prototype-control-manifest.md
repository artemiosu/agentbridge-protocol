---
title: Pre-Prototype Control Manifest
status: template-no-execute
created: 2026-09-15
updated: 2026-09-15
decision_id: AD-21
---

# Pre-Prototype Control Manifest

Пока все обязательные поля конкретного экземпляра не заполнены, независимо проверены, подписаны и заморожены с digest/expiry, **любая executable model или prototype запрещены**. Documentary Architecture work разрешён.

## Обязательные поля экземпляра

| Область | Что фиксируется | Current |
| --- | --- | --- |
| Identity | prototype ID, purpose, exact scope, owner, start/end, expiry | unfilled |
| Classification | artifact/data/evidence classes; publishability | unfilled |
| Inputs | exact source artifacts and digests | unfilled |
| Dependencies | direct/transitive BOM/SBOM, versions, sources, licenses, notices, vulnerability status | unfilled |
| Rights/IP | right to use/store/modify/output; AI/tool terms; patent/trademark exclusions | unfilled |
| Environment | reproducible sandbox, host/runtime/toolchain digests | unfilled |
| Storage | stores, regions, encryption keys, access roles, backups, retention/deletion | unfilled |
| Data | synthetic-only fixtures; no customer/production/personal/regulatory data | unfilled |
| Secrets | no production credentials; ephemeral test-secret lifecycle | unfilled |
| Network | deny-by-default egress; exact allowlist; telemetry/update prohibition | unfilled |
| Targets and effects | exact endpoints/accounts/namespaces/resources; local/simulated or owner-controlled isolated targets only; effect classes; effect detector; cleanup/rollback and residual-effect verification; zero real customer/third-party/production/financial/legal/public consequential effect | unfilled |
| External actions | zero spend, purchase, payment, asset/entitlement mutation, contract, registration, company/foundation/marks action, public communication/publication, message to a real third party or other external commitment; any future exception requires separate explicit authority outside this manifest | unfilled |
| Resources | CPU/memory/state/I/O/storage/network/wait/payload/depth/concurrency/fan-out/retry/amplification ceilings and deterministic safe-stop | unfilled |
| Observers | for every applicable SBC with Protected Disclosure or Consequential Effect, minimum two independently derived effect/evidence observer paths; qualification, common-mode dependencies, gap/disagreement handling (`invalid`/`inconclusive`, never pass) | unfilled |
| Failure | incident, evidence preservation, containment, cleanup, rollback/residual-effect verification and attribution rules | unfilled |
| Reproducibility | commands/config/seeds/logs and immutable result manifest | unfilled |
| Review | Security/Privacy, Rights/IP and architecture reviewers; conflicts and verdicts | unfilled |
| Authorization | Project Owner scope approval, digest, timestamp and expiry | unfilled |

## Verdict rule

- `frozen-approved`: executable work разрешено только в указанном scope и сроке.
- `incomplete/no-use`: запуск запрещён.
- Egress allowlist, synthetic data и отсутствие production credentials сами по себе не разрешают взаимодействие с реальным target или реальный consequential effect.
- Любое изменение manifest digest либо любого frozen input, dependency, right/term, environment, data, secret, store, target, endpoint, account, namespace, effect class, external action, egress, ceiling, observer path/common-mode dependency, failure/cleanup rule, scope или expiry автоматически возвращает `incomplete/no-use` до повторного independent review и authorization.
