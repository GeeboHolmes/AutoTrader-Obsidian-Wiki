---
type: source
title: "Advanced Smart Money Trading Strategy (Step by Step)"
source_type: video-transcript
author: Lewis Kelly
date_published: 2023-06-13
date_ingested: 2026-04-06
original_file: raw/Lewis Kelly/13-06-2023 - Advanced Smart Money Trading Strategy (Step by Step).txt
asset: multi
tags: [market-structure, multi-timeframe, supply-demand, 4h-15m, advanced-smc]
---

## Summary
An earlier advanced SMC course structured around 5 steps: basic structure, merging supply/demand with structure, multi-dimensional structure (4H + 15m), combined entry model, and a live trade (11R result). Key contribution: explicit "realignment" concept where the 15m CHoCH signals the 4H pullback phase, and the 15m OB inside a 4H demand zone is the entry.

Early source of the "liquidation ≠ BOS" rule: wick through a level is a liquidation (beneficial — clears stop losses); only a candle close counts as a structural break. This is LK's first explicit statement of the candle closure rule (later expanded in lk-candle-closure).

## Key Points
- **Liquidation vs BOS:** If price only wicks through a high/low (wick but body closes back inside range), that is a LIQUIDATION, NOT a BOS. Body close position determines whether it's truly lower/higher. Liquidations are useful — they remove stop clusters.
- **Realignment concept:** The 15m shifts from bullish to bearish when the 4H is in its pullback phase. The moment the 15m shifts back to bullish (CHoCH from bearish to bullish) = realignment with the 4H intent. This is the precise signal to look for entries.
- **Weak high definition:** "A high that failed to take the corresponding low is weak." It becomes a liquidity target. — consistent with lk-ms-8lessons.
- **Complex pullback = more liquidity:** When price makes a complex pullback (multiple legs), it creates equal highs/equal lows along the way. These are good because they add to stop cluster size at the OB level, increasing the reaction probability.
- **OB must have FVG:** The sell-to-buy OB is identified by having an imbalance (gap between candle 1 high and candle 3 low). OBs without FVG are low priority.
- **50% pullback rule:** Price "almost always" pulls back at least 50% after a BOS. Multiple examples in the transcript confirm 50–90% retracement depth.
- **4-phase supply/demand cycle:** Range → Initiation → Mitigation → Continuation. Applied to both individual price legs and the overall structure.
- **Trade management:** BOS after entry = move stop to break even. Let the continuation run.
- Live trade: 11R result. Variable RR depending on structure.

## Trading Rules / Setups Described
**Step 1 — Basic Structure:** Mark external swing points. Identify BOS (candle CLOSE, not wick). CHoCH = first opposite BOS. Trading range = current swing high to swing low. Internal action inside box = pullback/noise only.

**Step 2 — Supply/Demand + Structure:** OBs form inside structural legs. After BOS, identify sell-to-buy OBs in the pullback zone. OB must have FVG. OB must be in the discount of the structural leg (below 50% for bullish).

**Step 3 — Multi-Dimensional:** 4H = higher frame (trend). 15m = medium frame (entry frame). When 4H is bullish and 15m CHoCH bearish = 4H is in pullback phase. When 15m re-CHoCH bullish = realignment = ready to continue 4H trend. This 15m realignment + price in 4H demand zone = entry trigger.

**Step 4 — Combined Entry:**
1. 4H bullish, in pullback phase.
2. 15m bearish → enters 4H demand zone.
3. 15m CHoCH bullish (realignment). Identify 15m sell-to-buy OB with FVG at the CHoCH level.
4. Enter from 15m OB. Stop below the 15m structural low. Target 4H swing high.
5. After entry: if price BOS to the upside, move stop to break even.

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/market-structure|Market Structure]], [[concepts/order-blocks|Order Blocks]], [[concepts/fair-value-gap|Fair Value Gap]], [[concepts/liquidity|Liquidity]], [[concepts/premium-discount|Premium / Discount]], [[concepts/displacement|Displacement]], [[concepts/top-down-analysis|Top-Down Analysis]]

## Strategies Mentioned
[[strategies/lk-advanced-smc-2023|LK Advanced SMC 2023]]

## Contradictions / Open Questions
- This source uses 15m as the entry (not 1m), while later LK sources use 1m. The overall model evolved from "15m CHoCH = entry" (2023) to "15m CHoCH → 1m CHoCH = entry" (2024+). Both work, but the 1m approach gives better RR.
- "16 15min candles = 4H" — this is LK's stated rule for timeframe selection (15–20x ratio). Useful for bot timeframe configuration.

## Quotes
> "If price only wicks it's a liquidation — it doesn't count as a break of structure."
> "When the 15 minute shifts from bearish to bullish — that is usually pairing supply and demand with internal structure — if you compare them and you get this model inside of that zone, you've got the right zone."
> "Their entry point is our exit point — because we take profits at this 4-Hour High. Why? Because we expect when this High gets broken we're going to come and pull back."
