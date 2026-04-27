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


---

## New Concept Research (post-original-14, audited 2026-04-27)

Sourced from wiki concept pages, 21 wiki strategy pages, and 79 ingested LK/ICT/institutional sources. Evaluated against the 58-strategy index for novelty. Ranked by implementation feasibility and expected edge on 1H crypto.

### Candidate 1: Power of Three / AMD Session Pattern (RECOMMENDED)

**Description:** Detect the Accumulation-Manipulation-Distribution three-phase pattern within daily candles or sessions. Enter after manipulation phase (false move) completes, in the distribution direction. Wiki page: `concepts/power-of-three.md`.

- **Primary mechanic vs filter:** PRIMARY. New detection pipeline: measure intraday range relative to daily open, classify accumulation (low ATR cluster near open), manipulation (spike against eventual close direction), distribution (displacement toward close). No existing strategy uses daily-open-anchored phase classification.
- **Frequency expectation:** MODERATE. Every daily candle exhibits PO3 structure; the question is how often the manipulation phase is mechanically detectable with sufficient clarity. Proxy: S38 (candle character) gets 278t OOS using daily candle patterns -- PO3 adds intraday phase timing on top of similar daily context, expect 150-300t range.
- **Novelty vs existing 51:** S38 uses daily candle CHARACTER (continuation/rejection patterns vs PDH/PDL) but does NOT classify intraday phases. PO3 is structurally different: it anchors to the daily open price and classifies the intraday move into three temporal phases. No overlap with S38 detection pipeline.
- **Priority:** HIGH. Well-documented in wiki, maps to killzones (Asian=accumulation, London=manipulation, NY=distribution), and the daily open is an objective anchor that requires no lookahead.
- **Risk flags:** Phase classification may require hindsight to confirm manipulation vs real breakout. Mitigation: use displacement candle as manipulation-end signal (same mechanic proven in S4/S40/S52). Crypto 24/7 markets blur session boundaries -- use UTC-anchored killzone windows per wiki spec.

### Candidate 2: Stacked Liquidity Target with OB Entry

**Description:** Require 2+ independent liquidity types (equal highs/lows, session levels, weak highs/lows, trendlines) to stack at the same price level as the TAKE PROFIT target, then enter from the nearest OB/FVG between current price and the stacked target. Wiki source: `strategies/lk-advanced-smc-funded.md`.

- **Primary mechanic vs filter:** PRIMARY. Novel TP-selection mechanic. Existing strategies use fixed R:R or structural-swing TP. This inverts the logic: identify WHERE institutions will drive price (stacked liquidity), THEN find entry. No existing strategy scores liquidity targets.
- **Frequency expectation:** MODERATE-LOW. The stacked-target requirement is selective. Equal highs/lows + session levels are common; requiring 2+ at the same level narrows the funnel. Expect 80-200t OOS based on how loose the "same level" tolerance is.
- **Novelty vs existing 51:** Completely novel TP mechanic. Entry zone could be OB (shared with many), but the trade-selection and TP-targeting is unlike anything in the book. Closest relative is S39 (weak-swing TP) which FAILED at 26.4% WR -- but S39 used weak swings as BOTH entry qualifier AND TP, while this uses stacked liquidity ONLY as TP with standard OB entry.
- **Priority:** MEDIUM-HIGH. Well-documented in wiki with scoring system. Risk of scarcity similar to S51/S57.
- **Risk flags:** Equal-highs detection tolerance is undefined in source. Trendline detection (3+ touches) is complex and potentially brittle. Recommend starting with equal highs/lows + session levels only (drop trendlines for v1).

### Candidate 3: Session Sweep Continuation (Asia Range as Liquidity Pool)

**Description:** Use the Asian session high/low as a defined liquidity range. When London/NY sweeps one side, enter continuation in the sweep direction from a post-sweep OB with displacement confirmation. Single-TF 1H adaptation of LK multi-TF Asia sweep model. Wiki sources: `strategies/lk-5-step-asia-sweep.md`, `strategies/lk-easy-liquidity.md`, `strategies/lk-london-bias-ob.md`, `concepts/killzones.md`.

- **Primary mechanic vs filter:** PRIMARY. The session range defines both the liquidity pool (trigger) and the directional bias (which side was swept). No existing strategy uses session-defined ranges as the primary signal generator. S22 (sweep-gated OB) uses sweep as a filter on arbitrary swings; this REQUIRES the sweep to be of a specific session-defined level.
- **Frequency expectation:** MODERATE. Asian range forms every day. London sweeps one side approximately 60-70% of trading days per LK. At 1H resolution with 13 pairs, expect 200-400t OOS. The 1H adaptation avoids the scarcity trap of LTF session strategies (S5 got 0 trades on crypto at 15m/5m/1m).
- **Novelty vs existing 51:** Session-anchored levels are entirely new to the book. S17 (Session Breakout) FAILED but used Asian range BREAKOUT (close through), not SWEEP (wick through + close back). S22 uses any-swing sweep, not session-specific. The time-of-day dimension is unused by any passing strategy.
- **Priority:** MEDIUM-HIGH. Strong wiki coverage across 3 strategy pages. The 1H adaptation sidesteps the S5/S35/S36/S37 LTF failure pattern while preserving the session-sweep thesis.
- **Risk flags:** Crypto 24/7 may weaken session effects vs forex. S5 (LK 4-Step Proven) got 0 trades adapting a session strategy to crypto -- but S5 required 15m/5m/1m MTF which is the known failure mode, not the session concept itself. Session boundaries need careful UTC/EST handling (DST). BUG-014 risk if combining session detection with HTF bias.

### Candidate 4: Retail Trap Reversal (Equal Highs/Lows Sweep)

**Description:** Detect clusters of equal highs or equal lows (retail stop-loss magnets), wait for institutional sweep of the cluster, then enter reversal via LTF CHoCH + OB. Wiki source: `strategies/lk-retail-trap-reversal.md` (asset_class: multi).

- **Primary mechanic vs filter:** PRIMARY. Equal-highs/lows detection is the novel element -- no existing strategy detects or trades off equal-level clusters. The sweep + reversal after sweep is familiar territory (S6, S54 failed), but equal-highs as the SPECIFIC liquidity target is new.
- **Frequency expectation:** MODERATE. Equal highs/lows form frequently on 1H charts. LK states $8K live trade example with 1:3 RR target.
- **Novelty vs existing 51:** Equal-highs/lows detection is genuinely new. However, this is a REVERSAL strategy -- and the book has a confirmed structural failure pattern with sweep-reversal (S6 27.9% WR, S54 29.5% WR). Adding a different liquidity-source (equal highs vs arbitrary swings) may not fix the core timing problem (CHoCH after sweep fires too late).
- **Priority:** LOW-MEDIUM. Novel detection element but reversal risk is high given book history.
- **Risk flags:** REVERSAL strategy -- third attempt at sweep-reversal. The wiki page itself notes "no HTF bias filter" which is the exact gap that killed S6/S54. Would need to be designed as CONTINUATION (sweep of equal highs in trend direction, then continue) rather than reversal to avoid the known failure pattern. If redesigned as continuation, overlaps heavily with S22.

### Candidate 5: Multi-Oscillator Divergence at OB (SMC + Divergence Hybrid)

**Description:** Detect hidden or regular divergence across 2+ independent oscillators (RSI + MACD or RSI + MFI) specifically at an OB/FVG zone, as a timing filter for the structural entry. Wiki sources: `strategies/divergence-reversal.md`, `concepts/indicator-divergence.md`.

- **Primary mechanic vs filter:** FILTER on top of existing OB entry. S30 (SMC + RSI Divergence) is NEW status but uses SINGLE oscillator. This would require 2+ oscillators diverging simultaneously -- higher conviction but lower frequency.
- **Frequency expectation:** LOW-MODERATE. Double divergence at an OB zone is a multi-condition stack. The divergence-reversal wiki strategy targets 2-4 setups/month on major pairs at Daily/4H -- on 1H with 13 crypto pairs, expect 50-120t OOS.
- **Novelty vs existing 51:** S30 (RSI divergence at OB) is NEW, never tested. This extends S30 by requiring multi-oscillator agreement. If S30 passes, this becomes a tighter variant. If S30 fails, this likely fails harder (more filters on same concept).
- **Priority:** LOW. Depends on S30 outcome first. Building a tighter version of an untested concept is premature. Recommend testing S30 first, then deciding if multi-oscillator adds value.
- **Risk flags:** S30 must be tested first. Multi-condition stacks on top of OB have a poor track record in this book (S13 cluster filter, S51 containment). Divergence detection requires swing-point identification on oscillators which adds implementation complexity.

### Concepts Evaluated and Ruled Out

- **Killzones as standalone filter:** Already implicitly used by session-based strategies. As a pure time-window filter applied to existing strategies, this is a parameter (session mask), not a new strategy. Would be tested as a variant of existing strategies, not a new concept.
- **Wyckoff phases:** Too discretionary. Accumulation/distribution range identification requires subjective judgment about "institutional intent" that resists mechanical formalization. Spring/upthrust patterns are mechanically similar to sweep-reversal (already failed 2x). Ruled out.
- **ICT Silver Bullet (15-min window):** Requires 15m or lower TF execution -- the book LTF failure pattern (S35/S36/S37) makes this high-risk. The 10:00-10:15 AM window is extremely narrow for 1H candles. Ruled out for 1H.
- **Re-entry patterns (FVG after FVG):** Sequential imbalance is interesting but hard to distinguish from S55 mitigation tracking (which failed). Would need to prove the sequential element adds selectivity over single-FVG entry. Deferred pending S55 v2 redesign.
- **HTF imbalance-as-magnet:** Using unfilled D1/4H FVGs as directional targets is already covered by S46 (4H FVG Retest) and S38 (daily candle character with structural 4H TP). Adding a "magnet" concept without a novel entry mechanic just reframes existing approaches.
