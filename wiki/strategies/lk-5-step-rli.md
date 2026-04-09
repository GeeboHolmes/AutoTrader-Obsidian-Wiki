---
type: strategy
status: research
asset_class: forex
timeframes: [15m, 1m, 5m]
style: intraday
tags: [smc, lk, rli, range-liquidation-initiation, dxy, institutional]
---

## Overview
5-step intraday strategy built around the Range Liquidation Initiation (RLI) framework — an institutional mechanics model. Range = accumulation; Liquidation = inducement sweep; Initiation = expansion. DXY (Dollar Index) used as cross-reference confirmation for EUR/USD pairs. $30K/month claimed; $100K in 90 days.

## Market Bias (Direction Filter)
- **15m external structure:** HH/HL = bullish; LH/LL = bearish.
- Identifies the "Initiation" direction — which way the next expansion will go.

## Setup Conditions
1. 15m bias defined (swing high / swing low identified).
2. A Range (consolidation zone) is visible — tight range of candles where price is not trending = institutional accumulation.
3. A Liquidation phase has occurred or is in progress — price has made a fake move out of the range to sweep opposing liquidity.
4. Asia session high (bearish) or low (bullish) has been swept — the liquidation usually takes out the Asia session level.

## Entry Rules
1. Identify the Range zone (accumulation candles — the OB).
2. Wait for the Liquidation (sweep of Asia level / equal highs/lows).
3. After Liquidation, confirm 1m CHoCH in the direction of the 15m bias.
4. Enter at the 5m OB/FVG formed at the Range zone (the Initiation pullback retest).
5. **Optional DXY Cross-Reference (forex only):** If EUR/USD is near stop-loss during draw-down, check DXY. If DXY has a contradicting FVG or structural support holding, EUR/USD will hold. Use this to avoid premature exits.

## Stop Loss Rules
- Stop above the Range zone high (for shorts) or below Range zone low (for longs).
- Alternatively, above the session high (for shorts).

## Take Profit Rules
- Target: next 15m structural low (bearish) or high (bullish).
- Two targets: (1) nearby structural level (~3.6 RR), (2) 1:5 RR.

## Risk Management
- 1% risk per trade.
- DXY cross-reference can save trades during draw-down phases.

## Filters (What Invalidates the Setup)
- No visible Range (accumulation zone) present — setup not valid.
- Asia session level not swept during Liquidation phase.
- 1m CHoCH does not appear after Liquidation.

## Timeframe Alignment
- 15m: bias and Range identification.
- 1m: CHoCH confirmation.
- 5m: OB/FVG entry zone.

## Session / Time of Day
- London 2:00–5:00am EST; NY 7:00–10:00am EST.

## Backtesting Notes
- No specific public backtest data.
- LK claims $100K in 90 days and $30K/month; data is claimed but not independently verified.
- System traded by "inner circle" members replicating LK's live calls.

## Python Implementation Notes
- Range detection: N candles within X% price range (e.g., `high-low < 0.3% of price` for N >= 5 candles)
- RLI phases:
  ```python
  if range_detected and asia_level_swept and choch_confirmed:
      entry = ob_zone_retest
  ```
- DXY cross-reference: requires separate DXY data feed; `dxy_fvg_holding` check before exit decision
- **Note:** DXY cross-reference not applicable for crypto bot

## Open Issues / TODO
- Steps 3-5 are inferred from context; full transcript read needed to confirm exact rules
- DXY integration requires separate data source — skip for crypto bot
- RLI overlaps with Power of Three; clarify if they are equivalent concepts

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-5-step-rli\|LK 5-Step RLI Strategy]] | Initial page created |
