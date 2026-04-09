---
type: concept
concept_type: smc
aliases: [Swing High, Swing Low, Pivot, Swing Point, SH, SL]
tags: [smc, foundational, structure]
---

## Definition
Swing points are the local maxima and minima of price — the "pivots" or "turns" in price action. They are the foundation of all [[concepts/market-structure|Market Structure]] analysis. Every higher-level SMC concept (BOS, CHoCH, OBs, liquidity levels) is built on top of swing point identification.

## Types
- **Swing High (SH):** A candle whose high is higher than the highs of N candles on each side.
- **Swing Low (SL):** A candle whose low is lower than the lows of N candles on each side.
- **Significant swing:** A swing point that plays a structural role (creates a BOS/CHoCH when broken).
- **Minor swing:** A smaller pivot within a leg — used for LTF entries, not HTF structure.

## Identification Rules
1. **Simple definition:** `is_swing_high = candle[i].high > candle[i-N].high and candle[i].high > candle[i+N].high` for N candles on each side.
2. The value of N determines sensitivity:
   - Small N (e.g. 2–3): many swing points, noisy, good for LTF.
   - Large N (e.g. 5–10): fewer swing points, cleaner structure, good for HTF.
3. Only **confirmed** swing points can be labelled — the right side of the swing must be formed (future candles must exist).
4. Real-time trading note: at the right edge of the chart, the last swing point cannot be confirmed yet.

## Python Implementation Notes
- `scipy.signal.argrelextrema(prices, np.greater, order=N)` for swing highs.
- `scipy.signal.argrelextrema(prices, np.less, order=N)` for swing lows.
- Alternative: rolling window comparison — more controllable for real-time use.
- **Real-time caveat:** In live trading, swing points cannot be confirmed until N candles have formed to the right. Build a "pending" and "confirmed" state.
- Parametrize N and expose it as a strategy parameter for backtesting.

## Connections
- Related concepts: [[concepts/market-structure]], [[concepts/break-of-structure]], [[concepts/liquidity]]
- Part of: [[concepts/smart-money-concepts]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
