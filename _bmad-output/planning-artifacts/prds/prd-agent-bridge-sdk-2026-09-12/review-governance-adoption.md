# Независимая проверка PRD: governance, adoption и коммерческая нейтральность

**Артефакт:** `prd.md`  
**Дата проверки:** 2026-09-13  
**Роль reviewer:** независимый эксперт по открытым интернет-стандартам, governance, ecosystem adoption, OSS sustainability и коммерческой нейтральности  
**Область:** путь от технической валидации к легитимному открытому стандарту; IP/RF, change control, conformance claims/marks, независимые реализации, защита от capture, protocol/platform boundary, bridges/migration, adoption evidence и Gates 2–5

## Вердикт

**Условно принять после обязательной корректировки governance/adoption handoff.**

PRD уже содержит сильный и необычно дисциплинированный фундамент: открытая royalty-free нормативная основа (`NFR-27`), независимый self-conformance (`FR-83`), ограниченные и отзываемые claims (`FR-82`, `FR-95`, `NFR-29`), спецификация выше reference implementation (`FR-93`), outcome-neutral Gate 1 (`FR-109`–`FR-110`), separation технического, adoption, demand и revenue evidence (§15.1), а также явный запрет protocol toll и pay-to-pass (§15.3).

Однако эти принципы пока не полностью превращены в проверяемые условия Gates 2–5. Главные остаточные риски — позднее оформление IP/contribution commitments, отсутствие минимального исполнимого change-control charter и отсутствие структурного firewall между стандартом и будущими коммерческими активами. Эти пробелы **не блокируют Gate 1 и подготовку Validation Charter**, но должны стать обязательными условиями до публичного нормативного draft, внешних contributions/implementations и любых ecosystem claims.

## Сводка по серьёзности

| Уровень | Количество |
| --- | ---: |
| Критический | 3 |
| Высокий | 8 |
| Средний | 3 |
| **Всего** | **14** |

## Findings

### GOV-01 — IP/RF commitments возникают слишком поздно

**Уровень:** Критический  
**Где:** `NFR-27`; `FR-83`; §14.2 `DQ-2`; §15 Gate 2–3

**Условие:** `NFR-27` требует точную лицензию, IP contribution policy и trademark rules только до публичного нормативного release, а `DQ-2` оставляет формальную IP/governance policy на Gate 2–3. При этом Gate 3 уже предполагает внешних реализаторов по публичным материалам. Публичный draft, issue, test vector или contribution до появления правил может внести неясные авторские и патентные права, которые невозможно надёжно исправить задним числом.

**Ограниченная правка:** сделать до первого публичного normative draft и до принятия первого внешнего contribution обязательными: лицензию спецификации и conformance-материалов с правом независимой реализации, копирования и распространения; contribution terms; раскрытие существенных патентных притязаний; применимое RF/non-assert обязательство без field-of-use ограничений; правила происхождения contributions. Полная организация или standards body может оставаться отложенной.

**Последствие без правки:** внешняя реализация может получить юридическую зависимость от владельца IP либо несовместимые права, а заявление `royalty-free` останется обещанием, а не гарантией.

### GOV-02 — Neutral change control не определён как исполнимый процесс

**Уровень:** Критический  
**Где:** `FR-59`, `FR-66`, `FR-67`, `FR-94`; §14.2 `DQ-2`; §15 Gate 3–4

**Условие:** Gate 4 требует «neutral change control», но PRD не устанавливает минимальных признаков нейтральности. Gate 3 лишь разрешает подготовку governance. Один владелец может контролировать normative text, test verdicts, extensions и выпуск версий, формально оставляя материалы открытыми.

**Ограниченная правка:** сделать минимальный governance charter условием входа в Gate 3, а проверку его работы — условием Gate 4. Минимум: публичные предложения и нормативные diff; журнал решений и dissent; критерии участия; conflict disclosure/recusal; правила quorum/consensus или иной заранее объявленной decision procedure; апелляция; неизменность опубликованных версий; отдельный emergency-security path; запрет одному vendor менять и норму, и единственный oracle в свою пользу.

**Последствие без правки:** AgentBridge может стать open-source продуктом одного поставщика, но не легитимным открытым стандартом.

### GOV-03 — Firewall между протоколом и коммерческой платформой декларативен

**Уровень:** Критический  
**Где:** `NFR-7`, `NFR-28`; `R-11`, `R-17`; §15 Gate 4–5; §15.3

**Условие:** PRD запрещает обязательные cloud, certification, marketplace, payments и advertising, но не ограничивает совмещённый контроль над нормативным репозиторием, suite, trademarks, namespace, signing keys, registry/discovery defaults и коммерческими продуктами. Владелец может не вводить формальную плату за протокол, но создать фактическое преимущество своей платформы через управление essential assets или roadmap.

**Ограниченная правка:** к Gate 4 потребовать проверяемый protocol/platform firewall: нормативные решения и conformance verdicts отделены от продуктовой выручки; существенное финансирование и конфликты раскрыты; essential artifacts и namespaces доступны недискриминационно; нет обязательного platform account, эксклюзивного ключа, endpoint или registry; данные и конфигурация переносимы; коммерческий оператор не получает особого голоса за sponsorship, listing, certification, traffic или advertising spend.

**Последствие без правки:** рынок будет воспринимать AgentBridge как канал контроля или distribution moat, что подорвёт добровольное принятие даже при технически открытом Core.

### GOV-04 — Gate 4 возвращает постоянные client/service категории

**Уровень:** Высокий  
**Где:** §1.1; `FR-1`–`FR-2`; `FR-85`–`FR-87`; §5.1; §15 Gate 4

**Условие:** кандидатный минимум Gate 4 — «≥2 client и ≥2 service implementations». Это конфликтует с role-neutral моделью и позволяет четырём оболочкам одной организации либо одной semantic codebase формально выполнить числовой порог, не доказав agent↔agent, B2B, reverse/multi-party или role reversal.

**Ограниченная правка:** заменить client/service count на scope-aware criterion: реализации должны коллективно покрывать все роли и topology classes, для которых делается adoption claim; одна genealogy, общий semantic generator, общий controlling organization или одна operational deployment не считаются разными независимыми реализациями; каждое число сопровождается картой ownership, funding, code lineage и фактически проверенного scope.

**Последствие без правки:** Gate 4 может легитимизировать узкий двусторонний demo как принятие role-neutral стандарта.

### GOV-05 — «Повторяемое добровольное использование» не операционализировано

**Уровень:** Высокий  
**Где:** §6.2; §8.5; `R-8`, `R-16`; §15 Gate 3–4; §15.1

**Условие:** Gate 4 не задаёт наблюдаемый период, повторяемость, уровень реальности нагрузки, обновление версий, отсутствие постоянной помощи авторов, причины отказа и степень экономической или организационной независимости. Финансируемые пилоты, одноразовые demos и интеграции, поддерживаемые только core-командой, могут выглядеть как adoption.

**Ограниченная правка:** до Gate 4 заморозить отдельный Adoption Charter: claim scope; минимальный период и повторяемое использование; независимость организаций и deployments; unassisted integration; обновление версии; provider replacement или exit/migration; defects/incidents; abandoned integrations и причины; incentives/subsidies; критерии invalidation. Значения определяются на основе Gate 3, а не выдумываются в текущем PRD.

**Последствие без правки:** ранний интерес или оплаченный пилот будет ошибочно представлен как устойчивое экосистемное принятие.

### GOV-06 — Нет политики conformance marks и независимой верификации

**Уровень:** Высокий  
**Где:** `FR-82`–`FR-83`, `FR-92`, `FR-95`; `NFR-27`–`NFR-28`; §15.3

**Условие:** PRD хорошо ограничивает Conformance Claim и сохраняет self-conformance, но не определяет, кто и на каких условиях может использовать название/mark, как различаются self-tested, independently verified и certified, как mark приостанавливается, оспаривается и восстанавливается. Требование нескольких аудиторов не решает, кто аккредитует их и может ли владелец trademark исключить конкурента.

**Ограниченная правка:** до первого публичного conformance claim принять открытые claim vocabulary и mark policy: обязательные scope/version/date/evidence fields; различимые статусы self/independent/certified; недискриминационная лицензия mark; несколько независимых verifier paths без обязательной аккредитации одного продавца; revocation/suspension/appeal; expiry; запрет pay-to-pass и запрет расширять mark за пределы доказанного target.

**Последствие без правки:** платный verifier или владелец trademark может стать де-факто gatekeeper совместимости, а пользователи не отличат разные уровни доказательств.

### GOV-07 — Gate 5 слишком широко разрешает реализацию paid layer

**Уровень:** Высокий  
**Где:** §14.2 `DQ-5`–`DQ-7`; §15 Gate 5; §15.3

**Условие:** Gate 5 проверяет buyer/problem/WTP и затем разрешает реализацию коммерческого продукта. Demand evidence не является security, privacy, legal, competition, financial-risk или principal-alignment evidence. Особенно это критично для certification, transaction services, marketplace и advertising.

**Ограниченная правка:** переименовать разрешение Gate 5 в «разрешает отдельное product planning/implementation decision», но требовать для каждого слоя собственный PRD и применимые security/privacy/legal/domain/governance gates до production. Зафиксировать, что payments, certification, marketplace и advertising не могут делить одно общее автоматическое разрешение и не могут быть навязаны bundling или degraded-open-tier механикой.

**Последствие без правки:** подтверждённая готовность платить может ошибочно разрешить небезопасный или захватывающий стандарт коммерческий слой.

### GOV-08 — Security emergency governance описан как возможность, а не процесс

**Уровень:** Высокий  
**Где:** `FR-65`, `FR-94`–`FR-95`; `NFR-29`; §10.4; §15 Gate 4

**Условие:** PRD допускает withdrawal, исправления и suspension claims, но не разделяет confidential vulnerability handling, emergency errata, normative semantic change и обычный release. Нет владельца решения, критериев уведомления, восстановления claim, апелляции и сохранения исторического record.

**Ограниченная правка:** до публичного release утвердить security policy: intake и private triage; конфликтующие роли и recusal; область embargo; классификация fix/erratum/new-version; уведомление affected implementers; публичный advisory после допустимого embargo; machine-readable scope invalidation; restoration criteria и post-incident review. Процесс не должен давать одному vendor право тихо переписать норму.

**Последствие без правки:** критический инцидент приведёт либо к слишком позднему исправлению, либо к непрозрачному emergency capture нормативного процесса.

### GOV-09 — Управление shared namespaces, Profiles и Extensions отложено без минимальных гарантий

**Уровень:** Высокий  
**Где:** `FR-56`, `FR-59`, `FR-61`, `FR-67`; F6 validation gate; §14.2 `DQ-2`

**Условие:** `FR-59` требует владельца или rules of governance, но схема остаётся будущей. До появления правил возможны squatting, переиспользование идентичности, платное предпочтение, конфликт между vendor и shared namespaces или повышение популярного proprietary extension до Core.

**Ограниченная правка:** до внешней публикации Extensions/Profiles определить два различимых пути: автономные vendor-scoped identifiers и нейтрально управляемое shared пространство. Для shared пространства нужны публичные критерии, provenance, immutable meaning, lifecycle, конфликт/appeal, abandonment/reassignment и запрет платного приоритета. Promotion to Core остаётся по `FR-67` и governance charter.

**Последствие без правки:** namespace или популярный extension станет скрытым коммерческим choke point либо источником несовместимых форков.

### GOV-10 — Bridge governance не закрепляет эксплуатационного владельца и срок claim

**Уровень:** Средний  
**Где:** `FR-75`–`FR-81`; `FR-91`; `H-7`; `VC-11`; §15 Gate 3–4

**Условие:** техническое ограничение bridge claims сильное, но не требуется публично указывать maintainer, security contact, поддержку source/target версий, права на mapping/tests, срок обслуживания и условия abandonment. Также нет governance-правила, запрещающего сделать один bridge «рекомендуемым по умолчанию» так, что он станет обязательным фактически.

**Ограниченная правка:** добавить обязательный bridge manifest: ownership/maintainers, source/target versions, direction, IPR/license, tests, security contact, supported-until/expiry, drift triggers, known semantic loss и successor/migration path. Gate 4 проверяет, что bridge остаётся reimplementable, policy-selectable и не является необходимым для Native Path.

**Последствие без правки:** заброшенный или привилегированный bridge станет транзитивной точкой отказа и де-факто изменит Core.

### GOV-11 — OSS sustainability и continuity не входят в legitimacy gate

**Уровень:** Высокий  
**Где:** `R-15`; `NFR-7`, `NFR-27`–`NFR-29`; §15 Gate 4

**Условие:** PRD признаёт малую команду и role conflicts, но Gate 4 не проверяет maintainer diversity, bus factor, устойчивость Suite/security response, владение domains/repositories/signing keys и возможность продолжить стандарт при закрытии компании или смене стратегии спонсора.

**Ограниченная правка:** включить в Gate 4 continuity evidence: несколько активных maintainers с раскрытым organizational control; published succession/archival plan; воспроизводимые releases; резервирование или передаваемое управление essential domains, repositories, keys и test artifacts; прозрачное финансирование; практическое право fork и продолжения открытого стандарта без коммерческого владельца.

**Последствие без правки:** стандарт может быть технически децентрализован, но организационно прекратить существование из-за одного владельца или ключа.

### GOV-12 — Последовательность reference SDK создаёт риск monoculture

**Уровень:** Средний  
**Где:** `FR-85`, `FR-93`–`FR-94`; §15 Gate 2–3; §15.2

**Условие:** §15.2 ставит «один полный reference SDK» перед действительно независимой реализацией. Даже без общего кода внешний implementer может скопировать поведение или quirks reference SDK, а conformance suite — закрепить тот же взгляд на неоднозначность.

**Ограниченная правка:** в Gate 3 предусмотреть минимум один clean-room/spec-only implementation track до доступа к reference behavior либо с зафиксированной exposure genealogy. Reference SDK маркируется non-normative, не получает привилегированного claim, а обнаруженные совпадающие quirks возвращаются в spec ambiguity triage.

**Последствие без правки:** несколько реализаций могут воспроизвести одну ошибку и создать ложное доказательство независимой интерпретируемости.

### GOV-13 — Claim discipline не образует лестницу разрешённых публичных формулировок

**Уровень:** Высокий  
**Где:** `FR-82`, `FR-95`, `FR-107`; §10.4; §15 Gate 2–4; §15.1

**Условие:** запрещены преждевременные заявления, но «соответствующий gate» не сопоставлен с точной разрешённой формулировкой. Техническая conformance, внешний pilot, повторяемое adoption, open standard и industry standard остаются разными понятиями, которые маркетинг или партнёр может смешать.

**Ограниченная правка:** создать до внешней коммуникации versioned Claims Matrix: для каждого Gate — допустимые термины, обязательный evidence link, scope, exclusions, дата/expiry, issuer и invalidation triggers. Статус «industry standard» не разрешается одним Gate 4 threshold; он требует отдельно определённого широкого независимого adoption evidence и не может быть self-declared владельцем проекта.

**Последствие без правки:** корректные технические результаты будут превращены в более сильные ecosystem или security promises, чем подтверждает evidence.

### GOV-14 — Gate 4 legitimacy может быть утрачена, но процедура downgrade отсутствует

**Уровень:** Средний  
**Где:** `FR-95`; `NFR-29`; `R-11`, `R-16`; §15 Gate 4–5

**Условие:** PRD хорошо инвалидирует conformance при security/semantic изменениях, но не определяет пересмотр adoption/governance claim при захвате change control, исчезновении независимых maintainers/implementations, появлении обязательной коммерческой зависимости или прекращении реального использования.

**Ограниченная правка:** сделать Gate 4 claims time-bounded и определить non-security revalidation triggers: изменение control/IP/mark policy, maintainer collapse, потеря independent implementations, mandatory infrastructure, material governance dispute, adoption decay и incompatible fork. Исторический record сохраняется, текущий claim понижается или приостанавливается до повторной проверки.

**Последствие без правки:** устаревший статус открытого стандарта продолжит использоваться после исчезновения фактической нейтральности или adoption.

## Обязательные правки по воротам

| Когда | Минимальный пакет |
| --- | --- |
| **До первого публичного normative draft или внешнего contribution** | GOV-01, базовая часть GOV-08; лицензии и contribution/IP terms действуют до получения чужих материалов |
| **До входа в Gate 3** | GOV-02, GOV-04, GOV-06, GOV-12; зафиксированы независимость, claims/marks и минимальный change-control process |
| **До положительного Gate 4** | GOV-03, GOV-05, GOV-09, GOV-10, GOV-11, GOV-13, GOV-14; governance доказана поведением, а не только документом |
| **До реализации любого paid layer после Gate 5** | GOV-07 и отдельный layer-specific PRD/risk gate; §15.3 остаётся непреодолимым ограничением |

## Итоговая оценка Gates 2–5

- **Gate 2:** направление корректно, но любой публичный draft/contribution требует предварительного IP/contribution baseline. Reference tooling должно оставаться явно experimental и non-normative.
- **Gate 3:** недостаточно только получить внешние реализации; до их приглашения нужен минимальный governance/claims framework, а независимость необходимо оценивать по control, genealogy и exposure, а не только по числу авторов.
- **Gate 4:** правильное разделение adoption и governance, но текущий числовой минимум и слово «повторяемое» недостаточны для сильного ecosystem claim. Нужны Adoption Charter, работающий change control, continuity и protocol/platform firewall.
- **Gate 5:** правильно отделяет demand от adoption, но должен разрешать не production implementation вообще, а только переход к отдельному PRD и применимым risk gates каждого коммерческого слоя.

**Итог:** после внесения bounded fixes PRD создаст правдоподобный путь не только к открытому коду, но и к стандарту, который независимые стороны могут реализовывать, изменять, проверять и продолжать без разрешения или коммерческой зависимости от одного владельца.

## Re-review after fixes

**Дата:** 2026-09-13  
**Объект:** обновлённый `prd.md` после применения reviewer fixes  
**Итог повторной проверки:** все прежние Critical/High findings закрыты на уровне PRD; governance/adoption/commercial-neutrality регрессий не обнаружено.

| Finding | Статус | Краткое доказательство в обновлённом PRD |
| --- | --- | --- |
| **GOV-01 — IP/RF timing** | **Resolved** | `NFR-27` теперь требует открытые specification/conformance licenses, contribution/provenance terms, patent disclosure и RF/non-assert без field-of-use ограничений **до первого публичного normative draft и первого внешнего contribution**. То же является условием Gate 2 и §15.4. |
| **GOV-02 — Neutral change control** | **Resolved** | Gate 3 требует действующий минимальный governance/claims framework; §15.4 фиксирует public proposals/diffs, решения и dissent, participation, conflicts/recusal, decision procedure, appeal, immutable releases и отдельный emergency path. Один vendor не может единолично менять и норму, и oracle. |
| **GOV-03 — Protocol/platform firewall** | **Resolved** | `NFR-28` сохраняет добровольность коммерческих слоёв; §15.4 до Gate 4 требует недискриминационный доступ к specification, suites, namespaces, repositories и signing/release assets и запрещает особый governance voice за sponsorship, listing, certification, traffic или advertising spend. §15.3 отдельно запрещает protocol toll/pay-to-pass и влияние marketplace/ads на норму или verdict. |
| **GOV-04 — Role-neutral adoption threshold** | **Resolved** | Gate 4 заменил client/service count на ≥4 независимо контролируемых implementations/deployments, коллективно покрывающих роли и topology scope конкретного claim; общий controller, genealogy или deployment не считаются независимыми. Это согласовано с `FR-1`–`FR-2` и `FR-85`–`FR-87`. |
| **GOV-05 — Adoption evidence** | **Resolved** | Gate 4 и §15.4 требуют отдельный Adoption Charter с периодом, реальностью/повторяемостью использования, unassisted integration/update/migration, organizational/code/operational independence, incentives, incidents, exits и invalidation. |
| **GOV-06 — Conformance marks** | **Resolved** | §15.4 требует до первого публичного conformance claim Claims/Marks Policy со scope/version/date/evidence/expiry, различением self-tested/independently verified/certified, недискриминационной mark license и suspension/appeal/restoration без pay-to-pass. |
| **GOV-07 — Gate 5 over-authorization** | **Resolved** | Gate 5 теперь разрешает только отдельное product-planning/implementation decision и требует отдельный PRD с применимыми security/privacy/legal/domain/governance gates. `DQ-6`–`DQ-7` и §15.3 продолжают разделять payments, certification, marketplace, ads и другие paid layers. |
| **GOV-08 — Security emergency governance** | **Resolved** | `NFR-29` теперь требует до публичного release vulnerability intake/private triage, recusal, embargo, классификацию `fix/erratum/new version`, уведомление, restoration criteria и post-incident review; emergency path не разрешает vendor тихо переписать норму. |
| **GOV-09 — Namespaces/Profiles/Extensions** | **Resolved** | §15.4 разделяет vendor-scoped identifiers и нейтральный shared namespace; shared names получают публичные provenance/lifecycle/conflict/appeal rules без платного приоритета. Promotion to Core остаётся ограничен `FR-67`. |
| **GOV-10 — Bridge governance** | **Resolved** | §15.4 требует для bridge claim maintainer/security contact, source/target versions, direction, IPR/license, tests, known loss, expiry/drift и migration path, а также reimplementability и необязательность для Native Path; это дополняет `FR-75`–`FR-81`. |
| **GOV-11 — OSS continuity** | **Resolved** | §15.4 до сильного ecosystem claim требует нескольких раскрытых maintainers, succession/archival plan, reproducible releases, переносимое управление essential domains/repositories/keys/artifacts, прозрачное финансирование и практическую возможность продолжить стандарт без коммерческого владельца. |
| **GOV-12 — Reference SDK monoculture** | **Resolved** | §15.2 ставит clean-room/spec-only implementation track до доступа к reference behavior либо требует точную exposure genealogy; reference SDK явно non-normative. Это согласовано с `FR-85` и `FR-93`. |
| **GOV-13 — Claims ladder** | **Resolved** | Gate 4 разрешает только limited, time-bounded adoption claims; §15.4 вводит лестницу `technical conformance → external implementation → adoption → open-standard legitimacy`. `Industry standard` требует отдельно определённого широкого независимого adoption и не может быть self-declared. Claims/Marks Policy добавляет evidence/expiry. |
| **GOV-14 — Revalidation/downgrade** | **Resolved** | Gate 4 требует downgrade/suspension при утрате условий; §15.4 перечисляет non-security triggers: control/IP/marks, потеря maintainers/implementations, обязательная инфраструктура, существенный dispute, adoption decay и несовместимый fork, сохраняя исторический record. |

### Проверка возможных регрессий

- Gate 1 и Validation Charter не получили зависимости от будущей governance-организации или коммерческой платформы.
- Новый порог Gate 4 не сужает протокол до client/server: evidence ограничивается фактически заявленными ролями и topology scope.
- Коммерческие слои не получили права изменять Core, conformance verdict или базовую совместимость.
- Clean-room track усиливает независимость, не делая Reference SDK нормативным.
- `DQ-2` по-прежнему откладывает **формальную организационную форму** до Gate 2–3, но не откладывает обязательный IP/RF baseline: его более раннее вступление прямо закреплено в `NFR-27`, Gate 2 и §15.4. Это не является блокирующим противоречием.

### Остаточный вердикт

**Оставшихся Critical/High замечаний: 0.** PRD можно пропускать через governance/adoption reviewer gate. Конкретные лицензии, процедуры, владельцы и числовые пороги всё ещё должны быть созданы на указанных будущих этапах; PRD теперь правильно запрещает двигаться дальше без них.
