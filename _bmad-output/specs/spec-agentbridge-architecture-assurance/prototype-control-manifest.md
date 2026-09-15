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
| Resources | CPU/memory/state/I/O/storage/time/fan-out/retry/amplification ceilings and safe-stop | unfilled |
| Observers | effect/evidence observer paths, qualification and gap handling | unfilled |
| Failure | incident, evidence preservation, containment, cleanup and attribution rules | unfilled |
| Reproducibility | commands/config/seeds/logs and immutable result manifest | unfilled |
| Review | Security/Privacy, Rights/IP and architecture reviewers; conflicts and verdicts | unfilled |
| Authorization | Project Owner scope approval, digest, timestamp and expiry | unfilled |

## Verdict rule

- `frozen-approved`: executable work разрешено только в указанном scope и сроке.
- `incomplete/no-use`: запуск запрещён.
- Любое изменение digest, dependency, data, store, egress, ceiling или scope автоматически возвращает `incomplete/no-use` до повторного review.
