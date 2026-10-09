# Decision Log — NIFTY QuantLab

Format: date, ID, decision, context, rationale, status.

## D1 — 2026-10-09 — Phase 1 is documentation-only; no application code written

- **Context:** Phase 1 tasks are requirements, hypotheses, indicator specs, a risk-policy draft, a data-feasibility plan, a backtest methodology, and phase gates. Prompts.md Phase 1 task 9 says to add unit tests only for code changed, and warns not to build a trading strategy prematurely.
- **Decision:** write documentation only; no package scaffolding, no strategy or indicator code.
- **Rationale:** keeps Phase 1 reviewable by the trader before any implementation; avoids inventing trading rules.
- **Status:** Applied.

## D2 — 2026-10-09 — No unit tests added in Phase 1

- **Context:** Prompts.md Phase 1 task 9: "Add unit tests only for any code changed in this phase."
- **Decision:** no code changed → no tests. Test-infrastructure decisions are deferred until code exists (Phases 2–3).
- **Status:** Applied.

## D3 — 2026-10-09 — All trading specifics labeled Proposed until the trader confirms

- **Context:** Prompts.md: "These are initial statements, not all finalized specifications. Label them Proposed until the trader confirms exact definitions."
- **Decision:** requirements are split into Confirmed (owner operating rules) / Proposed (trading specifics) / Unknown / Blocked in `docs/requirements.md`.
- **Status:** Applied.

## D4 — 2026-10-09 — README restructured so required status sections sit near the top in the required order

- **Context:** Prompts.md §E requires these sections near the top, in this order: Current status, Latest developments, Owner action required, Blockers and risks, How to run and test, Phase tracker.
- **Decision:** reordered the README top sections accordingly; moved project overview, architecture direction, safety principles, and Arena workflow sections below.
- **Status:** Applied.

## D5 — 2026-10-09 — No technology stack committed in Phase 1

- **Context:** the repository has no code. The README lists an architecture direction (PostgreSQL, module boundaries) as a direction, not a decision.
- **Decision:** defer stack/dependency choices to Phase 2 (the data-source choice may constrain the stack). No package manifests added.
- **Status:** Open — revisit at Phase 2.

## D6 — 2026-10-09 — Data sources recorded as "candidates to verify", not selected

- **Context:** Phase 1 must not assume historical data availability; Phase 2 verifies official documentation.
- **Decision:** `docs/data-feasibility.md` lists candidates (Kite Connect Historical API, NSE archives, commercial vendors, other broker APIs, public datasets), all marked Unknown/to-verify.
- **Status:** Applied.
