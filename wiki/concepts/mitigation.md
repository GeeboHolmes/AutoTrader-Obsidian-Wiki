---
type: concept
concept_type: smc
aliases: [Mitigation, Mitigated OB, Mitigated Block]
tags: [smc, order-blocks, zones]
---

## Definition
Mitigation refers to the process by which an [[concepts/order-blocks|Order Block]] or other institutional zone is visited by price and the remaining institutional orders are filled. Once fully mitigated, the zone loses its significance as a future entry point.

## How It Works
When an OB forms, only a portion of institutional orders are filled on the original move. The remaining (unfilled) orders sit in the OB zone. When price returns:
1. Institutional orders are filled (mitigated).
2. Price reacts from the zone and continues.
3. If the reaction is strong and sustained, the OB is considered **partially mitigated** (still valid for re-entry).
4. If price closes *through* the entire OB zone, the OB is **fully mitigated** — it is now invalidated as an entry zone.

A fully mitigated bullish OB that price then reverses away from *above* becomes a **Breaker Block** — it flips from support to resistance.

## Mitigation Rules
1. **Partial mitigation:** Price touches the OB zone (entering the range) but does not close through the opposite extreme.
   - Zone remains valid.
   - Second touch entries are riskier — use tighter criteria.
2. **Full mitigation:** Price closes a candle body beyond the zone's opposite extreme.
   - OB is invalidated. Remove from active zone list.
3. **Breaker block formation:** After full mitigation, if price reverses *back into* the former OB and then rejects, the OB has "broken" and flipped.
   - A former bullish OB that is mitigated and then resists price on return = bearish Breaker Block.

## Trading Application
- Track mitigation status in real time — only trade **unmitigated** OBs.
- A partially mitigated OB can still be traded but with reduced confidence — reduce position size or require additional confirmation.
- A Breaker Block is itself an entry zone — trade in the opposite direction of the original OB.
- **Mitigation + [[concepts/fair-value-gap|FVG]] fill simultaneously:** Very common — OB and FVG often overlap; when both are filled at the same time, the area is fully cleared.

## Python Implementation Notes
- Maintain a status field on each OB: `{'unmitigated', 'partial', 'mitigated', 'breaker'}`
- On each new candle:
  - Check if `candle_low < ob_high` (for bullish OB) → price has entered the zone.
  - If `candle_close < ob_low` → fully mitigated; update status.
  - If partial touch and reversal → update to `partial`.
  - Breaker detection: after `mitigated`, if price returns above `ob_high` and then closes back below → status = `breaker`.

## Connections
- Related concepts: [[concepts/order-blocks]], [[concepts/fair-value-gap]], [[concepts/liquidity]]
- Part of: [[concepts/smart-money-concepts]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
