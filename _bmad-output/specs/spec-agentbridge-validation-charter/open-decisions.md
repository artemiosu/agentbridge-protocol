# Open decisions — plain-language register

Пока хотя бы одна строка открыта, Charter имеет статус **No Start**. Рекомендации ниже — отправная точка для осознанного решения, а не уже принятое решение.

| Порядок | ID | Что решаем простыми словами | Почему это важно | Рекомендуемый следующий шаг | Owner | Статус |
| ---: | --- | --- | --- | --- | --- | --- |
| 1 | OQ-2A | Какие общие правила обязан сохранять любой вариант | Без этого тест можно незаметно подстроить под придуманную архитектуру | EI-01–EI-25 прошли blind review и повторно приняты Project Owner; любое смысловое изменение требует нового review | Gate Chair | **frozen 2026-09-13** |
| 2 | OQ-3A | По каким заранее известным правилам строить и оценивать испытания | Иначе правильный ответ можно подобрать после просмотра кандидата | Все 12 findings закрыты, blind re-review PASS (0 Critical/High), исправленная редакция повторно принята Project Owner | Conformance Lead + Security Reviewer | **frozen 2026-09-13** |
| 3 | OQ-1 | С какими сильнейшими существующими решениями честно сравнивать Native | Слабый соперник даст ложную победу Native | C1 принят как Primary C; C2 — GNAP challenger; ANP — reserve; AGNTCY — bounded infrastructure experiment; manifest OQ1-M1 | Challenger Advocate + Gate Chair | **frozen 2026-09-13** |
| 4 | OQ-4 | Какие ошибки автоматически запрещают положительный результат | Иначе опасную ошибку можно назвать «средней» и проигнорировать | Утвердить immutable Safety Blocking Class, observer rules и residual-risk process | Security Reviewer + Project Owner | open |
| 5 | OQ-5 | Что означает «практически лучше» и в каких условиях измерять | Без порогов победителя можно выбрать после просмотра цифр | Утвердить primary/secondary endpoints, два cost estimands, environments, margins и SAP | Gate Chair + Project Owner | open |
| 6 | OQ-6 | Кто независимо проектирует, реализует и проверяет варианты | Одна и та же сторона может неосознанно помочь своему варианту | Назначить роли, зафиксировать exposure и помощь, определить blind adjudication | Gate Chair | open |
| 7 | OQ-7 | Сколько времени и ресурсов можно потратить | Без границы исследование может стать бесконечным; слишком маленький бюджет испортит доказательства | Задать бюджет по этапам и pause/re-scope rules без удаления hard gates | Project Owner + Gate Chair | open |
| 8 | OQ-8 | Какие зависимости можно использовать и что можно хранить/публиковать | Ошибка может раскрыть приватные материалы или сделать результат юридически непригодным | Проверить licenses/IP, storage boundaries и отдельный publication checklist | Evidence Custodian + Project Owner | open |
| 9 | OQ-3B | Какие точные общие и запечатанные задания войдут в экзамен | Одних шаблонов недостаточно для запуска проверки | После OQ-4/OQ-5/OQ-8 создать exact corpus, независимо проверить ответы и seal holdout до Candidate design | Conformance Lead + Evidence Custodian | blocked by OQ-4/OQ-5/OQ-8 |
| 10 | OQ-2B | Какие требования после принятия Ledger действительно относятся к Native Core | Если решить это раньше, Native может сам определить критерии своей победы | После полного OQ-3B проверить proposed allocations и выполнить Removal Tests | Gate Chair + Native Advocate | blocked by OQ-3B |

## Рекомендуемая последовательность работы

1. **Завершено:** OQ-2A, OQ-3A и OQ-1 заморожены без проектирования Candidate N/C.
2. **Текущий шаг:** закрыть OQ-4 — неизменяемые ошибки безопасности, автоматически запрещающие положительный результат.
3. Закрыть OQ-5–OQ-8, затем полный OQ-3B и только после этого OQ-2B/Candidate design; сформировать полный Charter package.
4. Провести независимые methodology, security, architecture и fairness reviews.
5. Провести Expert Advisory Checkpoint и объяснить рекомендацию Project Owner простым языком.
6. Только после осознанного `proceed` и полного freeze разрешить экспериментальный код.

OQ-2A, OQ-3A и OQ-1 frozen. OQ-1 после независимых проверок зафиксировал C1/C2, ANP reserve и ограниченный AGNTCY experiment, не выбрав архитектуру AgentBridge. Exact corpus остаётся OQ-3B; Allocation Matrix — гипотезой до OQ-2B. Charter и любой code сохраняют No Start.
