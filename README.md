# NIFTY QuantLab

> **Research-first NIFTY options strategy discovery, validation, and trader decision support.**
> Initial focus: hedged NIFTY 50 option selling. Initial mode: research, backtesting, paper trading, and full human control.

## Current status

- **Current phase:** Phase 1 — Requirements & Research Design
- **Overall status:** Planned / not yet verified against implementation
- **Last updated:** 2026-10-09
- **Latest verified repository state:** Starter documentation prepared; implementation status must be confirmed by the coding agent after inspecting the repository.
- **Live order execution:** Disabled / out of scope for Phases 1–7
- **Current priority:** Convert trading ideas into explicit, testable hypotheses and verify historical data availability.

> Agent instruction: Update this section after every meaningful task. Never leave stale claims here. Use the actual current date and actual repository/test state. Do not claim work is complete unless it is implemented and verified.

## Latest developments

Newest first. The coding agent must replace these starter entries with actual repository changes as work proceeds.

- **2026-10-09 — Project plan prepared:** Defined an eight-phase roadmap from requirements and data research through gated broker integration.
- **2026-10-09 — Agent workflow prepared:** `Prompts.md` defines the phase-by-phase instructions, safety rules, acceptance criteria, and required documentation updates.
- **Not yet verified:** No implementation or tests are claimed by this starter README. The agent must inspect the repository and update this log.

## Owner action required

The following questions need trader/owner input. Mark each as Open, Answered, or Deferred and record the answer/date.

1. **Trend definition:** Which trend measure should be researched first?
2. **PCR definition:** Use OI-based PCR, volume-based PCR, or compare both?
3. **Hedged structures:** Which structures should be included in the first research batch?
4. **Strike/expiry selection:** What selection rules should be investigated?
5. **Trend reversal:** How should a reversal be confirmed?
6. **Daily loss limit:** Does the proposed ₹5,000 limit include unrealized P&L, charges, and slippage?
7. **Existing positions:** What should happen to open positions when the daily loss limit is reached?
8. **Historical data:** What permitted NIFTY options data is available at five-minute resolution, for what date range, and at what cost?
9. **Evaluation criteria:** Beyond the aspirational win-rate goal above 55%, what drawdown, profit factor, net return, and trade-count requirements should be considered?
10. **Risk scope:** Does the proposed ₹5,000 per-trade loss limit apply per leg, per strategy position, or to the complete strategy?

If none of these are currently needed for the next safe task, proceed with independent research/documentation and keep the unanswered items visible. Do not invent answers.

## Project overview

NIFTY QuantLab aims to provide a disciplined workflow:

1. Define trading hypotheses with the trader.
2. Obtain and validate permitted historical/live market data.
3. Research trend, Open Interest (OI), Put-Call Ratio (PCR), volume, volatility, and relevant Greeks.
4. Backtest explicit strategies with realistic costs and execution assumptions.
5. Apply independent risk controls.
6. Present explainable candidate trades for human review.
7. Validate through paper trading.
8. Consider controlled broker integration only after a separate readiness review and explicit authorization.

### Initial assumptions and preferences

These are **proposed starting points**, not validated trading rules:

- Instrument: NIFTY 50 options.
- Strategy family: hedged option selling.
- Preferred interval: five-minute data.
- Preferred expiry: weekly.
- Initial capital assumption: ₹5,00,000.
- Proposed maximum loss per trade: ₹5,000.
- Proposed daily loss limit: ₹5,000.
- Win-rate goal: above 55%, not a guarantee and not a sufficient measure of profitability.
- Historical dataset: none confirmed at project start.
- Initial control: all entries/exits remain human-controlled; no automated live order submission.

## Phase tracker

| Phase | Name | Status | Exit gate |
|---|---|---|---|
| 1 | Requirements & Research Design | Planned | Requirements reviewed, open questions tracked, data feasibility plan documented |
| 2 | Data Access & Market Data Foundation | Planned | Reproducible ingestion and data-quality checks |
| 3 | Indicator & Signal Research | Planned | Tested calculations, explainable signals, no look-ahead leakage |
| 4 | Strategy Definition & Backtesting Engine | Planned | Realistic costs, reproducible runs, out-of-sample evaluation |
| 5 | Risk Engine & Safety Controls | Planned | Independent risk checks and failure-mode tests |
| 6 | Trader Dashboard & Manual Decision Support | Planned | Explainable signals, visible data freshness, human control |
| 7 | Paper Trading & Operational Validation | Planned | No real orders, auditable simulation, readiness review |
| 8 | Controlled Broker Integration & Future Automation | Gated / conditional | Explicit authorization, risk sign-off, reconciliation and rollback tests |

The agent must keep these statuses synchronized with actual work. Do not mark a phase complete because its documents or scaffolding exist; acceptance criteria must be met.

## Architecture direction

Keep responsibilities separated:

- **Data ingestion:** broker or other permitted sources, historical imports, normalization, validation.
- **Storage:** PostgreSQL or a justified alternative documented before changing the planned stack.
- **Indicators/features:** reproducible calculations with explicit timestamps and data lineage.
- **Strategy engine:** configurable, versioned rules; no undocumented thresholds.
- **Backtester:** chronological simulation, costs, realistic fills, and out-of-sample evaluation.
- **Risk engine:** independent ability to reject a candidate trade.
- **Paper execution:** isolated simulator with no live-order path.
- **Dashboard:** signal explanations, risk estimates, data freshness, manual decisions and audit history.
- **Broker integration:** disabled by default and gated behind explicit approval.

Do not adopt this architecture blindly if the repository already contains a sound implementation. Inspect first and document any changes.

## Safety principles

- No automated live orders in Phases 1–7.
- Phase 8 remains disabled until its prerequisites and explicit authorization are satisfied.
- Never commit credentials, API secrets, tokens, `.env` files, or private broker data.
- Missing, stale, or invalid data must not generate fabricated signals.
- Do not claim profitability without reproducible evidence.
- Evaluate net results after costs, win/loss sizes, profit factor, drawdown, worst-case behaviour, trade count, margin and capital utilization.
- Avoid look-ahead bias and overfitting.
- A strategy signal is not an order; risk checks and human approval govern action.

## How to work with Arena

1. Give Arena Agent mode access to this GitHub repository.
2. Ensure the agent reads `Prompts.md` and this `README.md` before changing files.
3. Use a short instruction, for example: **`Complete Phase 1`**.
4. For a review without implementation, use: **`Review Phase 1 against Prompts.md. Do not change files; report gaps and owner actions.`**
5. After each phase, inspect the diff and test report before accepting the work.
6. Do not ask the agent to complete multiple phases in one instruction unless you deliberately want to expand scope.

`Prompts.md` contains the complete scope, tasks, acceptance criteria, and permanent rules for all eight phases.

## How to run and test

> Commands are not yet verified against a concrete implementation. The agent must replace this note with exact commands after inspecting the repository and confirming the stack.

- **Install:** Not documented yet.
- **Run:** Not documented yet.
- **Tests:** Not documented yet.
- **Database setup:** Not documented yet.
- **Configuration:** Use an ignored local `.env` file and a sanitized `.env.example`; never commit secrets.

## Latest test results

- **Status:** Not yet verified.
- No tests are claimed as passed by this starter README.
- Every future update must state the exact command run and actual outcome. If tests were not run, say why.

## Blockers and risks

- Historical NIFTY options data availability, granularity, licensing and cost are not yet verified.
- Strategy entry, strike selection, exit, reversal and adjustment rules are not yet defined.
- The proposed daily/per-trade loss limits need exact operational definitions.
- Broker API capabilities, permissions, rate limits and costs must be checked against current official documentation before implementation.
- Six months of historical data is only an initial suggestion; sufficiency must be evaluated rather than assumed.

## Repository documentation

- [`Prompts.md`](Prompts.md) — Complete phase prompts and operating rules for Arena.
- `CHANGELOG.md` — Dated record of meaningful changes.
- `docs/requirements.md` — Requirements and unresolved decisions (to be created/maintained in Phase 1).
- `docs/strategy-hypotheses.md` — Research hypotheses (to be created/maintained in Phase 1).
- `docs/indicator-specification.md` — Indicator definitions (to be created/maintained in Phase 1).
- `docs/risk-policy-draft.md` — Draft risk policy (to be created/maintained in Phase 1).
- `docs/data-feasibility.md` — Data source assessment (to be created/maintained in Phase 1).
- `docs/backtest-methodology.md` — Backtesting methodology (to be created/maintained in Phase 1).
- `docs/phase-gates.md` — Phase entry/exit criteria (to be created/maintained in Phase 1).

## Changelog and decision logging

After every meaningful task or phase, update:
1. This README's **Current status**, **Latest developments**, **Owner action required**, **Blockers and risks**, **Latest test results**, and **Phase tracker**.
2. `CHANGELOG.md` with the date, changes, tests, and remaining work.
3. `docs/DECISIONS.md` when a meaningful technical or product decision is made.

Keep newest updates at the top. Do not erase previous history; append or reorder dated entries newest-first while preserving earlier entries.
