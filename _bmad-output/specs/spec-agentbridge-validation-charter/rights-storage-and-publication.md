---
title: OQ-8A Rights, Storage, Disclosure and Publication Policy
status: oq8a-frozen-no-start
created: 2026-09-14
updated: 2026-09-14
governing_spec: SPEC.md
---

# OQ-8A — права, хранение и публикация

## 1. Простое объяснение

Надпись «open source» не означает, что разрешено копировать любой текст, код или название. OQ-8A требует отдельно проверить авторские права, патенты, товарные знаки, условия вкладов, зависимости, закрытые данные и раскрытие уязвимостей. Пока точная проверка OQ-8B не пройдена, внешний материал можно изучать и описывать ссылкой, но нельзя автоматически копировать в AgentBridge или публиковать как часть проекта.

Это внутренняя политика подготовки Gate 1, а не юридическое заключение.

## 2. Разделение OQ-8A и OQ-8B

- **OQ-8A — policy:** неизменяемые правила допустимости, происхождения, хранения, disclosure и публикации.
- **OQ-8B-Pilot:** до любых внешних обязательств фиксирует exact licenses/terms, участников, agreements, stores, regions, keys, retention и publication boundary neutral pilot.
- **OQ-8B-Confirmatory:** до OQ-3B/Candidate exposure фиксирует тот же полный реестр для каждого exact commit/tag, transitive dependency, tool/model, dataset, Candidate artifact и evidence store полного Gate 1.

Отсутствующее, неоднозначное или изменившееся право означает `No Use / No Start`, а не предполагаемое разрешение.

## 3. Матрица прав

Для каждого входа OQ-8B хранит отдельные поля:

| Поле | Что должно быть доказано |
| --- | --- |
| Identity | точное имя, владелец, URL, tag/commit/digest, дата получения |
| Copyright | лицензия именно нужного файла/компонента и право на использование, изменение и распространение |
| Patent/IPR | применимый patent grant/promise, disclosures, exclusions и termination conditions |
| Contribution | автор/работодатель вправе передать вклад; DCO/CLA/иной agreement выполнен |
| Trademark | название/логотип используются только описательно либо по отдельному разрешению |
| Redistribution | NOTICE, attribution, source, modification и offer obligations выполнены |
| Compatibility | совместимость со способом использования и предполагаемой лицензией конкретного AgentBridge artifact |
| Provenance | источник, преобразования, AI-assistance, reviewer и scan/attestation сохранены |
| Change | кто и когда обязан повторно проверить license/IPR/governance/security policy |

Repo-level LICENSE не доказывает лицензию каждого submodule, generated artifact, dataset, model, hosted API или будущей спецификации другого органа.

До substantive contribution/publication ведётся **AgentBridge Output Rights Ledger**: artifact digest; creator и employer/contractor authority; legal owner/licensor; assignment либо достаточная non-exclusive license; inbound/outbound terms; право передачи нейтральному standards steward; relicensing consent rule; continuity при продаже/закрытии; AI/copyrightability uncertainty и результат независимой проверки. Неясный owner/licensor даёт `No Publication / No Transfer`.

## 4. Допустимость зависимостей

- Permissive licenses, подтверждённые как OSI-approved и записанные точным SPDX expression, могут пройти только после file/component-level verification, NOTICE и patent review. `LicenseRef-*`, exceptions, dual/multi-license expressions и `NOASSERTION` проверяются отдельно.
- Copyleft/weak-copyleft не запрещены автоматически, но требуют письменного compatibility/segregation analysis для конкретного linking, modification, distribution и hosted-use режима.
- `source available`, non-commercial, no-derivatives, field-of-use, custom, evaluation-only, click-through, unknown или conflicting terms дают `No Use`, пока независимый IP reviewer не подтвердит допустимость.
- IETF RFC, OpenID/FIDO и иные standards-body документы используются по их собственным copyright/IPR rules; ссылка на стандарт не равна разрешению копировать его текст или код.
- Hosted APIs, SaaS, models/plugins и datasets проверяются не только по лицензии репозитория, но и по Terms, data-use, telemetry, confidentiality, retention, training и export restrictions.
- Security policy, governance или лицензия, изменившиеся после pin, автоматически открывают затронутую OQ-8B запись и блокируют новый запуск до проверки.

## 5. AgentBridge: временная лицензионная позиция

- Для будущего кода, SDK, reference implementations и conformance tooling предпочтительный **проверяемый кандидат** — `Apache-2.0`. Для текста спецификации он может быть copyright-license candidate, но сам по себе не обеспечивает patent safety индустриального стандарта и не является окончательным решением.
- До внешних вкладов/публичного normative draft обязателен отдельный **AgentBridge Standards IPR Track**: определение Essential Claims; RF/RAND-Z implementer covenant; patent disclosure ledger; exclusions/withdrawal; охват полных, частичных и совместимых реализаций; reciprocity/defensive termination; continuity и требования будущего standards body. Это не гарантирует freedom-to-operate; существенный риск требует профильного patent search и заключения квалифицированного юриста.
- Обычная ненормативная документация может отдельно рассматривать `CC-BY-4.0`; данные/test corpus требуют собственной подходящей лицензии и privacy review.
- До публичного вклада репозиторий обязан иметь approved contribution policy. Минимум: signed-off provenance, явная inbound=outbound license, полномочие работодателя, AI-assistance disclosure и patent/IPR disclosure. DCO сам по себе не заменяет нужный patent grant.
- До появления независимого standards governance публичный draft маркируется как проект AgentBridge, а не как признанный индустриальный стандарт.
- Название `AgentBridge`, домены и логотипы не считаются юридически свободными. Любое внешнее использование как бренда — public repository/draft/site, domain activation, package namespace, social account, event или partner material — блокируется до clearance. OQ-8B фиксирует территории, классы, owner, дату/результат поиска и допустимое описательное использование.
- Essential assets включают normative specification, schemas/registries, conformance suite/test marks, reference artifacts, change history и protocol trademarks. Для них обязательны безотзывные уже выданные публичные лицензии, открытые change/conflict/appeal rules, отсутствие sponsor veto над лицензией/negative evidence/conformance и заранее определимые условия передачи нейтральному steward. Конкретный орган выбирается позже.

## 6. Текущая карта официальных источников

Карта фиксирует только исходную due-diligence позицию, полученную **2026-09-14**; она не является compatibility evidence. OQ-8B обязан сохранить для каждой строки прямые snapshots LICENSE, NOTICE, CONTRIBUTING/CLA/DCO и IPR policy с exact tag/commit, content digest и retrieval timestamp. Если snapshot ниже не извлечён точно, статус остаётся `unverified / No Use`.

| Инициатива | Наблюдение из официального источника | Решение сейчас |
| --- | --- | --- |
| A2A | repository заявляет Apache-2.0; OQ-1 pin `1736957…` | unverified per-file; exact LICENSE/NOTICE/IPR digest обязателен |
| MCP | exact OQ-1 pin `5f5440b…` LICENSE подтверждает переход MIT → Apache-2.0, прежние вклады могут оставаться MIT, docs отдельно CC-BY-4.0 | mixed; file provenance/relicense status обязателен |
| OpenAPI / Arazzo | repositories/specifications заявлены под Apache-2.0 | условно допустим; exact version/NOTICE проверить |
| UCP | repository заявлен под Apache-2.0 | условно допустим; schema/SDK/transitive closure проверить отдельно |
| AP2 | текущий repo заявлен под Apache-2.0, но core specification передан в FIDO | repo и будущий standards-body artifact — разные правовые объекты; FIDO status/IPR проверить заново |
| AuthZEN | Final Specification имеет специальное OIDF copyright grant и ссылается на OIDF patent promise/IPR policy | использовать по OIDF terms, не называть Apache-2.0 без отдельного основания |
| IETF RFCs | действуют IETF Trust rules/BCP 78 и disclosure process BCP 79 | implement/reference по применимым правилам; не копировать как обычный OSS source |
| GNAP, SCITT/COSE | RFC-артефакты следуют IETF rules; реализации могут иметь иные лицензии | спецификацию и каждую реализацию учитывать отдельно |
| AGNTCY / ANP | OQ-1 pins есть, но полный component/license/IPR BOM ещё не frozen | `No Use` до OQ-8B exact verification |

Официальные исходные точки: [A2A repository](https://github.com/a2aproject/A2A), [MCP LICENSE at frozen pin](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/5f5440bb26a62e2cf3440b92da5a667efa03b267/LICENSE), [MCP contribution policy](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/CONTRIBUTING.md), [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification), [Arazzo Specification](https://github.com/OAI/Arazzo-Specification), [UCP repository](https://github.com/Universal-Commerce-Protocol/ucp), [AP2 contribution status](https://github.com/google-agentic-commerce/AP2/blob/main/CONTRIBUTING.md), [AuthZEN Authorization API notices](https://github.com/openid/authzen/blob/main/api/authorization-api-1_0.md), [IETF BCP 79](https://datatracker.ietf.org/doc/rfc8179/).

## 7. Классы информации и хранилища

| Класс | Примеры | Где разрешено | Evidence minimum | Privacy maximum / disposition |
| --- | --- | --- | --- | --- |
| P0 Public | отдельно одобренные releases, public sources | public repository после checklist | нормальные releases — indefinitely | при sensitive/prohibited disclosure payload немедленно containment/purge по правилу §10; сохраняется безопасная audit-запись, не секрет |
| P1 Internal | планы без secrets, черновые обзоры | access-controlled project store | до final Gate record | удалить/архивировать не позднее final record + 24 месяца |
| P2 Confidential evidence | coded raw results, private candidate work | отдельный encrypted compartment с I2 ACL | final Gate record + 36 месяцев | удалить/анонимизировать в конце минимума, если нет approved extension/legal hold |
| P2-Personal | roster identity map, contracts/rates, contributor/employer records | отдельный identity/legal compartment, не technical evidence | только пока нужно для dispute/legal duty | удалить или необратимо отделить как можно раньше по OQ-8B lawful-purpose сроку |
| P3 Restricted | sealed holdout/oracle, credentials, security reports, keys | отдельные threshold-controlled compartments; не текущий shared workspace | по lifecycle ниже | по lifecycle ниже |
| P4 Prohibited input | production/customer data, real payment credentials/effects, приватные foundational files вне их local-only context | не допускается в experiment/evidence/publication systems | none | не копировать; обнаружение запускает incident response и verified removal |

P3 lifecycle:

- active credentials/keys отзываются и crypto-erased в конце фазы или немедленно при incident; активные остаточные копии удаляются в течение 7 дней, backups сразу становятся недоступными через key revocation и физически истекают не позднее 90 дней;
- sealed holdout хранится до final Gate 1 record (`native/profile/upstream/stop/No Decision`) либо documented abandonment; после reveal переходит в P2 evidence, нераскрытый остаток удаляется в течение 90 дней после final record/abandonment и всегда заменяется новым seed перед rerun;
- security reports хранятся до verified closure плюс 36 месяцев;
- access/audit logs хранятся 36 месяцев после Gate 1 decision;
- backup expiry не позднее 90 дней после применимого deletion event.

Legal hold приостанавливает удаление только для названного scope, сохраняет ACL/audit и требует документированных authority, начала, пересмотра каждые 90 дней и завершения. OQ-8B назначает post-project custodian и заранее финансирует transfer-or-destroy; закрытие проекта/поставщика не отменяет сроки. Не закрытый security report передаётся approved successor custodian либо сохраняется только на fully funded closure period. Более долгий законный срок требует записанного основания.

## 8. Обязательные storage/privacy controls

- Synthetic data only; минимально необходимые personal data ролей отделены от technical evidence и заменены кодами.
- Encryption in transit/at rest, разные keys и controller/admin/recovery roots для holdout, oracle, N, C, raw results и publication staging.
- Default deny, named identities, MFA/short-lived grants, two-person privileged actions, immutable access log и independently anchored digest.
- Ни secrets, ни private source documents, ни P2/P3 не помещаются в Git history, issues, CI logs, general AI prompts, telemetry, crash reports или candidate runtime.
- Region, subprocessors, backup path, telemetry/training policy, export/restore/delete tests и breach contacts фиксируются в OQ-8B.
- Потеря required evidence не маскируется удалением: запуск становится invalid/inconclusive; candidate-caused concealment следует SBC-08.
- Retention clock, deletion receipt и исключения записываются до ingestion. Нельзя сначала собрать данные, а затем придумать purpose.
- Export, screenshot, aggregate, cache, index/embedding, prompt, log, backup и иная производная наследуют наивысший класс источника до документированной независимой declassification. Deletion manifest перечисляет primary data, replicas, caches/indexes, derivatives, exports, provider prompt retention и backups; independent verifier подтверждает deletion receipt/exception. Сохраняются безопасные digests/provenance/incident metadata, но не отозванные secret values.
- OQ-8B для personal data фиксирует controller/processor, purpose/lawful basis, categories/subjects, minimization, notice/rights/appeal contact, subprocessors/transfers/regions, DPIA-threshold, breach/regulator/data-subject notification и deletion route.
- Privileged/break-glass controls наследуют OQ-6A §§5/11: R-01, R-10, affected arm и shared admin root не могут быть authorizers; bypass вызывает pause/quarantine/rotation и independent scope review.

## 9. Vulnerability intake и часы

Отсчёт начинается с immutable timestamp первого получения либо автоматического обнаружения и не сбрасывается поздним triage/notice. На intake применяется наивысшая разумно возможная provisional severity; R-09 или заранее назначенный alternate отвечает за неё. Downgrade требует timestamped rationale и независимого governance concurrence. Во время активного run действует круглосуточный elapsed-time clock; вне run — business days по заранее зафиксированному в OQ-8B календарю/часовому поясу. OQ-8B также фиксирует monitored channels, backup/on-call owner и outage fallback.

| Severity | Acknowledge | Initial triage/containment decision | Private affected-party notice |
| --- | ---: | ---: | ---: |
| Critical | 4 elapsed hours | 12 elapsed hours | 24 elapsed hours |
| High | 1 business day | 3 business days | 3 business days |
| Medium | 3 business days | 10 business days | 10 business days либо documented no-notice rationale |
| Low | 5 business days | 20 business days | по material impact |

Любой suspected SBC и любой suspected Critical/High немедленно приостанавливает affected work до классификации; часы не разрешают ждать до deadline, если containment возможен раньше. Missed clock сохраняется как process finding и требует escalation R-09 → R-02 + independent governance alternate → R-01; оно не меняет severity и не скрывает исходный finding.

Публичное technical disclosure по умолчанию не раньше исправления/mitigation и coordinated notice, но embargo заканчивается через 90 календарных дней от immutable intake timestamp; отсутствие исправления само по себе срок не продлевает. Одно продление максимум на 30 дней требует совместного documented решения независимого R-09 и governance alternate. На deadline выпускается risk-minimized advisory; active exploitation или немедленный широкий риск требуют более раннего предупреждения с минимальными exploit details. Иное допускается только по документированному требованию закона либо когда независимые security+governance reviewers установили, что публикация сама создаст больший непосредственный вред; исключение пересматривается каждые 30 дней. Standards-body/vendor security policy координируется, но не служит бесконечным embargo. Project Owner не вправе единолично публиковать P2/P3 или подавлять обязательное уведомление.

## 10. Publication pipeline

Переход разрешён только `P2/P3 → reviewed P1 extract → isolated P0 staging → P0 release`; прямой экспорт запрещён. Каждый release требует три подписи: Project Owner, independent Security/Privacy Reviewer и отдельный independent IP Reviewer. Два reviewer имеют разные I2 controller domains и независимы от Owner, sponsor, N/C teams и затронутых vendors; каждый конфликт требует заранее назначенного independent alternate, отсутствие любой подписи означает `No Release`. Автор не одобряет собственный материал. R-10 дополнительно даёт только механическую custody attestation источника, digest, разрешённого extract и chain-of-custody, не решая вопрос публикации.

Independent IP Reviewer включается в OQ-6B/OQ-8B: подтверждённая компетенция по применимому open-source/standards IP, раскрытые employer/funding/conflicts, минимально необходимый доступ, I2 boundary, recusal/alternate. Неразрешённое разногласие даёт `No Use / No Release`, а не голосование большинством.

Перед каждым release подтверждаются:

1. artifact identity, digest, source/provenance и scope;
2. license/IPR/trademark compatibility, NOTICE/attribution и modification marks;
3. отсутствие secrets, private foundational text, personal/customer/production data и sealed material;
4. security/privacy/redaction review, включая metadata, history, generated files, archives и links;
5. согласованное disclosure или отсутствие незакрытого embargo/legal duty;
6. корректную claim boundary: experimental evidence не назван adoption, certification или индустриальным стандартом;
7. сохранение отрицательных результатов/dissent либо явное описание того, что не публикуется и как это ограничивает claim;
8. воспроизводимый public subset, SBOM, license ledger и release manifest;
9. одинаковая для N и C юридически разрешённая воспроизводимость: если любой arm не позволяет симметричный minimum package, положительный N-vs-C comparative claim запрещён; arm остаётся только несравнительным описательным evidence, а ограничение маркируется как legal limitation, не architectural fail;
10. immutable release record и correction/withdrawal contact.

Любой публичный Gate 1/Native/N-vs-C claim выпускается только с independently attested симметричным minimum evidence package: positive, negative, invalid/inconclusive results, dissent и **санитизированные категории** закрытых материалов с объяснением влияния. Названия, identities и digests P2/P3 проходят тот же P2/P3→P1→P0 review; exact inventory/digests остаются в protected custody. Если такой package юридически/безопасно раскрыть нельзя, сравнительный claim запрещён.

Обычная содержательная ошибка после публикации не стирается переписыванием Git history: выпускаются signed withdrawal/correction advisory, неизменяемая пометка версии и исправленный release. Но credential, private foundational text, personal data или иной P2/P3/P4 payload требует немедленного containment: revoke/rotate, suspend distribution, purge project-controlled Git history/object/package/cache где юридически и технически возможно, запросить downstream cache/mirror takedown и сохранить только safe digest, redacted chain-of-custody и withdrawal/incident record. Стороннее удаление нельзя гарантировать; residual exposure отслеживается. Удаление секрета только из последнего commit недостаточно.

## 11. Contribution, AI и внешняя коммуникация

- До approved contribution policy внешние PR/issues/discussions для normative artifacts отключаются, где возможно. Иначе templates/CONTRIBUTING заранее помечают входящие материалы; непроверенный вклад quarantined и явно обозначен `Not a Contribution / not accepted for inclusion`, не копируется в normative artifacts до clearance.
- Каждый вклад имеет author/controller, employer authorization при необходимости, inbound license, patent/IPR declaration, AI/tool genealogy и conflict disclosure.
- AI-generated/assisted material не считается автоматически свободным от чужих прав. Для каждого существенного artifact ledger хранит provider/tool, model/version, timestamp, Terms/policy snapshot, jurisdiction/account type, input classification, output digest, human edits/reviewer, training/retention settings, similarity/license scan и итоговое основание публикации. Неопределённость означает rewrite/remove/No Use.
- Нельзя передавать закрытый материал внешнему AI/provider или участнику для «проверки лицензии» без OQ-8B разрешения и data-processing review.
- Обращение в upstream issue/PR, standards body, СМИ или к партнёру является отдельным внешним действием и требует scope-specific разрешения Project Owner после policy/AI-contribution check соответствующей площадки. **Исключение:** юридически обязательные уведомления, §9 private affected-party notices и emergency warnings заранее разрешены этой политикой и не требуют согласия Project Owner; их отправляет назначенная независимая роль с минимально необходимым раскрытием и immutable record.

## 12. Решения и границы

| Состояние | Последствие |
| --- | --- |
| license/IPR/provenance однозначны и совместимы | artifact может войти в exact OQ-8B manifest, но ещё не получает publication permission |
| право неизвестно, mixed либо зависит от непроверенных terms | `No Use` до разрешения; кандидат не получает бесплатный semantic/tooling credit |
| обязательная зависимость несовместима | заменить до exposure либо candidate становится infeasible; не ослаблять другой arm |
| storage/I2/retention controls недоступны | `No Start / seek resources` |
| утечка/несанкционированный доступ | pause, preserve, rotate/revoke, assess scope; OQ-6A contamination и OQ-4 SBC применяются |
| publication checklist не пройден | материал остаётся закрытым; отсутствие публикации сужает внешний claim |

OQ-8A не выбирает production governance, не регистрирует trademark, не заключает договоры, не выдаёт юридическое заключение и не разрешает публичный release. Exact legal text/agreements и jurisdiction-specific duties требуют квалифицированного юриста перед внешними обязательствами.

## 13. Freeze boundary

OQ-8A frozen Project Owner 2026-09-15 после независимых License/IP, Governance/Adoption/Publication и Security/Privacy reviews с нулём оставшихся findings. Теперь возможен внутренний A0 advisory. Neutral pilot остаётся `No Start` до согласованных OQ-8B-Pilot, OQ-7B-Pilot и OQ-6B-Pilot; полный Gate 1 — до OQ-8B-Confirmatory и остальных Charter freezes.
