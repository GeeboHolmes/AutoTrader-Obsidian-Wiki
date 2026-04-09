---
type: concept
concept_type: smc
aliases: [PO3, Power of Three, AMD, Accumulation Manipulation Distribution]
tags: [smc, ict, session-structure, macro-pattern]
---

## Definition
Power of Three (PO3), also called AMD (Accumulation → Manipulation → Distribution), is an ICT concept describing the three-phase structure of any significant price move — from daily candles to individual sessions to intraday moves. Every candle and every session follows this same three-act structure.

## The Three Phases

### 1. Accumulation
- Price consolidates in a tight range.
- Institutions quietly build their position.
- Retail traders see a "range" with no clear direction.
- Low volatility, overlapping candles.

### 2. Manipulation
- A false move in the *opposite* direction of the eventual distribution.
- Trips retail stop losses; fills institutional orders.
- This is the [[concepts/inducement|inducement/liquidity grab]].
- Often: a wick below a support level (for a bullish PO3) or above a resistance level (for bearish).

### 3. Distribution
- The real move — strong, impulsive [[concepts/displacement|displacement]] in the true direction.
- Retail traders who were stopped out are now wrong again.
- Institutions distribute (sell into strength or buy into weakness).

## Visual Pattern
```
Bearish PO3 (Daily candle example):
Open → accumulate near open → spike UP (manipulation, traps longs) → distribute DOWN → Close near low

Bullish PO3:
Open → accumulate near open → spike DOWN (manipulation, traps shorts) → distribute UP → Close near high
```

A bearish daily candle in a downtrend will: open, briefly push above the open (manipulation), then distribute downward and close near the low — leaving a wick above.

## Application
- **Daily candle:** The daily open is key. Manipulation typically happens in the London session; distribution in the NY session (or vice versa).
- **Session-level:** Asian session = accumulation; London = manipulation; New York = distribution.
- **Entry timing:** Enter *after* the manipulation phase (after the liquidity grab), in the direction of distribution.
- **Do not enter during accumulation** — no directional edge yet.
- **Do not chase during distribution** — look for a pullback to an OB/FVG.

## Connection to Killzones
PO3 maps directly to [[concepts/killzones|Killzones]]:
- Asian = Accumulation
- London = Manipulation
- New York = Distribution (or New York creates a second manipulation and London's move is the real one — context-dependent)

## Python Implementation Notes
- PO3 is pattern-level; harder to code than structural concepts.
- Proxy detection: identify accumulation (low ATR range), then a sharp spike against the range (manipulation), then displacement in the opposite direction.
- Daily PO3: track `daily_open`; measure intraday move above and below the open; classify phases.
- For backtesting, label candles/sessions with PO3 phase in hindsight; train a classifier.

## Connections
- Related concepts: [[concepts/inducement]], [[concepts/displacement]], [[concepts/killzones]], [[concepts/liquidity]]
- Part of: [[concepts/smart-money-concepts]]
- Source: [[entities/ict]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-05 | Initial setup | Stub created |
