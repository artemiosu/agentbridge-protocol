---
title: AgentBridge AA-1 — Glossary and Context Boundaries
status: final
created: 2026-09-16
updated: 2026-09-16
scope: Architecture Baseline v0.x
authority:
  - AD-1
  - AD-2
  - AD-3
---

# Glossary and Context Boundaries

## 1. Назначение

Этот документ фиксирует общий язык Architecture Baseline v0.x. Термины описывают нормативный смысл протокола, а не классы SDK, JSON-поля, базы данных или внутренние компоненты реализации.

Если конкретный Profile, Extension, Binding или Bridge использует другое имя, его mapping к этому словарю должен быть явным. Сходство названий или структуры данных не доказывает равенство смысла.

## 2. Участники, роли и адресация

| Термин | Каноническое значение |
| --- | --- |
| **Participant** | Независимо адресуемая сторона взаимодействия, которая отправляет, получает или проверяет протокольные утверждения. Participant может представлять человека, организацию, агента, сервис или инфраструктурный компонент. |
| **Principal** | Человек или организация, чьи интересы, ресурсы или полномочия затрагивает действие. Principal не обязан непосредственно отправлять сообщение. |
| **Actor** | Субъект, который совершает конкретное протокольное или внешнее действие. Actor не обязательно является Principal. |
| **Presenter** | Сторона, предъявляющая credential, grant, claim или иное authority material. Предъявление не доказывает, что Presenter является issuer, Principal или допустимым Actor. |
| **Initiator** | Роль Participant, начинающего конкретное взаимодействие или ветвь. Это контекстная роль, а не постоянный тип клиента. |
| **Responder** | Роль Participant, отвечающего в конкретном взаимодействии или ветви. Responder может инициировать обратный или новый поток. |
| **Delegate** | Actor, которому Principal или иной уполномоченный субъект передал ограниченную часть полномочий. Делегирование не может само по себе расширять Authority. |
| **Executor** | Сторона, непосредственно пытающаяся совершить Consequential Effect. Она может отличаться от Initiator, Delegate и владельца ресурса. |
| **Approver** | Сторона, чьё Approval требуется применимым Profile или policy. Несколько идентификаторов под общим controller или authority root не считаются независимыми Approvers без отдельного доказательства. |
| **Policy Authority** | Внешний источник policy decision или policy material. Его ответ является входом в Authorization, а не автоматическим доказательством исполнения или эффекта. |
| **Attestor** | Сторона, выпускающая Claim о наблюдаемом факте. Attestor отвечает только за точно заявленный subject, метод наблюдения и assurance. |
| **Audience** | Точно определённый получатель, verifier или effect boundary, для которого предназначены сообщение, Authority или Claim. |
| **Resource** | Объект, данные, возможность или внешний актив, к которому относится взаимодействие или Authority. |
| **Role** | Ограниченная конкретным Context функция Participant. Роль не выводится из владельца, DNS-имени, transport position или ярлыка `agent`, `service`, `client`, `server`. |

## 3. Взаимодействие и идентичность

| Термин | Каноническое значение |
| --- | --- |
| **Message** | Одна передаваемая протокольная единица. Получение Message не доказывает принятие, исполнение или внешний эффект. |
| **Exchange** | Ограниченная последовательность связанных сообщений с явно заданными правилами доставки и завершения. |
| **Interaction** | Нормативно связанный набор Exchanges, ветвей и состояний, объединённых целью и Context. |
| **Branch** | Отдельная причинно связанная часть многостороннего или разветвлённого Interaction. Несвязанные ветви не раскрываются автоматически друг другу. |
| **Correlation** | Явная связь объектов для поиска или группировки. Correlation сама по себе не доказывает Causation, ordering или authority. |
| **Causation** | Нормативное утверждение, что один объект или переход вызвал другой в установленной модели. Совпадение времени или correlation identifier этого не доказывает. |
| **Message ID** | Идентичность конкретного Message. Не заменяет Logical Operation ID, Delivery Attempt ID, Execution Attempt ID или Decision Subject. |
| **Logical Operation ID (`Operation ID`)** | Стабильная идентичность одной логической Operation и её outcome-retrieval scope. Она сохраняется через разрешённые retry/replay и все связанные delivery/execution attempts при неизменном Decision Subject. Повторное использование с изменённым смыслом запрещено. |
| **Delivery Attempt ID** | Идентичность одной попытки передать либо повторно доставить Message в рамках Logical Operation. Новая Delivery Attempt не создаёт новую Operation, Authority или разрешённый Effect. |
| **Execution Attempt ID** | Идентичность одной попытки обработать или исполнить ту же Logical Operation. Она обязана ссылаться на Logical Operation ID и Decision Subject; новая Execution Attempt не разрешает дополнительный Effect. |
| **Decision Subject** | Точная неизменная семантическая идентичность того, что предлагается, разрешается, одобряется или исполняется: действие, параметры, Actor/цепочка, Audience, Resource, Context, constraints, schema/version и применимые сроки. Security-critical изменение создаёт новый Decision Subject. |
| **Epoch** | Версионированная граница актуальности security-relevant состава, полномочий, policy, membership, ключей или правил. Материал из прежней Epoch не считается актуальным автоматически. |

## 4. Согласование и эволюция

| Термин | Каноническое значение |
| --- | --- |
| **Capability** | Заявление о поддерживаемой возможности. Оно не является доверием, Authority или разрешением использовать возможность. |
| **Requirement** | Условие, которое должно быть выполнено до зависимого обмена или claim. Неудовлетворённое обязательное Requirement приводит к safe refusal. |
| **Negotiated Set** | Один свежо согласованный набор версий, Profiles, Extensions, Binding properties и limits, удовлетворяющий requirements и security floor зависимых сторон. |
| **Core** | Минимальный набор универсальной, implementation-neutral и независимо проверяемой семантики, необходимой для безопасной совместимости во всех обязательных топологиях. |
| **Profile** | Версионированное сужение и усиление Core для явно названного класса применения. Profile выбирает обязательства и policy floor, но не ослабляет и не переопределяет Core. |
| **Extension** | Явно согласуемая additive-возможность с namespace, версией, зависимостями, safety classification и unknown-handling. Extension не может молча стать обязательной или расширить Authority. |
| **Binding** | Версионированное отображение Core/Profile semantics в конкретные transport, encoding, framing и security mechanisms. Binding переносит смысл, но не создаёт его. |
| **Native Binding** | Binding, позволяющая двум нативным реализациям AgentBridge взаимодействовать без другого agent-protocol runtime, обязательного Bridge или коммерческой платформы. |
| **Bridge** | Необязательное направленное и version-pinned преобразование между AgentBridge и внешним протоколом. Bridge раскрывает semantic loss и assurance ceiling и прекращает зависимое действие при потере security-critical смысла. |
| **External Infrastructure** | Заменяемые внешние identity, policy, discovery, audit, payment, domain, storage, time или shared-limit системы. Они могут владеть внешней истиной или enforcement, но не скрыто менять нормативный смысл Core. |
| **Conformance Claim** | Ограниченное утверждение о соответствии точных artifacts, versions, dependency closure и scope определённому набору нормативных обязательств. Claim не переносится на более широкий scope автоматически. |

## 5. Полномочия и решения

| Термин | Каноническое значение |
| --- | --- |
| **Authority** | Актуальное явное право указанного Actor совершить точно ограниченное действие или раскрытие для Audience, Resource, Context и Epoch в пределах time/use/amount/rate constraints. |
| **Delegation** | Явная передача части Authority другому Actor с сохранением или сужением всех применимых ограничений и provenance. |
| **Consent** | Выраженная воля Principal относительно точного Decision Subject. Consent не равен технической возможности исполнения или окончательной Authorization. |
| **Approval** | Требуемое Profile/policy решение Approver по точному Decision Subject. Предварительное согласие или negotiation response не является Approval. |
| **Authorization** | Решение, что Actor может совершить указанное действие при текущих Authority, policy, Context, Epoch и preconditions. Оно не доказывает, что действие было исполнено. |
| **Obligation** | Условие, которое должно быть выполнено до, во время или после действия, с указанными responsible/enforcing party, evidence, сроком и последствиями нарушения. |
| **Revocation** | Изменение, запрещающее новые действия в затронутой Authority scope. Revocation не переписывает прошлые эффекты и не доказывает остановку уже начатого исполнения. |
| **Protected Disclosure** | Раскрытие данных, metadata, identifier, linkability signal, credential или branch state, требующее явного permit и проверки на границе раскрытия. |
| **Consequential Effect** | Внешнее изменение состояния, прав, обязательств, ресурсов, средств или положения Principal/организации, для которого требуется отдельная safety/effect boundary. |

## 6. Исполнение, эффект и восстановление

| Термин | Каноническое значение |
| --- | --- |
| **Delivery** | Подтверждение доставки или транспортной доступности Message. Не означает Acceptance, Authorization, Execution или Effect. |
| **Acceptance** | Подтверждение, что получатель принял Operation в обработку в заявленном scope. Не доказывает Authorization или Effect. |
| **Execution** | Попытка исполнить Operation. Начало или окончание Execution не всегда равняется внешнему Effect. |
| **Effect** | Изменение внешнего состояния. Его истинность принадлежит применимому effect boundary/system of record, а протокол переносит только ограниченные Claims и Evidence. |
| **Outcome** | Нормативно ограниченное состояние знания о выполнении и Effect. Степень определённости Outcome не может повышаться без заранее определённого trust rule и Evidence. |
| **Partial** | Подтверждено выполнение или Effect только части Decision Subject. Partial не является полным success или полным zero-effect. |
| **Unknown** | Доступного Evidence недостаточно, чтобы честно установить нужную safety/effect projection. Unknown не означает failure, success или отсутствие Effect. |
| **Conflict** | Два или более применимых Claims несовместимы, а нормативное правило не позволяет выбрать один. Conflict сохраняется явно до разрешения. |
| **Retry** | Новая Delivery Attempt или Execution Attempt той же Logical Operation с новым соответствующим Attempt ID, но с теми же Logical Operation ID и Decision Subject; retry не создаёт новый разрешённый Effect. |
| **Replay** | Повторно наблюдаемое ранее сформированное сообщение или запрос. Replay должен вернуть связанный Outcome либо быть безопасно заблокирован, но не создавать новый commit. |
| **Cancellation** | Запрос прекратить ещё не завершённое исполнение. Cancellation не доказывает отсутствие предшествующего или позднего Effect. |
| **Compensation** | Новое отдельно разрешённое действие, пытающееся смягчить или обратить последствия прежнего Effect. Оно имеет собственные Outcome и возможность failure и не переписывает историю. |

## 7. Claims и Evidence

| Термин | Каноническое значение |
| --- | --- |
| **Claim** | Атрибутированное утверждение issuer/source о точном subject в указанном scope, времени и assurance. Claim не становится внешней истиной только потому, что подписан. |
| **Evidence** | Набор проверяемых Claims, provenance и observations, используемый verifier по явному trust rule. Evidence может быть неполным, конфликтующим или redacted. |
| **Receipt** | Claim о принятии, наблюдении, исполнении или ином событии, выпущенный определённой стороной. Вид Receipt обязан указывать, что именно он подтверждает и чего не подтверждает. |
| **Observer** | Компонент или сторона, фиксирующая протокольное/внешнее событие для проверки. Неполный или вмешивающийся Observer не может дать положительный conformance verdict. |
| **Assurance** | Ограниченная степень обоснованности Claim с явными trust, freshness, mechanism и scope assumptions. Bridge или преобразование не может повышать Assurance. |
| **Provenance** | Проверяемая история происхождения и допустимых преобразований Claim, Evidence, artifact или protocol object. |

## 8. Context Boundaries

Один универсальный `context`-объект не допускается. Независимые реализации обязаны различать следующие нормативные области, даже если конкретный Binding кодирует их совместно.

| Context | Что связывает | Чего не доказывает |
| --- | --- | --- |
| **Negotiation Context** | Bootstrap rules, transcript, versions, Profiles, Extensions, Binding properties, limits и выбранный Negotiated Set | Authority, Effect или domain truth |
| **Interaction Context** | Exchanges, branches, Participants, addressing, correlation, causation, delivery/order/termination scope | Право совершить действие или его успех |
| **Decision Context** | Точный Decision Subject, proposals, Consent, Approval, Authorization и Obligations | Execution или Effect |
| **Authority Context** | Principal, Actor/presenter, Audience, Resource, constraints, delegation chain, policy и Epoch | Факт исполнения или внешнюю истину |
| **Operation Context** | Logical Operation ID, Delivery/Execution Attempt IDs, lifecycle, retries, cancellation, dependencies и recovery | Сам по себе — внешний Effect |
| **Effect Context** | Effect boundary, system of record, observed state, Partial/Unknown/Conflict и применимая atomicity | Истину за пределами названной границы |
| **Evidence Context** | Claim subject, issuer/source, Observer/Attestor, provenance, freshness, Assurance и conflicts/gaps | Безусловную истинность Claim |
| **Privacy Context** | Purpose, disclosure/linkability sets, recipients/channels, classification, retention, deletion, residency и redaction bounds | Право на дальнейшее использование вне scope |
| **Dependency Context** | Artifact/dependency closure, controllers, versions, provides/requires/terminates, failure и compromise assumptions | Заменяемость, если она отдельно не доказана |
| **Conformance Context** | Точные artifacts, versions, Profile/Binding, environment, oracle/observer coverage, claim scope и expiry | Production safety, adoption или статус стандарта |

Пересечение Context boundaries разрешено только через явно определённую связь. Например, Operation ссылается на Decision Subject и Authority Context, но не копирует их так, чтобы stale snapshot мог выдать новую Authorization.

## 9. Запрещённые смешения

Следующие пары и группы всегда различаются нормативно:

- Principal ≠ Participant ≠ Actor ≠ Presenter ≠ Executor;
- organizational label ≠ Role;
- transport `client/server` ≠ protocol Initiator/Responder;
- identity/authentication ≠ Authority/Authorization;
- discovery/capability declaration ≠ trust/permit;
- proposal/counter-proposal/preliminary acceptance ≠ Consent ≠ Approval ≠ Authorization;
- Message ID ≠ Logical Operation ID ≠ Delivery Attempt ID ≠ Execution Attempt ID ≠ Decision Subject;
- Correlation ≠ Causation ≠ ordering;
- Delivery ≠ Acceptance ≠ Authorization ≠ Execution ≠ Effect ≠ Outcome;
- timeout/missing response ≠ failure ≠ zero Effect;
- signature ≠ truth;
- Receipt ≠ proof of external Effect, если Receipt явно не относится к квалифицированной effect boundary;
- Cancellation ≠ rollback;
- Compensation ≠ стирание прежнего Effect;
- retry/replay ≠ новая Logical Operation; retry создаёт новый соответствующий Attempt ID;
- Profile ≠ Extension ≠ Binding ≠ Bridge;
- Bridge compatibility ≠ Native conformance;
- reference implementation behavior ≠ нормативная спецификация;
- AA-6 Architecture Baseline ≠ реализованный протокол;
- AA-7 implementation candidate ≠ production-ready, adopted или industry standard.

## 10. Обязательные топологии

Один и тот же Core обязан сохранять определения и safety semantics как минимум в следующих топологиях:

1. agent ↔ service;
2. agent ↔ agent, включая разных владельцев;
3. service/backend ↔ service/backend (B2B);
4. reverse asynchronous communication;
5. streaming и long-running interaction;
6. multi-party/delegated chains;
7. interaction с заменяемой discovery, identity, trust, policy, audit и effect infrastructure через явные contracts.

Организационные категории используются только для сценариев проверки. Они не образуют отдельные wire-роли или разные версии Core.
