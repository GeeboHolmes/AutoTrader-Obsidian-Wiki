---
type: strategy
status: research
asset_class: multi
timeframes: [4H, 15m, 1m]
style: intraday
tags: [london-session, asia-sweep, liquidity, bias, lk]
---

## Overview
A 4-step London session strategy anchored around the Asia session sweep. Asia creates a range with defined highs/lows (liquidity). London regularly sweeps one side of that range. The strategy determines in advance which side London will target (via HTF bias), waits for that liquidity sweep to occur, then enters from an OB with 1m CHoCH confirmation. Specifically designed to solve both "wrong bias" and "wrong timing" in a single systematic framework.

## Market Bias (Direction Filter)
- **HTF structure (4H or Daily):** Determine whether the market is bullish or bearish.
- Bullish bias = expect London to sweep Asia session **low** → look for longs after sweep.
- Bearish bias = expect London to sweep Asia session **high** → look for shorts after sweep.

## Setup Conditions
1. Asia session (roughly 8pm–midnight EST) forms a clear high and low.
2. HTF (4H) bias is established (bullish or bearish).
3. Time is within the London session window: 2am–5am EST.
4. Bias points toward a specific side of the Asia range being swept.

## Entry Rules
1. Wait for price to trade through the Asia session low (for bullish setups) or Asia session high (for bearish).
2. Do NOT enter before or during the sweep — the sweep is the trigger, not the entry.
3. After the sweep, look for a pro-trend OB (sell-to-buy for bullish).
4. Drop to 1m. Wait for **1m bullish CHoCH** (1m structure flips from bearish to bullish inside or just above the OB).
5. Enter long at the CHoCH level. Stop below 1m swing low.

## Stop Loss Rules
- Stop below the 1m swing low after the CHoCH event.
- This will typically be close to or below the swept Asia session low.

## Take Profit Rules
- **TP1:** 5R — close 50%+ of position here.
- **TP2:** 15m swing high (next structural high of the active leg).

## Risk Management
- Risk per trade: 1–2% of account.
- Do not add to position after entry — manage TP1/TP2 as planned.
- Max 1 trade per London session.

## Filters (What Invalidates the Setup)
- HTF bias is unclear or ranging — no trade.
- Time window has passed (after 5am EST) — wait for New York session only if setup still valid.
- Asia session low has already been swept before London opens — the setup trigger may be compromised.
- No valid OB available within the post-sweep area.
- 1m CHoCH does not occur within the OB zone.

## Timeframe Alignment
| Timeframe | Role |
|-----------|------|
| 4H / Daily | HTF bias |
| 15m | OB identification, structural context |
| 1m | Entry confirmation (CHoCH) and stop |

## Session / Time of Day
- **Trade window:** 2am–5am EST (London session open).
- **Setup builder:** Asia session (8pm–midnight EST) creates the range to be swept.

## Backtesting Notes
- Key test: hit rate of Asia session low being swept in the London window when 4H is bullish.
- Key test: what % of Asia low sweeps produce a valid OB + 1m CHoCH signal vs false sweeps with no follow-through?

## Python Implementation Notes
- Requires: Asia session range detection (high/low between 8pm and midnight EST), HTF structure state, time filter (2am–5am EST), sweep detection, OB detection, 1m CHoCH detection.
- Logic: `if bullish_bias and london_window and asia_low_swept: find_ob(); if ltf_choch_bullish_inside_ob: enter()`

## Open Issues / TODO
- Clarify exact Asia session time window — is it 8pm–midnight EST? 10pm–2am EST? Confirm from LK session killzone data.
- Compare with lk-5-step-asia-sweep (02-02-2026): are these the same strategy at different levels of detail? The 5-step version adds a daily bias step and a wick entry refinement.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-losing-trades-london]] | Strategy page created from 04-03-2025 source |
