---
type: strategy
status: research
asset_class: multi
timeframes: [Daily, 15m, 1m, 5m]
style: intraday
tags: [smc, lewis-kelly, intraday, order-blocks, liquidity]
---

## Overview
Lewis Kelly's core intraday SMC strategy, synthesised from his complete body of work. A systematic four-step process using market structure bias (15m), order blocks as entry zones, liquidity conditions as filters, and 1m/5m confirmation for entries. Targets 1:3–1:5 RR intraday setups during London and New York sessions.

## Core Philosophy
- Trade only with institutional order flow — identified by market structure.
- Never enter before opposing liquidity is swept — waiting for the sweep is the filter.
- Use lower timeframe structure shift (1m CHoCH) as confirmation — not price hitting the zone alone.
- OBs only work in context: must be aligned with structure, associated with a BOS or liquidity sweep.

---

## Market Bias (Direction Filter)

### Daily Bias
- Read last 4–5 daily candles.
- 4+ consecutive bearish daily candles → bearish continuation bias for today.
- Mark daily FVGs and IFVGs as weekly directional targets.
- **Daily candle character during session:**
  - Daily candle moving bullish (counter-trend in bearish market) = manipulation phase → look for shorts.
  - Daily candle at extreme low (bearish, aggressively extended) = mean reversion likely → look for longs.

### 15m Structure Bias
- Mark external swing high and swing low on 15m.
- Bullish: price making HH and HL. Bearish: price making LH and LL.
- **CHoCH** on 15m = first sign of potential reversal — wait for confirmation before switching bias.
- **BOS** on 15m = trend continuation confirmed.
- Rule: Only use **external** structure. Internal BOS/CHoCH is entry timing only, not bias.
- Rule: Wick breaks do NOT count as BOS/CHoCH. Require candle body close beyond the level.

---

## Setup Conditions
All of the following must be satisfied before looking for an entry:

1. **Bias is clear:** 15m external structure is unambiguously bullish or bearish (not ranging).
2. **OB identified:** An unmitigated, valid OB exists in the discount zone (for longs) or premium zone (for shorts).
   - Valid OB = associated with a BOS event (Strong OB) OR preceded by a liquidity sweep (Sweep OB).
   - Invalid OBs: counter-trend, no structural significance, already mitigated.
3. **Liquidity to take swept:** The opposing-side session liquidity has been swept:
   - For longs: Asia low or London low swept.
   - For shorts: Asia high or London high swept.
4. **Liquidity to target exists:** Unswept opposing-side liquidity exists above (longs) or below (shorts):
   - PDH, session high, EQH for longs. PDL, session low, EQL for shorts.
5. **Session window:** Currently inside London (02:00–05:00 EST) or New York (07:00–10:00 EST) killzone.

---

## Entry Rules

**Standard Entry:**
1. Price enters the OB zone on 15m.
2. Switch to 1m timeframe.
3. Wait for 1m structure to shift in direction of trade (bearish 1m → bullish CHoCH for longs).
4. Switch to 5m timeframe.
5. Identify 5m OB or FVG at the same price level.
6. Place limit order at the 5m OB open/FVG top. (Do not chase with market order.)

**Alternative Entry (2-candle pullback):**
1. After 1m CHoCH, watch for a 2-candle counter-trend pullback on 1m.
2. When that pullback breaks (1m makes a new high for longs), enter.
3. Use for faster entry when RR is very high and OB is clear.

---

## Stop Loss Rules
- **Primary:** Below the lowest wick of the 5m OB used for entry (+ small buffer: 0.1% of price, or half the spread).
- **Maximum:** Below the 15m OB low (if 5m entry stops out, the 15m OB is the ultimate invalidation level).
- **Rule:** If price closes a 15m candle body through the 15m OB low, the trade setup is invalid — exit if still in trade.

---

## Take Profit Rules
- **TP1:** First identifiable liquidity level (closest unswept session high/low, EQH/EQL).
  - Close 50% of position at TP1.
- **TP2:** PDH/PDL or major session liquidity level.
  - Close 25% at TP2.
- **TP3:** Weekly/major liquidity if in strongly trending market.
  - Trail remaining 25% with stop at breakeven or below previous structure.
- **Minimum target:** Must offer at least 1:2 RR from entry to TP1 before taking trade. Prefer 1:3.

---

## Risk Management
- Risk per trade: 1% of account balance (see [[concepts/risk-management]])
- Maximum daily loss: 3% of account (bot halts for the day)
- Maximum drawdown circuit breaker: 10% from peak (bot halts, requires manual review)
- Maximum concurrent trades: 2 (on different pairs, not correlated)
- Minimum RR: 1:2 (prefer 1:3)

---

## Filters (What Invalidates the Setup)

**Hard invalidations — do NOT take the trade:**
- OB has already been mitigated (price closed through it).
- No liquidity to target in the trade direction.
- Opposing-side session liquidity has NOT been swept (you'd be the liquidity).
- Outside of London or New York killzone window.
- Major economic news event in the next 15 minutes (check economic calendar).
- 15m structure is in conflict (ranging, no clear swing range).

**Soft warnings — reduce size or require extra confirmation:**
- OB has been partially mitigated (touched but not closed through).
- Setup is counter to the daily candle direction (mean reversion vs trend continuation).
- Multiple OBs in the zone without a clear best choice.

---

## Timeframe Alignment

| Timeframe | Purpose |
|-----------|---------|
| Daily | Macro bias (4–5 candle trend read) + PDH/PDL targets |
| 15m | Primary analysis — external structure, OB identification, session context |
| 1m | Entry confirmation — CHoCH or 2-candle pullback break |
| 5m | Entry precision — OB/FVG at the 1m confirmation level |

---

## Session / Time of Day
- **Primary sessions:** London Open (02:00–05:00 EST) and New York Open (07:00–10:00 EST)
- **Pre-session prep:** 30–45 minutes before each session open. Mark Asia high/low, PDH/PDL, identify bias and OBs.
- **Avoid:** Asian session (accumulation, no entries), Friday afternoon (low liquidity), within 15 min before major news.
- **Session sequence (typical):** Asian session builds range → London sweeps one side of Asia range → NY sweeps London extreme or continues.

---

## Backtesting Notes
*(To be populated)*
- Primary pair: BTC/USDT (crypto, 24/7, but respects session times)
- Secondary: EUR/USD, GBP/USD (LK primarily uses forex — validate on crypto separately)
- Time period: minimum 3 months of intraday data at 1m, 5m, 15m, Daily
- Key parameters to test: lookback for swing points (N), OB definition (full range vs last candle), 3-candle pullback threshold, RR minimum filter

---

## Python Implementation Notes
```
State machine:
  IDLE
  → [Session opens] → ANALYZING
  → [Bias confirmed + OB found + liquidity conditions met] → SETUP_ACTIVE
  → [Price enters OB] → AWAITING_CONFIRMATION  
  → [1m CHoCH + 5m entry identified] → ORDER_PLACED
  → [Price fills order] → IN_TRADE
  → [TP hit or SL hit] → TRADE_CLOSED → IDLE

Key modules needed:
  - MarketStructureAnalyzer(timeframe, lookback_n)
  - OrderBlockDetector(definition='range'|'last_candle', strength='strong'|'sweep'|'any')
  - LiquidityTracker(session_times, levels=['PDH','PDL','Asia','London'])
  - LTFConfirmation(timeframe='1m', method='choch'|'two_candle')
  - EntryExecutor(timeframe='5m', entry_type='ob'|'fvg')
  - RiskManager(risk_pct=0.01, daily_limit=0.03, drawdown_limit=0.10)
  - KillzoneFilter(sessions=['london','newyork'], timezone='US/Eastern')
```

---

## Open Issues / TODO
- [ ] Define exact N parameter for swing point detection on 15m (test 3, 5, 7 candles each side).
- [ ] Define OB zone: full consolidation range vs last opposing candle vs first-to-last candle of range.
- [ ] Define valid OB confirmation: BOS-associated vs sweep-associated vs LTF-confirmed separately — test each.
- [ ] Define "session liquidity swept": requires close below, or wick-only sweep sufficient?
- [ ] Define 1m CHoCH: candle close beyond swing point, or wick?
- [ ] Test 3-candle pullback rule quantitatively: does it improve win rate vs no filter?
- [ ] Test on BTC/USDT: do SMC session-based strategies work as well as on forex?
- [ ] Define economic news filter: use an API (e.g. ForexFactory) or hardcode major events.
- [ ] Validate mean reversion trade separately as a distinct strategy.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | [[sources/lk-ultimate-smc-guide-2026]], [[sources/lk-market-structure-full-course]], [[sources/lk-ultimate-order-block-2025]], [[sources/lk-liquidity-masterclass]], [[sources/lk-top-down-analysis]], [[sources/lk-ultimate-smc-course-2024]], [[sources/lk-structure-ob-profits-2025]], [[sources/lk-supply-demand-guide-2026]] | Created from 8 Lewis Kelly source transcripts |
