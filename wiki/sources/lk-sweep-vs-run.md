---
type: source
title: "The Reason Liquidity Sweeps Keep Failing You (Full Guide)"
source_type: video-transcript
author: "Lewis Kelly"
date_published: 2025-04-21
date_ingested: 2026-04-06
original_file: raw/Lewis Kelly/21-04-2025 - The Reason Liquidity Sweeps Keep Failing You (Full Guide).txt
asset: multi
tags: [smc, liquidity, sweep-vs-run, context-stacking, weak-high, auction-theory]
---

## Summary
A deep conceptual piece explaining the fundamental difference between a liquidity sweep and a liquidity run — two mechanisms that look similar but have opposite implications. Most SMC traders fail because they can't distinguish them. LK also introduces "context stacking" for valid sweeps: combining weak high + Asia session high + equal highs creates near-certain reversal probability. Includes live trade example using stacked context.

## Key Points
- **Liquidity Sweep:** price wicks ABOVE a high but CLOSES BACK INSIDE the range → institutional sell entry; expect reversal down
- **Liquidity Run:** price aggressively breaks ABOVE a high AND continues with momentum → institutions using the liquidity as FUEL to push higher; NOT a reversal
- Key distinction: where the candle CLOSES relative to the swept level
- **Context stacking for valid sweeps** (probability increases with each layer):
  1. Directional bias (be bearish to trade a sweep of a high)
  2. **Weak high**: the last swing high that failed to break the corresponding low — it has an unfulfilled "responsibility" to take out that low; it's liquidity because sellers using it as a stop will get swept
  3. **Asia session high**: additional liquidity concentration
  4. **Equal highs**: multiple touches at same level = concentrated stop-losses
  - Stack 3+ types = near-certainty of sweep reversal
- Liquidity explanation: 3 ways liquidity enters market — position opens, take-profit hits, stop-loss triggered
- Active vs passive market participants; institutions must build positions gradually in range phases to avoid moving the market

## Trading Rules / Setups Described
1. **Sweep vs Run Identification:**
   - Sweep: price crosses high → candle CLOSES BELOW the high → bearish reversal signal
   - Run: price crosses high → candle CLOSES ABOVE the high → continuation signal; NOT a sweep
2. **Context Stacking for Valid Sweeps:**
   - Only trade sweeps aligned with your directional bias (don't short a sweep of a low if bias is bullish)
   - Count context layers: weak high + Asia high + equal highs = 3 layers = very high confidence
   - Minimum 2 layers for a valid setup; 3+ = A+ sweep entry
3. **Weak High Definition:** the swing high whose "responsibility" was to take out the prior low; it failed to do so. The prior low remained intact. This makes the high "weak" — it will likely be swept before price continues lower.
4. **Entry after stacked sweep:** after the sweep is confirmed (candle closes back inside range), drop to 1m for CHoCH → enter at OB/FVG in direction of bias.

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/liquidity|Liquidity]], [[concepts/market-structure|Market Structure]], [[concepts/order-blocks|Order Blocks]], [[concepts/break-of-structure|BOS / CHoCH]]

## Strategies Mentioned
[[strategies/lk-retail-trap-reversal|LK Retail Trap Reversal]], [[strategies/lk-advanced-smc-funded|LK Advanced SMC (Funded)]]

## Contradictions / Open Questions
- Sweep vs run distinction is CANDLE CLOSE based — important implementation note: need to check if price closed above/below the swept level, not just whether the wick crossed it
- "Weak high" definition: the swing high that failed to take out the corresponding low. This is a structural property — needs a lookback algorithm to identify
- Context stacking threshold: when does "3 types" become sufficient vs insufficient? Data-based threshold not given

## Quotes
> "A liquidity sweep is when price aggressively runs a high, wicks above it, and then closes inside of it. But the why behind it is what's most important."

> "A liquidity run is quite literally the opposite. A liquidity run is when price aggressively trades above a high and then also continues with that same level of momentum."
