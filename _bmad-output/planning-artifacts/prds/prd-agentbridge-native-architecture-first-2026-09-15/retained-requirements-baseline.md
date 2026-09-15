---
title: Retained Requirements Baseline
status: frozen-reference
created: 2026-09-15
source_revision: prd-agent-bridge-sdk-2026-09-12
---

# Retained Requirements Baseline

## Назначение

Этот manifest точно отделяет сохранённые требования от superseded Gate 1. Downstream Architecture обязана читать только указанные sections как канонический полный текст FR-1–FR-95 и NFR-1–NFR-29; краткие формулировки нового PRD являются индексом, а не заменой acceptance details.

## Frozen source

- Source: `../prd-agent-bridge-sdk-2026-09-12/prd.md`
- Whole-file SHA-256 after supersession banner: `3bd3025053c633d157ee267ad7388420a1c24d7e8c9facc9f6266e57e6c4eb4c`
- Snapshot date: 2026-09-15

| Retained text | Heading-bounded selector | Snapshot lines | Extract SHA-256 |
| --- | --- | ---: | --- |
| FR-1–FR-95 | From `### 4.1 F1` inclusive to immediately before `### 4.9 F9` | 175–1286 | `0bdf52d56f9edee19159434c65ad3b4e0df2033f2ed22ded7d0f3ec7f514be7a` |
| NFR-1–NFR-29 | From `## 9. Cross-Cutting Non-Functional Requirements` inclusive to immediately before `## 10. Guardrails текущего эксперимента` | 1706–1839 | `bcf681353c2eb0799b25ccc54750c726b21c0a683cfe1943fad631f9aebca78d` |

Heading selectors, not line numbers, govern extraction; line numbers are audit hints for this snapshot.

## Explicit exclusions

Не импортируются:

- F9 / прежние FR-96–FR-110;
- comparative Candidate N/C decision lattice;
- outcomes `profile/upstream/stop` как регулярный выбор направления;
- запрет Architecture до старого Gate 1;
- старые OQ sequencing и общий `No Start` для AA-1;
- RTTSI/arm-specific measurement rules, кроме явно adapted общих правил reproducibility, safety, evidence и resource discipline.

## Change control

Изменение сохранённого смысла требует нового versioned baseline, точного diff, affected independent reviews и conscious Project Owner acceptance. Нельзя обновить source или digest молча.
