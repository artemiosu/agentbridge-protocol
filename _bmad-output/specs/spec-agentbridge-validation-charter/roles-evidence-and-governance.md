# OQ-6 — Roles, independence and information barriers

Статус: **OQ-6A policy frozen by Project Owner on 2026-09-13**. Governance, fairness and security reviews: PASS, везде 0 findings. OQ-6B actual roster/access enforcement остаётся No Start.

## 1. Простое правило

Никто не должен одновременно создавать вариант, знать скрытые задания и решать, победил ли этот вариант. Все существенные вопросы, помощь, изменения и доступ фиксируются. Если независимость нельзя доказать, результат не считается подтверждением.

Разные fresh-context ИИ-процессы уменьшают влияние одной беседы, но **не являются независимыми компаниями, людьми, AI providers или security domains**. Они могут поддержать подготовку и internal corroboration для Gate 1, но не положительный confirmatory Gate 1 decision и не будущую внешнюю проверку Gate 3.

## 2. OQ-6A и OQ-6B

- **OQ-6A — policy:** роли, несовместимости, доступ, помощь, contamination, adjudication и claim limits этого документа.
- **OQ-6B — operational roster:** реальные назначенные identities, team composition, provider/model/tool versions, storage compartments, access-control proofs, budgets и signed acknowledgements.

Только `OQ-6B-Pilot` roster/barriers замораживаются после provisional OQ-7/OQ-8 и до neutral pilot; после pilot замораживается OQ-5B, затем `OQ-6B-Confirmatory` атомарно reconciled с OQ-5B до OQ-3B, Candidate exposure или кода. Невозможно назначить независимость задним числом.

## 3. Роли и полномочия

| ID | Роль | Владеет | Не вправе |
| --- | --- | --- | --- |
| R-01 | Project Owner | Charter acceptance, budget, final Decision Record, publication | Превратить hard-gate failure в pass; видеть sealed cases без emergency need |
| R-02 | Gate Chair / Method Steward | preregistration, parity, completeness, process calendar | Быть advocate/implementer/oracle author/final adjudicator; выбирать по вкусу |
| R-03 | Native Advocate | strongest-good-faith Candidate N contract/package | Видеть C private work или holdout; оценивать собственный итог |
| R-04 | Challenger Advocate | strongest-good-faith C1/C2/glue package | Видеть N private work или holdout; скрывать glue/dependencies; оценивать итог |
| R-05N | Native Implementation Teams | независимые N implementations из frozen package | Делиться semantic code; видеть C package, holdout или reference behavior до freeze |
| R-05C | Composition Implementation Teams | независимые C implementations из frozen package | Делиться pair-specific fixes; видеть N package или holdout |
| R-06 | Oracle Author | allowed/forbidden outcomes из frozen PRD/EI | Видеть Candidate design/output/metrics; участвовать в advocacy |
| R-07 | Blind Oracle Cross-checker | независимый semantic cross-check | Использовать общий oracle code как доказательство; видеть Candidate data |
| R-08 | Conformance Lead | suite/coverage/triage route | Делать reference behavior нормативным; быть candidate advocate |
| R-09 | Security Reviewer | OQ-4 application, threat/privacy review, closure recommendation | Быть advocate/implementer; видеть metrics/desired outcome до finding freeze; выбирать продуктовый исход вне evidence |
| R-10 | Evidence Custodian | sealed store, manifests, raw evidence, access log | Авторствовать cases/outcomes; adjudicate; удалять неблагоприятное evidence |
| R-11 | Seed Witness | independent entropy commitment/reveal witness | Видеть sealed cases или Candidate work; разрешать outcome-based reroll |
| R-12 | Outcome-blind Adjudicator | invalidity, source attribution, ambiguity и disputes | Получать unplanned arm disclosure, comparative metrics, desired outcome или advocate communications до ruling; inherent inference обрабатывает §10 |
| R-13 | Statistician | frozen SAP execution on coded arms | Менять analysis после unblinding; быть advocate/implementer |
| R-14 | Fresh-context Red Team | stronger challenger, counterexamples, bias, hidden assumptions | Менять thresholds post hoc; принимать финальное решение |
| R-15 | Expert Advisory Panel | non-binding pre-code recommendation in plain language | Выдавать мнение за executable evidence или скрывать dissent |
| R-16 | Blind Clarification Steward | coded classification вопросов, help и paired task defects | Видеть metrics/arm identity; быть advocate/implementer/statistician |

`R-05N/C` — набор команд, а не одна роль-identity. Exact team count/mix определяют OQ-5B/OQ-6B.

## 4. Несовместимости и назначение ролей

Одна identity/controller/organization/provider-root не считается двумя независимыми approvers только из-за разных аккаунтов или псевдонимов.

| Role | Нельзя совмещать в одном confirmatory scope |
| --- | --- |
| R-01 Owner | R-03–R-16; только governance acceptance/final decision |
| R-02 Chair | R-03–R-10, R-12–R-16; только process coordination |
| R-03 Native Advocate | R-04, R-05C, R-06–R-14, R-16 |
| R-04 Challenger Advocate | R-03, R-05N, R-06–R-14, R-16 |
| R-05N/R-05C Implementer | другая arm team; R-03/R-04 opposite arm; R-06–R-14, R-16 |
| R-06 Oracle Author | R-03–R-05, R-07–R-14, R-16 |
| R-07 Cross-checker | R-03–R-06, R-08–R-14, R-16 |
| R-08 Conformance Lead | R-03–R-07, R-09/R-10, R-12/R-13, R-16 |
| R-09 Security Reviewer | R-03–R-08, R-10, R-12/R-13, R-16 |
| R-10 Custodian | R-03–R-09, R-11–R-14, R-16 |
| R-11 Seed Witness | R-01–R-10, R-12–R-14, R-16 |
| R-12 Adjudicator | R-01–R-11, R-13/R-14, R-16 |
| R-13 Statistician | R-01–R-12, R-14, R-16 до signed output/unblinding |
| R-14 Red Team | R-03–R-13; prior candidate participation requires recusal or interested-only report |
| R-15 Advisory Panel | Может иметь disclosed viewpoint, но не занимает decisive R-03–R-14/R-16 role в том же recommendation scope |
| R-16 Clarification Steward | R-01–R-14; может использовать independent multi-member board с теми же ограничениями |

Project Owner публично назначает/заменяет R-02 и утверждает предложенный R-02 roster decisive roles после conflict review. R-02 назначает non-decisive operations и инициирует recusal/scope assessment; R-09 совместно с независимым governance reviewer утверждает security/custody/adjudication replacements. Если R-01/R-02 конфликтует, решение принимает заранее назначенный independent governance alternate; если его нет, Gate pauses. Break-glass authorizers задаются отдельно в §11 и не включают R-01/R-10/affected arm.

В малом проекте совмещение вне таблицы допустимо только для exploratory work; confirmatory claim становится `inconclusive`. Любой запрет действует симметрично, даже если указан только в строке одной из двух ролей.

## 5. Access matrix

Обозначения: `F` — full; `P` — только необходимая public/frozen часть; `B` — blinded/coded; `0` — нет доступа.

| Роль | PRD/EI/Charter | N private | C private | Shared corpus | Sealed holdout | Raw results | Arm labels/metrics |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| R-01 Owner | F | manifest only | manifest only | F | 0 | B summary | after rulings/SAP lock |
| R-02 Chair | F | manifest only | manifest only | F | 0 | B process status | after rulings/SAP lock |
| R-03 N Advocate | F | F | 0 | F | 0 | own only | own only |
| R-04 C Advocate | F | 0 | F | F | 0 | own only | own only |
| R-05N N teams | F | assigned N package | 0 | F | 0 | own only | own only |
| R-05C C teams | F | 0 | assigned C package | F | 0 | own only | own only |
| R-06 Oracle Author | F | 0 | 0 | F before design | F before seal only | 0 | 0 |
| R-07 Cross-checker | F | 0 | 0 | F before design | F before seal only | 0 | 0 |
| R-08 Conformance Lead | F | 0 | 0 | F | 0 | coded triage minimum | 0 |
| R-09 Security Reviewer | F | B after artifact freeze | B after artifact freeze | F | assigned scope after reveal | B | 0 until finding freeze |
| R-10 Custodian | manifests only | encrypted opaque store | encrypted opaque store | store | encrypted custody only | encrypted/coded custody | 0 |
| R-11 Seed Witness | commitment protocol only | 0 | 0 | generator metadata | entropy commitment/reveal only | 0 | 0 |
| R-12 Adjudicator | applicable rules | 0 | 0 | minimum packet | disputed case after reveal | B minimum packet | 0 |
| R-13 Statistician | SAP only | 0 | 0 | metadata | 0 | B analysis dataset | 0 until signed output |
| R-14 Red Team | F | after artifact freeze | after artifact freeze | F | after primary reveal | approved B | after primary rulings |
| R-15 Panel | F | approved summaries | approved summaries | F | 0 | approved summaries | after advisory record lock |
| R-16 Clarification Steward | taxonomy/rules | B question only | B question only | paired task map | 0 | effort-free question facts only | 0 |

Pre-reveal break-glass требует independently verified legal/security necessity и двух authorizers, независимых от R-01, R-10 и affected arm; self-authorization запрещена. Любое root/admin bypass немедленно останавливает runs, quarantines affected stratum, revokes/rotates credentials, сохраняет log и запускает independent scope assessment. Evidence не поддерживает positive claim, пока non-exposure независимо не доказан; иначе обязателен fresh preregistered tranche/new seed. Любопытство, срок или бюджет не являются основанием.

## 6. Реальная техническая граница текущего workspace

Текущие subagents работают с общей файловой системой и одним orchestration root. Поэтому локальная папка проекта **не является допустимым store ни для sealed holdout, oracle secrets, candidate private work, raw confirmatory results, credentials/keys, ни для publishable staging**, а root/operator технически способен объединить контексты. До OQ-6B/OQ-8 должны появиться реально изолированные least-access stores/credentials и проверяемые audit logs. До этого:

- exact holdout cases/seed здесь не создаются и не передаются implementers;
- выполняются только planning, public research и явно exploratory reviews;
- «fresh context» обозначает context separation, не cryptographic или organizational isolation;
- нельзя заявлять external, organizational, provider или storage independence.

OQ-6B обязан разделить holdout, oracle/outcomes, N private, C private, raw results и publishable staging по compartments и encryption keys без cross-mount/cache/backup. Keys/secrets не попадают в repo, prompts или candidate runtime; encrypted backups наследуют ACL/retention, а restore/deletion проверяются evidence.

## 7. Exposure and genealogy ledger

До assignment каждая identity подписывает/фиксирует:

- controller, organization, model/provider/version и tooling;
- prior domain/candidate/code/test exposure;
- доступ к reference implementations, upstream code и private mappings;
- prompts, retrieved context, consultations, copied fragments и AI assistance;
- team membership, shared libraries и code genealogy;
- UTC access events, artifact digests и help/correction consumption.

Неизвестная genealogy ограничивает результат functional interop конкретной пары и не доказывает independence. Ложная/скрытая запись рассматривается по SBC-08/SBC-09.

### Enforced access and supply chain

OQ-6B требует per-identity nonshared credentials, MFA/short-lived grants, deny-by-default object/action scopes, two-person privileged changes, joiner/mover/leaver review, immediate revoke/rotation, periodic access review и restore test. Independently anchored append-only/WORM digests защищают audit trail; чтение logs также журналируется с clock integrity. Access attempts, denials, exports, decryptions и privileged changes записываются. Один administrator не может менять и evidence, и его audit anchor. Невозможность доказать enforcement/logging даёт fail-closed pause.

Direct/transitive tooling, models, plugins/actions и build inputs инвентаризируются и digest. Build/test networks изолированы; offline closure либо synthetic allowlist запрещает telemetry/update egress. Artifact проходит scan/attestation до ingress; candidate/tooling никогда не исполняется в oracle/holdout/custody compartments. Compromise запускает contamination и incident process §11.

## 8. Равная помощь и clarifications

- До assignment обе arms получают frozen start/training package одинакового outcome-level scope и эквивалентного времени ознакомления.
- Общие clarification question/answer одновременно поступают всем затронутым teams через append-only ledger.
- Preregistered question canonicalization/redaction pipeline удаляет необязательные stack/arm cues; leakage test и R-16 guess/confidence log предшествуют ruling.
- R-16 получает coded question без metrics/desired outcome и по frozen taxonomy/evidence rules определяет `task defect`, `normative ambiguity`, `implementation help` или `out-of-scope`; ruling подписывается до unblinding. Unplanned cue invalidates ruling; если arm identity inherent, применяются frozen deterministic rule либо два independent I2 stewards, disagreement → `inconclusive`.
- Exact paired-case/block mapping замораживается до assignment. Task defect инвалидирует только заранее mapped counterpart block; если counterpart отсутствует, affected contrast inconclusive по frozen SAP. Normative ambiguity входит в effort/evidence и получает versioned clarification одновременно для всех. Implementation help расходует одинаковый OQ-7 budget.
- Help channel, response deadline, число correction cycles, active expert time, AI/tool access и tuning budget одинаковы по arm; неиспользованный лимит не переносится другой стороне.
- Закрытая помощь, необходимая для interop, считается ambiguity/effort и не может быть удалена из evidence.
- На R-16 ruling допустима одна appeal по §10 только при новых фактах либо доказанной ошибке применения taxonomy/process.

## 9. Assignment and implementation independence

- OQ-6B заранее задаёт candidate-neutral competence dimensions, minimum eligibility, matching tolerance, prior-exposure balance и randomization внутри qualified blocks. Team size/role mix одинаковы; одна identity/controller не входит в обе arms. Дисбаланс запрещает positive comparative claim и сужает population claim до sampled competence profile.
- Best-deployable regime даёт каждой arm strongest pinned declared public tooling. Common-substrate clean-room regime запрещает обеим candidate-semantic reference code и даёт только frozen commodity substrate. Teams/datasets разделены либо действует preregistered washout/carryover rule; exposure логируется.
- Между teams одной arm запрещён AgentBridge/composition semantic code, private fixes и outcome hints. Общие commodity libraries допустимы по frozen list и не считаются semantic independence.
- После first artifact digest cross-implementation testing разрешено только через frozen interfaces; changes получают genealogy и correction-budget classification.
- Минимум две independently owned implementations нужны для interop; OQ-5B может требовать больше команд для statistical precision.

## 10. Identity-coded adjudication and appeal

Полное arm blinding может быть невозможно: protocol shapes и dependency names иногда раскрывают N/C. Поэтому документ обещает только проверяемое identity-coding и outcome/metric blinding, а не вымышленную слепоту.

1. Preregistered canonicalization/redaction pipeline создаёт minimum evidence packet; custodian заменяет identities codes и удаляет необязательные arm cues без удаления causal facts.
2. Leakage test до rulings измеряет arm inference; adjudicator отдельно записывает guess/confidence и любое фактическое unblinding.
3. Adjudicator получает только applicable frozen rules, minimum raw paths и fault controls; comparative metrics, advocate explanations и desired outcome скрыты.
4. Ruling выбирает только `candidate-caused`, `harness/environment-caused`, `unknown`, `common-foundation defect` и связанный verdict по OQ-3A/OQ-4.
5. Written rationale/digest подписываются до metrics/desired-outcome unblinding.
6. Unplanned leakage invalidates ruling и требует fresh minimum-packet adjudication. Если inherent arm identity скрыть нельзя, нужны два независимых I2 rule-bound adjudicators; их disagreement даёт `inconclusive`.
7. Conflict с Security Reviewer рассматривает fresh I2 adjudicator; неразрешённое расхождение даёт `inconclusive`, а не majority pass.
8. После unblinding исправление factual identity error возможно только новым versioned ruling с сохранением исходного.

На ruling допускается одна versioned appeal в frozen срок только при новых raw facts либо доказанной ошибке применения frozen rule/process. Её рассматривает fresh I2 adjudicator из исходного minimum packet плюс допустимое новое evidence, без thresholds/metrics/desired outcome. Outcome-shopping и повторная appeal запрещены; исходное решение сохраняется, unresolved dispute даёт `inconclusive`.

Security review также identity-coded и outcome/metric-blind, но не обещает скрыть inherent protocol/dependency identity. Paired canonical security packets и frozen rubric/checklist удаляют лишь необязательные cues; каждый R-09 записывает arm guess/confidence. Unplanned disclosure invalidates finding. Если arm inherent, два independent I2 rule-bound Security Reviewers выдают отдельные preserved findings; disagreement остаётся `inconclusive`, а не majority pass.

## 11. Contamination and recusal

| Событие | Последствие |
| --- | --- |
| Implementer видит exact holdout case/hint | Contaminate entire applicable stratum; primary evidence сохранён, positive use запрещён |
| Implementer видит private package/reference behavior другой arm | Independence affected implementation/stratum invalid; matched replacement только по preregistered rule |
| Advocate видит holdout/generator hint | Applicable stratum contaminated; systemic/seed exposure contaminates whole holdout |
| Seed/generator state или системный access barrier раскрыт | Contaminate entire holdout; один preregistered fresh tranche по OQ-3A |
| Oracle author получает Candidate design/output | Затронутые expected outcomes invalid до independent re-derivation |
| Security Reviewer получает unplanned arm disclosure/metrics/desired outcome до finding freeze | Finding invalid до fresh I2 review; inherent inference следует §10 |
| Adjudicator получает unplanned identity cue/metrics/desired outcome | Ruling invalid; fresh minimum-packet adjudication или `inconclusive` |
| Statistician видит arm labels до signed output | Analysis invalid; fresh statistician и preregistered rerun/analysis required |
| Advocate влияет на attribution/security closure | Finding/ruling invalid до fresh review |
| Red Team/Panel получает protected material раньше разрешённого | Их affected output не считается independent; holdout impact оценивается отдельно |
| Общая task/harness ошибка | Только preregistered mapped counterpart block invalid; без counterpart affected contrast `inconclusive` по SAP |
| Неясный exposure либо конфликт | `inconclusive` для затронутого claim |

Каждая роль обязана заявить конфликт до доступа и немедленно после его обнаружения. Recusal заменяет identity до просмотра новых protected materials; прежний доступ остаётся в ledger и проходит scope-impact review.

### Incident state machine

Любое подозрение проходит `suspected → contained → attributed/unknown → restored или invalidated`. На `suspected` affected access/runs pause, evidence immutably preserved, credentials revoked/rotated. Scope review охватывает backups, derived artifacts, caches и prompts; OQ-8 clock уведомляет назначенные роли, dissent сохраняется. Restoration независимо подтверждает controls и пересматривает downstream claims. Unknown attribution остаётся `inconclusive`; повторная contamination следует OQ-3A/OQ-5A No Decision. Evidence не удаляется и не relabelled.

## 12. Independence dimensions, levels and allowed claims

Independence оценивается по четырём осям: `A controller/organization`, `B storage/admin/credential-recovery`, `C AI provider/model/toolchain`, `D code/genealogy`. Разные аккаунты под одним administrator/provider root не разделяют соответствующую ось.

| Level | Доказательство | Максимальный claim |
| --- | --- | --- |
| I0 | Один контекст/operator без barriers | exploratory note only |
| I1 | Fresh contexts, separate prompts/artifacts, logged exposure; общий provider/root/workspace | internal corroboration with explicit common-mode limitation |
| I2 | Разные controller, admin и credential-recovery roots; enforced least-access stores; independently reproducible artifacts. Для AI-based security-critical cross-check/adjudication нужны разные provider failure domains либо одна независимо работающая human derivation | Gate 1 confirmatory evidence в frozen experimental scope |
| I3 | I2 плюс внешние организации/люди независимо реализуют public contract | Gate 3 external implementability/DX evidence, не adoption |

Положительный Gate 1 Decision Record требует I2 для oracle author/cross-check, holdout custody/witness, adjudication, security review, statistics и минимум двух implementation owners каждого оцениваемого candidate scope. Ни один admin/recovery root не контролирует одновременно protected source и independent verifier; custodian хранит ciphertext, threshold/two-party release отделяет custody от semantic decryption, и custodian не может единолично decrypt/alter evidence. Два supposedly independent члена одной security-critical пары под общим AI provider/model failure domain дают максимум I1; второй путь может обеспечить genuinely tool-independent human derivation с записанной genealogy. Общая commodity infrastructure сама по себе не понижает I2, если она не управляет, не наблюдает и не меняет protected input/output пары. Market adoption относится минимум к Gate 4 и не следует даже из I3 автоматически.

## 13. Evidence and publication

Evidence package содержит frozen role roster, incompatibility attestations, access-control configuration/tests, append-only access/exposure/help/genealogy logs, coded rulings, recusal/contamination records, assistance consumption, dissent и known common-mode risks. Неблагоприятное/invalid evidence сохраняется.

Private foundational files, secrets, production credentials/customer data и sealed holdout не входят в publishable Git. Внешняя публикация требует OQ-8 checks и отдельного решения Project Owner; невозможность раскрыть часть evidence явно сужает claim.

## 14. OQ-6A/OQ-6B acceptance

OQ-6A можно заморозить после independent governance, fairness и security reviews и осознанного принятия Project Owner.

Operational DAG:

1. OQ-7 provisional feasibility + OQ-8 control design;
2. `OQ-6B-Pilot` — минимальный isolated roster/team profile для outcome-blind neutral pilot;
3. neutral pilot → OQ-5B exact N/τ/budget;
4. `OQ-6B-Confirmatory` — полный roster, I2 controllers/stores, access tests и attestations, атомарно согласованные с OQ-5B;
5. OQ-3B authoring/sealing под уже назначенными separated roles;
6. только затем Candidate exposure/design/code.

OQ-7 содержит явный checkpoint доступности/стоимости I2 people/controllers/stores. Если текущие ресурсы его не проходят, результат — `pause / seek external independence`; I1 нельзя тихо выдать за confirmatory evidence. Независимость не назначается задним числом.

Смысловое изменение OQ-6A снимает freeze и требует нового review/acceptance; замена OQ-6B identity после protected exposure требует contamination and scope-impact review.

### OQ-6A freeze record

- **Frozen scope:** §§1–13 и staged DAG §14.
- **Authority/date:** Project Owner, 2026-09-13.
- **Independent reviews:** governance PASS, fairness PASS, security PASS; Critical/High/Medium/Low — 0/0/0/0 в каждом финальном review.
- **Change control:** смысловое изменение снимает freeze, создаёт новую version/preregistration и требует затронутых reviews плюс повторного принятия Project Owner.
- **Не разрешено:** OQ-6B-Pilot/Confirmatory, утверждение текущего I2, sealed holdout, OQ-5B/OQ-3B, Candidate design или code.
