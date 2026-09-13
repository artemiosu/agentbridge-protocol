# Candidate manifests — OQ-1 selection frozen

OQ-1 фиксирует личности, границы и базовые версии Composition Challengers. Это **не executable manifest** и не разрешение на проектирование или код. Полные dependency locks, реализации, public mappings, licenses/IP, SBOM и test corpus должны быть заморожены в OQ-3B/OQ-8 до Candidate design.

## Selection protocol

1. Ledger EI-01–EI-25 и OQ-3A oracle rules заморожены раньше кандидатов.
2. По публичным первичным источникам выполнен freshness review.
3. Отсутствующая функция учитывается как gap, а не исключается из общего scope.
4. Custom objects, state, mappings, policy, storage, trust и runtime dependencies входят в стоимость композиции.
5. Primary C выбран после hard gates по покрытию Ledger, объёму нового нормативного glue, независимой развёртываемости и практической стоимости.
6. Любое смысловое изменение C1/C2 снимает OQ-1 freeze и требует повторных research, independent review и принятия Project Owner.

## Candidate N — Native AgentBridge

| Поле | Frozen boundary |
| --- | --- |
| Manifest ID | `N-draft-0` |
| Нормативная роль | Полноценный role-neutral semantic Core + необходимые Native Bindings/Profiles/Extensions для frozen scenarios |
| Запрещённая скрытая зависимость | Обязательный внешний agent runtime, AgentBridge cloud/broker/registry или единственный provider |
| Допустимое переиспользование | Стандартные transport, cryptography, identity и policy primitives через явные Bindings |
| Допуск Core-элемента | Только Allocation + Removal Test против frozen Ledger |
| Статус | design-blocked до OQ-3B и OQ-2B |

Выбор C1/C2 не превращает их компоненты в обязательные зависимости Native Core. Они остаются сильнейшими контрольными альтернативами и возможными fail-closed bridges.

## C1 — Primary Composition Challenger

| Область | Frozen OQ-1 selection |
| --- | --- |
| Manifest ID | `C1-oq1-m1` |
| Inter-agent task lifecycle | A2A v1.0.0, commit `173695755607e884aa9acf8ce4feed90e32727a1`; один владелец task state, MCP Tasks отключены |
| Tool/context boundary | MCP 2026-07-28, commit `5f5440bb26a62e2cf3440b92da5a667efa03b267` |
| HTTP/workflow descriptions | OpenAPI 3.2.1 + Arazzo 1.1.0; descriptive-only, без runtime semantic credit |
| Authority/token baseline | RFC 9700, 8707, 9396, 8693, 9449; DPoP обязателен на protected paths, Bearer fallback запрещён |
| Policy decision | AuthZEN Authorization API 1.0 Final; application vocabulary остаётся mandatory public glue |
| Change/revocation signals | SSF/CAEP 1.0 Final; сигнал может только изменить/сузить доступ и не создаёт permit |
| Commerce | UCP v2026-08-25, commit `cd78fb38e819de77d9b527d110476eccb876f1bd`, canonical REST state; MCP/A2A — adapters |
| Consequential commerce | AP2 v0.2.0, commit `b4587ac1d055888a73b4b21750973cffba961793`; только ограниченная checkout/payment scope |
| Evidence | Protocol-neutral action/effect evidence semantics обязательны; SCITT RFC 9943 + COSE Receipts RFC 9942 — optional carrier |
| Статус | Primary C identity frozen; executable closure blocked до OQ-3B/OQ-8 |

### C1 mandatory public glue

- канонические Participant/Principal/Actor и cross-protocol identity mapping;
- authority cell, attenuation algebra, exact decision-subject digest и approval lifecycle;
- единая связь task, execution, effect и outcome states;
- idempotency, concurrency/shared-limit и partition behavior;
- revocation/staleness rules на каждой protected boundary;
- directed versioned mappings с provenance и явной semantic loss;
- protocol-neutral evidence statements, retention/retrieval и dispute boundaries;
- полный fail-closed failure/recovery table и dependency closure.

COAZ-MCP Draft 1 **исключён как готовое glue**: он несовместим без изменений с MCP 2026-07-28. Нужен обновлённый публичный mapping с независимой проверкой.

## C2 — GNAP Authority Challenger

| Область | Frozen OQ-1 selection |
| --- | --- |
| Manifest ID | `C2-oq1-m1` |
| Прикладные слои | Те же A2A/MCP/OpenAPI/Arazzo/UCP/AP2 boundaries, что у C1 |
| Authority plane | GNAP RFC 9635 + GNAP Resource Servers RFC 9767 вместо OAuth issuance/RAR/Token Exchange/DPoP |
| Запрещённая композиция | Одновременный mandatory OAuth и GNAP authority plane |
| Дополнительный glue | GNAP→AuthZEN mapping, rights vocabulary, key-bound access, continuation/token management, RFC 9767 introspection и failure behavior |
| Статус | Условно проходит independent-implementability; не является deploy-today baseline; executable closure blocked до OQ-3B/OQ-8 |

Недостающие функции существующей GNAP-реализации нельзя исключить из проверки: они должны быть независимо реализованы либо C2 теряет eligibility перед experiment freeze.

## Required AGNTCY bounded experiment

AGNTCY не является отдельным semantic candidate и не становится mandatory runtime dependency C1. OQ-3B должен задать ограниченный augmentation/substitution experiment, чтобы проверить, улучшает ли он C1 по routing, identity, discovery, secure messaging и observability без сокрытия дополнительной сложности и стоимости.

Исходные пины: DIR v1.6.2 commit `ad2d2125eaed30b57d04838d9db56bd12386924b`; SLIM repository snapshot `16151eefd31195dcc600b359c2d401926e871d24`, где компонент `slim-version` имеет v2.3.2. Точный component BOM и совместимость набора остаются обязательным условием OQ-3B.

## ANP reserve

ANP 1.1, commit `6fc3854ca15f453fa607360844b3a98120f74284`, сохраняется как reserve благодаря широкому покрытию identity, naming, discovery, messaging, E2EE и federation. Он не входит в текущий Challenger Set, пока immutable released-only BOM, cross-layer mappings, governance/IP и conformance evidence не пройдут те же hard gates.

## Excluded as cores

- ACP beta — compatibility adapter, а не вторая canonical commerce model.
- x402 v2 — optional paid API/data/compute rail; stable immutable pin не подтверждён.
- Visa TAP — optional scheme edge integration, не mandatory independent trust root.
- BeeAI ACP — объединён с A2A.
- OpenAPI/Arazzo — описания, а не runtime executor или authority/effect semantics.

## Remaining manifest obligations

Для C1, C2 и каждого фактически проверяемого варианта до experiment freeze обязательны:

- immutable dependency/build locks, retrieval date, content digests и SBOM;
- licenses, patent/IP status и разрешённое экспериментальное использование;
- реализации/libraries, shared genealogy и independent implementation plan;
- все runtime components, state stores, trust/data boundaries и failure domains;
- поддерживаемые topology/mode/domain и точная claim boundary;
- mappings, storage, known hazards, unresolved issues и полный recovery behavior;
- owner, independent reviewer и append-only change history.

## Decision record

Project Owner принял OQ-1 2026-09-13 после technical research, independent selection/red-team, version/compatibility audit, semantic citation verification и structure/prose review. Freeze manifest: `oq1-freeze-manifest.md` (`OQ1-M1`).
