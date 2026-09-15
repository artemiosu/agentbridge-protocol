---
title: "Course Decision Record: Native Architecture-First"
status: approved
date: 2026-09-15
decision_owner: Project Owner
visibility: private-repository
supersedes: existential Gate 1 direction decision
---

# Course Decision Record: Native Architecture-First

## Решение

Project Owner утвердил проектирование и последующую реализацию самостоятельного Native AgentBridge Protocol. Регулярная развилка `native/profile/upstream/stop` прекращается.

## Обоснование

Уже собранных исследований достаточно, чтобы перейти от проверки права проекта на существование к тщательному проектированию. Основной риск теперь — не отсутствие ещё одного сравнительного отчёта, а архитектурная ошибка в протоколе, предназначенном для безопасных действий и долгосрочной совместимости.

## Последствия

- Architecture разрешается как следующий BMAD-этап.
- Существующие протоколы остаются источниками механизмов, дефектов, compatibility и optional fail-closed bridges; они не становятся обязательными runtime-зависимостями Native Path.
- EI-01–EI-25, SBC-01–SBC-10 и принятые safety, evidence, independence, IP и publication policies сохраняются.
- Проверки могут заблокировать или вернуть на переработку конкретную архитектуру, возможность, реализацию, релиз или claim.
- Нерешённый обязательный security/IP/legal/evidence/independence blocker нельзя отменить продуктовым решением.
- Emergency pause/stop всего проекта сохраняется только для неустранимой небезопасности, юридической невозможности royalty-free Core, доказанной практически нереализуемой сложности или невозможности независимой реализации. Недостаток ресурсов означает pause/re-scope/seek resources/archive until funded; полный stop требует определённого evidence, применимой I2-рекомендации и отдельного решения Project Owner.

## Разрешено сейчас

- новая редакция PRD;
- новый Architecture Assurance Charter;
- BMAD Architecture, формальные/исполняемые модели, threat model, conformance design;
- bounded disposable prototypes после применимых rights/storage/egress/resource/observer controls.

## Не разрешено этим решением

- production implementation/release и реальные consequential effects;
- customer data или production credentials;
- расходы, договоры, регистрация компании/фонда/mark или передача прав без отдельного согласия;
- публичные claims `industry standard`, `production-ready` или `secure`;
- публикация приватных исходных документов.

Полный approved change package: [course-correction-native-architecture-first-2026-09-15.md](course-correction-native-architecture-first-2026-09-15.md).
