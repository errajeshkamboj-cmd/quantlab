# Requirements — NIFTY QuantLab (Phase 1)

- **Status:** Draft for trader review (Phase 1 deliverable)
- **Last updated:** 2026-10-09
- **Owner:** Trader (domain expert) — review and confirmation required
- **Source of truth for phase scope:** `Prompts.md` → "Phase 1 — Requirements & Research Design"

## Status legend

Every requirement/assumption below is labeled:

- **Confirmed** — stated by the owner as a fixed operating requirement (mostly process and safety rules).
- **Proposed** — initial trader idea recorded as a starting point; not yet confirmed as an exact specification.
- **Unknown** — definition or fact not yet decided/verified; must not be guessed.
- **Blocked** — cannot progress until a dependency (data, owner decision, or a later phase) is resolved.

## Repository state at Phase 1 start (inspected 2026-10-09)

Inspected before any edits, per `Prompts.md` rule A:

- Branch: `arena/c24c2f8e-quantlab` (session branch), fast-forwarded to `origin/main` at `4c1cb49`.
- Files present: `README.md`, `Prompts.md`, `CHANGELOG.md` only.
- Git history: two commits — `3b074ef` "Initial commit" (README.md) and `4c1cb49` "Add files via upload" (Prompts.md, CHANGELOG.md, expanded README.md).
- **No application code, no tests, no CI configuration, no dependency manifests, no database, and no configuration files exist yet.**
- Consequence: Phase 1 is documentation-only; there is no existing implementation to preserve or modify.

## Confirmed requirements (owner-stated, fixed)

These come from the owner's operating rules in `Prompts.md` and the starter `README.md`. They are not open to reinterpretation by the agent.

| # | Requirement | Source |
|---|---|---|
| C1 | Product scope is research, backtesting, paper trading, and human-controlled decision support only. | Prompts.md §C |
| C2 | No real orders and no live order submission in Phases 1–7. Phase 8 is gated and stays disabled until explicit owner approval plus readiness checks. | Prompts.md §C |
| C3 | Strategy signals are proposals, not orders; an independent risk layer must be able to reject any signal. | Prompts.md §C |
| C4 | Missing, stale, contradictory, or invalid data must produce a safe no-signal state, never an invented value. | Prompts.md §C |
| C5 | Full human control initially; no automated live orders. | Prompts.md Phase 1 context |
| C6 | The system should avoid producing a trade recommendation when conditions are ambiguous. | Prompts.md Phase 1 context |
| C7 | Avoid revenge trading and repeated entries without a clear new signal. | Prompts.md Phase 1 context |
| C8 | Win rate above 55% is an aspiration, not a guarantee and not the sole success metric. | Prompts.md §B |
| C9 | Profitability must be evaluated alongside costs, average win/loss, drawdown, profit factor, tail risk, trade count, and execution realism. | Prompts.md §B |
| C10 | No strategy may be claimed to work without reproducible results and documented validation. | Prompts.md §B |
| C11 | Never commit secrets, credentials, `.env` files, or private broker data; use `.env.example` with placeholders. | Prompts.md §C |
| C12 | No live broker credentials in tests, examples, logs, screenshots, or documentation. | Prompts.md §C |
| C13 | Timezone-aware timestamps; the market timezone must be documented (NSE: IST, UTC+5:30). | Prompts.md §D |
| C14 | No look-ahead bias: a decision at time t may only use data available at time t. | Prompts.md §D |
| C15 | Transaction-cost, spread, slippage, and fill assumptions must be documented in every backtest. | Prompts.md §D |

## Proposed assumptions (trader ideas — NOT yet confirmed specifications)

From `Prompts.md` Phase 1 "Known initial context". Label: **Proposed** until the trader confirms exact definitions.

| # | Assumption | Value | Notes |
|---|---|---|---|
| P1 | Initial instrument | NIFTY 50 options | Weekly expiry preferred, subject to validation |
| P2 | Initial strategy family | Hedged option selling | Specific structures not yet chosen (owner question U3) |
| P3 | Analysis interval | 5 minutes | |
| P4 | Preferred expiry | Weekly | Subject to validation against data availability |
| P5 | Initial research inputs | Trend, change in Open Interest (OI), Put-Call Ratio (PCR) | Exact definitions Unknown (U1, U2) |
| P6 | Candidate additional inputs | Volume, implied volatility (IV), option Greeks | To be evaluated in Phase 3 |
| P7 | Starting capital assumption | ₹500,000 | Assumption for sizing/margin modeling only |
| P8 | Proposed maximum loss per trade | ₹5,000 | Scope (per leg / per position) is Unknown — U10 |
| P9 | Proposed daily loss limit | ₹5,000 | Semantics Unknown — U6, U7 |
| P10 | Desired win rate | Above 55% | Aspiration, not a guarantee (C8) |

## Unknowns — questions for the trader (owner action required)

Do not guess these. They are tracked in `README.md` → "Owner action required" and, where relevant, in `docs/risk-policy-draft.md`.

| # | Question | Why it matters | Status |
|---|---|---|---|
| U1 | Which trend definition should be researched first? | Indicator spec and Phase 3 implementation depend on it | Open |
| U2 | Should PCR be based on OI, volume, or both? | PCR has multiple variants; affects indicator and strategy design | Open |
| U3 | Which hedged structures should be compared first? | Determines Phase 4 strategy candidates | Open |
| U4 | How should strike and expiry be selected? | Strike/expiry selection drives all backtests | Open |
| U5 | How is a trend reversal confirmed? | Exit/adjustment rules depend on it | Open |
| U6 | Does the daily loss limit include unrealized P&L, costs, and slippage? | Risk semantics must be exact before any enforcement | Open |
| U7 | What should happen to open positions when the daily limit is reached? | Position-handling policy at limit breach | Open |
| U8 | What historical options data is available at five-minute resolution, and at what cost? | Gates all research; see `docs/data-feasibility.md` | Open |
| U9 | Which evaluation criteria, beyond win rate, are mandatory? | Defines strategy acceptance | Open |
| U10 | Does the ₹5,000 per-trade loss limit apply per leg, per strategy position, or to the complete strategy? | Risk semantics must be exact before any enforcement | Open |

## Blocked items

| # | Item | Blocked by | Unblock path |
|---|---|---|---|
| B1 | Any indicator implementation | U1, U2 (definitions) + Phase 2 data | Trader answers; then Phase 3 |
| B2 | Any backtest or profitability claim | B1 + verified historical data (U8) | Phases 2–4 |
| B3 | Risk-rule enforcement | U6, U7, U10 (exact semantics) | Trader answers; then Phase 5 |
| B4 | Any live order capability | Phase 8 gate: Phases 1–7 accepted + explicit owner authorization | Out of scope now; must remain disabled |

## Out of scope (Phase 1, and Phases 1–7 generally)

- Live order submission or any broker order workflow (Phase 8, gated).
- Automated/algorithmic order placement.
- Writing strategy, indicator, or backtesting code (Phases 3–4).
- Choosing or endorsing a "winning" strategy; all hypotheses remain unvalidated.
- Any claim of profitability or win rate.
- Paid data purchases, service purchases, or account changes (owner decision required first).

## Phase 1 acceptance criteria — self-check

| Criterion (from `Prompts.md`) | Status |
|---|---|
| Requirements separate Confirmed, Proposed, Unknown, Blocked | Met — this document |
| Strategy hypotheses documented as hypotheses only | Met — `docs/strategy-hypotheses.md` |
| Indicator definitions explicit or marked Unknown | Met — `docs/indicator-specification.md` |
| Risk-limit ambiguities recorded as owner questions | Met — `docs/risk-policy-draft.md` + U6/U7/U10 above |
| Historical data availability is not assumed | Met — `docs/data-feasibility.md` records gaps; nothing assumed |
| Backtest metrics include net profit after costs, win rate, average win/loss, profit factor, drawdown, worst day, losing streak, trade count, margin/capital usage, out-of-sample results | Met — `docs/backtest-methodology.md` |
| README shows latest developments and owner actions near the top | Met — `README.md` restructured 2026-10-09 |
| No real trading or order submission added | Met — no code written at all |

**Gate note:** the Phase 1 exit gate is "trader-reviewed requirements, explicit open questions, data feasibility plan". The documentation half is done; the **trader review half is pending** — see `README.md` → "Owner action required".
