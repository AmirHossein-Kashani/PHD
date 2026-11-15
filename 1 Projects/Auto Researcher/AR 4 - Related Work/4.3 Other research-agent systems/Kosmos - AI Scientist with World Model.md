---
type: paper
date: 2026-10-27
status: To read
year: 2025
venue: arXiv preprint (2511.02824)
link: https://arxiv.org/abs/2511.02824
code: no system code (commercial platform, Edison Scientific)
ar_role: related work; one of the three closest systems in our intro
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context]
created: 2026-10-27
updated: 2026-10-27
---
# Kosmos - AI Scientist with World Model

> [!summary] In short
> Long runs (up to 12 hours, about 200 agent rollouts) where data-analysis and literature agents share one **structured world model**. It is one of the three closest systems in our introduction. It keeps conclusions, not the path that produced them.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2511.02824](https://arxiv.org/abs/2511.02824) · [figure scripts only](https://github.com/EdisonScientific/kosmos-figures)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page and the paper. Not read in full by me yet.

---

## Paper

- **Title:** *Kosmos: An AI Scientist for Autonomous Discovery.*
- **Authors:** Ludovico Mitchener et al. (Edison Scientific, FutureHouse, Oxford and others), 2025, arXiv.
- **Code:** no public system code; only the figure and analysis scripts. It is a **commercial platform** (Edison Scientific). At launch a run cost about $200, with a free tier for academics.

## Problem

Agents lose coherence after many steps, which limits how deep a discovery can go.

## Method (main idea)

- Input: an open-ended objective and a dataset (up to about 5 GB).
- Up to 20 cycles, up to 10 tasks per cycle. A data-analysis agent and a literature-search agent work in parallel.
- A **structured world model** stores their findings and proposes the next tasks.
- A typical run: about 42,000 lines of code and about 1,500 papers read. Every statement in the report is cited to code or to a paper.

## Results

- 7 discoveries in metabolomics, materials science, neuroscience and genetics (3 reproduce earlier findings, 4 are new).
- Experts judged 79.4% of 102 statements accurate (data analysis 85.5%, literature 82.1%, synthesis 57.9%).
- The authors estimate one run equals about 4.1 expert-months of work.

## Limits

- Over-strong claims and weak interpretation; poor with raw images and sequencing files.
- Runs are random and scientists cannot steer during a run.
- Agents still lose coherence after many actions (the authors say so).

## How it relates to my work

- **Similar to:** many short agents writing into one shared memory (our "shared knowledge" part, which we do **not** claim).
- **Different from my work because:** the world model keeps outcomes, not the decisions that produced them. A failed line of work survives only as its result.
- **Can I use it as a baseline?** No (commercial, no code, no hidden-score tasks). Cite it as the closest long-run system.
