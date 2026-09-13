# Final structure and prose review

**Verdict: revision required before the report is treated as the final OQ-1 decision record.** The recommendation is decision-first and the audit trail is strong, but three unresolved status/modality conflicts can change how a reader interprets the selected challengers and Gate 1 scope.

This document exists to help AgentBridge decision-makers select reproducible Composition Challengers and understand exactly what Gate 1 must test. The appropriate structure model is **Strategic/Context (Pyramid)**.

## Required findings

| Pass | Original text or location | Revised text or action | Changes |
| --- | --- | --- | --- |
| structure | `C2 — GNAP Authority Challenger`; `C2 mandatory configuration`; Known unknowns | **QUESTION:** State one hard-gate result consistently: C2 passes now, passes only after named prerequisites, or is not yet a surviving candidate. If implementation from public RFCs is sufficient, say so explicitly and distinguish it from deployability using the examined public implementation. | The report selects C2, then says important features must be built and no complete conformance program was found. This is the most consequential internal ambiguity. No material word reduction. |
| prose | “C2 is non-dominated because GNAP offers…” | “C2 is **provisionally** non-dominated on authority semantics, subject to [the explicit hard-gate condition].” | Calibrates confidence to the evidence and prevents a conditional candidate from reading as an unconditional survivor. |
| structure | SCITT/COSE in the executive recommendation, C1 table, and glue list | **QUESTION:** Choose one rule throughout: either protocol-neutral evidence semantics are mandatory and SCITT/COSE are optional carriers, or the scoped SCITT/COSE profile itself is mandatory. | Current `may` versus mandatory-row wording changes C1’s actual manifest. No material word reduction. |
| structure | AGNTCY in “Reserve and optional variants,” C1 mandatory table, and next actions | **QUESTION:** Separate an optional infrastructure sensitivity test from mandatory C1 runtime components. State whether the bounded AGNTCY experiment is required for acceptance or merely recommended. | “Optional,” “mandatory configuration,” and “Include” currently conflict. Likely saves 10–20 repeated words after consolidation. |
| structure | Weighted comparison and Candidate-neutral invariant coverage | **MOVE** both sections to immediately after “Method and decision boundary”; place “Strongest contrary evidence” immediately after the scorecard. | The decisive support currently appears after roughly 1,300 words of background. Movement only; no word reduction. |
| structure | Executive recommendation (453 words) | **CONDENSE** to the selected candidates, decisive trade-off, qualifications, reserve status, and Gate 1 boundary. Move component inventory and detailed exclusions to the manifests. | The opening repeats later manifests and pins. Target about 220–260 words; saves about 190–230 words without losing the decision. |
| structure | Conclusions and required next actions (80 words) | **MERGE/CONDENSE** into “Required next actions.” Remove repeated selection and reserve decisions; retain only unique deliverables and admission conditions. | Saves about 30–40 words and prevents the report from making the same decision twice. |
| structure | First use of `authority cell`, `decision-subject digest`, `false-zero resistance`, and `dependency closure` | **QUESTION:** Add a short terminology note before the manifests or replace each term with plain language. | These terms carry decision-critical meaning but are not defined for a human reader. Adds roughly 50–80 words if definitions are retained. |

## Additional high-value edits

| Pass | Original text or location | Revised text or action | Changes |
| --- | --- | --- | --- |
| structure | Landscape findings (441 words) | **CONDENSE** repeated disposition language; keep evidence that explains the scores and Gate 1 gaps. Consider a compact disposition table for excluded candidates. | Target a 120–170 word reduction. |
| structure | Pinning baseline (108 words) | **MOVE** intact to a version-pins appendix immediately before the source appendix. | Preserves reproducibility while keeping the decision narrative focused. No reduction. |
| prose | “C1 is the strongest deployability baseline found” | “C1 is the strongest deployability baseline **within the reviewed candidate set**.” | Removes an unnecessarily universal reading. |
| prose | Weighted labels `Coverage`, `Safety`, `Topology`, `Deployability`, `Governance` | Use the same full labels as the method: `Invariant coverage`, `Authority/safety/effects`, `Topology/lifecycle`, `Deployability/glue`, `Governance/evolution`. | Prevents terminology drift between method and result. |
| prose | Dense forms including “task/effect state,” “provenance/loss annotations,” and “resource-server introspection and failure behavior” | Use “task state and effect state,” “annotations for provenance and semantic loss,” and “resource-server introspection, including failure behavior.” | Improves scanability without changing meaning. |

## Preserve

- Preserve the opening statement that the challengers are controls rather than the AgentBridge architecture.
- Preserve the manifests, invariant matrix, bounded score ranges, contrary-evidence section, known unknowns, dated recheck triggers, and complete source appendix.
- Preserve the explicit statement that overlapping score ranges do not justify a precise numerical winner.

If all recommendations are accepted, the 2,814-word report should shrink by a net **about 320–400 words (11–14%)**, after allowing for a short terminology note. The main benefit is not brevity: it is an unambiguous hard-gate verdict and a faster path from recommendation to its decisive evidence.

## Re-review after required fixes — 2026-09-13

**PASS.** No remaining Critical/High structure or prose findings.

Confirmed: C2 now has an explicit conditional hard-gate result; protocol-neutral evidence semantics are mandatory while SCITT/COSE is optional; AGNTCY is a required bounded comparison experiment rather than a C1 runtime component; invariant coverage and the scorecard now follow the method; the executive recommendation is materially shorter; and all four decision-critical terms are defined before use. The report now presents a clear decision, calibrated qualifications, consistent component status and an auditable path into Gate 1.

## Финальная проверка русской версии — 2026-09-13

**PASS.** Блокирующих замечаний по структуре или ясности нет; смысловых искажений принятой рекомендации C1/C2 не обнаружено.

Русская версия сохраняет decision-first структуру, статус C1 как Primary C, условное прохождение C2 только по критерию независимой реализуемости, обязательность нейтральной к протоколу семантики доказательств при необязательности SCITT/COSE как носителя и статус AGNTCY как обязательного ограниченного сравнительного эксперимента, а не зависимости исполнения. Термины, определяющие решение, объяснены до таблиц; диапазоны оценок и ограничения уверенности сформулированы понятно для неинженерного читателя.
