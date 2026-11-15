---
type: paper
date: 2026-10-27
status: To read
year: 2025
venue: NeurIPS 2025
link: https://arxiv.org/abs/2505.18705
code: https://github.com/HKUDS/AI-Researcher
ar_role: related work (end-to-end); one of the three closest systems in our intro
projects: ["Auto Researcher"]
topics: ["Autonomous Research Agents", "Benchmarks and Datasets"]
tags: [type/paper, project/auto-researcher, topic/autonomous-research-agents, topic/benchmarks-and-datasets]
created: 2026-10-27
updated: 2026-10-27
---
# AI-Researcher and Scientist-Bench

> [!summary] In short
> HKU's full pipeline from 10-15 reference papers to a written paper. It also introduces Scientist-Bench (22 AI papers as targets). It is one of the three closest systems in our introduction. Idea refinement is thin, and the benchmark is judged only by LLMs.

**Project:** [[AR 0 - Start Here|Auto Researcher]]  
**Topics:** [[Autonomous Research Agents]], [[Benchmarks and Datasets]]  
**Related work note:** [[AR 4 - Related Work and Baselines]]  
**Original:** [arXiv 2505.18705](https://arxiv.org/abs/2505.18705) · [GitHub](https://github.com/HKUDS/AI-Researcher)

> [!note] Source of these notes
> Written by Claude on 2026-10-27 from the arXiv page, the NeurIPS PDF and the GitHub README. Not read in full by me yet.

---

## Paper

- **Title:** *AI-Researcher: Autonomous Scientific Innovation.*
- **Authors:** Jiabin Tang, Lianghao Xia, Zhonghang Li, Chao Huang (University of Hong Kong).
- **Venue:** NeurIPS 2025 (the README says Spotlight).
- **Code:** public. Needs LLM APIs (through LiteLLM / OpenRouter) and Docker; GPU optional. About 5.8k GitHub stars. A hosted version exists (novix.science).

## Problem

Run the whole research process, from reading papers to a finished manuscript, without a human.

## Method (main idea)

1. **Resource Analyst** reads the reference papers, their code and LaTeX, and breaks ideas into "atomic concepts" that link math to code.
2. **Idea Generator** writes 5 different directions and keeps the best one (novelty, soundness).
3. **Code Agent** tests a prototype on small data, then runs full experiments.
4. **Advisor / Judge** reviews the code in mentor-student rounds and decides whether to continue.
5. **Documentation Agent** writes the paper.

## Results

- **Scientist-Bench:** 22 papers (2022-2024) in diffusion models, vector quantization, GNNs and recommender systems. Level 1 = guided (22 tasks); Level 2 = open-ended (5-6 tasks).
- Implementation completeness 93.8% with Claude models; mean correctness 2.65/5.
- On a matched subset: completeness 87.5% with Claude vs 50% with GPT-4o.

## Limits

- Code fails on tensor shape and data type errors. Correctness is lower for new ideas.
- LLM judges disagree a lot. The target papers are likely in the models' training data and can be found online.

## How it relates to my work

- **Similar to:** a full loop with an advisor that decides to continue.
- **Different from my work because:** only 1 of 5 ideas is implemented; the only revision is a continue/stop verdict; no tree and no reopening of ideas.
- **Can I use it as a baseline?** Possible end-to-end baseline, but its tasks are paper reproduction with LLM judging, not hidden-score tasks.

## Ideas it gave me

- Start the research from a set of input papers (a literature step), as in [[AR 5 - Open Questions]] ("explicit research steps").
