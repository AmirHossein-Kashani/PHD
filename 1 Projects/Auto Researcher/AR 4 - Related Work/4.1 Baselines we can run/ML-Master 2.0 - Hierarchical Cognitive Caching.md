---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2601.10402)
link: https://arxiv.org/abs/2601.10402
code: https://github.com/sjtu-sai-agents/ML-Master
ar_role: runnable baseline (strongest memory rival)
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context]
created: 2026-10-27
updated: 2026-10-27
---
# ML-Master 2.0 - Hierarchical Cognitive Caching

> [!summary] In short
> A long-horizon ML engineering agent with three memory levels: raw traces of the current phase, summaries of finished phases that keep judgments and reasons, and cross-task "wisdom". 56.44% medal rate on the full MLE-bench (24 h). It is the closest open-code rival to our memory claim.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2601.10402](https://arxiv.org/abs/2601.10402) · [GitHub](https://github.com/sjtu-sai-agents/ML-Master)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page and the GitHub README. Found as an extra baseline (not in my original list). Not read in full by me yet.

---

## Paper

- **Title:** *Toward Ultra-Long-Horizon Agentic Science: Cognitive Accumulation for Machine Learning Engineering.*
- **Authors:** Zhu et al. (Shanghai Jiao Tong University / Shanghai AI Lab), 2026, arXiv.
- **Code:** the README says the first 2.0 code is released, but the quick start still describes 1.0. **Check before planning runs.**

## Method (main idea)

**Hierarchical Cognitive Caching:**
- **L1:** raw traces of the current phase (plan, code patches, terminal output).
- **L2:** LLM summaries of finished phases that keep the judgments and the **reasons for decisions**.
- **L3:** cross-task "wisdom", found by embedding similarity to the task.

At each phase boundary the agent summarizes, then proposes a plan with several parallel directions. There is no explicit tree.

## Results

- Full MLE-bench (75 tasks), 24 h per task: **56.44% medal rate** (3 seeds) vs 29.3% for ML-Master 1.0.
- Removing L1, L2 or L3 each hurt on MLE-bench Lite.

## Limits

- Memory is organised by time (phases), not by idea.
- Summaries can drop the path that led to a decision.

## How it relates to my work

- **Similar to:** L2 keeps decision reasons, which overlaps with our decision / evidence record.
- **Different from my work because:** no path from the current idea to the root, no undo conditions, no tree.
- **Can I use it as a baseline?** **Yes.** It is the direct test of "path to root" vs "hierarchical caching" on MLE-bench, with the same 24 h protocol.
