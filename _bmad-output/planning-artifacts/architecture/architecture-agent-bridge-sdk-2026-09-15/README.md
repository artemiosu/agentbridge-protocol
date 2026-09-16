---
title: AgentBridge AA-1 Architecture Constitution Package
status: final
updated: 2026-09-16
---

# AA-1 package status and reading path

Owner decisions AD-1, AD-2, AD-3 and AD-3A are adopted. All required internal AA-1 reviews pass, the Project Owner accepted the final package, and AA-1 Gate is `PASS`. Only documentary AA-2 is authorized; executable work and reference implementation remain blocked by their later Gates.

| Reader need | Start here | Then use |
| --- | --- | --- |
| Project Owner | `OWNER-ARCHITECTURE-GUIDE.md` | `AA-1-GATE-RECORD.md` |
| Protocol designer or builder | `ARCHITECTURE-SPINE.md` | `GLOSSARY-AND-CONTEXT.md`, `AA-2-NORMATIVE-MODEL-CONTRACT.md` |
| Requirements or conformance reviewer | `ALLOCATION-MATRIX.md` | `QUALITY-ATTRIBUTE-SCENARIOS.md`, `AA-7-DIVERSITY-ELIGIBILITY.md` |
| Security or privacy reviewer | `ASSETS-ADVERSARIES-BOUNDARIES.md` | `QUALITY-ATTRIBUTE-SCENARIOS.md`, `AA-2-NORMATIVE-MODEL-CONTRACT.md` |
| Decision auditor | `decisions/AD-1.md` through `decisions/AD-3.md` | `ADR-DISCIPLINE.md`, `AA-1-GATE-RECORD.md` |
| Landscape or dependency reviewer | `LANDSCAPE-DISPOSITION.md` | decision ADRs and the Gate record |

The `reviews/` directory preserves internal I1 findings and closure evidence. It is review history, not normative protocol text.
