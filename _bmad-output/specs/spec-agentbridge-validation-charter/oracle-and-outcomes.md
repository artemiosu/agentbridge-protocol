# Candidate-neutral oracle and outcome contract — OQ-3A proposal

Статус: **proposal-ready; not accepted; not frozen**.

Oracle определяет допустимый смысл результата до появления моделей Candidate N/C. Он не задаёт названия сообщений, wire format, язык, transport, SDK API или внутреннюю архитектуру.

## 1. Oracle record

Каждый vector получает один versioned Oracle Record со следующими обязательными полями.

| Поле | Допустимое содержание | Правило |
| --- | --- | --- |
| `oracle_id/version` | неизменяемый ID и версия | Изменение смысла создаёт новую версию и новую preregistration |
| `scope` | EI/FR/NFR, VC, topology, mode, domain/Profile и claim boundary | Результат нельзя переносить за scope |
| `preconditions` | роли, dependency closure, trust/privacy boundaries, initial state | Неявная предпосылка запрещена |
| `stimulus_schedule` | входы, logical steps, interleavings и faults | Реальные часы не считаются глобальным порядком |
| `allowed_set` | один или несколько допустимых Outcome Tuples | Вариативность разрешена только явно |
| `forbidden_set` | запрещённые tuples, disclosures, effects и transitions | Запрещённый эффект блокирует pass независимо от удобного status |
| `observer_contract` | OBS IDs, channels, detection window и completeness condition | Неполное наблюдение даёт `invalid`, не pass |
| `resource_privacy_bounds` | ссылки на применимые frozen bounds | Выход за bound — нормативный результат, а не сбой измерения |
| `metamorphic_links` | MP IDs и связанные vectors | Проверяется отношение результатов, не только один пример |
| `applicability` | одинаковое правило включения/исключения для N и C | Исключение сужает claim и требует причины |
| `evidence` | expected artifacts, digest и provenance | Результат без обязательного evidence invalid |
| `authorship` | oracle author, independent blind cross-checker, decision owner | Security-critical oracle без cross-check invalid/inconclusive |

## 2. Outcome Tuple

Oracle сравнивает кандидатов по многомерному tuple; один общий `success/error` запрещён.

| Измерение | Нормативные классы |
| --- | --- |
| `run_validity` | `valid`, `invalid` |
| `compatibility` | `compatible`, `incompatible`, `unknown`, `not-applicable` |
| `authority` | `permit`, `non-permit`, `unknown`, `not-applicable` |
| `progress` | `continue`, `block`, `terminate`, `await`, `not-applicable` |
| `delivery` | `not-sent`, `sent`, `delivered`, `unknown`, `not-applicable` |
| `execution` | `not-started`, `running`, `completed`, `failed`, `unknown`, `not-applicable` |
| `effect` | `established-none`, `full`, `partial`, `unknown`, `not-applicable` |
| `obligations` | `pending`, `satisfied`, `failed`, `unknown`, `not-applicable` |
| `evidence_assurance` | элемент явно заданного Profile/Binding partial order либо `unknown`; абсолютная истина запрещена |
| `disclosures/effects` | точные наблюдаемые множества с actor, subject, audience, resource и epoch |
| `diagnostic_projection` | минимально необходимая диагностическая категория в разрешённой privacy-области |
| `resource_projection` | измеренные bounded consumption и safe-stop outcome |

`invalid` описывает непригодность запуска или наблюдения и не является безопасным результатом кандидата. `unknown` сохраняет неопределённость и не доказывает `established-none`. `permit` сам по себе не доказывает execution/effect.

### Cross-field consistency

- `authority != permit` запрещает новую Protected Disclosure или Consequential Action в применимой boundary.
- `compatibility = incompatible/unknown` по обязательной семантике блокирует зависимый обмен.
- `effect = established-none` допустим только при полном и откалиброванном observer contract; иначе `unknown` либо `invalid`.
- `run_validity = invalid` исключает candidate pass/fail inference для затронутого cell, но сохраняет evidence причины.
- `partial/unknown` не нормализуются в `full/completed/success`.
- Fault, bridge, Evidence gap или lower-assurance input не могут повысить permission, certainty либо assurance без frozen normative rule.

## 3. Global safety projection

Для каждого tuple вычисляется отдельная safety projection:

- был ли explicit permit на каждой новой Protected Disclosure/effect boundary;
- совпал ли фактический Decision Subject и security epoch с разрешённым;
- были ли forbidden disclosure/effect, authority amplification, double-spend, optimistic conflict resolution, assurance inflation или security-critical Semantic Loss;
- полон ли observer contract;
- были ли превышены privacy/resource bounds.

Любое значение `true` для запрещённого события либо `observer_complete=false` исключает pass. Средний балл, производительность или удобная диагностика это не компенсируют.

## 4. Metamorphic properties

Metamorphic property задаёт отношение между связанными vectors и ловит ошибку, даже когда единственный «правильный ответ» допускает вариативность.

| ID | Преобразование входа | Обязательное отношение результатов |
| --- | --- | --- |
| MP-01 | Переименовать agent/service/B2B и поменять transport client/server positions без смены ролей | Allowed set и safety projection неизменны |
| MP-02 | Заменить Endpoint или заявленный взаимозаменяемый provider | Смысл неизменен в заявленном scope; зависимый outage видим |
| MP-03 | Сузить Authority | Новый результат не даёт больше disclosures/effects, чем исходный |
| MP-04 | Изменить security-critical Decision Subject за разрешённым диапазоном | Старые Consent/Approval/Authority не дают permit |
| MP-05 | Повторить тот же Operation/retry/replay | Число consequential commits не увеличивается |
| MP-06 | Добавить timeout, loss, reorder, crash или неизвестность | Permission, certainty и assurance не повышаются |
| MP-07 | Добавить unknown Mandatory-to-understand либо снизить negotiated floor | Зависимый обмен не переходит из block/incompatible в continue |
| MP-08 | Уменьшить разрешённую privacy-boundary | Disclosure set не расширяется; невозможность соблюсти bound блокирует зависимый шаг |
| MP-09 | Переставить Binding layers | Результат неизменен только при заявленной коммутативности; иначе несовместимость/loss видимы |
| MP-10 | Добавить независимую ветвь вне dependency closure | Смысл и данные существующей ветви не меняются и не раскрываются новой |
| MP-11 | Добавить, удалить или redacted Evidence link | Causal gap остаётся типизированным; assurance не повышается |
| MP-12 | Предложить более старую/слабую версию после сильной | Silent downgrade не принимается |
| MP-13 | Добавить bridge hop | Semantic Loss и assurance могут сохраниться или ухудшиться, но не скрытно улучшиться |
| MP-14 | Увеличить hostile size/depth/rate до и за frozen ceiling | До ceiling смысл сохраняется; на boundary возникает определённый safe-stop без unbounded work |
| MP-15 | Добавить конфликтующий Lifecycle/Effect Claim | Итог не становится определённее без frozen trust/Profile rule |
| MP-16 | Заменить одного approver identity alias тем же controller/root | Quorum/separation-of-duty не становится выполненным |
| MP-17 | Сменить domain labels при сохранении абстрактных ролей/рисков | Общий Core claim и expected safety projection неизменны |
| MP-18 | Заменить hidden pair-specific mapping на одинаково доступный versioned normative mapping | Допустим только отдельный scoped bridge/comparability claim; общий claim не расширяется автоматически |

## 5. EI coverage contract

Каждый frozen EI получает минимум positive, negative и metamorphic coverage. Полная детализация vectors создаётся в OQ-3B.

| EI | Основные VC | MP | Обязательные observers |
| --- | --- | --- | --- |
| EI-01 | VC-1, VC-4–VC-6 | MP-01, MP-17 | OBS-01, OBS-02, OBS-08 |
| EI-02 | VC-1, VC-10, VC-12 | MP-06, MP-07, MP-12 | OBS-01, OBS-06, OBS-09 |
| EI-03 | VC-1, VC-8, VC-10 | MP-02, MP-07, MP-12 | OBS-01, OBS-02, OBS-10 |
| EI-04 | VC-3, VC-6, VC-7, VC-9 | MP-01, MP-06, MP-10 | OBS-01, OBS-07, OBS-08 |
| EI-05 | VC-2, VC-4, VC-5, VC-12 | MP-04 | OBS-02, OBS-04, OBS-09 |
| EI-06 | VC-2, VC-4–VC-6, VC-8, VC-11, VC-12 | MP-03, MP-04, MP-06 | OBS-02–OBS-04, OBS-09 |
| EI-07 | VC-2, VC-4, VC-6 | MP-04, MP-16 | OBS-02, OBS-04, OBS-09 |
| EI-08 | VC-2–VC-4, VC-12 | MP-04, MP-06 | OBS-02, OBS-04, OBS-07 |
| EI-09 | VC-5, VC-6, VC-12 | MP-03, MP-05, MP-06 | OBS-02, OBS-04, OBS-07 |
| EI-10 | VC-2–VC-4, VC-6, VC-7, VC-12 | MP-06, MP-15 | OBS-01, OBS-04, OBS-08 |
| EI-11 | VC-2, VC-3, VC-7, VC-12 | MP-05, MP-06 | OBS-04, OBS-07, OBS-08 |
| EI-12 | VC-3, VC-6, VC-7, VC-12 | MP-06, MP-15 | OBS-04, OBS-07–OBS-09 |
| EI-13 | VC-2–VC-4, VC-7, VC-8, VC-11 | MP-11, MP-13, MP-15 | OBS-04, OBS-08, OBS-09 |
| EI-14 | VC-2, VC-4, VC-10, VC-13 | MP-17, MP-18 | OBS-01, OBS-09, OBS-11 |
| EI-15 | VC-1, VC-10, VC-12 | MP-07, MP-12 | OBS-01, OBS-06, OBS-10 |
| EI-16 | VC-1, VC-8, VC-10, VC-12 | MP-02, MP-09, MP-12 | OBS-03, OBS-06, OBS-10 |
| EI-17 | VC-8, VC-11, VC-12 | MP-09, MP-13, MP-18 | OBS-03, OBS-04, OBS-09, OBS-10 |
| EI-18 | VC-5–VC-7, VC-12 | MP-10, MP-16 | OBS-02, OBS-04, OBS-07–OBS-09 |
| EI-19 | VC-5, VC-6, VC-8, VC-9, VC-12, VC-13 | MP-08, MP-10, MP-11 | OBS-03, OBS-06, OBS-11 |
| EI-20 | VC-7–VC-10, VC-12 | MP-06, MP-14 | OBS-05–OBS-08, OBS-10 |
| EI-21 | VC-1, VC-2, VC-5, VC-8 | MP-02 | OBS-06, OBS-10, OBS-12 |
| EI-22 | VC-1–VC-13 | MP-01–MP-18 as applicable | OBS-01–OBS-12 |
| EI-23 | VC-8, VC-10, VC-11 | MP-02, MP-07, MP-12, MP-13 | OBS-01, OBS-09, OBS-10, OBS-12 |
| EI-24 | VC-13 | MP-17, MP-18 | OBS-01, OBS-09, OBS-11 |
| EI-25 | Все применимые VC | MP-06, MP-14, MP-15 | OBS-05–OBS-07, OBS-09–OBS-12 |

## 6. Oracle authorship and adjudication

1. Oracle Author выводит allowed/forbidden sets только из frozen EI и PRD.
2. Blind Cross-checker получает requirement scope и vector, но не candidate outputs или implementation.
3. Разногласие разрешается до candidate design. Неустранённая неоднозначность превращает vector в exploratory либо сужает claim; она не получает произвольный expected outcome.
4. Security-critical record без независимого cross-check имеет `run_validity=invalid` для confirmatory evidence.
5. После freeze изменение allowed set, forbidden set, observer completeness или metamorphic relation создаёт новую oracle version; прежние результаты не пересчитываются.

## 7. OQ-3A acceptance criteria

- Outcome Tuple не предполагает объектов Candidate N/C и различает status, authority, execution, effect и evidence.
- Каждый EI-01–EI-25 имеет positive, negative, metamorphic и observer coverage route.
- Любой forbidden effect или неполный observer не может стать pass.
- Разрешённая вариативность задана allowed set, а не Reference Implementation.
- Security-critical outcomes требуют независимого blind cross-check.
- Project Owner осознанно принимает финальную редакцию после независимого review.
