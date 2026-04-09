---
type: source
title: "karpathy/autoresearch: AI agents running research on single-GPU nanochat training automatically"
source_type: other
author: "Andrej Karpathy"
date_published: 2026-03-01
date_ingested: 2026-04-06
original_file: raw/karpathy_autoresearch_ AI agents running research on single-GPU nanochat training automatically.md
asset: multi
tags: [ai-ml, autonomous-research, llm-training, agent-loop]
---

## Summary

`karpathy/autoresearch` is a GitHub repository by Andrej Karpathy providing a minimal autonomous AI research loop for single-GPU LLM training experiments. An AI agent (Claude, Codex, or similar) is given a simplified GPT training codebase (`nanochat`) and instructed to modify, train, evaluate, and iterate without human involvement. The agent runs overnight, producing a log of experiments and a progressively improved model.

The human's only lever is `program.md` — a Markdown file that acts as lightweight instructions for the research org. Everything else (architecture, hyperparameters, optimizer, batch size) is the agent's domain. The repo is intentionally minimal: three files, one GPU, one metric.

This repo is ingested as a reference for the "Karpathy optimization" stage of the trading bot pipeline, where a similar autonomous agent loop iterates on bot strategy/model parameters rather than LLM training.

## Key Points

- **Core loop:** agent modifies `train.py` → fixed 5-minute training run → evaluate `val_bpb` → keep or discard → repeat. ~12 experiments/hour, ~100 experiments overnight.
- **Metric:** `val_bpb` (validation bits per byte) — lower is better, vocab-size-independent, so architectural changes (model size, batch size, etc.) are fairly compared.
- **Single file to modify:** agent only touches `train.py` (GPT model + Muon/AdamW optimizer + training loop). `prepare.py` is fixed. This keeps diffs reviewable.
- **Human programs `program.md`:** defines the research org context and instructions. Iterating on `program.md` is the human's research contribution — finding the "research org code" that produces fastest progress.
- **Self-contained:** no distributed training, no complex config. Single NVIDIA GPU (tested on H100), PyTorch, `uv`.
- **Fixed time budget design rationale:** experiments are comparable across any architectural changes the agent makes; the search optimises for the best model achievable within that compute budget on that specific platform.

## File Structure

| File | Role | Modified by |
|------|------|-------------|
| `prepare.py` | Constants, data prep, dataloader, evaluation | Fixed — never modified |
| `train.py` | GPT model, optimizer, training loop | Agent iterates on this |
| `program.md` | Agent instructions / research org context | Human iterates on this |

## Smaller Hardware Notes

For sub-H100 platforms (e.g. consumer GPUs, MacBooks), the repo recommends:
- Lower-entropy dataset (e.g. TinyStories)
- Reduce `vocab_size` (8192 → 4096 → 2048 or byte-level 256)
- Lower `MAX_SEQ_LEN` and `EVAL_TOKENS` in `prepare.py`
- Lower `DEPTH` (default 8 → 4)
- Use `WINDOW_PATTERN = "L"` only (avoids banded attention inefficiency)
- Lower `TOTAL_BATCH_SIZE` to powers of 2 (e.g. `2**14`)

## Entities Mentioned

[[entities/karpathy|Andrej Karpathy]], [[entities/python-bot|Python Bot]]

## Concepts Mentioned

[[concepts/backtesting|Backtesting]]

## Contradictions / Open Questions

- Results are platform-specific (not comparable across hardware) — a known tradeoff of the fixed time budget design. For the trading bot adaptation, the equivalent constraint must be defined (fixed compute budget vs fixed number of backtested trades vs fixed calendar window).

## Quotes

> "The idea: give an AI agent a small but real LLM training setup and let it experiment autonomously overnight. It modifies the code, trains for 5 minutes, checks if the result improved, keeps or discards, and repeats. You wake up in the morning to a log of experiments and (hopefully) a better model."

> "You are not touching any of the Python files like you normally would as a researcher. Instead, you are programming the `program.md` Markdown files that provide context to the AI agents and set up your autonomous research org."
