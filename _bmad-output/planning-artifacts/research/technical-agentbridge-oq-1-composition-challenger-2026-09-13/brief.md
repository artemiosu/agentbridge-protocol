---
title: 'OQ-1 Composition Challenger selection brief'
type: technical
shape: select
status: approved
created: '2026-09-13'
---

# OQ-1 Composition Challenger selection

## Decision

Выбрать 1–3 Pareto-недоминируемые, реально разворачиваемые конфигурации существующих публичных стандартов и один Primary C для честного Gate 1 сравнения с гипотезой Native Core.

## Hard gates

- точные свежие версии и публичные нормативные материалы;
- независимая реализация и воспроизводимое развёртывание возможны без закрытой помощи;
- общий mandatory scope не сужается по желанию кандидата;
- обязательные components, glue, state, trust/data boundaries и failure surface раскрыты;
- нет hidden pair-specific semantics или обязательного proprietary runtime/service;
- лицензии/IP допускают Gate 1 реализацию и проверку;
- отсутствие функции учитывается как gap/claim limitation, а не `not-applicable` по выбору кандидата.

## Weighted criteria after hard gates

| Criterion | Weight |
| --- | ---: |
| Покрытие candidate-neutral invariants | 35% |
| Безопасность, authority, consent и effects | 25% |
| Topology/mode/lifecycle breadth | 15% |
| Independent deployability, glue и operational complexity | 15% |
| Evolution, governance и ecosystem durability | 10% |

## Dimensions

1. Текущий landscape, точные версии, normative status и maturity.
2. Семантическое покрытие EI-01–EI-25 и обязательные пробелы композиции.
3. Deployability, glue/state/trust/dependency/IP inventory.
4. Pareto screen, Primary C, runner-up conditions и reversibility hedge.
5. Независимая verification и red-team strongest counterargument.

## Method

- Native web run; technical pack + selection shape.
- Breadth-first fan-out, три исследовательских потока, до двух раундов.
- Только первичные официальные спецификации, репозитории, release notes и standards-body materials для выводов.
- High validation для решающих version/compatibility/comparison claims; red-team финального выбора.
- Existing Gate 0 research задаёт вопросы, но не является evidence OQ-1.
