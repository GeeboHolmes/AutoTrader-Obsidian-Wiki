---
type: source
title: "How To Accurately Read Market Structure (Step by Step)"
source_type: video-transcript
author: Lewis Kelly
date_published: 2025-01-19
date_ingested: 2026-04-06
original_file: raw/Lewis Kelly/19-01-2025 - How To Accurately Read Market Structure (Step by Step).txt
asset: multi
tags: [market-structure, confluence, refinement, entry-model]
---

## Summary
LK presents a 5-step blueprint for going from HTF directional bias to precise LTF entry. The video's most valuable contribution is "The Refinement" — a LTF analysis technique for resolving the problem of multiple valid confluences (OBs or FVGs) sitting in the same discount zone. The Refinement solves the "which one?" question by dropping to LTF and watching which level actually attracts a CHoCH.

## Key Points
- 5-step process: structure → premium/discount → liquidity sweep → demand zone (OB/FVG) → LTF confirmation.
- Premium/discount 50% line is the primary quality filter for entry zones.
- OTE (Optimal Trade Entry) reinforced: 0.618–0.786 Fibonacci zone within the discount is the highest-probability entry area.
- "The Refinement" = when two confluences are in the same discount zone, drop to LTF and watch which one produces a CHoCH. That is the real level. Entering before confirmation leads to half-price stops or split risk.
- The Refinement prevents guessing between multiple valid-looking OBs/FVGs.

## Trading Rules / Setups Described
1. **Step 1 — Structure:** Identify HTF (4H or Daily) directional bias using external structure HH/HL or LH/LL.
2. **Step 2 — Premium/Discount:** Map the current structural leg (swing low to swing high). Identify the 50% equilibrium line.
3. **Step 3 — Liquidity Sweep:** Wait for price to enter the discount zone by sweeping a liquidity level (equal lows, session low). This confirms manipulation is occurring before the real move.
4. **Step 4 — Demand Zone:** Identify OB or FVG within the discount. If multiple valid zones exist at similar price levels, do not guess — apply The Refinement.
5. **Step 5 — LTF Confirmation (The Refinement):** Drop to 1m or 5m. Observe which zone produces a CHoCH (shift from bearish to bullish 1m structure). The zone that produces the CHoCH is the real level. The other zones were fake-outs or will be skipped.
6. **Entry:** After CHoCH, enter long from the confirmed zone. Stop below CHoCH swing low.
7. **Target:** Next structural high (swing high of HTF leg).

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/market-structure|Market Structure]], [[concepts/premium-discount|Premium/Discount]], [[concepts/liquidity|Liquidity]], [[concepts/order-blocks|Order Blocks]], [[concepts/fair-value-gap|Fair Value Gap]], [[concepts/break-of-structure|BOS/CHoCH]]

## Strategies Mentioned
_(no distinct named strategy — folds into general intraday framework)_

## Contradictions / Open Questions
- The Refinement is a powerful concept for bot implementation: instead of entering all qualifying OBs in a zone, the bot should wait for LTF CHoCH confirmation to identify the specific level that holds.
- "Multiple OBs in same zone" problem is not resolved by static rules — requires live LTF observation. This creates a dependency on real-time LTF analysis in the bot.

## Quotes
> "This is what I call the refinement. You drop to the lower time frame. And you don't guess which OB it is — you let the market tell you. The one that shifts structure is the one that matters."
