---
type: strategy
status: research
asset_class: multi
timeframes: [15m, 1m, 5m]
style: intraday
tags: [smc, lk, intraday, order-blocks, fvg, institutional-logic]
---

## Overview
LK's "Simple SMC Strategy" — the pedagogical core of his framework. Three steps: 15m bias → OB/FVG POI → 1m CHoCH within zone. Emphasises the institutional logic behind why OBs form and why price returns to them. Live stats: 40% win rate, 1:6.4 avg RR. $24,000 live trade example.

## Market Bias (Direction Filter)
- **15m external structure (box method):** External swing points only. HH/HL = bullish, LH/LL = bearish.
- Institutional direction determines which zones to look for (buy zones for longs, sell zones for shorts).

## Setup Conditions
1. 15m bias clearly defined (external swing sequence visible).
2. Price has expanded away from a recent OB or FVG, leaving an unfilled institutional zone.
3. Price is now pulling back toward that zone.

## Entry Rules
1. Identify the 5m OB (last candle before expansion move — body direction flips and next candle is a large displacement candle) OR FVG (three-candle imbalance gap) in the direction of 15m bias. Prefer confluence of both at same level.
2. Wait for price to enter the zone.
3. Drop to 1m. Within the zone, 1m will be trending counter to 15m bias (pullback is in progress).
4. Wait for 1m to shift in the direction of 15m bias (1m CHoCH). Consolidation within the zone = seller/buyer exhaustion. CHoCH = entry trigger.
5. Enter on the CHoCH candle or at the subsequent 1m OB.
6. Stop at the 1m swing low (longs) or 1m swing high (shorts).

## Stop Loss Rules
- Stop at the 1m swing low/high at the CHoCH point (tight LTF stop).
- This placement generates high RR entries from institutional zones.

## Take Profit Rules
- Target: 15m structural swing high (longs) or swing low (shorts).
- No explicit partial rules — full position to target implied from source.

## Risk Management
- 40% win rate, 1:6.4 avg RR (source stats).
- At 1:6.4 RR and 40% win rate: expectancy = (0.4 × 6.4) - (0.6 × 1) = 1.96 per trade — highly profitable.

## Filters (What Invalidates the Setup)
- 15m structure is unclear or price is mid-range with no defined swing sequence.
- OB/FVG has been previously mitigated (price has traded through it before — the zone is "used up").
- 1m CHoCH does not materialise after price enters zone — do not force entry.

## Timeframe Alignment
- 15m: bias and zone identification
- 5m: zone precision (OB/FVG location)
- 1m: entry trigger (CHoCH) and stop placement

## Session / Time of Day
- Session windows not explicitly mandated in this source, but London and NY implied from LK's base framework.

## Backtesting Notes
- 40% win rate, 1:6.4 avg RR shown in source (instrument and sample size not specified).
- $24,000 live trade example demonstrates the institutional logic in practice.
- These stats differ from lk-5-rule-intraday-bias (33% win rate, 6.23 RR) — likely different sample or fewer filters.

## Python Implementation Notes
- OB identification: find last candle before large expansion; `ob_high = candle.high; ob_low = candle.low`
- Mitigation check: `if price_has_traded_through_ob_range: zone_is_mitigated = True; skip`
- FVG: gap between `candle[-1].high` and `candle[1].low` (bullish) or `candle[-1].low` and `candle[1].high` (bearish)
- Zone entry: `if current_low <= ob_high and current_high >= ob_low: in_zone = True`
- CHoCH on 1m: same mechanism as other models — track rolling swing high/low
- Confluence check: `if ob_zone overlaps fvg_zone: confluence = True; prefer this entry`

## Open Issues / TODO
- OB mitigation definition — "traded through" means body close beyond zone? Or any wick?
- Seller exhaustion / consolidation — define algorithmically (N bars of narrow range within zone?)
- Session filter — should London/NY windows be mandatory for this simpler model, or is it more flexible?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-simple-smc\|LK Simple SMC]] | Initial page created |
