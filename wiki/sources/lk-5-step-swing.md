---
type: source
title: "This 5 Step Swing Trading Strategy"
source_type: video-transcript
author: "Lewis Kelly"
date_published: 2025-04-28
date_ingested: 2026-04-06
original_file: raw/Lewis Kelly/28-04-2025 - This 5 Step Swing Trading Strategy.txt
asset: multi
tags: [smc, swing-trading, 4h-bias, premium-discount, scale-in, multi-day]
---

## Summary
LK's only explicit swing trading strategy — holds positions for days to weeks rather than hours. The shift from intraday to swing changes the bias timeframe (4H instead of 15m), adds a premium/discount filter (only trade shorts in premium above 50% fib), and introduces a scale-in management system where additional positions are added at subsequent sessions. The core entry mechanics (LTF shift + FVG/OB zone) remain the same as intraday variants.

The model is presented as a bearish-only example (LH/LL on 4H), which implies the same approach applies in reverse for longs. Multi-day holding is enabled by using 4H targets (next 4H structural swing low) rather than intraday session highs/lows.

## Key Points
- Bias timeframe: 4H (not 15m) — external structure; LH/LL = bearish, HH/HL = bullish
- Premium/Discount filter: 50% Fibonacci of current 4H leg; ONLY short in premium (above 50%)
- 15m LTF must shift to bearish before entry (same as intraday model)
- Entry: 15m FVG, OB, breaker block, or inverted FVG within the reversal zone
- Stop: above the 4H swing high (wider stop than intraday)
- Target: 4H structural swing low (multi-day holding expected)
- Scale-in: add additional positions at subsequent Asia/London highs as trade progresses
- Trail stops behind added positions; hold until 4H target reached

## Trading Rules / Setups Described
1. **Step 1 — 4H HTF Bias:** Use 4H chart. External structure only (box method). Bearish = LH/LL series. Bullish = HH/HL series. This is the primary directional filter.
2. **Step 2 — Premium/Discount Filter:** Draw 50% Fibonacci retracement of the most recent 4H expansion leg (swing high to swing low for bearish). Only enter shorts when price is in premium (above the 50% level). Do not enter at discount.
3. **Step 3 — 15m LTF Bias Shift:** Drop to 15m. Wait for 15m market structure to shift bearish (CHoCH to downside). This confirms the 4H retracement is ending and distribution is beginning.
4. **Step 4 — Entry Zone:** Look for 15m entry model: FVG, OB (last buy-to-sell candle), breaker block (previously bullish OB now acting as resistance), or inverted FVG. Enter from the zone. Stop above the 4H swing high used in Step 1.
5. **Step 5 — Scale-In Management:** After initial entry, hold for swing. At each subsequent London/NY session where price is still above entry, evaluate adding to the position at the next Asia high or London high. Trail stop loss behind each added position. Final target: 4H structural swing low. Close full position at target or on clear 4H BOS against position.

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/market-structure|Market Structure]], [[concepts/liquidity|Liquidity]], [[concepts/order-blocks|Order Blocks]], [[concepts/fair-value-gap|Fair Value Gap]], [[concepts/break-of-structure|BOS / CHoCH]], [[concepts/premium-discount|Premium / Discount]]

## Strategies Mentioned
[[strategies/lk-5-step-swing|LK 5-Step Swing Strategy]]

## Contradictions / Open Questions
- Premium/Discount filter (50% fib) is only mentioned in this source — not present in any intraday models. Intentional for swing context only?
- Scale-in rules lack precision — "subsequent Asia/London high" needs algorithmic definition (how far above the previous high constitutes a new high for scale-in purposes?).
- No specific backtest data provided for this strategy.
