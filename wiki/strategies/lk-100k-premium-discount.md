---
type: strategy
status: research
asset_class: forex
timeframes: [15m, 1m]
style: intraday
tags: [smc, lk, premium-discount, 1-to-3-fixed, backtested, pullback-reversal]
---

## Overview
The strategy that made LK's first $100K. Uses 50% Fibonacci premium/discount as the primary entry zone filter, with a fixed 1:3 RR for all trades. Two entry models: (1) discount reversal — buy at structural lows in discount; (2) premium continuation short — sell at OB when price retraces into premium. Backtested on EUR/USD 15m + 1m.

## Market Bias (Direction Filter)
- **15m external structure:** BOS confirmed by CLOSE only (not wick). Bearish = close below swing low.
- **50% Fib filter:** short only in premium (above 50% of current bearish price leg); long only in discount (below 50%).

## Setup Conditions
1. 15m BOS confirmed (close above/below swing level).
2. Price is in the correct zone: premium for shorts, discount for longs.
3. Session window: London 2:00–5:00am EST or NY 7:00–10:00am EST.

## Entry Rules
**Model 1 — Discount Reversal (Long):**
1. 15m structure bearish; price breaks to new low (BOS).
2. Price is in discount (below 50% of current bearish leg).
3. Drop to 1m. Wait for 1m lower low, then 1m CHoCH to bullish (swing high broken on 1m).
4. Identify the 1m OB cluster (the range that caused the shift).
5. Enter at the open of the range. Stop: 4–4.5 pips below range low.
6. Target: 1:3 RR fixed. Optional: hold partial to structural high.

**Model 2 — Premium Continuation Short:**
1. 15m structure bearish; price retraces into premium (above 50%).
2. Drop to 1m. Wait for 1m CHoCH bearish (swing low broken on 1m in the 15m OB zone).
3. Enter at 1m OB cluster. Stop: 4–4.5 pips above range high.
4. Target: 1:3 RR fixed.

## Stop Loss Rules
- Small stop: 4–4.5 pips above/below the 1m OB cluster.
- Same tight stop mechanism as 1m scalping entries.

## Take Profit Rules
- **Fixed 1:3 RR** on all trades — no discretion.
- Optional: partial at 1:3, hold remainder for next structural level.
- No minimum RR filter — 1:3 is both the minimum and the target.

## Risk Management
- 1% risk per trade.
- Fixed 1:3 RR provides consistent math: need only >25% win rate to be profitable.

## Filters (What Invalidates the Setup)
- Price is not in the correct zone (premium for shorts, discount for longs) — do not enter.
- BOS was a wick, not a close — not confirmed.
- Outside London/NY window.

## Timeframe Alignment
- 15m: structure, BOS, 50% fib calculation.
- 1m: entry trigger (CHoCH + OB cluster).

## Session / Time of Day
- London 2:00–5:00am EST; NY 7:00–10:00am EST.
- Can extend to 11:00am EST if setup is already in progress.

## Backtesting Notes
- Backtested live on EUR/USD 15m + 1m in source video.
- No explicit win rate or trade count given in source — "first $100K strategy" implies sustained profitability.
- 1:3 fixed RR is lower than LK's A+ standards (1:5), likely compensated by more frequent setups.

## Python Implementation Notes
- 50% fib: `mid_point = (swing_high + swing_low) / 2`
- Premium check: `current_price > mid_point` (for shorts)
- Discount check: `current_price < mid_point` (for longs)
- BOS close check: `if candle.close < swing_low: bos_confirmed = True` (NOT wick-based)
- 1m OB cluster: identify the last N bearish candles before the bullish CHoCH break
- Fixed RR: `tp = entry - 3 * (entry - stop)` (for shorts)

## Open Issues / TODO
- "Discount reversal" = buying in a bearish trend = counter-trend long. Is there a 15m structure filter to ensure the discount is a pullback in a larger uptrend, or is this always within a single bearish leg?
- Extension to 11am EST: define "setup in progress" algorithmically (price has entered zone and CHoCH pending?)

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-100k-strategy\|LK $100K Strategy]] | Initial page created |
