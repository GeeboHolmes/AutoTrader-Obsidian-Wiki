---
type: strategy
status: research
asset_class: multi
timeframes: [1h, 15m, 1m, 5m]
style: intraday
tags: [smc, lk, liquidity-trap, reversal, retail-stops, sweep-reversal]
---

## Overview
A reversal strategy that exploits the predictable clustering of retail stop-losses at visible price levels. Identify where retail traders place stops (equal highs, resistance, trendlines), wait for institutional sweep of those levels, then enter in the opposite direction after LTF CHoCH. $8,000 live trade example; 1:3 RR target.

## Market Bias (Direction Filter)
- Direction is determined by the sweep: upward sweep of equal highs/resistance → short.
- Downward sweep of equal lows/support → long.
- No HTF structure bias required — this is a pure liquidity-trap model.

## Setup Conditions
1. Identify a retail liquidity cluster on 15m or 1H: equal highs, trendline (3+ touches), prior session high/low, round number, or clear S/R level.
2. Mark the cluster as a pending liquidity pool.
3. During London or NY session, price runs THROUGH the cluster — stop-losses triggered.
4. Sweep candle often appears as a wick (spike through and back) or brief breakout that fails.

## Entry Rules
1. Sweep of the liquidity cluster confirmed (price has traded beyond the cluster level).
2. Drop to 1m. Wait for 1m CHoCH in the opposite direction (bearish CHoCH after upside sweep, bullish CHoCH after downside sweep).
3. After CHoCH, identify 1m or 5m OB/FVG in the reversal zone.
4. Enter at the OB/FVG.
5. Stop: above the sweep wick high (shorts) or below sweep wick low (longs) — outside the manipulation zone.

## Stop Loss Rules
- Stop placed beyond the wick of the sweep candle, not at the cluster level.
- This ensures the stop is above the manipulation high (for shorts) — if price continues beyond the wick, the trap thesis is invalid.

## Take Profit Rules
- Target: 1:3 RR from entry to stop.
- TP should coincide with a structural support/resistance level.
- If 1:3 level has no structural significance, reassess.

## Risk Management
- 1:3 RR — lower than LK's 1:5 standard for A+ setups. Reflects the shorter swing distance of trap reversals.
- No backtest data; $8,000 live trade is a single example.

## Filters (What Invalidates the Setup)
- Cluster level has already been swept previously and price did NOT reverse (institutional level, not retail) — avoid.
- Sweep wick is extremely large (stop must be so far away that RR falls below 1:3) — skip.
- 1m CHoCH does not appear after sweep — wait longer or abandon.
- Major news event within 30 minutes — avoid.

## Timeframe Alignment
- 1H/15m: cluster identification.
- 1m: CHoCH confirmation.
- 1m/5m: OB/FVG entry zone.

## Session / Time of Day
- London (2:00–5:00am EST) and NY (7:00–10:00am EST) — sweeps most commonly occur here.
- No hard cutoff specified in source but session context applies.

## Backtesting Notes
- No backtest data in source.
- Single live trade example: $8,000 in 3 hours.
- 1:3 RR is the stated target — lower than other LK models.

## Python Implementation Notes
- Equal highs detection: `abs(swing_high_A - swing_high_B) / swing_high_A < 0.0005` (within 5 bps = "equal")
- Trendline detection: fit line through N swing highs; if slope is consistent and R² > threshold, trendline valid
- Sweep detection: `if current_high > cluster_level and candle closes back below: sweep_confirmed = True`
- Alternative sweep: `if candle.high > cluster_level and candle.close < cluster_level: wick_sweep = True`
- CHoCH after sweep: same 1m structure tracking as intraday models
- RR validation: `stop = sweep_wick_high; tp = entry - 3 * (stop - entry); rr = (entry - tp) / (stop - entry)`

## Open Issues / TODO
- Equal highs threshold — what percentage difference counts as "equal"? Source doesn't specify.
- Trendline detection is complex — viable for automation? May need minimum 3 touches + recency requirement.
- Differentiating retail vs institutional levels — a prior session high is both. Need context (is it swept and reversed? Then retail. Does price consolidate above it? Then institutional.)
- No HTF bias — can lead to trading against a strong trend. Consider adding HTF filter optionally.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-retail-trap-reversal\|LK Retail Trap Reversal]] | Initial page created |
