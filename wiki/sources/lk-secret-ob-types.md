---
type: source
title: "This Secret Order Block Works Every Time (Proven Results)"
source_type: video-transcript
author: "Lewis Kelly"
date_published: 2025-10-29
date_ingested: 2026-04-06
original_file: raw/Lewis Kelly/29-10-2025 - This Secret Order Block Works Every Time (Proven Results).txt
asset: multi
tags: [smc, order-blocks, sweep-shift-ob, continuation-trap-ob, ob-selection, 800-trades]
---

## Summary
LK presents the two specific OB types that separate winning from failing OBs, identified from 5 years / 800 trades / 1,250 days of data. The "hack" to double RR: instead of entering at the OB directly, drop to 1m when price enters the OB zone and wait for 1m CHoCH confirmation before entering — this doubles or triples RR (from 1:3 to 1:7.2 shown) while also adding confirmation.

## Key Points
- 5 years / 800 trades / 1,250 days of data analyzed
- Two winning OB types identified:
  1. **Sweep Shift OB** — OB at the level that caused a sweep + CHoCH combo
  2. **Continuation Trap OB** — OB that caused an internal CHoCH within a larger external range, coinciding with Asia session liquidity
- **RR doubling hack:** drop to 1m at OB zone, wait for 1m CHoCH instead of placing limit order directly on OB
- 1:3 becomes 1:7.2 using LTF confirmation vs direct OB limit order
- Session window still required: London open was used in example

## Trading Rules / Setups Described
1. **Sweep Shift OB (Winning OB Type 1):** Price is in an uptrend (HH/HL). Price sweeps a swing high (breaks above old high = buy-side liquidity run). Then price aggressively reverses — CHoCH to bearish. The OB to trade is the LAST BULLISH CANDLE before the aggressive sell-off (the candle just before the expansion move down). This is the "sweep shift" — the OB is at the location of the sweep + shift. When price returns to this level, enter short from OB high, stop above OB high, target next structural swing low.
2. **Continuation Trap OB (Winning OB Type 2):** In a trending market (bearish example), after a structural reversal (CHoCH), price pulls back with internal bearish structure. Within this pullback, an internal CHoCH occurs to the bullish side. The OB that caused this internal CHoCH is the continuation trap — it sits just below Asia session low (as additional liquidity). Enter long from the OB, stop below OB low, target the external structural swing high.
3. **RR Doubling Hack:** Instead of placing a limit order directly on the OB zone, wait for price to enter the OB zone. Drop to 1m. Watch 1m price action — it will be trending counter to the trade direction as it enters the zone. Wait for 1m CHoCH in the trade direction (sellers take a 1m swing low for shorts). Then enter at the resulting 1m OB/FVG. This dramatically tightens the stop while keeping the same target = 2x–3x RR improvement.

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/order-blocks|Order Blocks]], [[concepts/market-structure|Market Structure]], [[concepts/liquidity|Liquidity]], [[concepts/break-of-structure|BOS / CHoCH]], [[concepts/killzones|Killzones]]

## Strategies Mentioned
[[strategies/lk-simple-smc|LK Simple SMC Strategy]], [[strategies/lk-5-rule-intraday-bias|LK 5-Rule Intraday Bias Model]]

## Contradictions / Open Questions
- "Sweep Shift OB" implies the OB is at a SWEEP level — this is essentially what lk-retail-trap-reversal already describes. Are these the same concept?
- Data (800 trades, 5 years) is for OB selection criteria, not full strategy — win rate of these specific OBs vs non-qualifying OBs not stated.
