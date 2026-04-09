---
type: concept
concept_type: smc
aliases: [IDM, Inducement, Manipulation Phase]
tags: [smc, manipulation, liquidity]
---

## Definition
Inducement (IDM) is a deliberate, engineered price move designed to lure retail traders into a position before the true directional move occurs. It is a form of manipulation — smart money induces retail traders to trade in the *wrong* direction, building liquidity for their own entry.

Inducement is the mechanism *before* a liquidity sweep. Understanding it allows a trader to anticipate the sweep rather than react to it.

## How It Works
**Classic bullish inducement sequence:**
1. Price is in a downtrend (bearish structure).
2. A small rally forms — retail traders see "support holding" and go long.
3. These longs create **buy stop liquidity** above the small rally high AND **stop losses** below the small rally low.
4. Smart money drives price *down* through the rally low, triggering stop losses ([[concepts/liquidity|SSL sweep]]).
5. Smart money simultaneously accumulates longs at the swept level.
6. Price then reverses aggressively higher — [[concepts/displacement|displacement]] — as the true direction unfolds.

The small rally was the **inducement** — it was engineered to provide liquidity for the institutional buy.

## Identification Rules
1. A counter-trend move that forms an obvious swing point (a "trap").
2. The swing point should attract retail positions — it must look like a valid S/R level or pattern (double bottom, trendline touch, etc.).
3. The move following the swing point is typically slow, grinding, or overlapping — retail-driven, not institutional.
4. After the inducement, a sharp sweep of the IDM level (wick or close) followed by immediate reversal.
5. The sweep should coincide with a nearby [[concepts/order-blocks|OB]] or [[concepts/fair-value-gap|FVG]].

## Trading Application
- **Do not trade the inducement move** — it is the trap.
- **Wait for the sweep:** After identifying an IDM, expect a sweep of its extreme level.
- **Entry after sweep:** The sweep + reversal (confirmed by LTF [[concepts/break-of-structure|CHoCH]]) is the entry signal.
- Inducement + sweep + OB/FVG confluence = very high-probability setup.
- **Pattern:** IDM → sweep of IDM level → CHoCH → entry at OB/FVG in discount.

## Timeframe Considerations
- HTF inducement: can be a multi-day fake-out before a major trend continuation.
- LTF inducement: intraday manipulation before a session move.

## Python Implementation Notes
- Inducement is inherently contextual — difficult to detect algorithmically without first knowing the bias.
- Approach: detect counter-trend swings that are smaller than the prior impulse (measured by candle count or price range).
- IDM candidate: `swing that retraces < 38.2% of the prior impulse, forms a clean swing point, then reverses`
- Flag for review: when a swing point is swept sharply and price reverses within N candles, backtrack and label the swing as IDM.

## Connections
- Related concepts: [[concepts/liquidity]], [[concepts/order-blocks]], [[concepts/break-of-structure]], [[concepts/displacement]]
- Part of: [[concepts/smart-money-concepts]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
