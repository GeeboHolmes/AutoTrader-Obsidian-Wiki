---
type: source
title: "My 3 Step Scalping Strategy (Backtested & Live Trade's)"
source_type: video-transcript
author: "Lewis Kelly"
date_published: 2025-10-22
date_ingested: 2026-04-06
original_file: raw/Lewis Kelly/22-10-2025 - My 3 Step Scalping Strategy (Backtested & Live Trade's).txt
asset: multi
tags: [smc, scalping, pdl-sweep, 3-steps, backtested, previous-day]
---

## Summary
LK presents a 3-step scalping strategy using the Previous Day's Low (PDL) as the liquidity sweep trigger. This is identified as "one of five variations" in LK's family of scalping strategies — all sharing the same structure but with different liquidity trigger levels. The PDL variant uses the 577-trade / 2.5-year dataset also referenced in the [[sources/lk-4-step-proven|4-step proven strategy]], suggesting these are variations of the same underlying backtested system.

The strategy is the simplest possible: sweep PDL during London/NY window → 1m CHoCH → 5m OB/FVG entry → target 1:3 RR. No directional bias step is mentioned. LK defines the daily boundary as 5pm EST (daily open/close). The previous day's low = lowest price reached from 5pm to next 5pm.

## Key Points
- Uses PDL (previous day's low) as the sweep trigger — not Asia session
- Daily boundary: 5pm EST (Forex daily open/close)
- PDL = lowest candle low between 5pm and next 5pm
- 577 trades, 2.5 years of data — same dataset as [[sources/lk-4-step-proven|4-step proven strategy]]
- PDL sweep is 1 of 5 variations; other variations use different liquidity levels
- Trading windows: London 2-5am EST, NY 7-10am EST — mandatory
- Target: 1:3 RR to next resistance level
- Stop: below FVG low / OB low

## Trading Rules / Setups Described
1. **Step 1 — Sweep Previous Day's Low:** Identify PDL from previous trading day (5pm–5pm EST). During London (2-5am) or New York (7-10am) session, wait for price to trade BELOW the PDL. Once breached, step 1 is complete.
2. **Step 2 — 1m Market Structure Shift:** Drop to 1m. Price is currently below PDL. Identify the most recent 1m swing high. Wait for that swing high to be taken — this is the market structure shift. Update the structural high if price makes a new low and fails to shift (wait for the most recent valid high).
3. **Step 3 — Area of Interest + 1:3 Target:** Go to 5m or 15m. Identify OB or FVG (the "sell-to-buy" at the reversal = area of interest). Two zones often overlap. Enter from OB high (cover OB low as stop). Target: 1 to 3 RR from entry to stop. Ensure TP level is a logical resistance (swing high, HOD, session high).

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/liquidity|Liquidity]], [[concepts/order-blocks|Order Blocks]], [[concepts/fair-value-gap|Fair Value Gap]], [[concepts/break-of-structure|BOS / CHoCH]], [[concepts/killzones|Killzones]]

## Strategies Mentioned
[[strategies/lk-3-step-scalping-pdl|LK 3-Step Scalping (PDL Variant)]]

## Contradictions / Open Questions
- No directional bias step — unlike all other LK strategies which require 15m bias confirmation. Deliberate simplification for mechanical backtesting, or was bias assumed?
- 1:3 RR target is fixed — LK other models use 1:5+. Lower RR may require higher win rate.
- "Five variations" — PDL is one. Others likely: Asia low, Asia high, Previous Day's High, equal lows.
