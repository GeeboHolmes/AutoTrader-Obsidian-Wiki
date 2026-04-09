---
type: analysis
question: "Which strategies have the strongest evidence base and are most suitable for the crypto bot pipeline?"
date: 2026-04-06
tags: [strategy-selection, crypto, automation, conviction]
---

## Question
Which of the 21 wiki strategies should be prioritised for the bot pipeline, given: (a) crypto-first asset class (BTC, ETH, SOL, DOGE), (b) automated execution, (c) multi-stage testing pipeline (backtest → Karpathy optimization → live)?

## Scoring Criteria
Each strategy is scored across five dimensions (1–3 each):

| Dimension | 1 | 2 | 3 |
|-----------|---|---|---|
| **Evidence** | Claimed, no data | Partial data / few sources | Multiple sources + backtest numbers |
| **Rule precision** | Discretionary / vague | Most rules codeable | Fully mechanical / unambiguous |
| **Crypto suitability** | Forex/session specific | Works on crypto with caveats | Crypto-native or universally applicable |
| **Automation** | Requires human judgement | Mostly automatable | Fully automatable state machine |
| **Signal frequency** | Very rare (swing only) | Moderate | High enough to test meaningfully |

---

## Rankings

### Tier 1 — Highest Priority

#### 1. lk-simple-ob-model | Score: 14/15
[[strategies/lk-simple-ob-model]]

| Dimension | Score | Notes |
|-----------|-------|-------|
| Evidence | 3 | The 3-rule OB framework appears across 10+ sources spanning 2023–2025. Most consistently cited entry model in the entire LK archive. Rule 3 (1m CHoCH inside OB) is referenced in nearly every strategy video as the canonical entry. |
| Rule precision | 3 | Three explicit rules with clear pass/fail logic. The 1m CHoCH has an unambiguous definition (close above recent 1m swing high). Trap check (unswept liquidity) is well-defined. |
| Crypto suitability | 3 | No mandatory session window. Rules are structural (swing points, OBs, liquidity) and apply 24/7 on crypto. Crypto's high volatility may actually produce cleaner OB reactions than forex. |
| Automation | 3 | Fully mechanical state machine: `BIAS → OB_IDENTIFIED → TRAP_CHECK → IN_ZONE → CHoCH → ENTRY`. Every step is binary. |
| Signal frequency | 2 | Requires BOS + retracement + 1m CHoCH confluence — not every hour, but intraday signals on BTC/ETH are common. |

**Start here.** This is the core LK model. The 1m CHoCH inside OB is the single most-cited, most-validated entry mechanism across the entire 77-transcript archive. Every other strategy is essentially a variant or filter on top of this foundation.

**Open issues for implementation:** Define "unswept liquidity" distance threshold. Test OB zone definition (LK-wide range vs ICT-last-candle).

---

#### 2. lk-4-step-proven | Score: 13/15
[[strategies/lk-4-step-proven]]

| Dimension | Score | Notes |
|-----------|-------|-------|
| Evidence | 3 | **Best sample size of any strategy: 577 trades, 2021–2025, 360%+ return.** Consistent profitable months (12–32%). Anti-breakout philosophy validated. |
| Rule precision | 3 | All rules are explicit. The 1:5 minimum RR constraint is mathematical — easily computed. State machine is defined in the wiki page. |
| Crypto suitability | 2 | Requires Asia session range + London/NY sweep. Crypto does exhibit session-based volatility (London/NY opens see higher volume), but 24/7 nature means Asia range is less defined than forex. Requires validation. |
| Automation | 3 | RR calculation is mechanical (scan OB from top to bottom until 1:5 achieved). Asia sweep detection is binary. 1m CHoCH is binary. |
| Signal frequency | 2 | Requires Asia session + session window + sweep + CHoCH confluence. Fewer signals than the plain OB model but each signal is higher-quality. |

**Second priority.** The 577-trade backtest is the strongest data backing of any strategy in the wiki. The 1:5 minimum RR filter is an excellent automation constraint — it eliminates low-quality entries programmatically. The main uncertainty is crypto applicability of the Asia session mechanic.

**Critical validation question:** Does BTC produce a reliable Asia range (8pm–midnight EST) with predictable London sweeps? Test this first before full implementation.

---

#### 3. lk-5-rule-intraday-bias | Score: 12/15
[[strategies/lk-5-rule-intraday-bias]]

| Dimension | Score | Notes |
|-----------|-------|-------|
| Evidence | 3 | LK explicitly calls this his "best-performing strategy." 230 trades, 2023–2025, 216% return, 33% WR, 6.23 avg RR (~10%/month). Not as many trades as lk-4-step-proven but explicitly LK's top-ranked model. |
| Rule precision | 3 | Hard time cutoffs (2–5am, 7–10am EST) are precise. The "Asia sweep mandatory" filter is binary. 5m OB/FVG entry is well-defined. Python implementation notes already written in the wiki. |
| Crypto suitability | 2 | Same session-dependency issue as lk-4-step-proven. Additionally, source states "EUR/USD implied; crypto applicability unvalidated." Hard time cutoffs may reduce signal frequency on crypto. |
| Automation | 2 | Time filter is mechanical. However, "7 variations" exist — only one is documented. Incomplete without the other 6. |
| Signal frequency | 2 | Hard time cutoffs (6 hours/day total) reduce signals considerably. |

**Third priority.** The clearest backtest numbers and LK's own endorsement ("best-performing") give this high conviction. Lower than #2 only because the data is forex-specific and the 7 variations are undocumented.

---

### Tier 2 — Secondary Candidates

#### 4. lk-easy-liquidity | Score: 10/15
[[strategies/lk-easy-liquidity]]

**Notable for:** Reactive design — no pre-bias needed. The strategy determines direction FROM the session sweep. This is automation-friendly: no subjective HTF bias read, just detect which extreme gets swept first. ~50% WR with ~1:4 RR.

**Limitation:** Session-dependent (London). Crypto sessions are softer. The "no bias" design means it will generate signals in both directions across a ranging market — needs a higher-TF filter for crypto.

**Recommended as:** A secondary strategy to run alongside the primary OB model. Easy to implement, provides a different signal type.

---

#### 5. lk-3-rule-sniper | Score: 10/15
[[strategies/lk-3-rule-sniper]]

**Notable for:** The "97% break-even success rate" claim (aggressive BE management after 1m CHoCH confirmed). If validated, this is the most capital-preserving model. Uses the same 1m CHoCH core as the top strategies.

**Limitation:** The 97% BE claim requires independent validation — strong claim with limited data transparency. Essentially a variant of lk-simple-ob-model with a specific BE management overlay.

**Recommended as:** Test the BE management system as a parameter variant within the lk-simple-ob-model backtest.

---

#### 6. lk-3-step-aplus | Score: 9/15
[[strategies/lk-3-step-aplus]]

**Notable for:** Triple confluence requirement (daily candle character + 15m structure + Asia sweep) + minimum 1:5 RR. Very selective — only A+ setups. When conditions are met, conviction is highest.

**Limitation:** Very few signals. The selectivity that makes it high quality also makes it hard to test statistically.

**Recommended as:** Define the triple-confluence filter as a scoring overlay on top of the base OB model — use it to identify A+ trades rather than as a separate strategy.

---

### Tier 3 — Lower Priority for Crypto Automation

| Strategy | Key Issue |
|----------|-----------|
| lk-boring-scalping | Session-range mechanics (London sweeps one Asia extreme); fine for crypto but narrow time window |
| lk-5-step-swing | Swing (4H+); fewer signals, longer hold times — useful later but not for initial testing |
| lk-london-bias-ob | London-specific, largely covered by Tier 1/2 strategies |
| lk-5-step-asia-sweep | Covered by lk-4-step-proven (same mechanic without the 1:5 constraint) |
| lk-daily-bias | Power of Three + candle character — more discretionary elements |
| lk-5-phase-manipulation | Conceptual framework, not directly executable as a standalone bot strategy |
| lk-retail-trap-reversal | Counter-trend reversals — harder to automate reliably |
| lk-advanced-smc-funded | Prop firm rules add complexity irrelevant to spot crypto |
| lk-5-step-rli | Forex only (DXY cross-reference) |
| lk-100k-premium-discount | EUR/USD specific |
| lewis-kelly-smc-intraday | Synthesis/overview — implement its rules via the specific strategies above |
| smc-crypto-intraday | Placeholder page — flesh out using Tier 1 strategies |

---

## Answer

**Recommended pipeline order:**

1. **lk-simple-ob-model** — implement first. Universal rules, best multi-source backing, fully mechanical. This is the core LK entry model that all other strategies build on. Testing it establishes the baseline for all future iterations.

2. **lk-4-step-proven** — implement second. Best backtest data (577 trades). The 1:5 RR filter adds a clean programmable constraint. Primary validation question: does crypto BTC/ETH produce reliable Asia session ranges?

3. **lk-5-rule-intraday-bias** — implement third. LK's stated best model. Most useful once #1 and #2 have established the infrastructure (swing detection, CHoCH, OB detection are all shared components).

4. **lk-easy-liquidity** — run in parallel as a reactive alternative. Different signal logic (sweep-first, then direction) provides a useful comparison signal type.

**Shared infrastructure:** All four top strategies share the same core components: swing point detection, OB identification, liquidity level tracking, and 1m CHoCH detection. Building these as reusable modules enables rapid iteration across all strategies.

---

## Cross-Strategy Pattern (Critical Finding)
The **1m CHoCH inside an OB zone** appears as the entry trigger in: lk-simple-ob-model, lk-4-step-proven, lk-5-rule-intraday-bias, lk-easy-liquidity, lk-3-rule-sniper, lk-3-step-aplus, lk-london-bias-ob, lk-5-step-asia-sweep, and lk-boring-scalping.

**This single mechanism is the LK entry model.** The strategies differ in their filters, session windows, and RR constraints — but all converge on the same entry trigger. This means:
- The CHoCH detector is the single most important component to get right.
- All strategies can share the same entry execution module.
- Backtesting the CHoCH entry in isolation (vs blind OB entry) is the most important first experiment.

---

## Contradictions / Open Questions

1. **Session timing on crypto:** LK's data is forex-derived. Does BTC produce reliable London/NY institutional sweeps? Crypto trading is global 24/7; session effects are real but softer. Requires empirical validation.

2. **OB definition:** LK uses "consolidation range before impulse" (wider than ICT's "last opposing candle"). Test both definitions in backtesting — the choice significantly affects entry price and win rate.

3. **FVG evolution:** June 2024 LK said FVGs = targets only. Oct 2025 LK added FVG entry model. Both are in the wiki. Test FVG entries vs OB entries separately.

4. **Asia range definition for crypto:** 8pm–midnight EST was designed for forex. BTC has no "Asia close." Consider using Binance 4H candle boundaries or defining the range as a rolling window.

---

## Implementation Implications
- Build shared modules first: swing detector, OB detector, liquidity tracker, LTF CHoCH detector.
- Parameterize everything: OB definition (wide vs narrow), swing lookback, CHoCH sensitivity — the Karpathy optimization stage will tune these.
- lk-simple-ob-model should be the first complete script. It exercises all shared components.
- The 1:5 RR filter from lk-4-step-proven is worth adding as an optional parameter to every strategy from the start.

## Follow-up Questions
- Does BTC/ETH produce reliable Asia session ranges? (empirical test needed)
- Which OB definition (LK wide vs ICT narrow) produces higher win rate on crypto?
- What is the win rate of 1m CHoCH entries vs blind OB entries?
- Does funding rate correlate with OB reaction quality on crypto perps?
