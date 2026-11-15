---
type: paper
date: 2026-10-27
status: To read
year: 2025
venue: arXiv 2024 (v1), arXiv 2025 (v2); Nature 2026 (both)
link: https://arxiv.org/abs/2504.08066
code: https://github.com/SakanaAI/AI-Scientist-v2
ar_role: related work (end-to-end); reviewers will expect it
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents]
created: 2026-10-27
updated: 2026-10-27
---
# The AI Scientist (v1 and v2)

> [!summary] In short
> Sakana AI's end-to-end research agent: idea, code, experiments, paper and an automatic reviewer. v2 drops the human code template and uses a staged best-first tree search (built on AIDE). It is the best-known end-to-end baseline, but each stage keeps only its best node, so earlier alternatives and their reasons are lost.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv v1](https://arxiv.org/abs/2408.06292) · [arXiv v2](https://arxiv.org/abs/2504.08066) · [Nature 2026](https://doi.org/10.1038/s41586-026-10265-5) · code [v1](https://github.com/SakanaAI/AI-Scientist) / [v2](https://github.com/SakanaAI/AI-Scientist-v2)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv pages, the GitHub READMEs and the Nature record. Not read in full by me yet.

---

## Paper

- **v1:** *The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery.* Chris Lu et al., 2024, arXiv 2408.06292.
- **v2:** *The AI Scientist-v2: Workshop-Level Automated Scientific Discovery via Agentic Tree Search.* Yutaro Yamada et al., 2025, arXiv 2504.08066.
- **Journal version (both):** Lu et al., *Towards end-to-end automation of AI research*, **Nature** 651, 914-919 (2026). doi:10.1038/s41586-026-10265-5.
- **Code:** public, with a custom "AI Scientist Source Code License" (not a standard open-source license). Needs paid LLM APIs and NVIDIA GPUs. About 14.7k (v1) and 7.3k (v2) GitHub stars on 27 Oct 2026.

## Problem

Automate the whole ML research cycle, from an idea to a written and reviewed paper.

## Method (main idea)

- **v1:** starts from a human-written code template. An LLM grows an archive of ideas, scores them (interesting, novel, feasible) and checks novelty with Semantic Scholar. The Aider coding agent runs up to 5 experiment rounds and writes notes after each one. Then it writes a LaTeX paper, and a GPT-4o reviewer scores it.
- **v2:** no template. An experiment-manager agent runs a best-first tree search over code nodes (built on [[AIDE - ML Engineering as Code Search|AIDE]]) in four stages: preliminary investigation, hyperparameter tuning, research agenda, ablations. The best node of each stage seeds the next stage. A vision-language model checks the figures.

## Results

- **v1:** the automatic reviewer reached 0.65 balanced accuracy on 500 ICLR 2022 papers (humans: 0.66). On the diffusion template, 38 of 51 ideas became full papers for about $250 in total.
- **v2:** one of three fully AI-written papers passed review at an ICLR 2025 workshop (scores 6/7/6). It was withdrawn as planned.
- In [[FML-bench - Search Dynamics of Research Agents|FML-bench]] (same tools and budget for all agents), AI Scientist v2 had the best mean test improvement of the existing agents (0.193).

## Limits

- Workshop level only; 1 of 3 papers accepted. Hallucinated citations and numbers.
- The v2 README says v2 does not always make better papers than v1.
- v1 tried unsafe actions (relaunching itself, editing its own time limit).

## How it relates to my work

- **Similar to:** tree search over experiments with an automatic score.
- **Different from my work because:** each stage keeps only its best node. Nodes keep code and scores, not reasons. Pruned alternatives are never reopened. There is no path to the root with undo conditions.
- **Can I use it as a baseline?** Reviewers will expect it because it is the best-known name. But it is built for idea-to-paper, not for a fixed task with a hidden test score. The cheapest fair option is its tree search on our tasks (as FML-bench did).
