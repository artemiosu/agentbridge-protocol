# Open decisions — plain-language register

Пока хотя бы одна строка открыта, Charter имеет статус **No Start**. Рекомендации ниже — отправная точка для осознанного решения, а не уже принятое решение.

| Порядок | ID | Что решаем простыми словами | Почему это важно | Рекомендуемый следующий шаг | Owner | Статус |
| ---: | --- | --- | --- | --- | --- | --- |
| 1 | OQ-2A | Какие общие правила обязан сохранять любой вариант | Без этого тест можно незаметно подстроить под придуманную архитектуру | EI-01–EI-25 прошли blind review и повторно приняты Project Owner; любое смысловое изменение требует нового review | Gate Chair | **frozen 2026-09-13** |
| 2 | OQ-3A | По каким заранее известным правилам строить и оценивать испытания | Иначе правильный ответ можно подобрать после просмотра кандидата | Все 12 findings закрыты, blind re-review PASS (0 Critical/High); требуется повторное осознанное принятие Project Owner | Conformance Lead + Security Reviewer | awaiting owner re-acceptance |
| 3 | OQ-1 | С какими сильнейшими существующими решениями честно сравнивать Native | Слабый соперник даст ложную победу Native | Обновить публичные первичные источники и по selection protocol заморозить 1–3 C и Primary C | Challenger Advocate + Gate Chair | open |
| 4 | OQ-4 | Какие ошибки автоматически запрещают положительный результат | Иначе опасную ошибку можно назвать «средней» и проигнорировать | Утвердить immutable Safety Blocking Class, observer rules и residual-risk process | Security Reviewer + Project Owner | open |
| 5 | OQ-5 | Что означает «практически лучше» и в каких условиях измерять | Без порогов победителя можно выбрать после просмотра цифр | Утвердить primary/secondary endpoints, два cost estimands, environments, margins и SAP | Gate Chair + Project Owner | open |
| 6 | OQ-6 | Кто независимо проектирует, реализует и проверяет варианты | Одна и та же сторона может неосознанно помочь своему варианту | Назначить роли, зафиксировать exposure и помощь, определить blind adjudication | Gate Chair | open |
| 7 | OQ-7 | Сколько времени и ресурсов можно потратить | Без границы исследование может стать бесконечным; слишком маленький бюджет испортит доказательства | Задать бюджет по этапам и pause/re-scope rules без удаления hard gates | Project Owner + Gate Chair | open |
| 8 | OQ-8 | Какие зависимости можно использовать и что можно хранить/публиковать | Ошибка может раскрыть приватные материалы или сделать результат юридически непригодным | Проверить licenses/IP, storage boundaries и отдельный publication checklist | Evidence Custodian + Project Owner | open |
| 9 | OQ-3B | Какие точные общие и запечатанные задания войдут в экзамен | Одних шаблонов недостаточно для запуска проверки | После OQ-3A/OQ-4/OQ-5/OQ-8 создать exact corpus, независимо проверить ответы и seal holdout до Candidate design | Conformance Lead + Evidence Custodian | blocked by OQ-3A/OQ-4/OQ-5/OQ-8 |
| 10 | OQ-2B | Какие требования после принятия Ledger действительно относятся к Native Core | Если решить это раньше, Native может сам определить критерии своей победы | Только после OQ-1 и полного OQ-3B проверить proposed allocations и выполнить Removal Tests | Gate Chair + Native Advocate | blocked by OQ-1/OQ-3B |

## Рекомендуемая последовательность работы

1. **Завершено:** OQ-2A закрыт на уровне candidate-neutral Ledger без проектирования Candidate N.
2. **Текущий шаг:** принять и независимо проверить OQ-3A — правила построения и оценки экзамена.
3. Провести freshness review и закрыть OQ-1.
4. Закрыть OQ-4–OQ-8, затем полный OQ-3B и только после этого OQ-2B/Candidate design; сформировать полный Charter package.
5. Провести независимые methodology, security, architecture и fairness reviews.
6. Провести Expert Advisory Checkpoint и объяснить рекомендацию Project Owner простым языком.
7. Только после осознанного `proceed` и полного freeze разрешить экспериментальный код.

OQ-2A frozen. Исправленная OQ-3A закрыла все 12 findings и прошла blind re-review с 0 Critical/High; она содержит total verdict, atomic coverage inventory, 24 metamorphic properties, synthetic grammar, 12 observer contracts и усиленное правило 39-case sealed holdout. До повторного принятия Project Owner OQ-3A не frozen. Exact corpus остаётся OQ-3B; Allocation Matrix — гипотезой до OQ-2B.
