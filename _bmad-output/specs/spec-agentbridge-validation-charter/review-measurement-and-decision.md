# Independent reviews — OQ-5A

Date: 2026-09-13  
Reviewed artifact: `measurement-and-decision.md`  
Final status: **OQ-5A accepted and frozen by Project Owner on 2026-09-13**

| Review | Initial findings | Final verdict |
| --- | --- | --- |
| Methodology/statistics | 3 Critical, 8 High, 5 Medium | **PASS — 0/0/0/0** |
| Fairness/adversarial | 2 Critical, 10 High, 4 Medium | **PASS — 0/0/0/0** |
| Security | 0 Critical, 5 High, 7 Medium | **PASS — 0/0/0/0** |

## Material corrections

- Full-team ITT RTTSI replaced ambiguous slice-weighted effort; noncompletion cannot appear fast.
- Power is designed at `R_alt=0.65`, separately from the 20% decision boundary.
- C1 is the primary contrast; C2, AGNTCY and ANP have frozen sensitivity/change-control roles.
- Necessity-first is disclosed as intentional PRD policy, not described as symmetric winner selection.
- Recovery, cellwise guardrails, tail precision, observer calibration and holdout have total decision rules.
- Aggregate resource/evidence budgets, privacy bounds, safe-stop and attribution preserve OQ-4.
- OQ-5A policy is separated from OQ-5B exact operational values, removing the OQ-3B dependency cycle.
- A `native` result is explicitly limited to technical/practical evidence in the frozen scope and is not market-adoption proof.

All three final re-reviews found no remaining Critical, High, Medium or Low findings. OQ-5B remains intentionally blocked; this PASS does not authorize Candidate design or code.
