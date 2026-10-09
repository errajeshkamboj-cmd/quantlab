# Risk Policy Draft — NIFTY QuantLab (Phase 1)

- **Status:** Draft for trader review. Proposed values only; unresolved definitions are recorded as owner questions and must not be guessed or enforced.
- **Last updated:** 2026-10-09
- **Standing rule:** do not enforce ambiguous financial rules in live execution (Prompts.md Phase 1 task 5). No live execution exists in Phases 1–7 in any case; this draft is an input to the Phase 5 risk engine.

## Proposed limits (Proposed — not confirmed)

| Parameter | Proposed value | Notes |
|---|---|---|
| Starting capital assumption | ₹500,000 | Sizing/margin modeling assumption only (P7) |
| Maximum loss per trade | ₹5,000 | Scope Unknown — per leg, per strategy position, or complete strategy? (U10) |
| Daily loss limit | ₹5,000 | Semantics Unknown (U6, U7) |

## Confirmed design principles for the future risk engine

- The risk layer is independent of strategy logic and can reject any candidate trade (C3).
- Missing, stale, contradictory, or invalid data → safe no-signal state (C4).
- Signals are proposals, not orders; human approval governs action (C3, C5).
- Risk controls never eliminate trading risk; documentation must not claim otherwise (Prompts.md §C).

## Unresolved definitions — owner questions (must be answered before Phase 5)

| # | Question | Why it blocks |
|---|---|---|
| U6 | Does the daily loss limit include unrealized P&L, charges (brokerage/taxes), and slippage? | The limit's numeric meaning depends on it |
| U7 | What happens to open positions when the daily limit is reached (alert only / stop new entries / flatten)? | Position-handling policy at breach |
| U10 | Does the ₹5,000 per-trade limit apply per leg, per strategy position, or to the complete strategy? | Per-trade accounting depends on it |
| R4 | Is the limit measured on realized (closed-trade) P&L only, or marked-to-market? | Monitoring basis |
| R5 | What exactly does a breach trigger in research/paper mode (Proposed default: stop generating new signals for the rest of the day)? | Paper-mode semantics for Phase 7 |
| R6 | Does the daily limit reset at the next NSE session open (IST)? | Reset semantics |
| R7 | Are there additional limits the trader wants (max open positions, max margin usage, max trades per day)? | Currently none proposed |

## Explicit non-goals for this draft

- No enforcement code (that is Phase 5, after definitions are approved).
- No claim that these limits are sufficient or correct — they are the trader's starting proposals.
- No live-trading semantics; live behaviour is a Phase 8 gated decision.
