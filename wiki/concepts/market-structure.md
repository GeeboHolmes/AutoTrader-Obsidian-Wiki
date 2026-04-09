---
type: concept
concept_type: smc
aliases: [MS, Structure, HH HL LH LL]
tags: [smc, foundational, bias]
---

## Definition
Market Structure is the framework of higher highs (HH), higher lows (HL), lower highs (LH), and lower lows (LL) formed by significant swing points. It is the foundation of directional bias in SMC — every other concept is interpreted in the context of current structure.

## How It Works
Price moves in a fractal sequence of impulse legs and retracements:
- **Bullish structure:** HH → HL → HH → HL (higher highs and higher lows)
- **Bearish structure:** LH → LL → LH → LL (lower highs and lower lows)
- **Ranging / consolidation:** No clear sequence of HH/HL or LH/LL

The dominant timeframe structure defines the *bias*. Lower timeframe structure is used for *entry timing* in the direction of higher timeframe bias.

## Identification Rules
1. A **swing high** is a candle with a higher high than both the candle before and the candle after it. (Minimum 1 candle on each side — but context-dependent; more confirmation = more reliable.)
2. A **swing low** is a candle with a lower low than both the candle before and the candle after it.
3. **Bullish structure** is confirmed when price creates a new HH (breaks above the last swing high).
4. **Bearish structure** is confirmed when price creates a new LL (breaks below the last swing low).
5. Structure is **timeframe-specific** — always note which timeframe you are reading.

## Trading Application
- **Higher timeframe (HTF) structure** (4H, Daily, Weekly) = directional bias for the session/week.
- **Intermediate timeframe (ITF) structure** (1H, 15m) = confirms setup context.
- **Lower timeframe (LTF) structure** (5m, 1m) = entry confirmation.
- Only trade setups aligned with HTF structure. A bullish HTF means: look for longs at discounts; shorts are counter-trend and higher risk.

## Timeframe Considerations
- HTF structure is slow to change and more reliable.
- LTF structure changes rapidly; use only for precision entries once HTF bias is established.
- The shift from HTF bullish to bearish (or vice versa) is called a [[concepts/break-of-structure|CHoCH]] — high-significance event.

## Asset Considerations
- Crypto: structure can be very clean on BTC/USDT due to high volume and 24/7 trading.
- Crypto altcoins: structure is less reliable; BTC dominance and correlation must be considered.
- Forex: structure aligns with session times (London, New York) — check for structure before session open.
- Gold (XAU/USD): highly reactive to macro events; structure can be broken rapidly by news.

## Python Implementation Notes
- Swing point detection: look for local maxima/minima with a lookback window (e.g. 5 candles each side).
- Common approach: `scipy.signal.argrelextrema` or a rolling comparison.
- Be explicit about lookback window — this parameter heavily affects results in backtesting.
- Store swing highs and lows as a list of `(timestamp, price, type)` tuples.
- Structure state machine: track `last_hh`, `last_hl`, `last_lh`, `last_ll` and current bias.
- A BOS event triggers when price closes beyond the last swing point in the trending direction.

## Connections
- Related concepts: [[concepts/break-of-structure]], [[concepts/swing-points]], [[concepts/premium-discount]]
- Part of: [[concepts/smart-money-concepts]]

## Lewis Kelly Specific Rules
These are LK-specific additions sourced from his transcripts — precise enough to implement:

### Internal vs External Structure (CRITICAL)
- **External structure:** The range from the most recent significant swing low to the most recent significant swing high. This is the "real" structure.
- **Internal structure:** Everything that happens *inside* the external range. Internal BOS and CHoCH signals should be used only for entry timing, NOT for bias decisions.
- **Common mistake:** Traders see an internal LH and LL sequence and go short — but they're trading internal structure inside a bullish external range. Price then sweeps their stops and continues higher.
- **Rule:** Never change bias based on internal structure alone. Wait for external range to break.

### Valid Pullback Rule (3-Candle Rule)
- A structural low (for bullish) is only confirmed when price pulls back with **3 consecutive bearish candles, each closing below the previous**.
- 1 or 2 candles = fake pullback. Not a real structural low. Don't trade it.
- This prevents false entries into fake lows that are actually still the original swing low.

### Correct Swing Low After BOS
- After a bullish BOS: the new structural low is the **lowest point price retraced FROM the high BEFORE the BOS candle**.
- Not the low of the BOS candle. Not any internal low. The lowest point of the pullback sequence that preceded the breakout.

### Wick Rule / Candle Closure Rule
Source: [[sources/lk-candle-closure]]

- A wick below a swing low (price pierces but closes above) = **liquidation / manipulation**. NOT a structural break.
- Only candle **closes** count for BOS and CHoCH detection.
- A wick sweep of a level followed by immediate close back above = strong reversal signal, not a break.
- **Multiple failed wicks at same level = increasing confidence of reversal.** Each failed close above a swing high = stronger evidence that the high will not break and the opposing side (low) will be targeted.
- **Do NOT assume a wick through a high is bullish.** The market only confirms a level is accepted when price closes above it. Until then, it is rejection.
- Common mistake: traders see wick through old high, assume bullish BOS, go long → get trapped as price fails to close above and reverses.
- Candle closure applies to ALL timeframes: daily, 15m, 1m.

**Python:** `bos_confirmed = candle.close > swing_high` (NOT `candle.high > swing_high`)

### Weak vs Strong Structure (CRITICAL)
Source: [[sources/lk-ms-8lessons]], [[sources/lk-advanced-smc-2023]]

Every structural swing point has a "responsibility" — a job it is meant to do:
- A **swing high's job** is to break the corresponding swing low below it.
- A **swing low's job** is to break the corresponding swing high above it.

Based on whether a swing point fulfils its job:

| State | Condition | Implication |
|-------|-----------|-------------|
| **Strong** | Swing point successfully completes its job (breaks the opposing level) | Protected — institutions hold positions from that level; price will defend it |
| **Weak** | Swing point fails to complete its job (starts reversing before breaking opposing level) | Targeted — this level WILL be swept; mark it as liquidity, not resistance/support |

**Rules:**
- When a high starts reversing back up toward the previous high without having taken out the corresponding low → the previous high is WEAK → it will be swept as liquidity.
- Weak structure is the precise definition of what makes a level a valid [[concepts/liquidity|liquidity sweep]] target.
- Each failed attempt to break the opposing level increases the weakness (more stop clusters accumulate).

**Example (bearish scenario):**
1. We have a strong high (H1) that caused CHoCH bearish.
2. Price creates a low (L1), then a lower high (H2). H2's job = take out L1.
3. If H2 reverses before taking L1 → H2 is WEAK → H2 will be swept.
4. When H2 gets swept: that is the liquidity raid. Expect CHoCH after the sweep (H2 was just taking stop losses to fill institutional positions).

**Box Method for Correct Swing Identification:**
- Draw a box from the current swing low to the current swing high.
- The NEW swing low after a bullish BOS = the LOWEST point inside the box before price broke out (not any internal low).
- This prevents using a false internal low that gets swept without invalidating structure.

**Python (weak structure detection):**
```python
def is_weak_high(high_level, swing_low_below, recent_candles):
    """A high is weak if it reversed without closing below the corresponding swing low."""
    # Check if the high's responsibility (swing_low_below) has been taken
    took_out_low = any(c.close < swing_low_below for c in recent_candles)
    if not took_out_low:
        # High reversed without taking the low → weak, will be targeted
        return True
    return False
```

### Realignment Concept
Source: [[sources/lk-advanced-smc-2023]], [[sources/lk-smc-full-2023]]

- **Realignment** = when a lower timeframe CHoCH aligns with the higher timeframe's intended next move.
- Example: 4H is bearish. 15m shifts to a temporary bullish structure (pullback phase). When 15m CHoCH back to bearish → the 15m has "realigned" with 4H intent → this is the signal the 4H pullback is done and the 4H will now continue lower.
- Realignment is the core entry trigger in LK's multi-timeframe system.
- Before realignment: no entry. After realignment + entry at OB: high-probability setup.

**Timeframe ratios (LK's 15–20x rule):** Use timeframes 15–20x apart. 15m × 16 = 4H. Don't use daily + 1m together.

## Order Flow Terminology (LK — July 2023)
Source: [[sources/lk-advanced-ms-course-2023]]

LK uses "order flow" as a distinct label for the internal structure within a swing range. Three levels of structure hierarchy:

| Level | Name | Definition |
|-------|------|------------|
| 1 | **Swing structure** | The external high/low defining the current trading range |
| 2 | **Internal structure** | All price action inside the swing range |
| 3 | **Order flow** | The present-moment directional flow within internal structure |

When external swing structure is bullish but order flow (internal) is bearish → pullback phase.
When order flow realigns bullish (internal CHoCH higher) → pullback complete → entry zone.

This is the earliest (July 2023) explicit labeling of what later became the "realignment" entry model.

## Probability Decreases Lower in Price Leg (CRITICAL Rule)
Source: [[sources/lk-rule-based-smc-2023]]

A fundamental trading decision rule: the probability of a reaction at an OB/FVG decreases the further price has already travelled into a price leg.

- **First CHoCH entry** (top of the price leg) = highest probability
- **Each subsequent entry** further down the leg = decreasing probability
- **Near the bottom of the leg** = avoid entirely; risk of inducement or outright reversal is highest

**Why:** When price is already extended into a leg, the next move is increasingly likely to be an inducement (sweeping resting liquidity at the bottom) rather than a continuation. The market needs to build a reason to continue.

**Rule:** Only trade the first 1–2 OBs after a CHoCH. Skip continuation entries that are more than 50% into the price leg unless there is a fresh CHoCH resetting the leg.

**Python:**
```python
def entry_probability_score(current_price, leg_high, leg_low, choch_count):
    """Score 0-1: probability of reaction decreasing as price extends into leg."""
    leg_range = leg_high - leg_low
    if leg_range == 0:
        return 0
    position_in_leg = (leg_high - current_price) / leg_range  # 0 = top, 1 = bottom
    choch_penalty = min(choch_count * 0.2, 0.6)  # each CHoCH resets slightly
    return max(0, 1.0 - position_in_leg - choch_penalty)
```

## "Responsible Low" CHoCH Definition (LK — March 2026)
Source: [[sources/lk-ultimate-smc-full-2026]]

The clearest statement of which swing low defines a CHoCH:

> CHoCH occurs when the **responsible low** is broken — the specific swing low that was directly responsible for creating (causing the breakout that produced) the last significant structural high.

**Why this matters:** In a bullish trend with multiple lows, only ONE low is the "responsible" one — the swing that produced the high. Other lows are internal noise. Breaking those internal lows is not a CHoCH; it is inducement or a failed pullback.

**Example:**
- Bullish trend: Low A → High A → Low B → High B (BOS above High A). 
- Low B is the "responsible low" for High B.
- If price breaks below Low B → CHoCH (potential reversal). 
- If price only breaks below some internal low between Low B and High B → NOT a CHoCH.

This is consistent with the "external vs internal structure" rule: only external structural lows are valid CHoCH/BOS reference points.

**Python:**
```python
def find_responsible_low(swing_lows, bos_high_timestamp):
    """Return the swing low that was responsible for the BOS at bos_high_timestamp."""
    # The responsible low is the lowest swing low between the previous BOS and the current BOS
    # Filter swing lows that occurred before the BOS that created the current high
    candidate_lows = [sl for sl in swing_lows if sl.timestamp < bos_high_timestamp]
    # The responsible low is the LOWEST of these (the deepest retracement before the last BOS)
    if candidate_lows:
        return min(candidate_lows, key=lambda sl: sl.price)
    return None
```

## Superseded Structural Levels Become Liquidity (LK — January 2026)
Source: [[sources/lk-ultimate-ms-guide-2026]]

> "The high that puts in a low is only important until that low has been broken — after which it becomes liquidity. Right? All that this means is that it's a future point of liquidity, meaning that at some point in the future price will revisit that price level but they don't mean anything structurally."

A structural level has two life phases:
1. **Active structural reference:** While the swing it created is intact, the level is a valid BOS/CHoCH reference point.
2. **Liquidity target:** Once the associated swing is superseded (broken by a new BOS), the level loses structural significance and becomes a draw — price will revisit it to collect stops clustered there.

**Rule:** When a structural level is superseded, stop treating it as support/resistance. Reclassify it as an upcoming liquidity target.

**Example:**
- Bearish market: High A creates Low A (responsible high for Low A).
- Price breaks below Low A (BOS downward) → new LL → High A is now superseded.
- High A is no longer meaningful resistance. It is where short-sellers' stops reside — a future sweep target.

**Python:**
```python
def reclassify_superseded_levels(structural_levels, new_bos_event):
    """After a BOS, reclassify the structural level that has been superseded."""
    for level in structural_levels:
        if level.type == 'swing_high' and new_bos_event.direction == 'bearish':
            # The swing high that created the now-broken swing low loses structural status
            if level.created_swing_low == new_bos_event.broken_low:
                level.status = 'liquidity_target'
                level.structural_active = False
        elif level.type == 'swing_low' and new_bos_event.direction == 'bullish':
            if level.created_swing_high == new_bos_event.broken_high:
                level.status = 'liquidity_target'
                level.structural_active = False
    return structural_levels
```

## Impulse vs Correction Framework (LK — January 2026)
Source: [[sources/lk-ultimate-ms-guide-2026]]

All price action is either **impulsive** or **corrective**. Only impulsive moves should be traded.

| Type | Characteristics | Trade? |
|------|----------------|--------|
| **Impulsive** | Aggressive, directional, multi-candle, minimal retracement, strong momentum | YES — institutional execution |
| **Corrective** | Choppy, overlapping candles, counter-trend, internal structure only, slow | NO — retail retracement noise |

**Framework:**
1. Identify the impulse direction (HTF bias).
2. Wait for correction phase to complete (price returns to demand zone/OB).
3. Enter at the start of the NEXT impulse.
4. Never chase or trade within correction phases.

**Why corrections fail:** Corrective moves have no institutional backing — they are retail retracements with overlapping orders in both directions. No clean structure; no clear target.

**Python:**
```python
def classify_move(candles, atr_multiplier=1.5):
    """Classify a sequence of candles as impulsive or corrective."""
    avg_atr = calculate_atr(candles, period=14)
    directional = all_same_direction(candles[-3:])  # last 3 candles same direction
    strong_momentum = candles[-1].body_size > avg_atr * atr_multiplier
    minimal_overlap = candle_overlap_ratio(candles[-3:]) < 0.3
    
    if directional and strong_momentum and minimal_overlap:
        return 'impulsive'
    return 'corrective'
```

## Three-Type Structure Framework (LK — August 2023, Named)
Source: [[sources/lk-ms-strategy-2023]], [[sources/lk-ultimate-ms-free-2023]]

The three types of structure given explicit labels in August 2023 (earliest named formulation):

| Type | Label | Color (LK's charts) | Description |
|------|-------|---------------------|-------------|
| 1 | **Swing Structure** | Red | External swing points where price reverses; defines the trading range and bias |
| 2 | **Order Flow** | Purple | Internal present-moment direction; always counter to swing during pullback |
| 3 | **Order Flow Realignment (OFR)** | Green | When internal aligns with swing direction — the entry signal |

**OFR label (earliest):** In the August 2023 video, LK explicitly uses "OFR" and "order flow realignment" as a proper noun. Later videos use "realignment" or "1m CHoCH" without the OFR label. Both refer to the same concept.

**November 2023 expanded naming:** By November 2023, LK adds "Internal Counter" (IC) for the pullback-phase internal structure, and "Internal Pro-Trend" (IP) for the OFR signal. These are the same concepts with more precise terminology.

| Label | Meaning |
|-------|---------|
| IC (Internal Counter) | Order flow counter to swing = pullback phase |
| IP (Internal Pro-Trend) | Order flow aligned with swing = OFR entry signal |

## Three-Phase Reversal Model (LK — November 2023)
Source: [[sources/lk-ultimate-ms-free-2023]]

The most conservative confirmation of a trend change requires all three phases:
1. **CHoCH** — first sign: swing structure produces a higher high (bearish trend) or lower low (bullish trend). Possible reversal, not confirmed.
2. **HL or LH formation** — price pulls back and forms a structural HL (for new bullish trend) or LH (for new bearish trend), respecting an area of demand/supply.
3. **BOS** — price breaks and closes beyond the next structural level, confirming the new trend.

**Note:** LK states he does trade the CHoCH alone (phase 1 only) but acknowledges it is lower probability. The full 3-phase model is the highest-confidence confirmation. Useful for bot strategy filter: use all 3 phases for A+ setups, CHoCH alone only when additional confluence (liquidity, OB) is strong.

## ICI Entry Model Label (LK — November 2023)
Source: [[sources/lk-cleanest-smc-2023]]

The core LK entry model is named "Impulse-Correction-Impulse" (ICI) in the November 2023 video. This maps exactly to the OFR-based entry:
- **Impulse 1** = M15 BOS (the structural impulse that sets bias)
- **Correction** = price pulls back into OB/demand zone; 1m order flow is counter to M15 (normal pullback)
- **Impulse 2** = 1m CHoCH/OFR confirms correction is done → entry for the continuation

This is the same model described in 2024–2026 videos as the standard LK entry. ICI is the 2023 label; later videos just describe the mechanics without the abbreviation.

## "Decisional Area" vs "Origin" OB Terminology (LK — August 2023)
Source: [[sources/lk-best-smc-2023]]

Two distinct OB zones identified for every structural setup:
- **Decisional area** = the OB/supply zone that directly caused the BOS/CHoCH. Primary POI. Most likely to be respected (highest probability).
- **Origin** = an unmitigated OB further up in the price leg (the original impulse source zone). Secondary POI — used if price overshoots the decisional.

This distinction is used in live trade analysis. For bot implementation: always identify BOTH zones; enter at decisional, with failsafe at origin if price runs through the first zone.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
| 2026-04-05 | [[sources/lk-market-structure-full-course]], [[sources/lk-ultimate-smc-guide-2026]], [[sources/lk-structure-ob-profits-2025]] | Added LK-specific rules: internal vs external, 3-candle pullback, correct swing low, wick rule |
| 2026-04-06 | [[sources/lk-ms-8lessons]], [[sources/lk-advanced-smc-2023]], [[sources/lk-smc-full-2023]] | Added weak/strong structure framework, box method for swing ID, realignment concept, 15-20x timeframe ratio rule |
| 2026-04-06 | [[sources/lk-advanced-ms-course-2023]], [[sources/lk-rule-based-smc-2023]] | Added order flow terminology (3-level hierarchy), probability decreases lower in price leg rule |
| 2026-04-06 | [[sources/lk-ultimate-smc-full-2026]] | Added "responsible low" CHoCH definition — only the structural low that caused the last BOS qualifies as CHoCH reference; Python implementation |
| 2026-04-06 | [[sources/lk-ultimate-ms-guide-2026]] | Added superseded levels → liquidity rule; impulse vs correction framework with Python classification logic |
| 2026-04-06 | [[sources/lk-ms-strategy-2023]], [[sources/lk-ultimate-ms-free-2023]], [[sources/lk-cleanest-smc-2023]], [[sources/lk-best-smc-2023]] | Added three-type structure framework (swing/OFR/IC/IP labels); three-phase reversal model; ICI entry model label; decisional area vs origin OB terminology |
