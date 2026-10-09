# Changelog

Meaningful project changes, newest first. Entries must reflect actual work and tests.

## 2026-10-09 — Phase 1: Requirements & Research Design (documentation delivered)

- Created `docs/requirements.md` — requirements classified Confirmed / Proposed / Unknown / Blocked, repository state at Phase 1 start, out-of-scope items, acceptance-criteria self-check.
- Created `docs/strategy-hypotheses.md` — research hypotheses H1–H5 recorded as hypotheses only; required baselines; no performance claims.
- Created `docs/indicator-specification.md` — indicators I1–I7 with formula/definition, inputs, timestamp semantics, update interval, limitations, and hypothesis links; undecided definitions marked Unknown.
- Created `docs/risk-policy-draft.md` — proposed per-trade/daily loss limits and unresolved semantics recorded as owner questions (no enforcement).
- Created `docs/data-feasibility.md` — required data fields, coverage needs, candidate sources to verify (all Unknown), API constraints, and gaps. Historical data availability is not assumed.
- Created `docs/backtest-methodology.md` — mandatory metrics (net profit after costs, win rate, average win/loss, profit factor, drawdown, worst day, losing streak, trade count, margin/capital usage, out-of-sample) and look-ahead/overfitting safeguards.
- Created `docs/phase-gates.md` — entry/exit criteria for all 8 phases, including Phase 8 preconditions.
- Created `docs/DECISIONS.md` — Phase 1 decisions (documentation-only scope, no tests, Proposed labeling, README restructure, deferred stack, sources as candidates).
- Restructured `README.md` — required status sections near the top in the required order (Current status, Latest developments, Owner action required, Blockers and risks, How to run and test, Phase tracker); updated all sections; phase tracker shows Phase 1 docs complete with the exit gate pending trader review.
- **No application code changed; no tests added or run** (documentation-only phase, per `Prompts.md` Phase 1 task 9).
- **Remaining:** trader review of Phase 1 docs and the 10 open owner questions; Phase 2 data-source verification.

## 2026-10-09 — Repository starter documentation

- Added `Prompts.md` with permanent agent rules and full prompts for all eight project phases.
- Added `README.md` with project status, owner actions, phase tracker, safety boundaries, and Arena workflow.
- No application implementation or tests are claimed by this starter documentation.
