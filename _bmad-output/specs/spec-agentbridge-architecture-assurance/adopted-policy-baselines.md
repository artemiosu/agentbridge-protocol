# Adopted policy baselines

## Правило применения

Старый Validation Charter superseded только в части existential direction и comparative decision lattice. Его замороженные policy остаются обязательными, пока новая архитектурная редакция явно не заменит их более строгим проверенным правилом.

| Baseline | Статус | Применение |
| --- | --- | --- |
| `evaluation-ledger.md`, EI-01–EI-25, OQ-2A | retained | Обязательные candidate-neutral/architecture-neutral invariants |
| `safety-blocking-and-risk.md`, SBC-01–SBC-10, OQ-4 | retained | Безусловные blockers и incident/disclosure semantics |
| `observers-and-holdout.md`, OQ-3A | adapted | Oracle/observer/fairness rules применяются к models, conformance и benchmarks; старая N-vs-C arm logic не управляет курсом |
| `measurement-and-decision.md`, OQ-5A | partially adapted | Reproducibility, guardrails, resource/evidence/statistical discipline retained; RTTSI comparative arms и native/profile/upstream/stop lattice historical |
| `roles-evidence-and-governance.md`, OQ-6A | retained/adapted | Independence, access, custody, exposure, appeal и incident rules применяются по риску конкретного Gate |
| `budget-and-feasibility.md`, OQ-7A | retained/adapted | USD 0 authorization и outcome-blind funding/closure rules retained; старый confirmatory N-vs-C budget path historical |
| `rights-storage-and-publication.md`, OQ-8A | retained | Rights/IPR/storage/privacy/disclosure/publication/anti-capture обязательны |
| `candidate-manifests.md`, OQ-1 | historical/reference | Standards pins, known compatibility/defect evidence и bridge candidates; C1/C2 не являются альтернативами направления проекта |
| `validation-charter.md` Gate 1A–1D | superseded | Не управляет направлением или запуском Architecture |
| OQ-2B/OQ-3B/OQ-5B/OQ-6B/OQ-7B/OQ-8B | re-scoped | Точные controls создаются перед соответствующим model/prototype/implementation/publication Gate, а не как единый existential experiment |

Все перечисленные файлы находятся в `../spec-agentbridge-validation-charter/` и сохраняются без переписывания истории.
