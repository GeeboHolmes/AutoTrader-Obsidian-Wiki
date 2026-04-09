---
type: strategy
status: research
asset_class: forex
timeframes: [4h, 15m, 1m]
style: intraday
tags: [manipulation-model, range-liquidation-initiation, institutional-mechanics]
---

## Overview
A five-phase institutional mechanics model: Range → Liquidation → Initiation → Mitigation → Continuation. The strategy is built on reading which phase the market is in and only entering during the Mitigation phase (Phase 4). Asia session = Phase 1 (Range). London = Phases 2–3 (Liquidation + Initiation). Entry = Phase 4 (when price returns to the OB after the manipulation). Exit target = Phase 5 liquidity level.

## Market Bias (Direction Filter)
- Established by observing Phase 2 (Liquidation) direction.
- If London sweeps Asia Low (SSL) first → institutions are positioning to go LONG → bias = bullish.
- If London sweeps Asia High (BSL) first → bias = bearish.
- Phase 3 (Initiation) must confirm: the opposing structural level must break (CHoCH/BOS) before entering.

## Setup Conditions
1. **Phase 1 — Range:** Asia session consolidates and creates BSL + SSL. Mark Asia High and Asia Low.
2. **Phase 2 — Liquidation:** London sweeps one side (e.g., Asia Low for bullish). Must close BACK INSIDE (wick sweep, not a run).
3. **Phase 3 — Initiation:** After the sweep, a high (bullish scenario) inside the recent range gets broken — this is the CHoCH/BOS confirming institutional intent.
4. **Phase 4 — Mitigation (entry zone):** Price returns to the OB level — the sell-to-buy candle area where the manipulation happened (the final bearish range/candle before the aggressive buy impulse that broke structure in Phase 3).
5. **Phase 5 — Continuation:** Price expands toward the pre-created BSL (equal highs, Asia High, PDH).

## Entry Rules
1. Phase 3 CHoCH confirmed (structural high broken on 15m).
2. Price retraces to the Phase 4 Mitigation OB.
3. Drop to 1m. Observe 1m bearish structure at the mitigation zone.
4. Wait for 1m CHoCH in the bullish direction (lower low → higher high).
5. Enter long from the 1m OB at the CHoCH level.

## Stop Loss Rules
- Below the 1m structural low (tight stop, inside the OB zone).
- Alternatively: below the Phase 4 OB low (wider stop).

## Take Profit Rules
- Primary: Next BSL (Asia High, PDH, equal highs) = Phase 5 target.
- Trail stop to break even after 2R.
- Optional partial close at 3R (50% of position), let rest run to full target.

## Risk Management
- Risk per trade: 1% of account.
- Max 2 trades per session.
- Only trade during London (Phase 2–4 typically occurs in London session).

## Filters (What Invalidates the Setup)
- Phase 2 sweep NOT confirmed (price runs through the level, closing beyond it → it's a run not a sweep → different model).
- Phase 3 CHoCH fails (no structural level broken after the sweep) → wait for re-setup.
- Price does not retrace to the OB (Phase 4 skipped) — this happens occasionally; do not chase entry.
- High-impact news between Phases 2 and 4 → stand aside.

## Timeframe Alignment
- 4H: establish weekly intent (is the 4H trending in the expected direction?).
- 15m: identify the 5 phases, confirm CHoCH in Phase 3, mark OB in Phase 4.
- 1m: entry confirmation inside the Phase 4 OB.

## Session / Time of Day
- Phase 1 (Range): 8pm–midnight EST (Asia).
- Phase 2–3 (Liquidation + Initiation): 2am–5am EST (London).
- Phase 4 (Mitigation/Entry): occurs within 1–3 hours of Phase 3, typically 3am–7am EST.
- Phase 5 (Continuation): London/NY session (5am–noon EST).

## Backtesting Notes
- Win rate and RR not stated explicitly for this model. Related models: lk-easy-liquidity (49–51% WR), lk-5-rule-intraday-bias (33% WR, 6.23 RR). This model sits between the two.
- Test parameter: how often does price skip Phase 4 (no mitigation) and go straight to Phase 5?
- Test parameter: what percentage of Phase 3 CHoCH setups retrace to the OB within 2 sessions?

## Python Implementation Notes
```python
# Phase detection state machine
class MarketPhase(Enum):
    RANGE = 1        # Asia session (accumulation)
    LIQUIDATION = 2  # SSL or BSL swept during London
    INITIATION = 3   # CHoCH after sweep
    MITIGATION = 4   # Price returns to OB (ENTRY ZONE)
    CONTINUATION = 5 # Expansion phase

# Phase 1: Asia range
asia_high = candles_in_session(ASIA_START, ASIA_END)['high'].max()
asia_low = candles_in_session(ASIA_START, ASIA_END)['low'].min()

# Phase 2: Sweep detection
ssl_swept = any(c.low < asia_low and c.close > asia_low for c in london_candles)
bsl_swept = any(c.high > asia_high and c.close < asia_high for c in london_candles)

# Phase 3: CHoCH after sweep
if ssl_swept:
    choch = detect_bullish_choch(m15_candles_after_sweep)
    bias = 'LONG' if choch else None

# Phase 4: Mitigation (price returns to OB)
if bias == 'LONG' and choch:
    ob_zone = detect_sell_to_buy_ob(m15_candles_pre_choch)
    in_mitigation = price_in_zone(current_price, ob_zone)
    if in_mitigation:
        entry_signal = detect_1m_choch(m1_candles)
```

## Open Issues / TODO
- Formalise the "OB zone" definition for Phase 4: is it the single sell-to-buy candle, or the entire consolidation range before the impulse?
- Determine how to detect when Phase 4 is skipped (price continues directly to Phase 5 without retracing).
- Add 4H bias alignment as a filter to improve WR.
- Phase 5 target precision: how to identify the most relevant BSL when multiple equal highs exist?

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/lk-5-phase-manipulation]] | Initial creation |
