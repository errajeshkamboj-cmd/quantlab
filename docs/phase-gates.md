# Phase Gates — NIFTY QuantLab (Phase 1)

- **Status:** Entry/exit criteria for all phases, consolidated from `Prompts.md`.
- **Last updated:** 2026-10-09
- **Rule:** do not silently skip a phase gate or begin a later phase just because it seems convenient (Prompts.md). A phase is not complete because files exist; its acceptance criteria must be met.

| Phase | Name | Entry criteria | Exit gate | Status (2026-10-09) |
|---|---|---|---|---|
| 1 | Requirements & Research Design | Owner instruction to start; repository inspected | Trader-reviewed requirements, explicit open questions, data feasibility plan | Docs delivered; trader review pending |
| 2 | Data Access & Market Data Foundation | Phase 1 gate passed | Validated, documented data pipeline and data-quality checks | Planned |
| 3 | Indicator & Signal Research | Phase 2 gate passed; trader-approved indicator definitions | Tested calculations, reproducible features, no look-ahead leakage | Planned |
| 4 | Strategy Definition & Backtesting Engine | Phase 3 gate passed; trader-reviewed hypotheses | Realistic costs, reproducible runs, out-of-sample evaluation | Planned |
| 5 | Risk Engine & Safety Controls | Phase 4 gate passed; trader-approved risk definitions | Independent risk checks and failure-mode tests | Planned |
| 6 | Trader Dashboard & Manual Decision Support | Phase 5 gate passed | Explainable signals, visible data freshness, human control | Planned |
| 7 | Paper Trading & Operational Validation | Phase 6 gate passed | No live order path, auditable simulations, readiness review | Planned |
| 8 | Controlled Broker Integration & Future Automation (GATED) | Phases 1–7 passed + documented validation + approved risk policy + completed reviews + broker terms checked + **explicit owner authorization** | Explicit authorization, safety review, broker reconciliation, rollback plan | Gated / disabled |

## Phase 8 preconditions (all must hold; otherwise the phase stays blocked)

1. Phases 1–7 have passed their applicable acceptance criteria.
2. Strategy validation and limitations documented.
3. Risk policy approved.
4. Paper-trading and operational reviews complete.
5. Broker API capabilities and current terms checked against official documentation.
6. The owner explicitly authorizes the phase.

If any precondition is absent: do not enable live orders; continue safe documentation, simulation, or tests and report the blocker.

## Cross-phase safety rules (apply always)

- No real orders in Phases 1–7.
- Paper-trading code must not have a path that can accidentally submit real orders.
- No secrets in git; `.env.example` with placeholders only.
