# NIFTY QuantLab — Arena Agent Prompts

This file is the operating manual for AI coding agents working on **NIFTY QuantLab**.

## How to use this file

1. Add this file and `README.md` to the root of the GitHub repository.
2. Give Arena Agent mode access to the repository.
3. To execute a phase, send a short instruction such as:
   - `Complete Phase 1`
   - `Complete Phase 2`
   - `Review Phase 3 and report blockers only`
4. The agent must read this file and the current `README.md` before acting.
5. Work on **one phase at a time**. Do not silently skip a phase gate or begin a later phase just because it seems convenient.

The command “Complete Phase N” means: inspect the repository, implement the phase's scope, run appropriate checks, update project documentation, and report what is done, what is not done, and what requires the owner's decision. It does **not** authorize live trading, paid services, account changes, or other external side effects.

---

## Permanent operating rules

### A. Start with repository discovery
Before editing:
- Read `README.md`, this `Prompts.md`, the project tree, package/configuration files, tests, and recent Git history.
- Identify the actual current phase and current implementation state. Do not assume the repository is empty or that previous work succeeded.
- Preserve existing working features unless a documented requirement calls for a change.
- Inspect existing architecture before choosing libraries or replacing components.
- Do not overwrite user work or perform destructive resets.

### B. Requirements and truthfulness
- Never invent trading rules, indicator thresholds, strategy results, API capabilities, or user approvals.
- Mark information as one of: **Confirmed**, **Proposed**, **Unknown**, or **Blocked**.
- If a critical requirement is missing, make safe progress on independent tasks and record the exact question in `README.md` under **Owner action required**. Do not guess.
- The trader is the domain expert; the engineer/agent implements and tests explicit rules.
- The win-rate goal above 55% is an aspiration, not a guarantee or the sole success metric.
- Evaluate profitability alongside costs, average win/loss, drawdown, profit factor, tail risk, trade count, and execution realism.
- Do not claim that a strategy works unless supported by reproducible results and documented validation.

### C. Trading safety — non-negotiable
- The initial product is research, backtesting, paper trading, and human-controlled decision support.
- **Do not place real orders. Do not implement or enable live order submission unless a later phase explicitly authorizes it and the owner confirms that authorization.**
- No live trading in Phases 1–7. Phase 8 is gated and must remain disabled until explicit approval and readiness checks.
- Paper-trading code must not have a path that can accidentally submit real orders.
- Never use real broker credentials in tests, commits, logs, examples, screenshots, or README content.
- Never commit `.env` files, API keys, passwords, access tokens, account IDs, or other secrets. Use `.env.example` with placeholders.
- Strategy signals are proposals, not orders. A separate risk layer must be able to reject a signal.
- Missing, stale, contradictory, or invalid data must lead to a safe no-signal state rather than an invented value.
- Never claim that risk controls eliminate trading risk.
- Do not advise bypassing broker rules, exchange rules, applicable law, or data licensing terms.

### D. Engineering standards
- Prefer small, reviewable changes over broad rewrites.
- Use clear module boundaries for data ingestion, data validation, indicators, strategies, backtesting, risk, paper execution, and user interface.
- Keep strategy rules configurable and versioned; do not bury them in UI code or hard-code undocumented assumptions.
- Add tests for calculations, risk rules, edge cases, data failures, and regressions.
- Use deterministic test data. Unit tests must not require live broker credentials or network access.
- Include type hints and useful comments where they clarify non-obvious logic.
- Handle API errors, rate limits, timeouts, retries, and duplicate ingestion safely.
- Use timezone-aware timestamps and document the market timezone.
- Avoid look-ahead bias: each decision may only use data available at that decision time.
- Document transaction-cost, spread, slippage, and fill assumptions in every backtest.
- Use migrations or documented schema changes for database evolution.
- Do not add dependencies without explaining why they are needed.
- Never report tests as passed unless they were actually run. If a test cannot run, state why.

### E. Required documentation updates after every task/phase
Update `README.md` whenever meaningful work is completed. Keep these sections near the top, in this order:
1. **Current status** — current phase, overall state, and last updated date.
2. **Latest developments** — newest completed work first, with commit/hash if available.
3. **Owner action required** — exact questions, approvals, credentials/setup tasks, or decisions needed from the user. If none, say `None currently`.
4. **Blockers and risks** — what prevents progress and its impact.
5. **How to run and test** — commands verified against the current repository.
6. **Phase tracker** — status of all phases and links to relevant files.

Also:
- Append a dated entry to `CHANGELOG.md` (create it if absent) for meaningful changes.
- Update the phase tracker in `README.md`.
- Record important choices and their reasons in `docs/DECISIONS.md` (create if useful).
- Add or update tests and test instructions.
- Keep a concise `docs/STATUS.md` if the repository needs more detail than the README.
- Do not say a feature is complete when it is only scaffolded or mocked.
- Do not put secrets or private broker data in documentation.

### F. Git and delivery
- Work in the current branch unless the user directs otherwise.
- Do not push, publish releases, deploy, buy services, or change account settings unless explicitly requested and supported by the available tools.
- Do not force-push or rewrite history.
- Inspect `git diff` before finishing.
- Keep changes scoped to the requested phase.
- At completion, provide a concise report: files changed, functionality delivered, tests run and outcomes, remaining work, owner actions, and suggested next phase.
- Do not mark a phase complete if its acceptance criteria are not met. Use `partial` or `blocked` and explain why.

---

## Phase tracker

| Phase | Name | Status at project start | Gate |
|---|---|---|---|
| 1 | Requirements & Research Design | Planned | Trader-reviewed requirements, explicit open questions, data feasibility plan |
| 2 | Data Access & Market Data Foundation | Planned | Validated, documented data pipeline and data-quality checks |
| 3 | Indicator & Signal Research | Planned | Tested calculations, reproducible features, no look-ahead leakage |
| 4 | Strategy Definition & Backtesting Engine | Planned | Realistic costs, reproducible runs, out-of-sample evaluation |
| 5 | Risk Engine & Safety Controls | Planned | Independent risk checks and failure-mode tests |
| 6 | Trader Dashboard & Manual Decision Support | Planned | Explainable signals, visible data freshness, human control |
| 7 | Paper Trading & Operational Validation | Planned | No live order path, auditable simulations, readiness review |
| 8 | Controlled Broker Integration & Future Automation | Conditional / gated | Explicit owner approval, safety review, broker reconciliation, rollback plan |

The tracker is a starting state only. The agent must reconcile it with the actual repository and update it truthfully.

---

# Phase 1 — Requirements & Research Design

## Objective
Turn the trader's initial ideas into a documented, testable research specification. Do not start by inventing a strategy or writing live-trading code.

## Known initial context
- Initial instrument: NIFTY 50 options.
- Initial strategy family: hedged option selling.
- Preferred analysis interval: five minutes.
- Preferred expiry: weekly, subject to validation.
- Initial research inputs: trend, change in Open Interest (OI), and Put-Call Ratio (PCR).
- Other candidate inputs: volume, implied volatility (IV), and option Greeks.
- Starting capital assumption: ₹500,000.
- Proposed maximum loss per trade: ₹5,000.
- Proposed daily loss limit: ₹5,000.
- Desired win rate: above 55%, an aspiration rather than a guarantee.
- The trader has no historical dataset at the start.
- The trader currently uses discretionary judgement/gut feeling and wants help formulating testable strategies.
- Full human control initially; no automated live orders.
- The system should avoid producing a trade recommendation when conditions are ambiguous.
- Avoid revenge trading and repeated entries without a clear new signal.

These are initial statements, not all finalized specifications. Label them Proposed until the trader confirms exact definitions.

## Tasks
1. Inspect the repo and record its actual state.
2. Create/update `docs/requirements.md` with:
   - confirmed requirements;
   - proposed assumptions;
   - unknowns and questions for the trader;
   - out-of-scope items;
   - acceptance criteria.
3. Create/update `docs/strategy-hypotheses.md`. Record research candidates without claiming they work:
   - trend plus OI-change behaviour;
   - PCR variants plus price/OI behaviour;
   - hedged option-selling structures under different market regimes;
   - volatility-aware strike/expiry selection;
   - explicit no-trade filters for ambiguity or poor data.
4. Create/update `docs/indicator-specification.md`. For each indicator record formula/definition, input data, timestamp, update interval, limitations, and hypothesis to test. If a definition is undecided, mark it Unknown.
5. Create/update `docs/risk-policy-draft.md` documenting the proposed per-trade and daily loss limits, and list unresolved definitions. Do not enforce ambiguous financial rules in live execution.
6. Create/update `docs/data-feasibility.md` listing required data fields, historical coverage needs, candidate sources to verify, licensing/cost questions, API constraints, and gaps.
7. Create/update `docs/backtest-methodology.md` with proposed metrics and safeguards against look-ahead bias and overfitting.
8. Create/update `docs/phase-gates.md` with entry/exit criteria for all phases.
9. Add unit tests only for any code changed in this phase. Do not build a trading strategy prematurely.
10. Update `README.md`, `CHANGELOG.md`, and the phase tracker.

## Phase 1 acceptance criteria
- Requirements separate Confirmed, Proposed, Unknown, and Blocked.
- Strategy hypotheses are documented as hypotheses only.
- Indicator definitions are explicit or marked Unknown.
- Risk-limit ambiguities are recorded as owner questions.
- Historical data availability is not assumed.
- Backtest metrics include net profit after costs, win rate, average win/loss, profit factor, drawdown, worst day, losing streak, trade count, margin/capital usage, and out-of-sample results.
- README shows latest developments and owner actions near the top.
- No real trading or order submission is added.

## Owner questions to surface in README
- Which trend definition should be researched first?
- Should PCR be based on OI, volume, or both?
- Which hedged structures should be compared first?
- How should strike and expiry be selected?
- How is a trend reversal confirmed?
- Does the daily loss limit include unrealized P&L, costs, and slippage?
- What should happen to open positions when the daily limit is reached?
- What historical options data is available at five-minute resolution, and at what cost?
- Which evaluation criteria, beyond win rate, are mandatory?

---

# Phase 2 — Data Access & Market Data Foundation

## Objective
Build a reliable and auditable market-data layer using permitted data sources. Do not assume Zerodha provides every required historical field or unlimited historical options data.

## Tasks
1. Verify current Kite Connect capabilities, subscription requirements, rate limits, authentication, and permitted usage from official documentation.
2. Inventory available historical NIFTY options data. Record coverage, granularity, fields, cost, license, and missing intervals before choosing a source.
3. Design the PostgreSQL schema for instruments, expiries, strikes, option type, timestamps, OHLC, volume, OI, source metadata, and quality flags.
4. Implement import/collection jobs with retries, rate-limit handling, idempotency, structured logging, and resumability.
5. Validate timestamps, timezone, duplicate rows, missing intervals, invalid prices, symbol/token mapping, expiry and strike metadata.
6. Add a data provenance record so each row or batch can be traced to its source and collection time.
7. Provide safe sample/demo data for tests. Never require live credentials for unit tests.
8. Document database setup, migrations, data import, limitations, and secret configuration.
9. Update README, CHANGELOG, status, tests, and phase tracker.

## Acceptance criteria
- A documented source and licensing position exists for every dataset used.
- A sample dataset can be imported repeatably.
- Re-running ingestion does not silently duplicate records.
- Data quality issues are detectable and reported.
- Missing fields are not fabricated.
- No strategy profitability claims are made from unverified data.

---

# Phase 3 — Indicator & Signal Research

## Objective
Implement transparent and tested indicators, then evaluate whether they provide useful information. Do not treat correlation as proof of predictive power.

## Tasks
1. Implement configurable trend measures selected or approved during Phase 1.
2. Implement OI level and OI-change features with explicit comparison windows and instrument/expiry aggregation.
3. Implement PCR variants only after defining whether the numerator/denominator uses OI, volume, or both.
4. Implement volume and volatility features supported by available data.
5. Calculate or source Greeks with documented model assumptions and input requirements.
6. Prevent look-ahead leakage: signals at time `t` must not use values that were unavailable at time `t`.
7. Add unit tests using known values, edge cases, missing values, expiry transitions, and market-session boundaries.
8. Create explainable signal records with timestamp, data snapshot/version, feature values, thresholds, and reason.
9. Compare indicators with simple baselines; document when an indicator adds no measurable value.
10. Update README, CHANGELOG, research notes, and phase tracker.

## Acceptance criteria
- Calculations are reproducible and unit-tested.
- Definitions and limitations are documented.
- Signals can be traced to input data.
- No arbitrary thresholds are presented as validated.
- Negative research results are retained rather than hidden.

---

# Phase 4 — Strategy Definition & Backtesting Engine

## Objective
Build a reproducible backtesting framework and use it to test explicit, trader-reviewed hypotheses. The engine must not make results look better by using future information or unrealistic fills.

## Tasks
1. Define a common strategy interface for entry, exit, sizing, adjustments, and no-trade decisions.
2. Model only documented candidate hedged structures, including every leg, expiry, strike, quantity, and payoff.
3. Simulate events chronologically and use only data available at each decision point.
4. Model transaction costs applicable to the selected instruments and period; document assumptions and verify current rates from authoritative sources before relying on them.
5. Include spread, slippage, fill uncertainty, order timing, and realistic execution constraints.
6. Report net P&L, win rate, average win/loss, profit factor, drawdown, worst day, longest losing streak, number of trades, capital utilization, and margin assumptions.
7. Separate development, validation, and out-of-sample periods. Record all parameter selection.
8. Add reproducible experiment records: code version, data version, configuration, costs, metrics, and run timestamp.
9. Test edge cases such as missing bars, expiry, gaps, partial data, and strategy state transitions.
10. Update README, CHANGELOG, and phase tracker.

## Acceptance criteria
- Known test cases pass.
- No look-ahead bias is identified in review.
- Costs and fill assumptions are explicit.
- Every result is reproducible.
- Out-of-sample results are clearly separated.
- A high win rate alone cannot qualify a strategy as successful.

---

# Phase 5 — Risk Engine & Safety Controls

## Objective
Implement an independent safety layer that can reject a candidate trade regardless of the strategy's signal.

## Tasks
1. Convert trader-approved risk definitions into explicit, tested rules.
2. Implement per-trade, daily, capital allocation, exposure, and position-count checks only after their definitions are approved.
3. Define whether limits use realized P&L, unrealized P&L, charges, and slippage.
4. Specify behaviour when a daily limit is reached, including what happens to existing positions. Do not guess this policy.
5. Reject new signals on stale, missing, inconsistent, or invalid market data.
6. Design safe order-state handling for future use: rejected, partial, delayed, unknown, and duplicate states.
7. Implement a kill switch for research/paper components and define future live-mode semantics separately.
8. Test API outage, database outage, restart, duplicate events, corrupted data, risk-limit breach, and recovery.
9. Ensure the strategy cannot bypass risk checks.
10. Update README, CHANGELOG, test reports, and phase tracker.

## Acceptance criteria
- Risk checks are independent from strategy logic.
- Unsafe candidate trades are blocked.
- Loss-limit semantics are explicit and tested.
- Failure modes and recovery behaviour are documented.
- No live order submission is enabled in this phase.

---

# Phase 6 — Trader Dashboard & Manual Decision Support

## Objective
Provide clear, auditable market context and candidate trade information while keeping the trader in control.

## Tasks
1. Inspect the existing repository and choose UI technology consistent with the current architecture; do not replace it without justification.
2. Prioritize dashboard needs with the trader.
3. Show NIFTY context, selected expiry, relevant option data, OI, PCR, volume, volatility, and Greeks where data is available.
4. Show each candidate structure, legs, rationale, assumptions, risk estimates, and invalidation/no-trade reasons.
5. Display timestamp and data freshness clearly.
6. Allow manual logging of accepted, rejected, skipped, and manually exited ideas, including notes.
7. Add alerts for candidate opportunities, risk blocks, data/API issues, and system health.
8. Add an audit trail linking signals to data, strategy version, risk checks, and trader decisions.
9. Keep all broker order submission disabled.
10. Update README, CHANGELOG, screenshots only if useful, tests, and phase tracker.

## Acceptance criteria
- Signals explain why they appeared.
- Human approval remains required.
- Data freshness and system health are visible.
- Decisions and signal history are auditable.
- The dashboard never implies a signal guarantees a profitable trade.

---

# Phase 7 — Paper Trading & Operational Validation

## Objective
Run the system on live or replayed market data without submitting real broker orders. Compare actual observations with backtest assumptions.

## Tasks
1. Implement a paper execution simulator isolated from any live order adapter.
2. Ensure paper mode has no callable path to submit real orders.
3. Record every signal, data snapshot, decision, simulated order, fill assumption, and resulting position.
4. Measure spread, slippage, latency, missed fills, and simulated rejections where measurable.
5. Compare paper behaviour with backtest results and investigate discrepancies.
6. Test restarts, data interruptions, database failures, clock/timezone issues, and recovery without duplicate positions.
7. Observe the system across varied market conditions for a period agreed with the trader; do not invent a short duration as proof of robustness.
8. Conduct a readiness review covering data quality, strategy evidence, risk controls, operational reliability, and trader approval.
9. Update README with paper-trading status, known limitations, evidence, and owner actions.
10. Keep all real order submission disabled.

## Acceptance criteria
- No real orders are sent.
- Simulated trades and positions are auditable.
- Failure and recovery scenarios are tested.
- Backtest/paper discrepancies are documented.
- Readiness is assessed against predefined criteria; profitability is not guaranteed.

---

# Phase 8 — Controlled Broker Integration & Future Automation (GATED)

## Objective
Only after separate, explicit authorization and a documented readiness review, design controlled broker order workflows. Human approval remains mandatory for the first live-capable version.

## Preconditions
- Phases 1–7 have passed their applicable acceptance criteria.
- Strategy validation and limitations are documented.
- Risk policy is approved.
- Paper-trading and operational reviews are complete.
- Broker API capabilities and current terms have been checked against official documentation.
- The owner explicitly authorizes this phase.

If any precondition is absent, do not enable live orders. Continue safe documentation, simulation, or tests and report the blocker.

## Tasks
1. Review live-trading readiness and record explicit approval.
2. Design the order lifecycle, confirmation, reconciliation, idempotency, and recovery workflow.
3. Keep live order submission behind an explicit configuration/permission gate that defaults to disabled.
4. Require human approval for each proposed live order in the initial live-capable version.
5. Reconcile internal positions/orders with broker-reported positions/orders and alert on discrepancies.
6. Handle unknown order state safely; never retry blindly if an earlier order may have succeeded.
7. Verify kill switch, exposure limits, daily loss behaviour, and rollback/stop procedures.
8. If separately approved, conduct a tightly controlled pilot with conservative limits and a documented stop plan.
9. Evaluate any later removal of human approval as a separate project decision requiring new explicit authorization.
10. Update README with the exact enabled/disabled state, approvals, tests, and unresolved risks.

## Acceptance criteria
- Phase remains blocked without explicit authorization.
- Human approval is enforced initially.
- Risk checks cannot be bypassed.
- Credentials are secured and excluded from Git.
- Order reconciliation and failure recovery are tested.
- No claims of safety or profitability exceed the evidence.

---

## Final completion report template

At the end of every phase, respond with:

1. **Phase status:** Complete / Partial / Blocked.
2. **What changed:** Key files and features.
3. **Tests:** Exact commands run and actual results.
4. **Acceptance criteria:** Passed, failed, and not yet checked.
5. **Latest developments:** What should appear at the top of README.
6. **Owner action required:** Questions, decisions, setup, or approvals needed.
7. **Blockers and risks:** Impact and next step.
8. **Next phase:** Suggested next phase, without starting it automatically.

A phase is not complete merely because files were created. Completion requires its acceptance criteria to be met and the documentation to match the actual repository state.
