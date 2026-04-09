---
type: strategy
status: research
asset_class: multi
timeframes: [daily, 4h, 15m, 1m]
style: intraday
tags: [smc, lk, daily-bias, power-of-three, htf-magnets, candle-character]
---

## Overview
A dedicated daily bias determination system — the HTF framework that sits above all intraday entries. The strategy reads the previous day's candle character to predict next-day direction, identifies HTF magnets (equal highs/lows, H4/Daily FVGs), then trades the Power of Three distribution phase after the manipulation fake-out. Can be used as a standalone strategy or layered with any LK intraday model.

## Market Bias (Direction Filter)
Determined entirely by the previous day's candle character. Four patterns:
- **Bearish continuation:** previous day closed below PDL (prior day's low) → today bearish.
- **Bullish continuation:** previous day closed above PDH (prior day's high) → today bullish.
- **Bearish rejection:** previous day wicked above PDH then closed inside prior range → today bearish.
- **Bullish rejection:** previous day wicked below PDL then closed inside prior range → today bullish.

## Setup Conditions
1. Previous day's candle has clearly closed (after 5pm EST).
2. One of four daily candle patterns identified — direction confirmed.
3. HTF magnet identified: nearest equal highs/lows, H4 FVG, or session extreme in the direction of the daily bias.
4. Asia accumulation phase has completed (midnight EST).

## Entry Rules
1. At London open (2:00am EST), observe direction of initial price movement.
2. If price moves AGAINST the daily bias (manipulation phase) — wait. This is the fake move to grab stop-loss liquidity.
3. Once manipulation completes (LTF CHoCH signals end of manipulation), enter in direction of daily bias.
4. Entry zone: 15m OB or FVG formed during or after the manipulation phase.
5. Stop: below manipulation low (longs) / above manipulation high (shorts) — the wick of the fake move.
6. Target: HTF magnet identified in setup.

## Stop Loss Rules
- Stop outside the manipulation wick: below the sweep low for longs, above the sweep high for shorts.
- This places the stop beyond the fake move, reducing false stop-outs.

## Take Profit Rules
- Target: HTF magnet (equal highs/lows, H4 FVG, prior session extreme) in the direction of daily bias.
- No partial rules specified — single target implied.

## Risk Management
- No specific RR or win rate data in source.
- Manipulation phase identification inherently provides a defined risk level (the fake move wick).

## Filters (What Invalidates the Setup)
- Daily candle pattern is ambiguous (inside bar, doji, no clear character) — skip day.
- No HTF magnet visible in the direction of daily bias — skip.
- Manipulation phase not observable (price goes directly in bias direction from open — no fake move) — can still trade but stop logic changes.

## Timeframe Alignment
- Daily: candle character read (bias determination)
- H4/Daily: HTF magnet identification (target)
- 15m: entry zone (OB/FVG after manipulation)
- 1m: CHoCH confirmation of manipulation phase end

## Session / Time of Day
- Daily candle read: after 5pm EST close.
- Asia: 8pm–midnight EST (accumulation forms).
- London: 2am–5am EST (manipulation then distribution).
- NY: 7am–10am EST (continuation/distribution phase 2).
- Power of Three: Asia = accumulation; London = manipulation; NY/London continuation = distribution.

## Backtesting Notes
- No specific backtest data in source.
- Framework is the theoretical basis for all LK intraday entries — used as context layer, not standalone in most LK models.

## Python Implementation Notes
- Daily candle data: require daily OHLC from previous session (5pm–5pm EST)
- Pattern detection:
  ```python
  if prev_day_close < prior_day_low:  # bearish continuation
  elif prev_day_close > prior_day_high:  # bullish continuation
  elif prev_day_high > prior_day_high and prev_day_close < prior_day_high:  # bearish rejection
  elif prev_day_low < prior_day_low and prev_day_close > prior_day_low:  # bullish rejection
  ```
- HTF magnet: scan H4/Daily for equal highs (two+ candles with high within X% of each other), unfilled FVGs, prior session highs/lows
- Manipulation detection: LTF CHoCH after initial move opposes daily bias
- State machine: `DAILY_READ → HTF_MAGNET → ACCUMULATION → MANIPULATION → DISTRIBUTION_ENTRY`

## Open Issues / TODO
- "Inside prior range" definition for rejection patterns — needs precise threshold
- Equal highs/lows detection threshold (what % difference counts as "equal"?)
- Crypto daily boundary: 5pm EST vs midnight UTC — which to use for 24h market?
- Manipulation phase can sometimes be very brief — minimum duration/distance needed to qualify?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-daily-bias\|LK Daily Bias Strategy]] | Initial page created |
