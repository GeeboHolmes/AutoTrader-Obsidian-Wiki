---
type: analysis
question: "What is the current state of the 10MGFoxAgents strategy pipeline?"
date: 2026-04-08
tags: [pipeline, status, agent-tracking]
---

# Pipeline Status

_Last updated: 2026-04-08 by Claude Code session_

This page is the authoritative record of where each strategy stands in the automated pipeline. Updated by agents at session end via the `obsidian-wiki` MCP server.

---

## Strategy Pipeline

| Strategy | Stage | Script | Last QA | Verdict | Notes |
|---|---|---|---|---|---|
| Simple OB STF v1 | successful | `simple_ob_stf_v1.py` | 2026-04-08 | PASS | Single-TF fallback (1h). 282 OOS trades, 39% WR, $137K PnL, 11.1% DD. Converged after 100 auto-research iterations. |
| Simple OB MTF v1 | successful | `simple_ob_mtf_v1.py` | 2026-04-08 | PASS | Multi-TF (15m/1m). 432 OOS trades, 41.2% WR, $308K agg PnL, PF 3.63, 4/4 pairs profitable. **May not have fully converged** — only 25 auto-research iterations across 4 rounds (5/25 accepted). Revisit for more testing. |
| FVG OTE Continuation v1 | successful | `fvg_ote_continuation_v1.py` | 2026-04-09 | PASS | Single-TF (1h). 170 OOS trades, 65.9% WR, $39.8K PnL, 4.7% DD, PF 4.62, 6/6 pairs profitable. 70 auto-research iterations, converged (10/70 accepted). |

---

## Recent Sessions

### 2026-04-08 — IS/OOS comparison + auto-research continuation

**Operator:** Claude Code (main)

**Tasks completed:**
- Ran full QA harness on LK Simple OB v1 (single-TF fallback, 1h data)
- All 6 pairs profitable OOS. Bias audit PASS. No overfitting flags.
- Aggregate IS: 437 trades, 39.4% WR, $242,816 PnL, 11.9% DD
- Aggregate OOS: 282 trades, 39.0% WR, $137,434 PnL, 11.1% DD
- Ran 50 additional auto-research iterations — 0/50 accepted (params converged)
- Added results matrix persistence (`backtests/results_matrix.json`)
- Built `obsidian-wiki` MCP server connecting 10MGFoxAgents ↔ Obsidian wiki

**Files changed:**
- `backtests/qa_lk_simple_ob_v1.py` — fixed import path, added matrix persistence
- `backtests/results_matrix.json` — NEW, stores IS/OOS per pair per strategy
- `backtests/run_auto_research_continuation.py` — NEW, continues from best params
- `src/mcp_obsidian_wiki.py` — NEW, MCP server for wiki access
- `.mcp.json` — registered obsidian-wiki server

**Decisions:**
- Strategy params converged: HTF_SWING_LOOKBACK=4, HTF_SWING_SIZE=51, DISPLACEMENT_ATR_MULT=1.3448, ATR_PERIOD=10, MAX_OB_LEG_POSITION=1
- 3 params at range boundaries (ATR_PERIOD, SL_BUFFER_PCT, MAX_OB_LEG_POSITION) — not blocking

### 2026-04-08 — MTF strategy QA + promotion to successful

**Operator:** Claude Code (main) + 10mg-qa agent

**Tasks completed:**
- Implemented detection caching for auto-research (precomputed_smc passthrough)
- Ran 4 rounds of auto-research (25 iterations total) across all 4 pairs (BTC, ETH, SOL, BNB)
- Round 1 (10 iter, BTC only): 6/50 accepted, but ETH collapsed to 4 trades — MIN_RR and LTF_CHOCH_HOLD_BARS over-fitted to BTC
- Round 2 (10 iter, all pairs): 2/10 accepted. ETH recovered to 84 trades/$84.7K. 4/4 profitable.
- Round 3 (5 iter, all pairs): 1/5 accepted. MIN_RR lowered to 2.45, agg PnL $188K.
- Round 4 (5 iter, all pairs): 2/5 accepted. MAX_OB_LEG_POSITION=4 was the big mover (+63% PnL). Final agg PnL $308K.
- Promoted MTF strategy to `Agents/Strategies/successful/`

**Best params (5 changes from default):**
- HTF_SWING_LOOKBACK: 5->4, HTF_SWING_SIZE: 50->51, LTF_SWING_LOOKBACK: 3->2, MAX_OB_LEG_POSITION: 2->4, MIN_RR: 3.0->2.545

**Flags:**
- DD degrades 50% IS->OOS (10.5%->15.8%), driven by SOL at 15.8%
- MAX_OB_LEG_POSITION, SL_BUFFER_PCT, LTF_SWING_LOOKBACK at range boundaries
- Only 25 total iterations — strategy may not have fully converged. Revisit for more auto-research rounds.

**Next steps:**
- Revisit MTF strategy for additional auto-research rounds (convergence not confirmed)
- Consider widening param ranges for boundary params
- Paper trading deployment
- Strategist can research next strategy from wiki's conviction ranking
