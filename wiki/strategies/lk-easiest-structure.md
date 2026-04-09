---
type: strategy
status: research
asset_class: multi
timeframes: [15m, 1m]
style: intraday
tags: [smc, lk, 4-steps, supply-zone, asia-sweep, market-structure]
---

## Overview
A clean 4-step market structure strategy. Entry is a 15m supply OB zone triggered by Asia high sweep, confirmed by 1m CHoCH. Distinctive: "sell a bullish candle, buy a bearish candle" — enter short while daily is still printing bullish because the intraday structure context is clear. $26K live trade example.

## Market Bias (Direction Filter)
- **15m external structure:** swing high + swing low + BOS to downside = bearish.
- Additional context: daily candle printing bullish in a bearish trend = manipulation phase = ideal entry timing.

## Setup Conditions
1. 15m bearish trend confirmed (swing high → swing low → BOS below swing low).
2. Asia session high identified.
3. London session takes out Asia session high (sweep).
4. A 15m supply zone (OB) identified — the area where demand exhausted and sellers took control.

## Entry Rules
1. Asia high swept during London/NY session.
2. Price retraces into the 15m supply OB zone.
3. Drop to 1m. Wait for 1m to show buyers in control first (normal OB approach).
4. Wait for 1m sellers to take control: 1m swing low broken (CHoCH bearish) while price is inside the 15m supply zone.
5. Enter short at the resulting 1m OB. Stop: above the 15m supply OB high.
6. Target: Asia low / stacked liquidity target.

## Stop Loss Rules
- Stop above the 15m supply OB high.
- If using LTF entry: stop above the 1m OB high (tighter, better RR).

## Take Profit Rules
- Target: Asia low or next major liquidity pool.
- No fixed RR specified — structural target.

## Risk Management
- 1% risk per trade.
- No explicit RR minimum stated.

## Filters (What Invalidates the Setup)
- Asia high not swept — no setup.
- No 15m supply OB visible between current price and Asia sweep.
- 1m CHoCH does not confirm while price is in the 15m supply zone.

## Timeframe Alignment
- 15m: bias, supply zone identification.
- 1m: CHoCH confirmation + entry.

## Session / Time of Day
- London 2:00–5:00am EST; NY 7:00–10:00am EST.

## Backtesting Notes
- $26K live trade shown (multiple accounts, 1 setup).
- LK mentions "5 different setups" in his system — this is setup #1.
- No backtest stats provided in source.

## Python Implementation Notes
- Supply zone = last bullish OB before bearish expansion on 15m
- "Buyers exhausted" signal: look for OB where bullish candles stopped making higher highs
- Asia high sweep: same detection as other models (`current_high > asia_high`)
- 1m CHoCH within zone: only valid when price is inside the 15m supply zone range
- State machine: `15M_BEARISH → ASIA_HIGH_SWEPT → IN_SUPPLY_ZONE → 1M_CHOCH → ENTRY`

## Open Issues / TODO
- "5 setups" — remaining 4 not defined in this source
- "Sell a bullish daily candle" — for automation, define when intraday short is valid despite daily still printing bullish (threshold: daily candle < X% of its expected range? Or just context-based?)
- Supply zone definition needs algorithmic precision — how many candles, what minimum displacement candle size?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-easiest-structure-strategy\|LK Easiest Structure Strategy]] | Initial page created |
