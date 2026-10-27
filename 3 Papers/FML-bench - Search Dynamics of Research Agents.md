---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2605.17373)
link: https://arxiv.org/abs/2605.17373
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Creativity and Exploration", "Benchmarks and Datasets"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/creativity-and-exploration, topic/benchmarks-and-datasets]
created: 2026-10-27
updated: 2026-10-27
---
# FML-bench - Search Dynamics of Research Agents

> [!summary] In short
> A benchmark of 18 ML research tasks used to compare six research agents (greedy, tree search, MCTS, evolution) with the same tools and budget. Early improvement predicts the final score, and scattered search does not help. Their **AdaptiveSearch** (greedy first, then branch when progress stalls) is the closest published version of our "jumping ideas + refinement" idea, so it is a must-cite and a likely baseline.

**Project:** [[Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Creativity and Exploration]], [[Benchmarks and Datasets]]  
**Original:** [arXiv 2605.17373](https://arxiv.org/abs/2605.17373) ([HTML v2](https://arxiv.org/html/2605.17373v2))

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv HTML (v2). Not read in full by me yet.

---

## Paper

- **Title:** FML-bench: A Controlled Study of AI Research Agent Strategies from the Perspective of Search Dynamics
- **Authors:** Qiran Zou, Hou Hei Lam, Wenhao Zhao, ..., Zhengyao Jiang, Anirudh Goyal, Dianbo Liu (National University of Singapore, Tsinghua, University of Minnesota, Weco, Meta)
- **Year:** 2026 (arXiv, May)

## Problem

Research agents use different search strategies (greedy, trees, evolution), but they are usually compared with different tools, budgets and tasks. So it is not clear which **search behaviour** actually leads to better research results.

## Method (main idea)

- **Benchmark:** 18 ML tasks in 10 areas: generalization, data efficiency, representation learning, continual learning, causality, robustness, privacy, fairness, unlearning, federated learning. Each task starts from a baseline in a real repo (for example DomainBed, FixMatch, MoCo, iCaRL, NOTEARS, OpenOOD, Opacus, Fairlearn, TOFU, FedAvg).
- **Controlled setup:** same execution infrastructure, fixed budget of 100 steps, one backbone LLM (GPT-5.4), native code editors of the agents removed.
- **Six agents compared:**

| Agent | Search style |
|---|---|
| Autoresearch | greedy hill-climbing |
| AI Scientist v1 | parallel linear idea chains |
| AI Scientist v2 | best-first tree search with an LLM journal |
| [[AIDE - ML Engineering as Code Search\|AIDE]] | solution tree with improve and debug |
| AIRA ([[AIRA-dojo - Search Policies and Operators\|AIRA-dojo]]) | UCT Monte Carlo tree search |
| OpenEvolve | MAP-Elites with islands (open version of [[AlphaEvolve]]) |

- **12 process metrics** that describe *how* the agent searched, not only the final score:
  - exploration: spread, uniqueness, reach, effective dimension
  - generalization: val-test gap, valid step ratio
  - timing: AUC over steps, first-improvement step, late-gain fraction, best-improvement step
  - cost: tokens, wall-clock time

## Results

- **Mean normalized test improvement:** AI Scientist v2 0.193, Autoresearch 0.192, AIDE 0.178, OpenEvolve 0.151, AI Scientist v1 0.132, AIRA 0.132.
- **What predicts the final score** (Spearman, 324 runs pooled):
  - AUC over steps: ρ = +0.784 (strongest; improving early matters most)
  - first-improvement step: ρ = −0.291 (the earlier the first gain, the better)
  - exploration reach: ρ = +0.115 (going far from the baseline helps a little)
  - effective dimension: ρ = −0.140 (searching in many scattered directions hurts)
  - uniqueness (diversity), token cost, wall-clock time: **not significant**
- **Task structure decides the balance.** Greedy search wins when small changes often help ("dense" tasks). Search with several frontiers wins when most changes fail ("sparse" tasks). Autoresearch is 1st on dense tasks but 6th of 7 on sparse tasks.
- **AdaptiveSearch (their new agent):**
  - starts as greedy hill-climbing;
  - when the best-so-far slope over a 50-step window falls to 0.0005 or less, it forks 1 to 3 branches (number set by remaining budget);
  - each new branch must start from a mechanistically different idea; branches run round-robin.
  - Result: 0.208 mean improvement and 58.6% win-rate, the best of the seven agents, and top 2 on both dense and sparse tasks.
- OpenEvolve's parent sampling in their setup: 20% exploration, 70% exploitation, 10% fitness-weighted.

## Limits

- One LLM, a fixed 100-step budget, and agents run without their native editors and extra parts, which may make some agents look weaker than they are.
- The correlations are pooled and observational.
- The dense / sparse split was defined after the fact, on the same data.

## How it relates to my work

- **Similar to:** our open question on exploration vs refinement ([[AR - Open Questions]]). AdaptiveSearch is "refine until stuck, then jump", which is very close to combining jumping ideas with refinement.
- **Different from my work because:** their branches keep the same context. There is no path to the root with overturn conditions, no shared failure memory, and no way to graft a branch's insight back into the main line (see [[AR - Method and Contributions]]).
- **Can I use it as a baseline?** Yes. AdaptiveSearch is a natural exploration baseline. The benchmark itself is a candidate evaluation task with real repos and a separate test score (see [[AR - Benchmarks]]).
- **Useful for the paper text:** "diversity alone is not enough". Uniqueness did not predict the score; early gains and focused depth did.

## Ideas it gave me

- Report their **process metrics** (AUC over steps, first-improvement step, late-gain fraction, reach, effective dimension) next to the final score in our experiments.
- Use a **stagnation trigger** (slope of the best score over a window) from our trajectory to decide when to explore.
- New "jumping" ideas must be followed by **focused refinement**, otherwise they become scattered search.
