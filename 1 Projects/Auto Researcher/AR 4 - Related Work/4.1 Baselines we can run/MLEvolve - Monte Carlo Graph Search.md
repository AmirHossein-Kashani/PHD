---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2606.06473)
link: https://arxiv.org/abs/2606.06473
code: https://github.com/InternScience/MLEvolve
ar_role: runnable baseline (open-code state of the art with failure memory)
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context", "Creativity and Exploration"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context, topic/creativity-and-exploration]
created: 2026-10-27
updated: 2026-10-27
---
# MLEvolve - Monte Carlo Graph Search

> [!summary] In short
> A tree search (UCT) with extra "reference edges" that pass information between branches, an exploration rate that goes down over time, and a global memory of both successful and failed nodes. 65.3% any medal on the full MLE-bench in only 12 hours.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]], [[Creativity and Exploration]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2606.06473](https://arxiv.org/abs/2606.06473) · [GitHub](https://github.com/InternScience/MLEvolve)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page and the GitHub README. Found as an extra baseline (not in my original list). Not read in full by me yet.

---

## Paper

- **Title:** *MLEvolve: A Self-Evolving Framework for Automated Machine Learning Algorithm Discovery.*
- **Authors:** Du, Yan et al. (Shanghai AI Lab / East China Normal University), 2026, arXiv (v2 Oct 2026).
- **Code:** Apache-2.0. MLE-bench is installed separately.

## Method (main idea)

- **Progressive Monte Carlo Graph Search:** a UCT tree plus **reference edges** that carry information across branches but are not used in backpropagation.
- The exploration weight goes from 1.0 down to 0.2. When progress stalls, it expands across branches.
- **Global memory:** plan, code, score, analysis and feedback for every node, **including failed ones**, found again with BM25 + FAISS. The Debug step looks up similar errors that were already fixed.

## Results

- Full MLE-bench: **65.3% any medal** (34.7% gold) with a 12 h budget (Gemini-3.1-Pro).

## Limits

- Memory is similarity retrieval over nodes, not structured reasons.
- A strong backbone model makes comparisons hard; compare at the same model.

## How it relates to my work

- **Similar to:** stores failures and shares them across branches; a stall trigger (like AdaptiveSearch in [[FML-bench - Search Dynamics of Research Agents|FML-bench]]).
- **Different from my work because:** it retrieves similar nodes; it does not record *why* an idea failed or *what would reopen it*.
- **Can I use it as a baseline?** **Yes.** Open code and state of the art.

## Ideas it gave me

- Reference edges are a version of our "graft a branch's insight back into the main line".
