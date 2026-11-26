---
type: paper
date: 2026-11-18
status: To read
year: 2026
venue: ICML 2026 (per the arXiv comment)
link: https://arxiv.org/abs/2602.02660
code: https://github.com/jfc43/MARS (paper says it holds the trajectories; not checked)
ar_role: must cite (closest to failure records); in the paper introduction
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context]
created: 2026-11-18
updated: 2026-11-18
---
# MARS - Modular Agent with Reflective Search

> [!summary] In short
> A tree search (budget-aware MCTS) over modular solution repositories, with a **Comparative Reflective Memory**: after each result, an agent writes a lesson saying what changed compared with the best solution, what effect it had, and a general rule. 56.0% any medal on MLE-Bench (24 h, Gemini-3-Pro-Preview) against 37.8% for AIRA-dojo. Its lessons are the closest published form of our failure records, but they are not organised by path and have no undo condition.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]]  
**Part of:** [[AR 4 - Related Work and Baselines]] (section 4, systems in our paper introduction)  
**Original:** [arXiv 2602.02660](https://arxiv.org/abs/2602.02660) · [PDF](https://arxiv.org/pdf/2602.02660) · [GitHub](https://github.com/jfc43/MARS)

> [!note] Source of these notes
> Written by Claude on 2026-11-18 from the arXiv page and the PDF (the second half of the PDF was read as a summary only). Not read in full by me yet.

---

## Paper

- **Title:** *MARS: Modular Agent with Reflective Search for Automated AI Research.*
- **Authors:** Jiefeng Chen, Bhavana Dalvi Mishra, Jaehyun Nam, Rui Meng, Tomas Pfister, Jinsung Yoon (Google Cloud AI Research; the same group as [[ScientistTwo]]).
- **Dates:** arXiv v1 2 Feb 2026, v3 19 May 2026. The arXiv comment says ICML 2026.
- **Code:** the paper points to github.com/jfc43/MARS for its trajectories. Whether the agent code is there is not checked.

## Problem

ML engineering tasks are slow to evaluate (training takes time), and it is hard to tell which change caused a gain. Agents tend to write one big script and ignore the cost of running it.

## Method (main idea)

- **Search:** budget-aware MCTS (UCT) over a tree of solution repositories, with three actions: Draft, Improve, Debug.
- **Modular solutions:** each candidate is a set of modules plus an orchestration script, written by an Idea agent, a Modular agent and a Coding agent with diff-based edits.
- **Cost in the reward:** G(v) x [t(v) / L(v)]^w with w = -0.07, so slow runs are penalised.
- **Comparative Reflective Memory:** a Lesson Distillation agent compares the new solution with the best known one and records the causal change, its impact and a general rule. Failed runs give debugging lessons.
- **Retrieval:** lessons are taken by recency, at most K_m = 30, and a Review agent removes redundant ones. They are organised by category and recency, not by tree path or similarity.
- **Failures and backtracking:** a buggy node gets up to N_d = 10 debug attempts and then stays in the tree as a record. Backtracking is only UCT revisiting nodes; when a leaf is fully expanded, the root is reopened for new drafts.

## Results

- **MLE-Bench** (75 Kaggle tasks), 24 h on one A100 40GB, 3 runs.
- **Any medal:** 56.0 ± 1.5% with Gemini-3-Pro-Preview, against 37.8 ± 2.5% for AIRA-dojo with the same model. Gold rate 31.1%, the highest of the agents reported.
- **Cost:** 43.1% any medal for $60.5 (24 h) against 24.4% for $39.0 for AIRA-dojo (Gemini-2.5-Pro setting). A 4-hour MARS run reaches 28.4% for $9.6.
- **MARS+** (two trees, 2 x H100): 62.7 ± 0.8% any medal.
- **Memory matters:** removing it gives a "drastic" drop (ablation figure). 63.0 ± 1.8% of the lessons used come from another branch. 88.34% of 3,611 lessons were judged causally correct (LLM audit; 90% on 20 checked by hand).

## Limits

- Only MLE-Bench.
- Many API calls, so it is expensive.
- Lessons can be hallucinated or attributed to the wrong change.

## How it relates to my work

- **Similar to:** tree search with lessons shared across branches; failures kept as records; cost as a signal. The lesson ("what changed, what it did, the general rule") is close to our failure record.
- **Different from my work because:** memory is the 30 most recent lessons, not the path from the current idea to the root. There is no undo condition, so a dropped idea is not reopened when new evidence arrives. Failures are recorded as debug fixes, not as refuted hypotheses with their evidence.
- **Can I use it as a baseline?** Possibly, if the agent code is in the repo (check). It runs on MLE-Bench, which fits [[AR 3 - Benchmarks]].
