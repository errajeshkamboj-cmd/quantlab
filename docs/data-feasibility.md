# Data Feasibility — NIFTY QuantLab (Phase 1)

- **Status:** Feasibility plan only. **Historical data availability is not assumed** (Phase 1 acceptance criterion). Every source below is a candidate to verify in Phase 2 against official documentation.
- **Last updated:** 2026-10-09

## Confirmed starting point

- The trader has **no historical dataset** at project start (Confirmed).
- Nothing in this document should be read as "data exists". Phase 2 tasks 1–2 (verify official API documentation; inventory available data) are the verification path.

## Required data fields

| Field | Needed for | Notes |
|---|---|---|
| Instrument identity: token/symbol, strike, expiry, CE/PE | All option series | Symbol/token mapping must be validated (Phase 2 task 5) |
| Timestamp (IST, timezone-aware) | All series | Market timezone documented: IST (UTC+5:30) |
| OHLC per 5-minute bar | Indicators, backtests | Bar labeled by close time |
| Volume per bar | I4, PCR_Vol | |
| Open Interest per bar | I2, PCR_OI | Per-strike availability unverified (U8) |
| Underlying NIFTY 50 index OHLC | Trend (I1), IV (I5) | Same interval |
| India VIX | Volatility regime context (H4) | Index-level; distinct from per-strike IV (I5) |
| Bid/ask or spread proxy | Realistic fills, spread cost | May not be available historically — gap |
| Corporate actions / event calendar | Adjustments, special days | Gap unless sourced |
| Lot size, tick size, margin (SPAN) parameters per period | Sizing, margin usage, assignment risk | Historical changes must be captured |
| Expiry calendar (weekly/monthly) | Expiry handling | NSE circulars |

## Historical coverage needs (Proposed — sufficiency must be evaluated, not assumed)

- **Interval:** 5 minutes (Proposed, P3).
- **Initial target coverage:** 6 months (initial suggestion only — sufficiency must be evaluated rather than assumed). Longer history (1–3+ years) is desirable for regime coverage; availability/cost Unknown.
- **Universe:** weekly expiries (Proposed, P4); strikes around ATM — how many (±N) is Unknown (U4). Options data is per strike × expiry, so the universe is large; storage and ingestion planning must account for this.
- **Development/validation/out-of-sample split:** the methodology requires three periods (`docs/backtest-methodology.md`); total coverage must support all three.

## Candidate sources to verify (all rows: status Unknown — to verify in Phase 2)

| Candidate source | What it may provide | Key questions to verify | Where to verify |
|---|---|---|---|
| Zerodha Kite Connect — Historical API | Archived candles (timestamp, OHLC, volume, OI) for instruments across exchanges, intervals including 5-minute, spanning back several years; OI via request flag; historical data is a paid subscription | Per-request date-range limits, rate limits, cost, license/permitted use, per-strike options OI coverage and depth, auth requirements | Official docs: kite.trade/docs/connect/v3/historical/ and Kite Connect terms |
| NSE India — bhavcopy / archives / data products | Official exchange data | Intraday (non-EOD) availability, options OI availability, cost, license, distribution terms | nseindia.com official pages |
| Commercial Indian market-data vendors (e.g., TrueData — an authorized NSE/BSE/MCX vendor with paid plans and per-user exchange fees; others such as TickerTape, GlobalDataFeeds, MarketFeed exist as candidates) | Intraday historical options data, OI, API access | Coverage, granularity, fields, cost, license, API constraints, data quality | Vendor official documentation and contracts |
| Other broker APIs (e.g., Fyers, Upstox, Angel One SmartAPI) | Historical candles | Same questions as above; options OI depth | Each broker's official API docs |
| Public/free datasets (e.g., Kaggle-hosted NIFTY 50 index minute data) | Underlying index data only — no options OI | License, coverage, quality, update cadence | Dataset documentation |

## API constraints to verify (Phase 2 task 1)

- Rate limits, authentication, subscription requirements, permitted usage — **all Unknown until verified from official documentation**. Do not assume Zerodha provides every required historical field or unlimited historical options data (Prompts.md Phase 2 objective).

## Gaps (recorded, not papered over)

1. No dataset at start (Confirmed).
2. 5-minute per-strike options OI historical availability, depth, and cost — Unknown (U8).
3. Historical bid/ask (spread) data — likely unavailable from most sources; backtests may need documented spread assumptions instead.
4. Historical margin/SPAN parameters — Unknown source.
5. Licensing/cost position for every dataset — must exist before any dataset is used (Phase 2 acceptance criterion).

## Next step

Phase 2 tasks 1–2: verify official documentation and inventory actual coverage/granularity/fields/cost/license before choosing a source. **No source is selected yet.**
