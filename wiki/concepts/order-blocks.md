---
type: concept
concept_type: smc
aliases: [OB, Order Block, Bullish OB, Bearish OB, BOB, Breaker Block]
tags: [smc, entry, supply-demand, institutional]
---

## Definition
An Order Block (OB) is the last candle (or group of candles) *before* a significant impulsive move, representing where institutional orders were placed. When price returns to an OB zone, institutions fill remaining orders, often causing a reaction. Order Blocks are the primary entry zones in SMC trading.

## Types

| Type | Formation | Bias |
|------|-----------|------|
| **Bullish OB** | Last bearish (down) candle before a strong bullish impulse | Long entry zone |
| **Bearish OB** | Last bullish (up) candle before a strong bearish impulse | Short entry zone |
| **Breaker Block** | A failed OB — price passes through it; the OB becomes support→resistance or resistance→support | Reversal zone after mitigation |

## How It Works
When institutions accumulate a large position, they can't fill it all at once without moving price against themselves. They place orders in layers. The last opposing candle before the impulse represents where the final cluster of orders was placed. When price retraces to that zone, those orders get filled — providing the reaction traders see.

**Bullish OB Logic:**
- Institutions accumulating longs will have unmitigated buy orders at the last bearish candle before the markup.
- Price returns to the OB → orders are filled → price continues bullish.

## Identification Rules
1. Identify a strong impulsive move (a [[concepts/displacement|Displacement]] move — multiple large candles in one direction with little retracement).
2. **Bullish OB:** The last *bearish* candle immediately before the impulsive bullish move. Use the open and close of that candle to define the zone (some use high/low).
3. **Bearish OB:** The last *bullish* candle immediately before the impulsive bearish move.
4. The impulse following the OB should:
   - Leave a [[concepts/fair-value-gap|Fair Value Gap]] (strong confirmation).
   - Break structural points ([[concepts/break-of-structure|BOS/CHoCH]]).
5. An OB is **mitigated** (used up) when price fully overlaps and closes through its range. A mitigated OB loses its significance.
6. An OB that price passes through and then reverses away from becomes a **Breaker Block** (polarity flip).

> **Contested definition:** Some educators (ICT) use the *high/low* of the OB candle; others use the *open/close* (body). The body approach creates a tighter zone and is more commonly used for entries. Document which your strategy uses.

## Trading Application
- **Entry:** Wait for price to retrace to the OB zone. Look for a lower timeframe entry confirmation ([[concepts/break-of-structure|LTF CHoCH]], rejection candle, etc.).
- **Stop loss:** Below the OB low (for bullish) or above the OB high (for bearish) — plus a small buffer.
- **Target:** Next liquidity level ([[concepts/liquidity|BSL/SSL]]), previous swing high/low, or [[concepts/fair-value-gap|FVG]] fill.
- **Confirmation stack:** OB + FVG alignment + [[concepts/premium-discount|Discount zone]] + HTF bias alignment = highest probability setup.
- **Do not enter** on first touch of a fresh OB without LTF confirmation — price may wick through before reacting.

## Timeframe Considerations
- HTF OBs (4H, Daily): high significance; price may travel far to reach them; used as major supply/demand zones.
- LTF OBs (15m, 5m, 1m): used for entry precision within the context of an HTF OB.
- Nested approach: HTF OB defines the zone → LTF OB within the HTF OB provides the precise entry.

## Winning OB Types (LK — 800 Trades, 5 Years Data)
Source: [[sources/lk-secret-ob-types]]

From 800 trades / 5 years / 1,250 days, LK identified two OB types with consistently high success rates:

**Type 1 — Sweep Shift OB:**
- Price is trending (e.g. bullish HH/HL).
- Price sweeps a swing high (BSL run), then aggressively reverses (CHoCH).
- The OB is the **last bullish candle immediately before the sell-off** at the swept level.
- When price returns to this OB, it is highly likely to hold.
- Entry: short from OB high; stop above OB; target next structural low.

**Type 2 — Continuation Trap OB:**
- In a larger trending move (e.g. external structure bearish), a pullback creates internal structure.
- Within the pullback's internal bearish structure, an internal CHoCH occurs to the bullish side.
- The OB that caused this internal CHoCH (especially if just below Asia session low) is the Continuation Trap OB.
- Entry: long from the OB; stop below; target the external structural swing high.

**RR Doubling Hack:** Instead of placing a limit order directly on the OB, drop to 1m when price enters the OB zone. Wait for 1m CHoCH confirmation inside the zone before entering. This converts a 1:3 entry into a 1:7+ entry while also adding confirmation.

## Asset Considerations
- Crypto: OBs work well on BTC/ETH; less reliable on low-cap alts with thin liquidity.
- Forex: very effective, especially during session opens when institutional orders are placed.
- Gold: effective but OBs can be blown through during high-impact news events.

## Python Implementation Notes
- Requires displacement detection (see [[concepts/displacement]]).
- Algorithm:
  1. Detect impulse moves: N consecutive candles in one direction exceeding a threshold (e.g. ATR multiple).
  2. Look back for the last opposing candle before the impulse.
  3. Record OB zone as `(open, close)` or `(high, low)` of that candle.
- Mitigation check: `if candle_close < ob_low (for bullish OB): mark_ob_mitigated()`
- OB data structure: `{id, type, high, low, open, close, timestamp, timeframe, mitigated: bool, breaker: bool}`
- Key parameter: how many candles define "the impulse"? Tune this in backtesting.
- Fresh vs. mitigated status must be tracked in real time as new candles form.

## Connections
- Related concepts: [[concepts/fair-value-gap]], [[concepts/displacement]], [[concepts/liquidity]], [[concepts/premium-discount]], [[concepts/mitigation]]
- Part of: [[concepts/smart-money-concepts]]
- Used by educator: [[entities/ict]]

## Lewis Kelly: Three Types of OBs That Work

Source: [[sources/lk-ultimate-order-block-2025]]

LK identifies three types of OBs with statistically proven performance (2.5 years, hundreds of trades):

### Type 1: The Strong OB
- The OB is the range/consolidation *before* a move that **breaks a significant structural level (BOS)**.
- The BOS is the qualifier — if the associated move didn't break structure, the OB is weak.
- This is the highest-probability OB type.
- **Code logic:** `ob_is_strong = True if ob_associated_with_bos_event else False`

### Type 2: The Sweep OB
- Price sweeps a session liquidity level (Asia high/low, Equal highs/lows, PDH/PDL).
- The OB is created at the sweep level — the range/candle cluster before the sweep reversal.
- Must be aligned with HTF trend direction.
- **Code logic:** `ob_is_sweep_type = True if liquidity_level_swept_within_N_candles_before_ob else False`

### Type 3: Confirmation Method (applies to any OB)
- Drop to 1m when price enters the OB.
- Wait for 1m CHoCH in trade direction.
- Drop to 5m, find 5m OB/FVG at the level.
- Enter from 5m. Stop below 5m OB. Target: 15m target.
- **Effect:** Improves RR from ~1:2 → ~1:4 or 1:5. Removes guessing between multiple OBs.

### Invalid/Weak OBs (never trade):
1. **Counter-trend OB:** OB in opposite direction to structure (bearish OB in bullish market). Will get traded through.
2. **Meaningless OB:** No BOS or sweep associated. Just a random internal reaction.
3. **Mitigated OB:** Price has already passed through the zone.

### LK OB Definition (note vs ICT)
LK defines the OB as the **consolidation range** before the aggressive expansion — this is a wider zone than the ICT "last opposing candle" definition. In practice, LK often marks the entire range from the first candle of the consolidation to the last candle before the expansion.

**Implication for bot:** Implement both definitions; allow parameter to choose. Test which produces higher win rate in backtesting.

## OB Selection Criteria (Three Rules, LK — 2024)
Source: [[sources/lk-ob-masterclass]], [[sources/lk-ob-forex-breakdown]]

Three rules that must ALL be satisfied for a high-probability OB:

1. **Pro-Trend:** OB must align with the external market structure trend. Never trade a bearish OB in a bullish market.
2. **Run on Liquidity:** The OB (sell-to-buy) must have swept a liquidity level immediately before the aggressive expansion. The "final sell" candle before the buy must have taken out a previous low (equal lows, swing low). Without a liquidity sweep, there is no institutional signature.
3. **BOS in the structural price leg:** The price leg that contains the OB must have produced a BOS. An OB inside a price leg with no BOS = weak OB, likely to fail.

**FVG as additional filter:** LK requires a Fair Value Gap (imbalance between candle 1 high and candle 3 low) to confirm an OB. An OB without FVG is lower priority.

**Right place, wrong time:** An OB that sits INSIDE the Asia session range (8pm–midnight EST) should be avoided. Asia liquidity will be swept by London before any OB inside that range can hold. Wait for Asia liquidity to be cleared first.

**The liquidity trap:** Avoid OBs that sit directly on resting liquidity (equal lows below, equal highs above). If price enters the OB with momentum, it will continue through the resting liquidity below, stopping you out. Only enter OBs where the resting liquidity has ALREADY been swept.

**Python:**
```python
def validate_ob(ob, market_context):
    """Check all 3 OB selection rules."""
    pro_trend = ob.direction == market_context.trend
    liquidity_swept = any(
        c.low < market_context.recent_low and c.close > market_context.recent_low
        for c in ob.pre_ob_candles
    )
    bos_in_leg = market_context.bos_occurred_in_current_leg
    has_fvg = ob.has_fvg  # candle1.high does not touch candle3.low
    
    return pro_trend and liquidity_swept and bos_in_leg and has_fvg
```

## Five-Principle OB Selection Framework (LK — June 2023, Earliest)
Source: [[sources/lk-only-ob-video]]

The earliest formulation of LK's OB selection framework (pre-dates the 3-rule version by ~10 months). Five principles must ALL be present:

1. **OB must take liquidity OR have liquidity to take.** Either the OB already swept a level, or there is a clear unswept liquidity level ahead for price to travel toward after the OB fills.
2. **OB must break something significant.** The move from the OB caused a BOS or CHoCH — it is structurally meaningful, not random.
3. **OB must be relevant to current price action.** It must sit inside the current trading range (the active structural box). OBs from stale ranges are irrelevant and will be broken through.
4. **OB must have imbalance.** A FVG must exist between candle 1 and candle 3. This is the institutional fingerprint.
5. **OB must have a liquidity pool to target.** After the OB is tapped and fills, there must be a clear liquidity target in the trade direction (equal highs/lows, swing high/low, trendline liquidity).

**"Spot the liquidity or BE the liquidity":** If your OB is the nearest unswept order cluster with no prior liquidity sweep, the market will use your OB AS its liquidity source. The OB becomes the target, not the entry.

**Relationship to 3-rule framework:** The 5-principle (2023) and 3-rule (2024) frameworks are the same concept at different levels of abstraction. Principles 1+5 (liquidity taken + liquidity to target) map to rule 2 (run on liquidity). Principles 2+3 map to rules 1+3 (pro-trend + BOS in leg). Principle 4 (imbalance/FVG) is unchanged. The 3-rule version is more compact; the 5-principle version more granular.

## Order Flow Confirmation — Rule 3 / RR Enhancement (LK — December 2025)
Source: [[sources/lk-simple-ob-2025]]

The most recent formulation of LK's OB entry model adds a critical third rule that dramatically improves RR without changing the setup requirements.

### Rule 3: Order Flow Confirmation
When price enters a validated OB (Rule 1: follow the money, Rule 2: avoid the traps), do not enter immediately at the OB zone. Instead:
1. Drop to 1m timeframe.
2. Observe: 1m will be bearish as price pulls into the zone (this is normal — it's the pullback).
3. Wait for the 1m bearish structure to **break bullish** (1m CHoCH) — a 1m swing high is taken out.
4. Enter at the 1m CHoCH level. Stop = below the 1m swing low that formed at or after the entry zone.

**Why this works mechanically:** The standard OB entry (blind limit at OB zone) places a stop below the OB bottom. The order flow confirmation entry uses a 1m structure shift as both confirmation AND tight stop anchor — placing the stop below a recent 1m low that may be only a fraction of the OB zone's height.

**RR example:**
- Without confirmation: OB zone = 10 pips; stop = 10 pips; target = 30 pips → RR 1:3
- With 1m CHoCH inside OB: entry at CHoCH level; stop = 3 pips below 1m low; same 30-pip target → RR 1:10
- Same trade direction, 3.3× better RR, plus added confirmation of zone validity.

**Relationship to Type 3 Confirmation Method:** This is the same technique documented in [[sources/lk-ultimate-order-block-2025]] as "Type 3 Confirmation Method." Both sources confirm 1m CHoCH inside OB = canonical LK entry model. Consistent across 2024 and 2025 sources.

**Python:**
```python
def ob_order_flow_entry(ob_zone, candles_1m, market_context):
    """Order flow confirmation entry inside OB zone."""
    # Step 1: Check if price is inside the OB zone
    current_price = candles_1m[-1].close
    if not (ob_zone.bottom <= current_price <= ob_zone.top):
        return None
    
    # Step 2: Verify 1m structure is currently bearish (pullback in progress)
    ltf_bias = get_1m_structure_bias(candles_1m)
    if ltf_bias != 'bearish':
        return None  # Not yet in pullback phase
    
    # Step 3: Wait for 1m CHoCH bullish (high breaks)
    if detect_1m_choch_bullish(candles_1m):
        choch_level = get_1m_choch_price(candles_1m)
        stop_level = get_1m_swing_low_after_choch(candles_1m)
        return {
            'entry': choch_level,
            'stop': stop_level,
            'target': market_context.next_structural_high
        }
    return None
```

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
| 2026-04-05 | [[sources/lk-ultimate-order-block-2025]], [[sources/lk-ultimate-smc-guide-2026]], [[sources/lk-structure-ob-profits-2025]] | Added LK three OB types, confirmation method, LK vs ICT definition comparison |
| 2026-04-06 | [[sources/lk-ob-masterclass]], [[sources/lk-ob-forex-breakdown]], [[sources/lk-smc-full-2023]] | Added 3-rule OB selection criteria, FVG requirement, liquidity trap filter, Asia session timing filter |
| 2026-04-06 | [[sources/lk-only-ob-video]] | Added 5-principle selection framework (earliest statement, June 2023); "spot the liquidity or be the liquidity" principle; 5-principle vs 3-rule mapping |
| 2026-04-06 | [[sources/lk-simple-ob-2025]] | Added Rule 3 (order flow confirmation); RR enhancement mechanics; Python entry logic; cross-reference to Type 3 confirmation method |
