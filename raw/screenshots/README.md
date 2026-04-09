# Strategy Screenshots

Drop screenshots of real trades into the correct folder. The agent uses these to validate that detectors and strategies match how Louis actually trades.

## Naming Convention

`YYYY-MM-DD_TIMEFRAME_DIRECTION_notes.png`

Examples:
- `2025-01-15_1D_long_fib-618-entry.png`
- `2025-03-02_4H_short_range-sweep-high.png`
- `2024-11-20_1D_long_bos-then-retrace.png`

## What to Include

For each screenshot:
- Make sure the date/time is visible on the chart (so the agent can cross-reference with historical data)
- Mark or note: where you'd enter, where the stop goes, where the target is
- If possible, annotate directly on the chart (TradingView drawing tools)

## Folders

- `fib-smc/` — Fib + SMC trend continuation setups (BOS > retrace > FVG + fib confluence)
- `range/` — Wyckoff accumulation/distribution setups (range formation > liquidity sweep > reversal)
