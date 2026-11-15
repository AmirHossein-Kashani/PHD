---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2604.13018)
link: https://arxiv.org/abs/2604.13018
code: no public code
ar_role: must cite (overlaps with our trajectory idea)
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Memory and Long Context"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/memory-and-long-context]
created: 2026-10-27
updated: 2026-10-27
---
# AiScientist - File-as-Bus Long-Horizon Engineering

> [!summary] In short
> "Thin control over thick state": a light orchestrator calls specialist agents, and all project state lives in a shared workspace (**File-as-Bus**) with versioned files and append-only logs. 81.82% any medal on MLE-bench Lite; removing File-as-Bus costs 31.8 points.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Memory and Long Context]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2604.13018](https://arxiv.org/abs/2604.13018)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv abstract and the paper. Found as an extra system (not in my original list). Not read in full by me yet.

---

## Paper

- **Title:** *Toward Autonomous Long-Horizon Engineering for ML Research.*
- **Authors:** Guoxin Chen, Jie Chen, Lei Chen, ..., Kai Jia (Renmin University of China), arXiv, Apr 2026 (v2 May 2026).
- **Code:** no public code (only anonymized supplementary material).

## Method (main idea)

- A small hierarchical research team: an orchestrator plus specialist agents.
- **File-as-Bus:** a shared workspace keeps decision-relevant files across roles and calls: versioned artifacts, append-only implementation and experiment logs, and code.
- Failures, diagnoses and metric gaps are logged for reuse. The orchestrator goes back to earlier analysis **when an assumption is invalidated**.
- No tree search.

## Results

- **MLE-bench Lite:** 81.82% any medal with both Gemini-3-Flash and GLM-5 (+4.55 and +16.67 points over the best matched baselines; +13.64 over a Codex / GPT-5.5 harness).
- **PaperBench:** +9.92 and +11.15 points over the best matched baselines.
- **Ablation:** without File-as-Bus, -31.82 points on MLE-bench Lite and -6.41 on PaperBench.

## Limits

- No code. "Going back when an assumption is invalidated" is described but not measured on its own.

## How it relates to my work

- **Similar to:** append-only logs (our trajectory) and revisiting invalidated assumptions (our undo condition).
- **Different from my work because:** the state is a flat workspace of files, not a path per idea; no tree.
- **Can I use it as a baseline?** No (no code). We must position against it explicitly.
- **Useful for the paper:** strong evidence that durable state is central to long runs (-31.8 points without it).
