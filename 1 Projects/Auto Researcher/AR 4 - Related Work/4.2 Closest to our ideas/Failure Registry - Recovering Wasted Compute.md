---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2608.10424)
link: https://arxiv.org/abs/2608.10424
code: no public code found
ar_role: must cite (failure memory + backtracking); possible plug-in ablation
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context]
created: 2026-10-27
updated: 2026-10-27
---
# Failure Registry - Recovering Wasted Compute

> [!summary] In short
> A plug-in "failure registry" for research agents: it records the error type, signature and failed strategy, turns them into **banned patterns** and **proven fixes**, and backtracks to the branch point when the same error repeats. It raised AIDE's gold medals from 22 to 38 on 9 tabular tasks.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2608.10424](https://arxiv.org/abs/2608.10424)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page. Found as an extra system (not in my original list). Not read in full by me yet.

---

## Paper

- **Title:** *Recovering Wasted Compute in Autoresearch Agents.*
- **Authors:** Au Kwok Chun et al. (Columbia University / Capital One), arXiv, Aug 2026.
- **Code:** none found.

## Method (main idea)

- A shared **failure registry** stores error type, error signature and the failed strategy.
- These are turned into "banned patterns" and "proven fixes" that are added to prompts.
- When the same error comes back on a path, the agent **backtracks to the branch point** and samples among sibling nodes with Thompson sampling.
- Works as a plug-in for [[AIDE - ML Engineering as Code Search|AIDE]], [[ML-Master 2.0 - Hierarchical Cognitive Caching|ML-Master]] and [[MLEvolve - Monte Carlo Graph Search|MLEvolve]].

## Results

- 9 tabular MLE-bench / Kaggle tasks, 10 seeds, 2 h, GPT-5-mini.
- AIDE gold medals 22 to 38; valid submissions 73/90 to 90/90; repeated-bug nodes 46% to 7.8%.

## Limits

- Short runs, a small model, tabular tasks only.
- Failures are code-level (crashes, bugs), not idea-level.

## How it relates to my work

- **Similar to:** our failure record plus backtracking.
- **Different from my work because:** it records **how the code crashed**; we want to record **why the idea failed and what would change the verdict**.
- **Can I use it as a baseline?** As an ablation: "Arbor + failure registry" vs "Arbor + our failure refinement". Must cite.
