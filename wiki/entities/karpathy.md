---
type: entity
entity_type: person
aliases: [Andrej Karpathy, @karpathy]
tags: [ai-ml, llm-training, autonomous-research]
---

## Overview

Andrej Karpathy is an AI researcher and engineer — formerly Director of AI at Tesla and founding member of OpenAI. Known for producing highly accessible implementations of neural network training (nanoGPT, nanochat) and, more recently, autonomous AI research tooling.

## Details

- Creator of **nanochat** — a simplified single-GPU GPT training implementation used as the base for `autoresearch`.
- Creator of **autoresearch** (March 2026) — the autonomous AI agent research loop that iterates on `train.py` overnight using a fixed 5-minute training budget and `val_bpb` as the objective metric.
- Influential in the Python/ML community for "minimal but real" implementations that are practical starting points rather than toy examples.

## Relevance to This Project

The `autoresearch` methodology is the reference design for the **Karpathy optimization** stage of the trading bot pipeline (`strategy → script → backtest → Karpathy optimization → live test → results matrix`). The autonomous agent loop — modify, evaluate, keep/discard, repeat — is adapted to iterate on bot strategy parameters and model components rather than LLM training.

## Connections

- Related entities: [[entities/python-bot|Python Bot]]
- Relevant concepts: [[concepts/backtesting|Backtesting]]

## Source History

| Date | Source | What was added/changed |
|------|--------|------------------------|
| 2026-04-06 | [[sources/karpathy-autoresearch\|Karpathy Autoresearch]] | Entity created; autoresearch methodology documented |
