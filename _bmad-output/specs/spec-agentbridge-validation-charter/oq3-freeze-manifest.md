# OQ-3 freeze manifest

Manifest version: `OQ3-M1`
Freeze authority/date: **Project Owner, 2026-09-13**
Current scope: **OQ-3A frozen; OQ-3B open / No Start**
Independent review: **PASS; Critical 0, High 0; 12/12 findings resolved**

## Frozen OQ-3A identities

| Artifact | SHA-256 |
| --- | --- |
| `oracle-and-outcomes.md` | `4aac799d07bab8d1aa4cf06b41cd1883724e4a7b27b3751bab113c5d1a04aa80` |
| `atomic-coverage-inventory.md` | `fd757a5822b057ff88e961e70546ff8f62e5c92186ba4c2f0e4dd76fa377e94e` |
| `scenario-corpus.md` | `67205d56cadca75b27bfe2425f37149c4c782bdfbe1a05149e0ecd8f7043b055` |
| `observers-and-holdout.md` | `129350d2bda6697430e29823c5a128667f0cb289222dc0e5967df8897c835c87` |
| `experiment-matrix.md` | `08c773a85537685b993d39b4e22ad294a175d84952f06f6d2fa9380cc6ee7cba` |

## Frozen dependencies and review evidence

| Artifact | SHA-256 | Role |
| --- | --- | --- |
| adopted `prd.md` | `21afec0a195e8e896a77b82264b180c10a6907bbc21f391f062fb6041867e09d` | requirement authority |
| frozen `evaluation-ledger.md` | `8290da34932c986f79067275e57f270e5d539585741633abae91c7eeb69ce575` | EI-01–EI-25 authority |
| `review-oq3a-blind.md` | `f25ee88556e6d65e630026d61ea07dd1d5f140929d4298a26db8c85b5b0b37a8` | independent review record |

## Precedence and change control

Normative precedence: frozen PRD/EI → this manifest → Oracle/Outcome → atomic inventory → scenario/observer rules → future exact vector. Любой конфликт блокирует run.

Редакционное изменение, меняющее SHA-256, требует classification. Любое смысловое изменение verdict algebra, coverage, grammar, applicability, observer rules, assurance order, holdout generation/sealing либо decision mapping снимает OQ-3A freeze, создаёт новый manifest/version, перечисляет затронутое evidence и требует нового blind review и принятия Project Owner. Старые версии и результаты сохраняются.

## OQ-3B slots — not frozen

Exact PRD clause index, shared-confirmatory vectors, calibration corpus, assurance orders, OQ-4/OQ-5/OQ-8 parameter values, holdout strata/probabilities/seed/sealed cases и полный closure report отсутствуют. До их атомарной preregistration запрещены OQ-2B, Candidate design и код.
