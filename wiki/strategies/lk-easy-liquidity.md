---
type: strategy
status: research
asset_class: forex
timeframes: [15m, 1m, 5m]
style: intraday
tags: [liquidity, session-based, reactive, london]
---

## Overview
A fully reactive session-based strategy. No pre-established HTF bias. The session itself reveals the trade direction: whichever liquidity extreme (Asia High or PDL/PDH) gets swept first in London determines the trade direction. After the sweep, wait for 1m CHoCH + 5m OB entry. Target the opposite liquidity extreme.

## Market Bias (Direction Filter)
- No pre-determined bias.
- Directional bias determined by which side London sweeps first.
- If London sweeps Asia High (BSL) first → go SHORT.
- If London sweeps Asia Low / PDL (SSL) first → go LONG.
- Only trade in London session (or NY as secondary).

## Setup Conditions
1. Mark the following liquidity levels on 15m before London session opens:
   - Asia session high and low (8pm–midnight EST range extremes)
   - Previous day high (PDH) and previous day low (PDL)
   - Equal highs and equal lows within the current swing range
2. Come to the session with no directional bias — simply observe which side gets swept first.
3. Sweep must be confirmed: price must wick through the level and CLOSE BACK INSIDE (not close through — that is a run, not a sweep).

## Entry Rules
1. Wait for sweep of Asia High or Asia Low (with close back inside).
2. On the 15m, observe for order flow shift signal (aggressive reversal candle or small CHoCH).
3. Drop to 1m. Look for 1m bearish (after BSL sweep) or 1m bullish (after SSL sweep) structure.
4. Wait for 1m CHoCH in trade direction (after sweep: if swept BSL, wait for 1m bearish CHoCH).
5. Identify the 5m sell-to-buy (or buy-to-sell) OB at the CHoCH level.
6. Enter from 5m OB.

## Stop Loss Rules
- Stop above the 5m OB high (for short) or below 5m OB low (for long).
- Alternatively: stop below the swing low of the sweep candle (wider, more conservative).
- Typical stop: 5–12 pips on forex.

## Take Profit Rules
- Primary target: the opposite session liquidity extreme.
  - If swept Asia High and entered short → target Asia Low.
  - If swept Asia Low and entered long → target Asia High.
- Secondary target: PDH/PDL (whichever was not swept).
- Move to break even after price moves 1R in favor.

## Risk Management
- Risk per trade: standard 1% account risk.
- Max daily: 2 losses.
- Position sizing: `lot_size = (account × risk_pct) / stop_pips / pip_value`.

## Filters (What Invalidates the Setup)
- Sweep does not have close-back-inside (i.e., it is a run, not a sweep — price closes through the level). Do not trade.
- No 1m CHoCH develops within 30 minutes of the sweep → skip the trade.
- Both Asia High AND Asia Low swept before London open (both sides already taken) → no valid directional signal.
- High-impact news scheduled within the session window → stand aside.

## Timeframe Alignment
- 15m: liquidity level identification and sweep confirmation.
- 5m: entry OB identification.
- 1m: CHoCH confirmation and OB timing.

## Session / Time of Day
- Primary: London session (2:00–5:00am EST).
- Secondary: New York session (7:00–10:00am EST) using London highs/lows as targets.
- Hard cutoff: no entries after session window ends.

## Backtesting Notes
- LK states ~49–51% win rate.
- Target RR is ~1:4 (Asia High to Asia Low range typical).
- With 50% WR and 1:4 RR: net 10R positive per 10 trades (5 × 4R win = +20R, 5 × 1R loss = -5R, net = +15R minus spread/commissions). 
- Parameter to test: sweep confirmation candle (close threshold in pips for "back inside" definition).
- Parameter to test: equal high tolerance (within N pips).

## Python Implementation Notes
```python
# Session range detection
asia_high = max(candles[asia_start:asia_end]['high'])
asia_low = min(candles[asia_start:asia_end]['low'])

# Sweep detection (close-back-inside)
def is_sweep(candle, level, direction='high'):
    if direction == 'high':
        return candle['high'] > level and candle['close'] < level
    else:
        return candle['low'] < level and candle['close'] > level

# Trade direction
if is_sweep(london_candle, asia_high, 'high'):
    bias = 'SHORT'
    target = asia_low
elif is_sweep(london_candle, asia_low, 'low'):
    bias = 'LONG'
    target = asia_high
else:
    bias = None  # No trade

# 1m CHoCH detection required before entry
```

## Open Issues / TODO
- Define the "1m CHoCH confirmation" rule precisely: how many candles must confirm the shift?
- Define tolerance for equal highs (equal within N pips?).
- Determine how to handle scenarios where both liquidity levels are swept in the same session.
- No HTF bias filter — consider adding a weekly/daily structure alignment as an optional filter to improve WR above 50%.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-easy-liquidity]] | Initial creation |
