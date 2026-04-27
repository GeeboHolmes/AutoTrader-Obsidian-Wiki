---
type: analysis
created: 2026-04-26
last_audited: 2026-04-27 (S58 PROMOTED — fills LAST green-field concept #9; coverage 11/14, all remaining missing concepts are deferred-failed-v1)
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
| 9 | Inducement → Sweep → Entry | ✅ COVERED | `inducement_sweep_v1` (S58) — promoted 2026-04-27. **Last green-field SMC concept covered.** $10.7k OOS / PF 4.41 / 6/6 profitable / **0/6 stress collapses (no carve-outs)**. AR ext STRICTLY CONVERGED (first of session). Inducement classification = shallow-retrace counter-trend swing (< 0.618 of prior BOS impulse) — distinguishes "retail trap" from "legitimate pullback." AR multi-objective rescue resolved 3 initial-QA hard fails simultaneously. |
| 10 | Range Liquidity Sweep Reversal | ⚠️ PARTIAL (v1 attempted) | `vwap_mr_v1` (S49) covers regime-driven mean-reversion via VWAP SD bands. **`range_sweep_reversal_v1` (S57) ATTEMPTED 2026-04-27 → FAILED** initial QA (18 OOS trades, gate 50) + AR scarcity-rescue (0/50 accepts). Edge real on rare setups (PF 2.92 on 18t / 0/6 stress) but does NOT scale — AR best loosening (MIN_RANGE_TOUCHES 2→1) lifted to 119t but PF crashed 1.43 + 3/6 stress collapses. **Diagnosis: classic scarcity-trap.** Future v2 must use fundamentally different range-detection (Bollinger / Donchian / ATR-band) — pure swing-cluster horizontal levels are below 1H crypto trade-count viability. |
| 11 | Premium/Discount Continuation | ✅ COVERED | `sweep_fvg_ote_v1` (OTE 0.618-0.786), `sd_fib_confluence_v1` (golden pocket 0.5-0.618). Note: `sweep_fvg_discount_v1` deep-discount variant FAIL'd 2026-04-26 |
| 12 | Multi-Timeframe OB Alignment | ✅ COVERED (5×) | `simple_ob_mtf_v1`, `h4_fvg_retest_v1`, `pdl_retest_v1`, `candle_char_ob_v1`, `stacked_ob_v1` |
| 13 | **Mitigation Block Continuation** | ❌ MISSING (v1 attempted) | **S55 `mitigation_block_continuation_v1` ATTEMPTED 2026-04-27 → FAILED initial QA** (PF 1.13, OOS DD 29.7%, IS DD 35.2% — both breach 25% cap). Used unfilled FVG as mitigation zone with three-state ledger. **Diagnosis: mitigation-state tracking is necessary-but-not-sufficient for edge** — concept takes any FVG in BOS impulse, no secondary selectivity. Future v2 must layer OTE / premium-discount / HTF-bias filter on top of the mitigation tracking, OR find a non-FVG mitigation definition that is intrinsically more selective. Adding OTE would essentially duplicate S7 — be careful with v2 design. |
| 14 | Double Liquidity Sweep (SSL+BSL) | ✅ COVERED | `double_sweep_v2` (S56) — promoted 2026-04-27. **First strategy in book to require BOTH-side sweeps within window.** $50.3k OOS / 5/6 canonical stress pass + 8/11 extended OK / BNB borderline risk-accepted. v1 (S27) was hidden S22 duplicate (same-side sweeps); v2 is truly novel. AR iter #17 stress-improv selected over AR best after extended-pair sweep showed iter #17 generalises better. |

---

## Priority order for strategist research

**Active queue (top of stack first):**

**🎯 ALL GREEN-FIELD CONCEPTS COVERED 2026-04-27.** No new concept to attempt — only deferred-failed-v1 retries remain. Future research must either:
- Find genuinely novel angles for the 3 deferred concepts (#5/#10/#13)
- Source new SMC concepts from outside the original 14 (e.g. LK's newer videos, ICT advanced concepts)
- Expand existing successful strategies (TSL variants, MTF variants, indicator confluence)
- Address book-level diversification gaps (asset class, timeframe, regime)

**Deferred (failed v1 — needs new angle before re-attempt):**
- ~~**Mitigation Block Continuation** (#13)~~ — DEFERRED 2026-04-27 after S55 v1 failed initial QA (PF 1.13, IS/OOS DD both >25% cap). FVG-only-mitigation variant lacks selectivity. Future v2 must layer OTE / premium-discount / HTF bias on top of mitigation tracking — risk of duplicating S7.
- ~~**Liquidity Grab → Reversal** (#5)~~ — DEFERRED 2026-04-26 after S54 v1 failed initial QA. OB-at-CHoCH-origin variant is confirmed structural failure pattern (S6 + S54). Future v2/v3 must use fundamentally different mechanic (FVG entry, HTF bias gate, momentum-confirmed sweep).

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
| 2026-04-27 | Orchestrator | 9 | 4 missing (#5/#9/#13/#14), partial #10 | S55 mitigation_block_continuation_v1 ATTEMPTED → FAILED initial QA (PF 1.13, OOS DD 29.7%, IS DD 35.2%). FVG-as-mitigation-zone with three-state ledger is mechanically sound but lacks selectivity. #13 stays MISSING with "needs OTE/fib/bias filter on top of mitigation tracking" flag. **Next priority: #14 (Double Liquidity Sweep) — review failed/double_sweep_ob_v1 first.** |
| 2026-04-27 | Orchestrator | 10 | 3 missing (#5/#9/#13), partial #10 | S56 double_sweep_v2 PROMOTED — first true dual-side-sweep specialist. AR best (DISP=floor) gave $97k OOS but 3/6 canonical stress collapses + 5/11 extended fails; user selected AR iter #17 stress-improv (DISP=default-area) for $50k OOS + 5/6 canonical pass + 8/11 extended OK. BNB borderline -$23 PnL flip risk-accepted. Extended-pair informational sweep was the deciding factor. Concept #14 ✅ COVERED. **Next priority: #10 (Range-Sweep Reversal) — note 2 prior sweep-reversal failures (S6, S54), design must justify range-context.** |
| 2026-04-27 | Orchestrator | 10 | 3 missing (#5/#9/#13), partial #10 | S57 range_sweep_reversal_v1 ATTEMPTED → FAILED initial QA (18 OOS, gate 50) + AR scarcity-rescue (0/50 accepts). Edge real on rare setups (PF 2.92, 0/6 stress) but does NOT scale. AR best stress-improv (iter #14, MIN_RANGE_TOUCHES 2→1) lifted to 119t but PF crashed 1.43 + 3/6 stress collapses. Classic scarcity-trap. Strategist's failure-pattern justification vs S6/S54 was sound (ADX gate + multi-touch + displacement) but the qualification stack is structurally too rare. #10 stays PARTIAL with deferred-v2 flag. **Next priority: #9 (Inducement) — last remaining green-field concept; fuzzier mechanism but no failed-attempt baggage.** |
| 2026-04-27 | Orchestrator | 11 | 3 missing (all deferred-failed-v1: #5/#10/#13), no green-field remaining | S58 inducement_sweep_v1 PROMOTED — **fills #9, the LAST green-field SMC concept**. OOS 82t / 72% WR / $10.7k / PF 4.41 / 0/6 stress / **NO carve-outs (first of session)**. AR multi-objective rescue resolved 3 initial-QA hard fails simultaneously (35t→82t / 2/6 stress→0/6 / 19pp IS-OOS WR drop→clean). Ext run **STRICTLY CONVERGED at iter #18/25 (first strict convergence of session)** — high-confidence operating point. Manual MAX_RETRACE_PCT sweep before formal QA established invariants ceiling (0.618). Inducement classification (shallow-retrace counter-trend swing) is the novel filter that distinguishes from S22's any-sweep approach. **Coverage 11/14 — all green-field concepts now attempted.** Remaining missing concepts are all deferred-failed-v1 awaiting fundamentally new angles. **Future research must source new concepts outside the original 14 OR find novel v2 mechanics for #5/#10/#13.** |
