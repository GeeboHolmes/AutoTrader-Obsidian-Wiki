---
type: strategy
status: research
asset_class: multi
timeframes: [4h, 15m]
style: swing
tags: [smc, lk, swing, 4h-bias, premium-discount, scale-in, multi-day]
---

## Overview
LK's swing trading strategy — holds for days to weeks. Uses 4H for bias, a 50% Fibonacci premium/discount filter, 15m LTF shift for entry confirmation, and a scale-in management system to build position size as the trade develops. Only LK strategy that explicitly uses premium/discount zone filtering.

## Market Bias (Direction Filter)
- **4H external structure (box method):** LH/LL = bearish; HH/HL = bullish.
- Use 4H as primary trend — only trade in direction of 4H structure.

## Setup Conditions
1. 4H external structure is clearly bearish (LH/LL) or bullish (HH/HL).
2. Price has made a significant expansion move on 4H (from swing high to swing low for bearish).
3. Price is now in a retracement phase.
4. **Premium/Discount filter:** Draw 50% Fibonacci retracement of the most recent 4H expansion leg. For bearish setups, only enter shorts when price is ABOVE the 50% level (in premium). Do not enter at or below 50% (discount = buy zone, not sell zone).
5. 15m market structure has shifted bearish (CHoCH on 15m — not 1m, unlike intraday models).

## Entry Rules
1. 4H bearish with price in premium (above 50% fib of last leg).
2. 15m CHoCH to bearish confirmed.
3. Look for 15m entry zone: OB (last buy-to-sell candle), FVG (three-candle gap), breaker block (previously bullish OB now acting as resistance), or inverted FVG (former FVG now overhead resistance).
4. Enter from the zone. Stop above 4H swing high.
5. Target: 4H structural swing low.

## Stop Loss Rules
- Stop above the 4H swing high used to define the bearish structure (or below 4H swing low for longs).
- This is a wide stop by intraday standards — position sizing must reflect the larger stop distance.

## Take Profit Rules
- Primary target: 4H structural swing low (bearish) / swing high (bullish).
- Multi-day hold expected — do not exit early without a clear 4H structural reason.
- Scale-out: close full position at 4H structural target.
- Alternative exit: clear 4H BOS against the position (4H HH in a bearish trade = invalidation).

## Risk Management
- Wide stop (4H swing high/low) requires smaller position size than intraday models for same % risk.
- Scale-in system: initial position at primary entry; add to position at subsequent Asia/London highs (for shorts) as trade progresses.
- Trail stops behind scale-in positions.
- No specific RR data in source — 4H target distances typically much larger than intraday.

## Filters (What Invalidates the Setup)
- Price in discount zone (below 50% fib) for bearish setup — do not enter. Wait for retest of premium.
- 15m structure does not confirm 4H direction — wait.
- No valid 15m entry zone (OB/FVG/breaker) present in the premium zone — wait.
- 4H makes HH (for bearish) — structure invalidated, exit existing position.

## Timeframe Alignment
- 4H: primary bias, target, and stop reference.
- 15m: entry confirmation (CHoCH) and entry zone (OB/FVG/breaker).
- 50% Fib: drawn on 4H expansion leg to define premium/discount.

## Session / Time of Day
- No strict session window — swing trades can be entered at any London/NY session.
- Scale-in opportunities evaluated at each London and NY session open while trade is active.
- Hold through multiple sessions; close only at structural target or invalidation.

## Backtesting Notes
- No specific backtest data in source.
- Scale-in approach is unique to this strategy — not validated with explicit statistics.

## Python Implementation Notes
- 4H swing detection: rolling N-bar high/low on 4H candles; `swing_high = max(candles[-N:]['high'])`
- Premium/Discount: `mid_point = (swing_high + swing_low) / 2; in_premium = current_price > mid_point`
- 15m CHoCH: same mechanism as intraday but on 15m timeframe; swing high/low tracking on 15m
- Breaker block: previously bullish OB that price has traded through (mitigation) and is now testing from below as resistance
- Inverted FVG: a previously bullish FVG now tested from above as resistance
- Scale-in: after initial entry, on each new London/NY session open, evaluate if price is still within trade range and above prior scale-in level for shorts; add position if conditions met
- Position tracker: maintain list of entries, stops, and sizes for each scale-in level

## Open Issues / TODO
- Scale-in trigger precision: "subsequent Asia/London high" — how to define "new high" for scale-in (minimum distance above prior entry?)
- Breaker block and inverted FVG detection are complex — need robust algorithmic implementations
- Premium/Discount filter: 50% of current leg, or of the entire swing range? (Source implies current retracement leg)
- Crypto applicability: 4H structure works on BTC/ETH but session dynamics differ from forex

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-5-step-swing\|LK 5-Step Swing Strategy]] | Initial page created |
