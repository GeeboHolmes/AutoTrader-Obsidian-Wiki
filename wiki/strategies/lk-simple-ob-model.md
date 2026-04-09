---
type: strategy
status: research
asset_class: multi
timeframes: [4H, 15m, 1m]
style: intraday
tags: [order-blocks, confirmation, rr-enhancement, lk]
---

## Overview
A three-rule OB entry model from LK (December 2025) designed to eliminate the two most common OB failures and dramatically improve RR through LTF order flow confirmation. Follow the money → avoid the traps → wait for order flow confirmation. The core innovation is Rule 3: using a 1m CHoCH inside the OB zone to tighten entry placement from "entire OB zone" to "specific 1m structure shift," converting a 1:2 trade into a 1:9+.

## Market Bias (Direction Filter)
- **External structure only:** Use external swing highs/lows to determine HTF trend.
- **Only trade pro-trend OBs.** A bearish OB in a bullish external range is counter-trend and will fail regardless of other signals.
- Confirm on 4H or 15m timeframe before looking for entries.

## Setup Conditions
1. External structure confirms bullish (HH/HL) or bearish (LH/LL) trend.
2. Valid pro-trend OB identified within the current price leg (sell-to-buy for bullish trend).
3. **Trap check:** No unswept liquidity (equal lows/highs, Asia H/L, PDH/PDL, London H/L) between current price and the OB. If unswept liquidity exists, wait for it to be swept first — the OB will be blown through to collect it.
4. Price retraces into the OB zone.

## Entry Rules
1. When price enters the validated OB zone, drop to 1m timeframe.
2. Observe 1m structure — it should be bearish (pullback phase) as price enters the zone.
3. Wait for **1m bullish CHoCH**: the 1m bearish structure breaks when price closes above a recent 1m swing high.
4. Enter long at the 1m CHoCH level (the close of the confirmation candle, or a limit at the 1m swing high level).
5. If 1m structure does not shift within the OB zone, do not enter — the OB is failing.

## Stop Loss Rules
- Stop below the 1m swing low established at or after the CHoCH event.
- NOT below the OB bottom — the 1m-based stop is tighter by design (this is the source of the RR improvement).

## Take Profit Rules
- Target: next structural high of the active 15m or 4H leg.
- Optional partial: consider partial close at 3R or 5R if the target is far.

## Risk Management
- Risk per trade: follow standard position sizing (1–2% of account).
- The tighter stop (1m-based vs OB-based) means position size can be slightly larger for the same dollar risk — take advantage of this.
- Do not enter if the 1m CHoCH is more than 50% through the OB zone.

## Filters (What Invalidates the Setup)
- OB is counter-trend (bearish OB in bullish external structure).
- Unswept liquidity between price and OB = trap, skip it.
- 1m structure shifts but immediately reverses (multiple failed CHoCH = OB failing, do not chase).
- OB already mitigated (price previously fully overlapped the zone).
- Price has travelled more than 2 OBs into the current price leg (probability decreases with each leg extension — see market-structure.md probability rule).

## Timeframe Alignment
| Timeframe | Role |
|-----------|------|
| 4H / Daily | HTF bias and external structure |
| 15m | OB identification, structural leg context |
| 1m | Entry confirmation (CHoCH) and stop placement |

## Session / Time of Day
- No explicit restriction, but best results during London (2am–10am EST) and New York AM sessions when institutional activity is highest.

## Backtesting Notes
- RR math: assuming OB zone = 10 pips, 1m CHoCH within zone = stop of 3 pips. Same target (30 pips) = RR jumps from 1:3 to 1:10. Real-world numbers will vary but the principle is structurally sound.
- Key metric to test: win rate of 1m CHoCH entries vs OB-level blind entries.

## Python Implementation Notes
- Requires: swing detection, external structure state, OB detection, liquidity level tracking, LTF CHoCH detection within OB zone.
- Logic: `if price_in_ob_zone and ltf_choch_bullish: enter_long(at=choch_level, stop=ltf_swing_low)`
- Must handle "trap rejection": if liquidity level exists between current price and OB, suppress entry signal until that liquidity is swept.

## Open Issues / TODO
- Define exact "unswept liquidity" distance threshold — how close to the OB does it have to be to count as a trap?
- Test: does the 1m CHoCH inside OB outperform blind OB entries at the zone top? What is the win rate differential?
- Confirm: does this strategy require a minimum HTF context (e.g. must be inside a premium/discount zone)?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-simple-ob-2025]] | Strategy page created from 01-12-2025 source |
