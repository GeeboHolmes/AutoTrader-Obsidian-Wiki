---
type: strategy
status: research
asset_class: multi
timeframes: [daily, 15m, 1m, 5m]
style: intraday
tags: [smc, lk, a-plus, daily-candle, triple-confluence, minimum-rr]
---

## Overview
LK's highest-confluence intraday setup — the "A+ setup." Adds a daily candle character layer on top of the standard 15m + Asia sweep model. Three independent filters must all agree. Minimum 1:5 RR required via LTF CHoCH bonus step. Claimed as LK's "first strategy that ever made me profitable."

## Market Bias (Direction Filter)
Three independent layers must all agree before a trade is valid:
1. **Daily candle character** (highest filter): previous day's candle pattern predicts next day direction.
2. **15m intraday structure:** external swing point sequence (HH/HL or LH/LL).
3. **Asia sweep confirmation:** Asia low swept (bullish) or Asia high swept (bearish) during session window.

## Setup Conditions
1. Read previous day's closed daily candle. Determine character (see rules below). Only trade in that direction.
2. Confirm 15m external structure agrees with daily character direction.
3. During London (2:00–5:00am EST) or NY (7:00–10:00am EST): Asia session low (bullish) or high (bearish) swept.
4. All three layers agree — if any disagree, no trade.

## Entry Rules
**Daily Candle Character Patterns:**
- **Bearish continuation:** previous day closed BELOW the prior day's low → next day bearish.
- **Bullish continuation:** previous day closed ABOVE the prior day's high → next day bullish.
- **Bearish rejection:** previous day wicked ABOVE prior day's high (PDH) then closed back inside the range → next day bearish.
- **Bullish rejection:** previous day wicked BELOW prior day's low (PDL) then closed back inside the range → next day bullish.

**Entry (Bonus Step — required for 1:5 RR):**
1. After all three setup conditions met, drop to 1m.
2. Wait for 1m CHoCH in the direction of the trade.
3. Enter at 5m OB/FVG within the reversal zone.
4. Calculate RR: `rr = (target - entry) / (entry - stop)`. Minimum 1:5.
5. If 1:5 not achievable, do not enter.

## Stop Loss Rules
- Stop at the 1m swing low (longs) or 1m swing high (shorts) at the CHoCH point.
- Same tight stop mechanism as lk-3-rule-sniper — LTF stop enables high RR.

## Take Profit Rules
- Target: 15m structural swing high (longs) or swing low (shorts).
- Minimum 1:5 RR required — if target is too close, skip the trade.
- No explicit partial-take rules specified in source.

## Risk Management
- Minimum RR: 1:5 (same constraint as lk-4-step-proven).
- Higher confluence = fewer setups but higher quality. Expect lower trade frequency than base intraday model.

## Filters (What Invalidates the Setup)
- Daily candle character is ambiguous (no clear rejection or continuation signal) — skip day.
- 15m structure disagrees with daily character — no trade.
- Asia sweep disagrees with daily character direction — no trade.
- All three agree but 1:5 RR not achievable — do not enter.
- Time outside London or NY window — hard reject.

## Timeframe Alignment
- Daily: primary bias filter (candle character)
- 15m: secondary bias confirmation (external structure)
- 1m: entry trigger (CHoCH) and stop placement
- 5m: entry zone (OB/FVG)

## Session / Time of Day
- **London:** 2:00–5:00am EST.
- **New York:** 7:00–10:00am EST.
- Daily candle read: after 5pm EST close of previous day.

## Backtesting Notes
- No explicit backtest data provided in source for this specific variant.
- LK claims this is his "first strategy that made me profitable" — implying consistent profitability, but no numbers given.
- Triple-confluence filter will significantly reduce trade frequency vs the base 5-rule intraday bias model.

## Python Implementation Notes
- Daily candle read: compare `prev_day_close` vs `prev_prev_day_high` and `prev_prev_day_low`
- Rejection pattern: `if prev_day_high > prior_day_high and prev_day_close < prior_day_high: bearish_rejection = True`
- Continuation: `if prev_day_close > prior_day_high: bullish_continuation = True`
- Extend base lk-5-rule-intraday-bias state machine with daily bias pre-filter step
- State machine: `DAILY_BIAS → 15M_BIAS_CONFIRM → WINDOW → SWEEP → CHoCH → RR_CHECK → ENTRY`
- RR check: `tp = next_15m_swing; rr = abs(tp - entry) / abs(entry - stop); if rr < 5: skip`

## Open Issues / TODO
- "Close inside range" for rejection patterns — define threshold (body close? wick close? within body of prior candle?)
- Relationship to lk-5-rule-intraday-bias: this is the same base + one extra filter. Consider whether these should merge or stay separate.
- Validate daily candle patterns on crypto (24-hour market has no true daily session boundary — use 5pm EST or midnight UTC?)

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-3-step-aplus\|LK 3-Step A+ Setup]] | Initial page created |
