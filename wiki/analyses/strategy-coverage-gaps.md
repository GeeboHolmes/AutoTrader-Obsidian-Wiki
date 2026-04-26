---
type: analysis
created: 2026-04-26
last_audited: 2026-04-26 (S52 + S53 promoted; S54 attempted + failed — sweep-reversal pattern confirmed broken)
status: living-doc
---

# Strategy Coverage Gaps — SMC Concept Map

This page maps the 14 canonical SMC strategy concepts to the strategies currently in `Agents/Strategies/successful/`. Use this as the **first reference** when picking what to research next — fill genuine gaps before iterating on covered concepts.

**Last audited:** 2026-04-26 (against `Agents/strategy_index.md` and the `successful/` folder).

---

## Coverage table

| # | Concept | Status | Covering Strategy(ies) |
|---|---|---|---|
| 1 | BOS Retest Continuation | ✅ COVERED | `bos_retest_continuation_v2` |
| 2 | OB Retest Continuation | ✅ COVERED (10×) | `ob_retest_continuation_v1`, `simple_ob_stf_v1`, `simple_ob_mtf_v1`, `adx_gate_ob_v1`, `roc_align_ob_v1`, `btc_confirm_ob_v1`, `squeeze_ob_v1`, `stoch_ob_v2`, `candle_char_ob_v1`, `supertrend_regime_ob_v1` |
| 3 | Breaker Block Setup | ✅ COVERED | `breaker_block_v1` (S52) — first OB-lifecycle strategy in book. Promoted 2026-04-26 with $330k OOS PnL / PF 2.74 / 0/6 stress collapses. AR plateau-confirmed across 75 iter / 2 runs. |
| 4 | Liquidity Grab → Continuation | ✅ COVERED | `sweep_gated_ob_v1`, `sweep_fvg_ote_v1` |
| 5 | **Liquidity Grab → Reversal** | ❌ MISSING (structural failure pattern) | **S54 `liq_grab_reversal_v1` ATTEMPTED 2026-04-26 → FAILED initial QA** (29.5% WR, -$1,056 OOS, IS also negative). Joins S6 (27.9% WR FAILED) — 2/2 sweep-reversal-with-OB-at-CHoCH-origin attempts have failed. **Pattern: sweep-then-CONTINUE works in this book (S22 59.9% WR PASSED), sweep-then-REVERSE does not.** Future attempts MUST use fundamentally different entry mechanic (FVG instead of OB, HTF bias gate, momentum-confirmed sweep) — same OB-at-CHoCH-origin design is NOT recommended for v3. |
| 6 | CHoCH + OB Entry | ✅ COVERED | `choch_ob_reversal_v2` (S53) — promoted 2026-04-26. $87.7k OOS PnL / PF 2.97 / 6/6 profitable / 1/6 stress collapse (BTC risk-accepted). v1 (S3) failed on signal scarcity; v2 fixed via custom internal-swing detector + removal of redundant trend-confirmation gate. |
| 7 | FVG Retest Entry | ✅ COVERED | `fvg_ote_continuation_v1`, `h4_fvg_retest_v1` |
| 8 | OB + FVG Confluence | ✅ COVERED | `ob_fvg_confluence_tsl_v2` |
| 9 | **Inducement → Sweep → Entry** | ❌ MISSING | never attempted (despite [[concepts/inducement|inducement]] page existing in wiki) |
| 10 | Range Liquidity Sweep Reversal | ⚠️ PARTIAL | `vwap_mr_v1` covers mean-reversion in range regime via VWAP SD bands, but NOT pure range-extreme sweep + rejection |
| 11 | Premium/Discount Continuation | ✅ COVERED | `sweep_fvg_ote_v1` (OTE 0.618-0.786), `sd_fib_confluence_v1` (golden pocket 0.5-0.618). Note: `sweep_fvg_discount_v1` deep-discount variant FAIL'd 2026-04-26 |
| 12 | Multi-Timeframe OB Alignment | ✅ COVERED (5×) | `simple_ob_mtf_v1`, `h4_fvg_retest_v1`, `pdl_retest_v1`, `candle_char_ob_v1`, `stacked_ob_v1` |
| 13 | **Mitigation Block Continuation** | ❌ MISSING | never attempted as a distinct mechanic (mitigation = unmitigated zone return for rebalance, distinct from OB retest) |
| 14 | **Double Liquidity Sweep (SSL+BSL)** | ❌ MISSING | `double_sweep_ob_v1` ATTEMPTED → in `failed/` |

---

## Priority order for strategist research

1. **Mitigation Block Continuation** (#13) — distinct from OB retest. Mitigation = price returns to rebalance an *unmitigated* zone before continuing the trend. No code in book attempts this mechanic. Wiki: [[concepts/mitigation|mitigation]]. **Now top priority after S54 failure (2026-04-26) deferred #5.**
2. ~~**Liquidity Grab → Reversal** (#5)~~ — DEFERRED 2026-04-26 after S54 v1 failed initial QA. The OB-at-CHoCH-origin variant of #5 is a confirmed structural failure pattern (S6 + S54). Future v2/v3 must use fundamentally different mechanic (FVG entry, HTF bias gate, momentum-confirmed sweep). Not a top priority until a novel-enough angle is proposed.
3. **Range-Sweep Reversal** (#10) — true range high/low sweep + reversal mechanic, complementing VWAP MR's regime-driven approach. Identify range → wait for sweep → CHoCH → reverse to opposite extreme.
4. **Double Sweep v2** (#14) — review `Agents/Strategies/failed/double_sweep_ob_v1.py` failure mode first. Often appears before big moves; concept is sound, execution likely the issue.
5. **Inducement** (#9) — fake-structure-trap-before-sweep. Wiki: [[concepts/inducement|inducement]]. Mechanism is fuzzier than other concepts — harder to formalise but high novelty.

**Design rule reinforced 2026-04-26 (S53 split):** one strategy per concept. If a strategy is pitched as "covers concepts X and Y", split into separate specialists. Bundling adds optional/disabled code paths that confuse AR optimization and dilute concept coverage claims.

---

## How to use this page

- **Strategist:** check this page BEFORE starting research on a new strategy. Fill a missing concept first; only iterate on a covered concept if there's a specific edge angle (regime gate, indicator confluence, MTF refinement) you want to test.
- **When promoting a strategy:** update the table above with the new file name. If a concept moves from MISSING → COVERED, change the status emoji and update `last_audited`.
- **When a strategy fails:** if it was the only attempt at a concept, the concept stays MISSING with a note pointing to the failed file (so the next strategist sees the prior attempt + can review the failure mode).
- **Partial coverage** (#10) means the niche has *something* but a pure implementation of the canonical concept is missing — these are softer gaps than ❌ MISSING but worth filling for completeness.

---

## Cross-references

- `Agents/strategy_index.md` — canonical roster (every strategy ever proposed, with status + key results)
- `Agents/strategy_archive.md` — append-only historical record (read by humans, not agents — too large)
- `Agents/strategy_successful.md` — append-only detailed specs of promoted strategies (read by humans for deep-dive)
- `Agents/Strategies/failed/` — failed attempts; ALWAYS read the relevant file before re-attempting a missing concept

---

## Audit history

| Date | Auditor | Concepts covered | Concepts missing | Notes |
|---|---|---|---|---|
| 2026-04-26 | User + orchestrator | 7 | 5 (#3 in pipeline), partial #10 | Initial audit. S33 sweep_fvg_discount FAIL'd same day; S52 breaker_block_v1 entered pipeline as response to #3 gap. |
| 2026-04-26 | Orchestrator | 8 | 4 missing (#5/#6/#9/#13/#14), partial #10 | S52 breaker_block_v1 PROMOTED (#3 ✅). Next priority remains #5/#6 CHoCH+OB Reversal v2. |
| 2026-04-26 | Orchestrator | 9 | 4 missing (#5/#9/#13/#14), partial #10 | S53 choch_ob_reversal_v2 PROMOTED (#6 ✅) with BTC stress risk-accepted. Originally pitched as covering #5+#6; user split scope to one-strategy-per-concept. **Next priority: #5 (Liquidity Grab → Reversal) — dedicated S54 specialist with mandatory sweep precondition.** |
| 2026-04-26 | Orchestrator | 9 | 4 missing (#5/#9/#13/#14), partial #10 | S54 liq_grab_reversal_v1 ATTEMPTED → FAILED initial QA (29.5% WR, both IS and OOS negative). **Joins S6 as 2nd sweep-reversal failure** — pattern confirmed: sweep+OB-at-CHoCH-origin doesn't work in 1H crypto. #5 stays MISSING with structural-failure flag. **Next priority: #13 (Mitigation Block Continuation)** — distinct from OB retest; no prior attempt. |
