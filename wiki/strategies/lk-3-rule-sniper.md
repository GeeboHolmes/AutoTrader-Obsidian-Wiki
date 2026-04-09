---
type: strategy
status: research
asset_class: multi
timeframes: [15m, 1m, 5m]
style: intraday
tags: [smc, lk, sniper-entry, trade-management, ltf-choch]
---

## Overview
A 3-rule intraday strategy where the third rule is trade management rather than an entry condition. Uses 15m bias + 1m LTF CHoCH sniper entry, then a validated management system: TP1 at 5R (50% off), break-even stop, TP2 at session level, TP3 at swing high. 97% of break-even moves saved money in 1000+ trade sample.

## Market Bias (Direction Filter)
- **15m external structure (box method):** External swing points only. HH/HL = bullish, LH/LL = bearish.
- Asia low is explicitly mentioned as required — "buyer below Asia session low" = prerequisite for long setups.

## Setup Conditions
1. 15m external structure defines clear bullish or bearish bias.
2. Asia session low (bullish) or high (bearish) has been swept — implicit filter from LK's base framework.
3. Price has pulled back from the recent expansion move toward the bias direction.

## Entry Rules
1. 15m is bullish; price pulling back (1m is trending bearish within the pullback).
2. Drop to 1m. Wait for 1m to shift bullish (CHoCH — swing high taken to upside after bearish pullback).
3. Enter on the 1m CHoCH candle's close or at the next 1m OB/FVG.
4. Stop: at the 1m swing low at the point of the CHoCH (tight stop = high RR).
5. Note: 1m CHoCH converts what would be a 1:3 entry into a 1:13+ entry due to dramatically tighter stop.

## Stop Loss Rules
- Stop at the 1m swing low (for longs) or 1m swing high (for shorts) that defined the CHoCH point.
- This is the tightest possible stop — placed at the last structural low/high before the shift.
- Do not place stop at OB low or session low — the LTF stop is what generates the high RR.

## Take Profit Rules
- **TP1:** 5R from entry (1:5). Take 50% of position off. Move stop to break even.
- **TP2:** Major structural level — session high (for longs), equal highs, HOD. Take a further portion.
- **TP3:** External swing high/low — full target. Close remainder of position.
- Move stop to break even after any initial push, regardless of TP1. Data: 97% of break-even stops resulted in the trade continuing through SL before reaching TP — only 3% of BE trades reversed and hit the original TP after touching BE.

## Risk Management
- Risk per trade: standard bot risk model (1–2% per trade).
- Break-even discipline is statistically validated: 97% of time, moving SL to BE protects capital with minimal missed profit.
- Losing the first setup attempt often provides a better (lower) re-entry point — net RR can be higher on second attempt even accounting for first trade loss.

## Filters (What Invalidates the Setup)
- 15m structure is ambiguous (price in middle of range, no clear swing sequence) — wait.
- 1m shifts in same direction as current 15m trend (no pullback present) — not a valid CHoCH entry.
- Asia sweep not confirmed — implicit filter.

## Timeframe Alignment
- 15m: directional bias
- 1m: entry trigger (CHoCH) and stop placement
- 5m: optional confirmation of entry zone (OB/FVG can be identified here)

## Session / Time of Day
- London (2:00–5:00am EST) and NY (7:00–10:00am EST) implied from base framework.
- Not explicitly stated in source — apply standard LK session filters.

## Backtesting Notes
- Management system validated on 1000+ trades (sample size for BE statistic).
- $35,000 single trade example on EUR/USD, 12 November (year not specified in source).
- No overall strategy win rate or return data provided in this source.

## Python Implementation Notes
- Entry state machine: `BIAS → PULLBACK → CHoCH_1M → ENTRY`
- CHoCH on 1m: track rolling 1m swing high (last N-bar high); breach = shift
- Stop calculation: `stop_price = last_1m_swing_low` (for longs)
- TP1 calculation: `tp1 = entry + 5 * (entry - stop_price)`
- Position management: at TP1, `close 50% of size; move stop to entry price`
- Session level detection for TP2: `session_high = candles[session_open:now]['high'].max()`
- Track first trade loss to detect re-entry opportunity at better price

## Open Issues / TODO
- "Major session level" definition for TP2 — need precise rules (HOD? Equal highs? Distance threshold?)
- Validate 97% BE statistic on crypto — different session structure may change the statistic
- Re-entry logic after first trade loss — needs implementation spec

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-3-rule-sniper\|LK 3-Rule Sniper]] | Initial page created |
