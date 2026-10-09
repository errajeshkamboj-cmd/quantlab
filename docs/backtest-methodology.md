# Backtest Methodology — NIFTY QuantLab (Phase 1)

- **Status:** Proposed methodology. No backtesting engine exists yet; it is built in Phase 4 against this document.
- **Last updated:** 2026-10-09

## Mandatory metrics (Phase 1 acceptance criterion)

Every backtest report must include:

1. Net profit after costs
2. Win rate (always reported with trade count — a win rate on few trades is not meaningful)
3. Average win and average loss (and win/loss ratio)
4. Profit factor (gross profit / gross loss)
5. Maximum drawdown
6. Worst day
7. Longest losing streak
8. Trade count
9. Margin / capital usage (against the ₹500,000 assumption, P7)
10. Out-of-sample results, clearly separated from development/validation results

Recommended additions: per-regime breakdown, cost breakdown, return distribution/tail risk.

Win rate above 55% is an aspiration, not a guarantee and not a sufficient success metric (C8, C9).

## Cost and execution assumptions — documented in every run

- Brokerage and taxes: verify current rates from authoritative sources before relying on them (Phase 4 task 4).
- Spread, slippage, fill uncertainty, order timing: explicit assumptions (e.g., signal at bar close → assumed fill at next bar open, or a documented alternative).
- Partial fills and rejected orders modeled where relevant.

## Look-ahead safeguards (Confirmed requirement C14)

- A decision at time t uses only data available at t.
- Indicators use closed bars only (`docs/indicator-specification.md` conventions).
- No using expiry outcomes, future OI, or revised data in past decisions.
- Corporate actions and stale-data handling must not leak future information.
- Events simulated chronologically, one at a time.

## Overfitting safeguards

- Split history into development / validation / out-of-sample periods; record the split.
- Record all parameter selection and every experiment (code version, data version, configuration, costs, metrics, run timestamp).
- Compare against simple baselines (`docs/strategy-hypotheses.md`).
- Prefer fewer parameters; treat in-sample improvement that does not survive out-of-sample as overfitting.
- Retain and report negative results.

## Options-specific realism

- Option sellers face assignment risk at expiry; expiry-day square-off/assignment rules must be explicit before any backtest is trusted.
- Margin (SPAN) modeled per period; capital usage reported against the capital assumption.
- Lot-size and strike-universe changes over time handled explicitly.
- Illiquidity: wide spreads / low volume on far strikes; fill assumptions must reflect it.

## Reproducibility

- Same code version + data version + configuration → same results (deterministic).
- Experiment records allow any reported number to be regenerated.

## Non-goals

- No profitability claims from unverified data (Phase 2 acceptance criterion).
- A high win rate alone cannot qualify a strategy as successful (Phase 4 acceptance criterion).
