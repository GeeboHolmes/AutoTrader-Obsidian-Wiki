---
type: concept
concept_type: smc
aliases: [Liquidity, BSL, SSL, EQH, EQL, Liquidity Pool, Resting Liquidity, Stop Hunt, Liquidity Sweep, Liquidity Raid]
tags: [smc, liquidity, institutional, foundational]
---

## Definition
Liquidity refers to clusters of resting orders (stop losses and pending orders) that accumulate above swing highs and below swing lows. Institutional participants need these order pools to fill their large positions — they deliberately engineer price to sweep these levels before reversing.

In SMC, liquidity is both a **target** (where price is drawn) and a **signal** (a sweep often precedes a reversal).

## Types of Liquidity

| Type | Abbreviation | Location | What's resting there |
|------|-------------|----------|----------------------|
| Buyside Liquidity | BSL | Above swing highs, equal highs, trendlines | Buy stops from short sellers; limit sell orders |
| Sellside Liquidity | SSL | Below swing lows, equal lows, trendlines | Sell stops from long holders; limit buy orders |
| Equal Highs | EQH | Two or more highs at the same price | Double-top stop clusters |
| Equal Lows | EQL | Two or more lows at the same price | Double-bottom stop clusters |
| Trendline Liquidity | — | Below ascending trendlines, above descending trendlines | Retail stop losses placed at trendlines |

## How It Works
1. Retail traders place stops predictably: above recent highs (for shorts) and below recent lows (for longs).
2. These stops accumulate to form a **liquidity pool**.
3. Smart money drives price into the pool, triggering the stops (which become market orders that fill institutional positions at favorable prices).
4. After the sweep, institutions reverse price in the opposite direction — this is the **liquidity raid** followed by **expansion**.

The sequence: **Build → Raid → Expand**

## Identification Rules
1. Mark all significant swing highs (BSL) and swing lows (SSL) on the chart.
2. **Equal highs/lows:** Two or more swing points at almost identical price levels (within a small tolerance — e.g. 0.1% of price).
3. **Trendline liquidity:** Retail-drawn trendlines have clusters of stops just beyond them — mark the other side of obvious trendlines.
4. **Old highs/lows:** Previous daily, weekly, or monthly highs/lows are major liquidity levels.
5. Higher timeframe liquidity is more significant than lower timeframe.

## Trading Application
- **As a target:** In a bullish trend, price will run toward the next BSL level (above swing highs). In bearish, toward SSL.
- **As an entry signal:** A liquidity sweep (price spikes through a level then quickly reverses) is a high-probability entry trigger, especially when combined with a [[concepts/order-blocks|Order Block]] or [[concepts/fair-value-gap|Fair Value Gap]] nearby.
- **Sweep + reversal pattern:** Price sweeps BSL → reversal candle → entry short (or sweeps SSL → entry long).
- **Never trade the sweep in the direction of the sweep** — wait for rejection and reversal confirmation.

## Timeframe Considerations
- HTF liquidity (weekly/daily highs/lows): major targets — price can travel significant distances to reach them.
- Intraday liquidity (session highs/lows, previous day H/L): relevant for intraday entries.
- LTF liquidity: used for stop placement and precise entry/exit.

## Asset Considerations
- Crypto: liquidity sweeps are very common and sharp, especially on altcoins. Funding rate manipulation adds another dimension.
- Forex: previous day/week highs/lows are classic liquidity levels. London and New York opens frequently sweep Asian session liquidity.
- Gold: very reactive to liquidity; often sweeps Asian session levels during London open.

## Python Implementation Notes
- BSL detection: identify all swing highs above current price; sort by significance (proximity, recency, volume).
- SSL detection: identify all swing lows below current price.
- Equal highs/lows: cluster swing points within a tolerance band (e.g. `abs(high_a - high_b) / high_a < 0.001`).
- Liquidity sweep detection: `if candle_high > swing_high and candle_close < swing_high: record_sweep('BSL', ...)`
- Store liquidity levels as: `(price, type, timeframe, timestamp_created, swept: bool, timestamp_swept)`.
- After a sweep, flag the level as `swept=True` — swept liquidity is less significant for future sweeps.

## Sweep vs Run Distinction
Source: [[sources/lk-sweep-vs-run]]

**Critical implementation distinction** — two patterns that look similar but have opposite implications:

| Pattern | What happens | Implication |
|---------|-------------|-------------|
| **Liquidity Sweep** | Price wicks ABOVE high → candle CLOSES BACK INSIDE range | Reversal — institutions sold into the sweep; expect price to go lower |
| **Liquidity Run** | Price breaks ABOVE high → candle CLOSES ABOVE and CONTINUES | Continuation — institutions using the liquidity to FUEL the move upward |

**Rule: only the CANDLE CLOSE determines which it is.** A wick is always a sweep; only a close confirms a run.

**Context stacking for valid sweeps** (probability increases with each layer):
1. Directional bias aligned (bearish bias to trade sweep of a high)
2. **Weak high** — the swing high whose "responsibility" was to take out the prior low but failed. This high is liquidity because it will be swept before structure continues.
3. Asia session high coinciding with the level
4. Equal highs coinciding with the level

Minimum 2 layers for a valid sweep entry; 3+ = A+ sweep.

**Python:** `sweep_confirmed = candle.high > level and candle.close < level`

## Connections
- Related concepts: [[concepts/market-structure]], [[concepts/order-blocks]], [[concepts/inducement]], [[concepts/displacement]]
- Part of: [[concepts/smart-money-concepts]]

## Lewis Kelly: Liquidity Framework

Source: [[sources/lk-liquidity-masterclass]], [[sources/lk-ultimate-smc-guide-2026]]

### LTT² (Liquidity to Take + Liquidity to Target)
LK's two-part liquidity requirement for every trade:

1. **Liquidity to Take (LTT):** Before entering, the opposing-side liquidity must already have been swept.
   - For longs: SSL (below swing lows, Asia low, London low) must have been swept.
   - For shorts: BSL (above swing highs, Asia high, London high) must have been swept.
   - **Rule:** "If you're buying before the liquidity is taken, you ARE the liquidity."
   - **Code:** `ssl_swept = any(candle.low < ssl_level for recent_candles)` — must be True before entry.

2. **Liquidity to Target (LTT2):** There must be unswept liquidity in the direction of trade above (for longs) or below (for shorts) current price.
   - For longs: Unswept BSL (session high, PDH, EQH) must exist above entry.
   - For shorts: Unswept SSL must exist below entry.
   - If no liquidity to target = no trade (not enough fuel for the move).

### Lewis Kelly's Liquidity Hierarchy
Most to least significant for intraday trading (15m framework):
1. Previous week's high/low
2. Previous day's high/low (PDH/PDL)
3. Current session high/low (London high/low, NY high/low)
4. Previous session high/low (Asia high that London will raid)
5. Equal highs/equal lows (EQH/EQL) within current swing range
6. Trendline liquidity (stops below ascending trendlines)

### How Retail Creates Liquidity (LK's "Dumb Money Map")
Retail trading strategies that create predictable stop clusters:
- **Trendline traders:** Stops below ascending trendlines → sweep the trendline, collect stops
- **S/R traders:** Stops at previous highs/lows → sweep the level, collect stops
- **Double top/bottom traders:** Stops above double tops → sweep, collect stops
- **Breakout traders:** Enter on breakouts → stops inside the range they broke from → sweep the retest, collect stops
- **Break-and-retest traders:** Stops above/below the retest level → sweep and collect

### Session-Based Liquidity Timing
- **London opens:** Will sweep Asia session high or low first (manipulation phase) before the real move.
- **New York opens:** Will sweep London session high or low (PO3 distribution) before continuing.
- **Strategy:** In a bullish market, London will sweep Asia lows (SSL) then London will push toward Asia high (BSL target).

## Institutional Position Mechanics (LK — May 2025)
Source: [[sources/lk-ultimate-smc-2025]]

The clearest explanation of WHY institutions need retail liquidity — and why they must manipulate price to collect it.

### The $500M Position Problem
A large institution wanting to buy $500M of EUR/USD cannot execute as a single order:
- A single massive buy order moves price sharply against itself.
- By the time the order is filled, the average entry price is far above the ideal level — a massive spread cost.

**Solution:** Partition the position. Split $500M into hundreds of small fragments (e.g. $5M each). Then accumulate gradually by absorbing sell-side liquidity as it arrives.

**The ranging/consolidation phase = institutional accumulation:**
- Price ranges because buyers and sellers are in balance (fair market value).
- During ranging, the institution absorbs sell orders: every seller who steps in gets matched by the institution's buy orders.
- When sellers are exhausted, the institution has filled most of its position → price aggressively moves in the direction of the institutional order.

**Why manipulation happens:**
- If not enough sell-side liquidity naturally arrives during the range, institutions actively push price into areas where they know retail will be selling (liquidity levels: EQL, trendlines, session lows).
- This engineered sweep generates the sell-side liquidity the institution needs.
- After sweeping and filling its position, the institution reverses and drives price higher.

### Liquidity = Three Forms Only
Every unit of liquidity that enters the market is one of only three things:
1. **Entries** — traders opening positions (buying or selling)
2. **Stop-losses** — positions being forcibly closed (converts to market orders)
3. **Take-profits** — positions being voluntarily closed

**Implication:** Identifying where retail stop-losses and take-profits cluster = identifying where institutional orders will be filled. Old highs = where sellers' stops are; old lows = where buyers' stops are.

### Context vs Pattern Recognition
Most SMC traders fail because they use **pattern recognition** (spotting equal highs, order blocks) without **context** (understanding who is positioned there and what they will do). Knowing an equal high is a liquidity level is necessary but not sufficient — understanding that trend-line traders, breakout traders, and range sellers are all clustered at the same level elevates the probability assessment.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
| 2026-04-05 | [[sources/lk-liquidity-masterclass]], [[sources/lk-ultimate-smc-guide-2026]] | Added LTT² framework, liquidity hierarchy, dumb money map, session timing |
| 2026-04-06 | [[sources/lk-sweep-vs-run]] | Added sweep vs run distinction with Python implementation |
| 2026-04-06 | [[sources/lk-ultimate-smc-2025]] | Added $500M position mechanics, ranging = accumulation model, three forms of liquidity, context vs pattern recognition |
