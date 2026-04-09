---
type: concept
concept_type: execution
aliases: [Killzones, Kill Zone, Trading Sessions, London Open, New York Open, Asian Session]
tags: [smc, sessions, timing, ict]
---

## Definition
Killzones are specific time windows during the trading day when institutional order flow is most active and high-probability setups are most likely to form. Coined by [[entities/ict|ICT]], these windows correspond to the opening hours of major financial centres.

## The Four Killzones (New York Time / EST)

| Killzone | Time (EST) | Characteristics |
|----------|-----------|-----------------|
| **Asian Session** | 20:00 – 00:00 | Low volatility; accumulation phase; sets liquidity for London to raid |
| **London Open** | 02:00 – 05:00 | High volatility; frequently sweeps Asian session high or low before reversing |
| **New York Open** | 07:00 – 10:00 | Highest volume; frequently sweeps London session high or low; major reversals |
| **New York PM** | 13:30 – 16:00 | Lower significance; often continuation of NY AM trend or closing consolidation |

## Why They Matter
Institutions execute large orders at session opens due to maximum liquidity availability. The sequence is typically:
1. **Asian:** Builds a range (accumulation). Liquidity accumulates above and below the Asian range.
2. **London:** Raids one side of the Asian range (inducement sweep) then moves strongly in one direction.
3. **New York:** Raids the opposing London session extreme before continuing or reversing.

This creates a predictable daily pattern: raid one side → reverse → run to the other side.

## Application to Crypto
Crypto markets are 24/7, but price action still respects these time windows because:
- Institutional crypto desks are active during the same hours.
- Algorithmic systems are calibrated to session times.
- BTC/ETH correlation with TradFi assets during US/EU hours.

London and New York opens remain the most reliable killzones for crypto intraday trading.

## Trading Rules
1. Only take high-confidence setups *within* killzone windows.
2. Outside killzones: reduce position size or avoid trading entirely.
3. London open: bias is set by higher timeframe structure; look for Asian range sweep then entry.
4. New York open: most powerful killzone; look for London range sweep then continuation or reversal.
5. **Do not trade** the last 30 minutes before a killzone — wait for the open.

## Python Implementation Notes
- Store all timestamps in UTC; convert to EST/EDT for killzone checks.
- `is_in_killzone(timestamp_utc) -> bool | str`
- Account for DST: EST = UTC-5 (Nov–Mar), EDT = UTC-4 (Mar–Nov). Use `pytz` or `zoneinfo`.
- Killzone filter is a session mask applied to the signal generator — only emit signals when `is_in_killzone() == True`.

## Connections
- Related concepts: [[concepts/liquidity]], [[concepts/inducement]], [[concepts/market-structure]]
- Part of: [[concepts/smart-money-concepts]]
- Source: [[entities/ict]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
