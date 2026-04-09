---
type: concept
concept_type: backtesting
aliases: [Backtesting, Backtest, Walk-Forward, Forward Test, Paper Trading]
tags: [backtesting, development, python]
---

## Definition
Backtesting is the process of simulating a trading strategy on historical data to evaluate its performance before deploying real capital. For an automated bot, rigorous backtesting is the core development loop.

## The Development Pipeline
```
Research → Code → Backtest → Optimise → Walk-Forward → Paper Trade → Live (small) → Live (full)
```
Each stage gates the next. A strategy that fails backtesting does not proceed.

## Backtesting Approaches

| Approach | Description | Use case |
|----------|-------------|----------|
| **Vectorised** | NumPy/pandas array operations; no loop; very fast | Initial research, parameter sweeps |
| **Event-driven** | Simulates real execution; handles orders, fills, slippage | Pre-live validation |
| **Walk-forward** | Train on in-sample, test on out-of-sample rolling window | Overfitting detection |

## Key Metrics (Target Values)
| Metric | Target |
|--------|--------|
| Total return | Context-dependent, but consistent |
| Max drawdown | < 15–20% |
| Sharpe ratio | > 1.5 |
| Profit factor | > 1.5 |
| Win rate | Consistent with RR ratio expectations |
| Number of trades | > 100 (statistical significance) |

## Overfitting Warning Signs
- Strategy performs well only on one specific time period.
- Too many parameters relative to number of trades.
- Performance degrades sharply on out-of-sample data.
- Sharpe ratio much higher in-sample than out-of-sample.

## Python Tools

| Tool | Use case |
|------|----------|
| `vectorbt` | Fast vectorised backtesting; NumPy-based; great for initial research |
| `backtrader` | Event-driven; full order simulation; slower but more realistic |
| `backtesting.py` | Lightweight event-driven; simple API |
| `freqtrade` | Full bot framework with built-in backtesting; good for crypto |
| Custom | Full control; required for complex SMC logic that existing frameworks can't handle |

## Data Requirements for SMC Backtesting
- OHLCV data at minimum (1m and 5m for LTF entries, 1H/4H for HTF bias).
- Multiple timeframes loaded simultaneously.
- Data must be clean: no gaps, adjusted for exchange downtime.
- For crypto: Binance/Bybit historical data via CCXT or direct API.

## Common Pitfalls
- **Look-ahead bias:** Using future data in indicator calculations. Always check that rolling calculations use only past data.
- **Survivorship bias:** Testing only assets that are still trading today.
- **Slippage:** Assume 0.05–0.1% slippage per trade for realistic crypto simulation.
- **Fees:** Always include exchange fees. Binance spot: 0.1%. Futures (with BNB discount): 0.02%.
- **Swing point confirmation:** In backtesting, swing points are fully known. In live trading, they require N future candles to confirm — this delay must be simulated correctly.

## Python Implementation Notes
- Separate data pipeline from strategy logic — strategy should receive a clean DataFrame.
- Use `pandas` DataFrames with MultiIndex for multi-timeframe data.
- Log every signal and trade for post-hoc analysis.
- Build a `BacktestEngine` class that handles portfolio tracking, order fill simulation, and metric calculation.
- Version your strategy parameters alongside backtesting results.

## Connections
- Related concepts: [[concepts/risk-management]]
- Key tools: [[entities/vectorbt]], [[entities/backtrader]], [[entities/freqtrade]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
