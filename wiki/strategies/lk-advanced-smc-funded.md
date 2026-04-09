---
type: strategy
status: research
asset_class: multi
timeframes: [1h, 15m, 1m]
style: intraday
tags: [smc, lk, advanced, stacked-liquidity, prop-trading, funded, high-probability]
---

## Overview
LK's most advanced intraday strategy — designed for prop firm (FTMO-style) consistency requirements. The key differentiator is **stacked liquidity**: requiring multiple liquidity types to coincide at the same price level before treating it as a valid target. Additionally introduces the "internal realignment signal" for earlier market structure reads. Requires FVG + OB confluence for entries (not single-zone).

## Market Bias (Direction Filter)
- **15m external structure** (primary for most instruments).
- **1H external structure** when the market is in a wide range — use whichever timeframe clearly shows external swing sequence.
- **Internal realignment signal:** an internal CHoCH within a larger external range signals that price is about to deliver to the opposite external extreme. This allows reading the next external BOS direction early.
- Wick through a swing point = liquidation, not BOS (same as all LK models).

## Setup Conditions
1. External structure defined on 15m or 1H.
2. Stacked liquidity cluster identified: at minimum TWO types coinciding at the same price level.
   - Type A: trendline (3+ touch points).
   - Type B: Asia session high or low.
   - Type C: equal highs or equal lows.
   - Type D: weak high (last swing high formed before a lower low) or weak low.
   - Type E: prior session high/low (HOD, LOD, prior day extremes).
3. The stacked cluster is the trade TARGET (not entry zone).
4. An institutional area of interest (FVG + OB confluence — both must be present) identified between current price and the stacked target.

## Entry Rules
1. All setup conditions met.
2. Price retraces from the stacked target direction toward the institutional zone.
3. Price enters the confluence zone (FVG + OB overlap).
4. Drop to 1m. Wait for 1m CHoCH in the direction of the stacked target.
5. Enter on 1m CHoCH confirmation (candle close or subsequent OB).
6. Stop: above the institutional zone high (shorts) / below zone low (longs).
7. Target: the stacked liquidity cluster.

## Stop Loss Rules
- Stop outside the institutional entry zone (above FVG high for shorts / below FVG low for longs).
- Conservative placement — zone should be respected if the setup is valid.

## Take Profit Rules
- Target: the stacked liquidity cluster (Step 2 setup condition).
- No partial-take specified in source — single target.
- If price reaches the stacked target without taking all individual liquidity types, it may continue — reassess.

## Risk Management
- Strict confluence requirements (FVG + OB both required) reduce trade frequency.
- Higher probability per trade justifies larger position sizing within prop firm rules.
- Stacked liquidity targets with 3+ types = A+ setup (highest confidence).

## Filters (What Invalidates the Setup)
- Only one liquidity type at target level — not stacked, skip.
- Entry zone has only FVG or only OB (not both) — reduce to standard model, not advanced model.
- Internal realignment signal points opposite to intended trade direction — reassess bias.
- Major news event within session window.

## Timeframe Alignment
- 1H or 15m: structure and stacked liquidity identification.
- 15m: institutional zone (FVG + OB).
- 1m: CHoCH entry trigger.

## Session / Time of Day
- London (2:00–5:00am EST) and NY (7:00–10:00am EST) — standard LK windows.
- No hard cutoff explicitly stated but session context applies.

## Backtesting Notes
- No specific backtest data in source.
- Designed for prop firm consistency — implies lower drawdown priority over maximum RR.

## Python Implementation Notes
- Stacked liquidity scoring:
  ```python
  stack_score = 0
  if trendline_present: stack_score += 1
  if asia_level_present: stack_score += 1
  if equal_highs_present: stack_score += 1
  if weak_high_present: stack_score += 1
  if prior_session_level_present: stack_score += 1
  if stack_score < 2: skip  # minimum 2 types required
  ```
- Weak high: last swing high where the NEXT swing is a lower low; `is_weak = (next_swing_low < prev_swing_low)`
- FVG + OB confluence: both zones must overlap; `confluence_zone = intersection(fvg_range, ob_range)`
- Internal realignment: track internal structure within the current external range; internal CHoCH = signal
- State machine: `STRUCTURE → STACKED_TARGET → INSTITUTIONAL_ZONE → CHoCH_1M → ENTRY`

## Open Issues / TODO
- Weak high/low algorithmic definition — depends on which swing algorithm is used and lookback period
- Equal highs threshold for this model — same as retail trap (within 5bps) or looser?
- Trendline detection: 3+ touch requirement is complex; consider simplifying to just equal highs/lows for initial implementation
- "Internal realignment signal" — needs more precise definition; which internal swing sequence constitutes a valid realignment?
- FVG + OB confluence overlap — what minimum overlap percentage counts as confluence?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-advanced-smc-funded\|LK Advanced SMC (Funded)]] | Initial page created |
