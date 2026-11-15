---
type: paper
date: 2026-10-27
status: To read
year: 2026
venue: arXiv preprint (2605.20025)
link: https://arxiv.org/abs/2605.20025
code: https://github.com/aiming-lab/AutoResearchClaw
ar_role: related work (full pipeline); optional end-to-end baseline
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Multi-Agent Debate"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/multi-agent-debate]
created: 2026-10-27
updated: 2026-10-27
---
# AutoResearchClaw

> [!summary] In short
> A 23-stage idea-to-paper pipeline: a debate makes the hypotheses, experiments run in a sandbox, and a decision step chooses **PROCEED, REFINE or PIVOT**. It keeps a cross-run lesson store and offers seven levels of human control. Popular, but evaluated with an LLM judge.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Multi-Agent Debate]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2605.20025](https://arxiv.org/abs/2605.20025) · [GitHub](https://github.com/aiming-lab/AutoResearchClaw)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page and the GitHub README. Not read in full by me yet.

---

## Paper

- **Title:** *AutoResearchClaw: Self-Reinforcing Autonomous Research with Human-AI Collaboration.*
- **Authors:** Jiaqi Liu et al. (UNC-Chapel Hill; last author Huaxiu Yao), arXiv, May 2026.
- **Code:** MIT license. Works with an OpenAI-compatible LLM or a coding-agent CLI (Claude Code, Codex, Gemini CLI). About $3-15 of LLM cost per run. About 14.6k GitHub stars on 27 Oct 2026.

## Problem

Full-auto research pipelines often produce empty or wrong papers. How much does a little human help change that?

## Method (main idea)

- **8 phases, 23 stages:** topic, literature, hypothesis, code, experiments, analysis, draft, simulated peer review, revision, citation check.
- **Hypotheses** come from a debate of three roles: Innovator, Pragmatist, Contrarian.
- **Experiments** run in a sandbox, Docker or over SSH; failed runs are repaired.
- **Stage 15 decides:** PROCEED, REFINE (back to experiments) or PIVOT (back to the hypothesis).
- **Memory:** a cross-run lesson store with a 30-day half-life. An optional "MetaClaw" mode turns failures into reusable skills.
- **Checks:** reported numbers must match the executed outputs; citations go through a 4-layer check.

## Results

- **ARC-Bench:** 25 ML topics plus 20 science topics, scored by an LLM judge with a rubric (GPT-5.3-codex for all systems).
- Human-in-the-loop mode: 0.648 vs 0.419 for AI Scientist v2 (+54.7%).
- End-to-end study on 10 topics: 87.5% accept with human help vs 25% full-auto.

## Limits

- LLM judge; a small end-to-end study; scripted human help; one backbone model.
- Full-auto runs can make empty results that still pass the checks.

## How it relates to my work

- **Similar to:** REFINE vs PIVOT is our "refine vs jump" question. The lesson store is like a shared failure memory.
- **Different from my work because:** a linear pipeline with loops back, not a tree. Lessons are global, not tied to a decision path.
- **Can I use it as a baseline?** Possible (open, cheap), but full-auto is weak and the evaluation is LLM-judged.

> [!question] Check
> "ARC-Bench-ML" in [[AR 3 - Benchmarks]] may be this ARC-Bench. Confirm.
