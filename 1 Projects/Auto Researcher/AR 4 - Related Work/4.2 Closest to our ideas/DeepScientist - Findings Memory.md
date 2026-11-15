---
type: paper
date: 2026-10-27
status: To read
year: 2025
venue: ICLR 2026
link: https://arxiv.org/abs/2509.26603
code: https://github.com/ResearAI/DeepScientist
ar_role: must cite (weeks-long runs with failure memory); too costly to run
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context", "Creativity and Exploration"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context, topic/creativity-and-exploration]
created: 2026-10-27
updated: 2026-10-27
---
# DeepScientist - Findings Memory

> [!summary] In short
> Treats discovery as Bayesian optimization: an LLM scores ideas, a UCB rule picks the next one to implement, and testing is staged so that only promising ideas get expensive runs. A **Findings Memory** keeps every idea, failed ones too. Month-long runs.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]], [[Creativity and Exploration]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2509.26603](https://arxiv.org/abs/2509.26603) · [GitHub](https://github.com/ResearAI/DeepScientist)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page and the GitHub README. Found as an extra system (not in my original list). Not read in full by me yet.

---

## Paper

- **Title:** *DeepScientist: Advancing Frontier-Pushing Scientific Findings Progressively.*
- **Authors:** Weng et al. (Westlake University).
- **Venue:** ICLR 2026.
- **Code:** Apache-2.0.

## Method (main idea)

- **Bayesian optimization view:** an LLM "surrogate" scores each idea; a UCB rule chooses which to implement.
- **Staged evaluation:** cheap tests first; only promising ideas get costly runs.
- **Findings Memory:** a database of every idea and result, failed ones included; the top 15 are given as context.
- Each research "quest" is a git repo that keeps failed paths.

## Results

- Month-long runs, about 20,000 GPU-hours.
- About 5,000 ideas, about 1,100 tested, 21 that made progress.
- Beat the human state of the art on frontier AI tasks, for example AI-text detection on RAID (AUROC 0.863 vs 0.800).

## Limits

- Very costly and a low hit rate (21 of about 5,000 ideas).
- Not run on MLE-bench, so hard to compare.

## How it relates to my work

- **Similar to:** long runs, failure memory, git as research history.
- **Different from my work because:** memory is a flat database ranked by score, not a path with undo conditions.
- **Can I use it as a baseline?** Too costly. Cite it as the canonical long-run agent with failure memory.

## Ideas it gave me

- Use a learned value (surrogate + UCB) to decide between refining and jumping.
