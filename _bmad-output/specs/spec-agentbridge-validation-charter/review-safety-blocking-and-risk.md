# Independent security review — OQ-4

Date: 2026-09-13  
Reviewed artifact: `safety-blocking-and-risk.md`  
Initial verdict: **CHANGES REQUIRED**

## Initial findings

| ID | Severity | Finding | Resolution |
| --- | --- | --- | --- |
| OQ4-SEC-01 | Critical | Не задан отдельный preregistered severity hard-gate threshold | Установлен immutable threshold High; unresolved Critical/High блокирует независимо от SBC |
| OQ4-SEC-02 | Critical | Severity definitions противоречили возможности SBC с Medium | Severity теперь определяется только impact/reach/blast radius; SBC — отдельная ось |
| OQ4-SEC-03 | High | TOCTOU class не называл Protected Disclosure boundary | SBC-02 охватывает disclosure и effect boundary |
| OQ4-SEC-04 | High | Неполнота общей threat model могла ошибочно стать candidate-fail | Common-foundation defect отделён от candidate-caused omission/equivocation |
| OQ4-SEC-05 | High | Responsible disclosure не включал полный lifecycle NFR-29 | Добавлены intake/ack, clocks, notifications, reviewer recommendation и post-incident review |
| OQ4-SEC-06 | Medium | Closure не требовал immutable identity исправленного artifact | Требуются version, digest и dependency closure; учтён новый post-holdout replication tranche |
| OQ4-SEC-07 | Medium | «Воспроизводимость» могла стереть единичное событие | Raw evidence + independent attribution достаточно; необъяснённое событие остаётся inconclusive |
| OQ4-SEC-08 | Medium | Не все privacy/data-lifecycle bounds были перечислены | SBC-06 расширен classification/retention/deletion/residency/redaction bounds |

## Re-review

Первый re-review подтвердил закрытие OQ4-SEC-01…08, но выявил один новый High attribution gap: artifact mismatch мог ошибочно стать finding кандидата при доказанном сбое harness/environment. SBC-09 и verdict table исправлены.

Финальный re-review: **PASS**. Critical 0, High 0, Medium 0, Low 0. Candidate-caused mismatch даёт `candidate-fail + SBC`; доказанный сбой harness/environment — `run-invalid`; неустановленная причина — `inconclusive`. Новых регрессий не найдено.
