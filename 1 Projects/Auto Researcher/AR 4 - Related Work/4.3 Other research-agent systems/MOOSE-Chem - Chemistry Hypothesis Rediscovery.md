---
type: paper
date: 2026-10-27
status: To read
year: 2024
venue: ICLR 2025
link: https://arxiv.org/abs/2410.07076
code: https://github.com/ZonglinY/MOOSE-Chem
ar_role: related work (domain hypothesis generation, no experiments)
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Creativity and Exploration"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/creativity-and-exploration]
created: 2026-10-27
updated: 2026-10-27
---
# MOOSE-Chem - Chemistry Hypothesis Rediscovery

> [!summary] In short
> Rediscovers chemistry hypotheses from a research background plus 1-3 "inspiration" papers: find the inspirations, compose and evolve hypotheses, then rank them. Tested on 51 chemistry papers from 2024. No experiments.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Creativity and Exploration]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2410.07076](https://arxiv.org/abs/2410.07076) · [GitHub](https://github.com/ZonglinY/MOOSE-Chem)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv pages, the ICLR page and the GitHub READMEs. Not read in full by me yet.

---

## Paper

- **Title:** *MOOSE-Chem: Large Language Models for Rediscovering Unseen Chemistry Scientific Hypotheses.*
- **Authors:** Zonglin Yang et al.
- **Venue:** ICLR 2025.
- **Code:** MIT license; needs an OpenAI / Azure OpenAI key.
- **Follow-ups:** [MOOSE-Chem2](https://arxiv.org/abs/2505.19209) (NeurIPS 2025; hierarchical search for fine-grained hypotheses) and [MOOSE-Chem3](https://arxiv.org/abs/2505.17873) (ranks hypotheses with feedback from a simulated experiment).

## Problem

Can an LLM find a real, unseen chemistry hypothesis when given only the research question and background?

## Method (main idea)

- Assumption: a hypothesis = background + 1-3 inspiration papers.
- **Retrieve:** the LLM screens the corpus in windows of 15 papers and picks 3 inspirations from each.
- **Compose:** an "evolutionary unit" mutates hypotheses, critiques them (validity, novelty, clarity, significance), refines and recombines the best.
- **Rank:** score on four aspects; beam search (3 rounds, width 15).

## Results

- **TOMATO-Chem:** 51 chemistry papers from 2024 (27 in Nature or Science), annotated by PhD chemists.
- The top 4% of a 3,000-paper corpus covers 86.9% of the true inspirations.
- Rediscovery score (GPT-4o judge, 300-paper corpus): 4.02 vs 2.88 (MOOSE), 2.69 (Qi et al.), 2.55 (SciMON). With expert scoring, only 2 of 51 reached 4 or more.
- MOOSE-Chem2: soft recall 40.35% vs 16.60% for greedy search.

## Limits

- Rediscovery only, and the true inspirations are in the corpus.
- GPT-4o scores are 1-2 points higher than expert scores. Chemistry only; no experiments.

## How it relates to my work

- **Similar to:** evolution and beam search over hypotheses; evaluation with a time cutoff (papers after the model's training data).
- **Different from my work because:** no execution and no score loop.
- **Can I use it as a baseline?** Only on a hypothesis-rediscovery task. MOOSE-Chem3 (simulated experiment feedback) is the closest to a test-and-revise loop.

## Ideas it gave me

- **Time-cutoff rediscovery** is a way to avoid contamination; it is the same idea as TRACES DRR in [[AR 3 - Benchmarks]].
