---
type: source
title: "The Ultimate Order Block Trading Strategy (Step by step)"
source_type: video-transcript
author: Lewis Kelly
date_published: 2025-07-14
date_ingested: 2026-04-05
original_file: raw/Lewis Kelly/14-07-2025 - The Ultimate Order Block Trading Strategy (Step by step).txt
asset: multi
tags: [order-blocks, smc, lewis-kelly, strategy]
---

## Summary
The definitive LK video on order blocks. Built from two and a half years of backtested data across hundreds of trades. Identifies the three types of order blocks that actually work and the two that consistently fail. Key insight: LTF confirmation transforms any OB into a higher-probability entry and simultaneously increases RR (from e.g. 1:2 → 1:5). The confirmation method removes the guessing about which OB will hold.

## Key Points
- **Three types of OBs that work:** Strong OB, Sweep OB, Confirmed OB
- **Two types that fail:** Counter-trend OBs (trading against structure), Weak OBs (no BOS/structural significance behind them)
- **OBs need context:** An OB without a structural reason (a BOS or liquidity sweep associated with it) will consistently fail
- **LTF confirmation method:** Drop to 1m when price enters any OB. Watch for 1m structure shift. Then drop to 5m for the exact entry from a 5m OB within the 15m OB zone.
- **RR improvement:** LTF confirmation method typically improves RR from 1:2 → 1:4 or 1:5 because entry is tighter
- **Time matters:** London open (1:45–2:00 a.m.) is the primary entry window. Asia session liquidity sets up the entry.

## Trading Rules / Setups Described

**Type 1 — The Strong OB (Best)**
1. Identify structural BOS on 15m.
2. The OB is the range/candle cluster immediately before the BOS move.
3. This OB must be responsible for breaking a significant structural level.
4. Trade this OB when price returns; expect follow-through beyond the opposing liquidity level.

**Type 2 — The Sweep OB**
1. Identify a session liquidity level (Asia high/low, Equal highs/lows, Previous day H/L).
2. Price sweeps that level aggressively.
3. The OB is created at the sweep level — the range/candle before the reversal from the sweep.
4. Enter on return to the OB in the direction of the sweep reversal.
5. This must be aligned with HTF trend.

**Type 3 — Confirmation Method (applies to any OB)**
1. Identify OB (any type) on 15m.
2. When price enters the OB, switch to 1m chart.
3. Wait for 1m to show a CHoCH in the trade direction.
4. Drop to 5m and find the 5m OB/FVG that corresponds to the 1m shift.
5. Enter from the 5m level. Stop below 5m OB low. Target: original 15m target (liquidity).
6. RR benefit: enter much closer to the bottom of the 15m OB = much larger RR.

**Invalid OBs (never trade):**
- Counter-trend OBs (bearish OB in a bullish structure = will get traded through)
- OBs from "meaningless" swing points (internal structure that hasn't broken anything)
- Mitigated OBs (price has already passed through)

## Entities Mentioned
[[entities/lewis-kelly|Lewis Kelly]]

## Concepts Mentioned
[[concepts/order-blocks]], [[concepts/market-structure]], [[concepts/break-of-structure]], [[concepts/liquidity]], [[concepts/fair-value-gap]], [[concepts/killzones]]

## Contradictions / Open Questions
- LK uses "range before expansion" as the OB definition (not just the last opposing candle). This is a wider zone. For bot implementation, define whether to use the full consolidation range or just the last opposing candle.
- "2.5 years of backtested data" mentioned but not quantified. Record any specific win rate numbers if found in other sources.

## Quotes
> "Order blocks alone are meaningless. They have to have context behind them to support them."

> "The only goal that a low or a high has is to take out the previous low or the previous high."

> "You wait for the market to show you which order block is going to work. And so this in of itself will take most types of order blocks and transform them into definitely more profitable versions."
