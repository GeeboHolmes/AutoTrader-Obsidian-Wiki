---
type: concept
concept_type: smc
aliases: [BOS, CHoCH, Change of Character, Break of Structure, MSS, Market Structure Shift]
tags: [smc, structure, trend-change]
---

## Definition
**Break of Structure (BOS):** A continuation signal. Price breaks through the last significant swing point *in the direction of the existing trend*, confirming the trend is continuing. BOS = trend continuation.

**Change of Character (CHoCH):** A reversal signal. Price breaks through the last significant swing point *against the existing trend*, suggesting a potential trend reversal. CHoCH = possible trend reversal.

Also called **Market Structure Shift (MSS)** in some SMC frameworks — same concept, different label.

## How It Works

### BOS (Bullish Example)
In a bullish market (HH, HL sequence), a BOS occurs when price breaks above the previous HH. This confirms the uptrend continues. Traders look for long entries on the subsequent pullback to a discount zone.

### CHoCH (Bullish-to-Bearish)
In a bullish market, a CHoCH occurs when price breaks *below* the previous HL. This is the first sign the trend may be reversing. The higher the timeframe, the more significant the CHoCH.

### BOS vs CHoCH Decision Tree
```
Current structure: BULLISH
  Price breaks above last HH → BOS (continuation)
  Price breaks below last HL → CHoCH (potential reversal)

Current structure: BEARISH
  Price breaks below last LL → BOS (continuation)
  Price breaks above last LH → CHoCH (potential reversal)
```

## Identification Rules
1. Identify the current trend using [[concepts/market-structure|Market Structure]].
2. Mark the most recent relevant swing high and swing low.
3. **BOS:** Candle close (or body close, depending on rule set) beyond the swing point in the trend direction.
4. **CHoCH:** Candle close beyond the swing point *against* the trend direction.
5. Only use **confirmed** closes — avoid wicks-only sweeps (these are [[concepts/liquidity|liquidity grabs]], not BOS/CHoCH).

> **Contested rule:** Some educators count wick breaks; others require candle body or full candle close. Document which rule your strategy uses and stick to it consistently.

## Trading Application
- **BOS** = do not trade against the trend; look for continuation entries at the next pullback.
- **CHoCH** = trend may be changing; wait for confirmation before reversing bias; watch for a new structure to form.
- A CHoCH followed by a BOS in the new direction = strong trend reversal confirmation.
- HTF CHoCH is a major event — can signal multi-day or multi-week reversals.

## Timeframe Considerations
- HTF CHoCH: very significant — signals macro trend change.
- LTF CHoCH: common and less reliable — use only for entry precision, not macro bias.
- Always label your BOS/CHoCH with the timeframe it occurred on.

## Python Implementation Notes
- Requires the swing point list from [[concepts/market-structure|Market Structure]] detection.
- BOS detection: `if current_close > last_swing_high and bias == 'bullish': record_bos()`
- CHoCH detection: `if current_close < last_swing_low and bias == 'bullish': record_choch()`
- Decision: candle close vs. wick. Implement a `confirmation_type` parameter: `'close'` or `'wick'`.
- Store BOS/CHoCH events as: `(timestamp, type, price_level, timeframe)`.
- After a CHoCH, reset the structure bias state and begin tracking new swing points.

## Connections
- Related concepts: [[concepts/market-structure]], [[concepts/liquidity]], [[concepts/swing-points]]
- Used in strategies: _(to be populated)_
- Part of: [[concepts/smart-money-concepts]]

## True Market Reversal — Three Phases (LK)
Source: [[sources/lk-ms-complete-guide-2024]]

A CHoCH alone is a *potential* reversal signal, not a confirmed one. True reversal requires three sequential phases:

| Phase | Event | Significance |
|-------|-------|-------------|
| 1 | **CHoCH** — price breaks the structural swing point against the trend | Potential reversal: "trend may be changing" |
| 2 | **Pullback** — price creates HL (if bullish CHoCH) or LH (if bearish CHoCH) | Trend is "testing" the new direction |
| 3 | **BOS in new direction** — price breaks structure confirming the new trend | Confirmed reversal: bias fully shifts |

**Before Phase 3:** Bias is provisional. A CHoCH can fail (price CHoCHs bullish then immediately CHoCHs back bearish). Trading Phase 1 directly exposes to fakeout stops.

**Trading options:**
- **Aggressive (Phase 1):** Enter on CHoCH itself from OB/FVG at the CHoCH level. Stop below CHoCH point. Highest RR but lowest confirmation.
- **Standard (Phase 2):** Enter on the HL/LH formation. Stop below the HL. Good balance of RR and confirmation.
- **Conservative (Phase 3):** Wait for BOS in new direction, then enter on pullback. Lowest RR but highest confirmation rate. Recommended for newer traders.

**LK rule:** "Maybe stop chasing the change of character and wait for the additional confirmation. It requires more patience but has a higher success rate."

**Python:**
```python
class ReversalPhase(Enum):
    NONE = 0
    CHOCH = 1       # potential reversal
    PULLBACK = 2    # HL/LH formed after CHoCH
    CONFIRMED = 3   # BOS in new direction

def get_reversal_phase(choch_detected, hl_lh_formed, bos_in_new_dir):
    if bos_in_new_dir:
        return ReversalPhase.CONFIRMED
    elif hl_lh_formed:
        return ReversalPhase.PULLBACK
    elif choch_detected:
        return ReversalPhase.CHOCH
    return ReversalPhase.NONE
```

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
| 2026-04-06 | [[sources/lk-ms-complete-guide-2024]] | Added true market reversal 3-phase model; trading options by confirmation level; Python enum |
