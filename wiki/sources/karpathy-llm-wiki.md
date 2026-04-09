---
type: source
title: "LLM Wiki — a pattern for building personal knowledge bases using LLMs"
source_type: other
author: "Andrej Karpathy"
date_published: 2026-04-06
date_ingested: 2026-04-06
original_file: raw/Karpathy_llm-wiki.md
asset: multi
tags: [ai-ml, knowledge-base, llm-wiki, methodology]
---

## Summary

A GitHub Gist by Andrej Karpathy describing the LLM Wiki pattern — the foundational methodology this AutoTrader knowledge base is built on. The document is intentionally abstract and designed to be pasted to an LLM agent, which then instantiates a domain-specific implementation in collaboration with the human.

The core idea: instead of RAG (retrieve-and-generate on every query), the LLM incrementally builds and maintains a persistent, interlinked wiki. Every new source is integrated into the existing structure — entity pages updated, contradictions flagged, synthesis revised. The wiki compounds over time. Knowledge is compiled once and kept current rather than re-derived on every question.

This document is the reference spec behind `CLAUDE.md`. The AutoTrader wiki is a direct implementation of this pattern, adapted for the SMC trading bot domain.

## Key Points

- **RAG vs LLM Wiki:** RAG re-derives knowledge on every query. The LLM Wiki pre-compiles it into a persistent structure. Cross-references, contradictions, and synthesis are already there at query time.
- **Three layers:** Raw sources (immutable) → Wiki (LLM-owned markdown) → Schema (CLAUDE.md/AGENTS.md — the operating contract for the LLM maintainer).
- **The schema is the key:** A CLAUDE.md (or equivalent) transforms the LLM from a generic chatbot into a disciplined wiki maintainer. Human and LLM co-evolve it over time.
- **Operations:** Ingest (source → wiki updates, 10–15 pages touched), Query (index first, then drill; answers filed back as wiki pages), Lint (health check for contradictions, orphans, gaps).
- **index.md + log.md pattern:** Index is content-oriented (catalog for navigation); log is chronological (append-only timeline, grep-parseable). Index-first query avoids embedding/RAG infrastructure at moderate scale (~100 sources).
- **Human role:** Curate sources, direct analysis, ask good questions, think about meaning.
- **LLM role:** Summarise, cross-reference, file, bookkeep, maintain consistency across all pages.
- **Why wikis die without LLMs:** Maintenance burden (updating cross-references, keeping summaries current, flagging contradictions) grows faster than value. LLMs don't get bored and can touch 15 files in one pass.
- **Intellectual lineage:** Relates to Vannevar Bush's Memex (1945) — private, curated knowledge with associative trails. The LLM solves the maintenance problem Bush couldn't.

## Tooling Mentioned

- **Obsidian** — IDE for reading/browsing the wiki in real time while LLM edits.
- **Obsidian Web Clipper** — browser extension converting web articles to markdown for raw collection.
- **Obsidian Dataview plugin** — runs queries over YAML frontmatter; generates dynamic tables.
- **Marp** — markdown-based slide deck format; Obsidian plugin available.
- **qmd** — local search engine for markdown (BM25/vector hybrid + LLM re-ranking, CLI + MCP server). Suggested for larger wikis where index.md is insufficient.
- **Git** — wiki is just a repo; version history, branching, collaboration included.

## Entities Mentioned

[[entities/karpathy|Andrej Karpathy]]

## Contradictions / Open Questions

- None. This source defines the pattern; CLAUDE.md is the implementation. Any divergence between this document and CLAUDE.md reflects intentional domain-specific adaptation, not contradiction.

## Quotes

> "The key difference: the wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read."

> "Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass."

> "The human's job is to curate sources, direct the analysis, ask good questions, and think about what it all means. The LLM's job is everything else."
