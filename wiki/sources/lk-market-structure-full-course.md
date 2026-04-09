---
type: source
title: "The Ultimate Market Structure Trading Strategy | SMC (FULL COURSE)"
source_type: video-transcript
author: Lewis Kelly
date_published: 2024-05-19
date_ingested: 2026-04-05
original_file: raw/Lewis Kelly/19-05-2024 - The Ultimate Market Structure Trading Strategy SMC (FULL COURSE).txt
asset: multi
tags: [market-structure, smc, lewis-kelly, full-course]
---

## Summary
Five-part deep dive into market structure from textbook basics to realistic application. The video's core contribution is the explicit distinction between internal and external structure (the single biggest mistake LK sees traders make), the valid pullback rule (three consecutive candles), and the fractal multi-timeframe confirmation model. Also covers how to identify the correct swing low after a BOS and why wick breaks are not structure breaks.

## Key Points
- **External vs internal structure:** The range from last swing low to last swing high defines the "external range." Everything inside is internal and should not be traded as structure. Many traders get stopped out trading internal structure that is merely noise inside a larger move.
- **Valid pullback rule:** A confirmed structural low (for bullish) requires **3 consecutive bearish candles closing below each other**. 1–2 candles = fake pullback = still the same structural low.
- **Correct swing low identification:** After a BOS to the upside, the structural low is the LOWEST POINT price retraced from the swing high BEFORE it broke structure — not any random internal low.
- **Wick rule:** A wick below a swing low is a liquidation/manipulation, NOT a structural break. Price must close (candle body) beyond the swing point for a valid BOS/CHoCH.
- **Fractal model:** Every 15m swing point has a 1m structure shift at its base. When the 15m is about to reverse (HL forming), the 1m will show a CHoCH first — this is the entry signal.
- **15m + 1m framework:** 15m gives direction and OB zone; 1m gives the precise entry confirmation trigger.

## Trading Rules / Setups Described
1. Identify external swing range on 15m (true swing high and swing low).
2. Anything inside that range = internal structure. Ignore BOS/CHoCH signals inside the range.
3. Wait for external BOS or external CHoCH.
4. After BOS, identify the correct structural low (lowest point of the pullback that led to the BOS).
5. Expect price to pull back into the demand zone (OB) within the structural range.
6. Require valid pullback: 3+ consecutive candles in opposite direction.
7. On 1m, watch for CHoCH → entry trigger at the 1m demand zone/FVG.
8. Stop: Below the structural low. Target: Next structural high (BSL).

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/market-structure]], [[concepts/break-of-structure]], [[concepts/swing-points]], [[concepts/order-blocks]], [[concepts/liquidity]]

## Contradictions / Open Questions
- "3 candles" valid pullback rule is a Lewis Kelly-specific rule. ICT does not use this specific rule. Worth testing quantitatively: does 3-candle pullback filter improve win rate?
- Wick = liquidation (not structure): LK is consistent here; confirms this is not a break. Important for bot implementation — must use candle close, not wick.

## Quotes
> "Three candles that succeed each other in the opposite direction... that is a confirmed pullback."

> "Never consider a wick as a break. It isn't. A wick is simply a liquidation."

> "The correct swing low is the lowest point that price reversed from the high that leads to a break of structure."
