---
type: paper
date: 2026-10-27
status: To read
year: 2025
venue: NeurIPS 2025
link: https://arxiv.org/abs/2507.02554
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Creativity and Exploration"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/creativity-and-exploration]
created: 2026-10-27
updated: 2026-10-27
---
# AIRA-dojo - Search Policies and Operators

> [!summary] In short
> Meta's study of research agents on MLE-bench. An agent = a **search policy** (greedy, MCTS, evolution) + a set of **operators** (draft, debug, improve, memory, crossover). Better operators mattered more than the choice of search policy. Agents also overfit to validation, the same problem we saw on Kaggle.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Creativity and Exploration]]  
**Original:** [arXiv 2507.02554](https://arxiv.org/abs/2507.02554) · [NeurIPS 2025](https://proceedings.nips.cc/paper_files/paper/2025/hash/328b81881da145412f2bc56c998dfb6a-Abstract-Conference.html)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv HTML (v2). Not read in full by me yet.

---

## Paper

- **Title:** AI Research Agents for Machine Learning: Search, Exploration, and Generalization in MLE-bench
- **Authors:** Edan Toledo, Karen Hambardzumyan, Martin Josifoski, ..., Yoram Bachrach (25 authors; FAIR at Meta, UCL, Örebro University)
- **Year / venue:** 2025, NeurIPS 2025

## Problem

Which part of a research agent makes it good: the **search policy** or the **actions (operators)** it can take? And why do gains on validation not turn into gains on the hidden test set?

## Method (main idea)

- **AIRA-dojo:** a framework to run research agents at scale. Code runs in Jupyter notebooks inside Apptainer containers. Each run gets one H200 GPU, 24 hours, and a 4-hour cap per code execution.
- **Formal view:** an agent is a graph search over candidate solutions, defined by a fitness function, a selection policy, an operator set, an operator policy and a stop rule.
- **Operators:**
  - AIDE set: Draft, Debug, Improve, Memory.
  - Added: **Crossover** (combine two solutions).
  - New **AIRA operators** add: a complexity cue based on how many children a node already has; **scoped memory** (sibling memories for Draft and Improve, ancestor memories for Debug); explicit thinking tokens.
- **Search policies:**
  - Greedy: always expand the best node.
  - MCTS: UCT selection, no simulated rollouts.
  - Evolutionary: fixed-size population, parents chosen by fitness, Improve or Crossover.

## Results (MLE-bench Lite, 22 tasks)

- With the AIDE operators, MCTS and evolution gave **no gain over greedy**, and changing the MCTS exploration constant did little. Conclusion: **the operator set is the bottleneck**, not the search policy.
- With the AIRA operators, every policy beat AIDE. Best: AIRA + MCTS + DeepSeek R1, medal rate **39.6% → 47.7%**. AIRA-Greedy 45.5% vs AIDE-Greedy 39.8%. Re-run on the newest framework: 55%.
- Non-greedy policies built more diverse search graphs.
- Removing memory barely changed results (with the AIDE operators).
- **Generalization gap:**
  - choosing the final solution by test score instead of validation score would add 9 to 13 points;
  - picking the best-validation node from the final graph closes most of this gap (more than 60% for greedy);
  - submitting the top 3 validation nodes adds about 10%;
  - AIDE's test score stopped improving while its validation score kept rising (overfitting over time).
- Longer runs (up to 120 hours) kept improving.

## Limits (my view)

- MLE-bench tasks are Kaggle competitions, so public solutions may be in the training data (the same copying risk as our Kaggle task).
- Very heavy compute per run (one H200, 24 hours).

## How it relates to my work

- **Similar to:** our Arbor-based loop is also a search policy plus operators. Their scoped memory (siblings vs ancestors) is close to our "parent history only" idea (path to the root).
- **Different from my work because:** their memory is scoped by operator but does not store *why* a decision was taken or what would overturn it, and there is no shared failure memory across branches (see [[AR 1 - Problem and Method]]).
- **Can I use it as a baseline?** Yes: AIRA-Greedy or AIRA-MCTS as search baselines. MLE-bench Lite is an evaluation option, with the Kaggle problems noted in [[AR 3 - Benchmarks]].
- **Supports our Kaggle finding** that good validation scores did not give good final scores ([[AR 2 - Experiments and Results]]). They measured this gap directly.

## Ideas it gave me

- Improving the **operators** (for example, what a new "jumping" idea is told to do) may matter more than tuning how much budget goes to exploration.
- Add a final **selection step** that guards against validation overfitting (top-k validation nodes, or a held-out check). This also helps with the limited Kaggle submissions.
- Ablation on memory scope: ancestors only (ours) vs siblings vs the whole tree (Arbor default).
