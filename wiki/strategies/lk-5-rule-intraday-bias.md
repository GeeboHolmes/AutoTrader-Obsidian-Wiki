---
type: strategy
status: research
asset_class: multi
timeframes: [15m, 1m, 5m]
style: intraday
tags: [smc, lk, intraday-bias-model, backtested, asia-sweep]
---

## Overview
LK's explicitly named best-performing strategy: the "Intraday Bias Model." Uses 15m external structure for direction, strict session windows, mandatory Asia/Frankfurt liquidation sweep, 1m CHoCH, and 5m OB/FVG entry. Backtested 230 trades over 2023–2025: 216% return, 33% win rate, 6.23 avg RR (~10%/month).

## Market Bias (Direction Filter)
- **15m external structure (box method):** External swing highs and lows only. HH/HL = bullish. LH/LL = bearish. Internal structure (anything within the current swing range) is ignored.
- **Wick ≠ BOS:** A wick through a swing point is a liquidation, not a break of structure.
- Bias must be confirmed on 15m before any other rule is evaluated.

## Setup Conditions
1. 15m bias is clearly bullish or bearish (external structure defined).
2. Current time is within London (2:00–5:00am EST) or New York (7:00–10:00am EST). Hard cutoffs — no entries at 1:58am or 5:05am.
3. Asia session low (bullish bias) OR Asia session high (bearish bias) has been swept. Frankfurt sweep counts for the London model.

## Entry Rules
1. After Setup Conditions 1–3 are met, drop to 1m.
2. Wait for 1m market structure to shift in the direction of the 15m bias (1m CHoCH). If 15m bullish, 1m must shift bullish. If 15m bearish, 1m must shift bearish.
3. After 1m CHoCH confirmed, go to 5m.
4. Identify entry zone: 5m OB (last sell-to-buy candle for longs / last buy-to-sell for shorts), FVG (three-candle imbalance), or inverted FVG.
5. Prefer zones where multiple types coincide (OB + FVG at same level).
6. Enter when price returns to the zone.

## Stop Loss Rules
- Stop above the session high (for shorts) or below the session low (for longs) — the most recent extreme within the current session window.
- Alternative: stop above the 5m OB high (shorts) / below OB low (longs) if session extreme is too far for acceptable RR.

## Take Profit Rules
- Target: 15m structural swing low (bearish) or swing high (bullish).
- No partial-take or scale-out rules specified in source — full position to target.

## Risk Management
- Risk per trade: not specified in source; apply standard bot risk model.
- 33% win rate requires minimum 1:2 RR to be profitable; source data shows 6.23 avg RR — significantly above minimum.

## Filters (What Invalidates the Setup)
- Time outside London or NY window — hard reject.
- No Asia/Frankfurt sweep — do not enter, even if CHoCH appears.
- 1m structure shifts SAME direction as 15m (both trending, no reversal) — the 1m must SHIFT (CHoCH), not confirm.
- Setup forms within 2 minutes of session open or close boundaries.

## Timeframe Alignment
- 15m: directional bias (external structure)
- 1m: reversal confirmation (CHoCH)
- 5m: entry zone (OB/FVG)

## Session / Time of Day
- **London:** 2:00–5:00am EST (hard cutoffs)
- **New York:** 7:00–10:00am EST (hard cutoffs)
- Asia range forms 8:00pm–midnight EST (for sweep detection)
- Frankfurt: 2:00am–2:30am EST (sweeps here count for London model)

## Backtesting Notes
- Source data: 230 trades, 2023–2025, ~2 years.
- 216% return, 33% win rate, 6.23 avg win/loss ratio.
- Approximately 10%/month average return.
- LK has 7 variations of this model; only London/Asia variant is publicly shared.
- Instrument: EUR/USD implied (forex); crypto applicability unvalidated.

## Python Implementation Notes
- Asia session: `asia_open = previous 8pm EST candle index; asia_close = midnight EST candle index`
- `asia_low = candles[asia_open:asia_close]['low'].min()`
- `asia_high = candles[asia_open:asia_close]['high'].max()`
- Sweep detection: `current_low < asia_low` (bullish sweep) or `current_high > asia_high` (bearish sweep)
- Time filter: `if not (0200 <= current_time_est <= 0500 or 0700 <= current_time_est <= 1000): skip`
- State machine: `BIAS_SET → WINDOW_OPEN → SWEEP_DETECTED → CHoCH_CONFIRMED → IN_ZONE → ENTRY`
- CHoCH: track last 1m swing high/low; BOS of that level in opposite direction = shift
- OB detection: last candle before expansion move — body direction flips, next candle is large

## Open Issues / TODO
- Define "external" swing point algorithmically (box method implementation)
- Validate on crypto (BTC, ETH) — session dynamics differ; Asia session may not produce same range
- Clarify stop level preference: session extreme vs OB boundary
- LK's 7 variations — NY/PDL/PDH variants worth investigating

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-5-rule-intraday-bias\|LK 5-Rule Intraday Bias]] | Initial page created |
