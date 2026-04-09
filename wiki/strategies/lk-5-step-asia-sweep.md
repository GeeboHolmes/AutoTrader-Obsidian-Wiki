---
type: strategy
status: research
asset_class: multi
timeframes: [15m, 5m, 1m]
style: intraday
tags: [smc, asia-sweep, lk, session-based, liquidity-sweep]
---

## Overview
Lewis Kelly's 5-step intraday strategy built around the Asia session as a predictable liquidity pool. Institutional participants take out Asia lows (for longs) or highs (for shorts) before the London reversal. The strategy waits for that sweep as a mandatory filter, then enters from a 5m OB with 1m CHoCH confirmation. Source: [[sources/lk-5-step-asia-sweep|02-02-2026 transcript]].

## Market Bias (Direction Filter)
1. Use 15m external market structure to determine bias.
2. **Bullish:** Most recent 15m structure has put in a higher high — wait for longs.
3. **Bearish:** Most recent 15m structure has put in a lower low — wait for shorts.
4. Only trade in direction of the current 15m external structure.
5. Do not trade during Asian session (accumulation window only).

## Setup Conditions
1. 15m bias is established (clear HH/HL or LH/LL sequence on external swings).
2. Asia session (8:00pm–midnight EST) has formed a range — note the Asia low (for longs) or Asia high (for shorts).
3. London session (2:00am EST) or New York session has begun.
4. Asia low (bullish) or Asia high (bearish) has been swept — **mandatory**. No entry without this.
5. A 5m OB exists in the expected reversal zone.

## Entry Rules
1. After Asia sweep, drop to 1m timeframe.
2. Wait for 1m CHoCH in the trade direction (for longs: 1m forms a higher high after sequence of lower highs/lower lows).
3. Identify the 5m OB at the reversal zone — the final bearish range (for longs) immediately before the aggressive bullish expansion.
4. Enter at the 5m OB high (for longs). Stop at the 5m OB low.
5. Do not enter if the next valid entry level doesn't provide at least enough RR to the liquidity target.

## Stop Loss Rules
- **Long:** Below the 5m OB low.
- **Short:** Above the 5m OB high.
- No buffer specified in source — use OB extreme as the stop.

## Take Profit Rules
- Target the nearest significant liquidity: session highs/lows (PDH/PDL), equal highs/lows, or trendline liquidity.
- Target must be pre-existing resting orders — identifiable as a previous swing high/low or equal level cluster.

## Risk Management
- Risk per trade: default 1% (see [[concepts/risk-management]])
- No explicit minimum RR stated in this source (see [[strategies/lk-4-step-proven]] for 1:5 minimum variant)
- Max daily loss: 3% (project standard)

## Filters (What Invalidates the Setup)
- Asia low NOT swept before entry attempt → wait.
- 15m bias unclear (ranging without clear HH/HL or LH/LL) → no trade.
- Entry attempted outside London or New York session windows → skip.
- OB has been previously mitigated ([[concepts/mitigation]]).
- 1m CHoCH has not occurred → no confirmation, no entry.

## Timeframe Alignment
| Timeframe | Purpose |
|-----------|---------|
| 15m | Directional bias (external swing structure only) |
| 5m | OB identification; entry zone |
| 1m | Confirmation (CHoCH — trigger to act) |

## Session / Time of Day
- **Asia session:** 8:00pm–midnight EST — sets liquidity range. Do not trade.
- **London session:** 2:00am EST — primary trading window (green box).
- **New York session:** secondary trading window (orange box).
- Avoid trading if Asia session has not yet formed and closed.

## Backtesting Notes
*(To be populated after backtesting runs.)*

## Python Implementation Notes
- Session boxes required: Asia (20:00–00:00 EST), London (02:00–05:00 EST), NY (07:00–10:00 EST).
- Asia range: `asia_high = max(high[session_open:session_close])`, `asia_low = min(low[session_open:session_close])`.
- Asia sweep detection: `price < asia_low` (for longs) — check if any candle low has closed below during London/NY window.
- OB detection: find last bearish candle (or range) before the first aggressive bullish candle (displacement) in the reversal zone.
- Entry: OB high. Stop: OB low.
- CHoCH detection on 1m: sequence of LH/LL then first HH (higher high forms above last swing high).
- State machine: `BIAS_SET → ASIA_FORMED → SWEEP_DETECTED → IN_ZONE → CHoCH_CONFIRMED → ENTRY`

## Open Issues / TODO
- [ ] Validate Asia session window on crypto (UTC offsets vary by exchange — confirm EST vs UTC mapping).
- [ ] Define "strong high" programmatically: most recent swing high that caused a break of previous swing low.
- [ ] Test: does the Asia sweep filter materially improve win rate vs entries without the sweep?
- [ ] Entry at OB high vs OB midpoint vs candle open — LK specifies OB high; test both.
- [ ] Determine minimum RR threshold for this model (source is silent on this; see lk-4-step-proven for 1:5 variant).
- [ ] New York variant: same rules or different OB/confirmation criteria?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | [[sources/lk-5-step-asia-sweep\|02-02-2026 transcript]] | Initial page created from source |
