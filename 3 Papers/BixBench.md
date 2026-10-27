---
type: paper
date: 2026-08-03
status: Done
projects: ["Reversible Exploration (ARIADNE and Retro-Fork)"]
topics: ["Biomedicine and Drug Discovery", "Benchmarks and Datasets"]
tags: [type/paper, project/reversible-exploration, topic/biomedicine-and-drug-discovery, topic/benchmarks-and-datasets]
notion: https://app.notion.com/p/3b1d68da32f080aa9f26f81ad91b11aa
created: 2026-08-03
updated: 2026-08-05
---
# BixBench

> [!summary] In short
> 61 expert bioinformatics capsules with 205 questions. Decided to ignore it because it needs a specific environment.

**Project:** [[Reversible Exploration (ARIADNE and Retro-Fork)]]  
**Topics:** [[Biomedicine and Drug Discovery]], [[Benchmarks and Datasets]]  
**Notion category:** [[Notion Category - Agentic AI Researchers]]  
**Part of:** [[Review - Benchmarks for MoA and Multi-Agent Systems]]  
**Sub-notes:** [[BixBench - Tasks and Baselines]]  
**Original:** [Notion page](https://app.notion.com/p/3b1d68da32f080aa9f26f81ad91b11aa)

---

Each task comes from a "capsule" — a real bioinformatics analysis done by an expert (PhD-level analyst). A capsule contains input data files, a research hypothesis, and the expert's Jupyter notebook. The benchmark has **61 capsules with 205 questions** (about 3.8 questions per capsule), covering areas like genomics, transcriptomics, differential expression, RNA-seq, phylogenetics, and single-cell analysis.
  
  
- **Open-answer (the main one)** — the agent's free-text answer is compared to the ground truth by a judge LLM (Claude 3.5 Sonnet), scored binary correct/incorrect. Each capsule is run 5 times in parallel and accuracy is averaged.
- **MCQ (secondary)** — after the run, a second LLM gets the agent's notebook + answer and picks from multiple choices, with majority voting over the 5 runs. Two variants: with an "insufficient information" refusal option, and without it.
