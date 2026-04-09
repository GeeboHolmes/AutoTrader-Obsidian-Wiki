# Wiki Log

Append-only. Parseable with: `grep "^## \[" log.md`

---

## [2026-04-06] ingest | karpathy/llm-wiki

- **Operation:** ingest
- **Summary:** Ingested Karpathy's LLM Wiki gist — the foundational pattern this AutoTrader wiki is built on. Source page only per user instruction. Documents the three-layer architecture (raw/wiki/schema), ingest/query/lint operations, and index+log pattern that CLAUDE.md implements.
- **Pages changed:** wiki/sources/karpathy-llm-wiki.md (NEW), index.md

---

## [2026-04-06] ingest | karpathy/autoresearch

- **Operation:** ingest
- **Summary:** Ingested Karpathy autoresearch GitHub README as reference source for the Karpathy optimization pipeline stage. Source page only — no concept or strategy pages created per user instruction. Entity page created for Andrej Karpathy.
- **Pages changed:** wiki/sources/karpathy-autoresearch.md (NEW), wiki/entities/karpathy.md (NEW), index.md

---

## [2026-04-06] ingest | LK Strategy batch 4 (10 files)

- **Operation:** ingest
- **Summary:** Ingested 10 Lewis Kelly transcripts. 10 source pages written, 3 new strategy pages, 3 concept pages updated with major new content. Key material: Sweep vs Run distinction (candle closure determines sweep vs continuation — critical implementation rule), Sweep Shift OB and Continuation Trap OB (800 trades / 5 years validated OB selection criteria), $100K strategy (50% fib premium/discount filter + fixed 1:3 RR backtested), RLI framework (Range Liquidation Initiation — institutional mechanics model), DXY cross-reference for EUR/USD confirmation, candle closure rule formally documented, easiest structure 4-step with $26K live trade. Concepts updated: Liquidity (sweep vs run), Order Blocks (Sweep Shift + Continuation Trap), Market Structure (candle closure expanded).
- **Pages changed:** wiki/sources/lk-3-step-scalping-premium.md (NEW), wiki/sources/lk-1min-scalping-2024.md (NEW), wiki/sources/lk-5-step-rli.md (NEW), wiki/sources/lk-1min-scalping-dxy.md (NEW), wiki/sources/lk-get-funded-live.md (NEW), wiki/sources/lk-secret-ob-types.md (NEW), wiki/sources/lk-100k-strategy.md (NEW), wiki/sources/lk-sweep-vs-run.md (NEW), wiki/sources/lk-candle-closure.md (NEW), wiki/sources/lk-easiest-structure-strategy.md (NEW), wiki/strategies/lk-5-step-rli.md (NEW), wiki/strategies/lk-100k-premium-discount.md (NEW), wiki/strategies/lk-easiest-structure.md (NEW), wiki/concepts/liquidity.md (UPDATED), wiki/concepts/order-blocks.md (UPDATED), wiki/concepts/market-structure.md (UPDATED), index.md

---

## [2026-04-06] ingest | LK Strategy batch 3 (10 files)

- **Operation:** ingest
- **Summary:** Ingested 10 Lewis Kelly strategy-focused transcripts. All 10 distinct named strategies identified and created as dedicated pages. Key material: Intraday Bias Model (backtested 230 trades / 216% return / 33% win / 6.23 RR), 3-Rule Sniper (TP1 at 5R with 97% BE-saves-money stat on 1000+ trades), Boring Scalping (Asia range opposite-extreme target), PDL Scalping (577 trades / 2.5yr same dataset as 4-step proven), A+ Setup (triple-confluence with daily candle character), Daily Bias (Power of Three + daily candle patterns), 5-Step Swing (4H bias + premium/discount + scale-in), Retail Trap Reversal (equal highs / trendline sweeps), Advanced SMC Funded (stacked liquidity concept), Simple SMC (institutional logic deep-dive with 40% WR / 1:6.4 RR).
- **Pages changed:** wiki/sources/lk-5-rule-intraday-bias.md (NEW), wiki/sources/lk-3-rule-sniper.md (NEW), wiki/sources/lk-boring-scalping.md (NEW), wiki/sources/lk-3-step-scalping-pdl.md (NEW), wiki/sources/lk-3-step-aplus.md (NEW), wiki/sources/lk-simple-smc.md (NEW), wiki/sources/lk-daily-bias.md (NEW), wiki/sources/lk-5-step-swing.md (NEW), wiki/sources/lk-retail-trap-reversal.md (NEW), wiki/sources/lk-advanced-smc-funded.md (NEW), wiki/strategies/lk-5-rule-intraday-bias.md (NEW), wiki/strategies/lk-3-rule-sniper.md (NEW), wiki/strategies/lk-boring-scalping.md (NEW), wiki/strategies/lk-3-step-scalping-pdl.md (NEW), wiki/strategies/lk-3-step-aplus.md (NEW), wiki/strategies/lk-simple-smc.md (NEW), wiki/strategies/lk-daily-bias.md (NEW), wiki/strategies/lk-5-step-swing.md (NEW), wiki/strategies/lk-retail-trap-reversal.md (NEW), wiki/strategies/lk-advanced-smc-funded.md (NEW), index.md

---

## [2026-04-05] ingest | LK Asia Sweep strategies (2 files)

- **Operation:** ingest
- **Summary:** Ingested 2 Lewis Kelly strategy-focused transcripts. Both describe Asia session sweep strategies. Created 2 source pages and 2 dedicated strategy pages (lk-5-step-asia-sweep, lk-4-step-proven). Key findings: Asia low sweep as mandatory filter; minimum 1:5 RR constraint (4-step only); 577-trade backtest showing 360%+ returns.
- **Pages changed:** wiki/sources/lk-5-step-asia-sweep.md (NEW), wiki/sources/lk-4-step-proven.md (NEW), wiki/strategies/lk-5-step-asia-sweep.md (NEW), wiki/strategies/lk-4-step-proven.md (NEW), index.md

---

## [2026-04-05] ingest | Lewis Kelly foundation batch (9 files, 8 successfully read)

- **Operation:** ingest
- **Summary:** Ingested 8 of 9 foundation files from Lewis Kelly's corpus (1 inaccessible due to unicode apostrophe in filename). Extracted comprehensive SMC rules, created 8 source pages, updated 3 concept pages, created 2 new concept/strategy pages, documented 3 inter-source contradictions in overview.
- **Pages changed:** wiki/sources/lk-ultimate-smc-guide-2026.md, wiki/sources/lk-market-structure-full-course.md, wiki/sources/lk-ultimate-order-block-2025.md, wiki/sources/lk-liquidity-masterclass.md, wiki/sources/lk-top-down-analysis.md, wiki/sources/lk-ultimate-smc-course-2024.md, wiki/sources/lk-supply-demand-guide-2026.md, wiki/sources/lk-structure-ob-profits-2025.md, wiki/concepts/market-structure.md, wiki/concepts/order-blocks.md, wiki/concepts/liquidity.md, wiki/concepts/top-down-analysis.md (NEW), wiki/strategies/lewis-kelly-smc-intraday.md (NEW), wiki/strategies/smc-crypto-intraday.md, wiki/overview.md, index.md

---

## [2026-04-05] setup | Lewis Kelly entity + raw file inventory

- **Operation:** setup
- **Summary:** Discovered 75 Lewis Kelly video transcripts already in raw/Lewis Kelly/. Created entity page. Updated index.
- **Pages changed:** wiki/entities/lewis-kelly.md, index.md

---

## [2026-04-05] setup | Full wiki initialisation

- **Operation:** setup
- **Summary:** Full wiki built from scratch for SMC cryptocurrency automated trading bot project.
- **Pages changed:** CLAUDE.md, index.md, log.md, wiki/overview.md, wiki/entities/ict.md, wiki/entities/python-bot.md, wiki/strategies/smc-crypto-intraday.md, wiki/concepts/smart-money-concepts.md, wiki/concepts/market-structure.md, wiki/concepts/break-of-structure.md, wiki/concepts/liquidity.md, wiki/concepts/order-blocks.md, wiki/concepts/fair-value-gap.md, wiki/concepts/displacement.md, wiki/concepts/premium-discount.md, wiki/concepts/inducement.md, wiki/concepts/mitigation.md, wiki/concepts/swing-points.md, wiki/concepts/killzones.md, wiki/concepts/power-of-three.md, wiki/concepts/risk-management.md, wiki/concepts/backtesting.md
