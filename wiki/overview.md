# Overview

_The evolving synthesis of everything in this wiki. Updated after every ingest._

---

## Project Vision
Build an AI-powered, automated Python trading bot that executes Smart Money Concepts strategies with high accuracy across cryptocurrency markets (primary) and eventually forex, gold, and equities. The goal: profitable, emotion-free, systematic trading that captures institutional order flow.

---

## Current Thesis
The foundation of the bot is the [[concepts/smart-money-concepts|SMC framework]] as systematised by [[entities/ict|ICT (Inner Circle Trader)]]. The hypothesis is:

> Institutional participants (banks, funds, market makers) engineer predictable liquidity raids before genuine directional moves. These raids are detectable algorithmically. A strategy that waits for the raid, confirms the reversal, and enters at institutional supply/demand zones ([[concepts/order-blocks|Order Blocks]] / [[concepts/fair-value-gap|FVGs]]) in the [[concepts/premium-discount|discount/premium]] zone during [[concepts/killzones|active killzones]] will produce a positive-expectancy edge.

The edge comes from three sources:
1. **Structural alignment** — trading with institutional bias, not against it.
2. **Precision entry** — entering after confirmation, not before, reducing stop-outs.
3. **Automation** — removing emotional execution errors; taking every qualified setup consistently.

---

## Open Questions
- What is the statistically validated win rate and RR of each core SMC setup (OB entry vs FVG entry vs OTE)?
- Which timeframe combination produces the best backtest results for crypto intraday?
- How does SMC perform on BTC vs ETH vs altcoins — is there a significant difference?
- Can a machine learning component (signal filter/classifier) improve the raw SMC signal quality?
- What is the optimal stop loss methodology — ATR-based, structure-based, or OB extreme?
- How to handle high-impact news events algorithmically?

---

## Key Entities
| Entity | What it is |
|--------|-----------|
| [[entities/ict\|ICT]] | Primary SMC educator — source of most core concept definitions |
| [[entities/python-bot\|Python Bot]] | The trading bot being built — project entity |

---

## Key Concepts
| Concept | Role |
|---------|------|
| [[concepts/smart-money-concepts\|Smart Money Concepts]] | The overarching methodology |
| [[concepts/market-structure\|Market Structure]] | Determines directional bias |
| [[concepts/break-of-structure\|BOS / CHoCH]] | Trend confirmation and reversal signals |
| [[concepts/liquidity\|Liquidity]] | Targets and reversal triggers |
| [[concepts/order-blocks\|Order Blocks]] | Primary entry zones |
| [[concepts/fair-value-gap\|Fair Value Gaps]] | Secondary entry zones / targets |
| [[concepts/displacement\|Displacement]] | Confirms institutional involvement |
| [[concepts/premium-discount\|Premium / Discount]] | Entry quality filter |
| [[concepts/inducement\|Inducement]] | Manipulation before the real move |
| [[concepts/mitigation\|Mitigation]] | OB invalidation tracking |
| [[concepts/swing-points\|Swing Points]] | Foundation of structure |
| [[concepts/killzones\|Killzones]] | Session timing filter |
| [[concepts/power-of-three\|Power of Three]] | Session-level macro pattern |
| [[concepts/risk-management\|Risk Management]] | Position sizing and capital protection |
| [[concepts/backtesting\|Backtesting]] | Development and validation methodology |

---

## Active Strategies
| Strategy | Status | Asset | Style |
|----------|--------|-------|-------|
| [[strategies/smc-crypto-intraday\|SMC Crypto Intraday]] | Research | Crypto (BTC/ETH) | Intraday |

---

## Contradictions

### OB Definition: ICT vs Lewis Kelly
- **ICT:** OB = the **last opposing candle** before the displacement move (e.g. last bearish candle before bullish impulse).
- **Lewis Kelly:** OB = the **full consolidation range** before the aggressive expansion (wider zone).
- **Impact on bot:** LK's definition is easier to detect algorithmically (range = consolidation). ICT's is more precise but harder to define. Both approaches should be backtested.
- **Status:** Unresolved. Test both definitions.

### 15m Bias vs 4H/Daily Bias
- **Lewis Kelly:** Intraday bias is determined by 15m structure (external swing range). Daily candle character confirms.
- **ICT:** Bias typically starts at 4H or Daily structure, then drops to entry timeframe.
- **Impact:** LK's approach produces more trades (shorter-term bias changes more often). ICT's produces fewer, higher-timeframe setups.
- **Status:** For a daily trading bot, LK's 15m-based approach is more suitable. For swing trading, will need to implement HTF bias later.

### Liquidity Sweep Confirmation: Wick vs Close
- **Lewis Kelly:** Explicitly states wicks are liquidations, NOT structure breaks. Candle close is required for BOS/CHoCH.
- **Some ICT content:** Wick sweeps of liquidity are sufficient to consider liquidity "swept."
- **Impact on bot:** Bot must be consistent — implement `sweep_confirmation = 'close'` as default, with `'wick'` as a parameter option to test.
- **Status:** LK's "close only" rule is stricter and likely reduces false signals. Use as default.

---

## Roadmap
1. **Phase 1 (Now):** Research SMC strategies → populate wiki → define precise rules.
2. **Phase 2:** Code core SMC components (structure, OBs, FVGs, liquidity detection) in Python.
3. **Phase 3:** Backtest on historical BTC/USDT data → iterate rules.
4. **Phase 4:** Paper trade on live data → validate.
5. **Phase 5:** Live trading with small capital → scale up.
6. **Phase 6:** Expand to forex, gold, equities with adapted strategies.
