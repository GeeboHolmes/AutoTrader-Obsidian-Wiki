---
type: strategy
status: research
asset_class: multi
timeframes: [15m, 5m, 1m]
style: intraday
tags: [smc, asia-sweep, lk, session-based, backtested, minimum-rr]
---

## Overview
Lewis Kelly's 4-step proven intraday strategy with documented backtest results: 577 trades, 360%+ return over 4–5 years (2021–2025). Identical Asia-sweep mechanic to [[strategies/lk-5-step-asia-sweep|lk-5-step-asia-sweep]] but adds a mandatory minimum 1:5 RR constraint — the entry point within the OB is adjusted (pulled lower for longs) until a 1:5 reward-to-risk ratio to the swing high target is achievable. This mathematical constraint is the core differentiator of this model. Source: [[sources/lk-4-step-proven|29-12-2025 transcript]].

## Market Bias (Direction Filter)
1. Use 15m external swing structure only — ignore internal structure completely.
2. **Bullish:** Most recent leg has broken the previous external swing high. Current range = swing low (support) to swing high (resistance).
3. **Bearish:** Most recent leg has broken the previous external swing low.
4. Old price action (prior swing highs/lows outside current range) is irrelevant — focus only on current swing high and current swing low.
5. After CHoCH on 15m (bias flip), the new structure supersedes all prior structure.

## Setup Conditions
1. 15m external bias is clearly established.
2. Asia session (8pm–midnight EST) has formed and closed — Asia low (for longs) or Asia high (for shorts) is identified.
3. London or New York session is active.
4. Asia low (bullish) or Asia high (bearish) has been swept by price. **Mandatory — no sweep = no trade.**
5. An OB or FVG exists in the reversal zone that provides at least 1:5 RR to the swing high target.

## Entry Rules
1. Price has swept Asia low (bullish) or Asia high (bearish).
2. Price is inside or near a 5m OB or FVG zone.
3. Drop to 1m — observe structure: confirm 1m is bearish (for a long setup: LH/LL sequence in progress as price pulls into zone).
4. Wait for 1m CHoCH (higher high forms, breaking the most recent 1m swing high) — this is the green light.
5. Identify the 5m OB at the CHoCH level.
6. **RR check:** Calculate entry (5m OB level) vs stop (1m swing low at CHoCH) vs target (15m swing high). If RR < 1:5, pull entry lower within the OB until RR ≥ 1:5. If no level within the OB achieves 1:5, skip the trade.
7. Enter at the adjusted entry level. Place limit order if price has already moved, or enter at next candle open.

## Stop Loss Rules
- Below the 1m swing low created at the CHoCH reversal point.
- Not at the OB low — tighter stop tied to the 1m structural low.

## Take Profit Rules
- Target: external swing high (15m) — the most recent HTF high that has not yet been taken.
- Full exit at target (no partials mentioned in source, but standard LK practice is single target).
- Do not close early — the math model requires following through to the full target.

## Risk Management
- Risk per trade: 1% of account (see [[concepts/risk-management]])
- **Minimum RR: 1:5** — hard rule; no trade below this threshold.
- Expected RR range: 1:5 to 1:8+ (LK shows 1:8.8 and 1:5 examples).
- Max daily loss: 3% of account (project standard).
- Do not override the strategy mid-trade (closing early is explicitly identified as the primary failure mode).

## Filters (What Invalidates the Setup)
- Asia low NOT swept → wait, no entry.
- RR check fails (< 1:5 achievable within OB) → skip trade.
- 15m bias unclear → no trade.
- 1m CHoCH has not confirmed → no entry.
- Entry outside London or New York session → skip.
- OB has been mitigated ([[concepts/mitigation]]).

## Timeframe Alignment
| Timeframe | Purpose |
|-----------|---------|
| 15m | Directional bias (external swings only) |
| 5m | OB/FVG identification; entry zone |
| 1m | Entry confirmation (CHoCH trigger); stop placement |

## Session / Time of Day
- **Asia session:** 8:00pm–midnight EST — liquidity pool formation only.
- **London session:** 2:00am EST — primary window (green box).
- **New York session:** secondary window (orange box).
- LK notes 7 variations of this strategy; this is the London/Asia low variant.

## Backtesting Notes
- LK reports: 577 trades, 360%+ return, 2021–2025, consistent profitable months (12–32%).
- Anti-breakout philosophy validated by data: retracement entries yield 1:6–1:8 RR; breakout entries yield ~1:1.
- Fractal principle: setup works on all timeframes simultaneously (30m OB may contain 5m OB may contain 1m OB).

## Python Implementation Notes
- Asia session: `asia_open = 20:00 EST`, `asia_close = 00:00 EST`. Convert to UTC for exchange API.
- `asia_low = candles[asia_open:asia_close].low.min()`
- Asia sweep: `current_price < asia_low` (for longs) — confirmed on candle close.
- Swing structure (15m): detect using 3-candle pivot rule (see [[concepts/swing-points]]) on external swings only.
- 1m CHoCH detection: track last N swing highs/lows on 1m; detect first candle close above most recent 1m swing high.
- RR calculation: `rr = (target_price - entry_price) / (entry_price - stop_price)`. If `rr < 5`, lower `entry_price` within OB range until `rr >= 5` or OB exhausted.
- OB range: `ob_low = min(ob_candle.low, ob_candle.open)`, `ob_high = max(ob_candle.high, ob_candle.close)`. Entry scans from `ob_high` downward until 1:5 found.
- State machine: `BIAS → ASIA_RANGE → SWEEP → CHoCH → RR_CHECK → ENTRY`

## Open Issues / TODO
- [ ] Verify backtested 577-trade sample is applicable to crypto (LK trades forex primarily — apply to BTC/ETH?).
- [ ] Implement RR scan within OB: loop from OB high → OB low, find first price level where RR ≥ 5.
- [ ] Define 1m swing high/low lookback: how many candles to validate a swing? LK uses 3-candle rule on 15m — apply same to 1m?
- [ ] Test this model against [[strategies/lk-5-step-asia-sweep]]: does the 1:5 minimum improve Sharpe ratio?
- [ ] "7 variations" — document other 6 variations as discovered in further transcripts.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | [[sources/lk-4-step-proven\|29-12-2025 transcript]] | Initial page created from source |
