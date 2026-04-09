---
type: entity
entity_type: strategy
aliases: [The Bot, AutoTrader, Trading Bot]
tags: [project, python, architecture]
---

## Overview
This is the core project: an AI-powered, automated trading bot written in Python. It executes [[concepts/smart-money-concepts|SMC]]-based strategies on cryptocurrency markets with planned expansion to forex, gold, and equities.

## Design Goals
- **Emotion-free:** All decisions are rule-based and executed automatically.
- **Accurate:** Only takes high-confluence setups; quality over quantity.
- **Modular:** Strategy logic is separate from execution, data, and risk management.
- **Auditable:** Every signal and trade is logged with full reasoning.
- **Expandable:** Adding a new asset class or strategy should not require rewriting core components.

## Planned Architecture
```
┌─────────────────────────────────────┐
│           Data Layer                │
│  CCXT / Exchange API → OHLCV data   │
│  Multi-timeframe aggregation        │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│         Analysis Layer              │
│  Market Structure detection         │
│  OB / FVG / Liquidity detection     │
│  Bias engine (HTF → LTF)            │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│         Strategy Layer              │
│  Signal generation (entry/exit)     │
│  Setup validation & scoring         │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│       Risk Management Layer         │
│  Position sizing                    │
│  Daily loss limit                   │
│  Drawdown circuit breaker           │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│        Execution Layer              │
│  Order management (CCXT)            │
│  Stop loss / TP management          │
│  Order status tracking              │
└───────────────┬─────────────────────┘
                │
┌───────────────▼─────────────────────┐
│         Logging / Monitoring        │
│  Trade journal                      │
│  Telegram / Discord alerts          │
│  Performance dashboard              │
└─────────────────────────────────────┘
```

## Current Development Status
- Stage: Research / Planning
- Asset: Cryptocurrency (BTC/USDT, ETH/USDT primary)
- Exchange: TBD (Binance or Bybit)
- Strategy: SMC-based (see [[strategies/smc-crypto-intraday]])

## Key Technology Choices (TBD / Under Research)
| Component | Options |
|-----------|---------|
| Exchange connectivity | CCXT |
| Data storage | SQLite / TimescaleDB / CSV |
| Backtesting | vectorbt / backtrader / custom |
| Notifications | Telegram Bot API |
| Deployment | Local / VPS / Cloud |
| AI/ML component | TBD — signal filtering, regime detection |

## Connections
- Strategy: [[strategies/smc-crypto-intraday]]
- Core concepts: [[concepts/smart-money-concepts]], [[concepts/risk-management]], [[concepts/backtesting]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Created with project architecture overview |
