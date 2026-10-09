# Strategy Hypotheses — NIFTY QuantLab (Phase 1)

- **Status:** Research candidates only. Nothing in this document is validated. No hypothesis below is a trading recommendation, and none may be presented as working until Phases 2–4 produce reproducible, out-of-sample evidence.
- **Last updated:** 2026-10-09
- **Standing rules:** correlation is not proof of predictive power (Prompts.md Phase 3); negative results are retained, not hidden (Phase 3 acceptance criteria); win rate above 55% is an aspiration, not a guarantee (C8).

## How hypotheses are recorded

Each entry: ID, neutral statement, inputs, what would count as support, what would count as falsification, status. All entries start as **Proposed — untested**.

## H1 — Regime behaviour of trend + OI change (Proposed — untested)

- **Statement:** Outcomes of hedged option selling may differ between trending and ranging regimes, and OI-change behaviour (I2) around the underlying may carry information about trend continuation or exhaustion.
- **Inputs:** trend measure (I1 — definition Unknown, U1), OI change (I2), price, PCR (I3).
- **Support:** a reproducible, out-of-sample difference in hedged-selling performance or signal quality across regimes, after all costs.
- **Falsification:** no measurable difference versus a simple baseline, or differences that vanish out-of-sample.
- **Blocked by:** U1 (trend definition), U8 (data).

## H2 — PCR variants vs price/OI behaviour (Proposed — untested)

- **Statement:** PCR (I3 — variant Unknown, U2) combined with price and OI changes may help distinguish crowded positioning from genuine participation.
- **Inputs:** PCR variant(s) (I3), price, OI change (I2), volume (I4).
- **Support:** PCR-based features add measurable, reproducible information over price/OI alone, out-of-sample, after costs.
- **Falsification:** no improvement over the simpler baseline.
- **Blocked by:** U2 (PCR definition), U8 (data).

## H3 — Hedged option-selling structures across regimes (Proposed — untested)

- **Statement:** Different hedged selling structures (e.g., short straddle/strangle with hedge wings, call/put credit spreads — exact structures Unknown, U3) will perform differently across market regimes, and a regime filter may improve risk-adjusted outcomes.
- **Inputs:** regime classification (from I1), structure payoff definitions (Phase 4), margin model.
- **Support:** one or more documented structures show acceptable, reproducible risk-adjusted results out-of-sample, with explicit costs.
- **Falsification:** no structure beats simple baselines after costs and risk.
- **Blocked by:** U3 (structures), U4 (strike/expiry selection), U8 (data).

## H4 — Volatility-aware strike/expiry selection (Proposed — untested)

- **Statement:** Selecting strikes/expiry using volatility information (I5, I6) may improve hedged-selling outcomes versus fixed-strike selection.
- **Inputs:** IV (I5), Greeks (I6), India VIX (context), strike/expiry universe (U4).
- **Support:** volatility-aware selection improves reproducible, out-of-sample, cost-adjusted results versus fixed rules.
- **Falsification:** no improvement, or improvement only in-sample (overfitting).
- **Blocked by:** U4, U8, and IV model choice (I5).

## H5 — Explicit no-trade filters improve trade quality (Proposed — untested)

- **Statement:** Applying explicit no-trade filters for ambiguous conditions or poor data quality (Confirmed requirements C4/C6) should reduce low-quality trades; the hypothesis to test is whether the filtered trade set outperforms the unfiltered set after costs.
- **Inputs:** data-quality flags (I7), ambiguity definitions (to be specified — currently Unknown).
- **Support:** the filtered set shows better reproducible cost-adjusted outcomes with fewer trades.
- **Falsification:** filters remove trades without improving outcomes (i.e., filters only reduce opportunity).
- **Blocked by:** ambiguity definitions (Unknown), U8.

## Required baselines (Prompts.md Phase 3 task 9)

Every hypothesis must be compared against simple baselines, including at least:

- Naive hedged option selling with fixed rules (no indicator timing).
- Buy-and-hold of the underlying (capital-adjusted).
- Random/periodic entry timing within the same regime filter.

An indicator or filter that adds no measurable value over these baselines must be documented as such — that is a valid, retainable result.

## What this document deliberately does not contain

- No entry/exit rules, thresholds, or parameter values (those come from trader-confirmed definitions).
- No performance numbers (no data has been verified; nothing has been backtested).
- No claim that any structure "works".
