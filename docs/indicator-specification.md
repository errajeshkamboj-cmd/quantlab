# Indicator Specification — NIFTY QuantLab (Phase 1)

- **Status:** Draft — definitions are explicit where standard, and marked **Unknown** where the trader has not decided.
- **Last updated:** 2026-10-09
- **Rule:** if a definition is undecided, it is marked Unknown (Prompts.md Phase 1 task 4). No threshold in this document is validated.

## Common conventions (Proposed)

- **Market timezone:** IST (UTC+5:30); NSE session 09:15–15:30 — verify against official exchange circulars before implementation.
- **Bar semantics:** a 5-minute bar is labeled with its **close** timestamp. A decision at time t may only use bars whose close timestamp is ≤ t (no partial bars, no look-ahead).
- **Update interval:** every 5 minutes (Proposed, P3 in `docs/requirements.md`).
- **Data failure:** if an input is missing/stale/invalid at decision time, the indicator must return a safe "no value" state, never an invented value (Confirmed requirement C4).

## I1 — Trend measure — Status: Unknown (owner question U1)

- **Formula/definition:** Not yet chosen. Candidate approaches to evaluate in Phase 3 (candidates only, none selected): moving-average slope or crossover; ADX; swing structure (higher-high/higher-low vs lower-high/lower-low).
- **Input data:** NIFTY 50 underlying OHLC (5-minute).
- **Timestamp:** value computed from bars closed ≤ t.
- **Update interval:** 5 minutes (Proposed).
- **Limitations:** any single trend definition is regime-dependent; a definition chosen on one period may not generalize.
- **Hypothesis to test:** H1 (`docs/strategy-hypotheses.md`).

## I2 — Open Interest (OI) level and OI change — Status: Proposed formula, Unknown parameters

- **Formula/definition:** OI change over a comparison window: ΔOI(t) = OI(t) − OI(t−k), where k is the comparison window in bars. **k is Unknown.**
- **Input data:** per-instrument OI series (strike, expiry, CE/PE) at 5-minute resolution.
- **Timestamp:** bar close ≤ t.
- **Update interval:** 5 minutes (Proposed).
- **Limitations:** OI is published per instrument; aggregating across strikes/expiries (e.g., "total call OI change") is a modeling choice — **aggregation rule Unknown**. OI resets/disappears at expiry and on rollover; series must be handled across expiry transitions. Availability of historical per-strike OI is unverified (U8).
- **Hypothesis to test:** H1.

## I3 — Put-Call Ratio (PCR) — Status: formula explicit, variant and scope Unknown (owner question U2)

- **Formula/definition (variants):**
  - PCR_OI = Σ(put OI) / Σ(call OI)
  - PCR_Vol = Σ(put volume) / Σ(call volume)
  - Combined/both variants possible.
  - **Which variant(s) to use is Unknown (U2).**
- **Input data:** per-instrument OI and/or volume, 5-minute.
- **Timestamp:** bar close ≤ t.
- **Update interval:** 5 minutes (Proposed).
- **Limitations:** **scope is Unknown** — whole option chain, ATM ± N strikes, or single strike; the choice materially changes the value. Extreme values when a denominator is near zero must be handled explicitly (no invented values).
- **Hypothesis to test:** H2.

## I4 — Volume — Status: Proposed

- **Formula/definition:** V(t) = sum of traded volume over the 5-minute interval ending at t (per instrument; aggregation rule for strategy use is Unknown).
- **Input data:** per-instrument volume, 5-minute.
- **Timestamp:** bar close ≤ t.
- **Update interval:** 5 minutes (Proposed).
- **Limitations:** volume spikes around expiry/rollover; contract volume vs notional; illiquid strikes may have zero/erratic volume.
- **Hypothesis to test:** context feature for H1/H2.

## I5 — Implied Volatility (IV) — Status: Unknown (model not chosen)

- **Formula/definition:** per-strike IV is the volatility implied by the option's market price under a documented option-pricing model (for index options, a Black-76-style model is the usual candidate). **Model choice is Unknown.** Separately, the index-level India VIX may be tracked as its own series — note these are different quantities and must not be mixed.
- **Input data:** option price (bid/ask/mid — choice Unknown), underlying price, strike, time to expiry, risk-free rate.
- **Timestamp:** computed from data available at t.
- **Update interval:** 5 minutes (Proposed).
- **Limitations:** IV backing-out fails on illiquid/wide-spread options; must return "no value" rather than a fabricated IV. Model assumptions must be documented (Phase 3 task 5).
- **Hypothesis to test:** H4 (volatility-aware strike/expiry selection).

## I6 — Option Greeks — Status: Unknown (depends on the I5 model)

- **Formula/definition:** delta, gamma, theta, vega computed from the chosen pricing model (I5).
- **Input data:** same as I5 plus model parameters.
- **Timestamp:** computed from data available at t.
- **Update interval:** 5 minutes (Proposed).
- **Limitations:** Greeks inherit all IV limitations; model assumptions and input requirements must be documented (Phase 3 task 5).
- **Hypothesis to test:** H4; risk estimates in later phases.

## I7 — Data freshness / staleness flag — Status: Proposed (safety gate, not a trading indicator)

- **Formula/definition:** flag = "fresh" if the latest input bar's close timestamp is within a maximum age τ of decision time t; otherwise "stale". **τ is Unknown** (proposed starting point: one bar interval for 5-minute decisions; to be confirmed).
- **Purpose:** enforces Confirmed requirement C4 — stale/missing data → safe no-signal state.
- **Hypothesis to test:** n/a — safety mechanism, exercised in Phase 5 failure-mode tests.
