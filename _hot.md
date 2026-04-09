# _hot.md — Session Cache
_Updated: 2026-04-06 | ~500 token target. Read this first, every session._

---

## Active Threads

- **All 77 Lewis Kelly transcripts ingested.** raw/ is empty — no pending sources.
- **Next decision point:** Select which strategy(ies) to promote from `research` → `development` and begin the Python bot pipeline. See `wiki/analyses/strategy-conviction-ranking.md` for ranked evidence.
- **No strategies are past `research` status yet.** 21 strategies documented, 0 in development/backtesting/live.
- **32 LK sources not yet ingested** from the later batch (lk-fvg-only-video, lk-simple-ob-2025, lk-losing-trades-london, lk-accurate-ms-2025, etc.) — these exist in index but may lack full concept/strategy page updates. Verify during lint.

---

## Key Numbers

| Metric | Value | Source |
|--------|-------|--------|
| Total sources | 79 | index.md |
| LK transcripts | 77 | index.md |
| Concepts | 16 | index.md |
| Strategies | 21 (all `research`) | index.md |
| Entities | 4 | index.md |
| Analyses | 1 | index.md |
| LK 5-Rule Intraday Bias backtest | 230 trades / 216% return / 33% WR / 6.23 RR | lk-5-rule-intraday-bias |
| LK 4-Step Proven backtest | 577 trades / 360%+ return / min 1:5 RR | lk-4-step-proven |
| LK 3-Rule Sniper stat | 97% of losers saved by TP1 at 5R (1000+ trades) | lk-3-rule-sniper |
| LK $100K backtest | 50% fib filter + fixed 1:3 RR, validated | lk-100k-strategy |
| LK Simple SMC | 40% WR / 1:6.4 RR | lk-simple-smc |

---

## Key Decisions Made

- **Candle closure rule (LK):** BOS/CHoCH confirmed only on candle close, never wicks. Implemented in `concepts/market-structure.md` and `concepts/break-of-structure.md`.
- **Sweep vs Run distinction:** Candle closure determines outcome — close beyond level = sweep; no close = run/continuation. Documented in `concepts/liquidity.md`.
- **OB types (LK):** Three types — Strong OB (displacement), Sweep Shift OB (sweeps level then reverses), Continuation Trap OB (false BOS then reversal). All in `concepts/order-blocks.md`.
- **Top-down TF stack (LK):** Daily → 15m → 1m → 5m entry. Documented in `concepts/top-down-analysis.md`.

---

## Open Contradictions

- OB definition varies by educator: LK uses 3-type system (Strong/Sweep/Continuation); ICT definition not yet fully ingested.
- Supply/Demand vs Order Blocks: LK treats them as equivalent zones; traditional S&D educators distinguish them. Flagged in `wiki/overview.md`.

---

## Pending Lint Items

- Concepts mentioned in sources but lacking dedicated pages: check lk-fvg-only-video (FVG updates), lk-5-phase-manipulation (manipulation model).
- Strategy pages created from batch 3/4 ingests may be missing Python Implementation Notes — not yet written for any strategy.
