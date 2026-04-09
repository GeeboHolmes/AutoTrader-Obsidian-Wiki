---
type: source
title: "Copy This 5 Rule SMC Trading Strategy (Backtested Results)"
source_type: video-transcript
author: "Lewis Kelly"
date_published: 2025-10-08
date_ingested: 2026-04-06
original_file: raw/Lewis Kelly/08-10-2025 - Copy This 5 Rule SMC Trading Strategy (Backtested Results).txt
asset: multi
tags: [smc, intraday, backtested, asia-sweep, 5-rules, intraday-bias-model]
---

## Summary
LK presents the "Intraday Bias Model" — his explicitly named best-performing strategy with 4 years of data (2023–2025): 230 trades, 216% return, 33% win rate, 6.23 average win/loss ratio (~10%/month average). The strategy is distilled into 5 named rules. LK frames the 33% win rate as intentional: combined with 6.23 avg RR, the math is highly profitable even at a low frequency.

The model uses 15m structure for bias, strict session time windows (London 2-5am EST, NY 7-10am EST), mandatory Asia/Frankfurt liquidation sweep, 1m CHoCH confirmation, and 5m OB/FVG/inverted FVG entry. LK explicitly notes this is one of 7 variations he has; only the London/Asia variant is shared publicly.

## Key Points
- Named strategy: "Intraday Bias Model" — LK's best performing playbook
- 230 trades 2023-2025, 216% return, 33% win rate, 6.23 avg win/loss
- Only trade inside London (2-5am EST) or New York (7-10am EST) — strict cutoffs
- No trade 2 mins before or after window — e.g. 1:58am or 5:05am are invalid
- Liquidation = Asia high (bearish) or Asia low (bullish) must be swept; Frankfurt counts for London model
- 1m CHoCH must align with 15m bias — not 15m bullish + 1m bullish; need 1m to SHIFT to bullish
- Entry: 5m OB, FVG, or inverted FVG; prefer confluence of multiple zones
- Stop: above high (for shorts) / below low (for longs); no explicit buffer specified

## Trading Rules / Setups Described
1. **Rule 1 — Directional Bias:** 15m external structure. Box method: external swing high to low; anything inside = internal. Ignore internal structure. Bullish = HH/HL; bearish = LH/LL. Wick ≠ BOS.
2. **Rule 2 — Time & Price:** London 2:00–5:00am EST; New York 7:00–10:00am EST. Hard cutoffs — no entry outside windows.
3. **Rule 3 — Liquidation:** Asia session high (bearish bias) or low (bullish bias) must be swept before entry. Frankfurt sweep also qualifies for London model. No sweep = no trade.
4. **Rule 4 — Reversal Confirmation:** 1m timeframe must shift to align with 15m bias. If 15m bearish, wait for 1m to shift bearish (1m CHoCH to downside). This provides the "green light."
5. **Rule 5 — Entry Model:** After 1m shift, go to 5m. Look for OB (last buy-to-sell or sell-to-buy), FVG, or inverted FVG. Prefer areas where multiple zones coincide. Enter from zone; stop above session high (for shorts); target 15m swing low/high.

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/market-structure|Market Structure]], [[concepts/liquidity|Liquidity]], [[concepts/order-blocks|Order Blocks]], [[concepts/fair-value-gap|Fair Value Gap]], [[concepts/break-of-structure|BOS / CHoCH]], [[concepts/killzones|Killzones]], [[concepts/swing-points|Swing Points]]

## Strategies Mentioned
[[strategies/lk-5-rule-intraday-bias|LK 5-Rule Intraday Bias Model]]

## Contradictions / Open Questions
- Results are stated for a specific forex instrument (EUR/USD implied); applicability to crypto needs validation.
- LK has 7 variations of this strategy — only 1 shared here (London/Asia low).

## Quotes
> "This is the strategy that I've used for the past 4 years. 230 trades, 216% return. Win rate of 33%. Average win to loss ratio is 6.23."

> "You can have a trading strategy alone and it will not make you money."
