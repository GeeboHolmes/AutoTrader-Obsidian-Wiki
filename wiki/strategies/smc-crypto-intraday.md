---
type: strategy
status: research
asset_class: crypto
timeframes: [1H, 15m, 5m, 1m]
style: intraday
tags: [smc, crypto, intraday, btc, eth]
---

## Overview
An intraday Smart Money Concepts strategy for BTC/USDT and ETH/USDT. Uses HTF (4H/1H) bias determination and LTF (15m/5m) entry execution during London and New York killzones. Core thesis: align with institutional order flow by reading market structure, targeting liquidity, and entering from high-confluence OB/FVG zones in the discount.

## Market Bias (Direction Filter)
*To be defined from source research. Placeholder framework:*
1. **4H structure:** Determine bullish or bearish bias using HH/HL or LH/LL sequence.
2. **1H structure:** Confirm alignment with 4H bias.
3. If 4H bullish: look for long entries only (or very high-conviction shorts counter-trend with tighter rules).
4. **Session filter:** Only trade during [[concepts/killzones|London Open (02:00–05:00 EST) or New York Open (07:00–10:00 EST)]].

## Setup Conditions
*To be populated from source material. Current working draft:*
1. 4H bias is clear (not ranging/consolidating).
2. Price has swept a significant liquidity level ([[concepts/liquidity|BSL or SSL]]) on the 1H or 4H.
3. A [[concepts/break-of-structure|CHoCH]] has formed on the 15m in the direction of 4H bias following the sweep.
4. An unmitigated [[concepts/order-blocks|OB]] or [[concepts/fair-value-gap|FVG]] exists in the [[concepts/premium-discount|discount zone]] (for longs) or premium zone (for shorts).

## Entry Rules
*To be defined. Placeholder:*
1. Price returns to the OB/FVG zone identified in setup conditions.
2. A 5m or 1m [[concepts/break-of-structure|CHoCH]] forms within the zone (LTF confirmation).
3. Enter at the next candle open after CHoCH confirmation.

## Stop Loss Rules
*To be defined:*
- Below the OB low (for longs) or above the OB high (for shorts).
- Buffer: TBD (e.g. 0.1% of entry price, or half a spread).

## Take Profit Rules
*To be defined:*
- TP1: Nearest liquidity level (previous swing high for longs).
- TP2: Next HTF liquidity level.
- Partial exit: 50% at TP1, trail remainder.

## Risk Management
- Risk per trade: 1% of account (see [[concepts/risk-management]])
- Minimum RR: 1:2
- Max daily loss: 3% of account
- Max concurrent trades: 2

## Filters (What Invalidates the Setup)
*To be populated from research:*
- Major economic news event in the next 30 minutes.
- BTC correlation breakdown (if trading alts).
- OB has been previously mitigated ([[concepts/mitigation]]).
- 4H and 1H structure are in conflict.

## Timeframe Alignment
- **4H:** Bias direction
- **1H:** Structure confirmation + setup context
- **15m:** Entry setup (CHoCH, OB/FVG identification)
- **5m / 1m:** Entry precision (LTF CHoCH for trigger)

## Session / Time of Day
- Primary: London Open + New York Open ([[concepts/killzones]])
- Avoid: Asian session (accumulation, low probability)
- Avoid: Friday afternoon (reduced liquidity)

## Backtesting Notes
*To be populated after backtesting runs.*

## Python Implementation Notes
*To be built out as development progresses:*
- Multi-timeframe data required: 4H, 1H, 15m, 5m (at minimum).
- State machine: `IDLE → BIASED → SETUP_FOUND → IN_TRADE → CLOSED`
- Each timeframe analysis runs independently and feeds the bias/setup state.
- Signal is only emitted when all conditions across all timeframes are satisfied simultaneously.

## Lewis Kelly Rules Applied

After ingesting 8 Lewis Kelly transcripts, the core rules for this strategy can now be derived from [[strategies/lewis-kelly-smc-intraday]]. The LK framework maps onto this crypto strategy as follows:

- **Bias:** Daily candle character (4+ bearish days = bearish bias) + 15m external structure
- **OB:** Use "Strong OB" (BOS-associated) or "Sweep OB" (preceded by session liquidity sweep)
- **Liquidity filter:** LTT² — sweep must occur before entry; target liquidity must exist
- **Entry:** 1m CHoCH → 5m OB/FVG entry
- **Sessions:** London and New York killzones apply to crypto (institutional desks active, algo systems calibrated to sessions)

## Open Issues / TODO
- [ ] Validate session timing on BTC/USDT: do London/NY opens produce reliable liquidity sweeps on crypto?
- [ ] Test LK swing point detection (3-candle pullback rule) vs standard N-candle lookback.
- [ ] Determine OB definition for crypto: full range vs last opposing candle (test both).
- [ ] Define economic news filter for crypto (FOMC, CPI have major BTC impact).
- [ ] Determine if BTC correlation affects altcoin entries — add correlation check as filter.
- [ ] Test FVG fill rate on BTC/USDT vs EUR/USD — crypto may have different characteristics.
- [ ] Research if funding rate (perpetual futures) should be an additional filter.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created from domain knowledge |
| 2026-04-05 | [[strategies/lewis-kelly-smc-intraday]] | Updated with LK-derived rules from 8 source transcripts |
