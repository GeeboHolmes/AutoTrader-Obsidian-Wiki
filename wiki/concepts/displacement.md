---
type: concept
concept_type: smc
aliases: [Displacement, Impulse, Expansion, Impulsive Move]
tags: [smc, confirmation, institutional]
---

## Definition
Displacement is a strong, rapid, high-momentum price move — typically 3 or more large candles in one direction with minimal retracement — that indicates institutional order flow is actively pushing price. It is a key confirmation that smart money is involved in a move, and it typically leaves behind [[concepts/order-blocks|Order Blocks]] and [[concepts/fair-value-gap|Fair Value Gaps]].

## How It Works
When institutions deliver orders aggressively, they do not "nibble" — they push price decisively. Displacement is the visual footprint of this aggression. It:
1. Creates inefficiencies ([[concepts/fair-value-gap|FVGs]]) because price moves faster than the market can fill.
2. Breaks structural points ([[concepts/break-of-structure|BOS/CHoCH]]) cleanly.
3. Originates from an area that becomes significant on the return ([[concepts/order-blocks|OB]]).

Without displacement, a BOS may be a fake-out. With displacement, a BOS is more credible.

## Identification Rules
1. **Multiple large candles** in one direction — typically 3+ candles, each closing near its high (bullish) or low (bearish).
2. **Candle size:** Each candle should be significantly larger than the recent ATR (e.g. > 1.5× ATR).
3. **Minimal retracement** between candles — wicks are small relative to bodies.
4. **FVG left behind:** The displacement should create at least one visible FVG.
5. **BOS:** The displacement should break through a notable swing point.
6. **Volume** (if available): displacement should coincide with elevated volume.

## Trading Application
- **Displacement = confirmation of directional intent.** After a displacement, the bias is set until a [[concepts/break-of-structure|CHoCH]] occurs.
- **Entry:** Do not chase displacement. Wait for price to retrace to the originating [[concepts/order-blocks|OB]] or into the [[concepts/fair-value-gap|FVG]] left behind.
- **No displacement = no trade:** If a BOS occurs without displacement (slow grind), treat it with skepticism — it may be a fake-out or retail-driven move.

## Timeframe Considerations
- On HTF (4H, Daily): displacement is a multi-day/week directional signal.
- On LTF (5m, 15m): displacement is an intraday directional signal.
- Always check if LTF displacement aligns with HTF bias.

## Python Implementation Notes
- Measure candle body size: `body = abs(close - open)`
- Measure recent ATR: `atr = average_true_range(candles, period=14)`
- Displacement candidate: `body > 1.5 * atr and close near extreme (close > open + 0.8*body for bullish)`
- Displacement run: find N consecutive displacement-candidate candles (N ≥ 2 or 3, parametrize).
- Store displacement events: `{start_timestamp, end_timestamp, direction, candle_count, fvgs_created, bos_broken}`

## Connections
- Related concepts: [[concepts/order-blocks]], [[concepts/fair-value-gap]], [[concepts/break-of-structure]], [[concepts/liquidity]]
- Part of: [[concepts/smart-money-concepts]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
