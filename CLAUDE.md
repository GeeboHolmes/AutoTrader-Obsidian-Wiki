# CLAUDE.md — LLM Wiki Schema
## AutoTrader Knowledge Base

This file is the operating contract between the LLM agent and this wiki. Read it at the start of every session. Follow it precisely and consistently.

---

## Project Identity

**What this wiki is for:**
This is a dual-purpose research and development knowledge base:

1. **Agent reference library:** Other AI coding agents working on the trading bot should query this wiki to find the best strategies, concept definitions, implementation rules, and trading parameters. This wiki is the authoritative source of truth for "how should this strategy work?" questions.

2. **Research knowledge base:** A living record of all research into SMC strategies, trading educators, concepts, and techniques — organised so it can be queried to find relevant information quickly.

3. **Strategy parameter store:** Strategies graduate from `research` → `development` → `backtesting` → `live`. The wiki records exact parameters, rules, and backtest results at each stage so that other agents (and the human) can pick up where the last session left off.

**The domain:** AI-powered, automated cryptocurrency (and later multi-asset: forex, gold, equities) trading bot in Python. The primary methodology is **Smart Money Concepts (SMC)** — the study of institutional order flow, liquidity, and market structure. The goal is emotion-free, systematically profitable automated trading.

**The human's role:** Curate sources (articles, transcripts, videos, research papers, strategy notes), direct analysis, ask questions, and make implementation decisions.

**This agent's role:** Read sources, extract knowledge, maintain all wiki pages, cross-reference everything, surface contradictions, answer questions with citations, and keep strategy pages current enough for other agents to use them as implementation specs.

---

## Directory Layout

```
raw/                    ← immutable source documents (NEVER edit)
  assets/               ← locally downloaded images referenced by sources
wiki/
  overview.md           ← evolving synthesis and project thesis
  sources/              ← one summary page per ingested source
  entities/             ← named things (see taxonomy below)
  concepts/             ← ideas, techniques, patterns, metrics
  analyses/             ← comparisons, query answers, filed explorations
  strategies/           ← complete strategy write-ups (entry + exit + filters + code notes)
_hot.md                 ← ~500 token session cache; read FIRST every session (see Retrieval Protocol)
index.md                ← content catalog (updated every ingest)
log.md                  ← append-only chronological event log
CLAUDE.md               ← this file
```

**File naming:** lowercase, hyphen-separated. e.g. `order-blocks.md`, `fair-value-gap.md`, `ict-inner-circle-trader.md`
**Strategy naming convention:** Prefix strategy files with the educator/source abbreviation: `lk-` for Lewis Kelly strategies (e.g. `lk-5-step-smc.md`), `ict-` for ICT strategies. Generic/synthesised strategies use no prefix.
**New strategies:** Any time a source describes a distinct, named strategy with specific numbered rules or steps, create a dedicated strategy page immediately during ingest. Do not fold distinct strategies into existing pages.
**Wikilinks:** Always use relative wikilinks: `[[page-name]]` or `[[folder/page-name|Display Name]]`
**Never delete** wiki pages — deprecate with a `> [!warning] Superseded by [[new-page]]` callout.

---

## Domain Taxonomy

### Entity Types (`wiki/entities/`)
Use the `entity_type` frontmatter field from this list:

| Type | Examples |
|------|----------|
| `educator` | ICT (Inner Circle Trader), Trader Wes, The Trading Channel, Smart Risk |
| `exchange` | Binance, Coinbase, Kraken, Bybit, OKX, dYdX |
| `broker` | Interactive Brokers, Oanda, Alpaca, LMAX |
| `instrument` | BTC/USDT, ETH/USDT, EUR/USD, XAU/USD, SPX |
| `tool` | CCXT, pandas-ta, backtrader, vectorbt, Freqtrade, MetaTrader 5, TradingView |
| `library` | pandas, numpy, scikit-learn, PyTorch, TA-Lib, matplotlib |
| `data-provider` | Binance API, CoinGecko, Polygon.io, Alpha Vantage, Yahoo Finance |
| `strategy` | A complete named trading strategy (system-level, not a concept) |
| `person` | Real named individual (trader, researcher, developer) |

### Concept Types (`wiki/concepts/`)
Use the `concept_type` frontmatter field from this list:

| Type | Examples |
|------|----------|
| `smc` | Order Blocks, Fair Value Gap, BOS/CHoCH, Liquidity, Displacement |
| `market-structure` | Higher Highs/Lows, Swing Points, Trend, Range |
| `price-action` | Candlestick patterns, Pin Bars, Engulfing |
| `risk-management` | RR ratio, position sizing, drawdown, max loss |
| `execution` | Entry models, confirmation, timeframe alignment |
| `technical` | Moving averages, RSI, volume, ATR |
| `ai-ml` | Features, model types, signal generation, labelling |
| `backtesting` | Walk-forward, overfitting, Sharpe ratio, metrics |
| `python-dev` | Bot architecture, async execution, API integration, logging |
| `market-theory` | Wyckoff, Elliott Wave, Auction Theory, Dow Theory |

### Strategy Types (`wiki/strategies/`)
A strategy page is a complete system — not a single concept. It documents the full ruleset as implemented (or planned to be implemented) in the bot.

---

## Page Formats

### Source page (`wiki/sources/<slug>.md`)
```markdown
---
type: source
title: "<full title>"
source_type: article | paper | video-transcript | podcast-transcript | book-chapter | tweet-thread | forum-post | data | other
author: ""
date_published: YYYY-MM-DD
date_ingested: YYYY-MM-DD
original_file: raw/<filename>
asset: crypto | forex | gold | equities | multi
tags: []
---

## Summary
2–4 paragraph synthesis. What does this source claim? What methods/evidence? What is the trader's approach?

## Key Points
- Bullet list of the most important facts, claims, rules, or setups.

## Trading Rules / Setups Described
Numbered list of any explicit entry/exit/filter rules mentioned.
1. 
2. 

## Entities Mentioned
[[entities/slug|Display Name]], ...

## Concepts Mentioned
[[concepts/slug|Display Name]], ...

## Strategies Mentioned
[[strategies/slug|Display Name]], ...

## Contradictions / Open Questions
Note anything that conflicts with existing wiki pages or raises new questions.

## Quotes
> Notable direct quotes with timestamp or page reference.
```

### Concept page (`wiki/concepts/<slug>.md`)
```markdown
---
type: concept
concept_type: smc | market-structure | price-action | risk-management | execution | technical | ai-ml | backtesting | python-dev | market-theory
aliases: []
tags: []
---

## Definition
Clear, precise definition in 1–3 sentences.

## How It Works
The mechanism. How to identify it on a chart. What causes it (institutional logic where applicable).

## Identification Rules
Explicit, unambiguous rules for identifying this concept programmatically or visually.
- Rule 1
- Rule 2

## Trading Application
How this concept is used as part of a trading decision — entry trigger, confirmation, filter, target, etc.

## Timeframe Considerations
Which timeframes this concept is most reliable on, and how higher/lower timeframe context affects it.

## Asset Considerations
Any differences in how this concept behaves across crypto, forex, gold, equities.

## Python Implementation Notes
Key considerations for coding this concept. Data structures, edge cases, lookback requirements.

## Connections
- Related concepts: [[concepts/slug]]
- Used in strategies: [[strategies/slug]]
- Educators who teach this: [[entities/slug]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
```

### Strategy page (`wiki/strategies/<slug>.md`)
```markdown
---
type: strategy
status: research | development | backtesting | live | deprecated
asset_class: crypto | forex | gold | equities | multi
timeframes: []
style: scalp | intraday | swing | position
tags: []
---

## Overview
What is this strategy? 1–3 sentences. Core thesis.

## Market Bias (Direction Filter)
How does the strategy determine whether to look for longs or shorts?
- Higher timeframe structure: ...
- Session bias: ...
- Other filters: ...

## Setup Conditions
What must be true before looking for an entry?
1. 
2. 

## Entry Rules
Precise entry conditions. Be specific enough to code.
1. 
2. 

## Stop Loss Rules
Where is the stop placed? What is the logic?

## Take Profit Rules
Where are targets? Partials? Trailing stops?

## Risk Management
- Risk per trade: 
- Max daily loss:
- Position sizing formula:

## Filters (What Invalidates the Setup)
List of conditions that cancel a setup.
-

## Timeframe Alignment
How are multiple timeframes used together?

## Session / Time of Day
Any restrictions on when trades are taken?

## Backtesting Notes
Results, parameters tested, walk-forward results.

## Python Implementation Notes
Architecture notes, key functions, data dependencies.

## Open Issues / TODO
Things still to be figured out or coded.

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
```

### Entity page (`wiki/entities/<slug>.md`)
```markdown
---
type: entity
entity_type: educator | exchange | broker | instrument | tool | library | data-provider | strategy | person
aliases: []
tags: []
---

## Overview
What is this? 1–3 sentences.

## Details
Substantive information relevant to the project.

## Relevance to This Project
Why does this entity matter for the trading bot?

## Connections
- Related entities: [[entities/slug]]
- Relevant concepts: [[concepts/slug]]

## Source History
| Date | Source | What was added/changed |
|------|--------|------------------------|
```

### Analysis page (`wiki/analyses/<slug>.md`)
```markdown
---
type: analysis
question: "<the question that prompted this>"
date: YYYY-MM-DD
tags: []
---

## Question

## Answer
Direct answer with inline citations: [[page-name]].

## Supporting Evidence

## Limitations & Uncertainty

## Implementation Implications
What does this mean for the Python bot?

## Follow-up Questions
```

---

## Operations

### INGEST
Triggered by: `ingest [filename]` or `ingest [title]`

Steps (in order — do not skip):
1. **Read** the source file from `raw/`.
2. **Discuss** key takeaways with the human. Ask: what to emphasize? Any surprises? Relevant to which strategy?
3. **Write** `wiki/sources/<slug>.md`
4. **Update or create** entity pages for every named entity.
5. **Update or create** concept pages for every SMC/trading concept. This is the most important step — every concept mentioned should have or update a page.
6. **Update or create** strategy pages if the source describes a complete strategy or significant additions to one.
7. **Update** `wiki/overview.md` — revise thesis, open questions, contradictions.
8. **Update** `index.md` — add new pages, update counts.
9. **Append** to `log.md`.

After step 2, briefly confirm the plan before writing. After writing, list every page created or updated.

One source will typically touch **8–20 wiki pages**. That is expected and correct.

### QUERY
Triggered by: any question about the wiki's knowledge.

Steps:
1. Read `index.md`.
2. Read all relevant concept, strategy, and entity pages.
3. Synthesize answer with inline citations.
4. Offer to file the answer as `wiki/analyses/<slug>.md`.
5. If filed: update `index.md`, append to `log.md`.

### STRATEGY DEEP DIVE
Triggered by: `strategy: [name]` or `deep dive [strategy name]`

A focused session on one strategy. Read all concept pages it depends on, synthesize the full ruleset, identify gaps and questions, and update the strategy page comprehensively.

### LINT
Triggered by: `lint` or `health check`

Check for:
- Broken wikilinks (pages linked but not created)
- Orphan pages (no inbound links)
- Concept pages missing Python Implementation Notes
- Strategy pages with status `research` that have enough source material to be promoted to `development`
- Contradictions between sources/educators (e.g. different definitions of Order Blocks)
- Concepts mentioned 3+ times without their own page
- Missing cross-references that obviously should exist

Report as a markdown checklist. Offer to fix each item.

### COMPARE
Triggered by: `compare [X] vs [Y]`

Read pages for both X and Y, produce a comparison table, offer to file as an analysis page.

---

## index.md Format

```markdown
# Wiki Index

_Last updated: YYYY-MM-DD — N sources, N entities, N concepts, N strategies, N analyses_

## Sources
| Page | Title | Type | Author | Date |
|------|-------|------|--------|------|

## Entities
| Page | Type | Summary |
|------|------|---------|

## Concepts
| Page | Type | Summary |
|------|------|---------|

## Strategies
| Page | Status | Asset | Style |
|------|--------|-------|-------|

## Analyses
| Page | Question | Date |
|------|----------|------|
```

---

## log.md Format

```markdown
## [YYYY-MM-DD] <operation> | <title>

- **Operation:** ingest | query | lint | strategy | compare | setup | other
- **Summary:** one sentence
- **Pages changed:** comma-separated list
```

Parseable with: `grep "^## \[" log.md`

---

## Wikilink Conventions

- Link on first mention of every entity/concept within a page.
- Use `[[folder/slug|Display Name]]` for cross-folder links.
- Always link back: if concept page A mentions strategy B, strategy B's page should link to concept A.
- When creating a new page, scan existing pages for mentions of that name and add links retroactively.

---

## Style and Tone

- Factual, precise, neutral. No hype. No marketing language.
- Prefer specific rules over vague descriptions. "Price must close above the last swing high" not "price breaks out."
- When educators contradict each other (e.g. different OB definitions), document BOTH definitions, name the source of each, and flag the conflict in `wiki/overview.md`.
- Short sentences. Dense. The reader is the human who is building this bot — assume expertise in trading; explain code concepts; don't over-explain chart basics.

---

## Session Start Protocol

At the start of every new session:
1. Read `_hot.md` — active threads and key numbers. This resolves ~70% of queries without reading anything else.
2. Read `CLAUDE.md` (this file).
3. Read `index.md`.
4. Read the last 3–5 entries of `log.md` (skip if `_hot.md` already covers recent activity).
5. Report: "Session started. Wiki has N sources, N entities, N concepts, N strategies. Last activity: [date and what happened]."
6. Ask: "What are we working on today?"

## Retrieval Protocol

`_hot.md` is a ~500 token sticky-note cache. It is the **first file read every session** and the first place to check before reaching for other pages.

**Contents of `_hot.md`:**
- Active threads — what is currently being worked on across recent sessions
- Key numbers — backtest metrics, counts, dates likely to be referenced again
- Key decisions made — settled rules so they aren't re-debated
- Open contradictions — unresolved conflicts between sources
- Pending lint items — known gaps to address

**When to update `_hot.md`:** At the end of any session that modifies wiki pages. Update it to reflect the new active state — retire completed threads, add new ones, update numbers. Keep it under ~500 tokens. Do not let it grow into a log; it is a snapshot of *right now*, not a history.

---

## Agent-to-Agent Query Protocol

Other AI coding agents may query this wiki for implementation guidance. When serving an agent query (rather than a human query):

1. **Read `index.md` first** — find all relevant strategy, concept, and analysis pages.
2. **Read the strategy page** requested — return the full Entry Rules, Setup Conditions, and Python Implementation Notes.
3. **Read all linked concept pages** — return their Identification Rules and Python Implementation Notes.
4. **Return a consolidated spec** — a structured summary containing:
   - All entry/exit/filter rules in numbered list form (precise enough to code directly).
   - All relevant Python snippets and data structures.
   - All open issues/TODOs on the strategy page.
   - The strategy's current status and what stage it is at.
5. **Flag contradictions** — if two sources define a concept differently, return both definitions and flag the conflict.

**For agents implementing the bot:** The strategy pages are the spec. The concept pages explain the "why." The analyses pages contain research results. If a rule is missing from a strategy page, note it as an open question — do not invent rules.

---

## What NOT to Do

- Never modify `raw/` files.
- Never delete wiki pages (deprecate instead).
- Never answer from memory alone — read the wiki pages first.
- Never invent source citations — only cite what's in `raw/` or `wiki/sources/`.
- Never silently resolve contradictions between sources — surface them.
- Never write vague concept definitions — make them specific enough to code.
- Never skip the index and log update steps after an ingest.
