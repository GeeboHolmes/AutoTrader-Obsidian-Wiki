---
type: concept
concept_type: execution
aliases: [Top-Down Analysis, TDA, Multi-Timeframe Analysis, MTFA]
tags: [execution, framework, lewis-kelly, timeframes]
---

## Definition
Top-down analysis is the process of reading higher timeframes first to establish directional context and key zones, then stepping down through progressively lower timeframes to find entry precision. The higher timeframe is the "why" (bias); the lower timeframe is the "when and where" (entry).

## Lewis Kelly's Three-Timeframe System

**Daily → 15m → 1m (entry via 5m)**

This is LK's primary framework for all intraday trading. Source: [[sources/lk-top-down-analysis]]

### Step 1: Daily Timeframe — Macro Bias
- Read the last 4–5 daily candles.
- 4 consecutive bearish daily candles = bearish continuation is probable for the next day.
- Identify any unmitigated **Daily FVGs or IFVGs** — these are weekly directional targets.
- Identify **previous day's high/low (PDH/PDL)** — major liquidity levels.
- **Daily candle character rule:**
  - When daily candle is bullish (in a bearish trend) = manipulation phase = look for shorts.
  - When daily candle is bearish near extreme (low of day far from open) = expect mean reversion bounce = look for longs.
  - "Mean reversion principle": Daily candle's low is almost always lower than its close. After bearish day, expect close to be higher than low = bounce trade opportunity.

### Step 2: 15-Minute Timeframe — Session Context
- New daily candle opens. Note the session open candle (London = 2:00 a.m. EST, NY = 7:00 a.m. EST).
- Confirm 15m structure aligns with daily bias.
- Identify internal session liquidity: Asia session high and low = the liquidity for London to sweep.
- Mark the current 15m swing range (swing low to swing high).
- Identify OBs within the discount zone (for longs) or premium zone (for shorts).

### Step 3: 1m Timeframe — Entry Confirmation
- When daily candle moves counter to bias (manipulation phase) → watch 1m for CHoCH.
- When price reaches the OB/FVG zone on 15m → switch to 1m.
- Wait for 1m structure to shift in the direction of trade (bearish → bullish for longs).
- Variation: "2-candle pullback break" — a 2-candle counter-trend move that then gets broken to the upside = simpler entry trigger.

### Step 4: 5m Timeframe — Entry Refinement
- After 1m CHoCH confirmed, switch to 5m.
- Find the 5m OB or FVG at the same level.
- Place entry at 5m OB/FVG. Stop below the 5m OB low (tighter stop = higher RR).
- Target: session liquidity (PDH, session high, EQH for longs).

## LK vs Full ICT Top-Down
| Aspect | Lewis Kelly | ICT (full approach) |
|--------|-------------|---------------------|
| Primary bias TF | Daily candle character | Weekly/Monthly/Daily structure |
| Execution TF | 15m → 1m → 5m | 4H → 1H → 15m → 5m → 1m |
| Liquidity context | Session-based (today's Asia/London/NY) | Macro (monthly/weekly highs/lows) |
| Trade frequency | Daily (1–3 setups per session) | Less frequent (higher timeframe setups) |

LK's approach is more intraday-focused and faster to execute. Suitable for a daily trading bot.

## Mean Reversion Trade
A secondary setup that exploits the daily candle structure:
1. Day has been strongly bearish (daily candle aggressively at/near low of day).
2. After session distribution, watch for exhaustion signals.
3. 15m: fails to close below previous 15m candle → swing low forming.
4. 1m: break of 2-candle bullish pullback = entry.
5. Entry: 5m/3m IFVG or FVG. Stop below the low of day. Target: toward the open/close of day (1:3–1:5 RR).

## Python Implementation Notes
- Requires daily OHLCV + intraday OHLCV in separate DataFrames.
- `daily_bias = 'bearish' if count_bearish_daily_candles(last_5) >= 3 else 'bullish'`
- Session candle detection: `is_london_open = (candle_time_est.hour == 2 and candle_time_est.minute == 0)`
- Daily candle character: compare current session's move relative to daily open.
- Mean reversion signal: track `daily_candle_extension = abs(current_low - daily_open) / average_daily_range`

## Connections
- Related concepts: [[concepts/market-structure]], [[concepts/killzones]], [[concepts/order-blocks]], [[concepts/fair-value-gap]], [[concepts/power-of-three]]
- Source: [[sources/lk-top-down-analysis]]
- Educator: [[entities/lewis-kelly]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | [[sources/lk-top-down-analysis]] | Created from Lewis Kelly's top-down video |
