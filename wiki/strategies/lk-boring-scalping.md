---
type: strategy
status: research
asset_class: multi
timeframes: [1m, 5m]
style: scalp
tags: [smc, lk, asia-range, session-scalp, boring-scalping]
---

## Overview
A session-range scalp built on the thesis that London sweeps one side of the Asia range then delivers to the opposite side. The unique feature is the target: always the opposite Asia range extreme (not an external swing). Quick results — examples show 10–15 minutes to target.

## Market Bias (Direction Filter)
- Determined by which side of the Asia range London sweeps first.
- Asia high swept → bearish (expect run to Asia low).
- Asia low swept → bullish (expect run to Asia high).
- No 15m structure bias required — the sweep itself defines direction.

## Setup Conditions
1. Asia session range identified (8:00pm–midnight EST): `asia_high` and `asia_low` marked.
2. London opens (2:00am EST).
3. By London open, price has already swept either the Asia high OR Asia low (taken out the level).

## Entry Rules
1. Identify which Asia extreme was swept at/after London open.
2. Drop to 1m. Price is currently beyond the swept level. Wait for 1m CHoCH in the opposite direction (toward the unswept Asia extreme).
3. After 1m CHoCH confirmed, go to 5m.
4. Look for 5m FVG in the reversal zone (the area where price turned after the sweep).
5. For shorts: can use 50% of FVG (fib 50% retracement of FVG range) as precise entry if OB and FVG coincide.
6. Enter at FVG (or 50% of FVG for precise short entries).

## Stop Loss Rules
- Longs: stop below the FVG low.
- Shorts: stop above the FVG high.

## Take Profit Rules
- **Single target:** opposite Asia range extreme.
  - Asia low swept → TP = Asia high.
  - Asia high swept → TP = Asia low.
- No partials specified — full position to single target.
- Expected time to target: 10–15 minutes (from examples shown).

## Risk Management
- RR is variable — depends on the width of the Asia range.
- Narrow Asia range = low RR potential. Wide Asia range = high RR.
- No minimum RR specified in source — consider filtering trades where Asia range is too narrow for acceptable RR (e.g., < 1:2).

## Filters (What Invalidates the Setup)
- Asia sweep has NOT occurred by London open — no setup available.
- Both sides of Asia range swept — ambiguous; skip.
- 1m CHoCH fails to materialise after sweep — wait, do not force entry.
- NY session: also valid per source, but London is primary window.

## Timeframe Alignment
- No HTF bias required — Asia range defines context.
- 1m: CHoCH confirmation.
- 5m: FVG entry zone.

## Session / Time of Day
- **London:** 2:00–5:00am EST (primary).
- **NY:** 7:00–10:00am EST (also valid per source).
- Asia range: 8:00pm–midnight EST.

## Backtesting Notes
- No specific backtest data provided in source.
- LK describes as "consistent" and "boring" — happens the same way every day.
- Daily cycle variation based on Power of Three (accumulation → manipulation → distribution).

## Python Implementation Notes
- `asia_high = candles[asia_open:asia_close]['high'].max()`
- `asia_low = candles[asia_open:asia_close]['low'].min()`
- Sweep detection at London open: `if current_high > asia_high: swept = 'high'; elif current_low < asia_low: swept = 'low'`
- Target: `tp = asia_low if swept == 'high' else asia_high`
- FVG detection: three-candle pattern where candle[1] body doesn't overlap with candle[-1] wick (gap between candle[-1] high and candle[1] low for bullish FVG)
- State machine: `ASIA_FORMED → LONDON_OPEN → SWEEP_DETECTED → CHoCH_1M → FVG_ENTRY`

## Open Issues / TODO
- Minimum RR filter: Asia range can be very narrow on low-volatility days — need a minimum pip/point threshold.
- "Both sides swept" handling — not addressed in source.
- 50% FVG entry rule for shorts only, or also applicable to longs?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-boring-scalping\|LK Boring Scalping]] | Initial page created |
