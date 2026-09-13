# Open decisions — plain-language register

Пока хотя бы одна строка открыта, Charter имеет статус **No Start**. Рекомендации ниже — отправная точка для осознанного решения, а не уже принятое решение.

| Порядок | ID | Что решаем простыми словами | Почему это важно | Рекомендуемый следующий шаг | Owner | Статус |
| ---: | --- | --- | --- | --- | --- | --- |
| 1 | OQ-2A | Какие общие правила обязан сохранять любой вариант | Без этого тест можно незаметно подстроить под придуманную архитектуру | EI-01–EI-25 прошли blind review и повторно приняты Project Owner; любое смысловое изменение требует нового review | Gate Chair | **frozen 2026-09-13** |
| 2 | OQ-2B | Какие требования после принятия Ledger действительно относятся к Native Core | Если решить это раньше, Native может сам определить критерии своей победы | После OQ-2A и candidate-neutral части OQ-3 проверить все proposed allocations и выполнить Removal Tests | Gate Chair + Native Advocate | blocked by OQ-2A/OQ-3 |
| 3 | OQ-3 | Какие конкретные испытания и правильные ответы докажут или опровергнут правила | Красивое demo может пропустить опасные ошибки | Развернуть VC-1–VC-13 в vectors, observers и sealed holdout до candidate design | Conformance Lead + Security Reviewer | open |
| 4 | OQ-1 | С какими сильнейшими существующими решениями честно сравнивать Native | Слабый соперник даст ложную победу Native | Обновить публичные первичные источники и по selection protocol заморозить 1–3 C и Primary C | Challenger Advocate + Gate Chair | open |
| 5 | OQ-4 | Какие ошибки автоматически запрещают положительный результат | Иначе опасную ошибку можно назвать «средней» и проигнорировать | Утвердить immutable Safety Blocking Class, observer rules и residual-risk process | Security Reviewer + Project Owner | open |
| 6 | OQ-5 | Что означает «практически лучше» и в каких условиях измерять | Без порогов победителя можно выбрать после просмотра цифр | Утвердить primary/secondary endpoints, два cost estimands, environments, margins и SAP | Gate Chair + Project Owner | open |
| 7 | OQ-6 | Кто независимо проектирует, реализует и проверяет варианты | Одна и та же сторона может неосознанно помочь своему варианту | Назначить роли, зафиксировать exposure и помощь, определить blind adjudication | Gate Chair | open |
| 8 | OQ-7 | Сколько времени и ресурсов можно потратить | Без границы исследование может стать бесконечным; слишком маленький бюджет испортит доказательства | Задать бюджет по этапам и pause/re-scope rules без удаления hard gates | Project Owner + Gate Chair | open |
| 9 | OQ-8 | Какие зависимости можно использовать и что можно хранить/публиковать | Ошибка может раскрыть приватные материалы или сделать результат юридически непригодным | Проверить licenses/IP, storage boundaries и отдельный publication checklist | Evidence Custodian + Project Owner | open |

## Рекомендуемая последовательность работы

1. **Завершено:** OQ-2A закрыт на уровне candidate-neutral Ledger без проектирования Candidate N.
2. **Следующий шаг:** закрыть candidate-neutral часть OQ-3 — oracle schema, scenario templates и holdout rules.
3. Провести freshness review и закрыть OQ-1.
4. Закрыть OQ-4–OQ-8 и сформировать полный Charter package.
5. Провести независимые methodology, security, architecture и fairness reviews.
6. Провести Expert Advisory Checkpoint и объяснить рекомендацию Project Owner простым языком.
7. Только после осознанного `proceed` и полного freeze разрешить экспериментальный код.

OQ-2A frozen: EI-01–EI-25 прошли финальный blind review с 0 Critical/High и повторно приняты Project Owner. Allocation Matrix уже предложена, но её Core/Profile/Binding решения остаются неблокирующими гипотезами до OQ-2B.
