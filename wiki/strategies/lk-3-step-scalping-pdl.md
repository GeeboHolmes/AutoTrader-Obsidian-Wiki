---
type: strategy
status: research
asset_class: multi
timeframes: [1m, 5m]
style: scalp
tags: [smc, lk, pdl-sweep, previous-day-low, scalping, 3-steps]
---

## Overview
A 3-step scalp using the Previous Day's Low (PDL) as the liquidity trigger instead of the Asia range. One of five variations in LK's scalping family — all share the same structure (sweep → CHoCH → OB/FVG) but use different trigger levels. Same 577-trade / 2.5-year dataset as the 4-step proven strategy. Target: 1:3 RR.

## Market Bias (Direction Filter)
- Directional bias is implied by the sweep: PDL sweep = bullish reversal expected.
- No explicit 15m structure bias required — the PDL sweep is the primary filter.
- Trading during London/NY session windows is mandatory.

## Setup Conditions
1. PDL identified: lowest candle low from previous 5pm EST to current 5pm EST.
2. Current session is within London (2:00–5:00am EST) or New York (7:00–10:00am EST).
3. Price trades BELOW the PDL during the session window (PDL swept).

## Entry Rules
1. PDL sweep confirmed (price has traded below PDL).
2. Drop to 1m. Price is currently below PDL. Identify the most recent 1m swing high.
3. Wait for that 1m swing high to be taken (price breaks above it) — this is the 1m CHoCH (market structure shift).
4. If price makes a new low and fails to shift, update the reference swing high to the most recent valid high and wait again.
5. After CHoCH, go to 5m (or 15m). Identify OB (last sell-to-buy candle) or FVG in the reversal zone. Two zones often coincide.
6. Enter from OB high (entry at the top of the OB zone).

## Stop Loss Rules
- Stop below OB low (or FVG low).
- OB low is the floor of the entry zone — if price returns below this, the setup is invalidated.

## Take Profit Rules
- Target: 1:3 RR from entry to stop.
- TP must be at a logical resistance level — swing high, HOD, session high.
- If the 1:3 level is not at or near a structural level, reassess trade.

## Risk Management
- 1:3 RR target — lower than LK's 1:5 A+ minimum. Requires higher win rate to be profitable long-term.
- 577-trade sample over 2.5 years — same dataset as 4-step proven. Win rate not stated in source for this variant.

## Filters (What Invalidates the Setup)
- PDL not swept during session window — no setup.
- 1m CHoCH fails to appear (price keeps making lower lows without shifting) — wait or skip.
- 1:3 TP level has no structural significance (no swing high/HOD nearby) — skip trade.
- Time outside London or NY window — hard reject.

## Timeframe Alignment
- No HTF bias required.
- 1m: CHoCH confirmation and swing high identification.
- 5m or 15m: OB/FVG entry zone.

## Session / Time of Day
- **London:** 2:00–5:00am EST.
- **New York:** 7:00–10:00am EST.
- Daily boundary: 5:00pm EST (defines when PDL resets — lowest low from 5pm to 5pm next day).

## Backtesting Notes
- 577 trades, 2.5 years of data (same dataset as lk-4-step-proven).
- PDL variant is 1 of 5 variations. Other variants use different trigger levels (Asia low, Asia high, PDH, equal lows implied).
- Returns and win rate not explicitly stated for PDL variant in source.

## Python Implementation Notes
- `pdl = candles_previous_day['low'].min()` where previous day = 5pm yesterday to 5pm today
- Daily reset: `if current_time_est == 1700: pdl = calculate_pdl(yesterday_candles)`
- Sweep: `if current_low < pdl: sweep_active = True`
- CHoCH: after sweep, track `latest_1m_swing_high`; if `current_high > latest_1m_swing_high: choch_confirmed = True`
- If new low after sweep: `latest_1m_swing_high = new_swing_high` (update reference)
- RR check: `rr = (tp_level - entry_price) / (entry_price - stop_price)`; require `rr >= 3`
- Validate TP is near structural level: check if any swing high within X% of TP price

## Open Issues / TODO
- Win rate for PDL variant not stated — need to backtest separately to validate profitability at 1:3 vs 1:5 RR
- Other 4 variations (triggers): identify and build separate strategy pages when source material found
- PDL reset at 5pm EST: ensure bot handles timezone correctly across DST changes

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-3-step-scalping-pdl\|LK 3-Step Scalping (PDL)]] | Initial page created |
