# NIFTY QuantLab

> **Research-first NIFTY options strategy discovery, validation, and trader decision support.**
> Initial focus: hedged NIFTY 50 option selling. Initial mode: research, backtesting, paper trading, and full human control.

## Current status

- **Current phase:** Phase 1 — Requirements & Research Design. **Documentation delivered; exit gate pending trader review.**
- **Overall status:** Phase 1 docs complete (`docs/` deliverables below). No application code exists yet; Phases 2–8 not started.
- **Last updated:** 2026-10-09
- **Verified repository state:** docs-only repository — `README.md`, `Prompts.md`, `CHANGELOG.md`, plus the Phase 1 deliverables in `docs/`. No code, no tests, no CI, no database.
- **Live order execution:** Disabled / out of scope for Phases 1–7 (Phase 8 gated).
- **Current priority:** trader review of the Phase 1 deliverables and the open owner questions below; then Phase 2 (verify data sources against official documentation).

> Agent instruction: update this section after every meaningful task. Never leave stale claims here. Use the actual current date and actual repository/test state. Do not claim work is complete unless it is implemented and verified.

## Latest developments

Newest first.

- **2026-10-09 — Phase 1 documentation delivered:** created `docs/requirements.md` (Confirmed/Proposed/Unknown/Blocked requirements, out-of-scope, acceptance self-check), `docs/strategy-hypotheses.md` (hypotheses only — no claims), `docs/indicator-specification.md` (explicit or Unknown definitions), `docs/risk-policy-draft.md` (proposed limits + unresolved semantics), `docs/data-feasibility.md` (required fields, coverage needs, candidate sources to verify, gaps), `docs/backtest-methodology.md` (mandatory metrics + look-ahead/overfitting safeguards), `docs/phase-gates.md` (entry/exit criteria for all 8 phases), `docs/DECISIONS.md` (Phase 1 decisions). Restructured this README and appended a `CHANGELOG.md` entry. **No code changed; no tests exist or were run** (documentation-only phase).
- **2026-10-09 — Project plan prepared:** defined an eight-phase roadmap from requirements and data research through gated broker integration.
- **2026-10-09 — Agent workflow prepared:** `Prompts.md` defines the phase-by-phase instructions, safety rules, acceptance criteria, and required documentation updates.

## Owner action required

The following questions need trader/owner input. Mark each as Open, Answered, or Deferred and record the answer/date. Do not invent answers.

0. **Review Phase 1 deliverables:** review `docs/requirements.md`, `docs/strategy-hypotheses.md`, `docs/indicator-specification.md`, `docs/risk-policy-draft.md`, `docs/data-feasibility.md`, `docs/backtest-methodology.md`, `docs/phase-gates.md` and confirm or adjust the **Proposed** items. — **Open**
1. **Trend definition:** which trend measure should be researched first? — **Open**
2. **PCR definition:** use OI-based PCR, volume-based PCR, or compare both? — **Open**
3. **Hedged structures:** which structures should be included in the first research batch? — **Open**
4. **Strike/expiry selection:** what selection rules should be investigated? — **Open**
5. **Trend reversal:** how should a reversal be confirmed? — **Open**
6. **Daily loss limit:** does the proposed ₹5,000 limit include unrealized P&L, charges, and slippage? — **Open**
7. **Existing positions:** what should happen to open positions when the daily loss limit is reached? — **Open**
8. **Historical data:** what permitted NIFTY options data is available at five-minute resolution, for what date range, and at what cost? — **Open**
9. **Evaluation criteria:** beyond the aspirational win-rate goal above 55%, what drawdown, profit factor, net return, and trade-count requirements should be considered? — **Open**
10. **Risk scope:** does the proposed ₹5,000 per-trade loss limit apply per leg, per strategy position, or to the complete strategy? — **Open**

If none of these are currently needed for the next safe task, proceed with independent research/documentation and keep the unanswered items visible.

## Blockers and risks

- **Historical NIFTY options data availability, granularity, licensing, and cost are unverified** (owner question 8). This gates all research; verification is a Phase 2 task. Data availability is not assumed.
- **Ten owner questions are open** (above); several definitions (trend, PCR, risk-limit semantics) block Phases 3–5.
- **No application code or tests exist yet** — nothing to run; Phase 1 was documentation-only by design. Test infrastructure is deferred until code exists.
- Strategy entry, strike selection, exit, reversal, and adjustment rules are not yet defined.
- Broker API capabilities, permissions, rate limits, and costs must be checked against current official documentation before implementation.
- Six months of historical data is only an initial suggestion; sufficiency must be evaluated rather than assumed.

## How to run and test

- **Application code:** none exists yet (Phase 1 was documentation-only). Nothing to install, run, or build.
- **Tests:** none exist and none were run — Phase 1 changed no code, so per `Prompts.md` Phase 1 task 9 no unit tests were added. Test commands will be documented when code exists (Phases 2–3).
- **Documentation verification:** review the files in `docs/` and inspect this phase's changes with `git show` / `git diff`.
- **Database setup:** not applicable yet (planned for Phase 2; see Architecture direction).
- **Configuration/secrets:** no secrets exist in the repo. Future configuration uses an ignored local `.env` plus a sanitized `.env.example`; never commit secrets.

## Phase tracker

| Phase | Name | Status | Exit gate |
|---|---|---|---|
| 1 | Requirements & Research Design | **Docs complete — gate pending trader review** | Requirements reviewed, open questions tracked, data feasibility plan documented |
| 2 | Data Access & Market Data Foundation | Planned | Reproducible ingestion and data-quality checks |
| 3 | Indicator & Signal Research | Planned | Tested calculations, explainable signals, no look-ahead leakage |
| 4 | Strategy Definition & Backtesting Engine | Planned | Realistic costs, reproducible runs, out-of-sample evaluation |
| 5 | Risk Engine & Safety Controls | Planned | Independent risk checks and failure-mode tests |
| 6 | Trader Dashboard & Manual Decision Support | Planned | Explainable signals, visible data freshness, human control |
| 7 | Paper Trading & Operational Validation | Planned | No real orders, auditable simulation, readiness review |
| 8 | Controlled Broker Integration & Future Automation | Gated / conditional | Explicit authorization, risk sign-off, reconciliation and rollback tests |

The agent must keep these statuses synchronized with actual work. Do not mark a phase complete because its documents or scaffolding exist; acceptance criteria must be met. Phase 1's documentation acceptance criteria are met; its exit gate additionally requires trader review, which is pending (see Owner action required).

## Latest test results

- **Status:** Not applicable — the repository contains no code and no tests.
- No tests are claimed as passed. Every future update must state the exact command run and the actual outcome; if tests were not run, say why.

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

These are **proposed starting points**, not validated trading rules (full classification in `docs/requirements.md`):

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

Do not adopt this architecture blindly if the repository already contains a sound implementation. Inspect first and document any changes. (No stack decision has been made yet — see `docs/DECISIONS.md` D5.)

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

## Repository documentation

- [`Prompts.md`](Prompts.md) — complete phase prompts and operating rules for Arena.
- [`CHANGELOG.md`](CHANGELOG.md) — dated record of meaningful changes.
- [`docs/requirements.md`](docs/requirements.md) — requirements classified Confirmed / Proposed / Unknown / Blocked; out-of-scope; acceptance self-check.
- [`docs/strategy-hypotheses.md`](docs/strategy-hypotheses.md) — research hypotheses (unvalidated by design).
- [`docs/indicator-specification.md`](docs/indicator-specification.md) — indicator definitions (explicit or Unknown).
- [`docs/risk-policy-draft.md`](docs/risk-policy-draft.md) — draft risk policy and unresolved loss-limit semantics.
- [`docs/data-feasibility.md`](docs/data-feasibility.md) — required data fields, coverage needs, candidate sources to verify, gaps.
- [`docs/backtest-methodology.md`](docs/backtest-methodology.md) — backtest metrics and look-ahead/overfitting safeguards.
- [`docs/phase-gates.md`](docs/phase-gates.md) — entry/exit criteria for all phases.
- [`docs/DECISIONS.md`](docs/DECISIONS.md) — record of meaningful technical/product decisions and reasons.

## Changelog and decision logging

After every meaningful task or phase, update:
1. This README's **Current status**, **Latest developments**, **Owner action required**, **Blockers and risks**, **How to run and test**, **Latest test results**, and **Phase tracker**.
2. `CHANGELOG.md` with the date, changes, tests, and remaining work.
3. `docs/DECISIONS.md` when a meaningful technical or product decision is made.

Keep newest updates at the top. Do not erase previous history; append or reorder dated entries newest-first while preserving earlier entries.
