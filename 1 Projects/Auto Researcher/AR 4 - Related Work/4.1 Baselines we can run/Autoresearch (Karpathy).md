---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: GitHub repository (no paper)
link: https://github.com/karpathy/autoresearch
code: https://github.com/karpathy/autoresearch
ar_role: simple runnable baseline (greedy loop)
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents]
created: 2026-10-27
updated: 2026-10-27
---
# Autoresearch (Karpathy)

> [!summary] In short
> A tiny loop: a coding agent edits one training file, trains for 5 minutes, keeps the git commit if the validation score improves, and resets otherwise. Greedy hill-climbing with git as memory. Very popular and a strong **simple baseline** for any task with a score.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [GitHub](https://github.com/karpathy/autoresearch)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the GitHub README. Not read in full by me yet.

---

## Repo

- **Author:** Andrej Karpathy, March 2026. No paper.
- **Code:** MIT license. One NVIDIA GPU, Python 3.10+, `uv`. You bring your own coding agent (Claude Code, Codex, ...). About 92.7k GitHub stars on 27 Oct 2026.

## Problem

Let an agent run many small ML experiments overnight without a human.

## Method (main idea)

- A human writes `program.md` (the instructions). The agent may only edit `train.py` (a small GPT training script). `prepare.py` (data and evaluation) is read-only.
- Each experiment: edit, git commit, train for exactly 5 minutes, read `val_bpb`.
- **Keep** the commit if the score improved, otherwise `git reset`. Crashes get a few fix attempts; runs over 10 minutes are killed.
- **State:** the git branch (current best) and `results.tsv` (commit, score, memory, keep/discard/crash, short description). It runs until the human stops it.

## Results

- No formal evaluation. README: about 12 experiments per hour, about 100 overnight.
- Karpathy's reported gains (from press, not checked): about 700 changes in two days, about 20 transferred to bigger models, nanochat "time to GPT-2" from 2.02 h to 1.80 h (about 11%).
- In [[FML-bench - Search Dynamics of Research Agents|FML-bench]]: 0.192 mean improvement; 1st on "dense" tasks but 6th of 7 on "sparse" tasks.

## Limits

- One GPU; the fixed time budget makes results depend on the hardware.
- Greedy keep/discard can overfit the validation set or chase noise.
- No memory of *why* something failed beyond one log line.

## How it relates to my work

- **Similar to:** our setting: a task, an automatic evaluator and a long unattended run.
- **Different from my work because:** no tree, no reopening, no reasons.
- **Can I use it as a baseline?** **Yes.** Cheap and easy to adapt to any task with a score. It is the "pure refinement" end of exploration vs refinement.

## Ideas it gave me

- `results.tsv` is the minimum possible trajectory. Our path record should beat it most on sparse tasks, where greedy search gets stuck.
