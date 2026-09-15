# Open decisions — plain-language register

Пока хотя бы одна строка открыта, Charter имеет статус **No Start**. Рекомендации ниже — отправная точка для осознанного решения, а не уже принятое решение.

| Порядок | ID | Что решаем простыми словами | Почему это важно | Рекомендуемый следующий шаг | Owner | Статус |
| ---: | --- | --- | --- | --- | --- | --- |
| 1 | OQ-2A | Какие общие правила обязан сохранять любой вариант | Без этого тест можно незаметно подстроить под придуманную архитектуру | EI-01–EI-25 прошли blind review и повторно приняты Project Owner; любое смысловое изменение требует нового review | Gate Chair | **frozen 2026-09-13** |
| 2 | OQ-3A | По каким заранее известным правилам строить и оценивать испытания | Иначе правильный ответ можно подобрать после просмотра кандидата | Все 12 findings закрыты, blind re-review PASS (0 Critical/High), исправленная редакция повторно принята Project Owner | Conformance Lead + Security Reviewer | **frozen 2026-09-13** |
| 3 | OQ-1 | С какими сильнейшими существующими решениями честно сравнивать Native | Слабый соперник даст ложную победу Native | C1 принят как Primary C; C2 — GNAP challenger; ANP — reserve; AGNTCY — bounded infrastructure experiment; manifest OQ1-M1 | Challenger Advocate + Gate Chair | **frozen 2026-09-13** |
| 4 | OQ-4 | Какие ошибки автоматически запрещают положительный результат | Иначе опасную ошибку можно назвать «средней» и проигнорировать | SBC-01–SBC-10, High severity threshold, attribution, residual-risk и disclosure rules приняты; exact operational slots закрываются в OQ-3B/OQ-5/OQ-6/OQ-8 без права ослабить policy | Security Reviewer + Project Owner | **frozen 2026-09-13** |
| 5 | OQ-5A | Что означает «практически лучше» и в каких условиях измерять | Без порогов победителя можно выбрать после просмотра цифр | RTTSI, necessity-first, 20% boundary, completion gate, challenger roles, guardrails, ceilings и statistics приняты после трёх independent reviews; manifest OQ5A-M1 | Gate Chair + Project Owner | **frozen 2026-09-13** |
| 5b | OQ-5B | Какие точные N/τ, команды, budgets, cells и schedules делают OQ-5A исполнимой | Без operational freeze проверку нельзя воспроизвести или профинансировать | После OQ-6/OQ-7 и neutral pilot заполнить exact slots и атомарно включить их в OQ-3B | Gate Chair + Project Owner | blocked by OQ-6/OQ-7 |
| 6 | OQ-6A | Кто что может создавать, видеть и решать | Одна и та же сторона может неосознанно помочь своему варианту | R-01–R-16, incompatibilities, access, assistance, adjudication, contamination и I0–I3 приняты после трёх independent reviews; manifest OQ6A-M1 | Gate Chair + Project Owner | **frozen 2026-09-13** |
| 6b | OQ-6B | Какие реальные identities и изолированные access domains займут роли | Политика без технического разделения не доказывает независимость | После OQ-7/OQ-8 назначить roster, I2 controllers/stores, access tests и attestations | Gate Chair | blocked by OQ-7/OQ-8 |
| 7 | OQ-7A | Какие бюджетные ступени и правила остановки действуют | Без границы исследование может стать бесконечным; слишком маленький бюджет испортит доказательства | B0–B3, A0/A1, funding firewall и pause rules приняты после трёх независимых PASS; cash authorization остаётся USD 0 | Project Owner + Gate Chair | **frozen 2026-09-14** |
| 7b | OQ-7B-Pilot | Какой точный лимит безопасно разрешает neutral pilot | Пилот сам требует денег, независимых ролей и incident reserve | После OQ-8 заморозить quotes, cap, approvers и обеспеченный closure reserve вместе с OQ-6B-Pilot | Project Owner + independent budget/governance approver | blocked by OQ-8 |
| 7c | OQ-7B-Confirmatory | Какие точные лимиты делают полный Gate 1 реально оплачиваемым | Оценки до pilot слишком неточны для обязательства | После pilot заморозить N/τ, все режимы/cells, compute/storage/retention и обязательные reserves вместе с OQ-5B/OQ-6B-Confirmatory | Project Owner + independent budget/governance approver | blocked by pilot |
| 8 | OQ-8A | Какие правила прав, хранения, disclosure и публикации нельзя ослаблять | Ошибка может раскрыть приватные материалы или сделать результат юридически непригодным | Rights/IPR, P0–P4, disclosure clocks, anti-capture и release rules приняты после трёх независимых PASS | Independent reviewers + Evidence Custodian + Project Owner | **frozen 2026-09-15** |
| 8b | OQ-8B-Pilot | Какие точные права, договоры и хранилища разрешают neutral pilot | Общая policy не доказывает право на конкретный файл или безопасность конкретного store | До pilot заморозить exact ledger вместе с OQ-7B-Pilot/OQ-6B-Pilot | Evidence Custodian + independent IP/Security reviewers | blocked by OQ-8A |
| 8c | OQ-8B-Confirmatory | Какие точные права и boundaries действуют для полного Gate 1 | Версии, зависимости, участники и evidence stores меняют юридический и privacy-риск | До OQ-3B/Candidate exposure заморозить полный ledger/BOM/agreements/storage manifest | Evidence Custodian + independent IP/Security reviewers | blocked by pilot/candidate closure |
| 9 | OQ-3B | Какие точные общие и запечатанные задания войдут в экзамен | Одних шаблонов недостаточно для запуска проверки | После OQ-5/OQ-8 создать exact corpus, независимо проверить ответы и seal holdout до Candidate design | Conformance Lead + Evidence Custodian | blocked by OQ-5/OQ-8 |
| 10 | OQ-2B | Какие требования после принятия Ledger действительно относятся к Native Core | Если решить это раньше, Native может сам определить критерии своей победы | После полного OQ-3B проверить proposed allocations и выполнить Removal Tests | Gate Chair + Native Advocate | blocked by OQ-3B |

## Рекомендуемая последовательность работы

1. **Завершено:** OQ-2A, OQ-3A и OQ-1 заморожены без проектирования Candidate N/C.
2. **Завершено:** OQ-4 safety policy заморожена после независимого review; операционные параметры обязаны реализовать её без ослабления.
3. **Завершено:** OQ-5A measurement policy заморожена; OQ-5B operational values остаются blocked до OQ-6/OQ-7/neutral pilot.
4. **Завершено:** OQ-6A independence policy заморожена; OQ-6B-Pilot/Confirmatory остаются blocked до OQ-7/OQ-8.
5. **Завершено:** OQ-7A заморожена после трёх независимых PASS; денежные расходы по-прежнему не разрешены.
6. **Завершено:** OQ-8A заморожена после трёх независимых PASS; никакие внешние действия или публикация не разрешены.
7. **Текущий шаг:** провести ранний A0 advisory; OQ-8B-Pilot/OQ-7B-Pilot/OQ-6B-Pilot предшествуют neutral pilot, а confirmatory freezes — OQ-3B/OQ-2B и формальному A1.
7. Провести независимые methodology, security, architecture и fairness reviews.
8. Провести Expert Advisory Checkpoint и объяснить рекомендацию Project Owner простым языком.
9. Только после осознанного `proceed` и полного freeze разрешить экспериментальный код.

OQ-2A, OQ-3A, OQ-1, OQ-4, OQ-5A, OQ-6A, OQ-7A и OQ-8A frozen. OQ-1 после независимых проверок зафиксировал C1/C2, ANP reserve и ограниченный AGNTCY experiment, не выбрав архитектуру AgentBridge. Exact OQ-8B, corpus и Allocation Matrix остаются открыты. Charter и любой code сохраняют No Start.
