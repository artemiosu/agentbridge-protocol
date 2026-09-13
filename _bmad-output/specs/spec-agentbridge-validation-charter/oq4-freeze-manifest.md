# OQ-4 freeze manifest

Manifest version: `OQ4-M1`  
Freeze authority/date: **Project Owner, 2026-09-13**  
Current scope: **OQ-4 safety policy frozen; Charter remains draft / No Start**  
Independent security review: **PASS; Critical 0, High 0, Medium 0, Low 0**

## Frozen identity

| Artifact | SHA-256 | Role |
| --- | --- | --- |
| `safety-blocking-and-risk.md` | `937c093fcc2b4a9dd5bc5fc0663c9ddd707ed25e29f9603014feeddbd2bc1fad` | frozen OQ-4 policy |

## Frozen dependencies and review evidence

| Artifact | SHA-256 | Role |
| --- | --- | --- |
| adopted `prd.md` | `21afec0a195e8e896a77b82264b180c10a6907bbc21f391f062fb6041867e09d` | requirement authority |
| frozen `evaluation-ledger.md` | `8290da34932c986f79067275e57f270e5d539585741633abae91c7eeb69ce575` | EI-01–EI-25 authority |
| frozen `oracle-and-outcomes.md` | `4aac799d07bab8d1aa4cf06b41cd1883724e4a7b27b3751bab113c5d1a04aa80` | verdict and oracle authority |
| frozen `observers-and-holdout.md` | `129350d2bda6697430e29823c5a128667f0cb289222dc0e5967df8897c835c87` | source-attribution authority |
| `review-safety-blocking-and-risk.md` | `4d03e545857dca1c08d3e7aa6522a033010a94c354846d9023d68ab804f65619` | independent review record |

## Frozen rules

- Любой SBC-01–SBC-10 запрещает положительный выбор при любом severity.
- Любой unresolved Critical или High finding является отдельным hard-gate failure.
- Candidate-caused failure, independently proven harness failure и unknown attribution дают соответственно `candidate-fail`, `run-invalid` и `inconclusive`.
- Единичное независимо подтверждённое запрещённое событие не стирается усреднением или rerun.
- Residual-risk acceptance не отменяет SBC или другой hard gate.
- Closure требует новой immutable artifact identity, scoped regression/replication и независимого подтверждения.

## Change control and downstream slots

Любое смысловое изменение frozen rules, SBC membership, severity threshold, attribution, residual-risk либо disclosure process снимает OQ-4 freeze, создаёт новый manifest/version и требует независимого security review и повторного принятия Project Owner. Старые evidence и dissent сохраняются.

OQ-3B, OQ-5, OQ-6 и OQ-8 обязаны заполнить exact vectors/observers, ceilings, roles и storage/disclosure controls без ослабления OQ-4. Если это невозможно, OQ-4 автоматически возвращается в `open`. Candidate design и код по-прежнему запрещены.
