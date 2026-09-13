# OQ-1 Freeze Manifest — OQ1-M1

Status: **frozen 2026-09-13 by Project Owner**.

Этот manifest фиксирует выбор контрольных кандидатов, а не их будущую реализацию. Candidate design и code остаются **No Start** до полного OQ-3B/OQ-8 и остальных обязательных решений Charter.

## Frozen selection

| Роль | Frozen identity | Практический смысл |
| --- | --- | --- |
| Primary C | `C1-oq1-m1` | Сильнейшая практически доступная композиция A2A/MCP + OAuth/AuthZEN + UCP/AP2 |
| Authority challenger | `C2-oq1-m1` | Те же прикладные слои, но GNAP RFC 9635/9767 вместо OAuth authority plane |
| Reserve | ANP 1.1 | Допускается только после released-only BOM и повторного прохождения hard gates |
| Required bounded experiment | AGNTCY augmentation/substitution | Проверяется как инфраструктурное усиление C1, но не является semantic core или mandatory runtime dependency |

Полные версии, границы, обязательный glue и исключения определены в `candidate-manifests.md` и не должны интерпретироваться шире.

## Frozen evidence

| Артефакт | SHA-256 | Роль |
| --- | --- | --- |
| OQ-1 `research.md` | `fd562fd950db82d8da97cba04dee2d4d20d8a7f08596577105031e008f94496b` | Итоговое исследование и рекомендация |
| OQ-1 `brief.md` | `43431c81aec1f3fee18cbc201f3e6cd2dd9e7fa21cc35dc5dba2f26b933071fa` | Принятые search/inclusion rules и веса |
| `semantic-citation-verifier-r1-1.md` | `cfd1fadf466898d2bd504c6edab42e43b2400ebb019069293ff53ed7c3e8459c` | Финальный semantic/version citation PASS |
| `final-structure-prose-review-r1-1.md` | `4b15226fd8bf3558a0aedca9a86ffa617dadfc8067dccb9ae437af229ef85577` | Финальный structure/prose PASS |

Путь evidence package: `../../planning-artifacts/research/technical-agentbridge-oq-1-composition-challenger-2026-09-13/`.

## Explicit non-decisions

- OQ-1 не выбирает C1/C2 как архитектуру AgentBridge и не отменяет полноценный Native Core.
- OQ-1 не утверждает, что существующие стандарты закрывают AgentBridge; обязательный cross-protocol glue остаётся их измеряемой стоимостью.
- OQ-1 не выбирает язык, transport, encoding, cryptographic provider, SDK shape или repository layout.
- OQ-1 не даёт итоговую рекомендацию `native/profile/upstream/stop`; это результат будущего Gate 1.
- OQ-1 не разрешает production specification, Architecture, SDK или экспериментальный code.

## Freshness and invalidation

- Version/compatibility claims перепроверить не позднее 2026-10-13.
- Fast-moving landscape перепроверить не позднее 2026-12-13.
- Ecosystem/adoption evidence перепроверить не позднее 2027-03-13.
- Security advisory, breaking release, erratum, governance/IP change или несовместимость любого выбранного компонента немедленно приостанавливает затронутый scope и требует re-review.

Любое смысловое изменение C1/C2 identity, component ownership, mandatory/optional boundary, hard-gate interpretation или AGNTCY/ANP disposition автоматически снимает OQ-1 freeze до нового исследования, независимой проверки и принятия Project Owner.
