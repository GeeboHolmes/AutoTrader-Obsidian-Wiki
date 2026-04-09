---
type: concept
concept_type: smc
aliases: [FVG, Imbalance, IFVG, Inverse FVG, Gap, Inefficiency]
tags: [smc, entry, imbalance, institutional]
---

## Definition
A Fair Value Gap (FVG) is a three-candle price imbalance where the wicks of candle 1 and candle 3 do not overlap, leaving an inefficiency in price delivery. The gap represents an area where price moved so quickly (due to institutional order flow) that it was not efficiently traded — price is likely to return to fill it.

## How It Works
Markets tend toward efficiency. When price moves impulsively, it skips over price levels without two-sided trading occurring. The resulting "gap" between candle 1's wick and candle 3's wick is an inefficiency. Smart money will use pullbacks to fill these gaps as it:
1. Provides liquidity for late entries.
2. Fills any remaining institutional orders placed in that zone.

**Bullish FVG:** Candle 1 high < Candle 3 low — a gap above in a bullish impulse.
**Bearish FVG:** Candle 1 low > Candle 3 high — a gap below in a bearish impulse.

## Identification Rules
1. Three consecutive candles.
2. **Bullish FVG:** `candle[i-2].high < candle[i].low` — gap between wick of 2 candles ago and current candle's low.
3. **Bearish FVG:** `candle[i-2].low > candle[i].high` — gap between wick of 2 candles ago and current candle's high.
4. **Significance filters:**
   - The middle candle (candle 2) should be large relative to recent ATR.
   - The FVG should be in the direction of the HTF trend.
   - An FVG inside or adjacent to an [[concepts/order-blocks|Order Block]] is higher probability.
5. An FVG is **filled** when price trades back through the entire gap. A *partial fill* (to 50% or equilibrium of the gap) often provides a reaction.
6. **Inverse FVG (IFVG):** An FVG that has been fully filled and then price continues through — the FVG flips polarity and becomes a resistance (if bullish FVG) or support (if bearish FVG).

## Trading Application
- **Entry zone:** Enter long at a bullish FVG during a pullback (when price returns to fill the gap). Enter short at a bearish FVG.
- **50% rule:** Many traders enter at the midpoint of the FVG rather than the bottom — tighter stop, same target.
- **Confluence:** FVG + OB alignment in the same zone = very high-probability entry.
- **Target:** Price tends to fill FVGs fully before continuing — so a gap above is a reasonable take-profit target.
- **Do not force:** Not every FVG gets filled. High-momentum moves may not retrace. Only trade FVGs in the context of HTF bias and a clear retracement.

## Timeframe Considerations
- HTF FVGs (4H, Daily): act as major support/resistance; high-significance targets.
- Intraday FVGs (1H, 15m): commonly used for entries.
- LTF FVGs (5m, 1m): entry precision within a larger zone.

## Asset Considerations
- Crypto: FVGs are extremely common due to volatile, gappy markets — high fill rate.
- Forex: less common than in crypto but highly significant when they appear (especially during news).
- Gold: common during large news candles; treat with caution near economic releases.

## Python Implementation Notes
- Detection algorithm:
```python
def detect_fvg(candles):
    fvgs = []
    for i in range(2, len(candles)):
        c1, c2, c3 = candles[i-2], candles[i-1], candles[i]
        if c1['high'] < c3['low']:  # Bullish FVG
            fvgs.append({'type': 'bullish', 'top': c3['low'], 'bottom': c1['high'],
                         'timestamp': c2['timestamp'], 'filled': False})
        elif c1['low'] > c3['high']:  # Bearish FVG
            fvgs.append({'type': 'bearish', 'top': c1['low'], 'bottom': c3['high'],
                         'timestamp': c2['timestamp'], 'filled': False})
    return fvgs
```
- Fill tracking: `if candle_low <= fvg_bottom (for bullish FVG): mark_fvg_filled()`
- Partial fill: track `fill_percentage = (fvg_top - current_low) / (fvg_top - fvg_bottom)`
- FVG data structure: `{type, top, bottom, timestamp, timeframe, filled: bool, fill_pct: float, ifvg: bool}`
- Filter by size: `fvg_size = fvg_top - fvg_bottom; if fvg_size < min_size_threshold: discard`

## Connections
- Related concepts: [[concepts/order-blocks]], [[concepts/displacement]], [[concepts/premium-discount]]
- Part of: [[concepts/smart-money-concepts]]
- Used by educator: [[entities/ict]]

## Lewis Kelly: FVG as OB Qualifier and Pullback Target
Source: [[sources/lk-ob-masterclass]], [[sources/lk-smc-full-2023]], [[sources/lk-10k-month]]

**FVG as OB validator (CRITICAL):** LK requires a FVG to confirm that an OB is real. The rule: the OB candle (sell-to-buy or buy-to-sell) must leave an imbalance — candle 1's high must NOT touch candle 3's low. If they touch, there is no FVG, and therefore no valid OB.
- `ob_valid = candle1.high < candle3.low` (bullish OB scenario)

**FVG as pullback zone target:** When a bearish 4H move creates a large FVG, price will almost certainly return to rebalance it before continuing lower. The FVG becomes a primary take-profit target for the counter-trend pullback trade — and a supply zone entry for the continuation trade.

**Fibonacci + FVG confluence:** LK uses Fibonacci (50–78.6%) to predict the pullback depth. When the FVG overlaps with the 61.8–78.6% Fibonacci zone from the BOS leg, this is the highest-confluence entry area.

**News event FVGs:** High-impact news (NFP) creates large FVGs due to sudden impulsive moves. These FVGs are priority targets for rebalancing. The move post-news reveals institutional intent; the FVG is where price will return first before continuing.
- Rule: after a large news move, the FVG created by that move will be filled before price continues in its news-revealed direction.

## Volume Profile Validation and Three-Rule FVG Framework (LK — October 2025)
Source: [[sources/lk-fvg-only-video]]

### Why FVGs Exist: Market Maker Mechanics
FVGs are not random price gaps. They are created when market makers legally withdraw their liquidity — most commonly during high-impact news events. When market makers step away, there is no counterparty to absorb aggressive order flow, and price moves rapidly through a zone with near-zero transaction volume. The resulting gap zone = the FVG.

This means a genuine FVG has a clear mechanical cause: institutional liquidity withdrawal. A "gap" with normal volume distribution is not a real FVG — it is a coincidental price move.

### Volume Profile Validation Rule
Before trading an FVG, validate it with a volume profile:
- A real FVG = **low-volume node (LVN)** in the volume profile. The gap zone will show significantly below-average transaction volume.
- If the gap zone shows normal or high volume → not a genuine FVG → skip.
- This prevents trading every three-candle gap regardless of institutional significance.

### Three-Rule FVG Framework
Three rules must ALL be satisfied for a high-probability FVG trade:

| Rule | Requirement |
|------|-------------|
| 1 — Volume Inefficiency | FVG zone is a genuine LVN confirmed by volume profile |
| 2 — Trend Alignment | FVG is in the direction of external structure trend |
| 3 — Liquidity Context | A liquidity level was swept into the FVG AND there is a clear liquidity target ahead |

Rule 3 in detail: the FVG must have been created in context of a liquidity sweep (price was pushed into the FVG zone to collect resting stop-losses), AND the post-FVG trade direction must have a clear target (equal highs/lows, swing high, session level). Without a liquidity target, the trade has no logical endpoint.

### Entry Model (LK — FVG Specific)
1. Price retraces into the FVG zone.
2. Drop to 5m. Wait for **5m CHoCH** in trade direction inside the FVG zone.
3. Enter after 5m CHoCH. Stop below FVG low (bullish FVG).
4. Target: next liquidity level in trend direction.

> **Note on confirmation timeframe:** This source specifies 5m CHoCH for FVG entries. Other LK sources specify 1m CHoCH for OB entries. Both are valid — use timeframe appropriate to the zone size and trade duration.

**Python:**
```python
def validate_fvg_entry(fvg, volume_profile, market_context):
    """Three-rule FVG validation."""
    # Rule 1: Volume inefficiency
    fvg_volume = volume_profile.get_volume_in_range(fvg.bottom, fvg.top)
    avg_volume = volume_profile.get_average_node_volume()
    is_low_volume = fvg_volume < avg_volume * 0.5  # below 50% of average = LVN
    
    # Rule 2: Trend alignment
    trend_aligned = fvg.direction == market_context.trend
    
    # Rule 3: Liquidity context
    liquidity_swept_into_fvg = market_context.liquidity_swept_before_fvg
    clear_liquidity_target = market_context.next_liquidity_target is not None
    liquidity_context = liquidity_swept_into_fvg and clear_liquidity_target
    
    return is_low_volume and trend_aligned and liquidity_context
```

## FVG as Target vs Entry: Contradiction
Source: [[sources/lk-5-advice-2024]] vs [[sources/lk-fvg-only-video]]

**June 2024 (Q&A):** LK explicitly states he does NOT use FVGs as points of interest or entry zones. He uses them only as targets — "liquidity voids" that price may return to fill. OBs are his entry tool.

**October 2025 (FVG Video):** LK describes a full FVG entry model with 5m CHoCH confirmation inside the FVG zone (see Three-Rule FVG Framework section above).

**Resolution:** This appears to be an evolution in LK's approach. By October 2025, FVGs had been elevated from target-only to a full entry zone with its own validation framework. For bot implementation: use the later (2025) framework as the current standard, but note that LK's primary entry tool remains the OB, and FVG entries require the three-rule validation (volume, trend, liquidity context) to qualify.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
| 2026-04-06 | [[sources/lk-ob-masterclass]], [[sources/lk-smc-full-2023]], [[sources/lk-10k-month]], [[sources/lk-advanced-trade-breakdown]] | Added LK FVG as OB qualifier rule, Fib+FVG confluence, news event FVGs |
| 2026-04-06 | [[sources/lk-fvg-only-video]] | Added market maker mechanics, volume profile LVN validation, three-rule FVG framework, 5m CHoCH entry model |
| 2026-04-06 | [[sources/lk-5-advice-2024]] | Added FVG contradiction note: June 2024 states FVGs = targets only; October 2025 adds full FVG entry model. Resolution: 2025 framework is current standard |
