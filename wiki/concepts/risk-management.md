---
type: concept
concept_type: risk-management
aliases: [Risk Management, RM, Position Sizing, RR, Risk-Reward]
tags: [risk, execution, bot-architecture]
---

## Definition
Risk management is the systematic set of rules governing how much capital is exposed per trade, per day, and per drawdown period. For an automated bot, risk management is non-negotiable code — not a guideline.

## Core Metrics

| Metric | Definition |
|--------|-----------|
| **Risk per trade (R)** | Fixed % of account or fixed dollar amount risked on one trade |
| **Risk-Reward ratio (RR)** | Target profit / risk amount. Minimum 1:2 recommended for SMC; 1:3+ preferred |
| **Win rate** | % of trades that hit target. At 1:2 RR, need >34% win rate to be profitable |
| **Expectancy** | `(Win% × Avg Win) - (Loss% × Avg Loss)`. Must be positive |
| **Max daily loss** | Hard stop on daily losses — bot stops trading for the day |
| **Max drawdown** | Hard stop on drawdown — bot stops trading until manual review |
| **Sharpe ratio** | `(Return - Risk-free rate) / StdDev of returns`. Target > 1.5 |

## Position Sizing Formula
```python
# Fixed fractional position sizing
account_balance = get_account_balance()
risk_pct = 0.01  # 1% risk per trade
stop_loss_distance_pct = abs(entry_price - stop_loss) / entry_price

position_size = (account_balance * risk_pct) / stop_loss_distance_pct
```

For crypto (with leverage):
```python
position_size_with_leverage = position_size * leverage
# Cap leverage at a sensible maximum (e.g. 5x for intraday)
```

## Rules for the Bot
1. **Never risk more than 1–2% per trade** on a live account.
2. **Daily loss limit:** If down 3–5% on the day, stop trading. No revenge trades.
3. **Drawdown limit:** If account drops X% from peak, halt bot and review.
4. **Minimum RR:** Only take trades with a projected RR of at least 1:2. Prefer 1:3.
5. **No averaging down:** If a trade is losing, the stop loss is the exit. No adding to losing positions.
6. **Correlation limit:** If multiple pairs are in trade simultaneously, ensure they are not highly correlated (avoids compounding risk).

## Backtesting Metrics to Track
- Total return %
- Max drawdown %
- Sharpe ratio
- Sortino ratio
- Win rate %
- Average RR of winning trades
- Profit factor: `gross profit / gross loss` (target > 1.5)
- Number of trades (avoid overfitting to small N)

## Python Implementation Notes
- Risk calculations must happen *before* order submission — never hardcode size.
- Position size must be rounded to exchange-valid lot sizes (`math.floor` to avoid over-ordering).
- Implement a `RiskManager` class with `calculate_position_size(entry, stop, balance, risk_pct)`.
- Daily loss tracker: store daily PnL in state; compare against daily limit before each order.
- Drawdown tracker: track `peak_balance`; compute `drawdown = (peak - current) / peak`.

## Connections
- Related concepts: [[concepts/backtesting]]
- Applied in: all strategy pages

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
